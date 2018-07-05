---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-vb
title: Introdução ao AJAX Control Toolkit (VB) | Microsoft Docs
author: microsoft
description: Aprenda tudo o que você precisa saber para começar a usar o AJAX Control Toolkit.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 9f8fa166-49a2-402c-b236-20caef0c658f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-vb
msc.type: authoredcontent
ms.openlocfilehash: 6041087df3a15ef42d2364881f08a991f4eeb4fc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37367768"
---
<a name="get-started-with-the-ajax-control-toolkit-vb"></a><span data-ttu-id="b68a4-103">Introdução ao AJAX Control Toolkit (VB)</span><span class="sxs-lookup"><span data-stu-id="b68a4-103">Get Started with the AJAX Control Toolkit (VB)</span></span>
====================
<span data-ttu-id="b68a4-104">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="b68a4-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="b68a4-105">Aprenda tudo o que você precisa saber para começar a usar o AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="b68a4-105">Learn all you need to know to get started using the AJAX Control Toolkit.</span></span>


<span data-ttu-id="b68a4-106">O AJAX Control Toolkit contém mais de 30 controles gratuitos que você pode usar em seus aplicativos ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b68a4-106">The AJAX Control Toolkit contains more than 30 free controls that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="b68a4-107">Neste tutorial, você aprenderá como baixar o AJAX Control Toolkit e adicione os controles do Kit de ferramentas para sua caixa de ferramentas do Visual Studio/Visual Web Developer Express.</span><span class="sxs-lookup"><span data-stu-id="b68a4-107">In this tutorial, you learn how to download the AJAX Control Toolkit and add the toolkit controls to your Visual Studio/Visual Web Developer Express toolbox.</span></span>

## <a name="downloading-the-ajax-control-toolkit"></a><span data-ttu-id="b68a4-108">Baixando o AJAX Control Toolkit</span><span class="sxs-lookup"><span data-stu-id="b68a4-108">Downloading the AJAX Control Toolkit</span></span>

<span data-ttu-id="b68a4-109">O [AJAX Control Toolkit](http://devexpress.com/act) é um projeto de código-fonte aberto desenvolvido por membros da comunidade do ASP.NET e a equipe do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b68a4-109">The [AJAX Control Toolkit](http://devexpress.com/act) is an open source project developed by the members of the ASP.NET community and the ASP.NET team.</span></span>


<span data-ttu-id="b68a4-110">[![Baixando o AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-vb/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b68a4-110">[![Downloading the AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-vb/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image1.png)</span></span>

<span data-ttu-id="b68a4-111">**Figura 01**: baixar o AJAX Control Toolkit ([clique para exibir a imagem em tamanho normal](get-started-with-the-ajax-control-toolkit-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="b68a4-111">**Figure 01**: Downloading the AJAX Control Toolkit([Click to view full-size image](get-started-with-the-ajax-control-toolkit-vb/_static/image2.png))</span></span>


<span data-ttu-id="b68a4-112">Depois de baixar o arquivo, você precisará desbloquear o arquivo.</span><span class="sxs-lookup"><span data-stu-id="b68a4-112">After you download the file, you need to unblock the file.</span></span> <span data-ttu-id="b68a4-113">O arquivo com o botão direito, selecione Propriedades e clique o **Unblock** botão (consulte a Figura 2).</span><span class="sxs-lookup"><span data-stu-id="b68a4-113">Right-click the file, select Properties, and click the **Unblock** button (see Figure 2).</span></span>


<span data-ttu-id="b68a4-114">[![Desbloquear o arquivo ZIP de kit de ferramentas de controle do AJAX](get-started-with-the-ajax-control-toolkit-vb/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="b68a4-114">[![Unblocking the AJAX Control Toolkit ZIP file](get-started-with-the-ajax-control-toolkit-vb/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image3.png)</span></span>

<span data-ttu-id="b68a4-115">**Figura 02**: desbloquear o arquivo ZIP de kit de ferramentas de controle do AJAX ([clique para exibir a imagem em tamanho normal](get-started-with-the-ajax-control-toolkit-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="b68a4-115">**Figure 02**: Unblocking the AJAX Control Toolkit ZIP file([Click to view full-size image](get-started-with-the-ajax-control-toolkit-vb/_static/image4.png))</span></span>


<span data-ttu-id="b68a4-116">Depois de desbloquear o arquivo, você pode descompactar o arquivo: o arquivo com o botão direito e selecione o **extrair tudo** opção de menu.</span><span class="sxs-lookup"><span data-stu-id="b68a4-116">After you unblock the file, you can unzip the file: Right-click the file and select the **Extract All** menu option.</span></span> <span data-ttu-id="b68a4-117">Agora, estamos prontos para adicionar o Kit de ferramentas para a caixa de ferramentas do Visual Studio/Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="b68a4-117">Now, we are ready to add the toolkit to the Visual Studio/Visual Web Developer toolbox.</span></span>

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a><span data-ttu-id="b68a4-118">Adicionando o AJAX Control Toolkit à caixa de ferramentas</span><span class="sxs-lookup"><span data-stu-id="b68a4-118">Adding the AJAX Control Toolkit to the Toolbox</span></span>

<span data-ttu-id="b68a4-119">A maneira mais fácil de usar o AJAX Control Toolkit é adicionar o Kit de ferramentas para sua caixa de ferramentas do Visual Studio/Visual Web Developer (veja a Figura 3).</span><span class="sxs-lookup"><span data-stu-id="b68a4-119">The easiest way to use the AJAX Control Toolkit is to add the toolkit to your Visual Studio/Visual Web Developer toolbox (see Figure 3).</span></span> <span data-ttu-id="b68a4-120">Dessa forma, você pode simplesmente arrastar um controle do Kit de ferramentas em uma página quando você deseja usá-lo.</span><span class="sxs-lookup"><span data-stu-id="b68a4-120">That way, you can simply drag a toolkit control onto a page when you want to use it.</span></span>


<span data-ttu-id="b68a4-121">[![AJAX Control Toolkit é exibido na caixa de ferramentas](get-started-with-the-ajax-control-toolkit-vb/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="b68a4-121">[![AJAX Control Toolkit appears in toolbox](get-started-with-the-ajax-control-toolkit-vb/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image5.png)</span></span>

<span data-ttu-id="b68a4-122">**Figura 03**: AJAX Control Toolkit é exibido na caixa de ferramentas ([clique para exibir a imagem em tamanho normal](get-started-with-the-ajax-control-toolkit-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="b68a4-122">**Figure 03**: AJAX Control Toolkit appears in toolbox([Click to view full-size image](get-started-with-the-ajax-control-toolkit-vb/_static/image6.png))</span></span>


<span data-ttu-id="b68a4-123">Primeiro, você precisará adicionar uma guia do AJAX Control Toolkit à caixa de ferramentas.</span><span class="sxs-lookup"><span data-stu-id="b68a4-123">First, you need to add an AJAX Control Toolkit tab to the toolbox.</span></span> <span data-ttu-id="b68a4-124">Siga estas etapas.</span><span class="sxs-lookup"><span data-stu-id="b68a4-124">Follow these steps.</span></span>

1. <span data-ttu-id="b68a4-125">Crie um novo site do ASP.NET, selecionando a opção de menu Arquivo, novo site.</span><span class="sxs-lookup"><span data-stu-id="b68a4-125">Create a new ASP.NET Website by selecting the menu option File, New Website.</span></span> <span data-ttu-id="b68a4-126">Clique duas vezes em Default. aspx na janela do Gerenciador de soluções para abrir o arquivo no editor.</span><span class="sxs-lookup"><span data-stu-id="b68a4-126">Double-click the Default.aspx in the Solution Explorer window to open the file in the editor.</span></span>
2. <span data-ttu-id="b68a4-127">A caixa de ferramentas sob a guia geral com o botão direito e selecione a opção de menu **Adicionar guia** (veja a Figura 4).</span><span class="sxs-lookup"><span data-stu-id="b68a4-127">Right-click the Toolbox beneath the General Tab and select the menu option **Add Tab** (see Figure 4).</span></span>
3. <span data-ttu-id="b68a4-128">Insira uma nova guia chamada AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="b68a4-128">Enter a new tab named AJAX Control Toolkit.</span></span>


<span data-ttu-id="b68a4-129">[![Adicionar uma nova guia](get-started-with-the-ajax-control-toolkit-vb/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="b68a4-129">[![Adding a new tab](get-started-with-the-ajax-control-toolkit-vb/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image7.png)</span></span>

<span data-ttu-id="b68a4-130">**Figura 04**: adicionar uma nova guia ([clique para exibir a imagem em tamanho normal](get-started-with-the-ajax-control-toolkit-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="b68a4-130">**Figure 04**: Adding a new tab([Click to view full-size image](get-started-with-the-ajax-control-toolkit-vb/_static/image8.png))</span></span>


<span data-ttu-id="b68a4-131">Em seguida, você precisa adicionar os controles do AJAX Control Toolkit para a nova guia. Siga estas etapas:</span><span class="sxs-lookup"><span data-stu-id="b68a4-131">Next, you need to add the AJAX Control Toolkit controls to the new tab. Follow these steps:</span></span>

- <span data-ttu-id="b68a4-132">Clique com botão direito sob a guia do AJAX Control Toolkit e selecionar a opção de menu **escolher itens (consulte a Figura 5)**.</span><span class="sxs-lookup"><span data-stu-id="b68a4-132">Right-click beneath the AJAX Control Toolkit tab and select the menu option **Choose Items (see Figure 5)**.</span></span>
- <span data-ttu-id="b68a4-133">Navegue até o local onde você descompactou o AJAX Control Toolkit e selecione o assembly AjaxControlToolkit.</span><span class="sxs-lookup"><span data-stu-id="b68a4-133">Browse to the location where you unzipped the AJAX Control Toolkit and select the AjaxControlToolkit.dll assembly.</span></span>


<span data-ttu-id="b68a4-134">[![Escolha os itens a serem adicionados à caixa de ferramentas](get-started-with-the-ajax-control-toolkit-vb/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="b68a4-134">[![Choose items to add to the toolbox](get-started-with-the-ajax-control-toolkit-vb/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image9.png)</span></span>

<span data-ttu-id="b68a4-135">**Figura 05**: escolha os itens a serem adicionados à caixa de ferramentas ([clique para exibir a imagem em tamanho normal](get-started-with-the-ajax-control-toolkit-vb/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="b68a4-135">**Figure 05**: Choose items to add to the toolbox([Click to view full-size image](get-started-with-the-ajax-control-toolkit-vb/_static/image10.png))</span></span>


<span data-ttu-id="b68a4-136">Depois de concluir essas etapas, todos os controles do Kit de ferramentas serão exibida na caixa de ferramentas.</span><span class="sxs-lookup"><span data-stu-id="b68a4-136">After you complete these steps, all of the toolkit controls will appear in your toolbox.</span></span>

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a><span data-ttu-id="b68a4-137">Atualizando para uma nova versão do Kit de ferramentas</span><span class="sxs-lookup"><span data-stu-id="b68a4-137">Upgrading to a New Version of the Toolkit</span></span>

<span data-ttu-id="b68a4-138">Se você estivesse usando uma versão mais antiga do Kit de ferramentas e agora preciso migrar para uma versão mais recente aqui são as etapas recomendadas:</span><span class="sxs-lookup"><span data-stu-id="b68a4-138">If you were using an older release of the Toolkit and now need to move to a later version here are the recommended steps:</span></span>

- <span data-ttu-id="b68a4-139">Binários - excluir a versão antiga do assembly AjaxControlToolkit da pasta Bin do site.</span><span class="sxs-lookup"><span data-stu-id="b68a4-139">Binaries - Delete the old version of the AjaxControlToolkit.dll assembly from your website Bin folder.</span></span>
- <span data-ttu-id="b68a4-140">Itens de caixa de ferramentas - excluir a guia do AJAX Control Toolkit e siga as etapas acima para recriar a guia com a nova versão do assembly AjaxControlToolkit.</span><span class="sxs-lookup"><span data-stu-id="b68a4-140">Toolbox Items - Delete the AJAX Control Toolkit tab and follow the steps above to re-create the tab with the new version of the AjaxControlToolkit.dll assembly.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b68a4-141">[Anterior](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)
> [Próximo](using-ajax-control-toolkit-controls-and-control-extenders-vb.md)</span><span class="sxs-lookup"><span data-stu-id="b68a4-141">[Previous](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)
[Next](using-ajax-control-toolkit-controls-and-control-extenders-vb.md)</span></span>
