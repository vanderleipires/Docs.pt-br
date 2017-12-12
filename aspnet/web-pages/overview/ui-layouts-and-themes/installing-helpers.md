---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: "Instalar um auxiliar em uma Web ASP.NET páginas Site (Razor) | Microsoft Docs"
author: tfitzmac
description: "Este artigo descreve como instalar um auxiliar em um site de páginas da Web do ASP.NET (Razor). Um auxiliar é um componente reutilizável que inclui o código e marcação para por..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2014
ms.topic: article
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: 842c5a56d14314217c1e6ad6d48ded28d3cc5b4e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="14802-104">Instalando um auxiliar em um Site de páginas (Razor) da Web do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="14802-104">Installing a Helper in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="14802-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="14802-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="14802-106">Este artigo descreve como instalar um auxiliar em um site de páginas da Web do ASP.NET (Razor).</span><span class="sxs-lookup"><span data-stu-id="14802-106">This article describes how to install a helper in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="14802-107">Um *auxiliar* é um componente reutilizável que inclui o código e marcação para executar uma tarefa que pode ser entediante ou complexos.</span><span class="sxs-lookup"><span data-stu-id="14802-107">A *helper* is a reusable component that includes code and markup to perform a task that might be tedious or complex.</span></span>
> 
> <span data-ttu-id="14802-108">O que você aprenderá:</span><span class="sxs-lookup"><span data-stu-id="14802-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="14802-109">Como instalar um auxiliar em um site criado usando o WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="14802-109">How to install a helper in a website created using WebMatrix 3.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="14802-110">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="14802-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="14802-111">O WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="14802-111">WebMatrix 3</span></span>


## <a name="overview-of-helpers"></a><span data-ttu-id="14802-112">Visão geral de auxiliares</span><span class="sxs-lookup"><span data-stu-id="14802-112">Overview of Helpers</span></span>

<span data-ttu-id="14802-113">Algumas tarefas que as pessoas desejam em páginas da web geralmente exigem muito código ou exigem conhecimento extra.</span><span class="sxs-lookup"><span data-stu-id="14802-113">Some tasks that people often want to do on web pages require a lot of code or require extra knowledge.</span></span> <span data-ttu-id="14802-114">Exemplos incluem a exibição de um gráfico de dados. colocar um botão "Seguir" do Twitter em uma página. Enviar email de seu site. recortando ou redimensionamento de imagens; usando PayPal para o seu site.</span><span class="sxs-lookup"><span data-stu-id="14802-114">Examples include displaying a chart for data; putting a Twitter "Follow" button on a page; sending email from your website; cropping or resizing images; using PayPal for your site.</span></span> <span data-ttu-id="14802-115">Para facilitar a esses tipos de coisas, o ASP.NET Web Pages permite que você use *auxiliares*.</span><span class="sxs-lookup"><span data-stu-id="14802-115">To make it easy to do these kinds of things, ASP.NET Web Pages lets you use *helpers*.</span></span> <span data-ttu-id="14802-116">Auxiliares são componentes que você instale um site e que permitem a você executam tarefas comuns usando apenas uma linha ou duas de código Razor.</span><span class="sxs-lookup"><span data-stu-id="14802-116">Helpers are components that you install for a site and that let you perform typical tasks by using just a line or two of Razor code.</span></span>

<span data-ttu-id="14802-117">Páginas da Web do ASP.NET tem alguns auxiliares internos.</span><span class="sxs-lookup"><span data-stu-id="14802-117">ASP.NET Web Pages has a few helpers built in.</span></span> <span data-ttu-id="14802-118">No entanto, muitos auxiliares estão disponíveis em pacotes (Suplementos) que são fornecidos usando o NuGet package manager.</span><span class="sxs-lookup"><span data-stu-id="14802-118">However, many helpers are available in packages (add-ins) that are provided using the NuGet package manager.</span></span> <span data-ttu-id="14802-119">O NuGet permite que você selecione um pacote para instalar e, em seguida, ele cuida de todos os detalhes da instalação.</span><span class="sxs-lookup"><span data-stu-id="14802-119">NuGet lets you select a package to install and then it takes care of all the details of the installation.</span></span>

## <a name="installing-a-helper-in-webmatrix-3"></a><span data-ttu-id="14802-120">Instalando um auxiliar no WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="14802-120">Installing a Helper in WebMatrix 3</span></span>

1. <span data-ttu-id="14802-121">No WebMatrix 3, clique no **NuGet** botão.</span><span class="sxs-lookup"><span data-stu-id="14802-121">In WebMatrix 3, click the **NuGet** button.</span></span>

    ![Caixa de diálogo do NuGet galeria no WebMatrix](installing-helpers/_static/image1.png)
2. <span data-ttu-id="14802-123">Isso inicia o NuGet package manager e exibe os pacotes disponíveis.</span><span class="sxs-lookup"><span data-stu-id="14802-123">This launches the NuGet package manager and displays available packages.</span></span> <span data-ttu-id="14802-124">Na caixa de pesquisa, digite uma palavra-chave para o auxiliar que você deseja instalar.</span><span class="sxs-lookup"><span data-stu-id="14802-124">In the search box, enter a keyword for the helper you want to install.</span></span>

    ![Caixa de diálogo do NuGet galeria no WebMatrix](installing-helpers/_static/image2.png)
- <span data-ttu-id="14802-126">Selecione o pacote e, em seguida, clique em **instalar**.</span><span class="sxs-lookup"><span data-stu-id="14802-126">Select the package and then click **Install**.</span></span> <span data-ttu-id="14802-127">Clique em **Sim** quando for perguntado se deseja instalar o pacote e indicar que você aceita os termos.</span><span class="sxs-lookup"><span data-stu-id="14802-127">Click **Yes** when asked if you want to install the package and indicate that you accept the terms.</span></span>

    <span data-ttu-id="14802-128">Se esta for a primeira vez em que você instalou um auxiliar, o NuGet cria pastas no seu site para o código que compõe o auxiliar.</span><span class="sxs-lookup"><span data-stu-id="14802-128">If this is the first time you've installed a helper, NuGet creates folders in your website for the code that makes up the helper.</span></span>
- <span data-ttu-id="14802-129">Para desinstalar um auxiliar, clique o **galeria** , clique no **instalado** guia e selecione o pacote que deseja desinstalar.</span><span class="sxs-lookup"><span data-stu-id="14802-129">To uninstall a helper, click the **Gallery** button, click the **Installed** tab, and pick the package you want to uninstall.</span></span>

## <a name="installing-the-twitter-helper"></a><span data-ttu-id="14802-130">Instalando o auxiliar do Twitter</span><span class="sxs-lookup"><span data-stu-id="14802-130">Installing the Twitter helper</span></span>

<span data-ttu-id="14802-131">A versão mais recente da API do Twitter não é compatível com o auxiliar do Twitter que instalar por meio do NuGet.</span><span class="sxs-lookup"><span data-stu-id="14802-131">The latest version of the Twitter API is not compatible with the Twitter helper you install through NuGet.</span></span> <span data-ttu-id="14802-132">Em vez disso, consulte o [auxiliar do Twitter com o WebMatrix](twitter-helper.md) tópico para obter informações sobre como configurar o auxiliar do Twitter em seu projeto.</span><span class="sxs-lookup"><span data-stu-id="14802-132">Instead, see the [Twitter Helper with WebMatrix](twitter-helper.md) topic for information about how to set up the Twitter helper in your project.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="14802-133">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="14802-133">Additional Resources</span></span>


[<span data-ttu-id="14802-134">Introdução ao ASP.NET Web Pages 2 - Noções básicas de programação</span><span class="sxs-lookup"><span data-stu-id="14802-134">Introducing ASP.NET Web Pages 2 - Programming Basics</span></span>](../getting-started/introducing-razor-syntax-c.md)

[<span data-ttu-id="14802-135">Auxiliar do Twitter com o WebMatrix</span><span class="sxs-lookup"><span data-stu-id="14802-135">Twitter Helper with WebMatrix</span></span>](twitter-helper.md)
