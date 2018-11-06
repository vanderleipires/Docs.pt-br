---
uid: web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
title: Executando versões diferentes do ASP.NET Web Pages (Razor) lado a lado | Microsoft Docs
author: Rick-Anderson
description: Este artigo explica como executar o ASP.NET Web Pages (Razor) sites no mesmo computador ou servidor quando os sites são configurados para usar diferentes versões...
ms.author: riande
ms.date: 02/10/2014
ms.assetid: a861409b-4ae6-4868-9e09-87edfac3535f
msc.legacyurl: /web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
msc.type: authoredcontent
ms.openlocfilehash: e587398b430795c12a1dcee394852b4e2b8a0e44
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021164"
---
<a name="running-different-versions-of-aspnet-web-pages-razor-side-by-side"></a><span data-ttu-id="15abf-103">Executando versões diferentes do ASP.NET Web Pages (Razor) lado a lado</span><span class="sxs-lookup"><span data-stu-id="15abf-103">Running Different Versions of ASP.NET Web Pages (Razor) Side by Side</span></span>
====================
<span data-ttu-id="15abf-104">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="15abf-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="15abf-105">Este artigo explica como executar o ASP.NET Web Pages (Razor) sites no mesmo computador ou servidor quando os sites são configurados para usar versões diferentes de páginas da Web do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="15abf-105">This article explains how to run ASP.NET Web Pages (Razor) websites on the same computer or server when the websites are configured to use different versions of ASP.NET Web Pages.</span></span>
> 
> <span data-ttu-id="15abf-106">O que você aprenderá:</span><span class="sxs-lookup"><span data-stu-id="15abf-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="15abf-107">O que o comportamento padrão é no ASP.NET quando você tiver sites criados com páginas da Web do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="15abf-107">What the default behavior is in ASP.NET when you have sites built with ASP.NET Web Pages.</span></span>
> - <span data-ttu-id="15abf-108">Como configurar um novo site para ser executado com uma versão mais antiga de páginas da Web do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="15abf-108">How to configure a new site to run with an older version of ASP.NET Web Pages.</span></span>
>   
> 
> <span data-ttu-id="15abf-109">Esse é o recurso ASP.NET introduzido no artigo:</span><span class="sxs-lookup"><span data-stu-id="15abf-109">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="15abf-110">O `webPages:Version` definição de configuração.</span><span class="sxs-lookup"><span data-stu-id="15abf-110">The `webPages:Version` configuration setting.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="15abf-111">Versões de software</span><span class="sxs-lookup"><span data-stu-id="15abf-111">Software versions</span></span>
> 
> 
> - <span data-ttu-id="15abf-112">Páginas da Web do ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="15abf-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="15abf-113">Este tutorial também funciona com ASP.NET Web Pages 2 e páginas da Web de ASP.NET 1.0.</span><span class="sxs-lookup"><span data-stu-id="15abf-113">This tutorial also works with ASP.NET Web Pages 2 and ASP.NET Web Pages 1.0.</span></span>


<span data-ttu-id="15abf-114">Páginas da Web do ASP.NET oferece suporte à capacidade de executar sites lado a lado.</span><span class="sxs-lookup"><span data-stu-id="15abf-114">ASP.NET Web Pages supports the ability to run websites side by side.</span></span> <span data-ttu-id="15abf-115">Isso permite que você continue a executar seus aplicativos mais antigos de páginas da Web ASP.NET, criar novos aplicativos de páginas da Web ASP.NET e executar todas elas no mesmo computador.</span><span class="sxs-lookup"><span data-stu-id="15abf-115">This lets you continue to run your older ASP.NET Web Pages applications, build new ASP.NET Web Pages applications, and run all of them on the same computer.</span></span>

<span data-ttu-id="15abf-116">Aqui estão algumas coisas a lembrar quando você instala as páginas da Web com o WebMatrix:</span><span class="sxs-lookup"><span data-stu-id="15abf-116">Here are some things to remember when you install the Web Pages with WebMatrix:</span></span>

- <span data-ttu-id="15abf-117">Por padrão, os aplicativos existentes de páginas da Web serão executado como a versão mais recente em seu computador.</span><span class="sxs-lookup"><span data-stu-id="15abf-117">By default, existing Web Pages applications will run as the latest version on your computer.</span></span> <span data-ttu-id="15abf-118">(Os assemblies são instalados no cache de assembly global (GAC) e são usados automaticamente.)</span><span class="sxs-lookup"><span data-stu-id="15abf-118">(The assemblies are installed in the global assembly cache (GAC) and are used automatically.)</span></span>
- <span data-ttu-id="15abf-119">Se você quiser executar um site usando uma versão diferente de páginas da Web do ASP.NET, você pode configurar o site para fazer isso.</span><span class="sxs-lookup"><span data-stu-id="15abf-119">If you want to run a site using a different version of ASP.NET Web Pages, you can configure the site to do that.</span></span> <span data-ttu-id="15abf-120">Se seu site ainda não tiver um *Web. config* de arquivo na raiz do site, crie um novo e copie o XML a seguir para ele, substituindo o conteúdo existente.</span><span class="sxs-lookup"><span data-stu-id="15abf-120">If your site doesn't already have a *web.config* file in the root of the site, create a new one and copy the following XML into it, overwriting the existing content.</span></span> <span data-ttu-id="15abf-121">Se o site já contém um *Web. config* do arquivo, adicione uma `<appSettings>` elemento como a seguir para o `<configuration>` seção.</span><span class="sxs-lookup"><span data-stu-id="15abf-121">If the site already contains a *web.config* file, add an `<appSettings>` element like the following one to the `<configuration>` section.</span></span>

    [!code-xml[Main](running-v1-and-v2-sites-side-by-side/samples/sample1.xml)]
  <span data-ttu-id="15abf-122">'-Se você não especificar uma versão na *Web. config* arquivo, um site é implantado como a versão mais recente.</span><span class="sxs-lookup"><span data-stu-id="15abf-122">\`- If you do not specify a version in the *web.config* file, a site is deployed as the latest version.</span></span> <span data-ttu-id="15abf-123">(Os assemblies são copiados para o *bin* pasta no site implantado.)</span><span class="sxs-lookup"><span data-stu-id="15abf-123">(The assemblies are copied to the *bin* folder in the deployed site.)</span></span>
- <span data-ttu-id="15abf-124">Novos aplicativos que você cria usando os modelos de site na Web Matrix incluem os assemblies da versão de páginas da Web do site *bin* pasta.</span><span class="sxs-lookup"><span data-stu-id="15abf-124">New applications that you create using the site templates in Web Matrix include the Web Pages version assemblies in the site's *bin* folder.</span></span>

<span data-ttu-id="15abf-125">Em geral, você sempre pode controlar qual versão das páginas da Web para usar com seu site usando o NuGet para instalar os assemblies apropriados para o site *bin* pasta.</span><span class="sxs-lookup"><span data-stu-id="15abf-125">In general, you can always control which version of Web Pages to use with your site by using NuGet to install the appropriate assemblies into the site's *bin* folder.</span></span> <span data-ttu-id="15abf-126">Para encontrar pacotes, visite [NuGet.org](http://NuGet.org).</span><span class="sxs-lookup"><span data-stu-id="15abf-126">To find packages, visit [NuGet.org](http://NuGet.org).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="15abf-127">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="15abf-127">Additional Resources</span></span>

[<span data-ttu-id="15abf-128">Os principais recursos do ASP.NET Web Pages 2</span><span class="sxs-lookup"><span data-stu-id="15abf-128">The Top Features in ASP.NET Web Pages 2</span></span>](top-features-in-web-pages-2.md)
