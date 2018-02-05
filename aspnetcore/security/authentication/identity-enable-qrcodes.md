---
title: "Habilitar a geração de código QR para aplicativos de autenticador no núcleo do ASP.NET"
author: rick-anderson
description: "Habilitar a geração de código QR para aplicativos de autenticador no núcleo do ASP.NET"
manager: wpickett
ms.author: riande
ms.date: 09/24/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: cf941314d54aa4a7bd1724805dc62c763ca71dfb
ms.sourcegitcommit: 7a87d66cf1d01febe6635c7306f2f679434901d1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/03/2018
---
# <a name="enabling-qr-code-generation-for-authenticator-apps-in-aspnet-core"></a><span data-ttu-id="a318f-103">Habilitar a geração de código QR para aplicativos de autenticador no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a318f-103">Enabling QR Code generation for authenticator apps in ASP.NET Core</span></span>

<span data-ttu-id="a318f-104">Observação: Este tópico se aplica ao ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="a318f-104">Note: This topic applies to ASP.NET Core 2.x</span></span>

<span data-ttu-id="a318f-105">ASP.NET Core é fornecido com suporte para aplicativos de autenticador para autenticação individual.</span><span class="sxs-lookup"><span data-stu-id="a318f-105">ASP.NET Core ships with support for authenticator applications for individual authentication.</span></span> <span data-ttu-id="a318f-106">Dois aplicativos factor authentication (2FA) autenticador, usando um baseada em tempo de uso único senha algoritmo (TOTP), são o setor abordagem para 2FA recomendado.</span><span class="sxs-lookup"><span data-stu-id="a318f-106">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approach for 2FA.</span></span> <span data-ttu-id="a318f-107">2FA usar TOTP é preferível à 2FA do SMS.</span><span class="sxs-lookup"><span data-stu-id="a318f-107">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="a318f-108">Um aplicativo autenticador fornece um código de 6 a 8 dígitos que os usuários devem digitar depois de confirmar seu nome de usuário e senha.</span><span class="sxs-lookup"><span data-stu-id="a318f-108">An authenticator app provides a 6 to 8 digit code which users must enter after confirming their username and password.</span></span> <span data-ttu-id="a318f-109">Normalmente, um aplicativo autenticador é instalado em um Smartphone.</span><span class="sxs-lookup"><span data-stu-id="a318f-109">Typically an authenticator app is installed on a smart phone.</span></span>

<span data-ttu-id="a318f-110">Os modelos de aplicativo web ASP.NET Core autenticadores de suporte, mas não fornecem suporte para geração de QRCode.</span><span class="sxs-lookup"><span data-stu-id="a318f-110">The ASP.NET Core web app templates support authenticators, but don't provide support for QRCode generation.</span></span> <span data-ttu-id="a318f-111">Geradores de QRCode facilitam a configuração de 2FA.</span><span class="sxs-lookup"><span data-stu-id="a318f-111">QRCode generators ease the setup of 2FA.</span></span> <span data-ttu-id="a318f-112">Este documento o orientará durante a adição de [código QR](https://wikipedia.org/wiki/QR_code) geração para a página de configuração 2FA.</span><span class="sxs-lookup"><span data-stu-id="a318f-112">This document will guide you through adding [QR Code](https://wikipedia.org/wiki/QR_code) generation to the 2FA configuration page.</span></span>

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a><span data-ttu-id="a318f-113">Adicionar códigos QR para a página de configuração 2FA</span><span class="sxs-lookup"><span data-stu-id="a318f-113">Adding QR Codes to the 2FA configuration page</span></span>

<span data-ttu-id="a318f-114">Essas instruções usam *qrcode.js* do repositório https://davidshimjs.github.io/qrcodejs/.</span><span class="sxs-lookup"><span data-stu-id="a318f-114">These instructions use *qrcode.js* from the https://davidshimjs.github.io/qrcodejs/ repo.</span></span>

* <span data-ttu-id="a318f-115">Baixe o [biblioteca de javascript qrcode.js](https://davidshimjs.github.io/qrcodejs/) para o `wwwroot\lib` pasta em seu projeto.</span><span class="sxs-lookup"><span data-stu-id="a318f-115">Download the [qrcode.js javascript library](https://davidshimjs.github.io/qrcodejs/) to the `wwwroot\lib` folder in your project.</span></span>

* <span data-ttu-id="a318f-116">Em *Pages\Account\Manage\EnableAuthenticator.cshtml* (páginas Razor) ou *Views\Manage\EnableAuthenticator.cshtml* (MVC), localize o `Scripts` seção no final do arquivo:</span><span class="sxs-lookup"><span data-stu-id="a318f-116">In *Pages\Account\Manage\EnableAuthenticator.cshtml* (Razor Pages) or *Views\Manage\EnableAuthenticator.cshtml* (MVC), locate the `Scripts` section at the end of the file:</span></span>

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* <span data-ttu-id="a318f-117">Atualização de `Scripts` seção para adicionar uma referência para o `qrcodejs` biblioteca que você adicionou e uma chamada para gerar o código QR.</span><span class="sxs-lookup"><span data-stu-id="a318f-117">Update the `Scripts` section to add a reference to the `qrcodejs` library you added and a call to generate the QR Code.</span></span> <span data-ttu-id="a318f-118">Ele deve ser da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="a318f-118">It should look as follows:</span></span>

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

* <span data-ttu-id="a318f-119">Exclua o parágrafo que vincula a essas instruções.</span><span class="sxs-lookup"><span data-stu-id="a318f-119">Delete the paragraph which links you to these instructions.</span></span>

<span data-ttu-id="a318f-120">Executar seu aplicativo e certifique-se de que você pode escanear o código QR e validar o código que é o autenticador.</span><span class="sxs-lookup"><span data-stu-id="a318f-120">Run your app and ensure that you can scan the QR code and validate the code the authenticator proves.</span></span>

## <a name="change-the-site-name-in-the-qr-code"></a><span data-ttu-id="a318f-121">Alterar o nome do site no código QR</span><span class="sxs-lookup"><span data-stu-id="a318f-121">Change the site name in the QR Code</span></span>

<span data-ttu-id="a318f-122">O nome do site no código QR é obtido do nome do projeto que você escolher ao criar seu projeto.</span><span class="sxs-lookup"><span data-stu-id="a318f-122">The site name in the QR Code is taken from the project name you choose when initially creating your project.</span></span> <span data-ttu-id="a318f-123">Você pode alterá-lo, procurando o `GenerateQrCodeUri(string email, string unformattedKey)` método o *Pages\Account\Manage\EnableAuthenticator.cshtml.cs* arquivo (páginas Razor) ou o *Controllers\ManageController.cs* arquivo (MVC).</span><span class="sxs-lookup"><span data-stu-id="a318f-123">You can change it by looking for the `GenerateQrCodeUri(string email, string unformattedKey)` method in the *Pages\Account\Manage\EnableAuthenticator.cshtml.cs* (Razor Pages) file or the *Controllers\ManageController.cs* (MVC) file.</span></span> 

<span data-ttu-id="a318f-124">O código padrão do modelo terá a seguinte aparência:</span><span class="sxs-lookup"><span data-stu-id="a318f-124">The default code from the template looks as follows:</span></span>

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

<span data-ttu-id="a318f-125">O segundo parâmetro na chamada para `string.Format` é o nome do site, obtido com o nome da solução.</span><span class="sxs-lookup"><span data-stu-id="a318f-125">The second parameter in the call to `string.Format` is your site name, taken from your solution name.</span></span> <span data-ttu-id="a318f-126">Ele pode ser alterado para qualquer valor, mas deve ser sempre codificados de URL.</span><span class="sxs-lookup"><span data-stu-id="a318f-126">It can be changed to any value, but it must always be URL encoded.</span></span>

## <a name="using-a-different-qr-code-library"></a><span data-ttu-id="a318f-127">Usando uma biblioteca de código QR diferente</span><span class="sxs-lookup"><span data-stu-id="a318f-127">Using a different QR Code library</span></span>

<span data-ttu-id="a318f-128">Você pode substituir a biblioteca de código QR por sua biblioteca preferencial.</span><span class="sxs-lookup"><span data-stu-id="a318f-128">You can replace the QR Code library with your preferred library.</span></span> <span data-ttu-id="a318f-129">O HTML não contém um `qrCode` fornece do elemento no qual você pode colocar um código QR por qualquer mecanismo sua biblioteca.</span><span class="sxs-lookup"><span data-stu-id="a318f-129">The HTML contains a `qrCode` element into which you can place a QR Code by whatever mechanism your library provides.</span></span>

<span data-ttu-id="a318f-130">A URL formatada corretamente para o código QR está disponível na:</span><span class="sxs-lookup"><span data-stu-id="a318f-130">The correctly formatted URL for the QR Code is available in the:</span></span>

* <span data-ttu-id="a318f-131">`AuthenticatorUri`propriedade do modelo.</span><span class="sxs-lookup"><span data-stu-id="a318f-131">`AuthenticatorUri` property of the model.</span></span>
* <span data-ttu-id="a318f-132">`data-url`propriedade no `qrCodeData` elemento.</span><span class="sxs-lookup"><span data-stu-id="a318f-132">`data-url` property in the `qrCodeData` element.</span></span> 

## <a name="totp-client-and-server-time-skew"></a><span data-ttu-id="a318f-133">TOTP cliente e servidor diferença de horário</span><span class="sxs-lookup"><span data-stu-id="a318f-133">TOTP client and server time skew</span></span>

<span data-ttu-id="a318f-134">Autenticação TOTP depende do dispositivo do servidor e o autenticador tendo um horário com precisão.</span><span class="sxs-lookup"><span data-stu-id="a318f-134">TOTP authentication depends on both the server and authenticator device having an accurate time.</span></span> <span data-ttu-id="a318f-135">Tokens apenas últimos 30 segundos.</span><span class="sxs-lookup"><span data-stu-id="a318f-135">Tokens only last for 30 seconds.</span></span> <span data-ttu-id="a318f-136">Se os logons de 2FA TOTP estiverem falhando, verifique se a hora do servidor, sincronização e precisão preferencialmente para um serviço NTP preciso.</span><span class="sxs-lookup"><span data-stu-id="a318f-136">If TOTP 2FA logins are failing, check that the server time is accurate, and preferably synchronized to an accurate NTP service.</span></span>
