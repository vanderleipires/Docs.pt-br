---
uid: web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
title: Renderização da Web do ASP.NET (Razor) Sites de páginas para dispositivos móveis | Microsoft Docs
author: tfitzmac
description: 'Este artigo descreve como criar páginas em um site de páginas da Web do ASP.NET (Razor) que será renderizado corretamente em dispositivos móveis. O que você vai aprender: como você...'
ms.author: riande
ms.date: 02/17/2014
ms.assetid: f15ab392-c05e-4269-83bf-7c6d2b8c8ec8
msc.legacyurl: /web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
msc.type: authoredcontent
ms.openlocfilehash: 7159283efcd4a82d5dff68f2d9e315f804e7cdcb
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831116"
---
<a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a><span data-ttu-id="3cf9b-104">Renderização ASP.NET Web Pages (Razor) Sites para dispositivos móveis</span><span class="sxs-lookup"><span data-stu-id="3cf9b-104">Rendering ASP.NET Web Pages (Razor) Sites for Mobile Devices</span></span>
====================
<span data-ttu-id="3cf9b-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="3cf9b-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="3cf9b-106">Este artigo descreve como criar páginas em um site de páginas da Web do ASP.NET (Razor) que será renderizado corretamente em dispositivos móveis.</span><span class="sxs-lookup"><span data-stu-id="3cf9b-106">This article describes how to create pages in an ASP.NET Web Pages (Razor) site that will render appropriately on mobile devices.</span></span>
> 
> <span data-ttu-id="3cf9b-107">O que você aprenderá:</span><span class="sxs-lookup"><span data-stu-id="3cf9b-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="3cf9b-108">Como usar uma convenção de nomenclatura para especificar que uma página foi projetado especificamente para dispositivos móveis.</span><span class="sxs-lookup"><span data-stu-id="3cf9b-108">How to use a naming convention to specify that a page is designed specifically for mobile devices.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="3cf9b-109">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="3cf9b-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="3cf9b-110">Páginas da Web do ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="3cf9b-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="3cf9b-111">Este tutorial também funciona com ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="3cf9b-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<span data-ttu-id="3cf9b-112">Páginas da Web ASP.NET permite criar exibições personalizadas para processar o conteúdo em celular ou outros dispositivos.</span><span class="sxs-lookup"><span data-stu-id="3cf9b-112">ASP.NET Web Pages lets you create custom displays for rendering content on mobile or other devices.</span></span>

<span data-ttu-id="3cf9b-113">A maneira mais simples para criar a página específica do dispositivo em um site de páginas da Web ASP.NET é usando um padrão de nomenclatura de arquivo como este: <em>FileName.</em> <em>Mobile</em><em>. cshtml</em>.</span><span class="sxs-lookup"><span data-stu-id="3cf9b-113">The simplest way to create device-specific page in an ASP.NET Web Pages site is by using a file-naming pattern like this: <em>FileName.</em><em>Mobile</em><em>.cshtml</em>.</span></span> <span data-ttu-id="3cf9b-114">Você pode criar duas versões de uma página (por exemplo, uma nomeada <em>MyFile.cshtml</em> e outro chamado <em>MyFile.Mobile.cshtml</em>).</span><span class="sxs-lookup"><span data-stu-id="3cf9b-114">You can create two versions of a page (for example, one named <em>MyFile.cshtml</em> and one named <em>MyFile.Mobile.cshtml</em>).</span></span> <span data-ttu-id="3cf9b-115">No tempo de execução, quando um dispositivo móvel solicita <em>MyFile.cshtml</em>, o ASP.NET processa o conteúdo de <em>MyFile.Mobile.cshtml</em>.</span><span class="sxs-lookup"><span data-stu-id="3cf9b-115">At run time, when a mobile device requests <em>MyFile.cshtml</em>, ASP.NET renders the content from <em>MyFile.Mobile.cshtml</em>.</span></span> <span data-ttu-id="3cf9b-116">Caso contrário, <em>MyFile.cshtml</em> é renderizado.</span><span class="sxs-lookup"><span data-stu-id="3cf9b-116">Otherwise, <em>MyFile.cshtml</em> is rendered.</span></span>

<span data-ttu-id="3cf9b-117">O exemplo a seguir mostra como habilitar a renderização móvel com a adição de uma página de conteúdo para dispositivos móveis.</span><span class="sxs-lookup"><span data-stu-id="3cf9b-117">The following example shows how to enable mobile rendering by adding a content page for mobile devices.</span></span> <span data-ttu-id="3cf9b-118">*Page1.cshtml* contém conteúdo além de uma barra lateral de navegação.</span><span class="sxs-lookup"><span data-stu-id="3cf9b-118">*Page1.cshtml* contains content plus a navigation sidebar.</span></span> <span data-ttu-id="3cf9b-119">*Page1.Mobile.cshtml* contém o mesmo conteúdo, mas omite a barra lateral.</span><span class="sxs-lookup"><span data-stu-id="3cf9b-119">*Page1.Mobile.cshtml* contains the same content, but omits the sidebar.</span></span>

1. <span data-ttu-id="3cf9b-120">Em um site de páginas da Web ASP.NET, crie um arquivo chamado *Page1.cshtml* e substitua o conteúdo atual pela seguinte marcação.</span><span class="sxs-lookup"><span data-stu-id="3cf9b-120">In an ASP.NET Web Pages site, create a file named *Page1.cshtml* and replace the current content with following markup.</span></span>

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. <span data-ttu-id="3cf9b-121">Crie um arquivo chamado *Page1.Mobile.cshtml* e substitua o conteúdo existente com a marcação a seguir.</span><span class="sxs-lookup"><span data-stu-id="3cf9b-121">Create a file named *Page1.Mobile.cshtml* and replace the existing content with the following markup.</span></span> <span data-ttu-id="3cf9b-122">Observe que a versão móvel da página omite a seção de navegação para a renderização de melhor em uma tela menor.</span><span class="sxs-lookup"><span data-stu-id="3cf9b-122">Notice that the mobile version of the page omits the navigation section for better rendering on a smaller screen.</span></span>

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. <span data-ttu-id="3cf9b-123">Execute um navegador de desktop e navegue até *Page1.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="3cf9b-123">Run a desktop browser and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="3cf9b-124">![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="3cf9b-124">![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)</span></span>
4. <span data-ttu-id="3cf9b-125">Execute um navegador móvel (ou um emulador de dispositivo móvel) e navegue até *Page1.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="3cf9b-125">Run a mobile browser (or a mobile device emulator) and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="3cf9b-126">(Observe que você não incluir *Mobile.*</span><span class="sxs-lookup"><span data-stu-id="3cf9b-126">(Notice that you do not include *.mobile.*</span></span> <span data-ttu-id="3cf9b-127">como parte da URL). Mesmo que a solicitação é para *Page1.cshtml*, o ASP.NET processa *Page1.Mobile.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="3cf9b-127">as part of the URL.) Even though the request is to *Page1.cshtml*, ASP.NET renders *Page1.Mobile.cshtml*.</span></span>

    ![mobilesites-2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="3cf9b-129">Para testar páginas para dispositivos móveis, você pode usar um simulador de dispositivo móvel que é executado em um computador desktop.</span><span class="sxs-lookup"><span data-stu-id="3cf9b-129">To test mobile pages, you can use a mobile device simulator that runs on a desktop computer.</span></span> <span data-ttu-id="3cf9b-130">Essa ferramenta lhe permite testar páginas da web conforme eles ficaria em dispositivos móveis (isto é, normalmente com muito menor área de exibição).</span><span class="sxs-lookup"><span data-stu-id="3cf9b-130">This tool lets you test web pages as they would look on mobile devices (that is, typically with a much smaller display area).</span></span> <span data-ttu-id="3cf9b-131">Um exemplo de um simulador de é o [complemento de alternador de agente do usuário](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) para o Mozilla Firefox, que lhe permite emular vários navegadores para dispositivos móveis de uma versão da área de trabalho do Firefox.</span><span class="sxs-lookup"><span data-stu-id="3cf9b-131">One example of a simulator is the [User Agent Switcher add-on](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) for Mozilla Firefox, which lets you emulate various mobile browsers from a desktop version of Firefox.</span></span>


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="3cf9b-132">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="3cf9b-132">Additional Resources</span></span>


<span data-ttu-id="3cf9b-133">[Emulador do Windows Phone](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)</span><span class="sxs-lookup"><span data-stu-id="3cf9b-133">[Windows Phone Emulator](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)</span></span>
