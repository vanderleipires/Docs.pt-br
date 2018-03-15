---
uid: web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
title: "Processamento da Web do ASP.NET páginas de Sites (Razor) para dispositivos móveis | Microsoft Docs"
author: tfitzmac
description: "Este artigo descreve como criar páginas em um site de páginas da Web do ASP.NET (Razor) que serão renderizados corretamente em dispositivos móveis. Você aprenderá: como você..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: f15ab392-c05e-4269-83bf-7c6d2b8c8ec8
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
msc.type: authoredcontent
ms.openlocfilehash: 899bbdef82d689be81cd77ea6805e0484fb614aa
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/15/2018
---
<a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a><span data-ttu-id="14bc2-104">Renderização de páginas (Razor) Sites do ASP.NET para dispositivos móveis</span><span class="sxs-lookup"><span data-stu-id="14bc2-104">Rendering ASP.NET Web Pages (Razor) Sites for Mobile Devices</span></span>
====================
<span data-ttu-id="14bc2-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="14bc2-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="14bc2-106">Este artigo descreve como criar páginas em um site de páginas da Web do ASP.NET (Razor) que serão renderizados corretamente em dispositivos móveis.</span><span class="sxs-lookup"><span data-stu-id="14bc2-106">This article describes how to create pages in an ASP.NET Web Pages (Razor) site that will render appropriately on mobile devices.</span></span>
> 
> <span data-ttu-id="14bc2-107">O que você aprenderá:</span><span class="sxs-lookup"><span data-stu-id="14bc2-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="14bc2-108">Como usar uma convenção de nomenclatura para especificar que uma página foi desenvolvido especificamente para dispositivos móveis.</span><span class="sxs-lookup"><span data-stu-id="14bc2-108">How to use a naming convention to specify that a page is designed specifically for mobile devices.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="14bc2-109">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="14bc2-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="14bc2-110">Páginas da Web do ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="14bc2-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="14bc2-111">Este tutorial também funciona com 2 de páginas da Web do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="14bc2-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<span data-ttu-id="14bc2-112">Páginas da Web ASP.NET permite criar exibições personalizadas para processar o conteúdo no celular ou outros dispositivos.</span><span class="sxs-lookup"><span data-stu-id="14bc2-112">ASP.NET Web Pages lets you create custom displays for rendering content on mobile or other devices.</span></span>

<span data-ttu-id="14bc2-113">É a maneira mais simples para criar a página específica do dispositivo em um site de páginas da Web ASP.NET usando um padrão de nomenclatura de arquivo como este: *FileName. **Mobile**. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="14bc2-113">The simplest way to create device-specific page in an ASP.NET Web Pages site is by using a file-naming pattern like this: *FileName.**Mobile**.cshtml*.</span></span> <span data-ttu-id="14bc2-114">Você pode criar duas versões de uma página (por exemplo, um denominado *MyFile.cshtml* e um chamado *MyFile.Mobile.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="14bc2-114">You can create two versions of a page (for example, one named *MyFile.cshtml* and one named *MyFile.Mobile.cshtml*).</span></span> <span data-ttu-id="14bc2-115">No tempo de execução, quando um dispositivo móvel solicita *MyFile.cshtml*, ASP.NET processa o conteúdo de *MyFile.Mobile.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="14bc2-115">At run time, when a mobile device requests *MyFile.cshtml*, ASP.NET renders the content from *MyFile.Mobile.cshtml*.</span></span> <span data-ttu-id="14bc2-116">Caso contrário, *MyFile.cshtml* é renderizado.</span><span class="sxs-lookup"><span data-stu-id="14bc2-116">Otherwise, *MyFile.cshtml* is rendered.</span></span>

<span data-ttu-id="14bc2-117">O exemplo a seguir mostra como habilitar renderização móvel com a adição de uma página de conteúdo para dispositivos móveis.</span><span class="sxs-lookup"><span data-stu-id="14bc2-117">The following example shows how to enable mobile rendering by adding a content page for mobile devices.</span></span> <span data-ttu-id="14bc2-118">*Page1.cshtml* contém conteúdo mais de uma barra lateral de navegação.</span><span class="sxs-lookup"><span data-stu-id="14bc2-118">*Page1.cshtml* contains content plus a navigation sidebar.</span></span> <span data-ttu-id="14bc2-119">*Page1.Mobile.cshtml* contém o mesmo conteúdo, mas omite a barra lateral.</span><span class="sxs-lookup"><span data-stu-id="14bc2-119">*Page1.Mobile.cshtml* contains the same content, but omits the sidebar.</span></span>

1. <span data-ttu-id="14bc2-120">Em um site de páginas da Web ASP.NET, crie um arquivo chamado *Page1.cshtml* e substitua o conteúdo atual pela seguinte marcação.</span><span class="sxs-lookup"><span data-stu-id="14bc2-120">In an ASP.NET Web Pages site, create a file named *Page1.cshtml* and replace the current content with following markup.</span></span>

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. <span data-ttu-id="14bc2-121">Crie um arquivo chamado *Page1.Mobile.cshtml* e substitua o conteúdo existente com a seguinte marcação.</span><span class="sxs-lookup"><span data-stu-id="14bc2-121">Create a file named *Page1.Mobile.cshtml* and replace the existing content with the following markup.</span></span> <span data-ttu-id="14bc2-122">Observe que a versão móvel da página omite a seção de navegação para renderização melhor em uma tela menor.</span><span class="sxs-lookup"><span data-stu-id="14bc2-122">Notice that the mobile version of the page omits the navigation section for better rendering on a smaller screen.</span></span>

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. <span data-ttu-id="14bc2-123">Executar um navegador da área de trabalho e navegue até *Page1.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="14bc2-123">Run a desktop browser and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="14bc2-124">![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="14bc2-124">![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)</span></span>
4. <span data-ttu-id="14bc2-125">Executar um navegador móvel (ou um emulador de dispositivo móvel) e navegue até *Page1.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="14bc2-125">Run a mobile browser (or a mobile device emulator) and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="14bc2-126">(Observe que você não incluir *.mobile.*</span><span class="sxs-lookup"><span data-stu-id="14bc2-126">(Notice that you do not include *.mobile.*</span></span> <span data-ttu-id="14bc2-127">como parte da URL). Mesmo que a solicitação é para *Page1.cshtml*, o ASP.NET processa *Page1.Mobile.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="14bc2-127">as part of the URL.) Even though the request is to *Page1.cshtml*, ASP.NET renders *Page1.Mobile.cshtml*.</span></span>

    ![mobilesites-2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="14bc2-129">Para testar as páginas de celular, você pode usar um simulador de dispositivo móvel que é executado em um computador desktop.</span><span class="sxs-lookup"><span data-stu-id="14bc2-129">To test mobile pages, you can use a mobile device simulator that runs on a desktop computer.</span></span> <span data-ttu-id="14bc2-130">Essa ferramenta permite que você teste páginas da web, como seria aparecem em dispositivos móveis (isto é, normalmente com muito menor Exibir área).</span><span class="sxs-lookup"><span data-stu-id="14bc2-130">This tool lets you test web pages as they would look on mobile devices (that is, typically with a much smaller display area).</span></span> <span data-ttu-id="14bc2-131">É um exemplo de um simulador de [complemento alternador de agente do usuário](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) para Mozilla Firefox, que permite que você emular vários navegadores móveis de uma versão de área de trabalho do Firefox.</span><span class="sxs-lookup"><span data-stu-id="14bc2-131">One example of a simulator is the [User Agent Switcher add-on](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) for Mozilla Firefox, which lets you emulate various mobile browsers from a desktop version of Firefox.</span></span>


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="14bc2-132">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="14bc2-132">Additional Resources</span></span>


<span data-ttu-id="14bc2-133">[Emulador do Windows Phone](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)</span><span class="sxs-lookup"><span data-stu-id="14bc2-133">[Windows Phone Emulator](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)</span></span>
