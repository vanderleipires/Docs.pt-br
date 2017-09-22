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
ms.openlocfilehash: 42b4b31d4a34bc54c2c9338db802dcc0801ee85a
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/22/2017
---
# <a name="enabling-qr-code-generation-for-authenticator-apps-in-aspnet-core"></a><span data-ttu-id="5daa5-104">Habilitar a geração de código QR para aplicativos de autenticador no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="5daa5-104">Enabling QR Code generation for authenticator apps in ASP.NET Core</span></span>

<span data-ttu-id="5daa5-105">Observação: Este tópico se aplica ao ASP.NET Core 2. x com páginas Razor.</span><span class="sxs-lookup"><span data-stu-id="5daa5-105">Note: This topic applies to ASP.NET Core 2.x with Razor Pages.</span></span>

<span data-ttu-id="5daa5-106">ASP.NET Core é fornecido com suporte para aplicativos de autenticador para autenticação individual.</span><span class="sxs-lookup"><span data-stu-id="5daa5-106">ASP.NET Core ships with support for authenticator applications for individual authentication.</span></span> <span data-ttu-id="5daa5-107">Dois aplicativos factor authentication (2FA) autenticador, usando um baseada em tempo de uso único senha algoritmo (TOTP), são o setor recomendado approch para 2FA.</span><span class="sxs-lookup"><span data-stu-id="5daa5-107">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approch for 2FA.</span></span> <span data-ttu-id="5daa5-108">2FA usar TOTP é preferível à 2FA do SMS.</span><span class="sxs-lookup"><span data-stu-id="5daa5-108">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="5daa5-109">Um aplicativo autenticador fornece um código de 6 a 8 dígitos que os usuários devem digitar depois de confirmar seu nome de usuário e senha.</span><span class="sxs-lookup"><span data-stu-id="5daa5-109">An authenticator app provides a 6 to 8 digit code which users must enter after confirming their username and password.</span></span> <span data-ttu-id="5daa5-110">Normalmente, um aplicativo autenticador é instalado em um Smartphone.</span><span class="sxs-lookup"><span data-stu-id="5daa5-110">Typically an authenticator app is installed on a smart phone.</span></span>

<span data-ttu-id="5daa5-111">Os modelos de aplicativo web ASP.NET Core autenticadores de suporte, mas não oferecem suporte para a geração de QRCode.</span><span class="sxs-lookup"><span data-stu-id="5daa5-111">The ASP.NET Core web app templates support authenticators, but do not provide support for QRCode generation.</span></span> <span data-ttu-id="5daa5-112">Geradores de QRCode facilitam a configuração de 2FA.</span><span class="sxs-lookup"><span data-stu-id="5daa5-112">QRCode generators ease the setup of 2FA.</span></span> <span data-ttu-id="5daa5-113">Este documento o orientará durante a adição de [código QR](https://wikipedia.org/wiki/QR_code) geração para a página de configuração 2FA.</span><span class="sxs-lookup"><span data-stu-id="5daa5-113">This document will guide you through adding [QR Code](https://wikipedia.org/wiki/QR_code) generation to the 2FA configuration page.</span></span>

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a><span data-ttu-id="5daa5-114">Adicionar códigos QR para a página de configuração 2FA</span><span class="sxs-lookup"><span data-stu-id="5daa5-114">Adding QR Codes to the 2FA configuration page</span></span>

<span data-ttu-id="5daa5-115">Essas instruções usam *qrcode.js* do repositório https://davidshimjs.github.io/qrcodejs/.</span><span class="sxs-lookup"><span data-stu-id="5daa5-115">These instructions use *qrcode.js* from the https://davidshimjs.github.io/qrcodejs/ repo.</span></span>

* <span data-ttu-id="5daa5-116">Baixe o [biblioteca de javascript qrcode.js](https://davidshimjs.github.io/qrcodejs/) para o `wwwroot\lib` pasta em seu projeto.</span><span class="sxs-lookup"><span data-stu-id="5daa5-116">Download the  [qrcode.js javascript library](https://davidshimjs.github.io/qrcodejs/) to the `wwwroot\lib` folder in your project.</span></span>

* <span data-ttu-id="5daa5-117">No *Pages\Account\Manage\EnableAuthenticator.cshtml* de arquivo, localize o `Scripts` seção no final do arquivo:</span><span class="sxs-lookup"><span data-stu-id="5daa5-117">In the *Pages\Account\Manage\EnableAuthenticator.cshtml* file, locate the `Scripts` section at the end of the file:</span></span>

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* <span data-ttu-id="5daa5-118">Atualização de `Scripts` seção para adicionar uma referência para o `qrcodejs` biblioteca que você adicionou e uma chamada para gerar o código QR.</span><span class="sxs-lookup"><span data-stu-id="5daa5-118">Update the `Scripts` section to add a reference to the `qrcodejs` library you added and a call to generate the QR Code.</span></span> <span data-ttu-id="5daa5-119">Ele deve ser da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="5daa5-119">It should look as follows:</span></span>

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

* <span data-ttu-id="5daa5-120">Exclua o parágrafo que vincula a essas instruções.</span><span class="sxs-lookup"><span data-stu-id="5daa5-120">Delete the paragraph which links you to these instructions.</span></span>

<span data-ttu-id="5daa5-121">Executar seu aplicativo e certifique-se de que você pode escanear o código QR e validar o código que é o autenticador.</span><span class="sxs-lookup"><span data-stu-id="5daa5-121">Run your app and ensure that you can scan the QR code and validate the code the authenticator proves.</span></span>

## <a name="change-the-site-name-in-the-qr-code"></a><span data-ttu-id="5daa5-122">Alterar o nome do site no código QR</span><span class="sxs-lookup"><span data-stu-id="5daa5-122">Change the site name in the QR Code</span></span>

<span data-ttu-id="5daa5-123">O nome do site no código QR é obtido do nome do projeto que você escolher ao criar seu projeto.</span><span class="sxs-lookup"><span data-stu-id="5daa5-123">The site name in the QR Code is taken from the project name you choose when initially creating your project.</span></span> <span data-ttu-id="5daa5-124">Você pode alterá-lo, procurando o `GenerateQrCodeUri(string email, string unformattedKey)` método o *EnableAuthenticator.cshtml.cs* arquivo.</span><span class="sxs-lookup"><span data-stu-id="5daa5-124">You can change it by looking for the `GenerateQrCodeUri(string email, string unformattedKey)` method in  the *EnableAuthenticator.cshtml.cs* file.</span></span> 

<span data-ttu-id="5daa5-125">O código padrão do modelo terá a seguinte aparência:</span><span class="sxs-lookup"><span data-stu-id="5daa5-125">The default code from the template looks as follows:</span></span>

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

<span data-ttu-id="5daa5-126">O segundo parâmetro na chamada para `string.Format` é o nome do site, obtido com o nome da solução.</span><span class="sxs-lookup"><span data-stu-id="5daa5-126">The second parameter in the call to `string.Format` is your site name, taken from your solution name.</span></span> <span data-ttu-id="5daa5-127">Ele pode ser alterado para qualquer valor, mas deve ser sempre codificados de URL.</span><span class="sxs-lookup"><span data-stu-id="5daa5-127">It can be changed to any value, but it must always be URL encoded.</span></span>

## <a name="using-a-different-qr-code-library"></a><span data-ttu-id="5daa5-128">Usando uma biblioteca de código QR diferente</span><span class="sxs-lookup"><span data-stu-id="5daa5-128">Using a different QR Code library</span></span>

<span data-ttu-id="5daa5-129">Você pode substituir a biblioteca de código QR por sua biblioteca preferencial.</span><span class="sxs-lookup"><span data-stu-id="5daa5-129">You can replace the QR Code library with your preferred library.</span></span> <span data-ttu-id="5daa5-130">O HTML não contém um `qrCode` fornece do elemento no qual você pode colocar um código QR por qualquer mecanismo sua biblioteca.</span><span class="sxs-lookup"><span data-stu-id="5daa5-130">The HTML contains a `qrCode` element into which you can place a QR Code by whatever mechanism your library provides.</span></span>

<span data-ttu-id="5daa5-131">A URL formatada corretamente para o código QR está disponível na:</span><span class="sxs-lookup"><span data-stu-id="5daa5-131">The correctly formatted URL for the QR Code is available in the:</span></span>

* <span data-ttu-id="5daa5-132">`AuthenticatorUri`propriedade do modelo.</span><span class="sxs-lookup"><span data-stu-id="5daa5-132">`AuthenticatorUri` property of the model.</span></span>
* <span data-ttu-id="5daa5-133">`data-url`propriedade no `qrCodeData` elemento.</span><span class="sxs-lookup"><span data-stu-id="5daa5-133">`data-url` property in the `qrCodeData` element.</span></span> 

<span data-ttu-id="5daa5-134">Use `@Html.Raw` para acessar a propriedade de modelo em um modo de exibição (caso contrário, o e comercial na url será codificado double e o parâmetro do rótulo do código QR será ignorado).</span><span class="sxs-lookup"><span data-stu-id="5daa5-134">Use `@Html.Raw` to access the model property in a view (otherwise the ampersands in the url will be double encoded and the label parameter of the QR Code will be ignored).</span></span>
