---
title: "Habilitar a geração de código QR para aplicativos de autenticador no núcleo do ASP.NET"
author: rick-anderson
description: "Habilitar a geração de código QR para aplicativos de autenticador no núcleo do ASP.NET"
keywords: "Núcleo do ASP.NET, MVC, geração de código QR, o autenticador, 2FA"
ms.author: riande
manager: wpickett
ms.date: 07/24/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: 0c3f0a6d666caf2690330199946b5153f10b4541
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/19/2017
---
# <a name="enabling-qr-code-generation-for-authenticator-apps-in-aspnet-core"></a>Habilitar a geração de código QR para aplicativos de autenticador no núcleo do ASP.NET

Observação: Este tópico se aplica ao ASP.NET Core 2. x com páginas Razor.

ASP.NET Core é fornecido com suporte para aplicativos de autenticador para autenticação individual. Dois aplicativos factor authentication (2FA) autenticador, usando um baseada em tempo de uso único senha algoritmo (TOTP), são o setor recomendado approch para 2FA. 2FA usar TOTP é preferível à 2FA do SMS. Um aplicativo autenticador fornece um código de 6 a 8 dígitos que os usuários devem digitar depois de confirmar seu nome de usuário e senha. Normalmente, um aplicativo autenticador é instalado em um Smartphone.

Os modelos de aplicativo web ASP.NET Core autenticadores de suporte, mas não oferecem suporte para a geração de QRCode. Geradores de QRCode facilitam a configuração de 2FA. Este documento o orientará durante a adição de [código QR](https://wikipedia.org/wiki/QR_code) geração para a página de configuração 2FA.

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a>Adicionar códigos QR para a página de configuração 2FA

Essas instruções usam *qrcode.js* do repositório https://davidshimjs.github.io/qrcodejs/.

* Baixe o [biblioteca de javascript qrcode.js](https://davidshimjs.github.io/qrcodejs/) para o `wwwroot\lib` pasta em seu projeto.

* No *Pages\Account\Manage\EnableAuthenticator.cshtml* de arquivo, localize o `Scripts` seção no final do arquivo:

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* Atualização de `Scripts` seção para adicionar uma referência para o `qrcodejs` biblioteca que você adicionou e uma chamada para gerar o código QR. Ele deve ser da seguinte maneira:

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")

    <script type="text/javascript" src="/lib/qrcode.js"></script>
    <script type="text/javascript">
        new QRCode(document.getElementById("qrCode"),
            {
                text: "@Html.Raw(Model.AuthenticatorUri)",
                width: 150,
                height: 150
            });
    </script>
}
```

* Exclua o parágrafo que vincula a essas instruções.

Executar seu aplicativo e certifique-se de que você pode escanear o código QR e validar o código que é o autenticador.

## <a name="change-the-site-name-in-the-qr-code"></a>Alterar o nome do site no código QR

O nome do site no código QR é obtido do nome do projeto que você escolher ao criar seu projeto. Você pode alterá-lo, procurando o `GenerateQrCodeUri(string email, string unformattedKey)` método o *EnableAuthenticator.cshtml.cs* arquivo. 

O código padrão do modelo terá a seguinte aparência:

```c#
private string GenerateQrCodeUri(string email, string unformattedKey)
{
    return string.Format(
        AuthenicatorUriFormat,
        _urlEncoder.Encode("Razor Pages"),
        _urlEncoder.Encode(email),
        unformattedKey);
}
```

O segundo parâmetro na chamada para `string.Format` é o nome do site, obtido com o nome da solução. Ele pode ser alterado para qualquer valor, mas deve ser sempre codificados de URL.

## <a name="using-a-different-qr-code-library"></a>Usando uma biblioteca de código QR diferente

Você pode substituir a biblioteca de código QR por sua biblioteca preferencial. O HTML não contém um `qrCode` fornece do elemento no qual você pode colocar um código QR por qualquer mecanismo sua biblioteca.

A URL formatada corretamente para o código QR está disponível na:

* `AuthenticatorUri`propriedade do modelo.
* `data-url`propriedade no `qrCodeData` elemento. 

Use `@Html.Raw` para acessar a propriedade de modelo em um modo de exibição (caso contrário, o e comercial na url será codificado double e o parâmetro do rótulo do código QR será ignorado).
