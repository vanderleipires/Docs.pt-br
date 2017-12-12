---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
title: "Introdução ao Kit de ferramentas de controle AJAX (c#) | Microsoft Docs"
author: microsoft
description: "Saiba tudo o que precisa saber para começar a usar o Kit de ferramentas de controle AJAX."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 16dc5c11-65be-4eae-a818-9fad7f8259c6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
msc.type: authoredcontent
ms.openlocfilehash: 8d3f4dd26a9f82dce78b1c3665f9da6b54bdacba
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="get-started-with-the-ajax-control-toolkit-c"></a><span data-ttu-id="fb9ab-103">Introdução ao Kit de ferramentas de controle AJAX (c#)</span><span class="sxs-lookup"><span data-stu-id="fb9ab-103">Get Started with the AJAX Control Toolkit (C#)</span></span>
====================
<span data-ttu-id="fb9ab-104">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="fb9ab-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="fb9ab-105">Saiba tudo o que precisa saber para começar a usar o Kit de ferramentas de controle AJAX.</span><span class="sxs-lookup"><span data-stu-id="fb9ab-105">Learn all you need to know to get started using the AJAX Control Toolkit.</span></span>


<span data-ttu-id="fb9ab-106">O Kit de ferramentas de controle AJAX contém mais de 30 controles livres que você pode usar em seus aplicativos ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="fb9ab-106">The AJAX Control Toolkit contains more than 30 free controls that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="fb9ab-107">Neste tutorial, você aprenderá como baixar o Kit de ferramentas de controle AJAX e adicionar os controles do Kit de ferramentas para sua caixa de ferramentas do Visual Studio/Visual Web Developer Express.</span><span class="sxs-lookup"><span data-stu-id="fb9ab-107">In this tutorial, you learn how to download the AJAX Control Toolkit and add the toolkit controls to your Visual Studio/Visual Web Developer Express toolbox.</span></span>

## <a name="downloading-the-ajax-control-toolkit"></a><span data-ttu-id="fb9ab-108">Baixar o Kit de ferramentas de controle AJAX</span><span class="sxs-lookup"><span data-stu-id="fb9ab-108">Downloading the AJAX Control Toolkit</span></span>

<span data-ttu-id="fb9ab-109">O [AJAX Control Toolkit](http://devexpress.com/act) é um projeto de software livre desenvolvido por membros da comunidade do ASP.NET e a equipe do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="fb9ab-109">The [AJAX Control Toolkit](http://devexpress.com/act) is an open source project developed by the members of the ASP.NET community and the ASP.NET team.</span></span> 


<span data-ttu-id="fb9ab-110">[![Baixar o Kit de ferramentas de controle AJAX](get-started-with-the-ajax-control-toolkit-cs/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="fb9ab-110">[![Downloading the AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image1.png)</span></span>

<span data-ttu-id="fb9ab-111">**Figura 01**: baixar o Kit de ferramentas de controle AJAX ([clique para exibir a imagem em tamanho normal](get-started-with-the-ajax-control-toolkit-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="fb9ab-111">**Figure 01**: Downloading the AJAX Control Toolkit([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image2.png))</span></span>


<span data-ttu-id="fb9ab-112">Depois de baixar o arquivo, você precisa desbloquear o arquivo.</span><span class="sxs-lookup"><span data-stu-id="fb9ab-112">After you download the file, you need to unblock the file.</span></span> <span data-ttu-id="fb9ab-113">Clique no arquivo, selecione Propriedades e clique no **desbloquear** botão (consulte a Figura 2).</span><span class="sxs-lookup"><span data-stu-id="fb9ab-113">Right-click the file, select Properties, and click the **Unblock** button (see Figure 2).</span></span>


<span data-ttu-id="fb9ab-114">[![Desbloquear o arquivo ZIP de kit de ferramentas de controle AJAX](get-started-with-the-ajax-control-toolkit-cs/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="fb9ab-114">[![Unblocking the AJAX Control Toolkit ZIP file](get-started-with-the-ajax-control-toolkit-cs/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image3.png)</span></span>

<span data-ttu-id="fb9ab-115">**Figura 02**: desbloquear o arquivo ZIP de kit de ferramentas de controle AJAX ([clique para exibir a imagem em tamanho normal](get-started-with-the-ajax-control-toolkit-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="fb9ab-115">**Figure 02**: Unblocking the AJAX Control Toolkit ZIP file([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image4.png))</span></span>


<span data-ttu-id="fb9ab-116">Depois de desbloquear o arquivo, você pode descompactar o arquivo: o arquivo e selecione o **extrair tudo** opção de menu.</span><span class="sxs-lookup"><span data-stu-id="fb9ab-116">After you unblock the file, you can unzip the file: Right-click the file and select the **Extract All** menu option.</span></span> <span data-ttu-id="fb9ab-117">Agora, você está pronto para adicionar o Kit de ferramentas para a caixa de ferramentas do Visual Studio/Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="fb9ab-117">Now, we are ready to add the toolkit to the Visual Studio/Visual Web Developer toolbox.</span></span>

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a><span data-ttu-id="fb9ab-118">Adicionando o Kit de ferramentas de controle AJAX para a caixa de ferramentas</span><span class="sxs-lookup"><span data-stu-id="fb9ab-118">Adding the AJAX Control Toolkit to the Toolbox</span></span>

<span data-ttu-id="fb9ab-119">É a maneira mais fácil de usar o Kit de ferramentas de controle AJAX adicionar o Kit de ferramentas para sua caixa de ferramentas do Visual Studio/Visual Web Developer (consulte a Figura 3).</span><span class="sxs-lookup"><span data-stu-id="fb9ab-119">The easiest way to use the AJAX Control Toolkit is to add the toolkit to your Visual Studio/Visual Web Developer toolbox (see Figure 3).</span></span> <span data-ttu-id="fb9ab-120">Dessa forma, você pode simplesmente arraste um kit de ferramentas controle para uma página quando você deseja usá-lo.</span><span class="sxs-lookup"><span data-stu-id="fb9ab-120">That way, you can simply drag a toolkit control onto a page when you want to use it.</span></span>


<span data-ttu-id="fb9ab-121">[![Kit de ferramentas de controle AJAX aparece na caixa de ferramentas](get-started-with-the-ajax-control-toolkit-cs/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="fb9ab-121">[![AJAX Control Toolkit appears in toolbox](get-started-with-the-ajax-control-toolkit-cs/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image5.png)</span></span>

<span data-ttu-id="fb9ab-122">**Figura 03**: AJAX Control Toolkit é exibido na caixa de ferramentas ([clique para exibir a imagem em tamanho normal](get-started-with-the-ajax-control-toolkit-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="fb9ab-122">**Figure 03**: AJAX Control Toolkit appears in toolbox([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image6.png))</span></span>


<span data-ttu-id="fb9ab-123">Primeiro, você precisa adicionar uma guia do Kit de ferramentas de controle AJAX para a caixa de ferramentas.</span><span class="sxs-lookup"><span data-stu-id="fb9ab-123">First, you need to add an AJAX Control Toolkit tab to the toolbox.</span></span> <span data-ttu-id="fb9ab-124">Siga estas etapas.</span><span class="sxs-lookup"><span data-stu-id="fb9ab-124">Follow these steps.</span></span>

1. <span data-ttu-id="fb9ab-125">Crie um novo site da Web ASP.NET, selecionando a opção de menu Arquivo, novo site.</span><span class="sxs-lookup"><span data-stu-id="fb9ab-125">Create a new ASP.NET Website by selecting the menu option File, New Website.</span></span> <span data-ttu-id="fb9ab-126">Clique duas vezes em Default.aspx na janela do Gerenciador de soluções para abrir o arquivo no editor.</span><span class="sxs-lookup"><span data-stu-id="fb9ab-126">Double-click the Default.aspx in the Solution Explorer window to open the file in the editor.</span></span>
2. <span data-ttu-id="fb9ab-127">A caixa de ferramentas sob a guia Geral e selecione a opção de menu **Adicionar guia** (consulte a Figura 4).</span><span class="sxs-lookup"><span data-stu-id="fb9ab-127">Right-click the Toolbox beneath the General Tab and select the menu option **Add Tab** (see Figure 4).</span></span>
3. <span data-ttu-id="fb9ab-128">Insira uma nova guia chamada AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="fb9ab-128">Enter a new tab named AJAX Control Toolkit.</span></span>


<span data-ttu-id="fb9ab-129">[![Adicionar uma nova guia](get-started-with-the-ajax-control-toolkit-cs/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="fb9ab-129">[![Adding a new tab](get-started-with-the-ajax-control-toolkit-cs/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image7.png)</span></span>

<span data-ttu-id="fb9ab-130">**Figura 04**: adicionando uma nova guia ([clique para exibir a imagem em tamanho normal](get-started-with-the-ajax-control-toolkit-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="fb9ab-130">**Figure 04**: Adding a new tab([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image8.png))</span></span>


<span data-ttu-id="fb9ab-131">Em seguida, você precisa adicionar os controles do Kit de ferramentas de controle AJAX para a nova guia. Siga estas etapas:</span><span class="sxs-lookup"><span data-stu-id="fb9ab-131">Next, you need to add the AJAX Control Toolkit controls to the new tab. Follow these steps:</span></span>

- <span data-ttu-id="fb9ab-132">Sob a guia AJAX Control Toolkit e selecione a opção de menu **escolher itens (consulte a Figura 5)**.</span><span class="sxs-lookup"><span data-stu-id="fb9ab-132">Right-click beneath the AJAX Control Toolkit tab and select the menu option **Choose Items (see Figure 5)**.</span></span>
- <span data-ttu-id="fb9ab-133">Navegue até o local onde você descompactou o Kit de ferramentas de controle AJAX e selecione o assembly AjaxControlToolkit.</span><span class="sxs-lookup"><span data-stu-id="fb9ab-133">Browse to the location where you unzipped the AJAX Control Toolkit and select the AjaxControlToolkit.dll assembly.</span></span>


<span data-ttu-id="fb9ab-134">[![Escolha os itens a serem adicionados à caixa de ferramentas](get-started-with-the-ajax-control-toolkit-cs/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="fb9ab-134">[![Choose items to add to the toolbox](get-started-with-the-ajax-control-toolkit-cs/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image9.png)</span></span>

<span data-ttu-id="fb9ab-135">**Figura 05**: escolha os itens a serem adicionados à caixa de ferramentas ([clique para exibir a imagem em tamanho normal](get-started-with-the-ajax-control-toolkit-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="fb9ab-135">**Figure 05**: Choose items to add to the toolbox([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image10.png))</span></span>


<span data-ttu-id="fb9ab-136">Depois de concluir essas etapas, todos os controles do Kit de ferramentas serão exibida na caixa de ferramentas.</span><span class="sxs-lookup"><span data-stu-id="fb9ab-136">After you complete these steps, all of the toolkit controls will appear in your toolbox.</span></span>

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a><span data-ttu-id="fb9ab-137">Atualizar para uma nova versão do Kit de ferramentas</span><span class="sxs-lookup"><span data-stu-id="fb9ab-137">Upgrading to a New Version of the Toolkit</span></span>

<span data-ttu-id="fb9ab-138">Se você estivesse usando uma versão mais antiga do Kit de ferramentas e agora precisa mover para uma versão posterior aqui estão as etapas recomendadas:</span><span class="sxs-lookup"><span data-stu-id="fb9ab-138">If you were using an older release of the Toolkit and now need to move to a later version here are the recommended steps:</span></span>

- <span data-ttu-id="fb9ab-139">Binários - excluir a versão antiga do assembly AjaxControlToolkit da pasta Bin do site.</span><span class="sxs-lookup"><span data-stu-id="fb9ab-139">Binaries - Delete the old version of the AjaxControlToolkit.dll assembly from your website Bin folder.</span></span>
- <span data-ttu-id="fb9ab-140">Itens de caixa de ferramentas - Excluir guia AJAX Control Toolkit e siga as etapas acima para recriar a guia com a nova versão do assembly AjaxControlToolkit.</span><span class="sxs-lookup"><span data-stu-id="fb9ab-140">Toolbox Items - Delete the AJAX Control Toolkit tab and follow the steps above to re-create the tab with the new version of the AjaxControlToolkit.dll assembly.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="fb9ab-141">Avançar</span><span class="sxs-lookup"><span data-stu-id="fb9ab-141">Next</span></span>](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
