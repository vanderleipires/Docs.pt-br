---
uid: web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-cs
title: Usando controles de kit de ferramentas de controle AJAX e extensores de controle (c#) | Microsoft Docs
author: microsoft
description: Saiba como adicionar controles AJAX Control Toolkit e extensores para suas páginas ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: c1e6b51c-3bc3-4bf7-9916-9991197af3dd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-cs
msc.type: authoredcontent
ms.openlocfilehash: 3d7cea2452db01ca116849ffb17631db3b935668
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="using-ajax-control-toolkit-controls-and-control-extenders-c"></a><span data-ttu-id="a458e-103">Usando controles de kit de ferramentas de controle AJAX e extensores de controle (c#)</span><span class="sxs-lookup"><span data-stu-id="a458e-103">Using AJAX Control Toolkit Controls and Control Extenders (C#)</span></span>
====================
<span data-ttu-id="a458e-104">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="a458e-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="a458e-105">Saiba como adicionar controles AJAX Control Toolkit e extensores para suas páginas ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a458e-105">Learn how to add AJAX Control Toolkit controls and extenders to your ASP.NET pages.</span></span>


<span data-ttu-id="a458e-106">O Kit de ferramentas de controle AJAX contém um conjunto de controles e extensores de controle.</span><span class="sxs-lookup"><span data-stu-id="a458e-106">The AJAX Control Toolkit contains a set of controls and control extenders.</span></span> <span data-ttu-id="a458e-107">Este breve tutorial, você aprenderá como adicionar controles e extensores de controle a uma página ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a458e-107">In this brief tutorial, you learn how to add both controls and control extenders to an ASP.NET page.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="a458e-108">Para obter instruções sobre como instalar o Kit de ferramentas de controle AJAX e adicionando o Kit de ferramentas de controle AJAX para a caixa de ferramentas do Visual Studio/Visual Web Developer, consulte o tutorial [começar com o Kit de ferramentas de controle AJAX](get-started-with-the-ajax-control-toolkit-cs.md).</span><span class="sxs-lookup"><span data-stu-id="a458e-108">For instructions on installing the AJAX Control Toolkit and adding the AJAX Control Toolkit to the Visual Studio/Visual Web Developer toolbox, see the tutorial [Get Started with the AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs.md).</span></span>


## <a name="using-ajax-control-toolkit-controls"></a><span data-ttu-id="a458e-109">Usando controles de kit de ferramentas de controle AJAX</span><span class="sxs-lookup"><span data-stu-id="a458e-109">Using AJAX Control Toolkit Controls</span></span>

<span data-ttu-id="a458e-110">Um controle AJAX Control Toolkit funciona como um controle normal do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a458e-110">An AJAX Control Toolkit control works just like a normal ASP.NET control.</span></span> <span data-ttu-id="a458e-111">Você pode arrastar o controle da caixa de ferramentas em uma página ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a458e-111">You can drag the control from the toolbox onto an ASP.NET page.</span></span> <span data-ttu-id="a458e-112">Você pode adicionar o controle para a página no modo Design ou exibição da fonte.</span><span class="sxs-lookup"><span data-stu-id="a458e-112">You can add the control to the page in either Design view or Source view.</span></span>

<span data-ttu-id="a458e-113">Há um requisito especial ao usar os controles do Kit de ferramentas de controle AJAX.</span><span class="sxs-lookup"><span data-stu-id="a458e-113">There is one special requirement when using the controls from the AJAX Control Toolkit.</span></span> <span data-ttu-id="a458e-114">A página deve conter um controle ScriptManager.</span><span class="sxs-lookup"><span data-stu-id="a458e-114">The page must contain a ScriptManager control.</span></span> <span data-ttu-id="a458e-115">O controle ScriptManager é responsável por incluindo todos o JavaScript necessário necessário para os controles do Kit de ferramentas de controle AJAX.</span><span class="sxs-lookup"><span data-stu-id="a458e-115">The ScriptManager control is responsible for including all of the necessary JavaScript required by the AJAX Control Toolkit controls.</span></span>

<span data-ttu-id="a458e-116">Por exemplo, a guia de AJAX Control Toolkit inclui um controle chamado o controle de Editor.</span><span class="sxs-lookup"><span data-stu-id="a458e-116">For example, the AJAX Control Toolkit tab includes a control named the Editor control.</span></span> <span data-ttu-id="a458e-117">Esse controle exibe um poderoso editor de HTML.</span><span class="sxs-lookup"><span data-stu-id="a458e-117">This control displays a rich HTML editor.</span></span> <span data-ttu-id="a458e-118">Siga estas etapas para adicionar o controle de Editor para uma página:</span><span class="sxs-lookup"><span data-stu-id="a458e-118">Follow these steps to add the Editor control to a page:</span></span>

1. <span data-ttu-id="a458e-119">Criar uma nova página ASP.NET chamada ShowEditor.aspx</span><span class="sxs-lookup"><span data-stu-id="a458e-119">Create a new ASP.NET page named ShowEditor.aspx</span></span>
2. <span data-ttu-id="a458e-120">Selecione o controle ScriptManager do guia Extensões AJAX na caixa de ferramentas e arraste o controle para a página.</span><span class="sxs-lookup"><span data-stu-id="a458e-120">Select the ScriptManager control from beneath the AJAX Extensions tab in the toolbox and drag the control onto the page.</span></span>
3. <span data-ttu-id="a458e-121">Selecione o controle de Editor do guia do Kit de ferramentas de controle AJAX na caixa de ferramentas e arraste o controle para a página (consulte a Figura 1).</span><span class="sxs-lookup"><span data-stu-id="a458e-121">Select the Editor control from beneath the AJAX Control Toolkit tab in the toolbox and drag the control onto the page (see Figure 1).</span></span> <span data-ttu-id="a458e-122">O Designer deve parecer com a Figura 2.</span><span class="sxs-lookup"><span data-stu-id="a458e-122">The Designer should look like Figure 2.</span></span>
4. <span data-ttu-id="a458e-123">Executar o site selecionando a opção de menu **depurar, iniciar depuração** ou pressionar a tecla F5.</span><span class="sxs-lookup"><span data-stu-id="a458e-123">Run the web site by selecting the menu option **Debug, Start Debugging** or hitting the F5 key.</span></span>
5. <span data-ttu-id="a458e-124">Você verá a página na Figura 3.</span><span class="sxs-lookup"><span data-stu-id="a458e-124">You should see the page in Figure 3.</span></span>


<span data-ttu-id="a458e-125">[![Selecionando o controle de Editor de HTML](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a458e-125">[![Selecting the HTML Editor control](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.png)</span></span>

<span data-ttu-id="a458e-126">**Figura 01**: selecionando o controle de Editor de HTML ([clique para exibir a imagem em tamanho normal](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="a458e-126">**Figure 01**: Selecting the HTML Editor control([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.png))</span></span>


<span data-ttu-id="a458e-127">[![Designer do Visual Studio com o controle ScriptManager e editar](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="a458e-127">[![Visual Studio Designer with ScriptManager and Edit control](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.png)</span></span>

<span data-ttu-id="a458e-128">**Figura 02**: Visual Studio Designer com controle ScriptManager e editar ([clique para exibir a imagem em tamanho normal](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="a458e-128">**Figure 02**: Visual Studio Designer with ScriptManager and Edit control([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.png))</span></span>


<span data-ttu-id="a458e-129">[![A página DisplayEditor.aspx](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="a458e-129">[![The DisplayEditor.aspx page](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.png)</span></span>

<span data-ttu-id="a458e-130">**Figura 03**: DisplayEditor.aspx a página ([clique para exibir a imagem em tamanho normal](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="a458e-130">**Figure 03**: The DisplayEditor.aspx page([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.png))</span></span>


## <a name="using-ajax-control-toolkit-control-extenders"></a><span data-ttu-id="a458e-131">Usando extensores de controle do Kit de ferramentas de controle AJAX</span><span class="sxs-lookup"><span data-stu-id="a458e-131">Using AJAX Control Toolkit Control Extenders</span></span>

<span data-ttu-id="a458e-132">O Kit de ferramentas de controle AJAX também contém os Extensores do controle.</span><span class="sxs-lookup"><span data-stu-id="a458e-132">The AJAX Control Toolkit also contains control extenders.</span></span> <span data-ttu-id="a458e-133">Como o nome sugere, um extensor de controle estende a funcionalidade de um controle existente.</span><span class="sxs-lookup"><span data-stu-id="a458e-133">As its name suggests, a control extender extends the functionality of an existing control.</span></span> <span data-ttu-id="a458e-134">Por exemplo, o extensor do controle ConfirmButton estende o controle de botão do ASP.NET padrão.</span><span class="sxs-lookup"><span data-stu-id="a458e-134">For example, the ConfirmButton control extender extends the standard ASP.NET Button control.</span></span> <span data-ttu-id="a458e-135">O extensor altera o comportamento do botão controle s para que o botão exibe uma caixa de diálogo de confirmação quando você clica nele.</span><span class="sxs-lookup"><span data-stu-id="a458e-135">The extender changes the Button control�s behavior so that the Button displays a confirmation dialog when you click it.</span></span>

<span data-ttu-id="a458e-136">Um extensor de controle, assim como um controle AJAX Control Toolkit, requer um controle ScriptManager.</span><span class="sxs-lookup"><span data-stu-id="a458e-136">A control extender, just like an AJAX Control Toolkit control, requires a ScriptManager control.</span></span> <span data-ttu-id="a458e-137">Você deve adicionar um controle ScriptManager para uma página para começar a usar os Extensores do controle na página.</span><span class="sxs-lookup"><span data-stu-id="a458e-137">You must add a ScriptManager control to a page before you start using control extenders in the page.</span></span>

<span data-ttu-id="a458e-138">Siga estas etapas para usar o extensor do controle ConfirmButton:</span><span class="sxs-lookup"><span data-stu-id="a458e-138">Follow these steps to use the ConfirmButton control extender:</span></span>

1. <span data-ttu-id="a458e-139">Criar uma nova página ASP.NET chamada ShowConfirmButton.aspx</span><span class="sxs-lookup"><span data-stu-id="a458e-139">Create a new ASP.NET page named ShowConfirmButton.aspx</span></span>
2. <span data-ttu-id="a458e-140">Adicione um controle ScriptManager à página, arrastando o controle para a página de guia Extensões AJAX.</span><span class="sxs-lookup"><span data-stu-id="a458e-140">Add a ScriptManager control to the page by dragging the control onto the page from beneath the AJAX Extensions tab.</span></span>
3. <span data-ttu-id="a458e-141">Adicione um controle de botão padrão para a página arrastando o botão de guia padrão na caixa de ferramentas para a superfície de Designer.</span><span class="sxs-lookup"><span data-stu-id="a458e-141">Add a standard Button control to the page by dragging the Button from beneath the Standard tab in the toolbox onto the Designer surface.</span></span>
4. <span data-ttu-id="a458e-142">Clique o **adicionar extensor** opção da tarefa (consulte a Figura 4).</span><span class="sxs-lookup"><span data-stu-id="a458e-142">Click the **Add Extender** task option (see Figure 4).</span></span>
5. <span data-ttu-id="a458e-143">Na caixa de diálogo Escolher extensor, selecione ConfirmButtonExtender (consulte a Figura 5) e clique no botão Okey.</span><span class="sxs-lookup"><span data-stu-id="a458e-143">In the Choose Extender dialog, select ConfirmButtonExtender (see Figure 5) and click the OK button.</span></span>
6. <span data-ttu-id="a458e-144">Selecione o controle de botão no Designer e expanda os extensores, Button1\_ConfirmButtonExtender nó na janela Propriedades (consulte a Figura 6).</span><span class="sxs-lookup"><span data-stu-id="a458e-144">Select the Button control in the Designer and expand the Extenders, Button1\_ConfirmButtonExtender node in the Properties window (see Figure 6).</span></span> <span data-ttu-id="a458e-145">Atribuir o valor *realmente?* para a propriedade ConfirmText.</span><span class="sxs-lookup"><span data-stu-id="a458e-145">Assign the value *�Really?�* to the ConfirmText property.</span></span>
7. <span data-ttu-id="a458e-146">Execute a página, selecionando a opção de menu **depurar, iniciar depuração** ou pressione a tecla F5.</span><span class="sxs-lookup"><span data-stu-id="a458e-146">Run the page by selecting the menu option **Debug, Start Debugging** or hit the F5 key.</span></span>


<span data-ttu-id="a458e-147">[![A opção de adicionar extensor de tarefas](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="a458e-147">[![The Add Extender task option](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.png)</span></span>

<span data-ttu-id="a458e-148">**Figura 04**: opção de tarefas a adicionar extensor ([clique para exibir a imagem em tamanho normal](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="a458e-148">**Figure 04**: The Add Extender task option([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image8.png))</span></span>


<span data-ttu-id="a458e-149">[![Selecionando o extensor do controle ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="a458e-149">[![Selecting the ConfirmButton control extender](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image9.png)</span></span>

<span data-ttu-id="a458e-150">**Figura 05**: selecionando o extensor do controle ConfirmButton ([clique para exibir a imagem em tamanho normal](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="a458e-150">**Figure 05**: Selecting the ConfirmButton control extender([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image10.png))</span></span>


<span data-ttu-id="a458e-151">[![Definir uma propriedade ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="a458e-151">[![Setting a ConfirmButton property](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image11.png)</span></span>

<span data-ttu-id="a458e-152">**Figura 06**: definir uma propriedade ConfirmButton ([clique para exibir a imagem em tamanho normal](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="a458e-152">**Figure 06**: Setting a ConfirmButton property([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image12.png))</span></span>


<span data-ttu-id="a458e-153">Quando a página é aberta, você verá um botão.</span><span class="sxs-lookup"><span data-stu-id="a458e-153">When the page opens, you should see a button.</span></span> <span data-ttu-id="a458e-154">Quando você clicar no botão, você obtém a caixa de diálogo de confirmação na Figura 7.</span><span class="sxs-lookup"><span data-stu-id="a458e-154">When you click the button, you get the confirmation dialog in Figure 7.</span></span>


<span data-ttu-id="a458e-155">[![Exibir a caixa de diálogo de confirmação](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="a458e-155">[![Displaying the confirmation dialog](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image13.png)</span></span>

<span data-ttu-id="a458e-156">**Figura 07**: exibindo a caixa de diálogo de confirmação ([clique para exibir a imagem em tamanho normal](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="a458e-156">**Figure 07**: Displaying the confirmation dialog([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image14.png))</span></span>


<span data-ttu-id="a458e-157">Observe que você normalmente não arrastar um extensor de controle para uma página.</span><span class="sxs-lookup"><span data-stu-id="a458e-157">Notice that you normally do not drag a control extender onto a page.</span></span> <span data-ttu-id="a458e-158">Em vez disso, você usar o **adicionar extensor** opção para adicionar um extensor para um controle que você já tiver adicionado a uma página de tarefas.</span><span class="sxs-lookup"><span data-stu-id="a458e-158">Instead, you use the **Add Extender** task option to add an extender to a control that you have already added to a page.</span></span> <span data-ttu-id="a458e-159">Além disso, observe que você defina controle propriedades estendidas, abra a folha de propriedades para o controle que está sendo estendido.</span><span class="sxs-lookup"><span data-stu-id="a458e-159">Notice, furthermore, that you set control extender properties by opening the property sheet for the control being extended.</span></span>

<span data-ttu-id="a458e-160">Um único controle ASP.NET pode ser estendido por vários extensores de controle.</span><span class="sxs-lookup"><span data-stu-id="a458e-160">A single ASP.NET control can be extended by multiple control extenders.</span></span> <span data-ttu-id="a458e-161">A folha de propriedades para o controle que está sendo estendido listará todos os Extensores do controle associados ao controle.</span><span class="sxs-lookup"><span data-stu-id="a458e-161">The property sheet for the control being extended will list all of the control extenders associated with the control.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a458e-162">[Anterior](get-started-with-the-ajax-control-toolkit-cs.md)
> [Próximo](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)</span><span class="sxs-lookup"><span data-stu-id="a458e-162">[Previous](get-started-with-the-ajax-control-toolkit-cs.md)
[Next](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)</span></span>
