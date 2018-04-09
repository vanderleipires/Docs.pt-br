---
title: Habilitar a geração de código QR para aplicativos de autenticador no núcleo do ASP.NET
author: rick-anderson
description: Saiba como habilitar a geração de código QR para aplicativos de autenticador que funcionam com a autenticação de dois fatores de núcleo do ASP.NET.
manager: wpickett
ms.author: riande
ms.date: 09/24/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: c61918d42b407b01484b67d740edc7a682c3a4b0
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/22/2018
---
# <a name="enable-qr-code-generation-for-authenticator-apps-in-aspnet-core"></a>Habilitar a geração de código QR para aplicativos de autenticador no núcleo do ASP.NET

Observação: Este tópico se aplica ao ASP.NET Core 2. x

ASP.NET Core é fornecido com suporte para aplicativos de autenticador para autenticação individual. Dois aplicativos factor authentication (2FA) autenticador, usando um baseada em tempo de uso único senha algoritmo (TOTP), são o setor abordagem para 2FA recomendado. 2FA usar TOTP é preferível à 2FA do SMS. Um aplicativo autenticador fornece um código de 6 a 8 dígitos que os usuários devem digitar depois de confirmar seu nome de usuário e senha. Normalmente, um aplicativo autenticador é instalado em um Smartphone.

Os modelos de aplicativo web ASP.NET Core autenticadores de suporte, mas não fornecem suporte para geração de QRCode. Geradores de QRCode facilitam a configuração de 2FA. Este documento o orientará durante a adição de [código QR](https://wikipedia.org/wiki/QR_code) geração para a página de configuração 2FA.

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a>Adicionar códigos QR para a página de configuração 2FA

Essas instruções usam *qrcode.js* do https://davidshimjs.github.io/qrcodejs/ repositório.

* Baixe o [biblioteca de javascript qrcode.js](https://davidshimjs.github.io/qrcodejs/) para o `wwwroot\lib` pasta em seu projeto.

* Em *Pages\Account\Manage\EnableAuthenticator.cshtml* (páginas Razor) ou *Views\Manage\EnableAuthenticator.cshtml* (MVC), localize o `Scripts` seção no final do arquivo:

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* Atualização de `Scripts` seção para adicionar uma referência para o `qrcodejs` biblioteca que você adicionou e uma chamada para gerar o código QR. Ele deve ser da seguinte maneira:

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")

    <script type="text/javascript" src="~/lib/qrcode.js"></script>
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

O nome do site no código QR é obtido do nome do projeto que você escolher ao criar seu projeto. Você pode alterá-lo, procurando o `GenerateQrCodeUri(string email, string unformattedKey)` método o *Pages\Account\Manage\EnableAuthenticator.cshtml.cs* arquivo (páginas Razor) ou o *Controllers\ManageController.cs* arquivo (MVC). 

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

* `AuthenticatorUri` propriedade do modelo.
* `data-url` propriedade no `qrCodeData` elemento. 

## <a name="totp-client-and-server-time-skew"></a>TOTP cliente e servidor diferença de horário

Autenticação TOTP depende do dispositivo do servidor e o autenticador tendo um horário com precisão. Tokens apenas últimos 30 segundos. Se os logons de 2FA TOTP estiverem falhando, verifique se a hora do servidor, sincronização e precisão preferencialmente para um serviço NTP preciso.
