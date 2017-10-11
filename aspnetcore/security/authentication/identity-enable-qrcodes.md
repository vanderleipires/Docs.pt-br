---
title: "Habilitar a geração de código QR para aplicativos de autenticador no núcleo do ASP.NET"
author: rick-anderson
description: "Habilitar a geração de código QR para aplicativos de autenticador no núcleo do ASP.NET"
keywords: "Núcleo do ASP.NET, MVC, geração de código QR, o autenticador, 2FA"
ms.author: riande
manager: wpickett
ms.date: 09/24/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: 01bb5597033fef7e1cb08e980c81d37d88ed253e
ms.sourcegitcommit: ab91aad2680efc4eb5c0642746e2b981db7f81b8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/10/2017
---
# <a name="enabling-qr-code-generation-for-authenticator-apps-in-aspnet-core"></a><span data-ttu-id="4d49e-104">Habilitar a geração de código QR para aplicativos de autenticador no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="4d49e-104">Enabling QR Code generation for authenticator apps in ASP.NET Core</span></span>

<span data-ttu-id="4d49e-105">Observação: Este tópico se aplica ao ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="4d49e-105">Note: This topic applies to ASP.NET Core 2.x</span></span>

<span data-ttu-id="4d49e-106">ASP.NET Core é fornecido com suporte para aplicativos de autenticador para autenticação individual.</span><span class="sxs-lookup"><span data-stu-id="4d49e-106">ASP.NET Core ships with support for authenticator applications for individual authentication.</span></span> <span data-ttu-id="4d49e-107">Dois aplicativos factor authentication (2FA) autenticador, usando um baseada em tempo de uso único senha algoritmo (TOTP), são o setor abordagem para 2FA recomendado.</span><span class="sxs-lookup"><span data-stu-id="4d49e-107">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approach for 2FA.</span></span> <span data-ttu-id="4d49e-108">2FA usar TOTP é preferível à 2FA do SMS.</span><span class="sxs-lookup"><span data-stu-id="4d49e-108">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="4d49e-109">Um aplicativo autenticador fornece um código de 6 a 8 dígitos que os usuários devem digitar depois de confirmar seu nome de usuário e senha.</span><span class="sxs-lookup"><span data-stu-id="4d49e-109">An authenticator app provides a 6 to 8 digit code which users must enter after confirming their username and password.</span></span> <span data-ttu-id="4d49e-110">Normalmente, um aplicativo autenticador é instalado em um Smartphone.</span><span class="sxs-lookup"><span data-stu-id="4d49e-110">Typically an authenticator app is installed on a smart phone.</span></span>

<span data-ttu-id="4d49e-111">Os modelos de aplicativo web ASP.NET Core autenticadores de suporte, mas não oferecem suporte para a geração de QRCode.</span><span class="sxs-lookup"><span data-stu-id="4d49e-111">The ASP.NET Core web app templates support authenticators, but do not provide support for QRCode generation.</span></span> <span data-ttu-id="4d49e-112">Geradores de QRCode facilitam a configuração de 2FA.</span><span class="sxs-lookup"><span data-stu-id="4d49e-112">QRCode generators ease the setup of 2FA.</span></span> <span data-ttu-id="4d49e-113">Este documento o orientará durante a adição de [código QR](https://wikipedia.org/wiki/QR_code) geração para a página de configuração 2FA.</span><span class="sxs-lookup"><span data-stu-id="4d49e-113">This document will guide you through adding [QR Code](https://wikipedia.org/wiki/QR_code) generation to the 2FA configuration page.</span></span>

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a><span data-ttu-id="4d49e-114">Adicionar códigos QR para a página de configuração 2FA</span><span class="sxs-lookup"><span data-stu-id="4d49e-114">Adding QR Codes to the 2FA configuration page</span></span>

<span data-ttu-id="4d49e-115">Essas instruções usam *qrcode.js* do repositório https://davidshimjs.github.io/qrcodejs/.</span><span class="sxs-lookup"><span data-stu-id="4d49e-115">These instructions use *qrcode.js* from the https://davidshimjs.github.io/qrcodejs/ repo.</span></span>

* <span data-ttu-id="4d49e-116">Baixe o [biblioteca de javascript qrcode.js](https://davidshimjs.github.io/qrcodejs/) para o `wwwroot\lib` pasta em seu projeto.</span><span class="sxs-lookup"><span data-stu-id="4d49e-116">Download the [qrcode.js javascript library](https://davidshimjs.github.io/qrcodejs/) to the `wwwroot\lib` folder in your project.</span></span>

* <span data-ttu-id="4d49e-117">Em *Pages\Account\Manage\EnableAuthenticator.cshtml* (páginas Razor) ou *Views\Manage\EnableAuthenticator.cshtml* (MVC), localize o `Scripts` seção no final do arquivo:</span><span class="sxs-lookup"><span data-stu-id="4d49e-117">In *Pages\Account\Manage\EnableAuthenticator.cshtml* (Razor Pages) or *Views\Manage\EnableAuthenticator.cshtml* (MVC), locate the `Scripts` section at the end of the file:</span></span>

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* <span data-ttu-id="4d49e-118">Atualização de `Scripts` seção para adicionar uma referência para o `qrcodejs` biblioteca que você adicionou e uma chamada para gerar o código QR.</span><span class="sxs-lookup"><span data-stu-id="4d49e-118">Update the `Scripts` section to add a reference to the `qrcodejs` library you added and a call to generate the QR Code.</span></span> <span data-ttu-id="4d49e-119">Ele deve ser da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="4d49e-119">It should look as follows:</span></span>

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

* <span data-ttu-id="4d49e-120">Exclua o parágrafo que vincula a essas instruções.</span><span class="sxs-lookup"><span data-stu-id="4d49e-120">Delete the paragraph which links you to these instructions.</span></span>

<span data-ttu-id="4d49e-121">Executar seu aplicativo e certifique-se de que você pode escanear o código QR e validar o código que é o autenticador.</span><span class="sxs-lookup"><span data-stu-id="4d49e-121">Run your app and ensure that you can scan the QR code and validate the code the authenticator proves.</span></span>

## <a name="change-the-site-name-in-the-qr-code"></a><span data-ttu-id="4d49e-122">Alterar o nome do site no código QR</span><span class="sxs-lookup"><span data-stu-id="4d49e-122">Change the site name in the QR Code</span></span>

<span data-ttu-id="4d49e-123">O nome do site no código QR é obtido do nome do projeto que você escolher ao criar seu projeto.</span><span class="sxs-lookup"><span data-stu-id="4d49e-123">The site name in the QR Code is taken from the project name you choose when initially creating your project.</span></span> <span data-ttu-id="4d49e-124">Você pode alterá-lo, procurando o `GenerateQrCodeUri(string email, string unformattedKey)` método o *Pages\Account\Manage\EnableAuthenticator.cshtml.cs* arquivo (páginas Razor) ou o *Controllers\AccountController.cs* arquivo (MVC).</span><span class="sxs-lookup"><span data-stu-id="4d49e-124">You can change it by looking for the `GenerateQrCodeUri(string email, string unformattedKey)` method in the *Pages\Account\Manage\EnableAuthenticator.cshtml.cs* (Razor Pages) file or the *Controllers\AccountController.cs* (MVC) file.</span></span> 

<span data-ttu-id="4d49e-125">O código padrão do modelo terá a seguinte aparência:</span><span class="sxs-lookup"><span data-stu-id="4d49e-125">The default code from the template looks as follows:</span></span>

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

<span data-ttu-id="4d49e-126">O segundo parâmetro na chamada para `string.Format` é o nome do site, obtido com o nome da solução.</span><span class="sxs-lookup"><span data-stu-id="4d49e-126">The second parameter in the call to `string.Format` is your site name, taken from your solution name.</span></span> <span data-ttu-id="4d49e-127">Ele pode ser alterado para qualquer valor, mas deve ser sempre codificados de URL.</span><span class="sxs-lookup"><span data-stu-id="4d49e-127">It can be changed to any value, but it must always be URL encoded.</span></span>

## <a name="using-a-different-qr-code-library"></a><span data-ttu-id="4d49e-128">Usando uma biblioteca de código QR diferente</span><span class="sxs-lookup"><span data-stu-id="4d49e-128">Using a different QR Code library</span></span>

<span data-ttu-id="4d49e-129">Você pode substituir a biblioteca de código QR por sua biblioteca preferencial.</span><span class="sxs-lookup"><span data-stu-id="4d49e-129">You can replace the QR Code library with your preferred library.</span></span> <span data-ttu-id="4d49e-130">O HTML não contém um `qrCode` fornece do elemento no qual você pode colocar um código QR por qualquer mecanismo sua biblioteca.</span><span class="sxs-lookup"><span data-stu-id="4d49e-130">The HTML contains a `qrCode` element into which you can place a QR Code by whatever mechanism your library provides.</span></span>

<span data-ttu-id="4d49e-131">A URL formatada corretamente para o código QR está disponível na:</span><span class="sxs-lookup"><span data-stu-id="4d49e-131">The correctly formatted URL for the QR Code is available in the:</span></span>

* <span data-ttu-id="4d49e-132">`AuthenticatorUri`propriedade do modelo.</span><span class="sxs-lookup"><span data-stu-id="4d49e-132">`AuthenticatorUri` property of the model.</span></span>
* <span data-ttu-id="4d49e-133">`data-url`propriedade no `qrCodeData` elemento.</span><span class="sxs-lookup"><span data-stu-id="4d49e-133">`data-url` property in the `qrCodeData` element.</span></span> 

<span data-ttu-id="4d49e-134">Use `@Html.Raw` para acessar a propriedade de modelo em um modo de exibição (caso contrário, o e comercial na url será codificado double e o parâmetro do rótulo do código QR será ignorado).</span><span class="sxs-lookup"><span data-stu-id="4d49e-134">Use `@Html.Raw` to access the model property in a view (otherwise the ampersands in the url will be double encoded and the label parameter of the QR Code will be ignored).</span></span>

## <a name="totp-client-and-server-time-skew"></a><span data-ttu-id="4d49e-135">TOTP cliente e servidor diferença de horário</span><span class="sxs-lookup"><span data-stu-id="4d49e-135">TOTP client and server time skew</span></span>

<span data-ttu-id="4d49e-136">Autenticação TOTP depende do dispositivo do servidor e o autenticador tendo um horário com precisão.</span><span class="sxs-lookup"><span data-stu-id="4d49e-136">TOTP authentication depends on both the server and authenticator device having an accurate time.</span></span> <span data-ttu-id="4d49e-137">Tokens apenas últimos 30 segundos.</span><span class="sxs-lookup"><span data-stu-id="4d49e-137">Tokens only last for 30 seconds.</span></span> <span data-ttu-id="4d49e-138">Se os logons de 2FA TOTP estiverem falhando, verifique se a hora do servidor, sincronização e precisão preferencialmente para um serviço NTP preciso.</span><span class="sxs-lookup"><span data-stu-id="4d49e-138">If TOTP 2FA logins are failing, check that the server time is accurate, and preferably synchronized to an accurate NTP service.</span></span>
