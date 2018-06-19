---
uid: visual-studio/overview/2013/visual-studio-2013-web-tools
title: 'Laboratório prático: Visual Studio 2013 Web Tools | Microsoft Docs'
author: rick-anderson
description: O Visual Studio é um ambiente de desenvolvimento excelente para. Janelas baseadas em rede e projetos da web. Ele inclui um editor de texto avançado que pode ser facilmente usado para...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: 09e82351-816b-402d-acd1-0f9ac6901d16
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/visual-studio-2013-web-tools
msc.type: authoredcontent
ms.openlocfilehash: ef8ab82f9043ef9da3a3e6a146a97f083149534d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/10/2018
ms.locfileid: "26507125"
---
<a name="hands-on-lab-visual-studio-2013-web-tools"></a><span data-ttu-id="19dd4-104">Laboratório prático: Ferramentas de Web Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="19dd4-104">Hands On Lab: Visual Studio 2013 Web Tools</span></span>
====================
<span data-ttu-id="19dd4-105">Por [Web Camps Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="19dd4-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="19dd4-106">Baixar o Kit de treinamento de Camps de Web</span><span class="sxs-lookup"><span data-stu-id="19dd4-106">Download Web Camps Training Kit</span></span>](http://aka.ms/webcamps-training-kit)

> <span data-ttu-id="19dd4-107">O Visual Studio é um ambiente de desenvolvimento excelente para. Janelas baseadas em rede e projetos da web.</span><span class="sxs-lookup"><span data-stu-id="19dd4-107">Visual Studio is an excellent development environment for .NET-based Windows and web projects.</span></span> <span data-ttu-id="19dd4-108">Ele inclui um editor de texto avançado que pode ser facilmente usado para editar arquivos autônomos sem um projeto.</span><span class="sxs-lookup"><span data-stu-id="19dd4-108">It includes a powerful text editor that can easily be used to edit standalone files without a project.</span></span>
> 
> <span data-ttu-id="19dd4-109">O Visual Studio mantém uma árvore de análise completa ao editar cada arquivo.</span><span class="sxs-lookup"><span data-stu-id="19dd4-109">Visual Studio maintains a full-featured parse tree as you edit each file.</span></span> <span data-ttu-id="19dd4-110">Isso permite que o Visual Studio fornecer preenchimento automático incomparável e ações com base em documento durante a experiência de desenvolvimento muito mais rápido e mais agradável.</span><span class="sxs-lookup"><span data-stu-id="19dd4-110">This allows Visual Studio to provide unparalleled auto-completion and document-based actions while making the development experience much faster and more pleasant.</span></span> <span data-ttu-id="19dd4-111">Esses recursos são especialmente eficientes em documentos HTML e CSS.</span><span class="sxs-lookup"><span data-stu-id="19dd4-111">These features are especially powerful in HTML and CSS documents.</span></span>
> 
> <span data-ttu-id="19dd4-112">Toda essa capacidade também está disponível para as extensões, tornando mais fácil estender os editores com novos recursos poderosos para atender às suas necessidades.</span><span class="sxs-lookup"><span data-stu-id="19dd4-112">All of this power is also available for extensions, making it simple to extend the editors with powerful new features to suit your needs.</span></span> <span data-ttu-id="19dd4-113">Web Essentials é uma coleção de (principalmente) relacionados à web aprimoramentos para o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="19dd4-113">Web Essentials is a collection of (mostly) web-related enhancements to Visual Studio.</span></span> <span data-ttu-id="19dd4-114">Ele inclui vários novos conclusões IntelliSense (especialmente para CSS), novos recursos de Link do navegador, automático JSHint para JavaScript arquivos, novos avisos para HTML, CSS e muitos outros recursos que são essenciais para o desenvolvimento na web moderna.</span><span class="sxs-lookup"><span data-stu-id="19dd4-114">It includes lots of new IntelliSense completions (especially for CSS), new Browser Link features, automatic JSHint for JavaScript files, new warnings for HTML and CSS, and many other features that are essential to modern web development.</span></span>
> 
> <span data-ttu-id="19dd4-115">Todo o código de exemplo e trechos de código são incluídos no Web Camps treinamento Kit, disponíveis em [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="19dd4-115">All sample code and snippets are included in the Web Camps Training Kit, available at [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span></span>


<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="19dd4-116">Visão geral</span><span class="sxs-lookup"><span data-stu-id="19dd4-116">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="19dd4-117">Objetivos</span><span class="sxs-lookup"><span data-stu-id="19dd4-117">Objectives</span></span>

<span data-ttu-id="19dd4-118">Este laboratório prático, você aprenderá como:</span><span class="sxs-lookup"><span data-stu-id="19dd4-118">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="19dd4-119">Usar os novos recursos do editor HTML incluídos no Web Essentials como ricos trechos de código do HTML5 e Zen codificação</span><span class="sxs-lookup"><span data-stu-id="19dd4-119">Use new HTML editor features included in Web Essentials such as rich HTML5 code snippets and Zen coding</span></span>
- <span data-ttu-id="19dd4-120">Use os novos recursos de editor de CSS incluídos no Essentials Web como o seletor de cores e uma dica de ferramenta de matriz de navegador</span><span class="sxs-lookup"><span data-stu-id="19dd4-120">Use new CSS editor features included in Web Essentials such as the Color picker and Browser matrix tooltip</span></span>
- <span data-ttu-id="19dd4-121">Usar novos recursos do editor JavaScript incluídos no Web Essentials como extrair para o arquivo e o IntelliSense para todos os elementos HTML</span><span class="sxs-lookup"><span data-stu-id="19dd4-121">Use new JavaScript editor features included in Web Essentials such as Extract to File and IntelliSense for all HTML elements</span></span>
- <span data-ttu-id="19dd4-122">Trocar dados entre o navegador e o Visual Studio usando o Link do navegador</span><span class="sxs-lookup"><span data-stu-id="19dd4-122">Exchange data between your browser and Visual Studio using Browser Link</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="19dd4-123">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="19dd4-123">Prerequisites</span></span>

<span data-ttu-id="19dd4-124">O exemplo a seguir é necessário para concluir este laboratório prático:</span><span class="sxs-lookup"><span data-stu-id="19dd4-124">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="19dd4-125">[Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) ou maior</span><span class="sxs-lookup"><span data-stu-id="19dd4-125">[Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="19dd4-126">Web Essentials 2013</span><span class="sxs-lookup"><span data-stu-id="19dd4-126">Web Essentials 2013</span></span>](http://vswebessentials.com/)
- [<span data-ttu-id="19dd4-127">Google Chrome</span><span class="sxs-lookup"><span data-stu-id="19dd4-127">Google Chrome</span></span>](https://www.google.com/chrome/)

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="19dd4-128">Configuração</span><span class="sxs-lookup"><span data-stu-id="19dd4-128">Setup</span></span>

<span data-ttu-id="19dd4-129">Para executar os exercícios neste laboratório prático, você precisará configurar seu ambiente primeiro.</span><span class="sxs-lookup"><span data-stu-id="19dd4-129">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="19dd4-130">Abra uma janela do Windows Explorer e navegue até o laboratório **fonte** pasta.</span><span class="sxs-lookup"><span data-stu-id="19dd4-130">Open a Windows Explorer window and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="19dd4-131">Clique com botão direito **Setup.cmd** e selecione **executar como administrador** para iniciar o processo de instalação que irá configurar seu ambiente e instale os trechos de código do Visual Studio para este laboratório.</span><span class="sxs-lookup"><span data-stu-id="19dd4-131">Right-click **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="19dd4-132">Se a caixa de diálogo controle de conta de usuário for exibida, confirme a ação para continuar.</span><span class="sxs-lookup"><span data-stu-id="19dd4-132">If the User Account Control dialog box is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="19dd4-133">Verifique se que você selecionou todas as dependências para este laboratório antes de executar a instalação.</span><span class="sxs-lookup"><span data-stu-id="19dd4-133">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="19dd4-134">Usando os trechos de código</span><span class="sxs-lookup"><span data-stu-id="19dd4-134">Using the Code Snippets</span></span>

<span data-ttu-id="19dd4-135">Em todo o documento de laboratório, você será instruído a inserir blocos de código.</span><span class="sxs-lookup"><span data-stu-id="19dd4-135">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="19dd4-136">Para sua conveniência, a maioria do código é fornecido como o Visual Studio trechos de código que pode ser acessada de dentro do Visual Studio 2013 para evitar a necessidade para adicioná-lo manualmente.</span><span class="sxs-lookup"><span data-stu-id="19dd4-136">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="19dd4-137">Cada exercício é acompanhado por uma solução inicial localizada no **começar** pasta do exercício que permite que você execute cada exercício independentemente de outras pessoas.</span><span class="sxs-lookup"><span data-stu-id="19dd4-137">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="19dd4-138">Esteja ciente de que os trechos de código que são adicionados durante um exercício estão ausentes dos seguintes iniciando soluções e podem não funcionar até que você concluiu o exercício.</span><span class="sxs-lookup"><span data-stu-id="19dd4-138">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="19dd4-139">Dentro do código-fonte para um exercício, você também encontrará um **final** pasta que contém uma solução do Visual Studio com o código que é o resultado de concluir as etapas no exercício correspondente.</span><span class="sxs-lookup"><span data-stu-id="19dd4-139">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="19dd4-140">Você pode usar essas soluções como orientação se você precisar de ajuda adicional usando este laboratório prático.</span><span class="sxs-lookup"><span data-stu-id="19dd4-140">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


* * *

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="19dd4-141">Exercícios</span><span class="sxs-lookup"><span data-stu-id="19dd4-141">Exercises</span></span>

<span data-ttu-id="19dd4-142">Este laboratório prático inclui os seguintes exercícios:</span><span class="sxs-lookup"><span data-stu-id="19dd4-142">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="19dd4-143">Trabalhando com os conceitos básicos de Web e de Link do navegador</span><span class="sxs-lookup"><span data-stu-id="19dd4-143">Working with Browser Link and Web Essentials</span></span>](#Exercise1)
2. [<span data-ttu-id="19dd4-144">Aproveitando as vantagens de trechos de código e o IntelliSense</span><span class="sxs-lookup"><span data-stu-id="19dd4-144">Taking Advantage of Code Snippets and IntelliSense</span></span>](#Exercise2)

> [!NOTE]
> <span data-ttu-id="19dd4-145">Quando você inicia o Visual Studio pela primeira vez, você deve selecionar uma das coleções de configurações predefinidas.</span><span class="sxs-lookup"><span data-stu-id="19dd4-145">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="19dd4-146">Cada coleção predefinida foi projetada para coincidir com um estilo de desenvolvimento específico e determina o comportamento do editor, layouts de janela, trechos de código IntelliSense e opções da caixa de diálogo.</span><span class="sxs-lookup"><span data-stu-id="19dd4-146">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="19dd4-147">Os procedimentos neste laboratório descrevem as ações necessárias para realizar uma tarefa específica no Visual Studio ao usar o **configurações gerais de desenvolvimento** coleção.</span><span class="sxs-lookup"><span data-stu-id="19dd4-147">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="19dd4-148">Se você escolher uma coleção de configurações diferentes para o seu ambiente de desenvolvimento, pode haver diferenças nas etapas que você deve levar em conta.</span><span class="sxs-lookup"><span data-stu-id="19dd4-148">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-browser-link-and-web-essentials"></a><span data-ttu-id="19dd4-149">Exercício 1: Trabalhando com os conceitos básicos de Web e de Link do navegador</span><span class="sxs-lookup"><span data-stu-id="19dd4-149">Exercise 1: Working with Browser Link and Web Essentials</span></span>

<span data-ttu-id="19dd4-150">**Web Essentials** é uma extensão do Visual Studio que adiciona uma variedade de recursos úteis para o desenvolvimento de web modernos, principalmente se concentra em tornar a experiência de desenvolvimento da web muito mais rápida e mais agradável.</span><span class="sxs-lookup"><span data-stu-id="19dd4-150">**Web Essentials** is a Visual Studio extension that adds a variety of useful features for modern web development, mostly focused on making the web development experience much faster and more pleasant.</span></span> <span data-ttu-id="19dd4-151">Você pode instalar o Web Essentials na Galeria de extensão no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="19dd4-151">You can install Web Essentials from the Extension Gallery in Visual Studio.</span></span>

<span data-ttu-id="19dd4-152">**Link do navegador** é um novo recurso incluído no Visual Studio 2013 que fornece um canal entre o Visual Studio IDE e qualquer navegador aberto para trocar dados entre seu aplicativo web e o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="19dd4-152">**Browser Link** is a new feature included in Visual Studio 2013 that provides a channel between the Visual Studio IDE and any open browser to exchange data between your web application and Visual Studio.</span></span> <span data-ttu-id="19dd4-153">Web Essentials estende o Link do navegador com ferramentas para manipular o modelo de objeto DOM e os estilos CSS de suas páginas da web diretamente do navegador.</span><span class="sxs-lookup"><span data-stu-id="19dd4-153">Web Essentials extends Browser Link with tools to manipulate the DOM object model and the CSS styles of your web pages directly from the browser.</span></span>

<span data-ttu-id="19dd4-154">Neste exercício, você irá explorar alguns dos recursos com suporte **Web Essentials** e **Link do navegador** para aprimorar uma página de teste simples.</span><span class="sxs-lookup"><span data-stu-id="19dd4-154">In this exercise, you will explore some of the features supported by **Web Essentials** and **Browser Link** to enhance a simple quiz page.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1---running-the-project-in-multiple-browsers"></a><span data-ttu-id="19dd4-155">Tarefa 1: executar o projeto em vários navegadores</span><span class="sxs-lookup"><span data-stu-id="19dd4-155">Task 1 - Running the Project in Multiple Browsers</span></span>

<span data-ttu-id="19dd4-156">Nesta tarefa, você irá configurar seu aplicativo da web para ser executado em vários navegadores de uma vez, que é útil para testes entre navegadores.</span><span class="sxs-lookup"><span data-stu-id="19dd4-156">In this task, you will configure your web application to run in multiple browsers at once, which is useful for cross-browser testing.</span></span>

1. <span data-ttu-id="19dd4-157">Abra **Microsoft Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="19dd4-157">Open **Microsoft Visual Studio**.</span></span>
2. <span data-ttu-id="19dd4-158">No **arquivo** menu, selecione **abrir | Projeto/solução...**  e navegue até **Ex1 WorkingwithBrowserLinkandWebEssentials\Begin** no **fonte** pasta do laboratório (C:\WebCampsTK\HOL\VSWebTooling\Source).</span><span class="sxs-lookup"><span data-stu-id="19dd4-158">In the **File** menu, select **Open | Project/Solution...** and browse to **Ex1-WorkingwithBrowserLinkandWebEssentials\Begin** in the **Source** folder of the lab (C:\WebCampsTK\HOL\VSWebTooling\Source).</span></span> <span data-ttu-id="19dd4-159">Selecione **Begin.sln** e clique em **abrir**.</span><span class="sxs-lookup"><span data-stu-id="19dd4-159">Select **Begin.sln** and click **Open**.</span></span>
3. <span data-ttu-id="19dd4-160">Na barra de ferramentas do Visual Studio, expanda o menu do navegador e selecione **procurar com...** .</span><span class="sxs-lookup"><span data-stu-id="19dd4-160">In the Visual Studio toolbar, expand the browser menu and select **Browse With...**.</span></span>

    <span data-ttu-id="19dd4-161">![Opção de menu procurar com](visual-studio-2013-web-tools/_static/image1.png "procurar com... no menu do navegador")</span><span class="sxs-lookup"><span data-stu-id="19dd4-161">![Browse With menu option](visual-studio-2013-web-tools/_static/image1.png "Browse with... in browser menu")</span></span>

    <span data-ttu-id="19dd4-162">*Procurar com opção de menu*</span><span class="sxs-lookup"><span data-stu-id="19dd4-162">*Browse With menu option*</span></span>
4. <span data-ttu-id="19dd4-163">No **procurar com** caixa de diálogo, selecione **Google Chrome** e **Internet Explorer** segurando o **CTRL** chave e clique em  **Definir como padrão**.</span><span class="sxs-lookup"><span data-stu-id="19dd4-163">In the **Browse With** dialog box, select both **Google Chrome** and **Internet Explorer** by holding down the **CTRL** key and click **Set as Default**.</span></span>

    <span data-ttu-id="19dd4-164">![Procurar a caixa de diálogo](visual-studio-2013-web-tools/_static/image2.png "procurar a caixa de diálogo")</span><span class="sxs-lookup"><span data-stu-id="19dd4-164">![Browse with dialog box](visual-studio-2013-web-tools/_static/image2.png "Browse with dialog box")</span></span>

    <span data-ttu-id="19dd4-165">*Selecionando vários navegadores padrão*</span><span class="sxs-lookup"><span data-stu-id="19dd4-165">*Selecting multiple default browsers*</span></span>
5. <span data-ttu-id="19dd4-166">Google Chrome e o Internet Explorer agora devem aparecer como os navegadores padrão.</span><span class="sxs-lookup"><span data-stu-id="19dd4-166">Both Google Chrome and Internet Explorer should now appear as the default browsers.</span></span> <span data-ttu-id="19dd4-167">Clique em **Cancelar** para fechar a caixa de diálogo.</span><span class="sxs-lookup"><span data-stu-id="19dd4-167">Click **Cancel** to close the dialog box.</span></span>

    <span data-ttu-id="19dd4-168">![Google Chrome e o Internet Explorer como navegadores padrão](visual-studio-2013-web-tools/_static/image3.png "navegadores de padrão do Internet Explorer e Google Chrome")</span><span class="sxs-lookup"><span data-stu-id="19dd4-168">![Google Chrome and Internet Explorer as default browsers](visual-studio-2013-web-tools/_static/image3.png "Google Chrome and Internet Explorer default browsers")</span></span>

    <span data-ttu-id="19dd4-169">*Google Chrome e o Internet Explorer como navegadores padrão*</span><span class="sxs-lookup"><span data-stu-id="19dd4-169">*Google Chrome and Internet Explorer as default browsers*</span></span>

    > [!NOTE]
    > <span data-ttu-id="19dd4-170">Depois de configurar os navegadores padrão, o **vários navegadores** opção é selecionada no menu do navegador.</span><span class="sxs-lookup"><span data-stu-id="19dd4-170">After configuring the default browsers, the **Multiple Browsers** option is selected in the browser menu.</span></span>
    > 
    > <span data-ttu-id="19dd4-171">![Vários navegadores](visual-studio-2013-web-tools/_static/image4.png "vários navegadores")</span><span class="sxs-lookup"><span data-stu-id="19dd4-171">![Multiple browsers](visual-studio-2013-web-tools/_static/image4.png "Multiple browsers")</span></span>
6. <span data-ttu-id="19dd4-172">Pressione **CTRL** + **F5** para executar o aplicativo sem depuração.</span><span class="sxs-lookup"><span data-stu-id="19dd4-172">Press **CTRL** + **F5** to run the application without debugging.</span></span>
7. <span data-ttu-id="19dd4-173">Quando ambas as janelas do navegador se abre, coloque um deles acima do outro para ver as atualizações em ambos os navegadores simultaneamente.</span><span class="sxs-lookup"><span data-stu-id="19dd4-173">When both browser windows open, place one of them above the other in order to see the updates on both browsers simultaneously.</span></span> <span data-ttu-id="19dd4-174">Os navegadores devem exibir uma página da web com um retângulo azul-claro.</span><span class="sxs-lookup"><span data-stu-id="19dd4-174">The browsers should display a web page with a light-blue rectangle.</span></span>

    <span data-ttu-id="19dd4-175">![Colocando um navegador acima do outro](visual-studio-2013-web-tools/_static/image5.png "colocando um navegador acima do outro")</span><span class="sxs-lookup"><span data-stu-id="19dd4-175">![Placing one browser above the other](visual-studio-2013-web-tools/_static/image5.png "Placing one browser above the other")</span></span>

    <span data-ttu-id="19dd4-176">*Colocando um navegador acima do outro*</span><span class="sxs-lookup"><span data-stu-id="19dd4-176">*Placing one browser above the other*</span></span>
8. <span data-ttu-id="19dd4-177">Não feche os navegadores.</span><span class="sxs-lookup"><span data-stu-id="19dd4-177">Do not close the browsers.</span></span> <span data-ttu-id="19dd4-178">Você irá usá-los na próxima tarefa.</span><span class="sxs-lookup"><span data-stu-id="19dd4-178">You will use them in the next task.</span></span>

<a id="Ex1Task2"></a>
#### <a name="task-2---using-zen-coding-to-create-html-elements"></a><span data-ttu-id="19dd4-179">Tarefa 2 - Zen usando codificação para criar elementos HTML</span><span class="sxs-lookup"><span data-stu-id="19dd4-179">Task 2 - Using Zen Coding to Create HTML Elements</span></span>

<span data-ttu-id="19dd4-180">**Codificação Zen** é um editor de codificação de plug-in de alta velocidade HTML, XML, XSL (ou qualquer outro formato de código estruturado) e edição.</span><span class="sxs-lookup"><span data-stu-id="19dd4-180">**Zen Coding** is an editor plugin for high-speed HTML, XML, XSL (or any other structured code format) coding and editing.</span></span> <span data-ttu-id="19dd4-181">O núcleo deste plug-in é um mecanismo de abreviação poderoso que permite que você expanda expressões - semelhantes para seletores CSS - em código HTML.</span><span class="sxs-lookup"><span data-stu-id="19dd4-181">The core of this plugin is a powerful abbreviation engine which allows you to expand expressions -similar to CSS selectors- into HTML code.</span></span> <span data-ttu-id="19dd4-182">Codificação Zen é uma maneira rápida de escrever a sintaxe do seletor de estilo HTML usando uma CSS.</span><span class="sxs-lookup"><span data-stu-id="19dd4-182">Zen Coding is a fast way to write HTML using a CSS style selector syntax.</span></span>

<span data-ttu-id="19dd4-183">Neste exercício, você usará o recurso Zen codificação fornecido pelo Web Essentials para gerar os botões HTML que representam as opções da pergunta.</span><span class="sxs-lookup"><span data-stu-id="19dd4-183">In this exercise, you will use the Zen Coding feature provided by Web Essentials to generate the HTML buttons that represent the options of the question.</span></span>

1. <span data-ttu-id="19dd4-184">Alterne para o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="19dd4-184">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="19dd4-185">Abra o **cshtml** arquivo localizado no **exibições** | **início** pasta.</span><span class="sxs-lookup"><span data-stu-id="19dd4-185">Open the **Index.cshtml** file located in the **Views** | **Home** folder.</span></span>
3. <span data-ttu-id="19dd4-186">Substitua o **&lt;! – TODO: Adicione aqui – opções&gt;** comentário com o seguinte código e pressione **guia**.</span><span class="sxs-lookup"><span data-stu-id="19dd4-186">Replace the **&lt;!-- TODO: add options here--&gt;** comment with the following code, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample1.css)]
4. <span data-ttu-id="19dd4-187">O código deve ser expandido para HTML.</span><span class="sxs-lookup"><span data-stu-id="19dd4-187">The code should be expanded to HTML.</span></span>

    <span data-ttu-id="19dd4-188">![Expandido HTML](visual-studio-2013-web-tools/_static/image6.png "expandido HTML")</span><span class="sxs-lookup"><span data-stu-id="19dd4-188">![Expanded HTML](visual-studio-2013-web-tools/_static/image6.png "Expanded HTML")</span></span>

    <span data-ttu-id="19dd4-189">*HTML expandido*</span><span class="sxs-lookup"><span data-stu-id="19dd4-189">*Expanded HTML*</span></span>

    > [!NOTE]
    > <span data-ttu-id="19dd4-190">Para saber mais sobre a sintaxe Zen de codificação, consulte o seguinte [artigo](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).</span><span class="sxs-lookup"><span data-stu-id="19dd4-190">To learn more about Zen Coding syntax, see the following [article](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).</span></span>
5. <span data-ttu-id="19dd4-191">Clique o **atualizar navegadores vinculados** botão atualizar ambos os navegadores.</span><span class="sxs-lookup"><span data-stu-id="19dd4-191">Click the **Refresh linked browsers** button to update both browsers.</span></span>

    <span data-ttu-id="19dd4-192">![Atualizar navegadores vinculados](visual-studio-2013-web-tools/_static/image7.png "atualizar navegadores vinculados")</span><span class="sxs-lookup"><span data-stu-id="19dd4-192">![Refresh linked browsers](visual-studio-2013-web-tools/_static/image7.png "Refresh linked browsers")</span></span>

    <span data-ttu-id="19dd4-193">*Atualizar navegadores vinculados*</span><span class="sxs-lookup"><span data-stu-id="19dd4-193">*Refresh linked browsers*</span></span>

    <span data-ttu-id="19dd4-194">![Internet Explorer - página atualizada com quatro botões](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer - página atualizada com quatro botões")</span><span class="sxs-lookup"><span data-stu-id="19dd4-194">![Internet Explorer - Page updated with four buttons](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer - Page updated with four buttons")</span></span>

    <span data-ttu-id="19dd4-195">*Internet Explorer - página atualizada com quatro botões*</span><span class="sxs-lookup"><span data-stu-id="19dd4-195">*Internet Explorer - Page updated with four buttons*</span></span>

    <span data-ttu-id="19dd4-196">![Google Chrome - página atualizada com quatro botões](visual-studio-2013-web-tools/_static/image9.png "Google Chrome - página atualizada com quatro botões")</span><span class="sxs-lookup"><span data-stu-id="19dd4-196">![Google Chrome - Page updated with four buttons](visual-studio-2013-web-tools/_static/image9.png "Google Chrome - Page updated with four buttons")</span></span>

    <span data-ttu-id="19dd4-197">*Google Chrome - página atualizada com quatro botões*</span><span class="sxs-lookup"><span data-stu-id="19dd4-197">*Google Chrome - Page updated with four buttons*</span></span>
6. <span data-ttu-id="19dd4-198">Alterne para o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="19dd4-198">Switch back to Visual Studio.</span></span>
7. <span data-ttu-id="19dd4-199">Adicionar os botões para a página, mas você ainda precisa adicionar uma pergunta de modelo.</span><span class="sxs-lookup"><span data-stu-id="19dd4-199">You have added the buttons to the page, but you still need to add a template question.</span></span> <span data-ttu-id="19dd4-200">Para fazer isso, você usará um novo recurso no Essentials Web chamado **Lorem Ipsum gerador**.</span><span class="sxs-lookup"><span data-stu-id="19dd4-200">To do so, you will use a new feature in Web Essentials called **Lorem Ipsum generator**.</span></span> <span data-ttu-id="19dd4-201">Localize o **div** elemento com o **classe** atributo **frontal**.</span><span class="sxs-lookup"><span data-stu-id="19dd4-201">Locate the **div** element with the **class** attribute **front**.</span></span>
8. <span data-ttu-id="19dd4-202">Adicione o seguinte código como o primeiro elemento filho do **div**e pressione **guia**.</span><span class="sxs-lookup"><span data-stu-id="19dd4-202">Add the following code as the first child element of the **div**, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample2.css)]
9. <span data-ttu-id="19dd4-203">O código deve ser expandido para HTML.</span><span class="sxs-lookup"><span data-stu-id="19dd4-203">The code should be expanded to HTML.</span></span>

    <span data-ttu-id="19dd4-204">![Gerado automaticamente do Lorem Ipsum](visual-studio-2013-web-tools/_static/image10.png "gerado Lorem Ipsum automaticamente")</span><span class="sxs-lookup"><span data-stu-id="19dd4-204">![Lorem Ipsum autogenerated](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum autogenerated")</span></span>

    <span data-ttu-id="19dd4-205">*Gerado Lorem Ipsum automaticamente*</span><span class="sxs-lookup"><span data-stu-id="19dd4-205">*Lorem Ipsum autogenerated*</span></span>

    > [!NOTE]
    > <span data-ttu-id="19dd4-206">Como parte da codificação Zen, agora é possível gerar código Lorem Ipsum diretamente no editor de HTML.</span><span class="sxs-lookup"><span data-stu-id="19dd4-206">As part of Zen Coding, you can now generate Lorem Ipsum code directly in the HTML editor.</span></span> <span data-ttu-id="19dd4-207">Basta digitar **lorem** e clique em **guia** e um 30 palavras Lorem Ipsum o texto será inserido.</span><span class="sxs-lookup"><span data-stu-id="19dd4-207">Simply type **lorem** and hit **TAB** and a 30 word Lorem Ipsum text will be inserted.</span></span> <span data-ttu-id="19dd4-208">Por exemplo,</span><span class="sxs-lookup"><span data-stu-id="19dd4-208">E.g.</span></span> <span data-ttu-id="19dd4-209">*lorem10* insere 10 Lorem Ipsum palavras.</span><span class="sxs-lookup"><span data-stu-id="19dd4-209">*lorem10* inserts 10 Lorem Ipsum words.</span></span>
10. <span data-ttu-id="19dd4-210">Você irá adicionar um logotipo na parte superior da pergunta usando outro novo recurso no Essentials Web chamado **gerador de Lorem Pixel**.</span><span class="sxs-lookup"><span data-stu-id="19dd4-210">You will add a logo at the top of the question by using another new feature in Web Essentials called **Lorem Pixel generator**.</span></span> <span data-ttu-id="19dd4-211">Adicione o seguinte código como o primeiro elemento filho do **div** elemento com **contêiner** como **classe** valor e pressione **guia**.</span><span class="sxs-lookup"><span data-stu-id="19dd4-211">Add the following code as the first child element of the **div** element with **container** as **class** value, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample3.css)]
11. <span data-ttu-id="19dd4-212">O código deve expandir para HTML.</span><span class="sxs-lookup"><span data-stu-id="19dd4-212">The code should expand to HTML.</span></span>

    <span data-ttu-id="19dd4-213">![Gerado automaticamente de Lorem Pixel](visual-studio-2013-web-tools/_static/image11.png "gerados automaticamente de Lorem Pixel")</span><span class="sxs-lookup"><span data-stu-id="19dd4-213">![Lorem Pixel autogenerated](visual-studio-2013-web-tools/_static/image11.png "Lorem Pixel autogenerated")</span></span>

    <span data-ttu-id="19dd4-214">*Gerado automaticamente de Lorem Pixel*</span><span class="sxs-lookup"><span data-stu-id="19dd4-214">*Lorem Pixel autogenerated*</span></span>

    > [!NOTE]
    > <span data-ttu-id="19dd4-215">Como parte da codificação Zen, você também pode gerar o código de Lorem Pixel diretamente no editor de HTML.</span><span class="sxs-lookup"><span data-stu-id="19dd4-215">As part of Zen Coding, you can also generate Lorem Pixel code directly in the HTML editor.</span></span> <span data-ttu-id="19dd4-216">Basta digitar **pix-200 x 200-animais** e clique em **guia** e um **img** marca com uma imagem de um animal de 200 x 200 será inserida.</span><span class="sxs-lookup"><span data-stu-id="19dd4-216">Simply type **pix-200x200-animals** and hit **TAB** and an **img** tag with a 200x200 image of an animal will be inserted.</span></span> <span data-ttu-id="19dd4-217">Para obter mais informações, consulte [Lorem Pixel](http://www.lorempixel.com).</span><span class="sxs-lookup"><span data-stu-id="19dd4-217">For more information, refer to [Lorem Pixel](http://www.lorempixel.com).</span></span>
12. <span data-ttu-id="19dd4-218">Clique o **atualizar navegadores vinculados** botão atualizar ambos os navegadores.</span><span class="sxs-lookup"><span data-stu-id="19dd4-218">Click the **Refresh linked browsers** button to update both browsers.</span></span>

    <span data-ttu-id="19dd4-219">![Internet Explorer - imagem gerada automaticamente e texto](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer - imagem gerada automaticamente e texto")</span><span class="sxs-lookup"><span data-stu-id="19dd4-219">![Internet Explorer - Autogenerated image and text](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer - Autogenerated image and text")</span></span>

    <span data-ttu-id="19dd4-220">*Internet Explorer - imagem gerada automaticamente e texto*</span><span class="sxs-lookup"><span data-stu-id="19dd4-220">*Internet Explorer - Autogenerated image and text*</span></span>

    <span data-ttu-id="19dd4-221">![Google Chrome - imagem gerada automaticamente e texto](visual-studio-2013-web-tools/_static/image13.png "Google Chrome - imagem gerada automaticamente e texto")</span><span class="sxs-lookup"><span data-stu-id="19dd4-221">![Google Chrome - Autogenerated image and text](visual-studio-2013-web-tools/_static/image13.png "Google Chrome - Autogenerated image and text")</span></span>

    <span data-ttu-id="19dd4-222">*Google Chrome - imagem gerada automaticamente e texto*</span><span class="sxs-lookup"><span data-stu-id="19dd4-222">*Google Chrome - Autogenerated image and text*</span></span>

    > [!NOTE]
    > <span data-ttu-id="19dd4-223">Como a imagem é selecionada aleatoriamente ao adicionar o trecho de código, a imagem mostrada os navegadores pode diferir.</span><span class="sxs-lookup"><span data-stu-id="19dd4-223">Because the image is selected randomly when adding the code snippet, the image shown in the browsers may differ.</span></span>
13. <span data-ttu-id="19dd4-224">Não feche os navegadores.</span><span class="sxs-lookup"><span data-stu-id="19dd4-224">Do not close the browsers.</span></span> <span data-ttu-id="19dd4-225">Você irá usá-los na próxima tarefa.</span><span class="sxs-lookup"><span data-stu-id="19dd4-225">You will use them in the next task.</span></span>

<a id="Ex1Task3"></a>
#### <a name="task-3---updating-a-style-property"></a><span data-ttu-id="19dd4-226">Tarefa 3 - atualizando uma propriedade de estilo</span><span class="sxs-lookup"><span data-stu-id="19dd4-226">Task 3 - Updating a Style Property</span></span>

<span data-ttu-id="19dd4-227">Nesta tarefa, você usará o Link de navegador **modo inspecionar** recurso para detectar o local exato em que o elemento DOM específico é gerado e, em seguida, atualize a propriedade de cor do elemento usando um seletor de cores fornecido pelo Web Conceitos básicos.</span><span class="sxs-lookup"><span data-stu-id="19dd4-227">In this task, you will use the Browser Link's **Inspect Mode** feature to detect the exact location where the specific DOM element is generated and then update the color property of that element using a color picker provided by Web Essentials.</span></span>

1. <span data-ttu-id="19dd4-228">No navegador Internet Explorer, pressione **CTRL** + **ALT** + **I** para habilitar o modo de inspecionar.</span><span class="sxs-lookup"><span data-stu-id="19dd4-228">In the Internet Explorer browser, press **CTRL** + **ALT** + **I** to enable Inspect Mode.</span></span>
2. <span data-ttu-id="19dd4-229">Mova o ponteiro sobre a borda azul clara e clique em.</span><span class="sxs-lookup"><span data-stu-id="19dd4-229">Move the pointer over the light blue border and click.</span></span>

    <span data-ttu-id="19dd4-230">![Mover o ponteiro sobre a borda azul clara](visual-studio-2013-web-tools/_static/image14.png "mover o ponteiro sobre a borda azul clara")</span><span class="sxs-lookup"><span data-stu-id="19dd4-230">![Moving the pointer over the light blue border](visual-studio-2013-web-tools/_static/image14.png "Moving the pointer over the light blue border")</span></span>

    <span data-ttu-id="19dd4-231">*Mover o ponteiro sobre a borda azul clara*</span><span class="sxs-lookup"><span data-stu-id="19dd4-231">*Moving the pointer over the light blue border*</span></span>
3. <span data-ttu-id="19dd4-232">Alterne para o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="19dd4-232">Switch back to Visual Studio.</span></span> <span data-ttu-id="19dd4-233">Observe como o elemento HTML que você selecionou no navegador também é selecionado no editor de HTML do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="19dd4-233">Notice how the HTML element that you selected in the browser is also selected in the Visual Studio HTML editor.</span></span>

    <span data-ttu-id="19dd4-234">![Elemento HTML selecionado no editor de HTML do Visual Studio](visual-studio-2013-web-tools/_static/image15.png "elemento HTML selecionado no editor de HTML do Visual Studio")</span><span class="sxs-lookup"><span data-stu-id="19dd4-234">![HTML element selected in the Visual Studio HTML editor](visual-studio-2013-web-tools/_static/image15.png "HTML element selected in the Visual Studio HTML editor")</span></span>

    <span data-ttu-id="19dd4-235">*Elemento HTML selecionado no editor de HTML do Visual Studio*</span><span class="sxs-lookup"><span data-stu-id="19dd4-235">*HTML element selected in the Visual Studio HTML editor*</span></span>
4. <span data-ttu-id="19dd4-236">Agora você atualizará o **frontal** classe CSS para alterar o estilo do elemento selecionado.</span><span class="sxs-lookup"><span data-stu-id="19dd4-236">You will now update the **front** CSS class in order to change the styling of the selected element.</span></span> <span data-ttu-id="19dd4-237">Para fazer isso, pressione **CTRL** + **,** para abrir o **navegar para** caixa de pesquisa.</span><span class="sxs-lookup"><span data-stu-id="19dd4-237">To do so, press **CTRL** + **,** to open the **Navigate To** search box.</span></span> <span data-ttu-id="19dd4-238">Tipo **site.css** e pressione **ENTER** para abrir o arquivo.</span><span class="sxs-lookup"><span data-stu-id="19dd4-238">Type **site.css** and press **ENTER** to open the file.</span></span>

    <span data-ttu-id="19dd4-239">![Abrir arquivo Site.css](visual-studio-2013-web-tools/_static/image16.png "Site.css ao abrir arquivo")</span><span class="sxs-lookup"><span data-stu-id="19dd4-239">![Opening file Site.css](visual-studio-2013-web-tools/_static/image16.png "Opening file Site.css")</span></span>

    <span data-ttu-id="19dd4-240">*Abrir o arquivo Site.css*</span><span class="sxs-lookup"><span data-stu-id="19dd4-240">*Opening file Site.css*</span></span>
5. <span data-ttu-id="19dd4-241">Pressione **CTRL** + **F** e tipo **.front .flip contêiner** para localizar o seletor de CSS.</span><span class="sxs-lookup"><span data-stu-id="19dd4-241">Press **CTRL** + **F** and type **.flip-containter .front** to find the CSS selector.</span></span>
6. <span data-ttu-id="19dd4-242">Clique no quadrado azul claro na propriedade da classe para abrir o seletor de cor de borda.</span><span class="sxs-lookup"><span data-stu-id="19dd4-242">Click the light blue square in the border property of the class to open the Color Picker.</span></span>

    <span data-ttu-id="19dd4-243">![Abrir o seletor de cores](visual-studio-2013-web-tools/_static/image17.png "abrir o seletor de cores")</span><span class="sxs-lookup"><span data-stu-id="19dd4-243">![Opening the Color Picker](visual-studio-2013-web-tools/_static/image17.png "Opening the Color Picker")</span></span>

    <span data-ttu-id="19dd4-244">*Abrir o seletor de cores*</span><span class="sxs-lookup"><span data-stu-id="19dd4-244">*Opening the Color Picker*</span></span>
7. <span data-ttu-id="19dd4-245">Expanda o seletor de cores, clicando no botão de seta e selecione uma cor.</span><span class="sxs-lookup"><span data-stu-id="19dd4-245">Expand the Color Picker by clicking the chevron button and select a new color.</span></span>

    <span data-ttu-id="19dd4-246">![Expandindo o seletor de cores](visual-studio-2013-web-tools/_static/image18.png "expandindo o seletor de cores")</span><span class="sxs-lookup"><span data-stu-id="19dd4-246">![Expanding the Color Picker](visual-studio-2013-web-tools/_static/image18.png "Expanding the Color Picker")</span></span>

    <span data-ttu-id="19dd4-247">*Expandindo o seletor de cores*</span><span class="sxs-lookup"><span data-stu-id="19dd4-247">*Expanding the Color Picker*</span></span>
8. <span data-ttu-id="19dd4-248">Pressione **CTRL** + **ALT** + **ENTER** atualizar navegadores vinculados.</span><span class="sxs-lookup"><span data-stu-id="19dd4-248">Press **CTRL** + **ALT** + **ENTER** to refresh linked browsers.</span></span>
9. <span data-ttu-id="19dd4-249">Alterne para o Internet Explorer e observe como a cor da borda foi alterado.</span><span class="sxs-lookup"><span data-stu-id="19dd4-249">Switch to Internet Explorer and notice how the color of the border has changed.</span></span>

    <span data-ttu-id="19dd4-250">![Internet Explorer - atualizado de cor de borda](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer - atualizado de cor de borda")</span><span class="sxs-lookup"><span data-stu-id="19dd4-250">![Internet Explorer - Border color updated](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer - Border color updated")</span></span>

    <span data-ttu-id="19dd4-251">*Internet Explorer - atualizado de cor de borda*</span><span class="sxs-lookup"><span data-stu-id="19dd4-251">*Internet Explorer - Border color updated*</span></span>
10. <span data-ttu-id="19dd4-252">Alterne para o Google Chrome e observe como a cor da borda foi alterado.</span><span class="sxs-lookup"><span data-stu-id="19dd4-252">Switch to Google Chrome and notice how the color of the border has changed.</span></span>

    <span data-ttu-id="19dd4-253">![Google Chrome - atualizado de cor de borda](visual-studio-2013-web-tools/_static/image20.png "Google Chrome - atualizado de cor de borda")</span><span class="sxs-lookup"><span data-stu-id="19dd4-253">![Google Chrome - Border color updated](visual-studio-2013-web-tools/_static/image20.png "Google Chrome - Border color updated")</span></span>

    <span data-ttu-id="19dd4-254">*Google Chrome - atualizado de cor de borda*</span><span class="sxs-lookup"><span data-stu-id="19dd4-254">*Google Chrome - Border color updated*</span></span>
11. <span data-ttu-id="19dd4-255">Alterne para o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="19dd4-255">Switch back to Visual Studio.</span></span>
12. <span data-ttu-id="19dd4-256">Vá até o final do **Site.css** e pressione **CTRL** + **F** para localizar o **.btn** seletor.</span><span class="sxs-lookup"><span data-stu-id="19dd4-256">Go to the end of the **Site.css** file and press **CTRL** + **F** to locate the **.btn** selector.</span></span>
13. <span data-ttu-id="19dd4-257">Observe que o **- webkit-border-radius** propriedade está sublinhada em verde.</span><span class="sxs-lookup"><span data-stu-id="19dd4-257">Notice that the **-webkit-border-radius** property is underlined in green.</span></span>

    <span data-ttu-id="19dd4-258">![propriedade - webkit-border-radius do seletor btn](visual-studio-2013-web-tools/_static/image21.png "- webkit-border-radius propriedade do seletor btn")</span><span class="sxs-lookup"><span data-stu-id="19dd4-258">![-webkit-border-radius property of the btn selector](visual-studio-2013-web-tools/_static/image21.png "-webkit-border-radius property of the btn selector")</span></span>

    <span data-ttu-id="19dd4-259">*propriedade - webkit-border-radius do seletor btn*</span><span class="sxs-lookup"><span data-stu-id="19dd4-259">*-webkit-border-radius property of the btn selector*</span></span>
14. <span data-ttu-id="19dd4-260">Coloque o cursor no **- webkit-border-radius** propriedade.</span><span class="sxs-lookup"><span data-stu-id="19dd4-260">Place the caret in the **-webkit-border-radius** property.</span></span> <span data-ttu-id="19dd4-261">Uma linha azul deve aparecer sob a primeira letra da primeira palavra da propriedade.</span><span class="sxs-lookup"><span data-stu-id="19dd4-261">A blue line should appear under the first letter of the first word of the property.</span></span> <span data-ttu-id="19dd4-262">Este é o **marca inteligente**.</span><span class="sxs-lookup"><span data-stu-id="19dd4-262">This is the **smart tag**.</span></span>
15. <span data-ttu-id="19dd4-263">Pressione **CTRL** + **.**</span><span class="sxs-lookup"><span data-stu-id="19dd4-263">Press **CTRL** + **.**</span></span> <span data-ttu-id="19dd4-264">Para abrir o menu de sugestões e clique em **adicionar (border-radius) de propriedade padrão ausente**.</span><span class="sxs-lookup"><span data-stu-id="19dd4-264">to open the suggestions menu and click **Add missing standard property (border-radius)**.</span></span>

    <span data-ttu-id="19dd4-265">![Adicionar ausente sugestão de propriedade padrão](visual-studio-2013-web-tools/_static/image22.png "adicionar ausente sugestão de propriedade padrão")</span><span class="sxs-lookup"><span data-stu-id="19dd4-265">![Add missing standard property suggestion](visual-studio-2013-web-tools/_static/image22.png "Add missing standard property suggestion")</span></span>

    <span data-ttu-id="19dd4-266">*Adicionar ausentes sugestão de propriedade padrão*</span><span class="sxs-lookup"><span data-stu-id="19dd4-266">*Add missing standard property suggestion*</span></span>
16. <span data-ttu-id="19dd4-267">O **border-radius** propriedade é adicionada automaticamente à regra de CSS.</span><span class="sxs-lookup"><span data-stu-id="19dd4-267">The **border-radius** property is automatically added to the CSS rule.</span></span>

    <span data-ttu-id="19dd4-268">![Não tem propriedade padrão adicionada](visual-studio-2013-web-tools/_static/image23.png "adicionada de propriedade padrão ausente")</span><span class="sxs-lookup"><span data-stu-id="19dd4-268">![Missing standard property added](visual-studio-2013-web-tools/_static/image23.png "Missing standard property added")</span></span>

    <span data-ttu-id="19dd4-269">*Propriedade padrão adicionada ausente*</span><span class="sxs-lookup"><span data-stu-id="19dd4-269">*Missing standard property added*</span></span>
17. <span data-ttu-id="19dd4-270">Mova o ponteiro sobre o **border-radius** propriedade para exibir o **dica de ferramenta do navegador matriz**.</span><span class="sxs-lookup"><span data-stu-id="19dd4-270">Move the pointer over the **border-radius** property to display the **Browser matrix tooltip**.</span></span> <span data-ttu-id="19dd4-271">O **dica de ferramenta do navegador matriz** mostra a disponibilidade da propriedade em cada navegador.</span><span class="sxs-lookup"><span data-stu-id="19dd4-271">The **Browser matrix tooltip** shows the availability of the property in each browser.</span></span>

    <span data-ttu-id="19dd4-272">![Dica de ferramenta do navegador matriz](visual-studio-2013-web-tools/_static/image24.png "dica de ferramenta de matriz de navegador")</span><span class="sxs-lookup"><span data-stu-id="19dd4-272">![Browser matrix tooltip](visual-studio-2013-web-tools/_static/image24.png "Browser matrix tooltip")</span></span>

    <span data-ttu-id="19dd4-273">*Dica de ferramenta de matriz de navegador*</span><span class="sxs-lookup"><span data-stu-id="19dd4-273">*Browser matrix tooltip*</span></span>
18. <span data-ttu-id="19dd4-274">Observe que o valor de **border-radius** propriedade está sublinhada ainda.</span><span class="sxs-lookup"><span data-stu-id="19dd4-274">Notice that the value of the **border-radius** property is still underlined.</span></span> <span data-ttu-id="19dd4-275">Mova o ponteiro sobre o valor para ver a mensagem de aviso.</span><span class="sxs-lookup"><span data-stu-id="19dd4-275">Move the pointer over the value to see the warning message.</span></span>

    <span data-ttu-id="19dd4-276">![Aviso de valor de propriedade Border-radius](visual-studio-2013-web-tools/_static/image25.png "aviso de valor de propriedade Border-radius")</span><span class="sxs-lookup"><span data-stu-id="19dd4-276">![Border-radius property value warning](visual-studio-2013-web-tools/_static/image25.png "Border-radius property value warning")</span></span>

    <span data-ttu-id="19dd4-277">*Aviso de valor de propriedade Border-radius*</span><span class="sxs-lookup"><span data-stu-id="19dd4-277">*Border-radius property value warning*</span></span>
19. <span data-ttu-id="19dd4-278">Remova a unidade da **border-radius** valor da propriedade conforme sugerido pela dica de ferramenta.</span><span class="sxs-lookup"><span data-stu-id="19dd4-278">Remove the unit of the **border-radius** property value as suggested by the tooltip.</span></span>
20. <span data-ttu-id="19dd4-279">Como **border-radius** é a propriedade padrão para a definição de borda como arredondada cantos estiverem, você pode remover o **- webkit-border-radius** propriedade e o valor da regra de CSS.</span><span class="sxs-lookup"><span data-stu-id="19dd4-279">As **border-radius** is the standard property for defining how rounded border corners are, you can remove the **-webkit-border-radius** property and value from the CSS rule.</span></span>
21. <span data-ttu-id="19dd4-280">Coloque o cursor no **quebra** propriedade e observe que a marca inteligente também aparece abaixo.</span><span class="sxs-lookup"><span data-stu-id="19dd4-280">Place the caret in the **word-wrap** property and notice that the smart tag also appears below.</span></span>
22. <span data-ttu-id="19dd4-281">Abra o menu e clique em **adicionar informações específicas de fornecedor ausente**.</span><span class="sxs-lookup"><span data-stu-id="19dd4-281">Open the menu and click **Add missing vendor specifics**.</span></span>

    <span data-ttu-id="19dd4-282">![Adicionar ausente sugestão de informações específicas de fornecedor](visual-studio-2013-web-tools/_static/image26.png "adicionar ausente sugestão de informações específicas de fornecedor")</span><span class="sxs-lookup"><span data-stu-id="19dd4-282">![Add missing vendor specifics suggestion](visual-studio-2013-web-tools/_static/image26.png "Add missing vendor specifics suggestion")</span></span>

    <span data-ttu-id="19dd4-283">*Adicionar ausente sugestão de informações específicas de fornecedor*</span><span class="sxs-lookup"><span data-stu-id="19dd4-283">*Add missing vendor specifics suggestion*</span></span>
23. <span data-ttu-id="19dd4-284">O **-ms-word-wrap** propriedade é adicionada automaticamente à regra de CSS.</span><span class="sxs-lookup"><span data-stu-id="19dd4-284">The **-ms-word-wrap** property is automatically added to the CSS rule.</span></span>

    <span data-ttu-id="19dd4-285">![Propriedade específica do fornecedor adicionada](visual-studio-2013-web-tools/_static/image27.png "adicionada de propriedade específica do fornecedor")</span><span class="sxs-lookup"><span data-stu-id="19dd4-285">![Vendor specific property added](visual-studio-2013-web-tools/_static/image27.png "Vendor specific property added")</span></span>

    <span data-ttu-id="19dd4-286">*Propriedade específica do fornecedor adicionada*</span><span class="sxs-lookup"><span data-stu-id="19dd4-286">*Vendor specific property added*</span></span>

<a id="Ex1Task4"></a>
#### <a name="task-4---updating-the-html-code-from-the-browser"></a><span data-ttu-id="19dd4-287">Tarefa 4 - atualizando o código HTML a partir do navegador</span><span class="sxs-lookup"><span data-stu-id="19dd4-287">Task 4 - Updating the HTML Code from the Browser</span></span>

<span data-ttu-id="19dd4-288">Nesta tarefa, você usará o Link de navegador **modo de Design** recurso para editar o objeto DOM do navegador e transferir as alterações no arquivo de origem HTML no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="19dd4-288">In this task, you will use the Browser Link's **Design Mode** feature to edit the DOM object from the browser and transfer the changes to the HTML source file in Visual Studio.</span></span>

1. <span data-ttu-id="19dd4-289">No Google Chrome, pressione **CTRL** + **ALT** + **D** para habilitar o modo de Design.</span><span class="sxs-lookup"><span data-stu-id="19dd4-289">In Google Chrome, press **CTRL** + **ALT** + **D** to enable Design Mode.</span></span>
2. <span data-ttu-id="19dd4-290">Mova o ponteiro sobre o **Lorem Ipsum dolor sit amet** rótulo e clique em.</span><span class="sxs-lookup"><span data-stu-id="19dd4-290">Move the pointer over the **Lorem Ipsum dolor sit amet** label and click.</span></span>

    <span data-ttu-id="19dd4-291">![Editando a pergunta](visual-studio-2013-web-tools/_static/image28.png "editando a pergunta")</span><span class="sxs-lookup"><span data-stu-id="19dd4-291">![Editing the question](visual-studio-2013-web-tools/_static/image28.png "Editing the question")</span></span>

    <span data-ttu-id="19dd4-292">*Editar a pergunta*</span><span class="sxs-lookup"><span data-stu-id="19dd4-292">*Editing the question*</span></span>
3. <span data-ttu-id="19dd4-293">Um cursor deve aparecer.</span><span class="sxs-lookup"><span data-stu-id="19dd4-293">A cursor should appear.</span></span> <span data-ttu-id="19dd4-294">Substitua o texto original por *o que é ele a aparência quando escrever uma pergunta mais?* e, em seguida, pressione **ESC** para sair do modo de Design.</span><span class="sxs-lookup"><span data-stu-id="19dd4-294">Replace the original text with *What does it look like when I write a longer question?*, and then press **ESC** to exit Design Mode.</span></span>

    <span data-ttu-id="19dd4-295">![Pergunta editada](visual-studio-2013-web-tools/_static/image29.png "pergunta editada")</span><span class="sxs-lookup"><span data-stu-id="19dd4-295">![Question edited](visual-studio-2013-web-tools/_static/image29.png "Question edited")</span></span>

    <span data-ttu-id="19dd4-296">*Pergunta editada*</span><span class="sxs-lookup"><span data-stu-id="19dd4-296">*Question edited*</span></span>
4. <span data-ttu-id="19dd4-297">Alternar para o Visual Studio e abra **cshtml**, se ainda não estiver aberto.</span><span class="sxs-lookup"><span data-stu-id="19dd4-297">Switch back to Visual Studio and open **Index.cshtml**, if not already opened.</span></span> <span data-ttu-id="19dd4-298">Observe que o texto interno do **&lt;p&gt;** elemento foi atualizado.</span><span class="sxs-lookup"><span data-stu-id="19dd4-298">Notice that the inner text of the **&lt;p&gt;** element has been updated.</span></span>

    <span data-ttu-id="19dd4-299">![Pergunta atualizadas na página HTML](visual-studio-2013-web-tools/_static/image30.png "pergunta atualizadas na página HTML")</span><span class="sxs-lookup"><span data-stu-id="19dd4-299">![Updated question in the HTML page](visual-studio-2013-web-tools/_static/image30.png "Updated question in the HTML page")</span></span>

    <span data-ttu-id="19dd4-300">*Pergunta atualizada na página HTML*</span><span class="sxs-lookup"><span data-stu-id="19dd4-300">*Updated question in the HTML page*</span></span>

<a id="Ex1Task5"></a>
#### <a name="task-5---reviewing-seo-related-warnings"></a><span data-ttu-id="19dd4-301">Tarefa 5: SEO revisão relacionada avisos</span><span class="sxs-lookup"><span data-stu-id="19dd4-301">Task 5 - Reviewing SEO Related Warnings</span></span>

<span data-ttu-id="19dd4-302">**Otimização do mecanismo de pesquisa** (SEO) é o processo de criação de uma classificação de site superior na lista do mecanismo de pesquisa de resultados.</span><span class="sxs-lookup"><span data-stu-id="19dd4-302">**Search Engine Optimization** (SEO) is the process of making a website rank higher on a search engine's list of results.</span></span> <span data-ttu-id="19dd4-303">Quanto maior o site classifica e mais consistentemente estiver listado, mais visitantes do site obterá desse mecanismo de pesquisa.</span><span class="sxs-lookup"><span data-stu-id="19dd4-303">The higher the site ranks and the more consistently it is listed, the more visitors the site will get from that search engine.</span></span> <span data-ttu-id="19dd4-304">Web Essentials incorpora uma ferramenta de análise que examina o HTML, relatórios de problemas encontrados e fornece assistência para corrigi-los.</span><span class="sxs-lookup"><span data-stu-id="19dd4-304">Web Essentials incorporates an analytical tool that examines HTML, reports the issues found and provides assistance to fix them.</span></span>

1. <span data-ttu-id="19dd4-305">Vá para o **exibição** menu e clique em **lista de erros** para abrir o **lista de erros** janela.</span><span class="sxs-lookup"><span data-stu-id="19dd4-305">Go to the **View** menu and click **Error List** to open the **Error List** window.</span></span>

    <span data-ttu-id="19dd4-306">![Menu de modo de exibição de lista de erros](visual-studio-2013-web-tools/_static/image31.png "lista de erros no menu Exibir")</span><span class="sxs-lookup"><span data-stu-id="19dd4-306">![Error List in View menu](visual-studio-2013-web-tools/_static/image31.png "Error List in View menu")</span></span>

    <span data-ttu-id="19dd4-307">*Menu de modo de exibição de lista de erros*</span><span class="sxs-lookup"><span data-stu-id="19dd4-307">*Error List in View menu*</span></span>
2. <span data-ttu-id="19dd4-308">Observe que há um aviso de SEO notificando que um **&lt;meta&gt;** marca para a descrição da página está ausente.</span><span class="sxs-lookup"><span data-stu-id="19dd4-308">Notice that there is an SEO warning notifying that a **&lt;meta&gt;** tag for the page description is missing.</span></span> <span data-ttu-id="19dd4-309">Clique duas vezes na entrada de aviso de SEO para corrigi-lo.</span><span class="sxs-lookup"><span data-stu-id="19dd4-309">Double-click the SEO warning entry to fix it.</span></span>

    <span data-ttu-id="19dd4-310">![Janela lista de erros](visual-studio-2013-web-tools/_static/image32.png "janela lista de erros")</span><span class="sxs-lookup"><span data-stu-id="19dd4-310">![Error List window](visual-studio-2013-web-tools/_static/image32.png "Error List window")</span></span>

    <span data-ttu-id="19dd4-311">*Janela lista de erros*</span><span class="sxs-lookup"><span data-stu-id="19dd4-311">*Error List window*</span></span>
3. <span data-ttu-id="19dd4-312">No **Web Essentials** caixa de diálogo, clique em **Sim** para inserir uma descrição &lt;meta&gt; marca.</span><span class="sxs-lookup"><span data-stu-id="19dd4-312">In the **Web Essentials** dialog box, click **Yes** to insert a description &lt;meta&gt; tag.</span></span>

    <span data-ttu-id="19dd4-313">![Caixa de diálogo do Web Essentials](visual-studio-2013-web-tools/_static/image33.png "caixa de diálogo Web Essentials")</span><span class="sxs-lookup"><span data-stu-id="19dd4-313">![Web Essentials dialog box](visual-studio-2013-web-tools/_static/image33.png "Web Essentials dialog box")</span></span>

    <span data-ttu-id="19dd4-314">*Caixa de diálogo do Web Essentials*</span><span class="sxs-lookup"><span data-stu-id="19dd4-314">*Web Essentials dialog box*</span></span>
4. <span data-ttu-id="19dd4-315">O editor de  **\_cshtml** abre e **&lt;meta&gt;** marca será adicionada automaticamente ao **head** seção o Arquivo HTML.</span><span class="sxs-lookup"><span data-stu-id="19dd4-315">The editor for **\_Layout.cshtml** opens and the **&lt;meta&gt;** tag is automatically added to the **head** section of the HTML file.</span></span>

    <span data-ttu-id="19dd4-316">![Marca meta adicionada automaticamente na página layout](visual-studio-2013-web-tools/_static/image34.png "MetaTag adicionado automaticamente no layout página")</span><span class="sxs-lookup"><span data-stu-id="19dd4-316">![Meta tag automatically added in _Layout page](visual-studio-2013-web-tools/_static/image34.png "Meta tag automatically added in _Layout page")</span></span>

    <span data-ttu-id="19dd4-317">*Marca meta adicionada automaticamente à \_página de Layout*</span><span class="sxs-lookup"><span data-stu-id="19dd4-317">*Meta tag automatically added to \_Layout page*</span></span>
5. <span data-ttu-id="19dd4-318">Alterar o valor da **conteúdo** atributo *GeekQuiz* e salve o arquivo.</span><span class="sxs-lookup"><span data-stu-id="19dd4-318">Change the value of the **content** attribute to *GeekQuiz* and save the file.</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-taking-advantage-of-code-snippets-and-intellisense"></a><span data-ttu-id="19dd4-319">Exercício 2: Aproveitando as vantagens de trechos de código e o IntelliSense</span><span class="sxs-lookup"><span data-stu-id="19dd4-319">Exercise 2: Taking Advantage of Code Snippets and IntelliSense</span></span>

<span data-ttu-id="19dd4-320">Com o Web Essentials, o editor de HTML foi estendido com funcionalidade adicional.</span><span class="sxs-lookup"><span data-stu-id="19dd4-320">With Web Essentials, the HTML editor has been extended with extra functionality.</span></span> <span data-ttu-id="19dd4-321">Neste exercício, você verá alguns novos recursos que são úteis ao desenvolver aplicativos da web.</span><span class="sxs-lookup"><span data-stu-id="19dd4-321">In this exercise, you will see some new features that are helpful when developing web applications.</span></span>

<a id="Ex2Task1"></a>
#### <a name="task-1---using-intellisense-in-html-documents"></a><span data-ttu-id="19dd4-322">Tarefa 1: usando o IntelliSense em documentos HTML</span><span class="sxs-lookup"><span data-stu-id="19dd4-322">Task 1 - Using IntelliSense in HTML Documents</span></span>

<span data-ttu-id="19dd4-323">O novo recurso primeiro, você verá essa tarefa é chamado **IntelliSense dinâmico**.</span><span class="sxs-lookup"><span data-stu-id="19dd4-323">The first new feature you will see in this task is called **Dynamic IntelliSense**.</span></span> <span data-ttu-id="19dd4-324">IntelliSense dinâmico lê outras marcas e atributos para deduzir as possíveis ids que você usará.</span><span class="sxs-lookup"><span data-stu-id="19dd4-324">Dynamic IntelliSense reads other tags and attributes to infer the possible ids you will use.</span></span>

<span data-ttu-id="19dd4-325">Nesta tarefa, você criará um novo elemento de formulário HTML que contém um rótulo e um campo de entrada.</span><span class="sxs-lookup"><span data-stu-id="19dd4-325">In this task, you will create a new HTML form element which contains a label and an input field.</span></span> <span data-ttu-id="19dd4-326">Em seguida, você adicionará um **para** de atributo para o rótulo para associá-lo para a entrada, e você verá o IntelliSense sugestões com base nas ids das entradas no escopo.</span><span class="sxs-lookup"><span data-stu-id="19dd4-326">Then you will add a **for** attribute to the label to bind it to the input, and you will see IntelliSense suggestions based on the ids of the inputs in scope.</span></span>

1. <span data-ttu-id="19dd4-327">Abra **Visual Studio Express 2013 para Web** e **Begin.sln** solução localizada no **fonte/o Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/início** pasta.</span><span class="sxs-lookup"><span data-stu-id="19dd4-327">Open **Visual Studio Express 2013 for Web** and the **Begin.sln** solution located in the **Source/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/Begin** folder.</span></span> <span data-ttu-id="19dd4-328">Como alternativa, você pode continuar com a solução que você obteve no exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="19dd4-328">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>
2. <span data-ttu-id="19dd4-329">Em **Solution Explorer**, abra o **cshtml** arquivo localizado no **exibições** | **início** pasta.</span><span class="sxs-lookup"><span data-stu-id="19dd4-329">In **Solution Explorer**, open the **Index.cshtml** file located in the **Views** | **Home** folder.</span></span>
3. <span data-ttu-id="19dd4-330">Adicione o seguinte formato dentro de **&lt;seção&gt;** elemento.</span><span class="sxs-lookup"><span data-stu-id="19dd4-330">Add the following form inside the **&lt;section&gt;** element.</span></span>

    <span data-ttu-id="19dd4-331">(Código de trecho - *VisualStudio2013WebTooling* - *o Ex2* - *formulário*)</span><span class="sxs-lookup"><span data-stu-id="19dd4-331">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *Form*)</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample4.html)]
4. <span data-ttu-id="19dd4-332">A marca de entrada deve ser precedida por um rótulo com uma descrição do campo.</span><span class="sxs-lookup"><span data-stu-id="19dd4-332">The input tag should be preceded by a label with some description of the field.</span></span> <span data-ttu-id="19dd4-333">Adicione o seguinte rótulo antes da marca de entrada.</span><span class="sxs-lookup"><span data-stu-id="19dd4-333">Add the following label before the input tag.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample5.html)]
5. <span data-ttu-id="19dd4-334">O **para** atributo de um **&lt;rótulo&gt;** Especifica qual elemento de formulário de um rótulo está associado.</span><span class="sxs-lookup"><span data-stu-id="19dd4-334">The **for** attribute of a **&lt;label&gt;** specifies which form element a label is bound to.</span></span> <span data-ttu-id="19dd4-335">O valor do atributo deve ser igual à id do elemento relacionado.</span><span class="sxs-lookup"><span data-stu-id="19dd4-335">The attribute's value should be equal to the id of the related element.</span></span> <span data-ttu-id="19dd4-336">Adicionar o **para** de atributo para o **&lt;rótulo&gt;** elemento.</span><span class="sxs-lookup"><span data-stu-id="19dd4-336">Add the **for** attribute to the **&lt;label&gt;** element.</span></span> <span data-ttu-id="19dd4-337">Conforme mostrado na figura a seguir, o &quot;nome&quot; valor exibido na caixa do IntelliSense, com base na id dos elementos dentro do mesmo escopo (circunscrição  **&lt;formulário&gt;**).</span><span class="sxs-lookup"><span data-stu-id="19dd4-337">As shown in the following figure, the &quot;name&quot; value pops up in the IntelliSense box, based on the id of the elements within the same scope (the enclosing **&lt;form&gt;**).</span></span>

    <span data-ttu-id="19dd4-338">![Mostrando a id do IntelliSense](visual-studio-2013-web-tools/_static/image35.png "mostrando a id do IntelliSense")</span><span class="sxs-lookup"><span data-stu-id="19dd4-338">![Showing the id in IntelliSense](visual-studio-2013-web-tools/_static/image35.png "Showing the id in IntelliSense")</span></span>

    <span data-ttu-id="19dd4-339">*Mostrando a id do IntelliSense*</span><span class="sxs-lookup"><span data-stu-id="19dd4-339">*Showing the id in IntelliSense*</span></span>
6. <span data-ttu-id="19dd4-340">Excluir adicionado recentemente **&lt;formulário&gt;** elemento e seu conteúdo.</span><span class="sxs-lookup"><span data-stu-id="19dd4-340">Delete the recently added **&lt;form&gt;** element and its content.</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2---using-html-code-snippets"></a><span data-ttu-id="19dd4-341">Tarefa 2: usando trechos de código HTML</span><span class="sxs-lookup"><span data-stu-id="19dd4-341">Task 2 - Using HTML Code Snippets</span></span>

<span data-ttu-id="19dd4-342">O HTML5 introduziu mais de 25 novas marcas semânticas.</span><span class="sxs-lookup"><span data-stu-id="19dd4-342">HTML5 introduced more than 25 new semantic tags.</span></span> <span data-ttu-id="19dd4-343">Visual Studio já tinha suporte do IntelliSense para essas marcas, mas o Visual Studio 2013 torna mais rápido e mais fácil de escrever marcação adicionando novos trechos de código.</span><span class="sxs-lookup"><span data-stu-id="19dd4-343">Visual Studio already had IntelliSense support for these tags, but Visual Studio 2013 makes it faster and easier to write markup by adding new code snippets.</span></span> <span data-ttu-id="19dd4-344">Embora essas marcas não sejam complicadas, elas apresentam algumas pequenas sutilezas, como a adição de fallbacks de codec corretos para o *áudio* marca.</span><span class="sxs-lookup"><span data-stu-id="19dd4-344">Though these tags are not complicated, they come with a few small subtleties, such as adding the correct codec fallbacks for the *audio* tag.</span></span> <span data-ttu-id="19dd4-345">Nesta tarefa, você verá os trechos de código HTML para a marca de áudio.</span><span class="sxs-lookup"><span data-stu-id="19dd4-345">In this task, you will see the HTML code snippets for the audio tag.</span></span>

1. <span data-ttu-id="19dd4-346">No **cshtml** de arquivos, digite  **&lt;aud** dentro de **&lt;seção&gt;** elemento conforme mostrado na figura a seguir.</span><span class="sxs-lookup"><span data-stu-id="19dd4-346">In the **Index.cshtml** file, type **&lt;aud** inside the **&lt;section&gt;** element as shown in the following figure.</span></span>

    <span data-ttu-id="19dd4-347">![Inserindo um elemento audio](visual-studio-2013-web-tools/_static/image36.png "inserindo um elemento de áudio")</span><span class="sxs-lookup"><span data-stu-id="19dd4-347">![Inserting an audio element](visual-studio-2013-web-tools/_static/image36.png "Inserting an audio element")</span></span>

    <span data-ttu-id="19dd4-348">*Inserindo um elemento de áudio*</span><span class="sxs-lookup"><span data-stu-id="19dd4-348">*Inserting an audio element*</span></span>
2. <span data-ttu-id="19dd4-349">Pressione **guia** duas vezes e observe como o código a seguir é adicionado na página e o cursor é colocado no **src** atributo da primeira fonte.</span><span class="sxs-lookup"><span data-stu-id="19dd4-349">Press **TAB** twice and notice how the following code is added on the page and the cursor is placed on the **src** attribute of the first source.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample6.html)]

    > [!NOTE]
    > <span data-ttu-id="19dd4-350">Pressionando o **guia** chave duas vezes, o trecho de código é inserido.</span><span class="sxs-lookup"><span data-stu-id="19dd4-350">By pressing the **TAB** key twice, the code snippet is inserted.</span></span> <span data-ttu-id="19dd4-351">O trecho de áudio mostra o uso padrão da *áudio* marca, com dois arquivos de origem para o suporte aprimorado.</span><span class="sxs-lookup"><span data-stu-id="19dd4-351">The audio snippet shows the standard usage of the *audio* tag, with two source files for improved support.</span></span>
3. <span data-ttu-id="19dd4-352">Exclua a segunda linha e atualizar a fonte da primeira linha com o seguinte link para a apresentação de WebCampsTV Katana: [ http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3 ](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3).</span><span class="sxs-lookup"><span data-stu-id="19dd4-352">Delete the second line and update the source of the first line with the following link to the WebCampsTV Katana show: [http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3).</span></span> <span data-ttu-id="19dd4-353">O código resultante é mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="19dd4-353">The resulting code is shown below.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample7.html)]

    > [!NOTE]
    > <span data-ttu-id="19dd4-354">O arquivo de origem *KatanaProject.mp3* é usado como um exemplo.</span><span class="sxs-lookup"><span data-stu-id="19dd4-354">The source file *KatanaProject.mp3* is used as an example.</span></span> <span data-ttu-id="19dd4-355">Se preferir, você pode usar outra fonte.</span><span class="sxs-lookup"><span data-stu-id="19dd4-355">You can use another source if you prefer.</span></span>
4. <span data-ttu-id="19dd4-356">Pressione **CTRL** + **S** para salvar o arquivo.</span><span class="sxs-lookup"><span data-stu-id="19dd4-356">Press **CTRL** + **S** to save the file.</span></span>
5. <span data-ttu-id="19dd4-357">Pressione **CTRL** + **F5** para iniciar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="19dd4-357">Press **CTRL** + **F5** to start the application.</span></span>
6. <span data-ttu-id="19dd4-358">Observe que um player de áudio foi adicionado ao aplicativo.</span><span class="sxs-lookup"><span data-stu-id="19dd4-358">Notice that an audio player was added to the application.</span></span>

    <span data-ttu-id="19dd4-359">![Player de áudio no Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "player de áudio no Internet Explorer")</span><span class="sxs-lookup"><span data-stu-id="19dd4-359">![Audio player in Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "Audio player in Internet Explorer")</span></span>

    <span data-ttu-id="19dd4-360">*Player de áudio no Internet Explorer*</span><span class="sxs-lookup"><span data-stu-id="19dd4-360">*Audio player in Internet Explorer*</span></span>

    <span data-ttu-id="19dd4-361">![Player de áudio no Google Chrome](visual-studio-2013-web-tools/_static/image38.png "player de áudio no Google Chrome")</span><span class="sxs-lookup"><span data-stu-id="19dd4-361">![Audio player in Google Chrome](visual-studio-2013-web-tools/_static/image38.png "Audio player in Google Chrome")</span></span>

    <span data-ttu-id="19dd4-362">*Player de áudio no Google Chrome*</span><span class="sxs-lookup"><span data-stu-id="19dd4-362">*Audio player in Google Chrome*</span></span>
7. <span data-ttu-id="19dd4-363">Não feche os navegadores.</span><span class="sxs-lookup"><span data-stu-id="19dd4-363">Do not close the browsers.</span></span> <span data-ttu-id="19dd4-364">Você irá usá-los na próxima tarefa.</span><span class="sxs-lookup"><span data-stu-id="19dd4-364">You will use them in the next task.</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3---using-intellisense-in-javascript-documents"></a><span data-ttu-id="19dd4-365">Tarefa 3: usando o IntelliSense em documentos de JavaScript</span><span class="sxs-lookup"><span data-stu-id="19dd4-365">Task 3 - Using IntelliSense in JavaScript Documents</span></span>

<span data-ttu-id="19dd4-366">Com o Web Essentials 2013, folhas de estilo e páginas HTML produzem uma lista de IDs e nomes de classe.</span><span class="sxs-lookup"><span data-stu-id="19dd4-366">With Web Essentials 2013, style sheets and HTML pages produce a list of IDs and class names.</span></span> <span data-ttu-id="19dd4-367">Nesta tarefa, você aprenderá como essas listas de melhoram o suporte a JavaScript IntelliSense na Web Essentials 2013.</span><span class="sxs-lookup"><span data-stu-id="19dd4-367">In this task, you will learn how those lists improve JavaScript IntelliSense support in Web Essentials 2013.</span></span>

1. <span data-ttu-id="19dd4-368">No **cshtml** de arquivo, adicione o código a seguir para definir um **script** marca para o código JavaScript.</span><span class="sxs-lookup"><span data-stu-id="19dd4-368">In the **Index.cshtml** file, add the following code to define a **script** tag for JavaScript code.</span></span>

    [!code-cshtml[Main](visual-studio-2013-web-tools/samples/sample8.cshtml)]
2. <span data-ttu-id="19dd4-369">Adicione o seguinte código dentro de **script** tag para definir a função de retorno de chamada pronto.</span><span class="sxs-lookup"><span data-stu-id="19dd4-369">Add the following code inside the **script** tag to define the ready callback function.</span></span>

    <span data-ttu-id="19dd4-370">(Código de trecho - *VisualStudio2013WebTooling* - *o Ex2* - *ReadyFunction*)</span><span class="sxs-lookup"><span data-stu-id="19dd4-370">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample9.js)]
3. <span data-ttu-id="19dd4-371">Coloque o cursor no **script** marca e pressione **CTRL** + **.**</span><span class="sxs-lookup"><span data-stu-id="19dd4-371">Place the caret in the **script** tag and press **CTRL** + **.**</span></span> <span data-ttu-id="19dd4-372">Para abrir o menu de sugestão.</span><span class="sxs-lookup"><span data-stu-id="19dd4-372">to open the suggestion menu.</span></span>
4. <span data-ttu-id="19dd4-373">Clique em **extrair para o arquivo**.</span><span class="sxs-lookup"><span data-stu-id="19dd4-373">Click **Extract To File**.</span></span>

    <span data-ttu-id="19dd4-374">![Extraia o JavaScript para sugestão de arquivo](visual-studio-2013-web-tools/_static/image39.png "JavaScript extrair a sugestão de arquivo")</span><span class="sxs-lookup"><span data-stu-id="19dd4-374">![JavaScript extract to file suggestion](visual-studio-2013-web-tools/_static/image39.png "JavaScript extract to file suggestion")</span></span>

    <span data-ttu-id="19dd4-375">*Extraia o JavaScript para sugestão de arquivo*</span><span class="sxs-lookup"><span data-stu-id="19dd4-375">*JavaScript extract to file suggestion*</span></span>
5. <span data-ttu-id="19dd4-376">No **Salvar como** janela, selecione o **Scripts** pasta, nomeie o arquivo **init.js** e clique em **salvar**.</span><span class="sxs-lookup"><span data-stu-id="19dd4-376">In the **Save As** window, select the **Scripts** folder, name the file **init.js** and click **Save**.</span></span>

    <span data-ttu-id="19dd4-377">![Salvar como janela](visual-studio-2013-web-tools/_static/image40.png "janela Salvar como")</span><span class="sxs-lookup"><span data-stu-id="19dd4-377">![Save As window](visual-studio-2013-web-tools/_static/image40.png "Save As window")</span></span>

    <span data-ttu-id="19dd4-378">*Janela Salvar como*</span><span class="sxs-lookup"><span data-stu-id="19dd4-378">*Save As window*</span></span>

    > [!NOTE]
    > <span data-ttu-id="19dd4-379">O **init.js** arquivo é criado e o conteúdo do script é movido para o arquivo.</span><span class="sxs-lookup"><span data-stu-id="19dd4-379">The **init.js** file is created and the content of the script is moved to the file.</span></span>
    > 
    > <span data-ttu-id="19dd4-380">![Arquivo init.js criado com o conteúdo incluído](visual-studio-2013-web-tools/_static/image41.png "Init.js arquivo criado com o conteúdo incluído")</span><span class="sxs-lookup"><span data-stu-id="19dd4-380">![Init.js file created with the content included](visual-studio-2013-web-tools/_static/image41.png "Init.js file created with the content included")</span></span>
    > 
    > <span data-ttu-id="19dd4-381">*Arquivo init.js criado com o conteúdo incluído*</span><span class="sxs-lookup"><span data-stu-id="19dd4-381">*Init.js file created with the content included*</span></span>
6. <span data-ttu-id="19dd4-382">Abra o **cshtml** de arquivo e verifique que a marca de script foi substituída por uma referência para o **init.js** arquivo.</span><span class="sxs-lookup"><span data-stu-id="19dd4-382">Open the **Index.cshtml** file and check that the script tag was replaced with a reference to the **init.js** file.</span></span>

    <span data-ttu-id="19dd4-383">![Referência de html init.js](visual-studio-2013-web-tools/_static/image42.png "Init.js referência de html")</span><span class="sxs-lookup"><span data-stu-id="19dd4-383">![Init.js html reference](visual-studio-2013-web-tools/_static/image42.png "Init.js html reference")</span></span>

    <span data-ttu-id="19dd4-384">*Referência de html init.js*</span><span class="sxs-lookup"><span data-stu-id="19dd4-384">*Init.js html reference*</span></span>
7. <span data-ttu-id="19dd4-385">Vá para o **Solution Explorer** e observe que o **init.js** arquivo foi incluído automaticamente na solução.</span><span class="sxs-lookup"><span data-stu-id="19dd4-385">Go to the **Solution Explorer** and notice that the **init.js** file was included automatically in the solution.</span></span>

    <span data-ttu-id="19dd4-386">![Arquivo init.js incluído na solução](visual-studio-2013-web-tools/_static/image43.png "Init.js arquivos incluídos na solução")</span><span class="sxs-lookup"><span data-stu-id="19dd4-386">![Init.js file included in solution](visual-studio-2013-web-tools/_static/image43.png "Init.js file included in solution")</span></span>

    <span data-ttu-id="19dd4-387">*Arquivo init.js incluído na solução*</span><span class="sxs-lookup"><span data-stu-id="19dd4-387">*Init.js file included in solution*</span></span>
8. <span data-ttu-id="19dd4-388">Volte para o **init.js** arquivo para atualizar o **pronto** retorno de chamada de função.</span><span class="sxs-lookup"><span data-stu-id="19dd4-388">Switch back to the **init.js** file to update the **ready** function callback.</span></span>
9. <span data-ttu-id="19dd4-389">Dentro da definição de retorno de chamada de função que é passada para *pronto*, adicione o seguinte código para obter todos os elementos por um atributo de classe específica.</span><span class="sxs-lookup"><span data-stu-id="19dd4-389">Inside the function callback definition that is passed to *ready*, add the following code to get all the elements by a specific class attribute.</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample10.js)]
10. <span data-ttu-id="19dd4-390">Pressione **CTRL** + **espaço** entre as aspas dentro de **getElementsByClassName** chamada de função.</span><span class="sxs-lookup"><span data-stu-id="19dd4-390">Press **CTRL** + **Space** between the quotes inside the **getElementsByClassName** function call.</span></span>

    <span data-ttu-id="19dd4-391">![Mostrando o IntelliSense para a função getElementsByClassName](visual-studio-2013-web-tools/_static/image44.png "mostrando o IntelliSense para a função getElementsByClassName")</span><span class="sxs-lookup"><span data-stu-id="19dd4-391">![Showing IntelliSense for the getElementsByClassName function](visual-studio-2013-web-tools/_static/image44.png "Showing IntelliSense for the getElementsByClassName function")</span></span>

    <span data-ttu-id="19dd4-392">*Mostrando o IntelliSense para a função getElementsByClassName*</span><span class="sxs-lookup"><span data-stu-id="19dd4-392">*Showing IntelliSense for the getElementsByClassName function*</span></span>

    > [!NOTE]
    > <span data-ttu-id="19dd4-393">Observe que o IntelliSense mostra as classes definidas nas folhas de estilo de projeto.</span><span class="sxs-lookup"><span data-stu-id="19dd4-393">Notice that IntelliSense shows the classes defined in the project style sheets.</span></span>
11. <span data-ttu-id="19dd4-394">Substitua a linha que você criou com o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="19dd4-394">Replace the line that you have created with the following code.</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample11.js)]
12. <span data-ttu-id="19dd4-395">Posicione o cursor depois **Austrália** dentro de aspas no **getElementsByTagName** função e pressione **CTRL** + **espaço**.</span><span class="sxs-lookup"><span data-stu-id="19dd4-395">Position the cursor after **au** inside the quotes in the **getElementsByTagName** function and press **CTRL** + **Space**.</span></span>

    <span data-ttu-id="19dd4-396">![Mostrando o IntelliSense para o método getElementByTagName](visual-studio-2013-web-tools/_static/image45.png "mostrando o IntelliSense para o método getElementByTagName")</span><span class="sxs-lookup"><span data-stu-id="19dd4-396">![Showing IntelliSense for the getElementByTagName method](visual-studio-2013-web-tools/_static/image45.png "Showing IntelliSense for the getElementByTagName method")</span></span>

    <span data-ttu-id="19dd4-397">*Mostrando o IntelliSense para o método getElementsByTagName*</span><span class="sxs-lookup"><span data-stu-id="19dd4-397">*Showing IntelliSense for the getElementsByTagName method*</span></span>
13. <span data-ttu-id="19dd4-398">Selecione **&quot;áudio&quot;** na lista e pressione **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="19dd4-398">Select **&quot;audio&quot;** from the list and press **ENTER**.</span></span> <span data-ttu-id="19dd4-399">O resultado é mostrado na figura a seguir.</span><span class="sxs-lookup"><span data-stu-id="19dd4-399">The result is shown in the following figure.</span></span>

    <span data-ttu-id="19dd4-400">![Recuperando elementos áudio](visual-studio-2013-web-tools/_static/image46.png "recuperar os elementos de áudio")</span><span class="sxs-lookup"><span data-stu-id="19dd4-400">![Retrieving Audio Elements](visual-studio-2013-web-tools/_static/image46.png "Retrieving Audio Elements")</span></span>

    <span data-ttu-id="19dd4-401">*Recuperação de elementos de áudio*</span><span class="sxs-lookup"><span data-stu-id="19dd4-401">*Retrieving Audio Elements*</span></span>
14. <span data-ttu-id="19dd4-402">Em **Solution Explorer**, com o botão direito a **init.js** do arquivo no **Scripts** pasta e selecione **arquivos JavaScript Minificada** da **Web Essentials** menu.</span><span class="sxs-lookup"><span data-stu-id="19dd4-402">In **Solution Explorer**, right-click the **init.js** file in the **Scripts** folder and select **Minify JavaScript file(s)** from the **Web Essentials** menu.</span></span>

    <span data-ttu-id="19dd4-403">![Minificada arquivos JavaScript](visual-studio-2013-web-tools/_static/image47.png "arquivos JavaScript Minificada")</span><span class="sxs-lookup"><span data-stu-id="19dd4-403">![Minify JavaScript file(s)](visual-studio-2013-web-tools/_static/image47.png "Minify JavaScript files")</span></span>

    <span data-ttu-id="19dd4-404">*Minificada arquivos JavaScript*</span><span class="sxs-lookup"><span data-stu-id="19dd4-404">*Minify JavaScript file(s)*</span></span>
15. <span data-ttu-id="19dd4-405">Quando solicitado a habilitar minimização automática quando as alterações de arquivo de origem em **Sim**.</span><span class="sxs-lookup"><span data-stu-id="19dd4-405">When prompted to enable automatic minification when the source file changes click **Yes**.</span></span>

    <span data-ttu-id="19dd4-406">![Habilitar o aviso de minimização automático](visual-studio-2013-web-tools/_static/image48.png "habilitando aviso minimização automática")</span><span class="sxs-lookup"><span data-stu-id="19dd4-406">![Enabling automatic minification warning](visual-studio-2013-web-tools/_static/image48.png "Enabling automatic minification warning")</span></span>

    <span data-ttu-id="19dd4-407">*Habilitar o aviso de minimização automática*</span><span class="sxs-lookup"><span data-stu-id="19dd4-407">*Enabling automatic minification warning*</span></span>

    > [!NOTE]
    > <span data-ttu-id="19dd4-408">O **init.min.js** é criado e é adicionado como uma dependência da **init.js** arquivo.</span><span class="sxs-lookup"><span data-stu-id="19dd4-408">The **init.min.js** is created and is added as a dependency of the **init.js** file.</span></span>
    > 
    > <span data-ttu-id="19dd4-409">![Arquivo init.min.js criado](visual-studio-2013-web-tools/_static/image49.png "Init.min.js arquivo criado")</span><span class="sxs-lookup"><span data-stu-id="19dd4-409">![Init.min.js file created](visual-studio-2013-web-tools/_static/image49.png "Init.min.js file created")</span></span>
    > 
    > <span data-ttu-id="19dd4-410">*Arquivo init.min.js criado*</span><span class="sxs-lookup"><span data-stu-id="19dd4-410">*Init.min.js file created*</span></span>
16. <span data-ttu-id="19dd4-411">Abra o **init.min.js** de arquivo e observe que o arquivo é minimizado.</span><span class="sxs-lookup"><span data-stu-id="19dd4-411">Open the **init.min.js** file and notice that the file is minified.</span></span>

    <span data-ttu-id="19dd4-412">![O conteúdo do arquivo init.min.js](visual-studio-2013-web-tools/_static/image50.png "Init.min.js o conteúdo do arquivo")</span><span class="sxs-lookup"><span data-stu-id="19dd4-412">![Init.min.js file content](visual-studio-2013-web-tools/_static/image50.png "Init.min.js file content")</span></span>

    <span data-ttu-id="19dd4-413">*Conteúdo do arquivo init.min.js*</span><span class="sxs-lookup"><span data-stu-id="19dd4-413">*Init.min.js file content*</span></span>
17. <span data-ttu-id="19dd4-414">No **init.js** arquivo, adicione o seguinte código abaixo o **getElementsByTagName** chamada para executar todos os elementos de áudio de função.</span><span class="sxs-lookup"><span data-stu-id="19dd4-414">In the **init.js** file, add the following code below the **getElementsByTagName** function call to play all the audio elements.</span></span>

    <span data-ttu-id="19dd4-415">(Código de trecho - *VisualStudio2013WebTooling* - *o Ex2* - *PlayAudioElements*)</span><span class="sxs-lookup"><span data-stu-id="19dd4-415">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)</span></span>

    [!code-csharp[Main](visual-studio-2013-web-tools/samples/sample12.cs)]
18. <span data-ttu-id="19dd4-416">Clique em **CTRL** + **S** para salvar o arquivo.</span><span class="sxs-lookup"><span data-stu-id="19dd4-416">Click **CTRL** + **S** to save the file.</span></span> <span data-ttu-id="19dd4-417">Desde que o arquivo minimizado já está aberto, você verá uma caixa de diálogo informando que o arquivo foi modificado fora do editor de origem.</span><span class="sxs-lookup"><span data-stu-id="19dd4-417">Since the minified file is already opened, you will see a dialog box saying that the file was modified outside of the source editor.</span></span> <span data-ttu-id="19dd4-418">Clique em **Sim**.</span><span class="sxs-lookup"><span data-stu-id="19dd4-418">Click **Yes**.</span></span>

    <span data-ttu-id="19dd4-419">![Aviso do Microsoft Visual Studio](visual-studio-2013-web-tools/_static/image51.png "aviso do Microsoft Visual Studio")</span><span class="sxs-lookup"><span data-stu-id="19dd4-419">![Microsoft Visual Studio warning](visual-studio-2013-web-tools/_static/image51.png "Microsoft Visual Studio warning")</span></span>

    <span data-ttu-id="19dd4-420">*Aviso do Microsoft Visual Studio*</span><span class="sxs-lookup"><span data-stu-id="19dd4-420">*Microsoft Visual Studio warning*</span></span>
19. <span data-ttu-id="19dd4-421">Volte para o **init.min.js** arquivo para verificar se o arquivo foi atualizado com o novo código.</span><span class="sxs-lookup"><span data-stu-id="19dd4-421">Switch back to the **init.min.js** file to verify that the file was updated with the new code.</span></span>

    <span data-ttu-id="19dd4-422">![Arquivo init.min.js atualizado](visual-studio-2013-web-tools/_static/image52.png "Init.min.js arquivo atualizado")</span><span class="sxs-lookup"><span data-stu-id="19dd4-422">![Init.min.js file updated](visual-studio-2013-web-tools/_static/image52.png "Init.min.js file updated")</span></span>

    <span data-ttu-id="19dd4-423">*Arquivo init.min.js atualizado*</span><span class="sxs-lookup"><span data-stu-id="19dd4-423">*Init.min.js file updated*</span></span>
20. <span data-ttu-id="19dd4-424">Clique o **atualização de Link do navegador** botão.</span><span class="sxs-lookup"><span data-stu-id="19dd4-424">Click the **Browser Link Refresh** button.</span></span>
21. <span data-ttu-id="19dd4-425">Depois que ambos os navegadores são atualizados os players de áudio que na tarefa anterior, você viu execução serão iniciada automaticamente.</span><span class="sxs-lookup"><span data-stu-id="19dd4-425">Once both browsers are refreshed the audio players you saw in the previous task will start playing automatically.</span></span>

    <span data-ttu-id="19dd4-426">![Player de áudio incluído na exibição](visual-studio-2013-web-tools/_static/image53.png "incluído na exibição de player de áudio")</span><span class="sxs-lookup"><span data-stu-id="19dd4-426">![Audio player included in view](visual-studio-2013-web-tools/_static/image53.png "Audio player included in view")</span></span>

    <span data-ttu-id="19dd4-427">*Player de áudio incluído no modo de exibição*</span><span class="sxs-lookup"><span data-stu-id="19dd4-427">*Audio player included in view*</span></span>

* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="19dd4-428">Resumo</span><span class="sxs-lookup"><span data-stu-id="19dd4-428">Summary</span></span>

<span data-ttu-id="19dd4-429">Ao concluir este laboratório prático você aprendeu como:</span><span class="sxs-lookup"><span data-stu-id="19dd4-429">By completing this hands-on lab you have learned how to:</span></span>

- <span data-ttu-id="19dd4-430">Usar os novos recursos do editor HTML incluídos no Web Essentials como ricos trechos de código do HTML5 e Zen codificação</span><span class="sxs-lookup"><span data-stu-id="19dd4-430">Use new HTML editor features included in Web Essentials such as rich HTML5 code snippets and Zen coding</span></span>
- <span data-ttu-id="19dd4-431">Use os novos recursos de editor de CSS incluídos no Essentials Web como o seletor de cores e uma dica de ferramenta de matriz de navegador</span><span class="sxs-lookup"><span data-stu-id="19dd4-431">Use new CSS editor features included in Web Essentials such as the Color picker and Browser matrix tooltip</span></span>
- <span data-ttu-id="19dd4-432">Usar novos recursos do editor JavaScript incluídos no Web Essentials como extrair para o arquivo e o IntelliSense para todos os elementos HTML</span><span class="sxs-lookup"><span data-stu-id="19dd4-432">Use new JavaScript editor features included in Web Essentials such as Extract to File and IntelliSense for all HTML elements</span></span>
- <span data-ttu-id="19dd4-433">Trocar dados entre o navegador e o Visual Studio usando o Link do navegador</span><span class="sxs-lookup"><span data-stu-id="19dd4-433">Exchange data between your browser and Visual Studio using Browser Link</span></span>
