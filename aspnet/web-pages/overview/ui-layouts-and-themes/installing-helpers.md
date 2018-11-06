---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: Instalação de um auxiliar em uma Web do ASP.NET (Razor) sites de páginas | Microsoft Docs
author: Rick-Anderson
description: Este artigo descreve como instalar um auxiliar em um site de páginas da Web do ASP.NET (Razor). Um auxiliar é um componente reutilizável que inclui código e marcação para por...
ms.author: riande
ms.date: 02/18/2014
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: 5ad717cd7c64e830ce66d5e1361d0eb6ef3cbbec
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021359"
---
<a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="e6570-104">Instalação de um auxiliar em um Site do ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="e6570-104">Installing a Helper in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="e6570-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="e6570-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="e6570-106">Este artigo descreve como instalar um auxiliar em um site de páginas da Web do ASP.NET (Razor).</span><span class="sxs-lookup"><span data-stu-id="e6570-106">This article describes how to install a helper in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="e6570-107">Um *auxiliar* é um componente reutilizável que inclui código e marcação para executar uma tarefa que pode ser entediante ou complexos.</span><span class="sxs-lookup"><span data-stu-id="e6570-107">A *helper* is a reusable component that includes code and markup to perform a task that might be tedious or complex.</span></span>
> 
> <span data-ttu-id="e6570-108">O que você aprenderá:</span><span class="sxs-lookup"><span data-stu-id="e6570-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="e6570-109">Como instalar um auxiliar em um site criado usando o WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="e6570-109">How to install a helper in a website created using WebMatrix 3.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="e6570-110">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="e6570-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="e6570-111">O WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="e6570-111">WebMatrix 3</span></span>


## <a name="overview-of-helpers"></a><span data-ttu-id="e6570-112">Visão geral dos auxiliares</span><span class="sxs-lookup"><span data-stu-id="e6570-112">Overview of Helpers</span></span>

<span data-ttu-id="e6570-113">Algumas tarefas que as pessoas frequentemente desejam fazer em páginas da web exigem um monte de código ou exigem conhecimento extra.</span><span class="sxs-lookup"><span data-stu-id="e6570-113">Some tasks that people often want to do on web pages require a lot of code or require extra knowledge.</span></span> <span data-ttu-id="e6570-114">Exemplos incluem a exibição de um gráfico para dados; colocar um botão "Seguir" do Twitter em uma página. envio de email do seu site; recortando ou redimensionamento de imagens; usando PayPal para o seu site.</span><span class="sxs-lookup"><span data-stu-id="e6570-114">Examples include displaying a chart for data; putting a Twitter "Follow" button on a page; sending email from your website; cropping or resizing images; using PayPal for your site.</span></span> <span data-ttu-id="e6570-115">Para que seja fácil fazer esses tipos de coisas, o ASP.NET Web Pages permite que você use *auxiliares*.</span><span class="sxs-lookup"><span data-stu-id="e6570-115">To make it easy to do these kinds of things, ASP.NET Web Pages lets you use *helpers*.</span></span> <span data-ttu-id="e6570-116">Os auxiliares são componentes que você instale para um site e que permitem que você realize tarefas típicas usando apenas uma ou duas linhas de código do Razor.</span><span class="sxs-lookup"><span data-stu-id="e6570-116">Helpers are components that you install for a site and that let you perform typical tasks by using just a line or two of Razor code.</span></span>

<span data-ttu-id="e6570-117">Páginas da Web do ASP.NET tem alguns auxiliares internos.</span><span class="sxs-lookup"><span data-stu-id="e6570-117">ASP.NET Web Pages has a few helpers built in.</span></span> <span data-ttu-id="e6570-118">No entanto, muitos auxiliares estão disponíveis em pacotes (Suplementos) que são fornecidos usando o Gerenciador de pacotes do NuGet.</span><span class="sxs-lookup"><span data-stu-id="e6570-118">However, many helpers are available in packages (add-ins) that are provided using the NuGet package manager.</span></span> <span data-ttu-id="e6570-119">O NuGet permite que você selecione um pacote para instalar e, em seguida, ele cuida de todos os detalhes da instalação.</span><span class="sxs-lookup"><span data-stu-id="e6570-119">NuGet lets you select a package to install and then it takes care of all the details of the installation.</span></span>

## <a name="installing-a-helper-in-webmatrix-3"></a><span data-ttu-id="e6570-120">Instalação de um auxiliar no WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="e6570-120">Installing a Helper in WebMatrix 3</span></span>

1. <span data-ttu-id="e6570-121">O WebMatrix 3, clique no **NuGet** botão.</span><span class="sxs-lookup"><span data-stu-id="e6570-121">In WebMatrix 3, click the **NuGet** button.</span></span>

    ![Caixa de diálogo de galeria do NuGet no WebMatrix](installing-helpers/_static/image1.png)
2. <span data-ttu-id="e6570-123">Isso inicia o Gerenciador de pacotes do NuGet e exibe os pacotes disponíveis.</span><span class="sxs-lookup"><span data-stu-id="e6570-123">This launches the NuGet package manager and displays available packages.</span></span> <span data-ttu-id="e6570-124">Na caixa de pesquisa, insira uma palavra-chave para o auxiliar que você deseja instalar.</span><span class="sxs-lookup"><span data-stu-id="e6570-124">In the search box, enter a keyword for the helper you want to install.</span></span>

    ![Caixa de diálogo de galeria do NuGet no WebMatrix](installing-helpers/_static/image2.png)
3. <span data-ttu-id="e6570-126">Selecione o pacote e, em seguida, clique em **instalar**.</span><span class="sxs-lookup"><span data-stu-id="e6570-126">Select the package and then click **Install**.</span></span> <span data-ttu-id="e6570-127">Clique em **Sim** quando for perguntado se você deseja instalar o pacote e indicar que você aceite os termos.</span><span class="sxs-lookup"><span data-stu-id="e6570-127">Click **Yes** when asked if you want to install the package and indicate that you accept the terms.</span></span>

     <span data-ttu-id="e6570-128">Se essa for a primeira vez em que você instalou um auxiliar, o NuGet cria pastas no seu site para o código que compõe o auxiliar.</span><span class="sxs-lookup"><span data-stu-id="e6570-128">If this is the first time you've installed a helper, NuGet creates folders in your website for the code that makes up the helper.</span></span>
4. <span data-ttu-id="e6570-129">Para desinstalar um auxiliar, clique o **galeria** , clique no **instalado** guia e, em seguida, selecione o pacote que você deseja desinstalar.</span><span class="sxs-lookup"><span data-stu-id="e6570-129">To uninstall a helper, click the **Gallery** button, click the **Installed** tab, and pick the package you want to uninstall.</span></span>

## <a name="installing-the-twitter-helper"></a><span data-ttu-id="e6570-130">Instalando o auxiliar do Twitter</span><span class="sxs-lookup"><span data-stu-id="e6570-130">Installing the Twitter helper</span></span>

<span data-ttu-id="e6570-131">A versão mais recente da API do Twitter não é compatível com o auxiliar do Twitter que instalar por meio do NuGet.</span><span class="sxs-lookup"><span data-stu-id="e6570-131">The latest version of the Twitter API is not compatible with the Twitter helper you install through NuGet.</span></span> <span data-ttu-id="e6570-132">Em vez disso, consulte a [auxiliar do Twitter com o WebMatrix](twitter-helper.md) tópico para obter informações sobre como configurar o auxiliar do Twitter em seu projeto.</span><span class="sxs-lookup"><span data-stu-id="e6570-132">Instead, see the [Twitter Helper with WebMatrix](twitter-helper.md) topic for information about how to set up the Twitter helper in your project.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="e6570-133">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="e6570-133">Additional Resources</span></span>


[<span data-ttu-id="e6570-134">Introdução ao ASP.NET Web Pages 2 - Noções básicas de programação</span><span class="sxs-lookup"><span data-stu-id="e6570-134">Introducing ASP.NET Web Pages 2 - Programming Basics</span></span>](../getting-started/introducing-razor-syntax-c.md)

[<span data-ttu-id="e6570-135">Auxiliar do Twitter com o WebMatrix</span><span class="sxs-lookup"><span data-stu-id="e6570-135">Twitter Helper with WebMatrix</span></span>](twitter-helper.md)
