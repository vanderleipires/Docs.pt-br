---
uid: mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
title: "O que há de novo no ASP.NET MVC 4 | Microsoft Docs"
author: rick-anderson
description: "ASP.NET MVC 4 é uma estrutura para criar aplicativos web escalonável, baseado em padrões usando padrões de design bem estabelecidos e o poder do ASP.NET e..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 48f7feb3-872f-485d-b96f-e30011ff8c4a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 3de952224e23eed29f90ed0e8c662e4ee3f531ce
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-mvc-4"></a><span data-ttu-id="2995b-103">O que há de novo no ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="2995b-103">What's New in ASP.NET MVC 4</span></span>
====================
<span data-ttu-id="2995b-104">por [Web Camps Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="2995b-104">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="2995b-105">Baixar o Kit de treinamento de Camps de Web</span><span class="sxs-lookup"><span data-stu-id="2995b-105">Download Web Camps Training Kit</span></span>](http://www.microsoft.com/en-us/download/29843)

> <span data-ttu-id="2995b-106">ASP.NET MVC 4 é uma estrutura para criar aplicativos web escalonável, baseado em padrões usando padrões de design bem estabelecidos e o poder do ASP.NET e o .NET framework.</span><span class="sxs-lookup"><span data-stu-id="2995b-106">ASP.NET MVC 4 is a framework for building scalable, standards-based web applications using well-established design patterns and the power of the ASP.NET and the .NET framework.</span></span> <span data-ttu-id="2995b-107">Essa nova quarta versão do framework enfoca facilita o desenvolvimento de aplicativos web móvel.</span><span class="sxs-lookup"><span data-stu-id="2995b-107">This new, fourth version of the framework focuses on making mobile web application development easier.</span></span>
> 
> <span data-ttu-id="2995b-108">Em primeiro lugar, quando você cria um novo projeto ASP.NET MVC 4 é agora um modelo de projeto de aplicativo móvel que você pode usar para compilar um aplicativo autônomo especificamente para dispositivos móveis.</span><span class="sxs-lookup"><span data-stu-id="2995b-108">To begin with, when you create a new ASP.NET MVC 4 project there is now a mobile application project template you can use to build a standalone app specifically for mobile devices.</span></span> <span data-ttu-id="2995b-109">Além disso, ASP.NET MVC 4 integra-se com o jQuery Mobile por meio de um pacote jQuery.Mobile.MVC NuGet.</span><span class="sxs-lookup"><span data-stu-id="2995b-109">Additionally, ASP.NET MVC 4 integrates with jQuery Mobile through a jQuery.Mobile.MVC NuGet package.</span></span> <span data-ttu-id="2995b-110">o jQuery Mobile é uma estrutura baseada em HTML5 para o desenvolvimento de aplicativos web que são compatíveis com todas as plataformas de dispositivo móvel popular, incluindo Windows Phone, iPhone, Android e assim por diante.</span><span class="sxs-lookup"><span data-stu-id="2995b-110">jQuery Mobile is an HTML5-based framework for developing web apps that are compatible with all popular mobile device platforms, including Windows Phone, iPhone, Android and so on.</span></span> <span data-ttu-id="2995b-111">No entanto, se você precisar de especialização, ASP.NET MVC 4 também permite que você atender a diferentes modos de exibição para diferentes dispositivos e fornecer otimizações específicas do dispositivo.</span><span class="sxs-lookup"><span data-stu-id="2995b-111">However, if you need specialization, ASP.NET MVC 4 also enables you to serve different views for different devices and provide device-specific optimizations.</span></span>
> 
> <span data-ttu-id="2995b-112">Este laboratório prático, você iniciará com o ASP.NET MVC 4 &quot;aplicativo de Internet&quot; modelo de projeto para criar um aplicativo da Galeria de fotos.</span><span class="sxs-lookup"><span data-stu-id="2995b-112">In this hands-on lab, you will start with the ASP.NET MVC 4 &quot;Internet Application&quot; project template to create a Photo Gallery application.</span></span> <span data-ttu-id="2995b-113">Você aprimorará progressivamente o aplicativo usando o jQuery Mobile e recursos novos do ASP.NET MVC 4 para torná-lo compatível com navegadores da web da área de trabalho e de dispositivos móveis diferentes.</span><span class="sxs-lookup"><span data-stu-id="2995b-113">You will progressively enhance the app using jQuery Mobile and ASP.NET MVC 4's new features to make it compatible with different mobile devices and desktop web browsers.</span></span> <span data-ttu-id="2995b-114">Você também aprenderá sobre novo receitas de código para geração de código e como o ASP.NET MVC 4 torna mais fácil para gravar os métodos de ação assíncrono com o suporte à tarefa&lt;ActionResult&gt; tipos de retorno.</span><span class="sxs-lookup"><span data-stu-id="2995b-114">You will also learn about new code recipes for code generation and how ASP.NET MVC 4 makes it easier for you to write asynchronous action methods by supporting Task&lt;ActionResult&gt; return types.</span></span>
> 
> <span data-ttu-id="2995b-115">Todo o código de exemplo e trechos de código são incluídos no Web Camps treinamento Kit, disponíveis em [https://www.microsoft.com/en-us/download/29843](https://www.microsoft.com/en-us/download/29843).</span><span class="sxs-lookup"><span data-stu-id="2995b-115">All sample code and snippets are included in the Web Camps Training Kit, available at [https://www.microsoft.com/en-us/download/29843](https://www.microsoft.com/en-us/download/29843).</span></span>


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="2995b-116">Objetivos</span><span class="sxs-lookup"><span data-stu-id="2995b-116">Objectives</span></span>

<span data-ttu-id="2995b-117">Este laboratório prático, você aprenderá como:</span><span class="sxs-lookup"><span data-stu-id="2995b-117">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="2995b-118">Aproveitar os aprimoramentos para o ASP.NET MVC projeto modelos, incluindo o novo modelo de projeto de aplicativo móvel</span><span class="sxs-lookup"><span data-stu-id="2995b-118">Take advantage of the enhancements to the ASP.NET MVC project templates-including the new mobile application project template</span></span>
- <span data-ttu-id="2995b-119">Use o atributo de visor do HTML5 e consultas de mídia CSS para melhorar a exibição em dispositivos móveis</span><span class="sxs-lookup"><span data-stu-id="2995b-119">Use the HTML5 viewport attribute and CSS media queries to improve the display on mobile devices</span></span>
- <span data-ttu-id="2995b-120">Use o jQuery Mobile para aprimoramentos progressivos e para a criação de interface da web otimizada para toque</span><span class="sxs-lookup"><span data-stu-id="2995b-120">Use jQuery Mobile for progressive enhancements and for building touch-optimized web UI</span></span>
- <span data-ttu-id="2995b-121">Criar exibições específicas para dispositivos móveis</span><span class="sxs-lookup"><span data-stu-id="2995b-121">Create mobile-specific views</span></span>
- <span data-ttu-id="2995b-122">Use o componente de alternância de exibição para alternar entre modos de exibição móveis e área de trabalho do aplicativo</span><span class="sxs-lookup"><span data-stu-id="2995b-122">Use the view-switcher component to toggle between mobile and desktop views in the application</span></span>
- <span data-ttu-id="2995b-123">Criar controladores assíncronos usando o suporte da tarefa</span><span class="sxs-lookup"><span data-stu-id="2995b-123">Create asynchronous controllers using task support</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="2995b-124">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="2995b-124">Prerequisites</span></span>

<span data-ttu-id="2995b-125">Você deve ter os seguintes itens para concluir este laboratório:</span><span class="sxs-lookup"><span data-stu-id="2995b-125">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="2995b-126">[Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou superior (leitura [Apêndice B](#AppendixB) para obter instruções sobre como instalá-lo).</span><span class="sxs-lookup"><span data-stu-id="2995b-126">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix B](#AppendixB) for instructions on how to install it).</span></span>
- <span data-ttu-id="2995b-127">[ASP.NET MVC 4](../../../mvc4.md) (incluído na instalação do Microsoft Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="2995b-127">[ASP.NET MVC 4](../../../mvc4.md) (included in the Microsoft Visual Studio 2012 installation)</span></span>
- <span data-ttu-id="2995b-128">Emulador do Windows Phone (incluído no [Windows Phone 7.1.1 SDK](https://www.microsoft.com/en-us/download/details.aspx?id=29233))</span><span class="sxs-lookup"><span data-stu-id="2995b-128">Windows Phone Emulator (included in the [Windows Phone 7.1.1 SDK](https://www.microsoft.com/en-us/download/details.aspx?id=29233))</span></span>
- <span data-ttu-id="2995b-129">Opcional - [o WebMatrix 2](https://www.microsoft.com/web/webmatrix/) com **Electric Plum iPhone simulador** extensão (somente para o Exercício 3 usada para procurar o aplicativo web com um simulador de iPhone)</span><span class="sxs-lookup"><span data-stu-id="2995b-129">Optional - [WebMatrix 2](https://www.microsoft.com/web/webmatrix/) with **Electric Plum iPhone Simulator** extension (only for Exercise 3 used to browse the web application with an iPhone simulator)</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="2995b-130">Configuração</span><span class="sxs-lookup"><span data-stu-id="2995b-130">Setup</span></span>

<span data-ttu-id="2995b-131">Em todo o documento de laboratório, você será instruído a inserir blocos de código.</span><span class="sxs-lookup"><span data-stu-id="2995b-131">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="2995b-132">Para sua conveniência, a maioria do código é fornecido como o Visual Studio trechos de código que pode ser usado de dentro do Visual Studio para evitar a necessidade para adicioná-lo manualmente.</span><span class="sxs-lookup"><span data-stu-id="2995b-132">For your convenience, most of that code is provided as Visual Studio Code Snippets, which you can use from within Visual Studio to avoid having to add it manually.</span></span>

<span data-ttu-id="2995b-133">Para instalar os trechos de código:</span><span class="sxs-lookup"><span data-stu-id="2995b-133">To install the code snippets:</span></span>

1. <span data-ttu-id="2995b-134">Abra uma janela do Windows Explorer e navegue até o laboratório **Source\Setup** pasta.</span><span class="sxs-lookup"><span data-stu-id="2995b-134">Open a Windows Explorer window and browse to the lab's **Source\Setup** folder.</span></span>
2. <span data-ttu-id="2995b-135">Clique duas vezes o **Setup.cmd** arquivo nessa pasta para instalar os trechos de código do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2995b-135">Double-click the **Setup.cmd** file in this folder to install the Visual Studio code snippets.</span></span>

<span data-ttu-id="2995b-136">Se você não estiver familiarizado com os trechos de código do Visual Studio e aprender como usá-los, consulte o Apêndice deste documento &quot; [trechos de código usando do apêndice a:](#AppendixA)&quot;.</span><span class="sxs-lookup"><span data-stu-id="2995b-136">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix A: Using Code Snippets](#AppendixA)&quot;.</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="2995b-137">Exercícios</span><span class="sxs-lookup"><span data-stu-id="2995b-137">Exercises</span></span>

<span data-ttu-id="2995b-138">Este laboratório prático inclui os seguintes exercícios:</span><span class="sxs-lookup"><span data-stu-id="2995b-138">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="2995b-139">Novos modelos de projeto do ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="2995b-139">New ASP.NET MVC 4 Project Templates</span></span>](#Exercise1)
2. [<span data-ttu-id="2995b-140">Criando o aplicativo Web de galeria de fotos</span><span class="sxs-lookup"><span data-stu-id="2995b-140">Creating the Photo Gallery Web Application</span></span>](#Exercise2)
3. [<span data-ttu-id="2995b-141">Adicionando suporte para dispositivos móveis</span><span class="sxs-lookup"><span data-stu-id="2995b-141">Adding Support for Mobile Devices</span></span>](#Exercise3)
4. [<span data-ttu-id="2995b-142">Usando controladores assíncronos</span><span class="sxs-lookup"><span data-stu-id="2995b-142">Using Asynchronous Controllers</span></span>](#Exercise4)

> [!NOTE]
> <span data-ttu-id="2995b-143">Cada exercício é acompanhado por um **final** pasta que contém a solução resultante, você deve obter depois de concluir os exercícios.</span><span class="sxs-lookup"><span data-stu-id="2995b-143">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="2995b-144">Você pode usar essa solução como um guia se você precisar trabalhar com os exercícios de ajuda adicional.</span><span class="sxs-lookup"><span data-stu-id="2995b-144">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="2995b-145">Tempo estimado para concluir este laboratório: **60 minutos**.</span><span class="sxs-lookup"><span data-stu-id="2995b-145">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_New_ASPNET_MVC_4_Project_Templates"></a>
### <a name="exercise-1-new-aspnet-mvc-4-project-templates"></a><span data-ttu-id="2995b-146">Exercício 1: Novos modelos de projeto do ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="2995b-146">Exercise 1: New ASP.NET MVC 4 Project Templates</span></span>

<span data-ttu-id="2995b-147">Neste exercício, você irá explorar os aprimoramentos nos modelos de projeto do ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="2995b-147">In this exercise, you will explore the enhancements in the ASP.NET MVC 4 Project templates.</span></span> <span data-ttu-id="2995b-148">Além do modelo de aplicativo de Internet, já está presente no MVC 3, esta versão agora inclui um modelo separado para aplicativos móveis.</span><span class="sxs-lookup"><span data-stu-id="2995b-148">In addition to the Internet Application template, already present in MVC 3, this version now includes a separate template for Mobile applications.</span></span> <span data-ttu-id="2995b-149">Primeiro, você examinará alguns recursos relevantes de cada um dos modelos.</span><span class="sxs-lookup"><span data-stu-id="2995b-149">First, you will look at some relevant features of each of the templates.</span></span> <span data-ttu-id="2995b-150">Em seguida, você trabalhará na sua página corretamente em diferentes plataformas de processamento usando a abordagem certa.</span><span class="sxs-lookup"><span data-stu-id="2995b-150">Then, you will work on rendering your page properly on the different platforms by using the right approach.</span></span>

<a id="Task_1_-_Exploring_the_Internet_Application_Template"></a>
#### <a name="task-1---exploring-the-internet-application-template"></a><span data-ttu-id="2995b-151">Tarefa 1: Explorando o modelo de aplicativo de Internet</span><span class="sxs-lookup"><span data-stu-id="2995b-151">Task 1 - Exploring the Internet Application Template</span></span>

1. <span data-ttu-id="2995b-152">Abra **Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="2995b-152">Open **Visual Studio**.</span></span>
2. <span data-ttu-id="2995b-153">Selecione o **arquivo | Novo | Projeto** comando de menu.</span><span class="sxs-lookup"><span data-stu-id="2995b-153">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="2995b-154">No **novo projeto** caixa de diálogo, selecione o **Visual c# | Web** modelo no painel esquerdo da árvore e escolha **aplicativo Web do ASP.NET MVC 4.**</span><span class="sxs-lookup"><span data-stu-id="2995b-154">In the **New Project** dialog, select the **Visual C# | Web** template on the left pane tree, and choose **ASP.NET MVC 4 Web Application.**</span></span> <span data-ttu-id="2995b-155">Nomeie o projeto **Galeriadefotos**, selecione um local (ou deixe o padrão) e clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="2995b-155">Name the project **PhotoGallery**, select a location (or leave the default) and click **OK**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2995b-156">Posteriormente, você personalizará a solução Galeriadefotos ASP.NET MVC 4 que você está criando agora.</span><span class="sxs-lookup"><span data-stu-id="2995b-156">You will later customize the PhotoGallery ASP.NET MVC 4 solution you are now creating.</span></span>

    <span data-ttu-id="2995b-157">![Criar um novo projeto](whats-new-in-aspnet-mvc-4/_static/image1.png "criar um novo projeto")</span><span class="sxs-lookup"><span data-stu-id="2995b-157">![Creating a new project](whats-new-in-aspnet-mvc-4/_static/image1.png "Creating a new project")</span></span>

    <span data-ttu-id="2995b-158">*Criar um novo projeto*</span><span class="sxs-lookup"><span data-stu-id="2995b-158">*Creating a new project*</span></span>
3. <span data-ttu-id="2995b-159">No **novo projeto ASP.NET MVC 4** caixa de diálogo, selecione o **aplicativo de Internet** modelo de projeto e clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="2995b-159">In the **New ASP.NET MVC 4 Project** dialog, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="2995b-160">Verifique se que você selecionou Razor como o mecanismo de exibição.</span><span class="sxs-lookup"><span data-stu-id="2995b-160">Make sure you have selected Razor as the view engine.</span></span>

    <span data-ttu-id="2995b-161">![Criar um novo aplicativo ASP.NET MVC 4 Internet](whats-new-in-aspnet-mvc-4/_static/image2.png "criar um novo aplicativo ASP.NET MVC 4 da Internet")</span><span class="sxs-lookup"><span data-stu-id="2995b-161">![Creating a new ASP.NET MVC 4 Internet Application](whats-new-in-aspnet-mvc-4/_static/image2.png "Creating a new ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="2995b-162">*Criar um novo aplicativo ASP.NET MVC 4 da Internet*</span><span class="sxs-lookup"><span data-stu-id="2995b-162">*Creating a new ASP.NET MVC 4 Internet Application*</span></span>

    > [!NOTE]
    > <span data-ttu-id="2995b-163">A sintaxe do Razor foi introduzida no ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="2995b-163">Razor syntax has been introduced in ASP.NET MVC 3.</span></span> <span data-ttu-id="2995b-164">Seu objetivo é minimizar o número de caracteres e pressionamentos de tecla necessários em um arquivo, permitindo um rápido e fluido fluxo de trabalho de codificação.</span><span class="sxs-lookup"><span data-stu-id="2995b-164">Its goal is to minimize the number of characters and keystrokes required in a file, enabling a fast and fluid coding workflow.</span></span> <span data-ttu-id="2995b-165">Aproveita Razor existente c# / VB (ou outros) habilidades de linguagem e fornece uma sintaxe de marcação de modelo que permite que um fluxo de trabalho de construção awesome HTML.</span><span class="sxs-lookup"><span data-stu-id="2995b-165">Razor leverages existing C# / VB (or other) language skills and delivers a template markup syntax that enables an awesome HTML construction workflow.</span></span>
4. <span data-ttu-id="2995b-166">Pressione **F5** para executar a solução e ver os modelos renovados.</span><span class="sxs-lookup"><span data-stu-id="2995b-166">Press **F5** to run the solution and see the renewed templates.</span></span> <span data-ttu-id="2995b-167">Você pode conferir os recursos a seguir:</span><span class="sxs-lookup"><span data-stu-id="2995b-167">You can check out the following features:</span></span>

    <span data-ttu-id="2995b-168">**Modelos de estilo moderno**</span><span class="sxs-lookup"><span data-stu-id="2995b-168">**Modern-style templates**</span></span>

    <span data-ttu-id="2995b-169">Os modelos foram renovados, fornecendo mais estilos aparência moderna.</span><span class="sxs-lookup"><span data-stu-id="2995b-169">The templates have been renewed, providing more modern-looking styles.</span></span>

    <span data-ttu-id="2995b-170">![Modelos do ASP.NET MVC 4 remodelado](whats-new-in-aspnet-mvc-4/_static/image3.png "remodelado modelos do MVC 4")</span><span class="sxs-lookup"><span data-stu-id="2995b-170">![ASP.NET MVC 4 restyled templates](whats-new-in-aspnet-mvc-4/_static/image3.png "MVC 4 restyled templates")</span></span>

    <span data-ttu-id="2995b-171">*Modelos do ASP.NET MVC 4 remodelado*</span><span class="sxs-lookup"><span data-stu-id="2995b-171">*ASP.NET MVC 4 restyled templates*</span></span>

    <span data-ttu-id="2995b-172">![Nova página de contato](whats-new-in-aspnet-mvc-4/_static/image4.png "página novo contato")</span><span class="sxs-lookup"><span data-stu-id="2995b-172">![New Contact page](whats-new-in-aspnet-mvc-4/_static/image4.png "New Contact page")</span></span>

    <span data-ttu-id="2995b-173">*Nova página de contato*</span><span class="sxs-lookup"><span data-stu-id="2995b-173">*New Contact page*</span></span>

    <span data-ttu-id="2995b-174">**Renderização adaptável**</span><span class="sxs-lookup"><span data-stu-id="2995b-174">**Adaptive Rendering**</span></span>

    <span data-ttu-id="2995b-175">Check-out de redimensionar a janela do navegador e observe como o layout de página adapta dinamicamente para o novo tamanho da janela.</span><span class="sxs-lookup"><span data-stu-id="2995b-175">Check out resizing the browser window and notice how the page layout dynamically adapts to the new window size.</span></span> <span data-ttu-id="2995b-176">Esses modelos de usam a técnica de renderização adaptável para processar corretamente em plataformas móveis e desktop sem nenhuma personalização.</span><span class="sxs-lookup"><span data-stu-id="2995b-176">These templates use the adaptive rendering technique to render properly in both desktop and mobile platforms without any customization.</span></span>

    <span data-ttu-id="2995b-177">![Modelo de projeto ASP.NET MVC 4 em tamanhos diferentes do navegador](whats-new-in-aspnet-mvc-4/_static/image5.png "modelo de projeto ASP.NET MVC 4 em tamanhos diferentes do navegador")</span><span class="sxs-lookup"><span data-stu-id="2995b-177">![ASP.NET MVC 4 project template in different browser sizes](whats-new-in-aspnet-mvc-4/_static/image5.png "ASP.NET MVC 4 project template in different browser sizes")</span></span>

    <span data-ttu-id="2995b-178">*Modelo de projeto ASP.NET MVC 4 em tamanhos diferentes do navegador*</span><span class="sxs-lookup"><span data-stu-id="2995b-178">*ASP.NET MVC 4 project template in different browser sizes*</span></span>

    <span data-ttu-id="2995b-179">**Interface de usuário mais rica com JavaScript**</span><span class="sxs-lookup"><span data-stu-id="2995b-179">**Richer UI with JavaScript**</span></span>

    <span data-ttu-id="2995b-180">Outro aprimoramento para modelos de projeto padrão é o uso de JavaScript para fornecer um JavaScript mais interativo.</span><span class="sxs-lookup"><span data-stu-id="2995b-180">Another enhancement to default project templates is the use of JavaScript to provide a more interactive JavaScript.</span></span> <span data-ttu-id="2995b-181">Os links de logon e registro usados no modelo exemplificam como usar o jQuery validações para validar os campos de entrada do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="2995b-181">The Login and Register links used in the template exemplify how to use the jQuery Validations to validate the input fields from client-side.</span></span>

    ![jQuery validação](whats-new-in-aspnet-mvc-4/_static/image6.png)

    <span data-ttu-id="2995b-183">*jQuery validação*</span><span class="sxs-lookup"><span data-stu-id="2995b-183">*jQuery Validation*</span></span>

    > [!NOTE]
    > <span data-ttu-id="2995b-184">Observe que os dois logon nas seções, a primeira seção você pode fazer logon usando uma conta de marca do site e a segunda seção que pode altenativelly logon usando outro serviço de autenticação com o google (desabilitado por padrão).</span><span class="sxs-lookup"><span data-stu-id="2995b-184">Notice the two log in sections, in the first section you can log in using a registerd account from the site and in the second section you can altenativelly log in using another authentication service like google (disabled by default).</span></span>
5. <span data-ttu-id="2995b-185">Feche o navegador para interromper o depurador e retornar ao Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2995b-185">Close the browser to stop the debugger and return to Visual Studio.</span></span>
6. <span data-ttu-id="2995b-186">Abra o arquivo **AuthConfig.cs** localizado sob o **aplicativo\_iniciar** pasta.</span><span class="sxs-lookup"><span data-stu-id="2995b-186">Open the file **AuthConfig.cs** located under the **App\_Start** folder.</span></span>
7. <span data-ttu-id="2995b-187">Remova o comentário da última linha para registrar o cliente do Google para *OAuth* autenticação.</span><span class="sxs-lookup"><span data-stu-id="2995b-187">Remove the comment from the last line to register Google client for *OAuth* authentication.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample1.cs)]

    > [!NOTE]
    > <span data-ttu-id="2995b-188">Observe que você pode facilmente habilitar autenticação usando qualquer serviço OpenID ou OAuth do Facebook, Twitter, Microsoft, etc.</span><span class="sxs-lookup"><span data-stu-id="2995b-188">Notice you can easily enable authentication using any OpenID or OAuth service like Facebook, Twitter, Microsoft, etc.</span></span>
8. <span data-ttu-id="2995b-189">Pressione **F5** para executar a solução e navegue até a página de logon.</span><span class="sxs-lookup"><span data-stu-id="2995b-189">Press **F5** to run the solution and navigate to the login page.</span></span>
9. <span data-ttu-id="2995b-190">Selecione **Google** serviço para fazer logon.</span><span class="sxs-lookup"><span data-stu-id="2995b-190">Select **Google** service to log in.</span></span>

    ![Selecionar o log no serviço](whats-new-in-aspnet-mvc-4/_static/image7.png)

    <span data-ttu-id="2995b-192">*Selecionar o log no serviço*</span><span class="sxs-lookup"><span data-stu-id="2995b-192">*Selecting the log in service*</span></span>
10. <span data-ttu-id="2995b-193">Faça logon usando a conta do Google.</span><span class="sxs-lookup"><span data-stu-id="2995b-193">Log in using your Google account.</span></span>
11. <span data-ttu-id="2995b-194">Permitir que o site (localhost) recuperar informações de conta do Google.</span><span class="sxs-lookup"><span data-stu-id="2995b-194">Allow the site (localhost) to retrieve information from Google account.</span></span>
12. <span data-ttu-id="2995b-195">Por fim, você terá que registrar no site para associar a conta do Google.</span><span class="sxs-lookup"><span data-stu-id="2995b-195">Finally, you will have to register in the site to associate the Google account.</span></span>

    ![Associe a conta do Google](whats-new-in-aspnet-mvc-4/_static/image8.png)

    <span data-ttu-id="2995b-197">*Associando a conta do Google*</span><span class="sxs-lookup"><span data-stu-id="2995b-197">*Associating your Google account*</span></span>
13. <span data-ttu-id="2995b-198">Feche o navegador para interromper o depurador e retornar ao Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2995b-198">Close the browser to stop the debugger and return to Visual Studio.</span></span>
14. <span data-ttu-id="2995b-199">Explore agora a solução para check-out de alguns outros novos recursos introduzidos pelo ASP.NET MVC 4 no modelo de projeto.</span><span class="sxs-lookup"><span data-stu-id="2995b-199">Now explore the solution to check out some other new features introduced by ASP.NET MVC 4 in the project template.</span></span>

    <span data-ttu-id="2995b-200">![O modelo de projeto de aplicativo do ASP.NET MVC 4 Internet](whats-new-in-aspnet-mvc-4/_static/image9.png "o modelo de projeto de aplicativo de Internet do ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="2995b-200">![The ASP.NET MVC 4 Internet Application Project Template](whats-new-in-aspnet-mvc-4/_static/image9.png "The ASP.NET MVC 4 Internet Application Project Template")</span></span>

    <span data-ttu-id="2995b-201">*O modelo de projeto de aplicativo de Internet do ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="2995b-201">*The ASP.NET MVC 4 Internet Application Project Template*</span></span>

    - <span data-ttu-id="2995b-202">**HTML 5 marcação**</span><span class="sxs-lookup"><span data-stu-id="2995b-202">**HTML 5 Markup**</span></span>

        <span data-ttu-id="2995b-203">Procure exibições de modelo para descobrir a nova marcação de tema.</span><span class="sxs-lookup"><span data-stu-id="2995b-203">Browse template views to find out the new theme markup.</span></span>

        <span data-ttu-id="2995b-204">![Novo modelo, usando About.cshtml de marcação Razor e HTML5. ] (whats-new-in-aspnet-mvc-4/_static/image10.png "Novo modelo, usando About.cshtml de marcação Razor e HTML5.")</span><span class="sxs-lookup"><span data-stu-id="2995b-204">![New template, using Razor and HTML5 markup About.cshtml.](whats-new-in-aspnet-mvc-4/_static/image10.png "New template, using Razor and HTML5 markup About.cshtml.")</span></span>

        <span data-ttu-id="2995b-205">*Novo modelo, usando marcação Razor e HTML5 (About.cshtml).*</span><span class="sxs-lookup"><span data-stu-id="2995b-205">*New template, using Razor and HTML5 markup (About.cshtml).*</span></span>
    - <span data-ttu-id="2995b-206">**Bibliotecas de JavaScript atualizadas**</span><span class="sxs-lookup"><span data-stu-id="2995b-206">**Updated JavaScript libraries**</span></span>

        <span data-ttu-id="2995b-207">O modelo de padrão do ASP.NET MVC 4 agora inclui KnockoutJS, uma estrutura de JavaScript MVVM que permite que você crie avançados e aplicativos web altamente responsivo usando JavaScript e HTML.</span><span class="sxs-lookup"><span data-stu-id="2995b-207">The ASP.NET MVC 4 default template now includes KnockoutJS, a JavaScript MVVM framework that lets you create rich and highly responsive web applications using JavaScript and HTML.</span></span> <span data-ttu-id="2995b-208">Como em MVC3, jQuery e jQuery bibliotecas de interface do usuário também estão incluídas no ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="2995b-208">Like in MVC3, jQuery and jQuery UI libraries are also included in ASP.NET MVC 4.</span></span>

        > [!NOTE]
        > <span data-ttu-id="2995b-209">Você pode obter mais informações sobre a biblioteca KnockOutJS neste link: [ [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/)](http://learn.knockoutjs.com/).</span><span class="sxs-lookup"><span data-stu-id="2995b-209">You can get more information about KnockOutJS library in this link: [[http://learn.knockoutjs.com/](http://learn.knockoutjs.com/)](http://learn.knockoutjs.com/).</span></span> <span data-ttu-id="2995b-210">Além disso, você pode aprender sobre jQuery e jQuery UI em [ [http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span><span class="sxs-lookup"><span data-stu-id="2995b-210">Additionally, you can learn about jQuery and jQuery UI in [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span></span>

<a id="Task_2_-_Exploring_the_Mobile_Application_Template"></a>
#### <a name="task-2---exploring-the-mobile-application-template"></a><span data-ttu-id="2995b-211">Tarefa 2: explorar o modelo de aplicativo móvel</span><span class="sxs-lookup"><span data-stu-id="2995b-211">Task 2 - Exploring the Mobile Application Template</span></span>

<span data-ttu-id="2995b-212">ASP.NET MVC 4 facilita o desenvolvimento de sites da Web para dispositivos móveis e navegadores do tablet.</span><span class="sxs-lookup"><span data-stu-id="2995b-212">ASP.NET MVC 4 facilitates the development of websites for mobile and tablet browsers.</span></span> <span data-ttu-id="2995b-213">Esse modelo tem a mesma estrutura de aplicativo como o modelo de aplicativo de Internet (Observe que o código do controlador é praticamente idêntico), mas seu estilo foi modificado para ser processada corretamente em dispositivos móveis baseados em toque.</span><span class="sxs-lookup"><span data-stu-id="2995b-213">This template has the same application structure as the Internet Application template (notice that the controller code is practically identical), but its style was modified to render properly in touch-based mobile devices.</span></span>

1. <span data-ttu-id="2995b-214">Selecione o **arquivo | Novo | Projeto** comando de menu.</span><span class="sxs-lookup"><span data-stu-id="2995b-214">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="2995b-215">No **novo projeto** caixa de diálogo, selecione o **Visual c# | Web** modelo no painel esquerdo da árvore e escolha o **aplicativo Web do ASP.NET MVC 4.**</span><span class="sxs-lookup"><span data-stu-id="2995b-215">In the **New Project** dialog, select the **Visual C# | Web** template on the left pane tree, and choose the **ASP.NET MVC 4 Web Application.**</span></span> <span data-ttu-id="2995b-216">Nomeie o projeto **PhotoGallery.Mobile**, selecione um local (ou deixe o padrão), selecione &quot;adicionar à solução&quot; e clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="2995b-216">Name the project **PhotoGallery.Mobile**, select a location (or leave the default), select &quot;Add to solution&quot; and click **OK**.</span></span>
2. <span data-ttu-id="2995b-217">No **novo projeto ASP.NET MVC 4** caixa de diálogo, selecione o **Mobile Application** modelo de projeto e clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="2995b-217">In the **New ASP.NET MVC 4 Project** dialog, select the **Mobile Application** project template and click **OK**.</span></span> <span data-ttu-id="2995b-218">Verifique se que você selecionou Razor como o mecanismo de exibição.</span><span class="sxs-lookup"><span data-stu-id="2995b-218">Make sure you have selected Razor as the view engine.</span></span>

    <span data-ttu-id="2995b-219">![Criar um novo aplicativo ASP.NET MVC 4 Mobile](whats-new-in-aspnet-mvc-4/_static/image11.png "criar um novo aplicativo ASP.NET MVC 4 Mobile")</span><span class="sxs-lookup"><span data-stu-id="2995b-219">![Creating a new ASP.NET MVC 4 Mobile Application](whats-new-in-aspnet-mvc-4/_static/image11.png "Creating a new ASP.NET MVC 4 Mobile Application")</span></span>

    <span data-ttu-id="2995b-220">*Criar um novo aplicativo ASP.NET MVC 4 Mobile*</span><span class="sxs-lookup"><span data-stu-id="2995b-220">*Creating a new ASP.NET MVC 4 Mobile Application*</span></span>
3. <span data-ttu-id="2995b-221">Agora é possível explorar a solução e check-out de alguns dos novos recursos introduzidos pelo modelo de solução do ASP.NET MVC 4 para dispositivos móveis:</span><span class="sxs-lookup"><span data-stu-id="2995b-221">Now you are able to explore the solution and check out some of the new features introduced by the ASP.NET MVC 4 solution template for mobile:</span></span>

    - <span data-ttu-id="2995b-222">**jQuery Mobile biblioteca**</span><span class="sxs-lookup"><span data-stu-id="2995b-222">**jQuery Mobile Library**</span></span>

        <span data-ttu-id="2995b-223">O modelo de projeto de aplicativo móvel inclui a biblioteca jQuery Mobile, que é uma biblioteca de software livre para compatibilidade com navegadores móveis.</span><span class="sxs-lookup"><span data-stu-id="2995b-223">The Mobile Application project template includes the jQuery Mobile library, which is an open source library for mobile browser compatibility.</span></span> <span data-ttu-id="2995b-224">o jQuery Mobile aplica aprimoramento progressivo para navegadores de dispositivos móveis que oferecem suporte a CSS e JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2995b-224">jQuery Mobile applies progressive enhancement to mobile browsers that support CSS and JavaScript.</span></span> <span data-ttu-id="2995b-225">Aprimoramento progressivo permite que todos os navegadores exibir o conteúdo básico de uma página da web, enquanto ele só permite que os navegadores mais eficientes exibir o conteúdo.</span><span class="sxs-lookup"><span data-stu-id="2995b-225">Progressive enhancement enables all browsers to display the basic content of a web page, while it only enables the most powerful browsers to display the rich content.</span></span> <span data-ttu-id="2995b-226">Os arquivos JavaScript e CSS, incluídos no jQuery Mobile estilo, ajudam navegadores móveis para ajustar o conteúdo na tela sem fazer qualquer alteração na marcação da página.</span><span class="sxs-lookup"><span data-stu-id="2995b-226">The JavaScript and CSS files, included in the jQuery Mobile style, help mobile browsers to fit the content in the screen without making any change in the page markup.</span></span>

        ![jQuery-mobile-library-included-in-the-template](whats-new-in-aspnet-mvc-4/_static/image12.png)

        <span data-ttu-id="2995b-228">*biblioteca de móveis jQuery incluída no modelo*</span><span class="sxs-lookup"><span data-stu-id="2995b-228">*jQuery mobile library included in the template*</span></span>
    - <span data-ttu-id="2995b-229">**Marcação de HTML5 com base**</span><span class="sxs-lookup"><span data-stu-id="2995b-229">**HTML5 based markup**</span></span>

        ![Mobile-application-template-using-HTML5-markup](whats-new-in-aspnet-mvc-4/_static/image13.png)

        <span data-ttu-id="2995b-231">*Modelo de aplicativo móvel usando HTML5 marcação, (cshtml e cshtml)*</span><span class="sxs-lookup"><span data-stu-id="2995b-231">*Mobile application template using HTML5 markup, (Login.cshtml and index.cshtml)*</span></span>
4. <span data-ttu-id="2995b-232">Pressione **F5** para executar a solução.</span><span class="sxs-lookup"><span data-stu-id="2995b-232">Press **F5** to run the solution.</span></span>
5. <span data-ttu-id="2995b-233">Abra o **Windows Phone 7 emulador**.</span><span class="sxs-lookup"><span data-stu-id="2995b-233">Open the **Windows Phone 7 Emulator**.</span></span>
6. <span data-ttu-id="2995b-234">Na tela de início do telefone, abra o Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="2995b-234">In the phone start screen, open Internet Explorer.</span></span> <span data-ttu-id="2995b-235">Confira a URL onde o aplicativo de área de trabalho foi iniciado e navegue para a URL do telefone (por exemplo, `http://localhost:[PortNumber]/`).</span><span class="sxs-lookup"><span data-stu-id="2995b-235">Check out the URL where the desktop application started and browse to that URL from the phone (e.g. `http://localhost:[PortNumber]/`).</span></span>
7. <span data-ttu-id="2995b-236">Agora é possível inserir a página de logon ou check-out de sobre a página.</span><span class="sxs-lookup"><span data-stu-id="2995b-236">Now you are able to enter the login page or check out the about page.</span></span> <span data-ttu-id="2995b-237">Observe que o estilo do site se baseia o novo aplicativo do Metro para dispositivos móveis.</span><span class="sxs-lookup"><span data-stu-id="2995b-237">Notice that the style of the website is based on the new Metro app for mobile.</span></span> <span data-ttu-id="2995b-238">O modelo de projeto ASP.NET MVC 4 é exibido corretamente em dispositivos móveis, tornando-se de que todos os elementos da página são visíveis e habilitados.</span><span class="sxs-lookup"><span data-stu-id="2995b-238">The ASP.NET MVC 4 project template is correctly displayed on mobile devices, making sure all the elements of the page are visible and enabled.</span></span> <span data-ttu-id="2995b-239">Observe que os links no cabeçalho grandes o suficiente para ser clicado ou tocado.</span><span class="sxs-lookup"><span data-stu-id="2995b-239">Notice that the links on the header are big enough to be clicked or tapped.</span></span>

    <span data-ttu-id="2995b-240">![Páginas de modelo em um dispositivo móvel do projeto](whats-new-in-aspnet-mvc-4/_static/image14.png "projeto páginas de modelo em um dispositivo móvel")</span><span class="sxs-lookup"><span data-stu-id="2995b-240">![Project template pages in a mobile device](whats-new-in-aspnet-mvc-4/_static/image14.png "Project template pages in a mobile device")</span></span>

    <span data-ttu-id="2995b-241">*Páginas de modelo de projeto em um dispositivo móvel*</span><span class="sxs-lookup"><span data-stu-id="2995b-241">*Project template pages in a mobile device*</span></span>
8. <span data-ttu-id="2995b-242">O novo modelo também usa o **marca meta Viewport**.</span><span class="sxs-lookup"><span data-stu-id="2995b-242">The new template also uses the **Viewport meta tag**.</span></span> <span data-ttu-id="2995b-243">A maioria dos navegadores definem uma largura de uma janela do navegador virtual ou &quot;visor&quot;, que é maior do que a largura real do dispositivo móvel.</span><span class="sxs-lookup"><span data-stu-id="2995b-243">Most mobile browsers define a width for a virtual browser window or &quot;viewport&quot;, which is larger than the actual width of the mobile device.</span></span> <span data-ttu-id="2995b-244">Isso permite que navegadores móveis exibir a página da web inteira dentro a exibição virtual.</span><span class="sxs-lookup"><span data-stu-id="2995b-244">This enables mobile browsers to display the entire web page inside the virtual display.</span></span> <span data-ttu-id="2995b-245">O **marca meta Viewport** permite que os desenvolvedores da web definir a largura, altura e escala da área do navegador em dispositivos móveis **.**</span><span class="sxs-lookup"><span data-stu-id="2995b-245">The **Viewport meta tag** allows web developers to set the width, height and scale of the browser area on mobile devices **.**</span></span> <span data-ttu-id="2995b-246">O modelo do ASP.NET MVC 4 para aplicativos móveis define o visor para a largura do dispositivo (&quot;largura = dispositivo largura&quot;) no modelo de layout (*exibições \ compartilhadas\_cshtml*), de modo que todos os a páginas terão seu visor definida como a largura de tela do dispositivo.</span><span class="sxs-lookup"><span data-stu-id="2995b-246">The ASP.NET MVC 4 template for Mobile Applications sets the viewport to the device width (&quot;width=device-width&quot;) in the layout template (*Views\Shared\_Layout.cshtml*), so that all the pages will have their viewport set to the device screen width.</span></span> <span data-ttu-id="2995b-247">Observe que a marca meta Viewport não alterará o modo de exibição de navegador padrão.</span><span class="sxs-lookup"><span data-stu-id="2995b-247">Notice that the Viewport meta tag will not change the default browser view.</span></span>
9. <span data-ttu-id="2995b-248">Abra  **\_cshtml**, localizado no **exibições | Compartilhado** pasta e comente a marca meta do visor.</span><span class="sxs-lookup"><span data-stu-id="2995b-248">Open **\_Layout.cshtml**, located in the **Views | Shared** folder, and comment the Viewport meta tag.</span></span> <span data-ttu-id="2995b-249">Executar o aplicativo, se não estiver já aberto e conferir as diferenças.</span><span class="sxs-lookup"><span data-stu-id="2995b-249">Run the application, if not already opened, and check out the differences.</span></span>


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample2.cshtml)]

    <span data-ttu-id="2995b-250">![O site após a marca meta viewport de comentário](whats-new-in-aspnet-mvc-4/_static/image15.png "o site após a marca meta viewport de comentário")</span><span class="sxs-lookup"><span data-stu-id="2995b-250">![The site after commenting the viewport meta tag](whats-new-in-aspnet-mvc-4/_static/image15.png "The site after commenting the viewport meta tag")</span></span>

    <span data-ttu-id="2995b-251">*O site após a marca meta viewport de comentário*</span><span class="sxs-lookup"><span data-stu-id="2995b-251">*The site after commenting the viewport meta tag*</span></span>
10. <span data-ttu-id="2995b-252">No Visual Studio, pressione **SHIFT** + **F5** para parar a depuração do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2995b-252">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>
11. <span data-ttu-id="2995b-253">Remova a marca meta viewport.</span><span class="sxs-lookup"><span data-stu-id="2995b-253">Uncomment the viewport meta tag.</span></span>


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample3.cshtml)]

<a id="Task_3_-_Using_Adaptive_Rendering"></a>
#### <a name="task-3---using-adaptive-rendering"></a><span data-ttu-id="2995b-254">Tarefa 3: usar a renderização adaptável</span><span class="sxs-lookup"><span data-stu-id="2995b-254">Task 3 - Using Adaptive Rendering</span></span>

<span data-ttu-id="2995b-255">Nesta tarefa, você aprenderá a outro método para renderizar uma página da Web corretamente em dispositivos móveis e navegadores da Web ao mesmo tempo sem nenhuma personalização.</span><span class="sxs-lookup"><span data-stu-id="2995b-255">In this task, you will learn another method to render a Web page correctly on mobile devices and Web browsers at the same time without any customization.</span></span> <span data-ttu-id="2995b-256">Você já usou marca meta Viewport com uma finalidade semelhante.</span><span class="sxs-lookup"><span data-stu-id="2995b-256">You have already used Viewport meta tag with a similar purpose.</span></span> <span data-ttu-id="2995b-257">Agora você atenderá outro método avançado: *processamento adaptável*.</span><span class="sxs-lookup"><span data-stu-id="2995b-257">Now you will meet another powerful method: *adaptive rendering*.</span></span>

<span data-ttu-id="2995b-258">Renderização adaptável é uma técnica que usa **consultas de mídia CSS3** para personalizar o estilo aplicado a uma página.</span><span class="sxs-lookup"><span data-stu-id="2995b-258">Adaptive rendering is a technique that uses **CSS3 media queries** to customize the style applied to a page.</span></span> <span data-ttu-id="2995b-259">As consultas de mídia definem condições dentro de uma folha de estilos, agrupando os estilos CSS em uma determinada condição.</span><span class="sxs-lookup"><span data-stu-id="2995b-259">Media queries define conditions inside a style sheet, grouping CSS styles under a certain condition.</span></span> <span data-ttu-id="2995b-260">Somente quando a condição for true, o estilo é aplicado aos objetos declarados.</span><span class="sxs-lookup"><span data-stu-id="2995b-260">Only when the condition is true, the style is applied to the declared objects.</span></span>

<span data-ttu-id="2995b-261">A flexibilidade fornecida pela técnica processamento adaptável permite que qualquer personalização para exibir o site em diferentes dispositivos.</span><span class="sxs-lookup"><span data-stu-id="2995b-261">The flexibility provided by the adaptive rendering technique enables any customization for displaying the site on different devices.</span></span> <span data-ttu-id="2995b-262">Você pode definir estilos quantos desejar em uma folha de estilos única sem escrever código de lógica para escolher o estilo.</span><span class="sxs-lookup"><span data-stu-id="2995b-262">You can define as many styles as you want on a single style sheet without writing logic code to choose the style.</span></span> <span data-ttu-id="2995b-263">Portanto, é uma maneira muito interessante de adaptar estilos de página, pois reduz a quantidade de código duplicado e lógica para fins de processamento.</span><span class="sxs-lookup"><span data-stu-id="2995b-263">Therefore, it is a very neat way of adapting page styles, as it reduces the amount of duplicated code and logic for rendering purposes.</span></span> <span data-ttu-id="2995b-264">Por outro lado, o consumo de largura de banda aumentaria, como o tamanho dos arquivos CSS pode aumentar um pouco.</span><span class="sxs-lookup"><span data-stu-id="2995b-264">On the other hand, bandwidth consumption would increase, as the size of your CSS files could grow marginally.</span></span>

<span data-ttu-id="2995b-265">Usando a técnica de renderização adaptável, o site será **exibido corretamente, independentemente do navegador.**</span><span class="sxs-lookup"><span data-stu-id="2995b-265">By using the adaptive rendering technique, your site will be **displayed properly, regardless of the browser.**</span></span> <span data-ttu-id="2995b-266">No entanto, você deve considerar se a largura de banda extra de carga é uma preocupação.</span><span class="sxs-lookup"><span data-stu-id="2995b-266">However, you should consider if the bandwidth extra load is a concern.</span></span>

> [!NOTE]
> <span data-ttu-id="2995b-267">O formato básico de uma consulta de mídia é: @media \[escopo: todos os | portáteis | imprimir | projeção | tela\] ([: valor da propriedade] e... [: valor da propriedade])</span><span class="sxs-lookup"><span data-stu-id="2995b-267">The basic format of a media query is: @media \[Scope: all | handheld | print | projection | screen\] ([property:value] and ... [property:value])</span></span>


<span data-ttu-id="2995b-268">Exemplos de consultas de mídia: &gt;  **@media todas as e (largura máxima: 1000px) e (largura mínima: 700px) {}:** para todas as resoluções entre 700px e 1000px.</span><span class="sxs-lookup"><span data-stu-id="2995b-268">Examples of media queries: &gt;**@media all and (max-width: 1000px) and (min-width: 700px) {}:** For all the resolutions between 700px and 1000px.</span></span>

> <span data-ttu-id="2995b-269">**@mediatela e (largura mínima: 400px) e (largura máxima: 700px) {...}:** somente para telas.</span><span class="sxs-lookup"><span data-stu-id="2995b-269">**@media screen and (min-width: 400px) and (max-width: 700px) { ... }:** Only for screens.</span></span> <span data-ttu-id="2995b-270">A resolução deve ficar entre 400 e 700px.</span><span class="sxs-lookup"><span data-stu-id="2995b-270">The resolution must be between 400 and 700px.</span></span>
> 
> <span data-ttu-id="2995b-271">**@mediaportáteis e (largura mínima: 20em), tela e (largura mínima: 20em) {...}:** para telas e portáteis (móvel e dispositivos).</span><span class="sxs-lookup"><span data-stu-id="2995b-271">**@media handheld and (min-width: 20em), screen and (min-width: 20em) { ... }:** For handhelds(mobile and devices) and screens.</span></span> <span data-ttu-id="2995b-272">A largura mínima deve ser maior que 20em.</span><span class="sxs-lookup"><span data-stu-id="2995b-272">The minimum width must be greater than 20em.</span></span>
> 
> <span data-ttu-id="2995b-273">Você pode encontrar mais informações sobre isso no [site do W3C](http://www.w3.org/TR/css3-mediaqueries/).</span><span class="sxs-lookup"><span data-stu-id="2995b-273">You can find more information about this on the [W3C site](http://www.w3.org/TR/css3-mediaqueries/).</span></span>


<span data-ttu-id="2995b-274">Agora você explorará como funciona a renderização adaptável, melhorando a legibilidade do ASP.NET MVC 4 padrão modelo de site.</span><span class="sxs-lookup"><span data-stu-id="2995b-274">You will now explore how the adaptive rendering works, improving the readability of the ASP.NET MVC 4 default website template.</span></span>

1. <span data-ttu-id="2995b-275">Abra o **PhotoGallery.sln** solução que você criou na tarefa 1 e selecione o **Galeriadefotos** projeto.</span><span class="sxs-lookup"><span data-stu-id="2995b-275">Open the **PhotoGallery.sln** solution you have created at Task 1 and select the **PhotoGallery** project.</span></span> <span data-ttu-id="2995b-276">Pressione **F5** para executar a solução.</span><span class="sxs-lookup"><span data-stu-id="2995b-276">Press **F5** to run the solution.</span></span>
2. <span data-ttu-id="2995b-277">Redimensione a largura do navegador, configuração do windows para a metade ou para menos de um quarto de seu tamanho original.</span><span class="sxs-lookup"><span data-stu-id="2995b-277">Resize the browser's width, setting the windows to half or to less than a quarter of its original size.</span></span> <span data-ttu-id="2995b-278">Observe o que acontece com os itens no cabeçalho: alguns elementos não aparecerá na área visível do cabeçalho.</span><span class="sxs-lookup"><span data-stu-id="2995b-278">Notice what happens with the items in the header: Some elements will not appear in the visible area of the header.</span></span>
3. <span data-ttu-id="2995b-279">Abra **Site.css** arquivo no Visual Studio Gerenciador de soluções, localizado em **conteúdo** pasta do projeto.</span><span class="sxs-lookup"><span data-stu-id="2995b-279">Open **Site.css** file from the Visual Studio Solution explorer, located in **Content** project folder.</span></span> <span data-ttu-id="2995b-280">Pressione **CTRL + F** para abrir a pesquisa integrada do Visual Studio e gravar  **@media**  para localizar o **consulta de mídia do CSS**.</span><span class="sxs-lookup"><span data-stu-id="2995b-280">Press **CTRL + F** to open Visual Studio integrated search, and write **@media** to locate the **CSS media query**.</span></span>

    <span data-ttu-id="2995b-281">A condição de consulta de mídia definida neste modelo funciona desta forma: quando o tamanho da janela do navegador é abaixo **850 px**, as regras de CSS aplicadas são aquelas definidas neste bloco de mídia.</span><span class="sxs-lookup"><span data-stu-id="2995b-281">The media query condition defined in this template works in this way: When the browser's window size is below **850 px**, the CSS rules applied are the ones defined inside this media block.</span></span>

    <span data-ttu-id="2995b-282">![Localizando a consulta de mídia](whats-new-in-aspnet-mvc-4/_static/image16.png "Localizando a consulta de mídia")</span><span class="sxs-lookup"><span data-stu-id="2995b-282">![Locating the media query](whats-new-in-aspnet-mvc-4/_static/image16.png "Locating the media query")</span></span>

    <span data-ttu-id="2995b-283">*Localizando a consulta de mídia*</span><span class="sxs-lookup"><span data-stu-id="2995b-283">*Locating the media query*</span></span>
4. <span data-ttu-id="2995b-284">Substitua o valor do atributo de largura máxima definido 850 px com **10px**, para desativar o processamento adaptável e pressione **CTRL + S** para salvar as alterações.</span><span class="sxs-lookup"><span data-stu-id="2995b-284">Replace the max-width attribute value set in 850 px with **10px**, in order to disable the adaptive rendering, and press **CTRL + S** to save the changes.</span></span> <span data-ttu-id="2995b-285">Voltar para o navegador e pressione **CTRL + F5** para atualizar a página com as alterações feitas por você.</span><span class="sxs-lookup"><span data-stu-id="2995b-285">Return to the browser and press **CTRL + F5** to refresh the page with the changes you have made.</span></span> <span data-ttu-id="2995b-286">Observe as diferenças entre as duas páginas ao ajustar a largura da janela.</span><span class="sxs-lookup"><span data-stu-id="2995b-286">Notice the differences in both pages when adjusting the width of the window.</span></span>

    <span data-ttu-id="2995b-287">![À esquerda, a página está aplicando o @media estilo, no canto direito, o estilo for omitido](whats-new-in-aspnet-mvc-4/_static/image17.png "à esquerda, a página está aplicando o @media estilo, no canto direito, o estilo for omitido")</span><span class="sxs-lookup"><span data-stu-id="2995b-287">![In the left, the page is applying the @media style, in the right, the style is omitted](whats-new-in-aspnet-mvc-4/_static/image17.png "In the left, the page is applying the @media style, in the right, the style is omitted")</span></span>

    <span data-ttu-id="2995b-288">*À esquerda, a página está aplicando o @media estilo, no canto direito, o estilo for omitido*</span><span class="sxs-lookup"><span data-stu-id="2995b-288">*In the left, the page is applying the @media style, in the right, the style is omitted*</span></span>

    <span data-ttu-id="2995b-289">Agora, vamos conferir o que acontece em dispositivos móveis:</span><span class="sxs-lookup"><span data-stu-id="2995b-289">Now, let's check out what happens on mobile devices:</span></span>

    <span data-ttu-id="2995b-290">![À esquerda, a página está aplicando o @media estilo, no canto direito, o estilo for omitido](whats-new-in-aspnet-mvc-4/_static/image18.png "à esquerda, a página está aplicando o @media estilo, no canto direito, o estilo for omitido")</span><span class="sxs-lookup"><span data-stu-id="2995b-290">![In the left, the page is applying the @media style, in the right, the style is omitted](whats-new-in-aspnet-mvc-4/_static/image18.png "In the left, the page is applying the @media style, in the right, the style is omitted")</span></span>

    <span data-ttu-id="2995b-291">*À esquerda, a página está aplicando o @media estilo, no canto direito, o estilo for omitido*</span><span class="sxs-lookup"><span data-stu-id="2995b-291">*In the left, the page is applying the @media style, in the right, the style is omitted*</span></span>

    <span data-ttu-id="2995b-292">Embora você observará que as alterações quando a página é processada em um navegador da Web não são muito significativas, ao usar um dispositivo móvel as diferenças se tornam mais óbvias.</span><span class="sxs-lookup"><span data-stu-id="2995b-292">Although you will notice that the changes when the page is rendered in a Web browser are not very significant, when using a mobile device the differences become more obvious.</span></span> <span data-ttu-id="2995b-293">No lado esquerdo da imagem, podemos ver que o estilo personalizado aprimorada a legibilidade.</span><span class="sxs-lookup"><span data-stu-id="2995b-293">On the left side of the image, we can see that the custom style improved the readability.</span></span>

    <span data-ttu-id="2995b-294">Renderização adaptável pode ser usada em muitos mais cenários, tornando mais fácil aplicar o estilo para um site da Web e resolver problemas comuns de estilo com uma abordagem interessante condicional.</span><span class="sxs-lookup"><span data-stu-id="2995b-294">Adaptive rendering can be used in many more scenarios, making it easier to apply conditional styling to a Web site and solving common style issues with a neat approach.</span></span>

    <span data-ttu-id="2995b-295">A marca meta do visor e consultas de mídia CSS não são específicas para ASP.NET MVC 4, portanto você pode tirar proveito desses recursos em um aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="2995b-295">The Viewport meta tag and CSS media queries are not specific to ASP.NET MVC 4, so you can take advantage of these features in any web application.</span></span>
5. <span data-ttu-id="2995b-296">No Visual Studio, pressione **SHIFT** + **F5** para parar a depuração do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2995b-296">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_the_Photo_Gallery_Web_Application"></a>
### <a name="exercise-2-creating-the-photo-gallery-web-application"></a><span data-ttu-id="2995b-297">Exercício 2: Criar o aplicativo Web de galeria de fotos</span><span class="sxs-lookup"><span data-stu-id="2995b-297">Exercise 2: Creating the Photo Gallery Web Application</span></span>

<span data-ttu-id="2995b-298">Neste exercício, você trabalhará em um aplicativo da Galeria de fotos para exibir fotos.</span><span class="sxs-lookup"><span data-stu-id="2995b-298">In this exercise, you will work on a Photo Gallery application to display photos.</span></span> <span data-ttu-id="2995b-299">Inicie com o modelo de projeto ASP.NET MVC 4 e, em seguida, você adicionará um recurso para recuperar fotos de um serviço e exibi-las na home page.</span><span class="sxs-lookup"><span data-stu-id="2995b-299">You will start with the ASP.NET MVC 4 project template, and then you will add a feature to retrieve photos from a service and display them in the home page.</span></span>

<span data-ttu-id="2995b-300">O exercício a seguir, você irá atualizar esta solução para aperfeiçoar a maneira como ele é exibido em dispositivos móveis.</span><span class="sxs-lookup"><span data-stu-id="2995b-300">In the following exercise, you will update this solution to enhance the way it is displayed on mobile devices.</span></span>

<a id="Task_1_-_Creating_a_Mock_Photo_Service"></a>
#### <a name="task-1---creating-a-mock-photo-service"></a><span data-ttu-id="2995b-301">Tarefa 1: Criando um serviço de simulação de fotos</span><span class="sxs-lookup"><span data-stu-id="2995b-301">Task 1 - Creating a Mock Photo Service</span></span>

<span data-ttu-id="2995b-302">Nesta tarefa, você criará uma simulação do serviço para recuperar o conteúdo que será exibido na Galeria de fotos.</span><span class="sxs-lookup"><span data-stu-id="2995b-302">In this task, you will create a mock of the photo service to retrieve the content that will be displayed in the gallery.</span></span> <span data-ttu-id="2995b-303">Para fazer isso, você adicionará um novo controlador que retornará apenas um arquivo JSON com os dados de cada foto.</span><span class="sxs-lookup"><span data-stu-id="2995b-303">To do this, you will add a new controller that will simply return a JSON file with the data of each photo.</span></span>

1. <span data-ttu-id="2995b-304">Abra **Visual Studio** se ainda não estiver aberto.</span><span class="sxs-lookup"><span data-stu-id="2995b-304">Open **Visual Studio** if not already opened.</span></span>
2. <span data-ttu-id="2995b-305">Selecione o **arquivo | Novo | Projeto** comando de menu.</span><span class="sxs-lookup"><span data-stu-id="2995b-305">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="2995b-306">No **novo projeto** caixa de diálogo, selecione o **Visual c# | Web** modelo no painel esquerdo da árvore e escolha **aplicativo Web do ASP.NET MVC 4.**</span><span class="sxs-lookup"><span data-stu-id="2995b-306">In the **New Project** dialog, select the **Visual C# | Web** template on the left pane tree, and choose **ASP.NET MVC 4 Web Application.**</span></span> <span data-ttu-id="2995b-307">Nomeie o projeto **Galeriadefotos**, selecione um local (ou deixe o padrão) e clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="2995b-307">Name the project **PhotoGallery**, select a location (or leave the default) and click **OK**.</span></span> <span data-ttu-id="2995b-308">Como alternativa, você pode continuar trabalhando de seu existente ASP.NET MVC 4 **aplicativo de Internet** solução de **Exercício 1** e passar para a próxima etapa.</span><span class="sxs-lookup"><span data-stu-id="2995b-308">Alternatively, you can continue working from your existing ASP.NET MVC 4 **Internet Application** solution from **Exercise 1** and skip the next step.</span></span>
3. <span data-ttu-id="2995b-309">No **novo projeto ASP.NET MVC 4** caixa de diálogo, selecione o **aplicativo de Internet** modelo de projeto e clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="2995b-309">In the **New ASP.NET MVC 4 Project** dialog box, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="2995b-310">Certifique-se de que ter selecionado como o mecanismo de exibição do Razor.</span><span class="sxs-lookup"><span data-stu-id="2995b-310">Make sure you have Razor selected as the View Engine.</span></span>
4. <span data-ttu-id="2995b-311">No **Solution Explorer**, com o botão direito do **aplicativo\_dados** pasta do seu projeto e selecione **adicionar | Item existente**.</span><span class="sxs-lookup"><span data-stu-id="2995b-311">In the **Solution Explorer**, right-click the **App\_Data** folder of your project, and select **Add | Existing Item**.</span></span> <span data-ttu-id="2995b-312">Navegue até o **Source\Assets\App\_dados** pasta deste laboratório e adicione o **Photos.json** arquivo.</span><span class="sxs-lookup"><span data-stu-id="2995b-312">Browse to the **Source\Assets\App\_Data** folder of this lab and add the **Photos.json** file.</span></span>
5. <span data-ttu-id="2995b-313">Criar um novo controlador com o nome **PhotoController**.</span><span class="sxs-lookup"><span data-stu-id="2995b-313">Create a new controller with the name **PhotoController**.</span></span> <span data-ttu-id="2995b-314">Para fazer isso, clique duas vezes no **controladores** pasta, vá para **adicionar** e selecione **controlador.**</span><span class="sxs-lookup"><span data-stu-id="2995b-314">To do this, right-click on the **Controllers** folder, go to **Add** and select **Controller.**</span></span> <span data-ttu-id="2995b-315">Complete o nome do controlador, deixe o **controlador MVC vazio** modelo e clique em **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="2995b-315">Complete the controller name, leave the **Empty MVC controller** template and click **Add**.</span></span>

    <span data-ttu-id="2995b-316">![Adicionando o PhotoController](whats-new-in-aspnet-mvc-4/_static/image19.png "adicionando o PhotoController")</span><span class="sxs-lookup"><span data-stu-id="2995b-316">![Adding the PhotoController](whats-new-in-aspnet-mvc-4/_static/image19.png "Adding the PhotoController")</span></span>

    <span data-ttu-id="2995b-317">*Adicionando o PhotoController*</span><span class="sxs-lookup"><span data-stu-id="2995b-317">*Adding the PhotoController*</span></span>
6. <span data-ttu-id="2995b-318">Substitua o **índice** método com o seguinte **galeria** ação e retornar o conteúdo do arquivo JSON que você recentemente adicionou ao projeto.</span><span class="sxs-lookup"><span data-stu-id="2995b-318">Replace the **Index** method with the following **Gallery** action, and return the content from the JSON file you have recently added to the project.</span></span>

    <span data-ttu-id="2995b-319">(Código de trecho - *ação de galeria do ASP.NET MVC 4 laboratório - Ex02 -*)</span><span class="sxs-lookup"><span data-stu-id="2995b-319">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Gallery Action*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample4.cs)]
7. <span data-ttu-id="2995b-320">Pressione **F5** para executar a solução e, em seguida, navegue até a URL a seguir para testar o serviço de foto fictícias: `http://localhost:[port]/photo/gallery` (o valor [port] depende da porta atual em que o aplicativo foi iniciado).</span><span class="sxs-lookup"><span data-stu-id="2995b-320">Press **F5** to run the solution, and then browse to the following URL in order to test the mocked photo service: `http://localhost:[port]/photo/gallery` (the [port] value depends on the current port where the application was launched).</span></span> <span data-ttu-id="2995b-321">A solicitação para essa URL deve recuperar o conteúdo a **Photos.json** arquivo.</span><span class="sxs-lookup"><span data-stu-id="2995b-321">The request to this URL should retrieve the content of the **Photos.json** file.</span></span>

    <span data-ttu-id="2995b-322">![Testando o serviço de foto fictícios](whats-new-in-aspnet-mvc-4/_static/image20.png "testando o serviço de foto fictícios")</span><span class="sxs-lookup"><span data-stu-id="2995b-322">![Testing the mocked photo service](whats-new-in-aspnet-mvc-4/_static/image20.png "Testing the mocked photo service")</span></span>

    <span data-ttu-id="2995b-323">*Testando o serviço de foto fictícios*</span><span class="sxs-lookup"><span data-stu-id="2995b-323">*Testing the mocked photo service*</span></span>

<span data-ttu-id="2995b-324">Em uma implementação real, você pode usar [ASP.NET Web API](../../../../web-api/index.md) para implementar o serviço de galeria de fotos.</span><span class="sxs-lookup"><span data-stu-id="2995b-324">In a real implementation you could use [ASP.NET Web API](../../../../web-api/index.md) to implement the Photo Gallery service.</span></span> <span data-ttu-id="2995b-325">API da Web do ASP.NET é uma estrutura que torna mais fácil criar serviços HTTP que alcançam uma ampla gama de clientes, incluindo navegadores e dispositivos móveis.</span><span class="sxs-lookup"><span data-stu-id="2995b-325">ASP.NET Web API is a framework that makes it easy to build HTTP services that reach a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="2995b-326">A API do ASP.NET Web é uma plataforma ideal para criar aplicativos com REST no .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="2995b-326">ASP.NET Web API is an ideal platform for building RESTful applications on the .NET Framework.</span></span>

<a id="Task_2_-_Displaying_the_Photo_Gallery"></a>
#### <a name="task-2---displaying-the-photo-gallery"></a><span data-ttu-id="2995b-327">Tarefa 2 - exibindo a Galeria de fotos</span><span class="sxs-lookup"><span data-stu-id="2995b-327">Task 2 - Displaying the Photo Gallery</span></span>

<span data-ttu-id="2995b-328">Nesta tarefa, você atualizará a página inicial para mostrar a Galeria de fotos usando o serviço fictícios que você criou na primeira tarefa deste exercício.</span><span class="sxs-lookup"><span data-stu-id="2995b-328">In this task, you will update the Home page to show the photo gallery by using the mocked service you created in the first task of this exercise.</span></span> <span data-ttu-id="2995b-329">Você adicionará os arquivos de modelo e atualizar os modos de exibição de galeria.</span><span class="sxs-lookup"><span data-stu-id="2995b-329">You will add model files and update the gallery views.</span></span>

1. <span data-ttu-id="2995b-330">No Visual Studio, pressione **SHIFT** + **F5** para parar a depuração do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2995b-330">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>
2. <span data-ttu-id="2995b-331">Criar o **foto** classe no **modelos** pasta.</span><span class="sxs-lookup"><span data-stu-id="2995b-331">Create the **Photo** class in the **Models** folder.</span></span> <span data-ttu-id="2995b-332">Para fazer isso, clique duas vezes no **modelos** pasta, selecione **adicionar** e clique em **classe**.</span><span class="sxs-lookup"><span data-stu-id="2995b-332">To do this, right-click on the **Models** folder, select **Add** and click **Class**.</span></span> <span data-ttu-id="2995b-333">Em seguida, defina o nome como **Photo.cs** e clique em **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="2995b-333">Then, set the name to **Photo.cs** and click **Add**.</span></span>
3. <span data-ttu-id="2995b-334">Adicionar os seguintes membros para o **foto** classe.</span><span class="sxs-lookup"><span data-stu-id="2995b-334">Add the following members to the **Photo** class.</span></span>

    <span data-ttu-id="2995b-335">(Código de trecho - *modelo foto do ASP.NET MVC 4 laboratório - Ex02 -*)</span><span class="sxs-lookup"><span data-stu-id="2995b-335">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Photo model*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample5.cs)]
4. <span data-ttu-id="2995b-336">Abra o **HomeController** arquivo o **controladores** pasta.</span><span class="sxs-lookup"><span data-stu-id="2995b-336">Open the **HomeController.cs** file from the **Controllers** folder.</span></span>
5. <span data-ttu-id="2995b-337">Adicione o seguinte usando instruções.</span><span class="sxs-lookup"><span data-stu-id="2995b-337">Add the following using statements.</span></span>

    <span data-ttu-id="2995b-338">(Código de trecho - *usos do ASP.NET MVC 4 laboratório - Ex02 - HomeController*)</span><span class="sxs-lookup"><span data-stu-id="2995b-338">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - HomeController Usings*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample6.cs)]
6. <span data-ttu-id="2995b-339">Atualização de **índice** ação a ser usada **HttpClient** para recuperar os dados da galeria e, em seguida, use o **JavaScriptSerializer** desserializá-lo para o modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="2995b-339">Update the **Index** action to use **HttpClient** to retrieve the gallery data, and then use the **JavaScriptSerializer** to deserialize it to the view model.</span></span>

    <span data-ttu-id="2995b-340">(Código de trecho - *ação de índice do ASP.NET MVC 4 laboratório - Ex02 -*)</span><span class="sxs-lookup"><span data-stu-id="2995b-340">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Index Action*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample7.cs)]
7. <span data-ttu-id="2995b-341">Abra o **cshtml** arquivo localizado sob o **Views\Home** pasta e substitua todo o conteúdo com o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="2995b-341">Open the **Index.cshtml** file located under the **Views\Home** folder and replace all the content with the following code.</span></span>

    <span data-ttu-id="2995b-342">Esse código percorre todas as fotos recuperadas do serviço e os exibe em uma lista não ordenada.</span><span class="sxs-lookup"><span data-stu-id="2995b-342">This code loops through all the photos retrieved from the service and displays them into an unordered list.</span></span>

    <span data-ttu-id="2995b-343">(Código de trecho - *lista de fotos do ASP.NET MVC 4 laboratório - Ex02 -*)</span><span class="sxs-lookup"><span data-stu-id="2995b-343">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Photo List*)</span></span>


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample8.cshtml)]
8. <span data-ttu-id="2995b-344">No **Solution Explorer**, com o botão direito do **conteúdo** pasta do seu projeto e selecione **adicionar | Item existente**.</span><span class="sxs-lookup"><span data-stu-id="2995b-344">In the **Solution Explorer**, right-click the **Content** folder of your project, and select **Add | Existing Item**.</span></span> <span data-ttu-id="2995b-345">Navegue até o **Source\Assets\Content** pasta deste laboratório e adicione o **Site.css** arquivo.</span><span class="sxs-lookup"><span data-stu-id="2995b-345">Browse to the **Source\Assets\Content** folder of this lab and add the **Site.css** file.</span></span> <span data-ttu-id="2995b-346">Você precisará confirmar sua substituição.</span><span class="sxs-lookup"><span data-stu-id="2995b-346">You will have to confirm its replacement.</span></span> <span data-ttu-id="2995b-347">Se você tiver o **Site.css** arquivo aberto, você precisará confirmar para recarregar o arquivo também.</span><span class="sxs-lookup"><span data-stu-id="2995b-347">If you have the **Site.css** file open, you will have to confirm to reload the file also.</span></span>
9. <span data-ttu-id="2995b-348">Abra o Explorador de arquivos e copie todo o **fotos** pasta localizado sob o **Source\Assets** pasta deste laboratório para a pasta raiz do seu projeto no Gerenciador de soluções.</span><span class="sxs-lookup"><span data-stu-id="2995b-348">Open File Explorer and copy the entire **Photos** folder located under the **Source\Assets** folder of this lab to the root folder of your project in Solution Explorer.</span></span>
10. <span data-ttu-id="2995b-349">Execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2995b-349">Run the application.</span></span> <span data-ttu-id="2995b-350">Agora você deve ver a home page exibe as fotos na Galeria.</span><span class="sxs-lookup"><span data-stu-id="2995b-350">You should now see the home page displaying the photos in the gallery.</span></span>

    <span data-ttu-id="2995b-351">![Galeria de fotos](whats-new-in-aspnet-mvc-4/_static/image21.png "Galeria de fotos")</span><span class="sxs-lookup"><span data-stu-id="2995b-351">![Photo Gallery](whats-new-in-aspnet-mvc-4/_static/image21.png "Photo Gallery")</span></span>

    <span data-ttu-id="2995b-352">*Galeria de fotos*</span><span class="sxs-lookup"><span data-stu-id="2995b-352">*Photo Gallery*</span></span>
11. <span data-ttu-id="2995b-353">No Visual Studio, pressione **SHIFT** + **F5** para parar a depuração do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2995b-353">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Adding_support_for_mobile_devices"></a>
### <a name="exercise-3-adding-support-for-mobile-devices"></a><span data-ttu-id="2995b-354">Exercício 3: Adicionando suporte para dispositivos móveis</span><span class="sxs-lookup"><span data-stu-id="2995b-354">Exercise 3: Adding support for mobile devices</span></span>

<span data-ttu-id="2995b-355">Uma das atualizações de chave no ASP.NET MVC 4 é o suporte para desenvolvimento móvel.</span><span class="sxs-lookup"><span data-stu-id="2995b-355">One of the key updates in ASP.NET MVC 4 is the support for mobile development.</span></span> <span data-ttu-id="2995b-356">Neste exercício, você irá explorar os novos recursos do ASP.NET MVC 4 para aplicativos móveis ao estender a solução Galeriadefotos que você criou no exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="2995b-356">In this exercise, you will explore ASP.NET MVC 4 new features for mobile applications by extending the PhotoGallery solution you have created in the previous exercise.</span></span>

<a id="Task_1_-_Installing_jQuery_Mobile_in_an_ASPNET_MVC_4_Application"></a>
#### <a name="task-1---installing-jquery-mobile-in-an-aspnet-mvc-4-application"></a><span data-ttu-id="2995b-357">Tarefa 1: instalar o jQuery Mobile em um aplicativo do ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="2995b-357">Task 1 - Installing jQuery Mobile in an ASP.NET MVC 4 Application</span></span>

1. <span data-ttu-id="2995b-358">Abra o **começar** solução localizado em **fonte/Ex3-MobileSupport/Begin/** pasta.</span><span class="sxs-lookup"><span data-stu-id="2995b-358">Open the **Begin** solution located at **Source/Ex3-MobileSupport/Begin/** folder.</span></span> <span data-ttu-id="2995b-359">Caso contrário, você pode continuar usando o **final** solução obtido executando o exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="2995b-359">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="2995b-360">Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="2995b-360">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="2995b-361">Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="2995b-361">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="2995b-362">No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="2995b-362">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="2995b-363">Por fim, compile a solução clicando **criar** | **compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="2995b-363">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2995b-364">Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="2995b-364">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="2995b-365">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="2995b-365">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="2995b-366">É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.</span><span class="sxs-lookup"><span data-stu-id="2995b-366">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="2995b-367">Abra o **Package Manager Console** clicando o **ferramentas** &gt; **Gerenciador de biblioteca de pacote** &gt; **Package Manager Console** opção de menu.</span><span class="sxs-lookup"><span data-stu-id="2995b-367">Open the **Package Manager Console** by clicking the **Tools** &gt; **Library Package Manager** &gt; **Package Manager Console** menu option.</span></span>

    <span data-ttu-id="2995b-368">![Abrindo o NuGet Package Manager Console](whats-new-in-aspnet-mvc-4/_static/image22.png "abrindo o NuGet Package Manager Console")</span><span class="sxs-lookup"><span data-stu-id="2995b-368">![Opening the NuGet Package Manager Console](whats-new-in-aspnet-mvc-4/_static/image22.png "Opening the NuGet Package Manager Console")</span></span>

    <span data-ttu-id="2995b-369">*Abrindo o Console do Gerenciador de pacote do NuGet*</span><span class="sxs-lookup"><span data-stu-id="2995b-369">*Opening the NuGet Package Manager Console*</span></span>
3. <span data-ttu-id="2995b-370">No Console do Gerenciador de pacotes, execute o seguinte comando para instalar o **jQuery.Mobile.MVC** pacote.</span><span class="sxs-lookup"><span data-stu-id="2995b-370">In the Package Manager Console run the following command to install the **jQuery.Mobile.MVC** package.</span></span>

    <span data-ttu-id="2995b-371">jQuery Mobile é uma biblioteca de código aberto para a criação de interface da web otimizada para toque.</span><span class="sxs-lookup"><span data-stu-id="2995b-371">jQuery Mobile is an open source library for building touch-optimized web UI.</span></span> <span data-ttu-id="2995b-372">O pacote jQuery.Mobile.MVC NuGet inclui auxiliares para usar o jQuery Mobile com um aplicativo ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="2995b-372">The jQuery.Mobile.MVC NuGet package includes helpers to use jQuery Mobile with an ASP.NET MVC 4 application.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2995b-373">Ao executar o comando a seguir, você irá ser baixando a biblioteca de jQuery.Mobile.MVC do Nuget.</span><span class="sxs-lookup"><span data-stu-id="2995b-373">By running the following command, you will be downloading the jQuery.Mobile.MVC library from Nuget.</span></span>

    <span data-ttu-id="2995b-374">PM</span><span class="sxs-lookup"><span data-stu-id="2995b-374">PM</span></span>

    [!code-powershell[Main](whats-new-in-aspnet-mvc-4/samples/sample9.ps1)]

    <span data-ttu-id="2995b-375">Esse comando instala o jQuery Mobile e alguns arquivos auxiliares, incluindo o seguinte:</span><span class="sxs-lookup"><span data-stu-id="2995b-375">This command installs jQuery Mobile and some helper files, including the following:</span></span>

    - <span data-ttu-id="2995b-376">**Exibições/compartilhadas/\_Layout.Mobile.cshtml**: é um layout com base em Mobile jQuery otimizado para uma tela menor.</span><span class="sxs-lookup"><span data-stu-id="2995b-376">**Views/Shared/\_Layout.Mobile.cshtml**: is a jQuery Mobile-based layout optimized for a smaller screen.</span></span> <span data-ttu-id="2995b-377">Quando o site recebe uma solicitação de um navegador móvel, ele substituirá o layout original (\_cshtml) com este.</span><span class="sxs-lookup"><span data-stu-id="2995b-377">When the website receives a request from a mobile browser, it will replace the original layout (\_Layout.cshtml) with this one.</span></span>
    - <span data-ttu-id="2995b-378">Um componente de alternância de exibição: consiste de **exibições/compartilhadas/\_ViewSwitcher.cshtml** exibição parcial e o **ViewSwitcherController.cs** controlador.</span><span class="sxs-lookup"><span data-stu-id="2995b-378">A view-switcher component: consists of the **Views/Shared/\_ViewSwitcher.cshtml** partial view and the **ViewSwitcherController.cs** controller.</span></span> <span data-ttu-id="2995b-379">Este componente exibirá um link em navegadores de dispositivos móveis para permitir que os usuários alternar para a versão da área de trabalho da página.</span><span class="sxs-lookup"><span data-stu-id="2995b-379">This component will show a link on mobile browsers to enable users to switch to the desktop version of the page.</span></span>  
        <span data-ttu-id="2995b-380">![Projeto da Galeria de fotos com suporte móvel](whats-new-in-aspnet-mvc-4/_static/image23.png "projeto da Galeria de fotos com suporte móvel")</span><span class="sxs-lookup"><span data-stu-id="2995b-380">![Photo Gallery project with mobile support](whats-new-in-aspnet-mvc-4/_static/image23.png "Photo Gallery project with mobile support")</span></span>

        <span data-ttu-id="2995b-381">*Projeto da Galeria de fotos com suporte móvel*</span><span class="sxs-lookup"><span data-stu-id="2995b-381">*Photo Gallery project with mobile support*</span></span>
4. <span data-ttu-id="2995b-382">Registre os pacotes móveis.</span><span class="sxs-lookup"><span data-stu-id="2995b-382">Register the Mobile bundles.</span></span> <span data-ttu-id="2995b-383">Para fazer isso, abra o **Global.asax.cs** e adicione a linha a seguir.</span><span class="sxs-lookup"><span data-stu-id="2995b-383">To do this, open the **Global.asax.cs** file and add the following line.</span></span>

    <span data-ttu-id="2995b-384">(Código de trecho - *pacotes móvel do ASP.NET MVC 4 laboratório - Ex03 - Register*)</span><span class="sxs-lookup"><span data-stu-id="2995b-384">(Code Snippet - *ASP.NET MVC 4 Lab - Ex03 - Register Mobile Bundles*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample10.cs)]
5. <span data-ttu-id="2995b-385">Execute o aplicativo usando um navegador da web da área de trabalho.</span><span class="sxs-lookup"><span data-stu-id="2995b-385">Run the application using a desktop web browser.</span></span>
6. <span data-ttu-id="2995b-386">Abra o **emulador do Windows Phone 7,** localizado em **Menu Iniciar | Todos os programas | Windows Phone SDK 7.1 | Emulador do Windows Phone.**</span><span class="sxs-lookup"><span data-stu-id="2995b-386">Open the **Windows Phone 7 Emulator,** located in **Start Menu | All Programs | Windows Phone SDK 7.1 | Windows Phone Emulator.**</span></span>
7. <span data-ttu-id="2995b-387">Na tela de início do telefone, abra o Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="2995b-387">In the phone start screen, open Internet Explorer.</span></span> <span data-ttu-id="2995b-388">Verifique a URL onde o aplicativo é iniciado e navegue até essa URL com o navegador do telefone (por exemplo, `http://localhost:[PortNumber]/`).</span><span class="sxs-lookup"><span data-stu-id="2995b-388">Check out the URL where the application started and browse to that URL with the phone browser (e.g. `http://localhost:[PortNumber]/`).</span></span>

    <span data-ttu-id="2995b-389">Você observará que seu aplicativo terá uma aparência diferente no emulador do Windows Phone, como o jQuery.Mobile.MVC criou novos ativos em seu projeto que mostram exibições otimizadas para dispositivos móveis.</span><span class="sxs-lookup"><span data-stu-id="2995b-389">You will notice that your application will look different in the Windows Phone emulator, as the jQuery.Mobile.MVC has created new assets in your project that show views optimized for mobile devices.</span></span>

    <span data-ttu-id="2995b-390">Observe a mensagem na parte superior do telefone, mostrando o link que alterna para o modo de exibição da área de trabalho.</span><span class="sxs-lookup"><span data-stu-id="2995b-390">Notice the message at the top of the phone, showing the link that switches to the Desktop view.</span></span> <span data-ttu-id="2995b-391">Além disso, o  **\_Layout.Mobile.cshtml** layout que foi criado com o pacote que você instalou incluem um layout diferente no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2995b-391">Additionally, the **\_Layout.Mobile.cshtml** layout that was created by the package you have installed is including a different layout in the application.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2995b-392">Até agora, há um link para voltar ao modo de exibição móvel.</span><span class="sxs-lookup"><span data-stu-id="2995b-392">So far, there is no link to get back to mobile view.</span></span> <span data-ttu-id="2995b-393">Ele será incluído em versões posteriores.</span><span class="sxs-lookup"><span data-stu-id="2995b-393">It will be included in later versions.</span></span>

    <span data-ttu-id="2995b-394">![Modo de exibição móvel da página inicial da Galeria de fotos](whats-new-in-aspnet-mvc-4/_static/image24.png "exibição móvel da página inicial da Galeria de fotos")</span><span class="sxs-lookup"><span data-stu-id="2995b-394">![Mobile view of the Photo Gallery Home page](whats-new-in-aspnet-mvc-4/_static/image24.png "Mobile view of the Photo Gallery Home page")</span></span>

    <span data-ttu-id="2995b-395">*Modo de exibição móvel da página inicial da Galeria de fotos*</span><span class="sxs-lookup"><span data-stu-id="2995b-395">*Mobile view of the Photo Gallery Home page*</span></span>
8. <span data-ttu-id="2995b-396">No Visual Studio, pressione **SHIFT** + **F5** para parar a depuração do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2995b-396">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>

<a id="Task_2_-_Creating_Mobile_Views"></a>
#### <a name="task-2---creating-mobile-views"></a><span data-ttu-id="2995b-397">Tarefa 2: Criando modos de exibição móvel</span><span class="sxs-lookup"><span data-stu-id="2995b-397">Task 2 - Creating Mobile Views</span></span>

<span data-ttu-id="2995b-398">Nesta tarefa, você criará uma versão móvel do modo de exibição de índice com conteúdo adaptado para appareance melhor em dispositivos móveis.</span><span class="sxs-lookup"><span data-stu-id="2995b-398">In this task, you will create a mobile version of the index view with content adapted for better appareance in mobile devices.</span></span>

1. <span data-ttu-id="2995b-399">Copie o **Views\Home\Index.cshtml** exibir e cole-o para criar uma cópia, renomeie o novo arquivo **Index.Mobile.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="2995b-399">Copy the **Views\Home\Index.cshtml** view and paste it to create a copy, rename the new file to **Index.Mobile.cshtml**.</span></span>
2. <span data-ttu-id="2995b-400">Abra o novo criado **Index.Mobile.cshtml** exibir e substituir o &lt;ul&gt; marca com este código.</span><span class="sxs-lookup"><span data-stu-id="2995b-400">Open the new created **Index.Mobile.cshtml** view and replace the existing &lt;ul&gt; tag with this code.</span></span> <span data-ttu-id="2995b-401">Ao fazer isso, você atualizará o &lt;ul&gt; marca com anotações de dados móveis jQuery para usar os temas móveis do jQuery.</span><span class="sxs-lookup"><span data-stu-id="2995b-401">By doing this, you will be updating the &lt;ul&gt; tag with jQuery Mobile data annotations to use the mobile themes from jQuery.</span></span>


    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample11.html)]

    > [!NOTE] 
    > 
    > <span data-ttu-id="2995b-402">Observe que:</span><span class="sxs-lookup"><span data-stu-id="2995b-402">Notice that:</span></span>
    > 
    > - <span data-ttu-id="2995b-403">O **função dados** atributo definido como **listview** irá renderizar a lista usando estilos de listview.</span><span class="sxs-lookup"><span data-stu-id="2995b-403">The **data-role** attribute set to **listview** will render the list using the listview styles.</span></span>
    > 
    > - <span data-ttu-id="2995b-404">O **inserção de dados** atributo definido como verdadeiro mostrará a lista com borda arredondada e margem.</span><span class="sxs-lookup"><span data-stu-id="2995b-404">The **data-inset** attribute set to true will show the list with rounded border and margin.</span></span>
    > 
    > - <span data-ttu-id="2995b-405">O **filtro de dados** atributo definido como **true** irá gerar uma caixa de pesquisa.</span><span class="sxs-lookup"><span data-stu-id="2995b-405">The **data-filter** attribute set to **true** will generate a search box.</span></span>
    > 
    > <span data-ttu-id="2995b-406">Você pode aprender mais sobre as convenções de jQuery Mobile na documentação do projeto: [ [http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)</span><span class="sxs-lookup"><span data-stu-id="2995b-406">You can learn more about jQuery Mobile conventions in the project documentation: [[http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)</span></span>
3. <span data-ttu-id="2995b-407">Pressione **CTRL + S** para salvar as alterações.</span><span class="sxs-lookup"><span data-stu-id="2995b-407">Press **CTRL + S** to save the changes.</span></span>
4. <span data-ttu-id="2995b-408">Alterne para o **emulador do Windows Phone** e atualizar o site.</span><span class="sxs-lookup"><span data-stu-id="2995b-408">Switch to the **Windows Phone Emulator** and refresh the site.</span></span> <span data-ttu-id="2995b-409">Observe a nova aparência da lista da galeria, bem como a nova caixa de pesquisa localizada na parte superior.</span><span class="sxs-lookup"><span data-stu-id="2995b-409">Notice the new look and feel of the gallery list, as well as the new search box located on the top.</span></span> <span data-ttu-id="2995b-410">Em seguida, digite uma palavra na caixa de pesquisa (por exemplo, **Tulips**) para iniciar uma pesquisa na Galeria de fotos.</span><span class="sxs-lookup"><span data-stu-id="2995b-410">Then, type a word in the search box (for instance, **Tulips**) to start a search in the photo gallery.</span></span>

    <span data-ttu-id="2995b-411">![Galeria usando o estilo de listview com filtragem](whats-new-in-aspnet-mvc-4/_static/image25.png "galeria usando o estilo de listview com filtragem")</span><span class="sxs-lookup"><span data-stu-id="2995b-411">![Gallery using listview style with filtering](whats-new-in-aspnet-mvc-4/_static/image25.png "Gallery using listview style with filtering")</span></span>

    <span data-ttu-id="2995b-412">*Usando o estilo de listview com a filtragem de galeria*</span><span class="sxs-lookup"><span data-stu-id="2995b-412">*Gallery using listview style with filtering*</span></span>

    <span data-ttu-id="2995b-413">Para resumir, você usou a receita mobilizador de exibição para criar uma cópia do modo de exibição de índice com o &quot;móvel&quot; sufixo.</span><span class="sxs-lookup"><span data-stu-id="2995b-413">To summarize, you have used the View Mobilizer recipe to create a copy of the Index view with the &quot;mobile&quot; suffix.</span></span> <span data-ttu-id="2995b-414">Este sufixo indica ao ASP.NET MVC 4 que cada solicitação gerada a partir de um dispositivo móvel usará esta cópia do índice.</span><span class="sxs-lookup"><span data-stu-id="2995b-414">This suffix indicates to ASP.NET MVC 4 that every request generated from a mobile device will use this copy of the index.</span></span> <span data-ttu-id="2995b-415">Além disso, você atualizou a versão móvel do modo de exibição de índice para usar o jQuery Mobile para melhorar a aparência e funcionalidade do site em dispositivos móveis.</span><span class="sxs-lookup"><span data-stu-id="2995b-415">Additionally, you have updated the mobile version of the Index view to use jQuery Mobile for enhancing the site look and feel in mobile devices.</span></span>
5. <span data-ttu-id="2995b-416">Volte para o Visual Studio e abra **Site.Mobile.css** localizado sob o **conteúdo** pasta.</span><span class="sxs-lookup"><span data-stu-id="2995b-416">Go back to Visual Studio and open **Site.Mobile.css** located under the **Content** folder.</span></span>
6. <span data-ttu-id="2995b-417">Corrija o posicionamento do título da foto para mostrá-la no lado direito da imagem.</span><span class="sxs-lookup"><span data-stu-id="2995b-417">Fix the positioning of the photo title to make it show at the right side of the image.</span></span> <span data-ttu-id="2995b-418">Para fazer isso, adicione o seguinte código para o **Site.Mobile.css** arquivo.</span><span class="sxs-lookup"><span data-stu-id="2995b-418">To do this, add the following code to the **Site.Mobile.css** file.</span></span>

    <span data-ttu-id="2995b-419">CSS</span><span class="sxs-lookup"><span data-stu-id="2995b-419">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-mvc-4/samples/sample12.css)]
7. <span data-ttu-id="2995b-420">Pressione **CTRL + S** para salvar as alterações.</span><span class="sxs-lookup"><span data-stu-id="2995b-420">Press **CTRL + S** to save the changes.</span></span>
8. <span data-ttu-id="2995b-421">Volte para o **emulador do Windows Phone** e atualizar o site.</span><span class="sxs-lookup"><span data-stu-id="2995b-421">Switch back to the **Windows Phone Emulator** and refresh the site.</span></span> <span data-ttu-id="2995b-422">Observe que o título de foto corretamente é posicionado agora.</span><span class="sxs-lookup"><span data-stu-id="2995b-422">Notice the photo title is properly positioned now.</span></span>

    <span data-ttu-id="2995b-423">![Título posicionado no lado direito da imagem](whats-new-in-aspnet-mvc-4/_static/image26.png "título posicionado no lado direito da imagem")</span><span class="sxs-lookup"><span data-stu-id="2995b-423">![Title positioned on the right side of the image](whats-new-in-aspnet-mvc-4/_static/image26.png "Title positioned on the right side of the image")</span></span>

    <span data-ttu-id="2995b-424">*Título posicionado no lado direito da imagem*</span><span class="sxs-lookup"><span data-stu-id="2995b-424">*Title positioned on the right side of the image*</span></span>

<a id="Task_3_-_jQuery_Mobile_Themes"></a>
#### <a name="task-3---jquery-mobile-themes"></a><span data-ttu-id="2995b-425">Tarefa 3 - jQuery Mobile temas</span><span class="sxs-lookup"><span data-stu-id="2995b-425">Task 3 - jQuery Mobile Themes</span></span>

<span data-ttu-id="2995b-426">Cada layout e o widget em jQuery Mobile foi projetada para um novo orientada a objeto CSS do framework que seja possível aplicar um tema concluída design visual unificada para sites e aplicativos.</span><span class="sxs-lookup"><span data-stu-id="2995b-426">Every layout and widget in jQuery Mobile is designed around a new object-oriented CSS framework that makes it possible to apply a complete unified visual design theme to sites and applications.</span></span>

<span data-ttu-id="2995b-427">padrão do jQuery Mobile tema inclui 5 amostras que recebem letras (a, b, c, r, e) para referência rápida.</span><span class="sxs-lookup"><span data-stu-id="2995b-427">jQuery Mobile's default Theme includes 5 swatches that are given letters (a, b, c, d, e) for quick reference.</span></span>

<span data-ttu-id="2995b-428">Nesta tarefa, você atualizará o layout móvel para usar um tema diferente do padrão.</span><span class="sxs-lookup"><span data-stu-id="2995b-428">In this task, you will update the mobile layout to use a different theme than the default.</span></span>

1. <span data-ttu-id="2995b-429">Alterne para o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2995b-429">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="2995b-430">Abra o  **\_Layout.Mobile.cshtml** arquivo localizado em **exibições \ compartilhadas**.</span><span class="sxs-lookup"><span data-stu-id="2995b-430">Open the **\_Layout.Mobile.cshtml** file located in **Views\Shared**.</span></span>
3. <span data-ttu-id="2995b-431">Localizar o elemento div com a função de dados definida como &quot;página&quot; e atualize o **dados tema** para &quot; **e**&quot;.</span><span class="sxs-lookup"><span data-stu-id="2995b-431">Find the div element with the data-role set to &quot;page&quot; and update the **data-theme** to &quot;**e**&quot;.</span></span>


    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample13.html)]
4. <span data-ttu-id="2995b-432">Pressione **CTRL + S** para salvar as alterações.</span><span class="sxs-lookup"><span data-stu-id="2995b-432">Press **CTRL + S** to save the changes.</span></span>
5. <span data-ttu-id="2995b-433">Atualizar o site no **emulador do Windows Phone** e observe o novo esquema de cores.</span><span class="sxs-lookup"><span data-stu-id="2995b-433">Refresh the site in the **Windows Phone Emulator** and notice the new colors scheme.</span></span>

    <span data-ttu-id="2995b-434">![Layout para dispositivos móveis com um esquema de cores diferentes](whats-new-in-aspnet-mvc-4/_static/image27.png "layout com um esquema de cores diferentes para dispositivos móveis")</span><span class="sxs-lookup"><span data-stu-id="2995b-434">![Mobile layout with a different color scheme](whats-new-in-aspnet-mvc-4/_static/image27.png "Mobile layout with a different color scheme")</span></span>

    <span data-ttu-id="2995b-435">*Layout para dispositivos móveis com um esquema de cores diferentes*</span><span class="sxs-lookup"><span data-stu-id="2995b-435">*Mobile layout with a different color scheme*</span></span>

<a id="Task_4_-_Using_the_View-Switcher_Component_and_the_Browser_Overriding_Features"></a>
#### <a name="task-4---using-the-view-switcher-component-and-the-browser-overriding-features"></a><span data-ttu-id="2995b-436">Tarefa 4 - usando o componente de alternância de exibição e o navegador substituindo recursos</span><span class="sxs-lookup"><span data-stu-id="2995b-436">Task 4 - Using the View-Switcher Component and the Browser Overriding Features</span></span>

<span data-ttu-id="2995b-437">É uma convenção mobile otimizado páginas da web para adicionar um link cujo texto é algo como o modo de exibição da área de trabalho ou modo completo do site que permite aos usuários a alternar para uma versão de área de trabalho da página.</span><span class="sxs-lookup"><span data-stu-id="2995b-437">A convention for mobile-optimized web pages is to add a link whose text is something like Desktop view or Full site mode that lets users switch to a desktop version of the page.</span></span> <span data-ttu-id="2995b-438">O pacote jQuery.Mobile.MVC inclui um exemplo **alternância de exibição** componente para essa finalidade usada no  **\_Layout.Mobile.cshtml** exibição.</span><span class="sxs-lookup"><span data-stu-id="2995b-438">The jQuery.Mobile.MVC package includes a sample **view-switcher** component for this purpose used in the **\_Layout.Mobile.cshtml** view.</span></span>

<span data-ttu-id="2995b-439">![Link para alternar para modo de área de trabalho](whats-new-in-aspnet-mvc-4/_static/image28.png "Link para alternar para modo de área de trabalho")</span><span class="sxs-lookup"><span data-stu-id="2995b-439">![Link to switch to Desktop View](whats-new-in-aspnet-mvc-4/_static/image28.png "Link to switch to Desktop View")</span></span>

<span data-ttu-id="2995b-440">*Para alternar para modo de exibição da área de trabalho*</span><span class="sxs-lookup"><span data-stu-id="2995b-440">*Link to switch to Desktop View*</span></span>

<span data-ttu-id="2995b-441">A alternância de exibição usa um novo recurso chamado **navegador substituindo**.</span><span class="sxs-lookup"><span data-stu-id="2995b-441">The view switcher uses a new feature called **Browser Overriding**.</span></span> <span data-ttu-id="2995b-442">Esse recurso permite que seu aplicativo lide com solicitações como se estivessem vindo de um navegador diferente (agente do usuário) que eles realmente são provenientes.</span><span class="sxs-lookup"><span data-stu-id="2995b-442">This feature lets your application treat requests as if they were coming from a different browser (user agent) than the one they are actually coming from.</span></span>

<span data-ttu-id="2995b-443">Nesta tarefa, você irá explorar a implementação de exemplo de um alternador de exibição adicionado jQuery.Mobile.MVC e o novo navegador substituindo recursos no ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="2995b-443">In this task, you will explore the sample implementation of a view-switcher added by jQuery.Mobile.MVC and the new browser overriding features in ASP.NET MVC 4.</span></span>

1. <span data-ttu-id="2995b-444">Alterne para o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2995b-444">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="2995b-445">Abra o  **\_Layout.Mobile.cshtml** exibição localizado sob o **exibições \ compartilhadas** pasta e observe o componente de alternância de exibição que está sendo referenciado como uma exibição parcial.</span><span class="sxs-lookup"><span data-stu-id="2995b-445">Open the **\_Layout.Mobile.cshtml** view located under the **Views\Shared** folder and notice the view-switcher component being referenced as a partial view.</span></span>

    <span data-ttu-id="2995b-446">![Layout para dispositivos móveis usando o componente de alternância de exibição](whats-new-in-aspnet-mvc-4/_static/image29.png "layout para dispositivos móveis usando o componente de alternância de exibição")</span><span class="sxs-lookup"><span data-stu-id="2995b-446">![Mobile layout using View-Switcher component](whats-new-in-aspnet-mvc-4/_static/image29.png "Mobile layout using View-Switcher component")</span></span>

    <span data-ttu-id="2995b-447">*Layout para dispositivos móveis usando o componente de alternância de exibição*</span><span class="sxs-lookup"><span data-stu-id="2995b-447">*Mobile layout using View-Switcher component*</span></span>
3. <span data-ttu-id="2995b-448">Abra o  **\_ViewSwitcher.cshtml** exibição parcial.</span><span class="sxs-lookup"><span data-stu-id="2995b-448">Open the **\_ViewSwitcher.cshtml** partial view.</span></span>

    <span data-ttu-id="2995b-449">A exibição parcial usa o novo método **ViewContext.HttpContext.GetOverriddenBrowser()** para determinar a origem da solicitação da web e mostrar o link correspondente para alternar entre os modos de exibição do Desktop ou Mobile.</span><span class="sxs-lookup"><span data-stu-id="2995b-449">The partial view uses the new method **ViewContext.HttpContext.GetOverriddenBrowser()** to determine the origin of the web request and show the corresponding link to switch either to the Desktop or Mobile views.</span></span>

    <span data-ttu-id="2995b-450">O **GetOverridenBrowser** método retorna um **HttpBrowserCapabilitiesBase** instância que corresponde ao agente do usuário definido atualmente para a solicitação (reais ou substituído).</span><span class="sxs-lookup"><span data-stu-id="2995b-450">The **GetOverridenBrowser** method returns an **HttpBrowserCapabilitiesBase** instance that corresponds to the user agent currently set for the request (actual or overridden).</span></span> <span data-ttu-id="2995b-451">Você pode usar esse valor para obter as propriedades como **IsMobileDevice**.</span><span class="sxs-lookup"><span data-stu-id="2995b-451">You can use this value to get properties such as **IsMobileDevice**.</span></span>

    <span data-ttu-id="2995b-452">![Exibição parcial ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image30.png "exibição parcial ViewSwitcher")</span><span class="sxs-lookup"><span data-stu-id="2995b-452">![ViewSwitcher partial view](whats-new-in-aspnet-mvc-4/_static/image30.png "ViewSwitcher partial view")</span></span>

    <span data-ttu-id="2995b-453">*Exibição parcial ViewSwitcher*</span><span class="sxs-lookup"><span data-stu-id="2995b-453">*ViewSwitcher partial view*</span></span>
4. <span data-ttu-id="2995b-454">Abra o **ViewSwitcherController.cs** classe localizado no **controladores** pasta.</span><span class="sxs-lookup"><span data-stu-id="2995b-454">Open the **ViewSwitcherController.cs** class located in the **Controllers** folder.</span></span> <span data-ttu-id="2995b-455">Check-out ação SwitchView é chamado por um link no componente ViewSwitcher e observe os novos métodos de HttpContext.</span><span class="sxs-lookup"><span data-stu-id="2995b-455">Check out that SwitchView action is called by the link in the ViewSwitcher component, and notice the new HttpContext methods.</span></span>

    - <span data-ttu-id="2995b-456">O **HttpContext.ClearOverridenBrowser()** método remove qualquer agente de usuário substituído para a solicitação atual.</span><span class="sxs-lookup"><span data-stu-id="2995b-456">The **HttpContext.ClearOverridenBrowser()** method removes any overridden user agent for the current request.</span></span>
    - <span data-ttu-id="2995b-457">O **HttpContext.SetOverridenBrowser()** método substitui o valor do agente de usuário real da solicitação usando o agente do usuário especificado.</span><span class="sxs-lookup"><span data-stu-id="2995b-457">The **HttpContext.SetOverridenBrowser()** method overrides the request's actual user agent value using the specified user agent.</span></span>  
        <span data-ttu-id="2995b-458">![Controlador ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher controlador")</span><span class="sxs-lookup"><span data-stu-id="2995b-458">![ViewSwitcher Controller](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher Controller")</span></span>  
<span data-ttu-id="2995b-459">*Controlador ViewSwitcher*</span><span class="sxs-lookup"><span data-stu-id="2995b-459">*ViewSwitcher Controller*</span></span>

        <span data-ttu-id="2995b-460">Substituição do navegador é dos principais recursos do ASP.NET MVC 4, que também está disponível mesmo se você não instalar o pacote jQuery.Mobile.MVC.</span><span class="sxs-lookup"><span data-stu-id="2995b-460">Browser Overriding is a core feature of ASP.NET MVC 4, which is also available even if you do not install the jQuery.Mobile.MVC package.</span></span> <span data-ttu-id="2995b-461">No entanto, esse recurso afeta somente exibição, layout e exibição parcial, e ele não afeta os recursos que dependem do objeto Request. browser.</span><span class="sxs-lookup"><span data-stu-id="2995b-461">However, this feature affects only view, layout, and partial-view, and it does not affect any of the features that depend on the Request.Browser object.</span></span>

<a id="Task_5_-_Adding_the_View-Switcher_in_the_Desktop_View"></a>
#### <a name="task-5---adding-the-view-switcher-in-the-desktop-view"></a><span data-ttu-id="2995b-462">Tarefa 5: adicionar a alternância de exibição na exibição da área de trabalho</span><span class="sxs-lookup"><span data-stu-id="2995b-462">Task 5 - Adding the View-Switcher in the Desktop View</span></span>

<span data-ttu-id="2995b-463">Nesta tarefa, você atualizará o layout da área de trabalho para incluir a alternância de exibição.</span><span class="sxs-lookup"><span data-stu-id="2995b-463">In this task, you will update the desktop layout to include the view-switcher.</span></span> <span data-ttu-id="2995b-464">Isso permitirá que os usuários móveis voltar para o modo de exibição móvel ao procurar o modo de exibição da área de trabalho.</span><span class="sxs-lookup"><span data-stu-id="2995b-464">This will allow mobile users to go back to the mobile view when browsing the desktop view.</span></span>

1. <span data-ttu-id="2995b-465">Atualizar o site no **emulador do Windows Phone**.</span><span class="sxs-lookup"><span data-stu-id="2995b-465">Refresh the site in the **Windows Phone Emulator**.</span></span>
2. <span data-ttu-id="2995b-466">Clique no **exibição da área de trabalho** link na parte superior da Galeria.</span><span class="sxs-lookup"><span data-stu-id="2995b-466">Click on the **Desktop view** link at the top of the gallery.</span></span> <span data-ttu-id="2995b-467">Observe que não há nenhuma alternância de exibição na exibição da área de trabalho para permitir que você retornar à exibição móvel.</span><span class="sxs-lookup"><span data-stu-id="2995b-467">Notice there is no view-switcher in the desktop view to allow you return to the mobile view.</span></span>
3. <span data-ttu-id="2995b-468">Volte para o Visual Studio e abra o  **\_cshtml** exibição.</span><span class="sxs-lookup"><span data-stu-id="2995b-468">Go back to Visual Studio and open the **\_Layout.cshtml** view.</span></span>
4. <span data-ttu-id="2995b-469">Localize a seção de login e inserir uma chamada para renderizar o  **\_ViewSwitcher** exibição parcial abaixo o  **\_LogOnPartial** exibição parcial.</span><span class="sxs-lookup"><span data-stu-id="2995b-469">Find the login section and insert a call to render the **\_ViewSwitcher** partial view below the **\_LogOnPartial** partial view.</span></span> <span data-ttu-id="2995b-470">Em seguida, pressione **CTRL + S** para salvar as alterações.</span><span class="sxs-lookup"><span data-stu-id="2995b-470">Then, press **CTRL + S** to save the changes.</span></span>


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample14.cshtml)]
5. <span data-ttu-id="2995b-471">Pressione **CTRL + S** para salvar as alterações.</span><span class="sxs-lookup"><span data-stu-id="2995b-471">Press **CTRL + S** to save the changes.</span></span>
6. <span data-ttu-id="2995b-472">Atualize a página no emulador do Windows Phone e clique duas vezes na tela para aumentar o zoom.</span><span class="sxs-lookup"><span data-stu-id="2995b-472">Refresh the page in the Windows Phone Emulator and double-click the screen to zoom in.</span></span> <span data-ttu-id="2995b-473">Observe que a página inicial agora mostra o **exibição móvel** link que alterna de móveis para o modo de área de trabalho.</span><span class="sxs-lookup"><span data-stu-id="2995b-473">Notice that the home page now shows the **Mobile view** link that switches from mobile to desktop view.</span></span>

    <span data-ttu-id="2995b-474">![Exibir alternador renderizado no modo de exibição da área de trabalho](whats-new-in-aspnet-mvc-4/_static/image32.png "alternância de exibição renderizado no modo de exibição da área de trabalho")</span><span class="sxs-lookup"><span data-stu-id="2995b-474">![View Switcher rendered in desktop view](whats-new-in-aspnet-mvc-4/_static/image32.png "View Switcher rendered in desktop view")</span></span>

    <span data-ttu-id="2995b-475">*Alternância de exibição renderizada no modo de exibição da área de trabalho*</span><span class="sxs-lookup"><span data-stu-id="2995b-475">*View Switcher rendered in desktop view*</span></span>
7. <span data-ttu-id="2995b-476">Alterne para o modo de exibição móvel novamente e navegue até **sobre** página (http://localhost [porta] / Home/sobre).</span><span class="sxs-lookup"><span data-stu-id="2995b-476">Switch to the Mobile view again and browse to **About** page (http://localhost[port]/Home/About).</span></span> <span data-ttu-id="2995b-477">Observe que, mesmo se você não criou uma exibição About.Mobile.cshtml, a página sobre é exibida usando o layout para dispositivos móveis (\_Layout.Mobile.cshtml).</span><span class="sxs-lookup"><span data-stu-id="2995b-477">Notice that, even if you haven't created an About.Mobile.cshtml view, the About page is displayed using the mobile layout (\_Layout.Mobile.cshtml).</span></span>

    <span data-ttu-id="2995b-478">![Sobre a página](whats-new-in-aspnet-mvc-4/_static/image33.png "sobre a página")</span><span class="sxs-lookup"><span data-stu-id="2995b-478">![About page](whats-new-in-aspnet-mvc-4/_static/image33.png "About page")</span></span>

    <span data-ttu-id="2995b-479">*Sobre a página*</span><span class="sxs-lookup"><span data-stu-id="2995b-479">*About page*</span></span>
8. <span data-ttu-id="2995b-480">Por fim, abra o site em um navegador da Web da área de trabalho.</span><span class="sxs-lookup"><span data-stu-id="2995b-480">Finally, open the site in a desktop Web browser.</span></span> <span data-ttu-id="2995b-481">Observe que nenhuma das atualizações do anteriores afetou a exibição da área de trabalho.</span><span class="sxs-lookup"><span data-stu-id="2995b-481">Notice that none of the previous updates has affected the desktop view.</span></span>

    <span data-ttu-id="2995b-482">![Exibição de área de trabalho Galeriadefotos](whats-new-in-aspnet-mvc-4/_static/image34.png "Galeriadefotos exibição de área de trabalho")</span><span class="sxs-lookup"><span data-stu-id="2995b-482">![PhotoGallery desktop view](whats-new-in-aspnet-mvc-4/_static/image34.png "PhotoGallery desktop view")</span></span>

    <span data-ttu-id="2995b-483">*Exibição de área de trabalho Galeriadefotos*</span><span class="sxs-lookup"><span data-stu-id="2995b-483">*PhotoGallery desktop view*</span></span>

<a id="Task_6_-_Creating_New_Display_Modes"></a>
#### <a name="task-6---creating-new-display-modes"></a><span data-ttu-id="2995b-484">Tarefa 6: criar novos modos de exibição</span><span class="sxs-lookup"><span data-stu-id="2995b-484">Task 6 - Creating New Display Modes</span></span>

<span data-ttu-id="2995b-485">O novo recurso de modos de exibição permite que um aplicativo selecione exibições dependendo do navegador que está gerando a solicitação.</span><span class="sxs-lookup"><span data-stu-id="2995b-485">The new display modes feature lets an application select views depending on the browser that is generating the request.</span></span> <span data-ttu-id="2995b-486">Por exemplo, se um navegador desktop solicitar a página inicial, o aplicativo retornará o **Views\Home\Index.cshtml** modelo.</span><span class="sxs-lookup"><span data-stu-id="2995b-486">For example, if a desktop browser requests the Home page, the application will return the **Views\Home\Index.cshtml** template.</span></span> <span data-ttu-id="2995b-487">Em seguida, se um navegador móvel solicitar a página inicial, o aplicativo retornará o **Views\Home\Index.mobile.cshtml** modelo.</span><span class="sxs-lookup"><span data-stu-id="2995b-487">Then, if a mobile browser requests the Home page, the application will return the **Views\Home\Index.mobile.cshtml** template.</span></span>

<span data-ttu-id="2995b-488">Nesta tarefa, você criará um layout personalizado para os dispositivos iPhone e você precisará simular solicitações de dispositivos iPhone.</span><span class="sxs-lookup"><span data-stu-id="2995b-488">In this task, you will create a customized layout for iPhone devices, and you will have to simulate requests from iPhone devices.</span></span> <span data-ttu-id="2995b-489">Para fazer isso, você pode usar qualquer um emulador/simulador de iPhone (como [Electric simulador de móveis](http://www.electricplum.com/)) ou um navegador com os complementos que modificam o agente do usuário.</span><span class="sxs-lookup"><span data-stu-id="2995b-489">To do this, you can use either an iPhone emulator/simulator (like [Electric Mobile Simulator](http://www.electricplum.com/)) or a browser with add-ons that modify the user agent.</span></span> <span data-ttu-id="2995b-490">Para obter instruções sobre como definir a cadeia de caracteres de agente do usuário em um navegador Safari emular um iPhone, consulte [como permitir que o Safari fingem é IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) no blog de David Alison.</span><span class="sxs-lookup"><span data-stu-id="2995b-490">For instructions on how to set the user agent string in an Safari browser to emulate an iPhone, see [How to let Safari pretend it's IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) in David Alison's blog.</span></span>

<span data-ttu-id="2995b-491">**Observe que essa tarefa é opcional e você pode continuar em todo o laboratório sem executá-la.**</span><span class="sxs-lookup"><span data-stu-id="2995b-491">**Notice that this task is optional and you can continue throughout the lab without executing it.**</span></span>

1. <span data-ttu-id="2995b-492">No Visual Studio, pressione **SHIFT** + **F5** para parar a depuração do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2995b-492">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>
2. <span data-ttu-id="2995b-493">Abra **Global.asax.cs** e adicione o seguinte usando a instrução.</span><span class="sxs-lookup"><span data-stu-id="2995b-493">Open **Global.asax.cs** and add the following using statement.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample15.cs)]
3. <span data-ttu-id="2995b-494">Adicione o seguinte código realçado no aplicativo\_método Start.</span><span class="sxs-lookup"><span data-stu-id="2995b-494">Add the following highlighted code into the Application\_Start method.</span></span>

    <span data-ttu-id="2995b-495">(Código de trecho - *ASP.NET MVC 4 laboratório - Ex03 - iPhone DisplayMode*)</span><span class="sxs-lookup"><span data-stu-id="2995b-495">(Code Snippet - *ASP.NET MVC 4 Lab - Ex03 - iPhone DisplayMode*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample16.cs)]

    <span data-ttu-id="2995b-496">Você registrou um novo **DefaultDisplayMode chamado &quot;iPhone&quot;**, dentro de estático **DisplayModeProvider.Instance.Modes** lista estática, o que será comparada com cada solicitação de entrada.</span><span class="sxs-lookup"><span data-stu-id="2995b-496">You have registered a new **DefaultDisplayMode named &quot;iPhone&quot;**, within the static **DisplayModeProvider.Instance.Modes** static list, that will be matched against each incoming request.</span></span> <span data-ttu-id="2995b-497">Se a solicitação de entrada contiver a cadeia de caracteres &quot;iPhone&quot;, ASP.NET MVC encontrará os modos de exibição cujo nome contém o &quot;iPhone&quot; sufixo.</span><span class="sxs-lookup"><span data-stu-id="2995b-497">If the incoming request contains the string &quot;iPhone&quot;, ASP.NET MVC will find the views whose name contain the &quot;iPhone&quot; suffix.</span></span> <span data-ttu-id="2995b-498">O parâmetro 0 indica como específico é o novo modo; Por exemplo, essa exibição é mais específica que geral &quot;.mobile&quot; regra que corresponde a solicitações de dispositivos móveis.</span><span class="sxs-lookup"><span data-stu-id="2995b-498">The 0 parameter indicates how specific is the new mode; for instance, this view is more specific than the general &quot;.mobile&quot; rule that matches requests from mobile devices.</span></span>

    <span data-ttu-id="2995b-499">Depois que esse código é executado, quando um navegador de iPhone gera uma solicitação, o aplicativo usará o **exibições \ compartilhadas\\_Layout.iPhone.cshtml** layout, você criará as próximas etapas.</span><span class="sxs-lookup"><span data-stu-id="2995b-499">After this code runs, when an iPhone browser generates a request, your application will use the **Views\Shared\\_Layout.iPhone.cshtml** layout you will create in the next steps.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2995b-500">Essa maneira de testar a solicitação para iPhone foi simplificado para fins de demonstração e pode não funcionar conforme o esperado para cada cadeia de caracteres de agente de usuário do iPhone (para teste de exemplo diferencia maiusculas de minúsculas).</span><span class="sxs-lookup"><span data-stu-id="2995b-500">This way of testing the request for iPhone has been simplified for demo purposes and might not work as expected for every iPhone user agent string (for example test is case sensitive).</span></span>
4. <span data-ttu-id="2995b-501">Criar uma cópia do  **\_Layout.Mobile.cshtml** arquivo o **exibições \ compartilhadas** pasta e renomeie a cópia de &quot;  **\_Layout.iPhone.csthml** &quot;.</span><span class="sxs-lookup"><span data-stu-id="2995b-501">Create a copy of the **\_Layout.Mobile.cshtml** file in the **Views\Shared** folder and rename the copy to &quot;**\_Layout.iPhone.csthml**&quot;.</span></span>
5. <span data-ttu-id="2995b-502">Abra  **\_Layout.iPhone.csthml** criado na etapa anterior.</span><span class="sxs-lookup"><span data-stu-id="2995b-502">Open **\_Layout.iPhone.csthml** you created in the previous step.</span></span>
6. <span data-ttu-id="2995b-503">Localizar o elemento div com o atributo de dados de função definido como **página** e altere o **dados tema** atributo &quot; **um**&quot;.</span><span class="sxs-lookup"><span data-stu-id="2995b-503">Find the div element with the data-role attribute set to **page** and change the **data-theme** attribute to &quot;**a**&quot;.</span></span>


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample17.cshtml)]

    <span data-ttu-id="2995b-504">Agora você tem 3 layouts em seu aplicativo ASP.NET MVC 4:</span><span class="sxs-lookup"><span data-stu-id="2995b-504">Now you have 3 layouts in your ASP.NET MVC 4 application:</span></span>

    1. <span data-ttu-id="2995b-505">**\_Cshtml**: layout padrão usado para navegadores de desktop.</span><span class="sxs-lookup"><span data-stu-id="2995b-505">**\_Layout.cshtml**: default layout used for desktop browsers.</span></span>
    2. <span data-ttu-id="2995b-506">**\_Layout.Mobile.cshtml**: layout padrão usado para dispositivos móveis.</span><span class="sxs-lookup"><span data-stu-id="2995b-506">**\_Layout.mobile.cshtml**: default layout used for mobile devices.</span></span>
    3. <span data-ttu-id="2995b-507">**\_Layout.iPhone.cshtml**: layout específicas para os dispositivos iPhone, usando um esquema de cores diferente para diferenciar de \_Layout.mobile.cshtml.</span><span class="sxs-lookup"><span data-stu-id="2995b-507">**\_Layout.iPhone.cshtml**: specific layout for iPhone devices, using a different color scheme to differentiate from \_Layout.mobile.cshtml.</span></span>
7. <span data-ttu-id="2995b-508">Pressione **F5** para executar o aplicativo e navegue até o site no **emulador do Windows Phone**.</span><span class="sxs-lookup"><span data-stu-id="2995b-508">Press **F5** to run the application and browse the site in the **Windows Phone Emulator**.</span></span>
8. <span data-ttu-id="2995b-509">Abra um **simulador de iPhone** (consulte [Apêndice C](#AppendixC) para obter instruções sobre como instalar e configurar um simulador de iPhone) e navegue até o site muito.</span><span class="sxs-lookup"><span data-stu-id="2995b-509">Open an **iPhone simulator** (see [Appendix C](#AppendixC) for instructions on how to install and configure an iPhone simulator), and browse to the site too.</span></span> <span data-ttu-id="2995b-510">Observe que cada telefone está usando o modelo específico.</span><span class="sxs-lookup"><span data-stu-id="2995b-510">Notice that each phone is using the specific template.</span></span>

    ![Using-different-Views-for-each-Mobile-device2](whats-new-in-aspnet-mvc-4/_static/image35.png)

    <span data-ttu-id="2995b-512">*Usando modos de exibição diferentes para cada dispositivo móvel*</span><span class="sxs-lookup"><span data-stu-id="2995b-512">*Using different views for each mobile device*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Using_Asynchronous_Controllers"></a>
### <a name="exercise-4-using-asynchronous-controllers"></a><span data-ttu-id="2995b-513">Exercício 4: Usando controladores assíncronos</span><span class="sxs-lookup"><span data-stu-id="2995b-513">Exercise 4: Using Asynchronous Controllers</span></span>

<span data-ttu-id="2995b-514">Microsoft .NET Framework 4.5 apresenta novos recursos de idioma em c# e Visual Basic para fornecer uma nova base de assincronia na programação .NET.</span><span class="sxs-lookup"><span data-stu-id="2995b-514">Microsoft .NET Framework 4.5 introduces new language features in C# and Visual Basic to provide a new foundation for asynchrony in .NET programming.</span></span> <span data-ttu-id="2995b-515">Essa nova base torna programação assíncrona semelhantes e sobre tão simples quanto - programação síncrona.</span><span class="sxs-lookup"><span data-stu-id="2995b-515">This new foundation makes asynchronous programming similar to - and about as straightforward as - synchronous programming.</span></span> <span data-ttu-id="2995b-516">Agora é possível gravar os métodos de ação assíncrono no ASP.NET MVC 4 usando o **AsyncController** classe.</span><span class="sxs-lookup"><span data-stu-id="2995b-516">You are now able to write asynchronous action methods in ASP.NET MVC 4 by using the **AsyncController** class.</span></span> <span data-ttu-id="2995b-517">Você pode usar métodos de ação assíncrono de longa duração, CPU não associado a solicitações.</span><span class="sxs-lookup"><span data-stu-id="2995b-517">You can use asynchronous action methods for long-running, non-CPU bound requests.</span></span> <span data-ttu-id="2995b-518">Isso evita que o servidor Web da execução de trabalho enquanto a solicitação está sendo processada de bloqueio.</span><span class="sxs-lookup"><span data-stu-id="2995b-518">This avoids blocking the Web server from performing work while the request is being processed.</span></span> <span data-ttu-id="2995b-519">A classe AsyncController normalmente é usada para chamadas de serviço Web de longa execução.</span><span class="sxs-lookup"><span data-stu-id="2995b-519">The AsyncController class is typically used for long-running Web service calls.</span></span>

<span data-ttu-id="2995b-520">Este exercício explica os conceitos básicos de uma operação assíncrona no ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="2995b-520">This exercise explains the basics of asynchronous operation in ASP.NET MVC 4.</span></span> <span data-ttu-id="2995b-521">Se você quiser se aprofundar mais, confira o seguinte artigo: [ [https://msdn.microsoft.com/en-us/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/en-us/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/en-us/library/ee728598%28v=vs.100%29.aspx)</span><span class="sxs-lookup"><span data-stu-id="2995b-521">If you want a deeper dive, you can check out the following article: [[https://msdn.microsoft.com/en-us/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/en-us/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/en-us/library/ee728598%28v=vs.100%29.aspx)</span></span>

<a id="Task_1_-_Implementing_an_Asynchronous_Controller"></a>
#### <a name="task-1---implementing-an-asynchronous-controller"></a><span data-ttu-id="2995b-522">Tarefa 1: Implementando um controlador assíncrono</span><span class="sxs-lookup"><span data-stu-id="2995b-522">Task 1 - Implementing an Asynchronous Controller</span></span>

1. <span data-ttu-id="2995b-523">Abra o **começar** solução localizado em **fonte/Ex4-Async/Begin/** pasta.</span><span class="sxs-lookup"><span data-stu-id="2995b-523">Open the **Begin** solution located at **Source/Ex4-Async/Begin/** folder.</span></span> <span data-ttu-id="2995b-524">Caso contrário, você pode continuar usando o **final** solução obtido executando o exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="2995b-524">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="2995b-525">Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="2995b-525">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="2995b-526">Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="2995b-526">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="2995b-527">No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="2995b-527">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="2995b-528">Por fim, compile a solução clicando **criar** | **compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="2995b-528">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2995b-529">Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="2995b-529">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="2995b-530">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="2995b-530">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="2995b-531">É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.</span><span class="sxs-lookup"><span data-stu-id="2995b-531">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="2995b-532">Abra o **HomeController** classe o **controladores** pasta.</span><span class="sxs-lookup"><span data-stu-id="2995b-532">Open the **HomeController.cs** class from the **Controllers** folder.</span></span>
3. <span data-ttu-id="2995b-533">Adicione o seguinte usando a instrução.</span><span class="sxs-lookup"><span data-stu-id="2995b-533">Add the following using statement.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample18.cs)]
4. <span data-ttu-id="2995b-534">Atualização de **HomeController** classe para herdar de **AsyncController**.</span><span class="sxs-lookup"><span data-stu-id="2995b-534">Update the **HomeController** class to inherit from **AsyncController**.</span></span> <span data-ttu-id="2995b-535">Controladores que derivam de AsyncController habilitar o ASP.NET processar solicitações assíncronas e ainda pode métodos de ação síncrono do serviço.</span><span class="sxs-lookup"><span data-stu-id="2995b-535">Controllers that derive from AsyncController enable ASP.NET to process asynchronous requests, and they can still service synchronous action methods.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample19.cs)]
5. <span data-ttu-id="2995b-536">Adicionar o **async** palavra-chave para o **índice** método e o tipo de retorno **tarefa&lt;ActionResult&gt;**.</span><span class="sxs-lookup"><span data-stu-id="2995b-536">Add the **async** keyword to the **Index** method and make it return the type **Task&lt;ActionResult&gt;**.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample20.cs)]

    > [!NOTE]
    > <span data-ttu-id="2995b-537">O **async** palavra-chave é uma das novas palavras-chave do .NET Framework 4.5 fornece; ele informa ao compilador que este método contém código assíncrono.</span><span class="sxs-lookup"><span data-stu-id="2995b-537">The **async** keyword is one of the new keywords the .NET Framework 4.5 provides; it tells the compiler that this method contains asynchronous code.</span></span> <span data-ttu-id="2995b-538">Um **tarefa** objeto representa uma operação assíncrona que pode ser concluída em algum momento no futuro.</span><span class="sxs-lookup"><span data-stu-id="2995b-538">A **Task** object represents an asynchronous operation that may complete at some point in the future.</span></span>
6. <span data-ttu-id="2995b-539">Substitua o **cliente. GetAsync()** chamada com a versão assíncrona completa usando palavra-chave await conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="2995b-539">Replace the **client.GetAsync()** call with the full async version using await keyword as shown below.</span></span>

    <span data-ttu-id="2995b-540">(Código de trecho - *GetAsync do ASP.NET MVC 4 laboratório - Ex04 -*)</span><span class="sxs-lookup"><span data-stu-id="2995b-540">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - GetAsync*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample21.cs)]

    > [!NOTE]
    > <span data-ttu-id="2995b-541">Na versão anterior, você estava usando o **resultados** propriedade o **tarefa** objeto para bloquear o thread até que o resultado é retornado (versão de sincronização).</span><span class="sxs-lookup"><span data-stu-id="2995b-541">In the previous version, you were using the **Result** property from the **Task** object to block the thread until the result is returned (sync version).</span></span>
    > 
    > <span data-ttu-id="2995b-542">Adicionando o **await** palavra-chave informa ao compilador de forma assíncrona espera para a tarefa retornada da chamada de método.</span><span class="sxs-lookup"><span data-stu-id="2995b-542">Adding the **await** keyword tells the compiler to asynchronously wait for the task returned from the method call.</span></span> <span data-ttu-id="2995b-543">Isso significa que o restante do código será executado como um retorno de chamada somente depois que o método espera é concluída.</span><span class="sxs-lookup"><span data-stu-id="2995b-543">This means that the rest of the code will be executed as a callback only after the awaited method completes.</span></span> <span data-ttu-id="2995b-544">Outra coisa a observar é que você não precisa alterar seu bloco try-catch para que funcione: as exceções que ocorrem em segundo plano ou em primeiro plano ainda serão capturadas sem qualquer trabalho extra usando um manipulador fornecido pelo framework.</span><span class="sxs-lookup"><span data-stu-id="2995b-544">Another thing to notice is that you do not need to change your try-catch block in order to make this work: the exceptions that happen in background or in foreground will still be caught without any extra work using a handler provided by the framework.</span></span>
7. <span data-ttu-id="2995b-545">Alterar o código para continuar com a implementação assíncrona, substituindo as linhas com o novo código, conforme mostrado abaixo</span><span class="sxs-lookup"><span data-stu-id="2995b-545">Change the code to continue with the asynchronous implementation by replacing the lines with the new code as shown below</span></span>

    <span data-ttu-id="2995b-546">(Código de trecho - *ReadAsStringAsync do ASP.NET MVC 4 laboratório - Ex04 -*)</span><span class="sxs-lookup"><span data-stu-id="2995b-546">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - ReadAsStringAsync*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample22.cs)]
8. <span data-ttu-id="2995b-547">Execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2995b-547">Run the application.</span></span> <span data-ttu-id="2995b-548">Você não observará nenhuma alteração principal, mas seu código não impedirá que um thread do pool de threads, tornando um melhor uso dos recursos do servidor e melhorar o desempenho.</span><span class="sxs-lookup"><span data-stu-id="2995b-548">You will notice no major changes, but your code will not block a thread from the thread pool making a better usage of the server resources and improving performance.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2995b-549">Você pode aprender mais sobre os novos recursos de programação assíncronos no laboratório &quot; **programação assíncrona no .NET 4.5 com c# e Visual Basic** &quot; incluído no Kit de treinamento do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2995b-549">You can learn more about the new asynchronous programming features in the lab &quot;**Asynchronous Programming in .NET 4.5 with C# and Visual Basic**&quot; included in the Visual Studio Training Kit.</span></span>

<a id="Task_2_-_Handling_Time-Outs_with_Cancellation_Tokens"></a>
#### <a name="task-2---handling-time-outs-with-cancellation-tokens"></a><span data-ttu-id="2995b-550">Tarefa 2 - tempos limite de tratamento com Tokens de cancelamento</span><span class="sxs-lookup"><span data-stu-id="2995b-550">Task 2 - Handling Time-Outs with Cancellation Tokens</span></span>

<span data-ttu-id="2995b-551">Métodos de ação assíncrono que retornam instâncias de tarefa também podem dar suporte a tempos limite.</span><span class="sxs-lookup"><span data-stu-id="2995b-551">Asynchronous action methods that return Task instances can also support time-outs.</span></span> <span data-ttu-id="2995b-552">Nesta tarefa, você atualizará o código do método de índice para lidar com um cenário de tempo limite usando um token de cancelamento.</span><span class="sxs-lookup"><span data-stu-id="2995b-552">In this task, you will update the Index method code to handle a time-out scenario using a cancellation token.</span></span>

1. <span data-ttu-id="2995b-553">Volte para o Visual Studio e pressione **SHIFT + F5** para parar a depuração.</span><span class="sxs-lookup"><span data-stu-id="2995b-553">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>
2. <span data-ttu-id="2995b-554">Adicione o seguinte usando a instrução de **HomeController** arquivo.</span><span class="sxs-lookup"><span data-stu-id="2995b-554">Add the following using statement to the **HomeController.cs** file.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample23.cs)]
3. <span data-ttu-id="2995b-555">Atualizar a ação de índice para receber um **CancellationToken** argumento.</span><span class="sxs-lookup"><span data-stu-id="2995b-555">Update the Index action to receive a **CancellationToken** argument.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample24.cs)]
4. <span data-ttu-id="2995b-556">Atualização de **GetAsync** chamada para passar o token de cancelamento.</span><span class="sxs-lookup"><span data-stu-id="2995b-556">Update the **GetAsync** call to pass the cancellation token.</span></span>

    <span data-ttu-id="2995b-557">(Código de trecho - *ASP.NET MVC 4 laboratório - Ex04 - SendAsync com CancellationToken*)</span><span class="sxs-lookup"><span data-stu-id="2995b-557">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - SendAsync with CancellationToken*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample25.cs)]
5. <span data-ttu-id="2995b-558">Decore o *índice* método com um **AsyncTimeout** atributo definido como 500 milissegundos e um **HandleError** atributo configurado para tratar  **TaskCanceledException** redirecionando para um **TimedOut** exibição.</span><span class="sxs-lookup"><span data-stu-id="2995b-558">Decorate the *Index* method with an **AsyncTimeout** attribute set to 500 milliseconds and a **HandleError** attribute configured to handle **TaskCanceledException** by redirecting to a **TimedOut** view.</span></span>

    <span data-ttu-id="2995b-559">(Código de trecho - *atributos do ASP.NET MVC 4 laboratório - Ex04 -*)</span><span class="sxs-lookup"><span data-stu-id="2995b-559">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - Attributes*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample26.cs)]
6. <span data-ttu-id="2995b-560">Abra o **PhotoController** classe e atualizar o **galeria** método para atrasar os milissegundos de execução 1000 (1 segundo) para simular uma tarefa demorada.</span><span class="sxs-lookup"><span data-stu-id="2995b-560">Open the **PhotoController** class and update the **Gallery** method to delay the execution 1000 miliseconds (1 second) to simulate a long running task.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample27.cs)]
7. <span data-ttu-id="2995b-561">Abra o **Web. config** de arquivos e habilitar os erros personalizados, adicionando o seguinte elemento.</span><span class="sxs-lookup"><span data-stu-id="2995b-561">Open the **Web.config** file and enable custom errors by adding the following element.</span></span>


    [!code-xml[Main](whats-new-in-aspnet-mvc-4/samples/sample28.xml)]
8. <span data-ttu-id="2995b-562">Criar uma nova exibição em **exibições \ compartilhadas** chamado **TimedOut** e use o layout padrão.</span><span class="sxs-lookup"><span data-stu-id="2995b-562">Create a new view in **Views\Shared** named **TimedOut** and use the default layout.</span></span> <span data-ttu-id="2995b-563">No Gerenciador de soluções, clique com botão direito do **exibições \ compartilhadas** pasta e selecione **adicionar | Exibição**.</span><span class="sxs-lookup"><span data-stu-id="2995b-563">In the Solution Explorer, right-click the **Views\Shared** folder and select **Add | View**.</span></span>

    <span data-ttu-id="2995b-564">![Usando modos de exibição diferentes para cada dispositivo móvel](whats-new-in-aspnet-mvc-4/_static/image36.png "usando diferentes modos de exibição para cada dispositivo móvel")</span><span class="sxs-lookup"><span data-stu-id="2995b-564">![Using different views for each mobile device](whats-new-in-aspnet-mvc-4/_static/image36.png "Using different views for each mobile device")</span></span>

    <span data-ttu-id="2995b-565">*Usando modos de exibição diferentes para cada dispositivo móvel*</span><span class="sxs-lookup"><span data-stu-id="2995b-565">*Using different views for each mobile device*</span></span>
9. <span data-ttu-id="2995b-566">Atualização de **TimedOut** exibir conteúdo, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="2995b-566">Update the **TimedOut** view content as shown below.</span></span>


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample29.cshtml)]
10. <span data-ttu-id="2995b-567">Execute o aplicativo e navegue até a URL raiz.</span><span class="sxs-lookup"><span data-stu-id="2995b-567">Run the application and navigate to the root URL.</span></span> <span data-ttu-id="2995b-568">Como você adicionou um **Sleep** de 1000 milissegundos, você obterá um erro de tempo limite, gerado pelo **AsyncTimeout** de atributos e catch pelo **HandleError** atributo.</span><span class="sxs-lookup"><span data-stu-id="2995b-568">As you have added a **Thread.Sleep** of 1000 milliseconds, you will get a time-out error, generated by the **AsyncTimeout** attribute and catch by the **HandleError** attribute.</span></span>

    <span data-ttu-id="2995b-569">![Exceção de tempo limite tratada](whats-new-in-aspnet-mvc-4/_static/image37.png "tratada de exceção de tempo limite")</span><span class="sxs-lookup"><span data-stu-id="2995b-569">![Time-out exception handled](whats-new-in-aspnet-mvc-4/_static/image37.png "Time-out exception handled")</span></span>

    <span data-ttu-id="2995b-570">*Exceção de tempo limite tratada*</span><span class="sxs-lookup"><span data-stu-id="2995b-570">*Time-out exception handled*</span></span>

> [!NOTE]
> <span data-ttu-id="2995b-571">Além disso, você pode implantar esse aplicativo para Windows Azure Web Sites a seguir [apêndice d: publicação um aplicativo ASP.NET MVC 4 usando a implantação da Web](#AppendixD).</span><span class="sxs-lookup"><span data-stu-id="2995b-571">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix D: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixD).</span></span>


<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="2995b-572">Resumo</span><span class="sxs-lookup"><span data-stu-id="2995b-572">Summary</span></span>

<span data-ttu-id="2995b-573">Neste prático laboratório, você tenha observado alguns dos novos recursos residente no ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="2995b-573">In this hands-on-lab, you've observed some of the new features resident in ASP.NET MVC 4.</span></span> <span data-ttu-id="2995b-574">Foram discutidos os seguintes conceitos:</span><span class="sxs-lookup"><span data-stu-id="2995b-574">The following concepts have been discussed:</span></span>

- <span data-ttu-id="2995b-575">Aproveitar os aprimoramentos para o ASP.NET MVC projeto modelos, incluindo o novo modelo de projeto de aplicativo móvel</span><span class="sxs-lookup"><span data-stu-id="2995b-575">Take advantage of the enhancements to the ASP.NET MVC project templates-including the new mobile application project template</span></span>
- <span data-ttu-id="2995b-576">Use o atributo de visor do HTML5 e consultas de mídia CSS para melhorar a exibição em dispositivos móveis</span><span class="sxs-lookup"><span data-stu-id="2995b-576">Use the HTML5 viewport attribute and CSS media queries to improve the display on mobile devices</span></span>
- <span data-ttu-id="2995b-577">Use o jQuery Mobile para aprimoramentos progressivos e para a criação de interface da web otimizada para toque</span><span class="sxs-lookup"><span data-stu-id="2995b-577">Use jQuery Mobile for progressive enhancements and for building touch-optimized web UI</span></span>
- <span data-ttu-id="2995b-578">Criar exibições específicas para dispositivos móveis</span><span class="sxs-lookup"><span data-stu-id="2995b-578">Create mobile-specific views</span></span>
- <span data-ttu-id="2995b-579">Use o componente de alternância de exibição para alternar entre modos de exibição móveis e área de trabalho do aplicativo</span><span class="sxs-lookup"><span data-stu-id="2995b-579">Use the view-switcher component to toggle between mobile and desktop views in the application</span></span>
- <span data-ttu-id="2995b-580">Criar controladores assíncronos usando o suporte da tarefa</span><span class="sxs-lookup"><span data-stu-id="2995b-580">Create asynchronous controllers using task support</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a><span data-ttu-id="2995b-581">Apêndice a: usar trechos de código</span><span class="sxs-lookup"><span data-stu-id="2995b-581">Appendix A: Using Code Snippets</span></span>

<span data-ttu-id="2995b-582">Com trechos de código, você tem todo o código que é necessário ao seu alcance.</span><span class="sxs-lookup"><span data-stu-id="2995b-582">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="2995b-583">O documento de laboratório informará exatamente quando você pode usá-los, conforme mostrado na figura a seguir.</span><span class="sxs-lookup"><span data-stu-id="2995b-583">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="2995b-584">![Usando trechos de código do Visual Studio para inserir o código no seu projeto](whats-new-in-aspnet-mvc-4/_static/image38.png "trechos de código com o Visual Studio para inserir código em seu projeto")</span><span class="sxs-lookup"><span data-stu-id="2995b-584">![Using Visual Studio code snippets to insert code into your project](whats-new-in-aspnet-mvc-4/_static/image38.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="2995b-585">*Usando trechos de código do Visual Studio para inserir código em seu projeto*</span><span class="sxs-lookup"><span data-stu-id="2995b-585">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="2995b-586">***Para adicionar um trecho de código usando o teclado (apenas c#)***</span><span class="sxs-lookup"><span data-stu-id="2995b-586">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="2995b-587">Posicione o cursor onde você deseja inserir o código.</span><span class="sxs-lookup"><span data-stu-id="2995b-587">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="2995b-588">Comece a digitar o nome do trecho (sem espaços ou hifens).</span><span class="sxs-lookup"><span data-stu-id="2995b-588">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="2995b-589">Observe como IntelliSense exibe os nomes dos trechos de código correspondentes.</span><span class="sxs-lookup"><span data-stu-id="2995b-589">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="2995b-590">Selecione o trecho correto (ou continue digitando até que o nome do trecho de código inteiro é selecionado).</span><span class="sxs-lookup"><span data-stu-id="2995b-590">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="2995b-591">Pressione a tecla Tab duas vezes para inserir o trecho de código no local do cursor.</span><span class="sxs-lookup"><span data-stu-id="2995b-591">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="2995b-592">![Comece a digitar o nome do fragmento](whats-new-in-aspnet-mvc-4/_static/image39.png "comece a digitar o nome do trecho de código")</span><span class="sxs-lookup"><span data-stu-id="2995b-592">![Start typing the snippet name](whats-new-in-aspnet-mvc-4/_static/image39.png "Start typing the snippet name")</span></span>

<span data-ttu-id="2995b-593">*Comece a digitar o nome do trecho de código*</span><span class="sxs-lookup"><span data-stu-id="2995b-593">*Start typing the snippet name*</span></span>

<span data-ttu-id="2995b-594">![Pressione Tab para selecionar o trecho de código realçado](whats-new-in-aspnet-mvc-4/_static/image40.png "pressione Tab para selecionar o trecho de código realçado")</span><span class="sxs-lookup"><span data-stu-id="2995b-594">![Press Tab to select the highlighted snippet](whats-new-in-aspnet-mvc-4/_static/image40.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="2995b-595">*Pressione Tab para selecionar o trecho de código realçado*</span><span class="sxs-lookup"><span data-stu-id="2995b-595">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="2995b-596">![Pressione Tab novamente e o trecho expandirá](whats-new-in-aspnet-mvc-4/_static/image41.png "expandirá pressione Tab novamente e o trecho de código")</span><span class="sxs-lookup"><span data-stu-id="2995b-596">![Press Tab again and the snippet will expand](whats-new-in-aspnet-mvc-4/_static/image41.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="2995b-597">*Pressione Tab novamente e o trecho de código serão expandida*</span><span class="sxs-lookup"><span data-stu-id="2995b-597">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="2995b-598">***Para adicionar um trecho de código usando o mouse (c#, Visual Basic e XML)***</span><span class="sxs-lookup"><span data-stu-id="2995b-598">***To add a code snippet using the mouse (C#, Visual Basic and XML)***</span></span>

1. <span data-ttu-id="2995b-599">Clique com botão direito em que você deseja inserir o trecho de código.</span><span class="sxs-lookup"><span data-stu-id="2995b-599">Right-click where you want to insert the code snippet.</span></span>
2. <span data-ttu-id="2995b-600">Selecione **Inserir trecho** seguido por **Meus trechos de código**.</span><span class="sxs-lookup"><span data-stu-id="2995b-600">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
3. <span data-ttu-id="2995b-601">Selecione o trecho relevante na lista, clicando nele.</span><span class="sxs-lookup"><span data-stu-id="2995b-601">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="2995b-602">![Com o botão direito em que você deseja inserir o trecho de código e selecione Inserir trecho](whats-new-in-aspnet-mvc-4/_static/image42.png "com o botão direito em que você deseja inserir o trecho de código e selecione Inserir trecho")</span><span class="sxs-lookup"><span data-stu-id="2995b-602">![Right-click where you want to insert the code snippet and select Insert Snippet](whats-new-in-aspnet-mvc-4/_static/image42.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="2995b-603">*Clique com botão direito em que você deseja inserir o trecho de código e selecione Inserir trecho*</span><span class="sxs-lookup"><span data-stu-id="2995b-603">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="2995b-604">![Selecione o trecho relevante na lista, clicando nele](whats-new-in-aspnet-mvc-4/_static/image43.png "escolher o trecho relevante na lista, clicando nele")</span><span class="sxs-lookup"><span data-stu-id="2995b-604">![Pick the relevant snippet from the list, by clicking on it](whats-new-in-aspnet-mvc-4/_static/image43.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="2995b-605">*Selecione o trecho relevante na lista, clicando nele*</span><span class="sxs-lookup"><span data-stu-id="2995b-605">*Pick the relevant snippet from the list, by clicking on it*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="2995b-606">Apêndice b: instalar o Visual Studio Express 2012 para Web</span><span class="sxs-lookup"><span data-stu-id="2995b-606">Appendix B: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="2995b-607">Você pode instalar **Microsoft Visual Studio Express 2012 para Web** ou outro &quot;Express&quot; versão usando o  **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="2995b-607">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="2995b-608">As instruções a seguir guiá-lo pelas etapas necessárias para instalar o *Visual studio Express 2012 para Web* usando *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="2995b-608">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="2995b-609">Vá para [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="2995b-609">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="2995b-610">Como alternativa, se você já tiver instalado o Web Platform Installer, você pode abri-la e procure o produto &quot; *Visual Studio Express 2012 para Web com o SDK do Windows Azure*&quot;.</span><span class="sxs-lookup"><span data-stu-id="2995b-610">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="2995b-611">Clique em **instalar agora**.</span><span class="sxs-lookup"><span data-stu-id="2995b-611">Click on **Install Now**.</span></span> <span data-ttu-id="2995b-612">Se você não tem **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.</span><span class="sxs-lookup"><span data-stu-id="2995b-612">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="2995b-613">Uma vez **Web Platform Installer** é aberto, clique em **instalar** para iniciar a instalação.</span><span class="sxs-lookup"><span data-stu-id="2995b-613">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="2995b-614">![Instalar Visual Studio Express](whats-new-in-aspnet-mvc-4/_static/image44.png "instalar Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="2995b-614">![Install Visual Studio Express](whats-new-in-aspnet-mvc-4/_static/image44.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="2995b-615">*Instalar Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="2995b-615">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="2995b-616">Leia os termos e licenças de todos os produtos e clique em **aceito** para continuar.</span><span class="sxs-lookup"><span data-stu-id="2995b-616">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Aceitar os termos de licença](whats-new-in-aspnet-mvc-4/_static/image45.png)

    <span data-ttu-id="2995b-618">*Aceitar os termos de licença*</span><span class="sxs-lookup"><span data-stu-id="2995b-618">*Accepting the license terms*</span></span>
5. <span data-ttu-id="2995b-619">Aguarde até que o processo de download e instalação seja concluída.</span><span class="sxs-lookup"><span data-stu-id="2995b-619">Wait until the downloading and installation process completes.</span></span>

    ![Progresso da instalação](whats-new-in-aspnet-mvc-4/_static/image46.png)

    <span data-ttu-id="2995b-621">*Progresso da instalação*</span><span class="sxs-lookup"><span data-stu-id="2995b-621">*Installation progress*</span></span>
6. <span data-ttu-id="2995b-622">Quando a instalação for concluída, clique em **concluir**.</span><span class="sxs-lookup"><span data-stu-id="2995b-622">When the installation completes, click **Finish**.</span></span>

    ![Instalação concluída](whats-new-in-aspnet-mvc-4/_static/image47.png)

    <span data-ttu-id="2995b-624">*Instalação concluída*</span><span class="sxs-lookup"><span data-stu-id="2995b-624">*Installation completed*</span></span>
7. <span data-ttu-id="2995b-625">Clique em **saída** para fechar o Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="2995b-625">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="2995b-626">Para abrir o Visual Studio Express para Web, vá para o **iniciar** tela e comece a escrever &quot; **VS Express**&quot;, em seguida, clique no **VS Express para Web** lado a lado.</span><span class="sxs-lookup"><span data-stu-id="2995b-626">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express para o bloco de Web](whats-new-in-aspnet-mvc-4/_static/image48.png)

    <span data-ttu-id="2995b-628">*VS Express para o bloco de Web*</span><span class="sxs-lookup"><span data-stu-id="2995b-628">*VS Express for Web tile*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Installing_WebMatrix_2_and_iPhone_Simulator"></a>
## <a name="appendix-c-installing-webmatrix-2-and-iphone-simulator"></a><span data-ttu-id="2995b-629">Apêndice c: Instalando o WebMatrix 2 e o simulador de iPhone</span><span class="sxs-lookup"><span data-stu-id="2995b-629">Appendix C: Installing WebMatrix 2 and iPhone Simulator</span></span>

<span data-ttu-id="2995b-630">Para executar o seu site em um dispositivo simulado iPhone, você pode usar a extensão do WebMatrix &quot;Electric simulador Mobile para iPhone&quot;.</span><span class="sxs-lookup"><span data-stu-id="2995b-630">To run your site in a simulated iPhone device you can use the WebMatrix extension &quot;Electric Mobile Simulator for the iPhone&quot;.</span></span> <span data-ttu-id="2995b-631">Além disso, você pode configurar a mesma extensão para executar o simulador do Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="2995b-631">Also, you can configure the same extension to run the simulator from Visual Studio 2012.</span></span>

<a id="ApxCTask1"></a>

<a id="Task_1_-_Installing_WebMatrix_2"></a>
#### <a name="task-1---installing-webmatrix-2"></a><span data-ttu-id="2995b-632">Tarefa 1: instalar o WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="2995b-632">Task 1 - Installing WebMatrix 2</span></span>

1. <span data-ttu-id="2995b-633">Vá para [ [https://go.microsoft.com/?linkid=9809776](https://go.microsoft.com/?linkid=9809776)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="2995b-633">Go to [[https://go.microsoft.com/?linkid=9809776](https://go.microsoft.com/?linkid=9809776)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="2995b-634">Como alternativa, se você já tiver instalado o Web Platform Installer, você pode abri-la e procure o produto &quot; *o WebMatrix 2*&quot;.</span><span class="sxs-lookup"><span data-stu-id="2995b-634">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*WebMatrix 2*&quot;.</span></span>
2. <span data-ttu-id="2995b-635">Clique em **instalar agora**.</span><span class="sxs-lookup"><span data-stu-id="2995b-635">Click on **Install Now**.</span></span> <span data-ttu-id="2995b-636">Se você não tem **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.</span><span class="sxs-lookup"><span data-stu-id="2995b-636">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="2995b-637">Uma vez **Web Platform Installer** é aberto, clique em **instalar** para iniciar a instalação.</span><span class="sxs-lookup"><span data-stu-id="2995b-637">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="2995b-638">![Instalar o WebMatrix 2](whats-new-in-aspnet-mvc-4/_static/image49.png "instalar o WebMatrix 2")</span><span class="sxs-lookup"><span data-stu-id="2995b-638">![Install WebMatrix 2](whats-new-in-aspnet-mvc-4/_static/image49.png "Install WebMatrix 2")</span></span>

    <span data-ttu-id="2995b-639">*Instalar o WebMatrix 2*</span><span class="sxs-lookup"><span data-stu-id="2995b-639">*Install WebMatrix 2*</span></span>
4. <span data-ttu-id="2995b-640">Leia os termos e licenças de todos os produtos e clique em **aceito** para continuar.</span><span class="sxs-lookup"><span data-stu-id="2995b-640">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    <span data-ttu-id="2995b-641">![Aceitar os termos de licença](whats-new-in-aspnet-mvc-4/_static/image50.png "aceitar os termos de licença")</span><span class="sxs-lookup"><span data-stu-id="2995b-641">![Accepting the license terms](whats-new-in-aspnet-mvc-4/_static/image50.png "Accepting the license terms")</span></span>

    <span data-ttu-id="2995b-642">*Aceitar os termos de licença*</span><span class="sxs-lookup"><span data-stu-id="2995b-642">*Accepting the license terms*</span></span>
5. <span data-ttu-id="2995b-643">Aguarde até que o processo de download e instalação seja concluída.</span><span class="sxs-lookup"><span data-stu-id="2995b-643">Wait until the downloading and installation process completes.</span></span>

    <span data-ttu-id="2995b-644">![Progresso da instalação](whats-new-in-aspnet-mvc-4/_static/image51.png "progresso da instalação")</span><span class="sxs-lookup"><span data-stu-id="2995b-644">![Installation progress](whats-new-in-aspnet-mvc-4/_static/image51.png "Installation progress")</span></span>

    <span data-ttu-id="2995b-645">*Progresso da instalação*</span><span class="sxs-lookup"><span data-stu-id="2995b-645">*Installation progress*</span></span>
6. <span data-ttu-id="2995b-646">Quando a instalação for concluída, clique em **concluir**.</span><span class="sxs-lookup"><span data-stu-id="2995b-646">When the installation completes, click **Finish**.</span></span>

    <span data-ttu-id="2995b-647">![Instalação concluída](whats-new-in-aspnet-mvc-4/_static/image52.png "instalação concluída")</span><span class="sxs-lookup"><span data-stu-id="2995b-647">![Installation completed](whats-new-in-aspnet-mvc-4/_static/image52.png "Installation completed")</span></span>

    <span data-ttu-id="2995b-648">*Instalação concluída*</span><span class="sxs-lookup"><span data-stu-id="2995b-648">*Installation completed*</span></span>
7. <span data-ttu-id="2995b-649">Clique em **saída** para fechar o Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="2995b-649">Click **Exit** to close Web Platform Installer.</span></span>

<a id="ApxCTask2"></a>

<a id="Task_2_-_Installing_the_iPhone_Simulator_Extension"></a>
#### <a name="task-2---installing-the-iphone-simulator-extension"></a><span data-ttu-id="2995b-650">Tarefa 2 - instalar o extensão do simulador de iPhone</span><span class="sxs-lookup"><span data-stu-id="2995b-650">Task 2 - Installing the iPhone Simulator Extension</span></span>

1. <span data-ttu-id="2995b-651">Executar **WebMatrix** e abrir qualquer site existente ou criar um novo.</span><span class="sxs-lookup"><span data-stu-id="2995b-651">Run **WebMatrix** and open any existing Web site or create a new one.</span></span>
2. <span data-ttu-id="2995b-652">Clique o **executar** botão do **início** de faixa de opções e selecione **adicionar novo**.</span><span class="sxs-lookup"><span data-stu-id="2995b-652">Click the **Run** button from the **Home** ribbon and select **Add new**.</span></span>

    <span data-ttu-id="2995b-653">![Adicionar nova extensão do WebMatrix](whats-new-in-aspnet-mvc-4/_static/image53.png "adicionar nova extensão do WebMatrix")</span><span class="sxs-lookup"><span data-stu-id="2995b-653">![Adding new WebMatrix extension](whats-new-in-aspnet-mvc-4/_static/image53.png "Adding new WebMatrix extension")</span></span>

    <span data-ttu-id="2995b-654">*Adicionar nova extensão do WebMatrix*</span><span class="sxs-lookup"><span data-stu-id="2995b-654">*Adding new WebMatrix extension*</span></span>
3. <span data-ttu-id="2995b-655">Selecione **iPhone simulador** e clique em **instalar**.</span><span class="sxs-lookup"><span data-stu-id="2995b-655">Select **iPhone Simulator** and click **Install**.</span></span>

    <span data-ttu-id="2995b-656">![Procurando extensões WebMatrix](whats-new-in-aspnet-mvc-4/_static/image54.png "extensões de pesquisa no WebMatrix")</span><span class="sxs-lookup"><span data-stu-id="2995b-656">![Browsing WebMatrix extensions](whats-new-in-aspnet-mvc-4/_static/image54.png "Browsing WebMatrix extensions")</span></span>

    <span data-ttu-id="2995b-657">*Procurando extensões do WebMatrix*</span><span class="sxs-lookup"><span data-stu-id="2995b-657">*Browsing WebMatrix extensions*</span></span>
4. <span data-ttu-id="2995b-658">Nos detalhes do pacote, clique em **instalar** para continuar com a instalação da extensão.</span><span class="sxs-lookup"><span data-stu-id="2995b-658">In the package details, click **Install** to continue with the extension installation.</span></span>

    <span data-ttu-id="2995b-659">![extensão do simulador de iPhone](whats-new-in-aspnet-mvc-4/_static/image55.png "extensão simulador de iPhone")</span><span class="sxs-lookup"><span data-stu-id="2995b-659">![iPhone Simulator extension](whats-new-in-aspnet-mvc-4/_static/image55.png "iPhone Simulator extension")</span></span>

    <span data-ttu-id="2995b-660">*extensão do simulador de iPhone*</span><span class="sxs-lookup"><span data-stu-id="2995b-660">*iPhone Simulator extension*</span></span>
5. <span data-ttu-id="2995b-661">Leia e aceite o EULA de extensão.</span><span class="sxs-lookup"><span data-stu-id="2995b-661">Read and accept the extension EULA.</span></span>

    <span data-ttu-id="2995b-662">![Extensão do WebMatrix EULA](whats-new-in-aspnet-mvc-4/_static/image56.png "EULA de extensão do WebMatrix")</span><span class="sxs-lookup"><span data-stu-id="2995b-662">![WebMatrix extension EULA](whats-new-in-aspnet-mvc-4/_static/image56.png "WebMatrix extension EULA")</span></span>

    <span data-ttu-id="2995b-663">*Extensão do WebMatrix EULA*</span><span class="sxs-lookup"><span data-stu-id="2995b-663">*WebMatrix extension EULA*</span></span>
6. <span data-ttu-id="2995b-664">Agora, você pode executar o site do WebMatrix usando o opção de simulador de iPhone.</span><span class="sxs-lookup"><span data-stu-id="2995b-664">Now, you can run your Web site from WebMatrix using the iPhone Simulator option.</span></span>

    <span data-ttu-id="2995b-665">![Executar usando iPhone](whats-new-in-aspnet-mvc-4/_static/image57.png "executados usando iPhone")</span><span class="sxs-lookup"><span data-stu-id="2995b-665">![Run using iPhone](whats-new-in-aspnet-mvc-4/_static/image57.png "Run using iPhone")</span></span>

    <span data-ttu-id="2995b-666">*Executar usando iPhone*</span><span class="sxs-lookup"><span data-stu-id="2995b-666">*Run using iPhone*</span></span>

<a id="ApxCTask3"></a>

<a id="Task_3_-_Configuring_Visual_Studio_2012_to_run_iPhone_Simulator"></a>
#### <a name="task-3---configuring-visual-studio-2012-to-run-iphone-simulator"></a><span data-ttu-id="2995b-667">Tarefa 3: configurar o Visual Studio 2012 para executar o simulador de iPhone</span><span class="sxs-lookup"><span data-stu-id="2995b-667">Task 3 - Configuring Visual Studio 2012 to run iPhone Simulator</span></span>

1. <span data-ttu-id="2995b-668">Abra **Visual Studio 2012** e abrir qualquer site da Web ou criar um novo projeto.</span><span class="sxs-lookup"><span data-stu-id="2995b-668">Open **Visual Studio 2012** and open any Web site or create a new project.</span></span>
2. <span data-ttu-id="2995b-669">Clique na seta para baixo no botão Executar e selecione **procurar com**.</span><span class="sxs-lookup"><span data-stu-id="2995b-669">Click the down arrow from the Run button and select **Browse with**.</span></span>

    <span data-ttu-id="2995b-670">![Procurar com](whats-new-in-aspnet-mvc-4/_static/image58.png "procurar com")</span><span class="sxs-lookup"><span data-stu-id="2995b-670">![Browse with](whats-new-in-aspnet-mvc-4/_static/image58.png "Browse with")</span></span>

    <span data-ttu-id="2995b-671">*Procurar com*</span><span class="sxs-lookup"><span data-stu-id="2995b-671">*Browse with*</span></span>
3. <span data-ttu-id="2995b-672">No &quot;procurar com&quot; caixa de diálogo, clique em **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="2995b-672">In the &quot;Browse With&quot; dialog, click **Add**.</span></span>
4. <span data-ttu-id="2995b-673">No &quot;adicionar programa&quot; caixa de diálogo, use os seguintes valores:</span><span class="sxs-lookup"><span data-stu-id="2995b-673">In the &quot;Add Program&quot; dialog, use the following values:</span></span>

    - <span data-ttu-id="2995b-674">**Programa**: C:\Users\*{CurrentUser}*\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe *(adequadamente o caminho de atualização)*</span><span class="sxs-lookup"><span data-stu-id="2995b-674">**Program**: C:\Users\*{CurrentUser}*\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe *(update the path accordingly)*</span></span>
    - <span data-ttu-id="2995b-675">**Argumentos**: &quot;1&quot;</span><span class="sxs-lookup"><span data-stu-id="2995b-675">**Arguments**: &quot;1&quot;</span></span>
    - <span data-ttu-id="2995b-676">**Nome amigável**: simulador de iPhone</span><span class="sxs-lookup"><span data-stu-id="2995b-676">**Friendly name**: iPhone Simulator</span></span>

    <span data-ttu-id="2995b-677">![Adicionar programa](whats-new-in-aspnet-mvc-4/_static/image59.png "adicionar programa")</span><span class="sxs-lookup"><span data-stu-id="2995b-677">![Add program](whats-new-in-aspnet-mvc-4/_static/image59.png "Add program")</span></span>

    <span data-ttu-id="2995b-678">*Adicionar programa para procurar com*</span><span class="sxs-lookup"><span data-stu-id="2995b-678">*Add program to browse with*</span></span>
5. <span data-ttu-id="2995b-679">Clique em **Okey** e feche as caixas de diálogo.</span><span class="sxs-lookup"><span data-stu-id="2995b-679">Click **OK** and close the dialogs.</span></span>
6. <span data-ttu-id="2995b-680">Agora é possível executar seus aplicativos Web no iPhone simulador do Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="2995b-680">Now you are able to run your Web applications in the iPhone simulator from Visual Studio 2012.</span></span>

    <span data-ttu-id="2995b-681">![Procurar com iPhone simulador](whats-new-in-aspnet-mvc-4/_static/image60.png "procurar com simulador de iPhone")</span><span class="sxs-lookup"><span data-stu-id="2995b-681">![Browse with iPhone Simulator](whats-new-in-aspnet-mvc-4/_static/image60.png "Browse with iPhone Simulator")</span></span>

    <span data-ttu-id="2995b-682">*Procurar com simulador de iPhone*</span><span class="sxs-lookup"><span data-stu-id="2995b-682">*Browse with iPhone Simulator*</span></span>

<a id="AppendixD"></a>

<a id="Appendix_D_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-d-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="2995b-683">Apêndice d: publicar um aplicativo ASP.NET MVC 4 usando a implantação da Web</span><span class="sxs-lookup"><span data-stu-id="2995b-683">Appendix D: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="2995b-684">Este apêndice mostram como criar um novo site do Portal de gerenciamento do Windows Azure e publicar o aplicativo que você obteve seguindo o laboratório, aproveitando o recurso de publicação de implantação da Web fornecido pelo Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="2995b-684">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxDTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="2995b-685">Tarefa 1: criar um novo Site do Windows Azure Portal</span><span class="sxs-lookup"><span data-stu-id="2995b-685">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="2995b-686">Vá para o [Portal de gerenciamento do Windows Azure](https://manage.windowsazure.com/) e entre usando as credenciais da Microsoft associadas à sua assinatura.</span><span class="sxs-lookup"><span data-stu-id="2995b-686">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2995b-687">Com o Windows Azure, você pode hospedar 10 Sites da Web ASP.NET gratuitamente e, em seguida, dimensione conforme seu tráfego cresce.</span><span class="sxs-lookup"><span data-stu-id="2995b-687">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="2995b-688">Você pode inscrever [aqui](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="2995b-688">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="2995b-689">![Faça logon no portal do Windows Azure](whats-new-in-aspnet-mvc-4/_static/image61.png "faça logon no portal do Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="2995b-689">![Log on to Windows Azure portal](whats-new-in-aspnet-mvc-4/_static/image61.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="2995b-690">*Faça logon no Portal de gerenciamento do Azure do Windows*</span><span class="sxs-lookup"><span data-stu-id="2995b-690">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="2995b-691">Clique em **novo** na barra de comandos.</span><span class="sxs-lookup"><span data-stu-id="2995b-691">Click **New** on the command bar.</span></span>

    <span data-ttu-id="2995b-692">![Criando um novo Site](whats-new-in-aspnet-mvc-4/_static/image62.png "criar um novo Site")</span><span class="sxs-lookup"><span data-stu-id="2995b-692">![Creating a new Web Site](whats-new-in-aspnet-mvc-4/_static/image62.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="2995b-693">*Criando um novo Site*</span><span class="sxs-lookup"><span data-stu-id="2995b-693">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="2995b-694">Clique em **de computação** | **Site da Web**.</span><span class="sxs-lookup"><span data-stu-id="2995b-694">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="2995b-695">Em seguida, selecione **criação rápida** opção.</span><span class="sxs-lookup"><span data-stu-id="2995b-695">Then select **Quick Create** option.</span></span> <span data-ttu-id="2995b-696">Forneça uma URL disponível para o novo site e clique em **Criar Site**.</span><span class="sxs-lookup"><span data-stu-id="2995b-696">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2995b-697">Um Site do Windows Azure é o host para um aplicativo web em execução na nuvem que você pode controlar e gerenciar.</span><span class="sxs-lookup"><span data-stu-id="2995b-697">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="2995b-698">A opção criação rápida permite que você implante um aplicativo web concluído ao Site do Windows Azure de fora do portal.</span><span class="sxs-lookup"><span data-stu-id="2995b-698">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="2995b-699">Ele não inclui etapas para configurar um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="2995b-699">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="2995b-700">![Criando um novo Site usando a criação rápida](whats-new-in-aspnet-mvc-4/_static/image63.png "criar um novo Site usando a criação rápida")</span><span class="sxs-lookup"><span data-stu-id="2995b-700">![Creating a new Web Site using Quick Create](whats-new-in-aspnet-mvc-4/_static/image63.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="2995b-701">*Criando um novo Site usando a criação rápida*</span><span class="sxs-lookup"><span data-stu-id="2995b-701">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="2995b-702">Aguarde até que o novo **Site da Web** é criado.</span><span class="sxs-lookup"><span data-stu-id="2995b-702">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="2995b-703">Depois que o Site da Web é criado, clique no link sob a **URL** coluna.</span><span class="sxs-lookup"><span data-stu-id="2995b-703">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="2995b-704">Verifique se o novo Site da Web está funcionando.</span><span class="sxs-lookup"><span data-stu-id="2995b-704">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="2995b-705">![Navegando para o novo site](whats-new-in-aspnet-mvc-4/_static/image64.png "navegando para o novo site")</span><span class="sxs-lookup"><span data-stu-id="2995b-705">![Browsing to the new web site](whats-new-in-aspnet-mvc-4/_static/image64.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="2995b-706">*Navegando para o novo site*</span><span class="sxs-lookup"><span data-stu-id="2995b-706">*Browsing to the new web site*</span></span>

    <span data-ttu-id="2995b-707">![Site da Web em execução](whats-new-in-aspnet-mvc-4/_static/image65.png "site em execução")</span><span class="sxs-lookup"><span data-stu-id="2995b-707">![Web site running](whats-new-in-aspnet-mvc-4/_static/image65.png "Web site running")</span></span>

    <span data-ttu-id="2995b-708">*Site da Web em execução*</span><span class="sxs-lookup"><span data-stu-id="2995b-708">*Web site running*</span></span>
6. <span data-ttu-id="2995b-709">Acesse o portal e clique no nome do site sob o **nome** coluna para exibir as páginas de gerenciamento.</span><span class="sxs-lookup"><span data-stu-id="2995b-709">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="2995b-710">![Abrir as páginas de gerenciamento do site da web](whats-new-in-aspnet-mvc-4/_static/image66.png "abrir páginas de gerenciamento do site")</span><span class="sxs-lookup"><span data-stu-id="2995b-710">![Opening the web site management pages](whats-new-in-aspnet-mvc-4/_static/image66.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="2995b-711">*Abrir as páginas de gerenciamento do Site da Web*</span><span class="sxs-lookup"><span data-stu-id="2995b-711">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="2995b-712">No **painel** página no **visão rápida** seção, clique o **baixar perfil de publicação** link.</span><span class="sxs-lookup"><span data-stu-id="2995b-712">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2995b-713">O *perfil de publicação* contém todas as informações necessárias para publicar um aplicativo web para um site do Windows Azure para cada método de publicação habilitado.</span><span class="sxs-lookup"><span data-stu-id="2995b-713">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="2995b-714">O perfil de publicação contém as URLs, as credenciais do usuário e cadeias de caracteres de banco de dados necessárias para se conectar e autenticar cada um dos pontos de extremidade para o qual um método de publicação está habilitado.</span><span class="sxs-lookup"><span data-stu-id="2995b-714">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="2995b-715">**Microsoft WebMatrix 2**, **Microsoft Visual Studio para Web Express** e **Microsoft Visual Studio 2012** oferecem suporte à leitura perfis de publicação para automatizar a configuração desses programas para publicação de aplicativos web para sites do Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="2995b-715">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="2995b-716">![Baixando o site da web de perfil de publicação](whats-new-in-aspnet-mvc-4/_static/image67.png "baixando o site da web de perfil de publicação")</span><span class="sxs-lookup"><span data-stu-id="2995b-716">![Downloading the web site publish profile](whats-new-in-aspnet-mvc-4/_static/image67.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="2995b-717">*Baixando o Site da Web de perfil de publicação*</span><span class="sxs-lookup"><span data-stu-id="2995b-717">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="2995b-718">Baixe o arquivo de perfil de publicação para um local conhecido.</span><span class="sxs-lookup"><span data-stu-id="2995b-718">Download the publish profile file to a known location.</span></span> <span data-ttu-id="2995b-719">Mais neste exercício, você verá como usar esse arquivo para publicar um aplicativo web para um Windows Azure Web Sites do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2995b-719">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="2995b-720">![Salvando o arquivo de perfil de publicação](whats-new-in-aspnet-mvc-4/_static/image68.png "salvar o perfil de publicação")</span><span class="sxs-lookup"><span data-stu-id="2995b-720">![Saving the publish profile file](whats-new-in-aspnet-mvc-4/_static/image68.png "Saving the publish profile")</span></span>

    <span data-ttu-id="2995b-721">*Salvando o arquivo de perfil de publicação*</span><span class="sxs-lookup"><span data-stu-id="2995b-721">*Saving the publish profile file*</span></span>

<a id="ApxDTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="2995b-722">Tarefa 2 – Configurando o servidor de banco de dados</span><span class="sxs-lookup"><span data-stu-id="2995b-722">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="2995b-723">Se seu aplicativo utiliza o SQL Server bancos de dados que você precisará criar um servidor de banco de dados SQL.</span><span class="sxs-lookup"><span data-stu-id="2995b-723">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="2995b-724">Se você deseja implantar um aplicativo simples que não usa o SQL Server, você pode ignorar esta tarefa.</span><span class="sxs-lookup"><span data-stu-id="2995b-724">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="2995b-725">Você precisará de um servidor de banco de dados SQL para armazenar o banco de dados do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2995b-725">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="2995b-726">Você pode exibir os servidores de banco de dados SQL de sua assinatura no portal de gerenciamento do Windows Azure em **bancos de dados Sql** | **servidores** | **do servidor Painel**.</span><span class="sxs-lookup"><span data-stu-id="2995b-726">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="2995b-727">Se você não tiver um servidor criado, você pode criar um usando o **adicionar** botão na barra de comandos.</span><span class="sxs-lookup"><span data-stu-id="2995b-727">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="2995b-728">Anote o **nome do servidor e o URL, o nome de logon de administrador e senha**, pois você usará nas próximas tarefas.</span><span class="sxs-lookup"><span data-stu-id="2995b-728">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="2995b-729">Não crie o banco de dados, como ele será criado em um estágio posterior.</span><span class="sxs-lookup"><span data-stu-id="2995b-729">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="2995b-730">![Painel do servidor de banco de dados SQL](whats-new-in-aspnet-mvc-4/_static/image69.png "painel de banco de dados do SQL Server")</span><span class="sxs-lookup"><span data-stu-id="2995b-730">![SQL Database Server Dashboard](whats-new-in-aspnet-mvc-4/_static/image69.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="2995b-731">*Painel de banco de dados do SQL Server*</span><span class="sxs-lookup"><span data-stu-id="2995b-731">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="2995b-732">A próxima tarefa, você testará a conexão de banco de dados do Visual Studio, por esse motivo, você precisa incluir seu endereço IP local na lista do servidor de **endereços IP permitidos**.</span><span class="sxs-lookup"><span data-stu-id="2995b-732">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="2995b-733">Para fazer isso, clique em **configurar**, selecione o endereço IP do **endereço IP do cliente atual** e cole-o no **endereço IP inicial** e **oendereçoIPfinal** caixas de texto e clique no ![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png) botão.</span><span class="sxs-lookup"><span data-stu-id="2995b-733">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png) button.</span></span>

    ![Adicionar endereço IP do cliente](whats-new-in-aspnet-mvc-4/_static/image71.png)

    <span data-ttu-id="2995b-735">*Adicionar endereço IP do cliente*</span><span class="sxs-lookup"><span data-stu-id="2995b-735">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="2995b-736">Uma vez o **endereço IP do cliente** é adicionado para os endereços IP permitidos, clique em **salvar** para confirmar as alterações.</span><span class="sxs-lookup"><span data-stu-id="2995b-736">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Confirmar alterações](whats-new-in-aspnet-mvc-4/_static/image72.png)

    <span data-ttu-id="2995b-738">*Confirmar alterações*</span><span class="sxs-lookup"><span data-stu-id="2995b-738">*Confirm Changes*</span></span>

<a id="ApxDTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="2995b-739">Tarefa 3: publicar um aplicativo ASP.NET MVC 4 usando a implantação da Web</span><span class="sxs-lookup"><span data-stu-id="2995b-739">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="2995b-740">Volte para a solução do ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="2995b-740">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="2995b-741">No **Solution Explorer**, com o botão direito no projeto de site da web e selecione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="2995b-741">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="2995b-742">![Publicar o aplicativo](whats-new-in-aspnet-mvc-4/_static/image73.png "a publicação do aplicativo")</span><span class="sxs-lookup"><span data-stu-id="2995b-742">![Publishing the Application](whats-new-in-aspnet-mvc-4/_static/image73.png "Publishing the Application")</span></span>

    <span data-ttu-id="2995b-743">*Publicar o site*</span><span class="sxs-lookup"><span data-stu-id="2995b-743">*Publishing the web site*</span></span>
2. <span data-ttu-id="2995b-744">Importe o perfil de publicação que você salvou na primeira tarefa.</span><span class="sxs-lookup"><span data-stu-id="2995b-744">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="2995b-745">![Importar o perfil de publicação](whats-new-in-aspnet-mvc-4/_static/image74.png "importar o perfil de publicação")</span><span class="sxs-lookup"><span data-stu-id="2995b-745">![Importing the publish profile](whats-new-in-aspnet-mvc-4/_static/image74.png "Importing the publish profile")</span></span>

    <span data-ttu-id="2995b-746">*Importar o perfil de publicação*</span><span class="sxs-lookup"><span data-stu-id="2995b-746">*Importing publish profile*</span></span>
3. <span data-ttu-id="2995b-747">Clique em **validar Conexão**.</span><span class="sxs-lookup"><span data-stu-id="2995b-747">Click **Validate Connection**.</span></span> <span data-ttu-id="2995b-748">Quando a validação estiver concluída, clique em **próximo**.</span><span class="sxs-lookup"><span data-stu-id="2995b-748">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2995b-749">A validação é concluída depois que aparecer uma marca de seleção verde ao lado do botão Validar Conexão.</span><span class="sxs-lookup"><span data-stu-id="2995b-749">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="2995b-750">![Validando a conexão](whats-new-in-aspnet-mvc-4/_static/image75.png "validação de conexão")</span><span class="sxs-lookup"><span data-stu-id="2995b-750">![Validating connection](whats-new-in-aspnet-mvc-4/_static/image75.png "Validating connection")</span></span>

    <span data-ttu-id="2995b-751">*Validação de conexão*</span><span class="sxs-lookup"><span data-stu-id="2995b-751">*Validating connection*</span></span>
4. <span data-ttu-id="2995b-752">No **configurações** página no **bancos de dados** seção, clique no botão ao lado da caixa de texto da sua conexão de banco de dados (ou seja, **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="2995b-752">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="2995b-753">![Configuração de implantação da Web](whats-new-in-aspnet-mvc-4/_static/image76.png "configuração de implantação da Web")</span><span class="sxs-lookup"><span data-stu-id="2995b-753">![Web deploy configuration](whats-new-in-aspnet-mvc-4/_static/image76.png "Web deploy configuration")</span></span>

    <span data-ttu-id="2995b-754">*Configuração de implantação da Web*</span><span class="sxs-lookup"><span data-stu-id="2995b-754">*Web deploy configuration*</span></span>
5. <span data-ttu-id="2995b-755">Configure a conexão de banco de dados da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="2995b-755">Configure the database connection as follows:</span></span>

    - <span data-ttu-id="2995b-756">No **nome do servidor** digite sua URL de servidor de banco de dados SQL usando o *tcp:* prefixo.</span><span class="sxs-lookup"><span data-stu-id="2995b-756">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
    - <span data-ttu-id="2995b-757">Em **nome de usuário** digite seu nome de logon de administrador do servidor.</span><span class="sxs-lookup"><span data-stu-id="2995b-757">In **User name** type your server administrator login name.</span></span>
    - <span data-ttu-id="2995b-758">Em **senha** digite sua senha de logon de administrador do servidor.</span><span class="sxs-lookup"><span data-stu-id="2995b-758">In **Password** type your server administrator login password.</span></span>
    - <span data-ttu-id="2995b-759">Digite um novo nome de banco de dados, por exemplo: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="2995b-759">Type a new database name, for example: *MVC4SampleDB*.</span></span>

    <span data-ttu-id="2995b-760">![Configurando a cadeia de caracteres de conexão de destino](whats-new-in-aspnet-mvc-4/_static/image77.png "Configurando a cadeia de caracteres de conexão de destino")</span><span class="sxs-lookup"><span data-stu-id="2995b-760">![Configuring destination connection string](whats-new-in-aspnet-mvc-4/_static/image77.png "Configuring destination connection string")</span></span>

    <span data-ttu-id="2995b-761">*Configurando a cadeia de caracteres de conexão de destino*</span><span class="sxs-lookup"><span data-stu-id="2995b-761">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="2995b-762">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="2995b-762">Then click **OK**.</span></span> <span data-ttu-id="2995b-763">Quando solicitado a criar o banco de dados, clique em **Sim**.</span><span class="sxs-lookup"><span data-stu-id="2995b-763">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="2995b-764">![Criando o banco de dados](whats-new-in-aspnet-mvc-4/_static/image78.png "criar a cadeia de caracteres do banco de dados")</span><span class="sxs-lookup"><span data-stu-id="2995b-764">![Creating the database](whats-new-in-aspnet-mvc-4/_static/image78.png "Creating the database string")</span></span>

    <span data-ttu-id="2995b-765">*Criando o banco de dados*</span><span class="sxs-lookup"><span data-stu-id="2995b-765">*Creating the database*</span></span>
7. <span data-ttu-id="2995b-766">A cadeia de caracteres de conexão que você usará para se conectar ao banco de dados SQL no Windows Azure é mostrada na caixa de texto de Conexão padrão.</span><span class="sxs-lookup"><span data-stu-id="2995b-766">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="2995b-767">Clique em **Avançar**.</span><span class="sxs-lookup"><span data-stu-id="2995b-767">Then click **Next**.</span></span>

    <span data-ttu-id="2995b-768">![Cadeia de caracteres de Conexão que aponta para o banco de dados SQL](whats-new-in-aspnet-mvc-4/_static/image79.png "cadeia de caracteres de Conexão que aponta para o banco de dados SQL")</span><span class="sxs-lookup"><span data-stu-id="2995b-768">![Connection string pointing to SQL Database](whats-new-in-aspnet-mvc-4/_static/image79.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="2995b-769">*Cadeia de caracteres de Conexão que aponta para o banco de dados SQL*</span><span class="sxs-lookup"><span data-stu-id="2995b-769">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="2995b-770">No **visualização** , clique em **publicar**.</span><span class="sxs-lookup"><span data-stu-id="2995b-770">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="2995b-771">![Publicar o aplicativo web](whats-new-in-aspnet-mvc-4/_static/image80.png "publicar o aplicativo web")</span><span class="sxs-lookup"><span data-stu-id="2995b-771">![Publishing the web application](whats-new-in-aspnet-mvc-4/_static/image80.png "Publishing the web application")</span></span>

    <span data-ttu-id="2995b-772">*Publicar o aplicativo web*</span><span class="sxs-lookup"><span data-stu-id="2995b-772">*Publishing the web application*</span></span>
9. <span data-ttu-id="2995b-773">Quando termina o processo de publicação, o navegador padrão abrirá o site da web publicado.</span><span class="sxs-lookup"><span data-stu-id="2995b-773">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="2995b-774">![Aplicativo publicado no Windows Azure](whats-new-in-aspnet-mvc-4/_static/image81.png "aplicativo publicado no Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="2995b-774">![Application published to Windows Azure](whats-new-in-aspnet-mvc-4/_static/image81.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="2995b-775">*Aplicativos publicados no Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="2995b-775">*Application published to Windows Azure*</span></span>
