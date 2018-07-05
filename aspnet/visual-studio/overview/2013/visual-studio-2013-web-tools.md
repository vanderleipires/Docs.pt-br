---
uid: visual-studio/overview/2013/visual-studio-2013-web-tools
title: 'Laboratório prático: Visual Studio 2013 Web Tools | Microsoft Docs'
author: rick-anderson
description: Visual Studio é um ambiente de desenvolvimento excelente para. Windows com base em NET e projetos da web. Ele inclui um editor de texto avançada que pode ser facilmente usado para...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: 09e82351-816b-402d-acd1-0f9ac6901d16
ms.technology: ''
msc.legacyurl: /visual-studio/overview/2013/visual-studio-2013-web-tools
msc.type: authoredcontent
ms.openlocfilehash: 8f0f19a184d50cdc6e8e5a2da0e55a0e8ca44dc5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399394"
---
<a name="hands-on-lab-visual-studio-2013-web-tools"></a><span data-ttu-id="13fc6-104">Laboratório prático: Ferramentas de Web Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="13fc6-104">Hands On Lab: Visual Studio 2013 Web Tools</span></span>
====================
<span data-ttu-id="13fc6-105">por [Web Camps equipe](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="13fc6-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="13fc6-106">Baixe o Kit de treinamento do Web Camps</span><span class="sxs-lookup"><span data-stu-id="13fc6-106">Download Web Camps Training Kit</span></span>](http://aka.ms/webcamps-training-kit)

> <span data-ttu-id="13fc6-107">Visual Studio é um ambiente de desenvolvimento excelente para. Windows com base em NET e projetos da web.</span><span class="sxs-lookup"><span data-stu-id="13fc6-107">Visual Studio is an excellent development environment for .NET-based Windows and web projects.</span></span> <span data-ttu-id="13fc6-108">Ele inclui um editor de texto avançada que pode ser facilmente usado para editar arquivos autônomos sem um projeto.</span><span class="sxs-lookup"><span data-stu-id="13fc6-108">It includes a powerful text editor that can easily be used to edit standalone files without a project.</span></span>
> 
> <span data-ttu-id="13fc6-109">O Visual Studio mantém uma árvore de análise completa ao editar cada arquivo.</span><span class="sxs-lookup"><span data-stu-id="13fc6-109">Visual Studio maintains a full-featured parse tree as you edit each file.</span></span> <span data-ttu-id="13fc6-110">Isso permite que o Visual Studio forneça inigualável automático e as ações do documento ao fazer a experiência de desenvolvimento muito mais rápida e mais agradável.</span><span class="sxs-lookup"><span data-stu-id="13fc6-110">This allows Visual Studio to provide unparalleled auto-completion and document-based actions while making the development experience much faster and more pleasant.</span></span> <span data-ttu-id="13fc6-111">Esses recursos são particularmente poderosos em documentos HTML e CSS.</span><span class="sxs-lookup"><span data-stu-id="13fc6-111">These features are especially powerful in HTML and CSS documents.</span></span>
> 
> <span data-ttu-id="13fc6-112">Toda essa capacidade também está disponível para extensões, tornando mais fácil estender os editores com novos recursos poderosos para atender às suas necessidades.</span><span class="sxs-lookup"><span data-stu-id="13fc6-112">All of this power is also available for extensions, making it simple to extend the editors with powerful new features to suit your needs.</span></span> <span data-ttu-id="13fc6-113">Web Essentials é uma coleção de (principalmente) aprimoramentos relacionados à web para o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="13fc6-113">Web Essentials is a collection of (mostly) web-related enhancements to Visual Studio.</span></span> <span data-ttu-id="13fc6-114">Ele inclui vários novos preenchimentos do IntelliSense (especialmente para CSS), novos recursos de Link do navegador, automático de arquivos JSHint para JavaScript, novos avisos para HTML e CSS e muitos outros recursos que são essenciais para o desenvolvimento web moderno.</span><span class="sxs-lookup"><span data-stu-id="13fc6-114">It includes lots of new IntelliSense completions (especially for CSS), new Browser Link features, automatic JSHint for JavaScript files, new warnings for HTML and CSS, and many other features that are essential to modern web development.</span></span>
> 
> <span data-ttu-id="13fc6-115">Todo o código de exemplo e trechos de código são incluídos no Web Camps treinamento Kit, disponível em [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="13fc6-115">All sample code and snippets are included in the Web Camps Training Kit, available at [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span></span>


<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="13fc6-116">Visão geral</span><span class="sxs-lookup"><span data-stu-id="13fc6-116">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="13fc6-117">Objetivos</span><span class="sxs-lookup"><span data-stu-id="13fc6-117">Objectives</span></span>

<span data-ttu-id="13fc6-118">Neste laboratório prático, você aprenderá como:</span><span class="sxs-lookup"><span data-stu-id="13fc6-118">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="13fc6-119">Use os novos recursos do editor HTML incluídos no Web Essentials, como trechos de código avançados do HTML5 e o Zen de codificação</span><span class="sxs-lookup"><span data-stu-id="13fc6-119">Use new HTML editor features included in Web Essentials such as rich HTML5 code snippets and Zen coding</span></span>
- <span data-ttu-id="13fc6-120">Use os novos recursos do editor CSS incluídos no Web Essentials, como o seletor de cor e a dica de ferramenta de matriz de navegador</span><span class="sxs-lookup"><span data-stu-id="13fc6-120">Use new CSS editor features included in Web Essentials such as the Color picker and Browser matrix tooltip</span></span>
- <span data-ttu-id="13fc6-121">Use os novos recursos do editor JavaScript incluídos no Web Essentials, como extrair para o arquivo e o IntelliSense para todos os elementos HTML</span><span class="sxs-lookup"><span data-stu-id="13fc6-121">Use new JavaScript editor features included in Web Essentials such as Extract to File and IntelliSense for all HTML elements</span></span>
- <span data-ttu-id="13fc6-122">Trocar dados entre o navegador e o Visual Studio usando o Link do navegador</span><span class="sxs-lookup"><span data-stu-id="13fc6-122">Exchange data between your browser and Visual Studio using Browser Link</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="13fc6-123">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="13fc6-123">Prerequisites</span></span>

<span data-ttu-id="13fc6-124">O exemplo a seguir é necessário para concluir este laboratório prático:</span><span class="sxs-lookup"><span data-stu-id="13fc6-124">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="13fc6-125">[Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) ou maior</span><span class="sxs-lookup"><span data-stu-id="13fc6-125">[Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="13fc6-126">Web Essentials 2013</span><span class="sxs-lookup"><span data-stu-id="13fc6-126">Web Essentials 2013</span></span>](http://vswebessentials.com/)
- [<span data-ttu-id="13fc6-127">Google Chrome</span><span class="sxs-lookup"><span data-stu-id="13fc6-127">Google Chrome</span></span>](https://www.google.com/chrome/)

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="13fc6-128">Configuração</span><span class="sxs-lookup"><span data-stu-id="13fc6-128">Setup</span></span>

<span data-ttu-id="13fc6-129">Para executar os exercícios neste laboratório prático, você precisará configurar seu ambiente pela primeira vez.</span><span class="sxs-lookup"><span data-stu-id="13fc6-129">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="13fc6-130">Abra uma janela do Windows Explorer e navegue até o laboratório **origem** pasta.</span><span class="sxs-lookup"><span data-stu-id="13fc6-130">Open a Windows Explorer window and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="13fc6-131">Clique com botão direito **Setup. cmd** e selecione **executar como administrador** para iniciar o processo de instalação que irá configurar seu ambiente e instalem os trechos de código do Visual Studio para este laboratório.</span><span class="sxs-lookup"><span data-stu-id="13fc6-131">Right-click **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="13fc6-132">Se a caixa de diálogo controle de conta de usuário for mostrada, confirme a ação para continuar.</span><span class="sxs-lookup"><span data-stu-id="13fc6-132">If the User Account Control dialog box is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="13fc6-133">Verifique se que você tiver marcado todas as dependências para este laboratório antes de executar a instalação.</span><span class="sxs-lookup"><span data-stu-id="13fc6-133">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="13fc6-134">Usando os trechos de código</span><span class="sxs-lookup"><span data-stu-id="13fc6-134">Using the Code Snippets</span></span>

<span data-ttu-id="13fc6-135">Em todo o documento de laboratório, você será instruído a inserir blocos de código.</span><span class="sxs-lookup"><span data-stu-id="13fc6-135">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="13fc6-136">Para sua conveniência, a maioria desse código é fornecido como o Visual Studio trechos de código que pode ser acessada de dentro do Visual Studio 2013 para evitar ter que adicioná-lo manualmente.</span><span class="sxs-lookup"><span data-stu-id="13fc6-136">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="13fc6-137">Cada exercício é acompanhado por uma solução inicial localizada na **começar** pasta do exercício que permite que você siga cada exercício independentemente dos outros.</span><span class="sxs-lookup"><span data-stu-id="13fc6-137">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="13fc6-138">Esteja ciente de que os trechos de código são adicionados durante um exercício estão ausentes desses iniciando soluções e podem não funcionar até concluir o exercício.</span><span class="sxs-lookup"><span data-stu-id="13fc6-138">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="13fc6-139">Dentro do código-fonte para um exercício, você também encontrará uma **final** pasta que contém uma solução do Visual Studio com o código que é o resultado de concluir as etapas no exercício correspondente.</span><span class="sxs-lookup"><span data-stu-id="13fc6-139">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="13fc6-140">Você pode usar essas soluções como uma diretriz se você precisar de ajuda adicional ao trabalhar com este laboratório prático.</span><span class="sxs-lookup"><span data-stu-id="13fc6-140">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


* * *

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="13fc6-141">Exercícios</span><span class="sxs-lookup"><span data-stu-id="13fc6-141">Exercises</span></span>

<span data-ttu-id="13fc6-142">Este laboratório prático inclui os seguintes exercícios:</span><span class="sxs-lookup"><span data-stu-id="13fc6-142">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="13fc6-143">Trabalhando com o Web Essentials e o Link do navegador</span><span class="sxs-lookup"><span data-stu-id="13fc6-143">Working with Browser Link and Web Essentials</span></span>](#Exercise1)
2. [<span data-ttu-id="13fc6-144">Aproveitando as vantagens de trechos de código e o IntelliSense</span><span class="sxs-lookup"><span data-stu-id="13fc6-144">Taking Advantage of Code Snippets and IntelliSense</span></span>](#Exercise2)

> [!NOTE]
> <span data-ttu-id="13fc6-145">Quando você inicia o Visual Studio pela primeira vez, você deve selecionar uma das coleções de configurações predefinidas.</span><span class="sxs-lookup"><span data-stu-id="13fc6-145">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="13fc6-146">Cada coleção predefinida foi projetada para corresponder a um estilo de desenvolvimento específico e determina o comportamento do editor, layouts de janela, trechos de código IntelliSense e opções da caixa de diálogo.</span><span class="sxs-lookup"><span data-stu-id="13fc6-146">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="13fc6-147">Os procedimentos neste laboratório descrevem as ações necessárias para realizar uma determinada tarefa no Visual Studio ao usar o **configurações gerais de desenvolvimento** coleção.</span><span class="sxs-lookup"><span data-stu-id="13fc6-147">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="13fc6-148">Se você escolher uma coleção de configurações diferentes para seu ambiente de desenvolvimento, pode haver diferenças nas etapas que você deve levar em conta.</span><span class="sxs-lookup"><span data-stu-id="13fc6-148">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-browser-link-and-web-essentials"></a><span data-ttu-id="13fc6-149">Exercício 1: Trabalhar com o Link do navegador e o Web Essentials</span><span class="sxs-lookup"><span data-stu-id="13fc6-149">Exercise 1: Working with Browser Link and Web Essentials</span></span>

<span data-ttu-id="13fc6-150">**Web Essentials** é uma extensão do Visual Studio que adiciona uma variedade de recursos úteis para desenvolvimento web moderno, principalmente concentrada em tornar a experiência de desenvolvimento da web muito mais rápida e mais agradável.</span><span class="sxs-lookup"><span data-stu-id="13fc6-150">**Web Essentials** is a Visual Studio extension that adds a variety of useful features for modern web development, mostly focused on making the web development experience much faster and more pleasant.</span></span> <span data-ttu-id="13fc6-151">Você pode instalar o Web Essentials na Galeria de extensão no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="13fc6-151">You can install Web Essentials from the Extension Gallery in Visual Studio.</span></span>

<span data-ttu-id="13fc6-152">**Link do navegador** é um novo recurso incluído no Visual Studio 2013 que fornece um canal entre o IDE do Visual Studio e qualquer navegador aberto para trocar dados entre seu aplicativo web e o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="13fc6-152">**Browser Link** is a new feature included in Visual Studio 2013 that provides a channel between the Visual Studio IDE and any open browser to exchange data between your web application and Visual Studio.</span></span> <span data-ttu-id="13fc6-153">Web Essentials estende o Link do navegador com as ferramentas para manipular o modelo de objeto do DOM e os estilos CSS de suas páginas da web diretamente do navegador.</span><span class="sxs-lookup"><span data-stu-id="13fc6-153">Web Essentials extends Browser Link with tools to manipulate the DOM object model and the CSS styles of your web pages directly from the browser.</span></span>

<span data-ttu-id="13fc6-154">Neste exercício, você irá explorar alguns dos recursos com suporte pelo **Web Essentials** e **Link do navegador** para aprimorar uma página de teste simples.</span><span class="sxs-lookup"><span data-stu-id="13fc6-154">In this exercise, you will explore some of the features supported by **Web Essentials** and **Browser Link** to enhance a simple quiz page.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1---running-the-project-in-multiple-browsers"></a><span data-ttu-id="13fc6-155">Tarefa 1: executar o projeto em vários navegadores</span><span class="sxs-lookup"><span data-stu-id="13fc6-155">Task 1 - Running the Project in Multiple Browsers</span></span>

<span data-ttu-id="13fc6-156">Nesta tarefa, você irá configurar seu aplicativo web para ser executado em vários navegadores, ao mesmo tempo, o que é útil para testes entre navegadores.</span><span class="sxs-lookup"><span data-stu-id="13fc6-156">In this task, you will configure your web application to run in multiple browsers at once, which is useful for cross-browser testing.</span></span>

1. <span data-ttu-id="13fc6-157">Abra **Microsoft Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="13fc6-157">Open **Microsoft Visual Studio**.</span></span>
2. <span data-ttu-id="13fc6-158">No **arquivo** menu, selecione **abrir | Projeto/solução...**  e navegue até **Ex1 WorkingwithBrowserLinkandWebEssentials\Begin** no **origem** pasta do laboratório (C:\WebCampsTK\HOL\VSWebTooling\Source).</span><span class="sxs-lookup"><span data-stu-id="13fc6-158">In the **File** menu, select **Open | Project/Solution...** and browse to **Ex1-WorkingwithBrowserLinkandWebEssentials\Begin** in the **Source** folder of the lab (C:\WebCampsTK\HOL\VSWebTooling\Source).</span></span> <span data-ttu-id="13fc6-159">Selecione **Begin.sln** e clique em **abrir**.</span><span class="sxs-lookup"><span data-stu-id="13fc6-159">Select **Begin.sln** and click **Open**.</span></span>
3. <span data-ttu-id="13fc6-160">Na barra de ferramentas do Visual Studio, expanda o menu de navegador e selecione **procurar com...** .</span><span class="sxs-lookup"><span data-stu-id="13fc6-160">In the Visual Studio toolbar, expand the browser menu and select **Browse With...**.</span></span>

    <span data-ttu-id="13fc6-161">![Opção de menu procurar com](visual-studio-2013-web-tools/_static/image1.png "procurar com... no menu do navegador")</span><span class="sxs-lookup"><span data-stu-id="13fc6-161">![Browse With menu option](visual-studio-2013-web-tools/_static/image1.png "Browse with... in browser menu")</span></span>

    <span data-ttu-id="13fc6-162">*Opção de menu procurar com*</span><span class="sxs-lookup"><span data-stu-id="13fc6-162">*Browse With menu option*</span></span>
4. <span data-ttu-id="13fc6-163">No **procurar com** caixa de diálogo, selecione **Google Chrome** e **Internet Explorer** segurando o **CTRL** da chave e clique em  **Definir como padrão**.</span><span class="sxs-lookup"><span data-stu-id="13fc6-163">In the **Browse With** dialog box, select both **Google Chrome** and **Internet Explorer** by holding down the **CTRL** key and click **Set as Default**.</span></span>

    <span data-ttu-id="13fc6-164">![Procurar com a caixa de diálogo](visual-studio-2013-web-tools/_static/image2.png "procurar com a caixa de diálogo")</span><span class="sxs-lookup"><span data-stu-id="13fc6-164">![Browse with dialog box](visual-studio-2013-web-tools/_static/image2.png "Browse with dialog box")</span></span>

    <span data-ttu-id="13fc6-165">*Selecionar vários navegadores padrão*</span><span class="sxs-lookup"><span data-stu-id="13fc6-165">*Selecting multiple default browsers*</span></span>
5. <span data-ttu-id="13fc6-166">Google Chrome e o Internet Explorer agora devem aparecer como os navegadores padrão.</span><span class="sxs-lookup"><span data-stu-id="13fc6-166">Both Google Chrome and Internet Explorer should now appear as the default browsers.</span></span> <span data-ttu-id="13fc6-167">Clique em **Cancelar** para fechar a caixa de diálogo.</span><span class="sxs-lookup"><span data-stu-id="13fc6-167">Click **Cancel** to close the dialog box.</span></span>

    <span data-ttu-id="13fc6-168">![Google Chrome e o Internet Explorer como navegadores padrão](visual-studio-2013-web-tools/_static/image3.png "navegadores de padrão do Internet Explorer e Google Chrome")</span><span class="sxs-lookup"><span data-stu-id="13fc6-168">![Google Chrome and Internet Explorer as default browsers](visual-studio-2013-web-tools/_static/image3.png "Google Chrome and Internet Explorer default browsers")</span></span>

    <span data-ttu-id="13fc6-169">*Google Chrome e o Internet Explorer como navegadores padrão*</span><span class="sxs-lookup"><span data-stu-id="13fc6-169">*Google Chrome and Internet Explorer as default browsers*</span></span>

    > [!NOTE]
    > <span data-ttu-id="13fc6-170">Depois de configurar os navegadores padrão, o **vários navegadores** opção é selecionada no menu do navegador.</span><span class="sxs-lookup"><span data-stu-id="13fc6-170">After configuring the default browsers, the **Multiple Browsers** option is selected in the browser menu.</span></span>
    > 
    > <span data-ttu-id="13fc6-171">![Vários navegadores](visual-studio-2013-web-tools/_static/image4.png "vários navegadores")</span><span class="sxs-lookup"><span data-stu-id="13fc6-171">![Multiple browsers](visual-studio-2013-web-tools/_static/image4.png "Multiple browsers")</span></span>
6. <span data-ttu-id="13fc6-172">Pressione **CTRL** + **F5** para executar o aplicativo sem depuração.</span><span class="sxs-lookup"><span data-stu-id="13fc6-172">Press **CTRL** + **F5** to run the application without debugging.</span></span>
7. <span data-ttu-id="13fc6-173">Quando ambas as janelas do navegador se abre, coloque um deles acima do outro para ver as atualizações em ambos os navegadores simultaneamente.</span><span class="sxs-lookup"><span data-stu-id="13fc6-173">When both browser windows open, place one of them above the other in order to see the updates on both browsers simultaneously.</span></span> <span data-ttu-id="13fc6-174">Os navegadores devem exibir uma página da web com um retângulo azul claro.</span><span class="sxs-lookup"><span data-stu-id="13fc6-174">The browsers should display a web page with a light-blue rectangle.</span></span>

    <span data-ttu-id="13fc6-175">![Colocando um navegador acima do outro](visual-studio-2013-web-tools/_static/image5.png "colocando um navegador acima do outro")</span><span class="sxs-lookup"><span data-stu-id="13fc6-175">![Placing one browser above the other](visual-studio-2013-web-tools/_static/image5.png "Placing one browser above the other")</span></span>

    <span data-ttu-id="13fc6-176">*Colocando um navegador acima do outro*</span><span class="sxs-lookup"><span data-stu-id="13fc6-176">*Placing one browser above the other*</span></span>
8. <span data-ttu-id="13fc6-177">Não feche os navegadores.</span><span class="sxs-lookup"><span data-stu-id="13fc6-177">Do not close the browsers.</span></span> <span data-ttu-id="13fc6-178">Você irá usá-los na próxima tarefa.</span><span class="sxs-lookup"><span data-stu-id="13fc6-178">You will use them in the next task.</span></span>

<a id="Ex1Task2"></a>
#### <a name="task-2---using-zen-coding-to-create-html-elements"></a><span data-ttu-id="13fc6-179">Tarefa 2 - Zen usando codificação para criar elementos HTML</span><span class="sxs-lookup"><span data-stu-id="13fc6-179">Task 2 - Using Zen Coding to Create HTML Elements</span></span>

<span data-ttu-id="13fc6-180">**Codificação Zen** é um editor de codificação de plug-in para alta velocidade HTML, XML, XSL (ou qualquer outro formato de código estruturado) e de edição.</span><span class="sxs-lookup"><span data-stu-id="13fc6-180">**Zen Coding** is an editor plugin for high-speed HTML, XML, XSL (or any other structured code format) coding and editing.</span></span> <span data-ttu-id="13fc6-181">O núcleo desse plug-in é um mecanismo de abreviação poderosa que permite que você expanda expressões - semelhantes para seletores de CSS - no código HTML.</span><span class="sxs-lookup"><span data-stu-id="13fc6-181">The core of this plugin is a powerful abbreviation engine which allows you to expand expressions -similar to CSS selectors- into HTML code.</span></span> <span data-ttu-id="13fc6-182">Codificação de Zen é uma maneira rápida de escrever a sintaxe do seletor de estilo HTML usando CSS.</span><span class="sxs-lookup"><span data-stu-id="13fc6-182">Zen Coding is a fast way to write HTML using a CSS style selector syntax.</span></span>

<span data-ttu-id="13fc6-183">Neste exercício, você usará o recurso Zen codificação fornecido pelo Web Essentials para gerar os botões HTML que representam as opções da pergunta.</span><span class="sxs-lookup"><span data-stu-id="13fc6-183">In this exercise, you will use the Zen Coding feature provided by Web Essentials to generate the HTML buttons that represent the options of the question.</span></span>

1. <span data-ttu-id="13fc6-184">Alterne para Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="13fc6-184">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="13fc6-185">Abra o **index. cshtml** arquivo localizado na **exibições** | **Home** pasta.</span><span class="sxs-lookup"><span data-stu-id="13fc6-185">Open the **Index.cshtml** file located in the **Views** | **Home** folder.</span></span>
3. <span data-ttu-id="13fc6-186">Substitua os **&lt;! – TODO: Adicione aqui – opções&gt;** comentário com o seguinte código e pressione **guia**.</span><span class="sxs-lookup"><span data-stu-id="13fc6-186">Replace the **&lt;!-- TODO: add options here--&gt;** comment with the following code, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample1.css)]
4. <span data-ttu-id="13fc6-187">O código deve ser expandido para HTML.</span><span class="sxs-lookup"><span data-stu-id="13fc6-187">The code should be expanded to HTML.</span></span>

    <span data-ttu-id="13fc6-188">![Expandido HTML](visual-studio-2013-web-tools/_static/image6.png "expandido HTML")</span><span class="sxs-lookup"><span data-stu-id="13fc6-188">![Expanded HTML](visual-studio-2013-web-tools/_static/image6.png "Expanded HTML")</span></span>

    <span data-ttu-id="13fc6-189">*HTML expandido*</span><span class="sxs-lookup"><span data-stu-id="13fc6-189">*Expanded HTML*</span></span>

    > [!NOTE]
    > <span data-ttu-id="13fc6-190">Para saber mais sobre Zen codificação sintaxe, consulte o seguinte [artigo](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).</span><span class="sxs-lookup"><span data-stu-id="13fc6-190">To learn more about Zen Coding syntax, see the following [article](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).</span></span>
5. <span data-ttu-id="13fc6-191">Clique o **atualizar navegadores vinculados** botão atualizar ambos os navegadores.</span><span class="sxs-lookup"><span data-stu-id="13fc6-191">Click the **Refresh linked browsers** button to update both browsers.</span></span>

    <span data-ttu-id="13fc6-192">![Atualizar navegadores vinculados](visual-studio-2013-web-tools/_static/image7.png "atualizar navegadores vinculados")</span><span class="sxs-lookup"><span data-stu-id="13fc6-192">![Refresh linked browsers](visual-studio-2013-web-tools/_static/image7.png "Refresh linked browsers")</span></span>

    <span data-ttu-id="13fc6-193">*Atualizar navegadores vinculados*</span><span class="sxs-lookup"><span data-stu-id="13fc6-193">*Refresh linked browsers*</span></span>

    <span data-ttu-id="13fc6-194">![Internet Explorer - página atualizada com quatro botões](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer - página atualizada com quatro botões")</span><span class="sxs-lookup"><span data-stu-id="13fc6-194">![Internet Explorer - Page updated with four buttons](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer - Page updated with four buttons")</span></span>

    <span data-ttu-id="13fc6-195">*Internet Explorer - página atualizada com quatro botões*</span><span class="sxs-lookup"><span data-stu-id="13fc6-195">*Internet Explorer - Page updated with four buttons*</span></span>

    <span data-ttu-id="13fc6-196">![Google Chrome - página atualizada com quatro botões](visual-studio-2013-web-tools/_static/image9.png "Google Chrome - página atualizada com quatro botões")</span><span class="sxs-lookup"><span data-stu-id="13fc6-196">![Google Chrome - Page updated with four buttons](visual-studio-2013-web-tools/_static/image9.png "Google Chrome - Page updated with four buttons")</span></span>

    <span data-ttu-id="13fc6-197">*Google Chrome - página atualizada com quatro botões*</span><span class="sxs-lookup"><span data-stu-id="13fc6-197">*Google Chrome - Page updated with four buttons*</span></span>
6. <span data-ttu-id="13fc6-198">Alterne para Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="13fc6-198">Switch back to Visual Studio.</span></span>
7. <span data-ttu-id="13fc6-199">Você adicionou os botões para a página, mas você ainda precisa adicionar uma pergunta de modelo.</span><span class="sxs-lookup"><span data-stu-id="13fc6-199">You have added the buttons to the page, but you still need to add a template question.</span></span> <span data-ttu-id="13fc6-200">Para fazer isso, você usará um novo recurso na chamada do Web Essentials **Lorem Ipsum gerador**.</span><span class="sxs-lookup"><span data-stu-id="13fc6-200">To do so, you will use a new feature in Web Essentials called **Lorem Ipsum generator**.</span></span> <span data-ttu-id="13fc6-201">Localize o **div** elemento com o **classe** atributo **front**.</span><span class="sxs-lookup"><span data-stu-id="13fc6-201">Locate the **div** element with the **class** attribute **front**.</span></span>
8. <span data-ttu-id="13fc6-202">Adicione o seguinte código como o primeiro elemento filho do **div**e pressione **guia**.</span><span class="sxs-lookup"><span data-stu-id="13fc6-202">Add the following code as the first child element of the **div**, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample2.css)]
9. <span data-ttu-id="13fc6-203">O código deve ser expandido para HTML.</span><span class="sxs-lookup"><span data-stu-id="13fc6-203">The code should be expanded to HTML.</span></span>

    <span data-ttu-id="13fc6-204">![Gerado automaticamente do Lorem Ipsum](visual-studio-2013-web-tools/_static/image10.png "gerado automaticamente do Lorem Ipsum")</span><span class="sxs-lookup"><span data-stu-id="13fc6-204">![Lorem Ipsum autogenerated](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum autogenerated")</span></span>

    <span data-ttu-id="13fc6-205">*Gerado automaticamente do Lorem Ipsum*</span><span class="sxs-lookup"><span data-stu-id="13fc6-205">*Lorem Ipsum autogenerated*</span></span>

    > [!NOTE]
    > <span data-ttu-id="13fc6-206">Como parte da codificação Zen, agora você pode gerar o código do Lorem Ipsum diretamente no editor de HTML.</span><span class="sxs-lookup"><span data-stu-id="13fc6-206">As part of Zen Coding, you can now generate Lorem Ipsum code directly in the HTML editor.</span></span> <span data-ttu-id="13fc6-207">Basta digitar **lorem** e clique em **guia** e um 30 word Lorem Ipsum o texto será inserido.</span><span class="sxs-lookup"><span data-stu-id="13fc6-207">Simply type **lorem** and hit **TAB** and a 30 word Lorem Ipsum text will be inserted.</span></span> <span data-ttu-id="13fc6-208">Por exemplo,</span><span class="sxs-lookup"><span data-stu-id="13fc6-208">E.g.</span></span> <span data-ttu-id="13fc6-209">*lorem10* insere 10 palavras Lorem Ipsum.</span><span class="sxs-lookup"><span data-stu-id="13fc6-209">*lorem10* inserts 10 Lorem Ipsum words.</span></span>
10. <span data-ttu-id="13fc6-210">Você irá adicionar um logotipo na parte superior da pergunta usando outro novo recurso na chamada do Web Essentials **gerador de Lorem Pixel**.</span><span class="sxs-lookup"><span data-stu-id="13fc6-210">You will add a logo at the top of the question by using another new feature in Web Essentials called **Lorem Pixel generator**.</span></span> <span data-ttu-id="13fc6-211">Adicione o seguinte código como o primeiro elemento filho do **div** elemento com **contêiner** como **classe** valor e pressione **guia**.</span><span class="sxs-lookup"><span data-stu-id="13fc6-211">Add the following code as the first child element of the **div** element with **container** as **class** value, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample3.css)]
11. <span data-ttu-id="13fc6-212">O código deve se expandir para HTML.</span><span class="sxs-lookup"><span data-stu-id="13fc6-212">The code should expand to HTML.</span></span>

    <span data-ttu-id="13fc6-213">![Gerado automaticamente de Lorem Pixel](visual-studio-2013-web-tools/_static/image11.png "gerado automaticamente de Lorem Pixel")</span><span class="sxs-lookup"><span data-stu-id="13fc6-213">![Lorem Pixel autogenerated](visual-studio-2013-web-tools/_static/image11.png "Lorem Pixel autogenerated")</span></span>

    <span data-ttu-id="13fc6-214">*Gerado automaticamente de Lorem Pixel*</span><span class="sxs-lookup"><span data-stu-id="13fc6-214">*Lorem Pixel autogenerated*</span></span>

    > [!NOTE]
    > <span data-ttu-id="13fc6-215">Como parte do Zen de codificação, você também pode gerar o código de Lorem Pixel diretamente no editor de HTML.</span><span class="sxs-lookup"><span data-stu-id="13fc6-215">As part of Zen Coding, you can also generate Lorem Pixel code directly in the HTML editor.</span></span> <span data-ttu-id="13fc6-216">Basta digitar **pix de 200 x 200 de animais** e clique em **guia** e um **img** marca com uma imagem de 200 x 200 de um animal será inserida.</span><span class="sxs-lookup"><span data-stu-id="13fc6-216">Simply type **pix-200x200-animals** and hit **TAB** and an **img** tag with a 200x200 image of an animal will be inserted.</span></span> <span data-ttu-id="13fc6-217">Para obter mais informações, consulte [Lorem Pixel](http://www.lorempixel.com).</span><span class="sxs-lookup"><span data-stu-id="13fc6-217">For more information, refer to [Lorem Pixel](http://www.lorempixel.com).</span></span>
12. <span data-ttu-id="13fc6-218">Clique o **atualizar navegadores vinculados** botão atualizar ambos os navegadores.</span><span class="sxs-lookup"><span data-stu-id="13fc6-218">Click the **Refresh linked browsers** button to update both browsers.</span></span>

    <span data-ttu-id="13fc6-219">![Internet Explorer - imagem gerada automaticamente e o texto](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer - imagem gerada automaticamente e texto")</span><span class="sxs-lookup"><span data-stu-id="13fc6-219">![Internet Explorer - Autogenerated image and text](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer - Autogenerated image and text")</span></span>

    <span data-ttu-id="13fc6-220">*Internet Explorer - imagem gerada automaticamente e texto*</span><span class="sxs-lookup"><span data-stu-id="13fc6-220">*Internet Explorer - Autogenerated image and text*</span></span>

    <span data-ttu-id="13fc6-221">![Google Chrome - imagem gerada automaticamente e o texto](visual-studio-2013-web-tools/_static/image13.png "Google Chrome - imagem gerada automaticamente e texto")</span><span class="sxs-lookup"><span data-stu-id="13fc6-221">![Google Chrome - Autogenerated image and text](visual-studio-2013-web-tools/_static/image13.png "Google Chrome - Autogenerated image and text")</span></span>

    <span data-ttu-id="13fc6-222">*Google Chrome - imagem gerada automaticamente e texto*</span><span class="sxs-lookup"><span data-stu-id="13fc6-222">*Google Chrome - Autogenerated image and text*</span></span>

    > [!NOTE]
    > <span data-ttu-id="13fc6-223">Como a imagem é selecionada aleatoriamente ao adicionar o trecho de código, a imagem mostrada nos navegadores pode diferir.</span><span class="sxs-lookup"><span data-stu-id="13fc6-223">Because the image is selected randomly when adding the code snippet, the image shown in the browsers may differ.</span></span>
13. <span data-ttu-id="13fc6-224">Não feche os navegadores.</span><span class="sxs-lookup"><span data-stu-id="13fc6-224">Do not close the browsers.</span></span> <span data-ttu-id="13fc6-225">Você irá usá-los na próxima tarefa.</span><span class="sxs-lookup"><span data-stu-id="13fc6-225">You will use them in the next task.</span></span>

<a id="Ex1Task3"></a>
#### <a name="task-3---updating-a-style-property"></a><span data-ttu-id="13fc6-226">Tarefa 3: atualizar uma propriedade de estilo</span><span class="sxs-lookup"><span data-stu-id="13fc6-226">Task 3 - Updating a Style Property</span></span>

<span data-ttu-id="13fc6-227">Nesta tarefa, você usará o Link do navegador **inspecionar modo** recurso para detectar o local exato em que o elemento DOM específico é gerado e, em seguida, atualize a propriedade de cor do elemento usando um seletor de cor fornecido pela Web Conceitos básicos.</span><span class="sxs-lookup"><span data-stu-id="13fc6-227">In this task, you will use the Browser Link's **Inspect Mode** feature to detect the exact location where the specific DOM element is generated and then update the color property of that element using a color picker provided by Web Essentials.</span></span>

1. <span data-ttu-id="13fc6-228">No navegador Internet Explorer, pressione **CTRL** + **ALT** + **eu** para habilitar o modo de inspecionar.</span><span class="sxs-lookup"><span data-stu-id="13fc6-228">In the Internet Explorer browser, press **CTRL** + **ALT** + **I** to enable Inspect Mode.</span></span>
2. <span data-ttu-id="13fc6-229">Mova o ponteiro sobre a borda azul claro e clique em.</span><span class="sxs-lookup"><span data-stu-id="13fc6-229">Move the pointer over the light blue border and click.</span></span>

    <span data-ttu-id="13fc6-230">![Mover o ponteiro sobre a borda azul claro](visual-studio-2013-web-tools/_static/image14.png "movendo o ponteiro sobre a borda azul claro")</span><span class="sxs-lookup"><span data-stu-id="13fc6-230">![Moving the pointer over the light blue border](visual-studio-2013-web-tools/_static/image14.png "Moving the pointer over the light blue border")</span></span>

    <span data-ttu-id="13fc6-231">*Mover o ponteiro sobre a borda azul claro*</span><span class="sxs-lookup"><span data-stu-id="13fc6-231">*Moving the pointer over the light blue border*</span></span>
3. <span data-ttu-id="13fc6-232">Alterne para Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="13fc6-232">Switch back to Visual Studio.</span></span> <span data-ttu-id="13fc6-233">Observe como o elemento HTML que você selecionou no navegador também está selecionado no editor de HTML do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="13fc6-233">Notice how the HTML element that you selected in the browser is also selected in the Visual Studio HTML editor.</span></span>

    <span data-ttu-id="13fc6-234">![Elemento HTML selecionado no editor de HTML do Visual Studio](visual-studio-2013-web-tools/_static/image15.png "elemento HTML selecionado no editor de HTML do Visual Studio")</span><span class="sxs-lookup"><span data-stu-id="13fc6-234">![HTML element selected in the Visual Studio HTML editor](visual-studio-2013-web-tools/_static/image15.png "HTML element selected in the Visual Studio HTML editor")</span></span>

    <span data-ttu-id="13fc6-235">*Elemento HTML selecionado no editor de HTML do Visual Studio*</span><span class="sxs-lookup"><span data-stu-id="13fc6-235">*HTML element selected in the Visual Studio HTML editor*</span></span>
4. <span data-ttu-id="13fc6-236">Agora você irá atualizar o **front** classe CSS para alterar o estilo do elemento selecionado.</span><span class="sxs-lookup"><span data-stu-id="13fc6-236">You will now update the **front** CSS class in order to change the styling of the selected element.</span></span> <span data-ttu-id="13fc6-237">Para fazer isso, pressione **CTRL** + **,** para abrir o **navegar até** caixa de pesquisa.</span><span class="sxs-lookup"><span data-stu-id="13fc6-237">To do so, press **CTRL** + **,** to open the **Navigate To** search box.</span></span> <span data-ttu-id="13fc6-238">Tipo de **CSS** e pressione **ENTER** para abrir o arquivo.</span><span class="sxs-lookup"><span data-stu-id="13fc6-238">Type **site.css** and press **ENTER** to open the file.</span></span>

    <span data-ttu-id="13fc6-239">![Abrir o arquivo CSS](visual-studio-2013-web-tools/_static/image16.png "abrir o arquivo CSS")</span><span class="sxs-lookup"><span data-stu-id="13fc6-239">![Opening file Site.css](visual-studio-2013-web-tools/_static/image16.png "Opening file Site.css")</span></span>

    <span data-ttu-id="13fc6-240">*Abrir o arquivo CSS*</span><span class="sxs-lookup"><span data-stu-id="13fc6-240">*Opening file Site.css*</span></span>
5. <span data-ttu-id="13fc6-241">Pressione **CTRL** + **F** e digite **.front .flip contêiner** para localizar o seletor de CSS.</span><span class="sxs-lookup"><span data-stu-id="13fc6-241">Press **CTRL** + **F** and type **.flip-containter .front** to find the CSS selector.</span></span>
6. <span data-ttu-id="13fc6-242">Clique no quadrado azul claro na propriedade da classe para abrir o seletor de cores da borda.</span><span class="sxs-lookup"><span data-stu-id="13fc6-242">Click the light blue square in the border property of the class to open the Color Picker.</span></span>

    <span data-ttu-id="13fc6-243">![Abrir o seletor de cores](visual-studio-2013-web-tools/_static/image17.png "abrindo o seletor de cores")</span><span class="sxs-lookup"><span data-stu-id="13fc6-243">![Opening the Color Picker](visual-studio-2013-web-tools/_static/image17.png "Opening the Color Picker")</span></span>

    <span data-ttu-id="13fc6-244">*Abrir o seletor de cores*</span><span class="sxs-lookup"><span data-stu-id="13fc6-244">*Opening the Color Picker*</span></span>
7. <span data-ttu-id="13fc6-245">Expanda o seletor de cores, clique no botão de divisa e selecione uma nova cor.</span><span class="sxs-lookup"><span data-stu-id="13fc6-245">Expand the Color Picker by clicking the chevron button and select a new color.</span></span>

    <span data-ttu-id="13fc6-246">![Expandindo o seletor de cores](visual-studio-2013-web-tools/_static/image18.png "expandindo o seletor de cores")</span><span class="sxs-lookup"><span data-stu-id="13fc6-246">![Expanding the Color Picker](visual-studio-2013-web-tools/_static/image18.png "Expanding the Color Picker")</span></span>

    <span data-ttu-id="13fc6-247">*Expandindo o seletor de cores*</span><span class="sxs-lookup"><span data-stu-id="13fc6-247">*Expanding the Color Picker*</span></span>
8. <span data-ttu-id="13fc6-248">Pressione **CTRL** + **ALT** + **ENTER** atualizar navegadores vinculados.</span><span class="sxs-lookup"><span data-stu-id="13fc6-248">Press **CTRL** + **ALT** + **ENTER** to refresh linked browsers.</span></span>
9. <span data-ttu-id="13fc6-249">Alterne para o Internet Explorer e observe como a cor da borda foi alterada.</span><span class="sxs-lookup"><span data-stu-id="13fc6-249">Switch to Internet Explorer and notice how the color of the border has changed.</span></span>

    <span data-ttu-id="13fc6-250">![Internet Explorer – a cor da borda atualizada](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer – a cor da borda atualizado")</span><span class="sxs-lookup"><span data-stu-id="13fc6-250">![Internet Explorer - Border color updated](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer - Border color updated")</span></span>

    <span data-ttu-id="13fc6-251">*Internet Explorer – a cor da borda atualizado*</span><span class="sxs-lookup"><span data-stu-id="13fc6-251">*Internet Explorer - Border color updated*</span></span>
10. <span data-ttu-id="13fc6-252">Alterne para o Google Chrome e observe como a cor da borda foi alterada.</span><span class="sxs-lookup"><span data-stu-id="13fc6-252">Switch to Google Chrome and notice how the color of the border has changed.</span></span>

    <span data-ttu-id="13fc6-253">![Google Chrome - atualizado de cor de borda](visual-studio-2013-web-tools/_static/image20.png "Google Chrome - atualizado de cor de borda")</span><span class="sxs-lookup"><span data-stu-id="13fc6-253">![Google Chrome - Border color updated](visual-studio-2013-web-tools/_static/image20.png "Google Chrome - Border color updated")</span></span>

    <span data-ttu-id="13fc6-254">*Google Chrome - atualizado de cor de borda*</span><span class="sxs-lookup"><span data-stu-id="13fc6-254">*Google Chrome - Border color updated*</span></span>
11. <span data-ttu-id="13fc6-255">Alterne para Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="13fc6-255">Switch back to Visual Studio.</span></span>
12. <span data-ttu-id="13fc6-256">Vá até o final do **CSS** arquivo e pressione **CTRL** + **F** para localizar o **.btn** seletor.</span><span class="sxs-lookup"><span data-stu-id="13fc6-256">Go to the end of the **Site.css** file and press **CTRL** + **F** to locate the **.btn** selector.</span></span>
13. <span data-ttu-id="13fc6-257">Observe que o **- webkit-border-radius** propriedade está sublinhada em verde.</span><span class="sxs-lookup"><span data-stu-id="13fc6-257">Notice that the **-webkit-border-radius** property is underlined in green.</span></span>

    <span data-ttu-id="13fc6-258">![propriedade - webkit-border-radius do seletor de btn](visual-studio-2013-web-tools/_static/image21.png "propriedade - webkit-border-radius do seletor de btn")</span><span class="sxs-lookup"><span data-stu-id="13fc6-258">![-webkit-border-radius property of the btn selector](visual-studio-2013-web-tools/_static/image21.png "-webkit-border-radius property of the btn selector")</span></span>

    <span data-ttu-id="13fc6-259">*propriedade - webkit-border-radius do seletor de btn*</span><span class="sxs-lookup"><span data-stu-id="13fc6-259">*-webkit-border-radius property of the btn selector*</span></span>
14. <span data-ttu-id="13fc6-260">Coloque o cursor na **- webkit-border-radius** propriedade.</span><span class="sxs-lookup"><span data-stu-id="13fc6-260">Place the caret in the **-webkit-border-radius** property.</span></span> <span data-ttu-id="13fc6-261">Uma linha azul deve aparecer sob a primeira letra da primeira palavra da propriedade.</span><span class="sxs-lookup"><span data-stu-id="13fc6-261">A blue line should appear under the first letter of the first word of the property.</span></span> <span data-ttu-id="13fc6-262">Esse é o **marca inteligente**.</span><span class="sxs-lookup"><span data-stu-id="13fc6-262">This is the **smart tag**.</span></span>
15. <span data-ttu-id="13fc6-263">Pressione **CTRL** + **.**</span><span class="sxs-lookup"><span data-stu-id="13fc6-263">Press **CTRL** + **.**</span></span> <span data-ttu-id="13fc6-264">Para abrir o menu de sugestões e clique em **adicionar a propriedade padrão (raio de borda) ausente**.</span><span class="sxs-lookup"><span data-stu-id="13fc6-264">to open the suggestions menu and click **Add missing standard property (border-radius)**.</span></span>

    <span data-ttu-id="13fc6-265">![Adicionar ausente sugestão de propriedade padrão](visual-studio-2013-web-tools/_static/image22.png "adicionar ausente sugestão de propriedade padrão")</span><span class="sxs-lookup"><span data-stu-id="13fc6-265">![Add missing standard property suggestion](visual-studio-2013-web-tools/_static/image22.png "Add missing standard property suggestion")</span></span>

    <span data-ttu-id="13fc6-266">*Adicionar ausentes sugestão de propriedade padrão*</span><span class="sxs-lookup"><span data-stu-id="13fc6-266">*Add missing standard property suggestion*</span></span>
16. <span data-ttu-id="13fc6-267">O **raio de borda** propriedade é adicionada automaticamente à regra de CSS.</span><span class="sxs-lookup"><span data-stu-id="13fc6-267">The **border-radius** property is automatically added to the CSS rule.</span></span>

    <span data-ttu-id="13fc6-268">![Adicionada a propriedade padrão ausente](visual-studio-2013-web-tools/_static/image23.png "adicionada a propriedade padrão ausente")</span><span class="sxs-lookup"><span data-stu-id="13fc6-268">![Missing standard property added](visual-studio-2013-web-tools/_static/image23.png "Missing standard property added")</span></span>

    <span data-ttu-id="13fc6-269">*Adicionada a propriedade padrão ausente*</span><span class="sxs-lookup"><span data-stu-id="13fc6-269">*Missing standard property added*</span></span>
17. <span data-ttu-id="13fc6-270">Mova o ponteiro sobre o **raio de borda** propriedade para exibir o **dica de ferramenta do navegador matriz**.</span><span class="sxs-lookup"><span data-stu-id="13fc6-270">Move the pointer over the **border-radius** property to display the **Browser matrix tooltip**.</span></span> <span data-ttu-id="13fc6-271">O **dica de ferramenta do navegador matriz** mostra a disponibilidade da propriedade em cada navegador.</span><span class="sxs-lookup"><span data-stu-id="13fc6-271">The **Browser matrix tooltip** shows the availability of the property in each browser.</span></span>

    <span data-ttu-id="13fc6-272">![Dica de ferramenta do navegador matrix](visual-studio-2013-web-tools/_static/image24.png "dica de ferramenta de matriz de navegador")</span><span class="sxs-lookup"><span data-stu-id="13fc6-272">![Browser matrix tooltip](visual-studio-2013-web-tools/_static/image24.png "Browser matrix tooltip")</span></span>

    <span data-ttu-id="13fc6-273">*Dica de ferramenta de matriz de navegador*</span><span class="sxs-lookup"><span data-stu-id="13fc6-273">*Browser matrix tooltip*</span></span>
18. <span data-ttu-id="13fc6-274">Observe que o valor de **raio de borda** propriedade está sublinhada ainda.</span><span class="sxs-lookup"><span data-stu-id="13fc6-274">Notice that the value of the **border-radius** property is still underlined.</span></span> <span data-ttu-id="13fc6-275">Mova o ponteiro sobre o valor para ver a mensagem de aviso.</span><span class="sxs-lookup"><span data-stu-id="13fc6-275">Move the pointer over the value to see the warning message.</span></span>

    <span data-ttu-id="13fc6-276">![Aviso de valor de propriedade de raio de borda](visual-studio-2013-web-tools/_static/image25.png "aviso de valor de propriedade de raio de borda")</span><span class="sxs-lookup"><span data-stu-id="13fc6-276">![Border-radius property value warning](visual-studio-2013-web-tools/_static/image25.png "Border-radius property value warning")</span></span>

    <span data-ttu-id="13fc6-277">*Aviso de valor de propriedade de raio de borda*</span><span class="sxs-lookup"><span data-stu-id="13fc6-277">*Border-radius property value warning*</span></span>
19. <span data-ttu-id="13fc6-278">Remova a unidade do **raio de borda** valor da propriedade conforme sugerido pela dica de ferramenta.</span><span class="sxs-lookup"><span data-stu-id="13fc6-278">Remove the unit of the **border-radius** property value as suggested by the tooltip.</span></span>
20. <span data-ttu-id="13fc6-279">Como **raio de borda** é a propriedade padrão para a definição de borda como arredondada cantos são, você pode remover o **- webkit-border-radius** propriedade e o valor da regra de CSS.</span><span class="sxs-lookup"><span data-stu-id="13fc6-279">As **border-radius** is the standard property for defining how rounded border corners are, you can remove the **-webkit-border-radius** property and value from the CSS rule.</span></span>
21. <span data-ttu-id="13fc6-280">Coloque o cursor na **quebra** propriedade e observe que a marca inteligente também é exibido abaixo.</span><span class="sxs-lookup"><span data-stu-id="13fc6-280">Place the caret in the **word-wrap** property and notice that the smart tag also appears below.</span></span>
22. <span data-ttu-id="13fc6-281">Abra o menu e clique em **adicionar informações específicas de fornecedor ausente**.</span><span class="sxs-lookup"><span data-stu-id="13fc6-281">Open the menu and click **Add missing vendor specifics**.</span></span>

    <span data-ttu-id="13fc6-282">![Adicione ausente sugestão de informações específicas de fornecedor](visual-studio-2013-web-tools/_static/image26.png "adicione ausente sugestão de informações específicas de fornecedor")</span><span class="sxs-lookup"><span data-stu-id="13fc6-282">![Add missing vendor specifics suggestion](visual-studio-2013-web-tools/_static/image26.png "Add missing vendor specifics suggestion")</span></span>

    <span data-ttu-id="13fc6-283">*Adicione ausente sugestão de informações específicas de fornecedor*</span><span class="sxs-lookup"><span data-stu-id="13fc6-283">*Add missing vendor specifics suggestion*</span></span>
23. <span data-ttu-id="13fc6-284">O **ms quebra** propriedade é adicionada automaticamente à regra de CSS.</span><span class="sxs-lookup"><span data-stu-id="13fc6-284">The **-ms-word-wrap** property is automatically added to the CSS rule.</span></span>

    <span data-ttu-id="13fc6-285">![Propriedade específica do fornecedor adicionada](visual-studio-2013-web-tools/_static/image27.png "adicionada de propriedade específica do fornecedor")</span><span class="sxs-lookup"><span data-stu-id="13fc6-285">![Vendor specific property added](visual-studio-2013-web-tools/_static/image27.png "Vendor specific property added")</span></span>

    <span data-ttu-id="13fc6-286">*Propriedade específica do fornecedor adicionada*</span><span class="sxs-lookup"><span data-stu-id="13fc6-286">*Vendor specific property added*</span></span>

<a id="Ex1Task4"></a>
#### <a name="task-4---updating-the-html-code-from-the-browser"></a><span data-ttu-id="13fc6-287">Tarefa 4 - atualizando o código HTML no navegador</span><span class="sxs-lookup"><span data-stu-id="13fc6-287">Task 4 - Updating the HTML Code from the Browser</span></span>

<span data-ttu-id="13fc6-288">Nesta tarefa, você usará o Link do navegador **modo de Design** recurso para editar o objeto do DOM do navegador e transferir as alterações para o arquivo de código-fonte HTML no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="13fc6-288">In this task, you will use the Browser Link's **Design Mode** feature to edit the DOM object from the browser and transfer the changes to the HTML source file in Visual Studio.</span></span>

1. <span data-ttu-id="13fc6-289">No Google Chrome, pressione **CTRL** + **ALT** + **1!d** para habilitar o modo de Design.</span><span class="sxs-lookup"><span data-stu-id="13fc6-289">In Google Chrome, press **CTRL** + **ALT** + **D** to enable Design Mode.</span></span>
2. <span data-ttu-id="13fc6-290">Mova o ponteiro sobre o **Lorem Ipsum dolor sit amet** de rótulo e clique em.</span><span class="sxs-lookup"><span data-stu-id="13fc6-290">Move the pointer over the **Lorem Ipsum dolor sit amet** label and click.</span></span>

    <span data-ttu-id="13fc6-291">![Editando a pergunta](visual-studio-2013-web-tools/_static/image28.png "editando a pergunta")</span><span class="sxs-lookup"><span data-stu-id="13fc6-291">![Editing the question](visual-studio-2013-web-tools/_static/image28.png "Editing the question")</span></span>

    <span data-ttu-id="13fc6-292">*Editando a pergunta*</span><span class="sxs-lookup"><span data-stu-id="13fc6-292">*Editing the question*</span></span>
3. <span data-ttu-id="13fc6-293">Um cursor deve aparecer.</span><span class="sxs-lookup"><span data-stu-id="13fc6-293">A cursor should appear.</span></span> <span data-ttu-id="13fc6-294">Substitua o texto original com *o que ele se parece, como quando escrevo uma pergunta mais longa?* e, em seguida, pressione **ESC** para sair do modo de Design.</span><span class="sxs-lookup"><span data-stu-id="13fc6-294">Replace the original text with *What does it look like when I write a longer question?*, and then press **ESC** to exit Design Mode.</span></span>

    <span data-ttu-id="13fc6-295">![Pergunta editada](visual-studio-2013-web-tools/_static/image29.png "pergunta editada")</span><span class="sxs-lookup"><span data-stu-id="13fc6-295">![Question edited](visual-studio-2013-web-tools/_static/image29.png "Question edited")</span></span>

    <span data-ttu-id="13fc6-296">*Pergunta editada*</span><span class="sxs-lookup"><span data-stu-id="13fc6-296">*Question edited*</span></span>
4. <span data-ttu-id="13fc6-297">Alternar para o Visual Studio e abra **index. cshtml**, se ainda não estiver aberto.</span><span class="sxs-lookup"><span data-stu-id="13fc6-297">Switch back to Visual Studio and open **Index.cshtml**, if not already opened.</span></span> <span data-ttu-id="13fc6-298">Observe que o texto interno do **&lt;p&gt;** elemento foi atualizado.</span><span class="sxs-lookup"><span data-stu-id="13fc6-298">Notice that the inner text of the **&lt;p&gt;** element has been updated.</span></span>

    <span data-ttu-id="13fc6-299">![Pergunta atualizadas na página HTML](visual-studio-2013-web-tools/_static/image30.png "pergunta atualizadas na página HTML")</span><span class="sxs-lookup"><span data-stu-id="13fc6-299">![Updated question in the HTML page](visual-studio-2013-web-tools/_static/image30.png "Updated question in the HTML page")</span></span>

    <span data-ttu-id="13fc6-300">*Pergunta atualizada na página HTML*</span><span class="sxs-lookup"><span data-stu-id="13fc6-300">*Updated question in the HTML page*</span></span>

<a id="Ex1Task5"></a>
#### <a name="task-5---reviewing-seo-related-warnings"></a><span data-ttu-id="13fc6-301">Tarefa 5 – revisão SEO relacionados avisos</span><span class="sxs-lookup"><span data-stu-id="13fc6-301">Task 5 - Reviewing SEO Related Warnings</span></span>

<span data-ttu-id="13fc6-302">**Otimização do mecanismo de pesquisa** (SEO) é o processo de criação de uma classificação de site mais alto na lista do mecanismo de pesquisa de resultados.</span><span class="sxs-lookup"><span data-stu-id="13fc6-302">**Search Engine Optimization** (SEO) is the process of making a website rank higher on a search engine's list of results.</span></span> <span data-ttu-id="13fc6-303">Quanto maior o site classifica e mais consistentemente estiver listado, os visitantes mais o site obterá-se de que o mecanismo de pesquisa.</span><span class="sxs-lookup"><span data-stu-id="13fc6-303">The higher the site ranks and the more consistently it is listed, the more visitors the site will get from that search engine.</span></span> <span data-ttu-id="13fc6-304">Web Essentials incorpora uma ferramenta analítica que examina o HTML, relatórios de problemas encontrados e fornece assistência para corrigi-los.</span><span class="sxs-lookup"><span data-stu-id="13fc6-304">Web Essentials incorporates an analytical tool that examines HTML, reports the issues found and provides assistance to fix them.</span></span>

1. <span data-ttu-id="13fc6-305">Vá para o **modo de exibição** menu e clique em **lista de erros** para abrir o **lista de erros** janela.</span><span class="sxs-lookup"><span data-stu-id="13fc6-305">Go to the **View** menu and click **Error List** to open the **Error List** window.</span></span>

    <span data-ttu-id="13fc6-306">![Menu de modo de exibição de lista de erros](visual-studio-2013-web-tools/_static/image31.png "lista de erros no menu Exibir")</span><span class="sxs-lookup"><span data-stu-id="13fc6-306">![Error List in View menu](visual-studio-2013-web-tools/_static/image31.png "Error List in View menu")</span></span>

    <span data-ttu-id="13fc6-307">*Menu de modo de exibição de lista de erros*</span><span class="sxs-lookup"><span data-stu-id="13fc6-307">*Error List in View menu*</span></span>
2. <span data-ttu-id="13fc6-308">Observe que há um aviso de SEO notificando que uma **&lt;meta&gt;** de marca para a descrição da página está ausente.</span><span class="sxs-lookup"><span data-stu-id="13fc6-308">Notice that there is an SEO warning notifying that a **&lt;meta&gt;** tag for the page description is missing.</span></span> <span data-ttu-id="13fc6-309">Clique duas vezes na entrada de aviso de SEO para corrigi-lo.</span><span class="sxs-lookup"><span data-stu-id="13fc6-309">Double-click the SEO warning entry to fix it.</span></span>

    <span data-ttu-id="13fc6-310">![Janela lista de erros](visual-studio-2013-web-tools/_static/image32.png "janela lista de erros")</span><span class="sxs-lookup"><span data-stu-id="13fc6-310">![Error List window](visual-studio-2013-web-tools/_static/image32.png "Error List window")</span></span>

    <span data-ttu-id="13fc6-311">*Janela lista de erros*</span><span class="sxs-lookup"><span data-stu-id="13fc6-311">*Error List window*</span></span>
3. <span data-ttu-id="13fc6-312">No **Web Essentials** caixa de diálogo, clique em **Yes** para inserir uma descrição &lt;meta&gt; marca.</span><span class="sxs-lookup"><span data-stu-id="13fc6-312">In the **Web Essentials** dialog box, click **Yes** to insert a description &lt;meta&gt; tag.</span></span>

    <span data-ttu-id="13fc6-313">![Caixa de diálogo do Web Essentials](visual-studio-2013-web-tools/_static/image33.png "caixa de diálogo do Web Essentials")</span><span class="sxs-lookup"><span data-stu-id="13fc6-313">![Web Essentials dialog box](visual-studio-2013-web-tools/_static/image33.png "Web Essentials dialog box")</span></span>

    <span data-ttu-id="13fc6-314">*Caixa de diálogo do Web Essentials*</span><span class="sxs-lookup"><span data-stu-id="13fc6-314">*Web Essentials dialog box*</span></span>
4. <span data-ttu-id="13fc6-315">O editor para  **\_layout. cshtml** abre e a **&lt;meta&gt;** marca é adicionada automaticamente para o **head** seção o Arquivo HTML.</span><span class="sxs-lookup"><span data-stu-id="13fc6-315">The editor for **\_Layout.cshtml** opens and the **&lt;meta&gt;** tag is automatically added to the **head** section of the HTML file.</span></span>

    <span data-ttu-id="13fc6-316">![Marca meta adicionado automaticamente à página layout](visual-studio-2013-web-tools/_static/image34.png "MetaTag adicionado automaticamente à página layout")</span><span class="sxs-lookup"><span data-stu-id="13fc6-316">![Meta tag automatically added in _Layout page](visual-studio-2013-web-tools/_static/image34.png "Meta tag automatically added in _Layout page")</span></span>

    <span data-ttu-id="13fc6-317">*Marca meta adicionada automaticamente ao \_página de Layout*</span><span class="sxs-lookup"><span data-stu-id="13fc6-317">*Meta tag automatically added to \_Layout page*</span></span>
5. <span data-ttu-id="13fc6-318">Altere o valor da **conteúdo** atributo *GeekQuiz* e salve o arquivo.</span><span class="sxs-lookup"><span data-stu-id="13fc6-318">Change the value of the **content** attribute to *GeekQuiz* and save the file.</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-taking-advantage-of-code-snippets-and-intellisense"></a><span data-ttu-id="13fc6-319">Exercício 2: Aproveitando os trechos de código e IntelliSense</span><span class="sxs-lookup"><span data-stu-id="13fc6-319">Exercise 2: Taking Advantage of Code Snippets and IntelliSense</span></span>

<span data-ttu-id="13fc6-320">Com o Web Essentials, o editor de HTML foi estendido com funcionalidade adicional.</span><span class="sxs-lookup"><span data-stu-id="13fc6-320">With Web Essentials, the HTML editor has been extended with extra functionality.</span></span> <span data-ttu-id="13fc6-321">Neste exercício, você verá alguns novos recursos que são úteis ao desenvolver aplicativos da web.</span><span class="sxs-lookup"><span data-stu-id="13fc6-321">In this exercise, you will see some new features that are helpful when developing web applications.</span></span>

<a id="Ex2Task1"></a>
#### <a name="task-1---using-intellisense-in-html-documents"></a><span data-ttu-id="13fc6-322">Tarefa 1: usando o IntelliSense em documentos HTML</span><span class="sxs-lookup"><span data-stu-id="13fc6-322">Task 1 - Using IntelliSense in HTML Documents</span></span>

<span data-ttu-id="13fc6-323">O primeiro recurso novo, você verá essa tarefa é chamado **IntelliSense dinâmico**.</span><span class="sxs-lookup"><span data-stu-id="13fc6-323">The first new feature you will see in this task is called **Dynamic IntelliSense**.</span></span> <span data-ttu-id="13fc6-324">IntelliSense dinâmico lê outras marcas e atributos para inferir as possíveis ids que você usará.</span><span class="sxs-lookup"><span data-stu-id="13fc6-324">Dynamic IntelliSense reads other tags and attributes to infer the possible ids you will use.</span></span>

<span data-ttu-id="13fc6-325">Nesta tarefa, você criará um novo elemento de formulário HTML que contém um rótulo e um campo de entrada.</span><span class="sxs-lookup"><span data-stu-id="13fc6-325">In this task, you will create a new HTML form element which contains a label and an input field.</span></span> <span data-ttu-id="13fc6-326">Em seguida, você adicionará uma **para** de atributo para o rótulo para associá-lo à entrada, e você verá sugestões do IntelliSense com base nas ids das entradas no escopo.</span><span class="sxs-lookup"><span data-stu-id="13fc6-326">Then you will add a **for** attribute to the label to bind it to the input, and you will see IntelliSense suggestions based on the ids of the inputs in scope.</span></span>

1. <span data-ttu-id="13fc6-327">Abra **Visual Studio Express 2013 para Web** e o **Begin.sln** solução localizada no **origem/o Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/início** pasta.</span><span class="sxs-lookup"><span data-stu-id="13fc6-327">Open **Visual Studio Express 2013 for Web** and the **Begin.sln** solution located in the **Source/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/Begin** folder.</span></span> <span data-ttu-id="13fc6-328">Como alternativa, você pode continuar com a solução que você obteve no exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="13fc6-328">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>
2. <span data-ttu-id="13fc6-329">Na **Gerenciador de soluções**, abra o **index. cshtml** arquivo localizado no **modos de exibição** | **início** pasta.</span><span class="sxs-lookup"><span data-stu-id="13fc6-329">In **Solution Explorer**, open the **Index.cshtml** file located in the **Views** | **Home** folder.</span></span>
3. <span data-ttu-id="13fc6-330">Adicione o seguinte formulário dentro do **&lt;seção&gt;** elemento.</span><span class="sxs-lookup"><span data-stu-id="13fc6-330">Add the following form inside the **&lt;section&gt;** element.</span></span>

    <span data-ttu-id="13fc6-331">(Código de trecho de código – *VisualStudio2013WebTooling* - *o Ex2* - *formulário*)</span><span class="sxs-lookup"><span data-stu-id="13fc6-331">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *Form*)</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample4.html)]
4. <span data-ttu-id="13fc6-332">A marca de entrada deve ser precedida por um rótulo com uma descrição do campo.</span><span class="sxs-lookup"><span data-stu-id="13fc6-332">The input tag should be preceded by a label with some description of the field.</span></span> <span data-ttu-id="13fc6-333">Adicione o seguinte rótulo antes da marca de entrada.</span><span class="sxs-lookup"><span data-stu-id="13fc6-333">Add the following label before the input tag.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample5.html)]
5. <span data-ttu-id="13fc6-334">O **para** atributo de uma **&lt;rótulo&gt;** Especifica qual elemento de formulário um rótulo está associado.</span><span class="sxs-lookup"><span data-stu-id="13fc6-334">The **for** attribute of a **&lt;label&gt;** specifies which form element a label is bound to.</span></span> <span data-ttu-id="13fc6-335">O valor do atributo deve ser igual à id do elemento relacionado.</span><span class="sxs-lookup"><span data-stu-id="13fc6-335">The attribute's value should be equal to the id of the related element.</span></span> <span data-ttu-id="13fc6-336">Adicione a **para** atributo para o **&lt;rótulo&gt;** elemento.</span><span class="sxs-lookup"><span data-stu-id="13fc6-336">Add the **for** attribute to the **&lt;label&gt;** element.</span></span> <span data-ttu-id="13fc6-337">Conforme mostrado na figura a seguir, o &quot;nome&quot; valor aparece na caixa de IntelliSense, com base na id dos elementos dentro do mesmo escopo (circunscrição  **&lt;formulário&gt;**).</span><span class="sxs-lookup"><span data-stu-id="13fc6-337">As shown in the following figure, the &quot;name&quot; value pops up in the IntelliSense box, based on the id of the elements within the same scope (the enclosing **&lt;form&gt;**).</span></span>

    <span data-ttu-id="13fc6-338">![Mostrando a id no IntelliSense](visual-studio-2013-web-tools/_static/image35.png "mostrando a id no IntelliSense")</span><span class="sxs-lookup"><span data-stu-id="13fc6-338">![Showing the id in IntelliSense](visual-studio-2013-web-tools/_static/image35.png "Showing the id in IntelliSense")</span></span>

    <span data-ttu-id="13fc6-339">*Mostrando a id no IntelliSense*</span><span class="sxs-lookup"><span data-stu-id="13fc6-339">*Showing the id in IntelliSense*</span></span>
6. <span data-ttu-id="13fc6-340">Excluir recentemente adicionados **&lt;formulário&gt;** elemento e seu conteúdo.</span><span class="sxs-lookup"><span data-stu-id="13fc6-340">Delete the recently added **&lt;form&gt;** element and its content.</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2---using-html-code-snippets"></a><span data-ttu-id="13fc6-341">Tarefa 2 - usando trechos de código HTML</span><span class="sxs-lookup"><span data-stu-id="13fc6-341">Task 2 - Using HTML Code Snippets</span></span>

<span data-ttu-id="13fc6-342">O HTML5 introduziu mais de 25 novas marcas semânticas.</span><span class="sxs-lookup"><span data-stu-id="13fc6-342">HTML5 introduced more than 25 new semantic tags.</span></span> <span data-ttu-id="13fc6-343">Visual Studio já tinha suporte do IntelliSense para essas marcas, mas o Visual Studio 2013 tornam mais rápidos e mais fáceis de escrever a marcação com a adição de novos trechos de código.</span><span class="sxs-lookup"><span data-stu-id="13fc6-343">Visual Studio already had IntelliSense support for these tags, but Visual Studio 2013 makes it faster and easier to write markup by adding new code snippets.</span></span> <span data-ttu-id="13fc6-344">Embora essas marcas não sejam complicadas, elas vêm com algumas pequenas sutilezas, como a adição de fallbacks de codec corretos para o *áudio* marca.</span><span class="sxs-lookup"><span data-stu-id="13fc6-344">Though these tags are not complicated, they come with a few small subtleties, such as adding the correct codec fallbacks for the *audio* tag.</span></span> <span data-ttu-id="13fc6-345">Nesta tarefa, você verá os trechos de código HTML para a marca de áudio.</span><span class="sxs-lookup"><span data-stu-id="13fc6-345">In this task, you will see the HTML code snippets for the audio tag.</span></span>

1. <span data-ttu-id="13fc6-346">No **index. cshtml** do arquivo, digite  **&lt;aud** dentro a **&lt;seção&gt;** elemento, conforme mostrado na figura a seguir.</span><span class="sxs-lookup"><span data-stu-id="13fc6-346">In the **Index.cshtml** file, type **&lt;aud** inside the **&lt;section&gt;** element as shown in the following figure.</span></span>

    <span data-ttu-id="13fc6-347">![Inserção de um elemento de áudio](visual-studio-2013-web-tools/_static/image36.png "inserção de um elemento de áudio")</span><span class="sxs-lookup"><span data-stu-id="13fc6-347">![Inserting an audio element](visual-studio-2013-web-tools/_static/image36.png "Inserting an audio element")</span></span>

    <span data-ttu-id="13fc6-348">*Inserção de um elemento de áudio*</span><span class="sxs-lookup"><span data-stu-id="13fc6-348">*Inserting an audio element*</span></span>
2. <span data-ttu-id="13fc6-349">Pressione **guia** duas vezes e observe como o código a seguir é adicionado na página e o cursor é colocado sobre o **src** atributo da primeira fonte.</span><span class="sxs-lookup"><span data-stu-id="13fc6-349">Press **TAB** twice and notice how the following code is added on the page and the cursor is placed on the **src** attribute of the first source.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample6.html)]

    > [!NOTE]
    > <span data-ttu-id="13fc6-350">Ao pressionar o **guia** chave duas vezes, o trecho de código é inserido.</span><span class="sxs-lookup"><span data-stu-id="13fc6-350">By pressing the **TAB** key twice, the code snippet is inserted.</span></span> <span data-ttu-id="13fc6-351">O trecho de áudio mostra o uso padrão do *áudio* marca, com dois arquivos de origem para o suporte aprimorado.</span><span class="sxs-lookup"><span data-stu-id="13fc6-351">The audio snippet shows the standard usage of the *audio* tag, with two source files for improved support.</span></span>
3. <span data-ttu-id="13fc6-352">Excluir a segunda linha e atualizar a fonte da primeira linha com o link a seguir para mostrar o WebCampsTV Katana: [ http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3 ](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3).</span><span class="sxs-lookup"><span data-stu-id="13fc6-352">Delete the second line and update the source of the first line with the following link to the WebCampsTV Katana show: [http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3).</span></span> <span data-ttu-id="13fc6-353">O código resultante é mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="13fc6-353">The resulting code is shown below.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample7.html)]

    > [!NOTE]
    > <span data-ttu-id="13fc6-354">O arquivo de origem *KatanaProject.mp3* é usado como um exemplo.</span><span class="sxs-lookup"><span data-stu-id="13fc6-354">The source file *KatanaProject.mp3* is used as an example.</span></span> <span data-ttu-id="13fc6-355">Se você preferir, você pode usar outra fonte.</span><span class="sxs-lookup"><span data-stu-id="13fc6-355">You can use another source if you prefer.</span></span>
4. <span data-ttu-id="13fc6-356">Pressione **CTRL** + **S** para salvar o arquivo.</span><span class="sxs-lookup"><span data-stu-id="13fc6-356">Press **CTRL** + **S** to save the file.</span></span>
5. <span data-ttu-id="13fc6-357">Pressione **CTRL** + **F5** para iniciar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="13fc6-357">Press **CTRL** + **F5** to start the application.</span></span>
6. <span data-ttu-id="13fc6-358">Observe que um player de áudio foi adicionado ao aplicativo.</span><span class="sxs-lookup"><span data-stu-id="13fc6-358">Notice that an audio player was added to the application.</span></span>

    <span data-ttu-id="13fc6-359">![Player de áudio no Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "player de áudio no Internet Explorer")</span><span class="sxs-lookup"><span data-stu-id="13fc6-359">![Audio player in Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "Audio player in Internet Explorer")</span></span>

    <span data-ttu-id="13fc6-360">*Player de áudio no Internet Explorer*</span><span class="sxs-lookup"><span data-stu-id="13fc6-360">*Audio player in Internet Explorer*</span></span>

    <span data-ttu-id="13fc6-361">![Player de áudio no Google Chrome](visual-studio-2013-web-tools/_static/image38.png "player de áudio no Google Chrome")</span><span class="sxs-lookup"><span data-stu-id="13fc6-361">![Audio player in Google Chrome](visual-studio-2013-web-tools/_static/image38.png "Audio player in Google Chrome")</span></span>

    <span data-ttu-id="13fc6-362">*Player de áudio no Google Chrome*</span><span class="sxs-lookup"><span data-stu-id="13fc6-362">*Audio player in Google Chrome*</span></span>
7. <span data-ttu-id="13fc6-363">Não feche os navegadores.</span><span class="sxs-lookup"><span data-stu-id="13fc6-363">Do not close the browsers.</span></span> <span data-ttu-id="13fc6-364">Você irá usá-los na próxima tarefa.</span><span class="sxs-lookup"><span data-stu-id="13fc6-364">You will use them in the next task.</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3---using-intellisense-in-javascript-documents"></a><span data-ttu-id="13fc6-365">Tarefa 3: usando o IntelliSense em documentos de JavaScript</span><span class="sxs-lookup"><span data-stu-id="13fc6-365">Task 3 - Using IntelliSense in JavaScript Documents</span></span>

<span data-ttu-id="13fc6-366">Com o Web Essentials 2013, folhas de estilo e páginas HTML produzem uma lista de IDs e nomes de classe.</span><span class="sxs-lookup"><span data-stu-id="13fc6-366">With Web Essentials 2013, style sheets and HTML pages produce a list of IDs and class names.</span></span> <span data-ttu-id="13fc6-367">Nesta tarefa, você aprenderá como essas listas de melhoram o suporte ao IntelliSense de JavaScript no Web Essentials 2013.</span><span class="sxs-lookup"><span data-stu-id="13fc6-367">In this task, you will learn how those lists improve JavaScript IntelliSense support in Web Essentials 2013.</span></span>

1. <span data-ttu-id="13fc6-368">No **index. cshtml** do arquivo, adicione o código a seguir para definir um **script** marca para o código JavaScript.</span><span class="sxs-lookup"><span data-stu-id="13fc6-368">In the **Index.cshtml** file, add the following code to define a **script** tag for JavaScript code.</span></span>

    [!code-cshtml[Main](visual-studio-2013-web-tools/samples/sample8.cshtml)]
2. <span data-ttu-id="13fc6-369">Adicione o seguinte código dentro de **script** marca para definir a função de retorno de chamada pronto.</span><span class="sxs-lookup"><span data-stu-id="13fc6-369">Add the following code inside the **script** tag to define the ready callback function.</span></span>

    <span data-ttu-id="13fc6-370">(Código de trecho de código – *VisualStudio2013WebTooling* - *o Ex2* - *ReadyFunction*)</span><span class="sxs-lookup"><span data-stu-id="13fc6-370">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample9.js)]
3. <span data-ttu-id="13fc6-371">Coloque o cursor na **script** marca e pressione **CTRL** + **.**</span><span class="sxs-lookup"><span data-stu-id="13fc6-371">Place the caret in the **script** tag and press **CTRL** + **.**</span></span> <span data-ttu-id="13fc6-372">Para abrir o menu de sugestão.</span><span class="sxs-lookup"><span data-stu-id="13fc6-372">to open the suggestion menu.</span></span>
4. <span data-ttu-id="13fc6-373">Clique em **extrair arquivo**.</span><span class="sxs-lookup"><span data-stu-id="13fc6-373">Click **Extract To File**.</span></span>

    <span data-ttu-id="13fc6-374">![Extraia o JavaScript para sugestão de arquivo](visual-studio-2013-web-tools/_static/image39.png "JavaScript extrair a sugestão de arquivo")</span><span class="sxs-lookup"><span data-stu-id="13fc6-374">![JavaScript extract to file suggestion](visual-studio-2013-web-tools/_static/image39.png "JavaScript extract to file suggestion")</span></span>

    <span data-ttu-id="13fc6-375">*Extraia o JavaScript para sugestão de arquivo*</span><span class="sxs-lookup"><span data-stu-id="13fc6-375">*JavaScript extract to file suggestion*</span></span>
5. <span data-ttu-id="13fc6-376">No **Salvar como** janela, selecione a **Scripts** pasta, nomeie o arquivo **init.js** e clique em **salvar**.</span><span class="sxs-lookup"><span data-stu-id="13fc6-376">In the **Save As** window, select the **Scripts** folder, name the file **init.js** and click **Save**.</span></span>

    <span data-ttu-id="13fc6-377">![Janela Salvar como](visual-studio-2013-web-tools/_static/image40.png "janela Salvar como")</span><span class="sxs-lookup"><span data-stu-id="13fc6-377">![Save As window](visual-studio-2013-web-tools/_static/image40.png "Save As window")</span></span>

    <span data-ttu-id="13fc6-378">*Janela Salvar como*</span><span class="sxs-lookup"><span data-stu-id="13fc6-378">*Save As window*</span></span>

    > [!NOTE]
    > <span data-ttu-id="13fc6-379">O **init.js** arquivo é criado e o conteúdo do script é movido para o arquivo.</span><span class="sxs-lookup"><span data-stu-id="13fc6-379">The **init.js** file is created and the content of the script is moved to the file.</span></span>
    > 
    > <span data-ttu-id="13fc6-380">![Arquivo init.js criado com o conteúdo incluído](visual-studio-2013-web-tools/_static/image41.png "arquivo Init.js criado com o conteúdo incluído")</span><span class="sxs-lookup"><span data-stu-id="13fc6-380">![Init.js file created with the content included](visual-studio-2013-web-tools/_static/image41.png "Init.js file created with the content included")</span></span>
    > 
    > <span data-ttu-id="13fc6-381">*Arquivo init.js criado com o conteúdo incluído*</span><span class="sxs-lookup"><span data-stu-id="13fc6-381">*Init.js file created with the content included*</span></span>
6. <span data-ttu-id="13fc6-382">Abra o **index. cshtml** do arquivo e verifique que a marca de script foi substituída por uma referência para o **init.js** arquivo.</span><span class="sxs-lookup"><span data-stu-id="13fc6-382">Open the **Index.cshtml** file and check that the script tag was replaced with a reference to the **init.js** file.</span></span>

    <span data-ttu-id="13fc6-383">![Referência de html init.js](visual-studio-2013-web-tools/_static/image42.png "Init.js referência de html")</span><span class="sxs-lookup"><span data-stu-id="13fc6-383">![Init.js html reference](visual-studio-2013-web-tools/_static/image42.png "Init.js html reference")</span></span>

    <span data-ttu-id="13fc6-384">*Referência de html init.js*</span><span class="sxs-lookup"><span data-stu-id="13fc6-384">*Init.js html reference*</span></span>
7. <span data-ttu-id="13fc6-385">Vá para o **Gerenciador de soluções** e observe que o **init.js** arquivo foi incluído automaticamente na solução.</span><span class="sxs-lookup"><span data-stu-id="13fc6-385">Go to the **Solution Explorer** and notice that the **init.js** file was included automatically in the solution.</span></span>

    <span data-ttu-id="13fc6-386">![Arquivo init.js incluído na solução](visual-studio-2013-web-tools/_static/image43.png "arquivo Init.js incluído na solução")</span><span class="sxs-lookup"><span data-stu-id="13fc6-386">![Init.js file included in solution](visual-studio-2013-web-tools/_static/image43.png "Init.js file included in solution")</span></span>

    <span data-ttu-id="13fc6-387">*Arquivo init.js incluído na solução*</span><span class="sxs-lookup"><span data-stu-id="13fc6-387">*Init.js file included in solution*</span></span>
8. <span data-ttu-id="13fc6-388">Volte para o **init.js** arquivo para atualizar o **pronto** retorno de chamada de função.</span><span class="sxs-lookup"><span data-stu-id="13fc6-388">Switch back to the **init.js** file to update the **ready** function callback.</span></span>
9. <span data-ttu-id="13fc6-389">Dentro da definição de retorno de chamada de função que é passada para *pronto*, adicione o seguinte código para obter todos os elementos por um atributo de classe específica.</span><span class="sxs-lookup"><span data-stu-id="13fc6-389">Inside the function callback definition that is passed to *ready*, add the following code to get all the elements by a specific class attribute.</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample10.js)]
10. <span data-ttu-id="13fc6-390">Pressione **CTRL** + **espaço** entre aspas dentro do **getElementsByClassName** chamada de função.</span><span class="sxs-lookup"><span data-stu-id="13fc6-390">Press **CTRL** + **Space** between the quotes inside the **getElementsByClassName** function call.</span></span>

    <span data-ttu-id="13fc6-391">![Mostrando o IntelliSense para a função getElementsByClassName](visual-studio-2013-web-tools/_static/image44.png "mostrando o IntelliSense para a função getElementsByClassName")</span><span class="sxs-lookup"><span data-stu-id="13fc6-391">![Showing IntelliSense for the getElementsByClassName function](visual-studio-2013-web-tools/_static/image44.png "Showing IntelliSense for the getElementsByClassName function")</span></span>

    <span data-ttu-id="13fc6-392">*Mostrando o IntelliSense para a função getElementsByClassName*</span><span class="sxs-lookup"><span data-stu-id="13fc6-392">*Showing IntelliSense for the getElementsByClassName function*</span></span>

    > [!NOTE]
    > <span data-ttu-id="13fc6-393">Observe que o IntelliSense mostra as classes definidas as folhas de estilo do projeto.</span><span class="sxs-lookup"><span data-stu-id="13fc6-393">Notice that IntelliSense shows the classes defined in the project style sheets.</span></span>
11. <span data-ttu-id="13fc6-394">Substitua a linha que você criou com o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="13fc6-394">Replace the line that you have created with the following code.</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample11.js)]
12. <span data-ttu-id="13fc6-395">Posicione o cursor após **au** dentro de aspas na **getElementsByTagName** função e pressione **CTRL** + **espaço**.</span><span class="sxs-lookup"><span data-stu-id="13fc6-395">Position the cursor after **au** inside the quotes in the **getElementsByTagName** function and press **CTRL** + **Space**.</span></span>

    <span data-ttu-id="13fc6-396">![Mostrando o IntelliSense para o método getElementByTagName](visual-studio-2013-web-tools/_static/image45.png "mostrando o IntelliSense para o método getElementByTagName")</span><span class="sxs-lookup"><span data-stu-id="13fc6-396">![Showing IntelliSense for the getElementByTagName method](visual-studio-2013-web-tools/_static/image45.png "Showing IntelliSense for the getElementByTagName method")</span></span>

    <span data-ttu-id="13fc6-397">*Mostrando o IntelliSense para o método getElementsByTagName*</span><span class="sxs-lookup"><span data-stu-id="13fc6-397">*Showing IntelliSense for the getElementsByTagName method*</span></span>
13. <span data-ttu-id="13fc6-398">Selecione **&quot;áudio&quot;** na lista e pressione **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="13fc6-398">Select **&quot;audio&quot;** from the list and press **ENTER**.</span></span> <span data-ttu-id="13fc6-399">O resultado é mostrado na figura a seguir.</span><span class="sxs-lookup"><span data-stu-id="13fc6-399">The result is shown in the following figure.</span></span>

    <span data-ttu-id="13fc6-400">![Recuperando elementos áudio](visual-studio-2013-web-tools/_static/image46.png "recuperando elementos de áudio")</span><span class="sxs-lookup"><span data-stu-id="13fc6-400">![Retrieving Audio Elements](visual-studio-2013-web-tools/_static/image46.png "Retrieving Audio Elements")</span></span>

    <span data-ttu-id="13fc6-401">*Recuperação de elementos de áudio*</span><span class="sxs-lookup"><span data-stu-id="13fc6-401">*Retrieving Audio Elements*</span></span>
14. <span data-ttu-id="13fc6-402">Na **Gerenciador de soluções**, com o botão direito a **init.js** de arquivo no **Scripts** pasta e selecione **JavaScript Minificar arquivos** do **Web Essentials** menu.</span><span class="sxs-lookup"><span data-stu-id="13fc6-402">In **Solution Explorer**, right-click the **init.js** file in the **Scripts** folder and select **Minify JavaScript file(s)** from the **Web Essentials** menu.</span></span>

    <span data-ttu-id="13fc6-403">![Reduzir arquivo (s) de JavaScript](visual-studio-2013-web-tools/_static/image47.png "arquivos JavaScript Minificar")</span><span class="sxs-lookup"><span data-stu-id="13fc6-403">![Minify JavaScript file(s)](visual-studio-2013-web-tools/_static/image47.png "Minify JavaScript files")</span></span>

    <span data-ttu-id="13fc6-404">*Reduzir arquivo (s) de JavaScript*</span><span class="sxs-lookup"><span data-stu-id="13fc6-404">*Minify JavaScript file(s)*</span></span>
15. <span data-ttu-id="13fc6-405">Quando solicitado para habilitar a minificação automática quando as alterações de arquivo de origem clica **Sim**.</span><span class="sxs-lookup"><span data-stu-id="13fc6-405">When prompted to enable automatic minification when the source file changes click **Yes**.</span></span>

    <span data-ttu-id="13fc6-406">![Habilitar o aviso automático de minimização](visual-studio-2013-web-tools/_static/image48.png "habilitando aviso automático de minimização")</span><span class="sxs-lookup"><span data-stu-id="13fc6-406">![Enabling automatic minification warning](visual-studio-2013-web-tools/_static/image48.png "Enabling automatic minification warning")</span></span>

    <span data-ttu-id="13fc6-407">*Habilitar o aviso automático de minimização*</span><span class="sxs-lookup"><span data-stu-id="13fc6-407">*Enabling automatic minification warning*</span></span>

    > [!NOTE]
    > <span data-ttu-id="13fc6-408">O **init.min.js** é criado e adicionado como uma dependência da **init.js** arquivo.</span><span class="sxs-lookup"><span data-stu-id="13fc6-408">The **init.min.js** is created and is added as a dependency of the **init.js** file.</span></span>
    > 
    > <span data-ttu-id="13fc6-409">![Arquivo init.min.js criado](visual-studio-2013-web-tools/_static/image49.png "Init.min.js arquivo criado")</span><span class="sxs-lookup"><span data-stu-id="13fc6-409">![Init.min.js file created](visual-studio-2013-web-tools/_static/image49.png "Init.min.js file created")</span></span>
    > 
    > <span data-ttu-id="13fc6-410">*Arquivo init.min.js criado*</span><span class="sxs-lookup"><span data-stu-id="13fc6-410">*Init.min.js file created*</span></span>
16. <span data-ttu-id="13fc6-411">Abra o **init.min.js** de arquivo e observe que o arquivo é minimizado.</span><span class="sxs-lookup"><span data-stu-id="13fc6-411">Open the **init.min.js** file and notice that the file is minified.</span></span>

    <span data-ttu-id="13fc6-412">![Conteúdo do arquivo init.min.js](visual-studio-2013-web-tools/_static/image50.png "Init.min.js o conteúdo do arquivo")</span><span class="sxs-lookup"><span data-stu-id="13fc6-412">![Init.min.js file content](visual-studio-2013-web-tools/_static/image50.png "Init.min.js file content")</span></span>

    <span data-ttu-id="13fc6-413">*Conteúdo do arquivo init.min.js*</span><span class="sxs-lookup"><span data-stu-id="13fc6-413">*Init.min.js file content*</span></span>
17. <span data-ttu-id="13fc6-414">No **init.js** do arquivo, adicione o código a seguir abaixo de **getElementsByTagName** chamada de função para reproduzir todos os elementos de áudio.</span><span class="sxs-lookup"><span data-stu-id="13fc6-414">In the **init.js** file, add the following code below the **getElementsByTagName** function call to play all the audio elements.</span></span>

    <span data-ttu-id="13fc6-415">(Código de trecho de código – *VisualStudio2013WebTooling* - *o Ex2* - *PlayAudioElements*)</span><span class="sxs-lookup"><span data-stu-id="13fc6-415">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)</span></span>

    [!code-csharp[Main](visual-studio-2013-web-tools/samples/sample12.cs)]
18. <span data-ttu-id="13fc6-416">Clique em **CTRL** + **S** para salvar o arquivo.</span><span class="sxs-lookup"><span data-stu-id="13fc6-416">Click **CTRL** + **S** to save the file.</span></span> <span data-ttu-id="13fc6-417">Uma vez que o arquivo reduzido já está aberto, você verá uma caixa de diálogo informando que o arquivo foi modificado fora do editor de código-fonte.</span><span class="sxs-lookup"><span data-stu-id="13fc6-417">Since the minified file is already opened, you will see a dialog box saying that the file was modified outside of the source editor.</span></span> <span data-ttu-id="13fc6-418">Clique em **Sim**.</span><span class="sxs-lookup"><span data-stu-id="13fc6-418">Click **Yes**.</span></span>

    <span data-ttu-id="13fc6-419">![Aviso do Microsoft Visual Studio](visual-studio-2013-web-tools/_static/image51.png "aviso do Microsoft Visual Studio")</span><span class="sxs-lookup"><span data-stu-id="13fc6-419">![Microsoft Visual Studio warning](visual-studio-2013-web-tools/_static/image51.png "Microsoft Visual Studio warning")</span></span>

    <span data-ttu-id="13fc6-420">*Aviso do Microsoft Visual Studio*</span><span class="sxs-lookup"><span data-stu-id="13fc6-420">*Microsoft Visual Studio warning*</span></span>
19. <span data-ttu-id="13fc6-421">Volte para o **init.min.js** arquivo para verificar que o arquivo foi atualizado com o novo código.</span><span class="sxs-lookup"><span data-stu-id="13fc6-421">Switch back to the **init.min.js** file to verify that the file was updated with the new code.</span></span>

    <span data-ttu-id="13fc6-422">![Arquivo init.min.js atualizado](visual-studio-2013-web-tools/_static/image52.png "Init.min.js arquivo atualizado")</span><span class="sxs-lookup"><span data-stu-id="13fc6-422">![Init.min.js file updated](visual-studio-2013-web-tools/_static/image52.png "Init.min.js file updated")</span></span>

    <span data-ttu-id="13fc6-423">*Arquivo init.min.js atualizado*</span><span class="sxs-lookup"><span data-stu-id="13fc6-423">*Init.min.js file updated*</span></span>
20. <span data-ttu-id="13fc6-424">Clique o **Atualizar Link do navegador** botão.</span><span class="sxs-lookup"><span data-stu-id="13fc6-424">Click the **Browser Link Refresh** button.</span></span>
21. <span data-ttu-id="13fc6-425">Depois que ambos os navegadores são atualizados os players de áudio que você viu na tarefa anterior começará a ser reproduzido automaticamente.</span><span class="sxs-lookup"><span data-stu-id="13fc6-425">Once both browsers are refreshed the audio players you saw in the previous task will start playing automatically.</span></span>

    <span data-ttu-id="13fc6-426">![Player de áudio incluído na exibição](visual-studio-2013-web-tools/_static/image53.png "incluído na exibição de player de áudio")</span><span class="sxs-lookup"><span data-stu-id="13fc6-426">![Audio player included in view](visual-studio-2013-web-tools/_static/image53.png "Audio player included in view")</span></span>

    <span data-ttu-id="13fc6-427">*Player de áudio incluído na exibição*</span><span class="sxs-lookup"><span data-stu-id="13fc6-427">*Audio player included in view*</span></span>

* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="13fc6-428">Resumo</span><span class="sxs-lookup"><span data-stu-id="13fc6-428">Summary</span></span>

<span data-ttu-id="13fc6-429">Ao concluir este laboratório prático que você aprendeu como:</span><span class="sxs-lookup"><span data-stu-id="13fc6-429">By completing this hands-on lab you have learned how to:</span></span>

- <span data-ttu-id="13fc6-430">Use os novos recursos do editor HTML incluídos no Web Essentials, como trechos de código avançados do HTML5 e o Zen de codificação</span><span class="sxs-lookup"><span data-stu-id="13fc6-430">Use new HTML editor features included in Web Essentials such as rich HTML5 code snippets and Zen coding</span></span>
- <span data-ttu-id="13fc6-431">Use os novos recursos do editor CSS incluídos no Web Essentials, como o seletor de cor e a dica de ferramenta de matriz de navegador</span><span class="sxs-lookup"><span data-stu-id="13fc6-431">Use new CSS editor features included in Web Essentials such as the Color picker and Browser matrix tooltip</span></span>
- <span data-ttu-id="13fc6-432">Use os novos recursos do editor JavaScript incluídos no Web Essentials, como extrair para o arquivo e o IntelliSense para todos os elementos HTML</span><span class="sxs-lookup"><span data-stu-id="13fc6-432">Use new JavaScript editor features included in Web Essentials such as Extract to File and IntelliSense for all HTML elements</span></span>
- <span data-ttu-id="13fc6-433">Trocar dados entre o navegador e o Visual Studio usando o Link do navegador</span><span class="sxs-lookup"><span data-stu-id="13fc6-433">Exchange data between your browser and Visual Studio using Browser Link</span></span>
