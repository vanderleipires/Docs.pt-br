---
uid: mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
title: O que há de novo no ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC 4 é uma estrutura para criar aplicativos web escalonável, baseado em padrões usando padrões de design bem estabelecidos e o poder do ASP.NET e...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 48f7feb3-872f-485d-b96f-e30011ff8c4a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 977a6b5a84825ebd087752dcc2ebc0c5410e1657
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
# <a name="whats-new-in-aspnet-mvc-4"></a><span data-ttu-id="65df1-103">O que há de novo no ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="65df1-103">What's New in ASP.NET MVC 4</span></span>

<span data-ttu-id="65df1-104">Por [Web Camps Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="65df1-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="65df1-105">Baixar o Kit de treinamento de Camps de Web</span><span class="sxs-lookup"><span data-stu-id="65df1-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="65df1-106">ASP.NET MVC 4 é uma estrutura para criar aplicativos web escalonável, baseado em padrões usando padrões de design bem estabelecidos e o poder do ASP.NET e o .NET framework.</span><span class="sxs-lookup"><span data-stu-id="65df1-106">ASP.NET MVC 4 is a framework for building scalable, standards-based web applications using well-established design patterns and the power of the ASP.NET and the .NET framework.</span></span> <span data-ttu-id="65df1-107">Essa nova quarta versão do framework enfoca facilita o desenvolvimento de aplicativos web móvel.</span><span class="sxs-lookup"><span data-stu-id="65df1-107">This new, fourth version of the framework focuses on making mobile web application development easier.</span></span>

<span data-ttu-id="65df1-108">Em primeiro lugar, quando você cria um novo projeto ASP.NET MVC 4 é agora um modelo de projeto de aplicativo móvel que você pode usar para compilar um aplicativo autônomo especificamente para dispositivos móveis.</span><span class="sxs-lookup"><span data-stu-id="65df1-108">To begin with, when you create a new ASP.NET MVC 4 project there is now a mobile application project template you can use to build a standalone app specifically for mobile devices.</span></span> <span data-ttu-id="65df1-109">Além disso, ASP.NET MVC 4 integra-se com o jQuery Mobile por meio de um pacote jQuery.Mobile.MVC NuGet.</span><span class="sxs-lookup"><span data-stu-id="65df1-109">Additionally, ASP.NET MVC 4 integrates with jQuery Mobile through a jQuery.Mobile.MVC NuGet package.</span></span> <span data-ttu-id="65df1-110">o jQuery Mobile é uma estrutura baseada em HTML5 para o desenvolvimento de aplicativos web que são compatíveis com todas as plataformas de dispositivo móvel popular, incluindo Windows Phone, iPhone, Android e assim por diante.</span><span class="sxs-lookup"><span data-stu-id="65df1-110">jQuery Mobile is an HTML5-based framework for developing web apps that are compatible with all popular mobile device platforms, including Windows Phone, iPhone, Android and so on.</span></span> <span data-ttu-id="65df1-111">No entanto, se você precisar de especialização, ASP.NET MVC 4 também permite que você atender a diferentes modos de exibição para diferentes dispositivos e fornecer otimizações específicas do dispositivo.</span><span class="sxs-lookup"><span data-stu-id="65df1-111">However, if you need specialization, ASP.NET MVC 4 also enables you to serve different views for different devices and provide device-specific optimizations.</span></span>

<span data-ttu-id="65df1-112">Este laboratório prático, você iniciará com o ASP.NET MVC 4 &quot;aplicativo de Internet&quot; modelo de projeto para criar um aplicativo da Galeria de fotos.</span><span class="sxs-lookup"><span data-stu-id="65df1-112">In this hands-on lab, you will start with the ASP.NET MVC 4 &quot;Internet Application&quot; project template to create a Photo Gallery application.</span></span> <span data-ttu-id="65df1-113">Você aprimorará progressivamente o aplicativo usando o jQuery Mobile e recursos novos do ASP.NET MVC 4 para torná-lo compatível com navegadores da web da área de trabalho e de dispositivos móveis diferentes.</span><span class="sxs-lookup"><span data-stu-id="65df1-113">You will progressively enhance the app using jQuery Mobile and ASP.NET MVC 4's new features to make it compatible with different mobile devices and desktop web browsers.</span></span> <span data-ttu-id="65df1-114">Você também aprenderá sobre novo receitas de código para geração de código e como o ASP.NET MVC 4 torna mais fácil para gravar os métodos de ação assíncrono com o suporte à tarefa&lt;ActionResult&gt; tipos de retorno.</span><span class="sxs-lookup"><span data-stu-id="65df1-114">You will also learn about new code recipes for code generation and how ASP.NET MVC 4 makes it easier for you to write asynchronous action methods by supporting Task&lt;ActionResult&gt; return types.</span></span>

> [!NOTE]
> <span data-ttu-id="65df1-115">Todo o código de exemplo e trechos de código são incluídos no Web Camps treinamento Kit, disponíveis em [versões Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="65df1-115">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="65df1-116">O projeto específico para este laboratório está disponível em [o que há de novo no Web Forms no ASP.NET 4.5](https://github.com/Microsoft-Web/HOL-ASPNETWebForms).</span><span class="sxs-lookup"><span data-stu-id="65df1-116">The project specific to this lab is available at [What's New in Web Forms in ASP.NET 4.5](https://github.com/Microsoft-Web/HOL-ASPNETWebForms).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="65df1-117">Objetivos</span><span class="sxs-lookup"><span data-stu-id="65df1-117">Objectives</span></span>

<span data-ttu-id="65df1-118">Este laboratório prático, você aprenderá como:</span><span class="sxs-lookup"><span data-stu-id="65df1-118">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="65df1-119">Aproveitar os aprimoramentos para o ASP.NET MVC projeto modelos, incluindo o novo modelo de projeto de aplicativo móvel</span><span class="sxs-lookup"><span data-stu-id="65df1-119">Take advantage of the enhancements to the ASP.NET MVC project templates-including the new mobile application project template</span></span>
- <span data-ttu-id="65df1-120">Use o atributo de visor do HTML5 e consultas de mídia CSS para melhorar a exibição em dispositivos móveis</span><span class="sxs-lookup"><span data-stu-id="65df1-120">Use the HTML5 viewport attribute and CSS media queries to improve the display on mobile devices</span></span>
- <span data-ttu-id="65df1-121">Use o jQuery Mobile para aprimoramentos progressivos e para a criação de interface da web otimizada para toque</span><span class="sxs-lookup"><span data-stu-id="65df1-121">Use jQuery Mobile for progressive enhancements and for building touch-optimized web UI</span></span>
- <span data-ttu-id="65df1-122">Criar exibições específicas para dispositivos móveis</span><span class="sxs-lookup"><span data-stu-id="65df1-122">Create mobile-specific views</span></span>
- <span data-ttu-id="65df1-123">Use o componente de alternância de exibição para alternar entre modos de exibição móveis e área de trabalho do aplicativo</span><span class="sxs-lookup"><span data-stu-id="65df1-123">Use the view-switcher component to toggle between mobile and desktop views in the application</span></span>
- <span data-ttu-id="65df1-124">Criar controladores assíncronos usando o suporte da tarefa</span><span class="sxs-lookup"><span data-stu-id="65df1-124">Create asynchronous controllers using task support</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="65df1-125">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="65df1-125">Prerequisites</span></span>

<span data-ttu-id="65df1-126">Você deve ter os seguintes itens para concluir este laboratório:</span><span class="sxs-lookup"><span data-stu-id="65df1-126">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="65df1-127">[Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou superior (leitura [Apêndice B](#AppendixB) para obter instruções sobre como instalá-lo).</span><span class="sxs-lookup"><span data-stu-id="65df1-127">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix B](#AppendixB) for instructions on how to install it).</span></span>
- <span data-ttu-id="65df1-128">[ASP.NET MVC 4](../../../mvc4.md) (incluído na instalação do Microsoft Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="65df1-128">[ASP.NET MVC 4](../../../mvc4.md) (included in the Microsoft Visual Studio 2012 installation)</span></span>
- <span data-ttu-id="65df1-129">Emulador do Windows Phone (incluído no [Windows Phone 7.1.1 SDK](https://www.microsoft.com/download/details.aspx?id=29233))</span><span class="sxs-lookup"><span data-stu-id="65df1-129">Windows Phone Emulator (included in the [Windows Phone 7.1.1 SDK](https://www.microsoft.com/download/details.aspx?id=29233))</span></span>
- <span data-ttu-id="65df1-130">Opcional - [o WebMatrix 2](https://www.microsoft.com/web/webmatrix/) com **Electric Plum iPhone simulador** extensão (somente para o Exercício 3 usada para procurar o aplicativo web com um simulador de iPhone)</span><span class="sxs-lookup"><span data-stu-id="65df1-130">Optional - [WebMatrix 2](https://www.microsoft.com/web/webmatrix/) with **Electric Plum iPhone Simulator** extension (only for Exercise 3 used to browse the web application with an iPhone simulator)</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="65df1-131">Configuração</span><span class="sxs-lookup"><span data-stu-id="65df1-131">Setup</span></span>

<span data-ttu-id="65df1-132">Em todo o documento de laboratório, você será instruído a inserir blocos de código.</span><span class="sxs-lookup"><span data-stu-id="65df1-132">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="65df1-133">Para sua conveniência, a maioria do código é fornecido como o Visual Studio trechos de código que pode ser usado de dentro do Visual Studio para evitar a necessidade para adicioná-lo manualmente.</span><span class="sxs-lookup"><span data-stu-id="65df1-133">For your convenience, most of that code is provided as Visual Studio Code Snippets, which you can use from within Visual Studio to avoid having to add it manually.</span></span>

<span data-ttu-id="65df1-134">Para instalar os trechos de código:</span><span class="sxs-lookup"><span data-stu-id="65df1-134">To install the code snippets:</span></span>

1. <span data-ttu-id="65df1-135">Abra uma janela do Windows Explorer e navegue até o laboratório **Source\Setup** pasta.</span><span class="sxs-lookup"><span data-stu-id="65df1-135">Open a Windows Explorer window and browse to the lab's **Source\Setup** folder.</span></span>
2. <span data-ttu-id="65df1-136">Clique duas vezes o **Setup.cmd** arquivo nessa pasta para instalar os trechos de código do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="65df1-136">Double-click the **Setup.cmd** file in this folder to install the Visual Studio code snippets.</span></span>

<span data-ttu-id="65df1-137">Se você não estiver familiarizado com os trechos de código do Visual Studio e aprender como usá-los, consulte o Apêndice deste documento &quot; [trechos de código usando do apêndice a:](#AppendixA)&quot;.</span><span class="sxs-lookup"><span data-stu-id="65df1-137">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix A: Using Code Snippets](#AppendixA)&quot;.</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="65df1-138">Exercícios</span><span class="sxs-lookup"><span data-stu-id="65df1-138">Exercises</span></span>

<span data-ttu-id="65df1-139">Este laboratório prático inclui os seguintes exercícios:</span><span class="sxs-lookup"><span data-stu-id="65df1-139">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="65df1-140">Novos modelos de projeto do ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="65df1-140">New ASP.NET MVC 4 Project Templates</span></span>](#Exercise1)
2. [<span data-ttu-id="65df1-141">Criando o aplicativo Web de galeria de fotos</span><span class="sxs-lookup"><span data-stu-id="65df1-141">Creating the Photo Gallery Web Application</span></span>](#Exercise2)
3. [<span data-ttu-id="65df1-142">Adicionando suporte para dispositivos móveis</span><span class="sxs-lookup"><span data-stu-id="65df1-142">Adding Support for Mobile Devices</span></span>](#Exercise3)
4. [<span data-ttu-id="65df1-143">Usando controladores assíncronos</span><span class="sxs-lookup"><span data-stu-id="65df1-143">Using Asynchronous Controllers</span></span>](#Exercise4)

> [!NOTE]
> <span data-ttu-id="65df1-144">Cada exercício é acompanhado por um **final** pasta que contém a solução resultante, você deve obter depois de concluir os exercícios.</span><span class="sxs-lookup"><span data-stu-id="65df1-144">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="65df1-145">Você pode usar essa solução como um guia se você precisar trabalhar com os exercícios de ajuda adicional.</span><span class="sxs-lookup"><span data-stu-id="65df1-145">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="65df1-146">Tempo estimado para concluir este laboratório: **60 minutos**.</span><span class="sxs-lookup"><span data-stu-id="65df1-146">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_New_ASPNET_MVC_4_Project_Templates"></a>
### <a name="exercise-1-new-aspnet-mvc-4-project-templates"></a><span data-ttu-id="65df1-147">Exercício 1: Novos modelos de projeto do ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="65df1-147">Exercise 1: New ASP.NET MVC 4 Project Templates</span></span>

<span data-ttu-id="65df1-148">Neste exercício, você irá explorar os aprimoramentos nos modelos de projeto do ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="65df1-148">In this exercise, you will explore the enhancements in the ASP.NET MVC 4 Project templates.</span></span> <span data-ttu-id="65df1-149">Além do modelo de aplicativo de Internet, já está presente no MVC 3, esta versão agora inclui um modelo separado para aplicativos móveis.</span><span class="sxs-lookup"><span data-stu-id="65df1-149">In addition to the Internet Application template, already present in MVC 3, this version now includes a separate template for Mobile applications.</span></span> <span data-ttu-id="65df1-150">Primeiro, você examinará alguns recursos relevantes de cada um dos modelos.</span><span class="sxs-lookup"><span data-stu-id="65df1-150">First, you will look at some relevant features of each of the templates.</span></span> <span data-ttu-id="65df1-151">Em seguida, você trabalhará na sua página corretamente em diferentes plataformas de processamento usando a abordagem certa.</span><span class="sxs-lookup"><span data-stu-id="65df1-151">Then, you will work on rendering your page properly on the different platforms by using the right approach.</span></span>

<a id="Task_1_-_Exploring_the_Internet_Application_Template"></a>
#### <a name="task-1---exploring-the-internet-application-template"></a><span data-ttu-id="65df1-152">Tarefa 1: Explorando o modelo de aplicativo de Internet</span><span class="sxs-lookup"><span data-stu-id="65df1-152">Task 1 - Exploring the Internet Application Template</span></span>

1. <span data-ttu-id="65df1-153">Abra **Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="65df1-153">Open **Visual Studio**.</span></span>
2. <span data-ttu-id="65df1-154">Selecione o **arquivo | Novo | Projeto** comando de menu.</span><span class="sxs-lookup"><span data-stu-id="65df1-154">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="65df1-155">No **novo projeto** caixa de diálogo, selecione o **Visual c# | Web** modelo no painel esquerdo da árvore e escolha **aplicativo Web do ASP.NET MVC 4.**</span><span class="sxs-lookup"><span data-stu-id="65df1-155">In the **New Project** dialog, select the **Visual C# | Web** template on the left pane tree, and choose **ASP.NET MVC 4 Web Application.**</span></span> <span data-ttu-id="65df1-156">Nomeie o projeto **Galeriadefotos**, selecione um local (ou deixe o padrão) e clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="65df1-156">Name the project **PhotoGallery**, select a location (or leave the default) and click **OK**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="65df1-157">Posteriormente, você personalizará a solução Galeriadefotos ASP.NET MVC 4 que você está criando agora.</span><span class="sxs-lookup"><span data-stu-id="65df1-157">You will later customize the PhotoGallery ASP.NET MVC 4 solution you are now creating.</span></span>

    <span data-ttu-id="65df1-158">![Criar um novo projeto](whats-new-in-aspnet-mvc-4/_static/image1.png "criar um novo projeto")</span><span class="sxs-lookup"><span data-stu-id="65df1-158">![Creating a new project](whats-new-in-aspnet-mvc-4/_static/image1.png "Creating a new project")</span></span>

    <span data-ttu-id="65df1-159">*Criar um novo projeto*</span><span class="sxs-lookup"><span data-stu-id="65df1-159">*Creating a new project*</span></span>
3. <span data-ttu-id="65df1-160">No **novo projeto ASP.NET MVC 4** caixa de diálogo, selecione o **aplicativo de Internet** modelo de projeto e clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="65df1-160">In the **New ASP.NET MVC 4 Project** dialog, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="65df1-161">Verifique se que você selecionou Razor como o mecanismo de exibição.</span><span class="sxs-lookup"><span data-stu-id="65df1-161">Make sure you have selected Razor as the view engine.</span></span>

    <span data-ttu-id="65df1-162">![Criar um novo aplicativo ASP.NET MVC 4 Internet](whats-new-in-aspnet-mvc-4/_static/image2.png "criar um novo aplicativo ASP.NET MVC 4 da Internet")</span><span class="sxs-lookup"><span data-stu-id="65df1-162">![Creating a new ASP.NET MVC 4 Internet Application](whats-new-in-aspnet-mvc-4/_static/image2.png "Creating a new ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="65df1-163">*Criar um novo aplicativo ASP.NET MVC 4 da Internet*</span><span class="sxs-lookup"><span data-stu-id="65df1-163">*Creating a new ASP.NET MVC 4 Internet Application*</span></span>

    > [!NOTE]
    > <span data-ttu-id="65df1-164">A sintaxe do Razor foi introduzida no ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="65df1-164">Razor syntax has been introduced in ASP.NET MVC 3.</span></span> <span data-ttu-id="65df1-165">Seu objetivo é minimizar o número de caracteres e pressionamentos de tecla necessários em um arquivo, permitindo um rápido e fluido fluxo de trabalho de codificação.</span><span class="sxs-lookup"><span data-stu-id="65df1-165">Its goal is to minimize the number of characters and keystrokes required in a file, enabling a fast and fluid coding workflow.</span></span> <span data-ttu-id="65df1-166">Aproveita Razor existente c# / VB (ou outros) habilidades de linguagem e fornece uma sintaxe de marcação de modelo que permite que um fluxo de trabalho de construção awesome HTML.</span><span class="sxs-lookup"><span data-stu-id="65df1-166">Razor leverages existing C# / VB (or other) language skills and delivers a template markup syntax that enables an awesome HTML construction workflow.</span></span>
4. <span data-ttu-id="65df1-167">Pressione **F5** para executar a solução e ver os modelos renovados.</span><span class="sxs-lookup"><span data-stu-id="65df1-167">Press **F5** to run the solution and see the renewed templates.</span></span> <span data-ttu-id="65df1-168">Você pode conferir os recursos a seguir:</span><span class="sxs-lookup"><span data-stu-id="65df1-168">You can check out the following features:</span></span>

    <span data-ttu-id="65df1-169">**Modelos de estilo moderno**</span><span class="sxs-lookup"><span data-stu-id="65df1-169">**Modern-style templates**</span></span>

    <span data-ttu-id="65df1-170">Os modelos foram renovados, fornecendo mais estilos aparência moderna.</span><span class="sxs-lookup"><span data-stu-id="65df1-170">The templates have been renewed, providing more modern-looking styles.</span></span>

    <span data-ttu-id="65df1-171">![Modelos do ASP.NET MVC 4 remodelado](whats-new-in-aspnet-mvc-4/_static/image3.png "remodelado modelos do MVC 4")</span><span class="sxs-lookup"><span data-stu-id="65df1-171">![ASP.NET MVC 4 restyled templates](whats-new-in-aspnet-mvc-4/_static/image3.png "MVC 4 restyled templates")</span></span>

    <span data-ttu-id="65df1-172">*Modelos do ASP.NET MVC 4 remodelado*</span><span class="sxs-lookup"><span data-stu-id="65df1-172">*ASP.NET MVC 4 restyled templates*</span></span>

    <span data-ttu-id="65df1-173">![Nova página de contato](whats-new-in-aspnet-mvc-4/_static/image4.png "página novo contato")</span><span class="sxs-lookup"><span data-stu-id="65df1-173">![New Contact page](whats-new-in-aspnet-mvc-4/_static/image4.png "New Contact page")</span></span>

    <span data-ttu-id="65df1-174">*Nova página de contato*</span><span class="sxs-lookup"><span data-stu-id="65df1-174">*New Contact page*</span></span>

    <span data-ttu-id="65df1-175">**Renderização adaptável**</span><span class="sxs-lookup"><span data-stu-id="65df1-175">**Adaptive Rendering**</span></span>

    <span data-ttu-id="65df1-176">Check-out de redimensionar a janela do navegador e observe como o layout de página adapta dinamicamente para o novo tamanho da janela.</span><span class="sxs-lookup"><span data-stu-id="65df1-176">Check out resizing the browser window and notice how the page layout dynamically adapts to the new window size.</span></span> <span data-ttu-id="65df1-177">Esses modelos de usam a técnica de renderização adaptável para processar corretamente em plataformas móveis e desktop sem nenhuma personalização.</span><span class="sxs-lookup"><span data-stu-id="65df1-177">These templates use the adaptive rendering technique to render properly in both desktop and mobile platforms without any customization.</span></span>

    <span data-ttu-id="65df1-178">![Modelo de projeto ASP.NET MVC 4 em tamanhos diferentes do navegador](whats-new-in-aspnet-mvc-4/_static/image5.png "modelo de projeto ASP.NET MVC 4 em tamanhos diferentes do navegador")</span><span class="sxs-lookup"><span data-stu-id="65df1-178">![ASP.NET MVC 4 project template in different browser sizes](whats-new-in-aspnet-mvc-4/_static/image5.png "ASP.NET MVC 4 project template in different browser sizes")</span></span>

    <span data-ttu-id="65df1-179">*Modelo de projeto ASP.NET MVC 4 em tamanhos diferentes do navegador*</span><span class="sxs-lookup"><span data-stu-id="65df1-179">*ASP.NET MVC 4 project template in different browser sizes*</span></span>

    <span data-ttu-id="65df1-180">**Interface de usuário mais rica com JavaScript**</span><span class="sxs-lookup"><span data-stu-id="65df1-180">**Richer UI with JavaScript**</span></span>

    <span data-ttu-id="65df1-181">Outro aprimoramento para modelos de projeto padrão é o uso de JavaScript para fornecer um JavaScript mais interativo.</span><span class="sxs-lookup"><span data-stu-id="65df1-181">Another enhancement to default project templates is the use of JavaScript to provide a more interactive JavaScript.</span></span> <span data-ttu-id="65df1-182">Os links de logon e registro usados no modelo exemplificam como usar o jQuery validações para validar os campos de entrada do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="65df1-182">The Login and Register links used in the template exemplify how to use the jQuery Validations to validate the input fields from client-side.</span></span>

    ![jQuery validação](whats-new-in-aspnet-mvc-4/_static/image6.png)

    <span data-ttu-id="65df1-184">*jQuery validação*</span><span class="sxs-lookup"><span data-stu-id="65df1-184">*jQuery Validation*</span></span>

    > [!NOTE]
    > <span data-ttu-id="65df1-185">Observe que os dois logon nas seções, a primeira seção você pode fazer logon usando uma conta de marca do site e a segunda seção que pode altenativelly logon usando outro serviço de autenticação com o google (desabilitado por padrão).</span><span class="sxs-lookup"><span data-stu-id="65df1-185">Notice the two log in sections, in the first section you can log in using a registerd account from the site and in the second section you can altenativelly log in using another authentication service like google (disabled by default).</span></span>
5. <span data-ttu-id="65df1-186">Feche o navegador para interromper o depurador e retornar ao Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="65df1-186">Close the browser to stop the debugger and return to Visual Studio.</span></span>
6. <span data-ttu-id="65df1-187">Abra o arquivo **AuthConfig.cs** localizado sob o **aplicativo\_iniciar** pasta.</span><span class="sxs-lookup"><span data-stu-id="65df1-187">Open the file **AuthConfig.cs** located under the **App\_Start** folder.</span></span>
7. <span data-ttu-id="65df1-188">Remova o comentário da última linha para registrar o cliente do Google para *OAuth* autenticação.</span><span class="sxs-lookup"><span data-stu-id="65df1-188">Remove the comment from the last line to register Google client for *OAuth* authentication.</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample1.cs)]

> [!NOTE]
> Notice you can easily enable authentication using any OpenID or OAuth service like Facebook, Twitter, Microsoft, etc.
~~~
8. <span data-ttu-id="65df1-189">Pressione **F5** para executar a solução e navegue até a página de logon.</span><span class="sxs-lookup"><span data-stu-id="65df1-189">Press **F5** to run the solution and navigate to the login page.</span></span>
9. <span data-ttu-id="65df1-190">Selecione **Google** serviço para fazer logon.</span><span class="sxs-lookup"><span data-stu-id="65df1-190">Select **Google** service to log in.</span></span>

    ![Selecionar o log no serviço](whats-new-in-aspnet-mvc-4/_static/image7.png)

    <span data-ttu-id="65df1-192">*Selecionar o log no serviço*</span><span class="sxs-lookup"><span data-stu-id="65df1-192">*Selecting the log in service*</span></span>
10. <span data-ttu-id="65df1-193">Faça logon usando a conta do Google.</span><span class="sxs-lookup"><span data-stu-id="65df1-193">Log in using your Google account.</span></span>
11. <span data-ttu-id="65df1-194">Permitir que o site (localhost) recuperar informações de conta do Google.</span><span class="sxs-lookup"><span data-stu-id="65df1-194">Allow the site (localhost) to retrieve information from Google account.</span></span>
12. <span data-ttu-id="65df1-195">Por fim, você terá que registrar no site para associar a conta do Google.</span><span class="sxs-lookup"><span data-stu-id="65df1-195">Finally, you will have to register in the site to associate the Google account.</span></span>

   ![Associe a conta do Google](whats-new-in-aspnet-mvc-4/_static/image8.png)

   <span data-ttu-id="65df1-197">*Associando a conta do Google*</span><span class="sxs-lookup"><span data-stu-id="65df1-197">*Associating your Google account*</span></span>
13. <span data-ttu-id="65df1-198">Feche o navegador para interromper o depurador e retornar ao Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="65df1-198">Close the browser to stop the debugger and return to Visual Studio.</span></span>
14. <span data-ttu-id="65df1-199">Explore agora a solução para check-out de alguns outros novos recursos introduzidos pelo ASP.NET MVC 4 no modelo de projeto.</span><span class="sxs-lookup"><span data-stu-id="65df1-199">Now explore the solution to check out some other new features introduced by ASP.NET MVC 4 in the project template.</span></span>

   <span data-ttu-id="65df1-200">![O modelo de projeto de aplicativo do ASP.NET MVC 4 Internet](whats-new-in-aspnet-mvc-4/_static/image9.png "o modelo de projeto de aplicativo de Internet do ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="65df1-200">![The ASP.NET MVC 4 Internet Application Project Template](whats-new-in-aspnet-mvc-4/_static/image9.png "The ASP.NET MVC 4 Internet Application Project Template")</span></span>

   <span data-ttu-id="65df1-201">*O modelo de projeto de aplicativo de Internet do ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="65df1-201">*The ASP.NET MVC 4 Internet Application Project Template*</span></span>

   - <span data-ttu-id="65df1-202">**HTML 5 marcação**</span><span class="sxs-lookup"><span data-stu-id="65df1-202">**HTML 5 Markup**</span></span>

       <span data-ttu-id="65df1-203">Procure exibições de modelo para descobrir a nova marcação de tema.</span><span class="sxs-lookup"><span data-stu-id="65df1-203">Browse template views to find out the new theme markup.</span></span>

       <span data-ttu-id="65df1-204">![Novo modelo, usando About.cshtml de marcação Razor e HTML5. ] (whats-new-in-aspnet-mvc-4/_static/image10.png "Novo modelo, usando About.cshtml de marcação Razor e HTML5.")</span><span class="sxs-lookup"><span data-stu-id="65df1-204">![New template, using Razor and HTML5 markup About.cshtml.](whats-new-in-aspnet-mvc-4/_static/image10.png "New template, using Razor and HTML5 markup About.cshtml.")</span></span>

       <span data-ttu-id="65df1-205">*Novo modelo, usando marcação Razor e HTML5 (About.cshtml).*</span><span class="sxs-lookup"><span data-stu-id="65df1-205">*New template, using Razor and HTML5 markup (About.cshtml).*</span></span>
   - <span data-ttu-id="65df1-206">**Bibliotecas de JavaScript atualizadas**</span><span class="sxs-lookup"><span data-stu-id="65df1-206">**Updated JavaScript libraries**</span></span>

       <span data-ttu-id="65df1-207">O modelo de padrão do ASP.NET MVC 4 agora inclui KnockoutJS, uma estrutura de JavaScript MVVM que permite que você crie avançados e aplicativos web altamente responsivo usando JavaScript e HTML.</span><span class="sxs-lookup"><span data-stu-id="65df1-207">The ASP.NET MVC 4 default template now includes KnockoutJS, a JavaScript MVVM framework that lets you create rich and highly responsive web applications using JavaScript and HTML.</span></span> <span data-ttu-id="65df1-208">Como em MVC3, jQuery e jQuery bibliotecas de interface do usuário também estão incluídas no ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="65df1-208">Like in MVC3, jQuery and jQuery UI libraries are also included in ASP.NET MVC 4.</span></span>

     > [!NOTE]
     > <span data-ttu-id="65df1-209">Você pode obter mais informações sobre a biblioteca KnockOutJS neste link: [ [ http://learn.knockoutjs.com/ ](http://learn.knockoutjs.com/) ](http://learn.knockoutjs.com/).</span><span class="sxs-lookup"><span data-stu-id="65df1-209">You can get more information about KnockOutJS library in this link: [[http://learn.knockoutjs.com/](http://learn.knockoutjs.com/)](http://learn.knockoutjs.com/).</span></span> <span data-ttu-id="65df1-210">Além disso, você pode aprender sobre jQuery e jQuery UI em [ [ http://docs.jquery.com/ ](http://docs.jquery.com/) ](http://docs.jquery.com/).</span><span class="sxs-lookup"><span data-stu-id="65df1-210">Additionally, you can learn about jQuery and jQuery UI in [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span></span>

<a id="Task_2_-_Exploring_the_Mobile_Application_Template"></a>
#### <a name="task-2---exploring-the-mobile-application-template"></a><span data-ttu-id="65df1-211">Tarefa 2: explorar o modelo de aplicativo móvel</span><span class="sxs-lookup"><span data-stu-id="65df1-211">Task 2 - Exploring the Mobile Application Template</span></span>

<span data-ttu-id="65df1-212">ASP.NET MVC 4 facilita o desenvolvimento de sites da Web para dispositivos móveis e navegadores do tablet.</span><span class="sxs-lookup"><span data-stu-id="65df1-212">ASP.NET MVC 4 facilitates the development of websites for mobile and tablet browsers.</span></span> <span data-ttu-id="65df1-213">Esse modelo tem a mesma estrutura de aplicativo como o modelo de aplicativo de Internet (Observe que o código do controlador é praticamente idêntico), mas seu estilo foi modificado para ser processada corretamente em dispositivos móveis baseados em toque.</span><span class="sxs-lookup"><span data-stu-id="65df1-213">This template has the same application structure as the Internet Application template (notice that the controller code is practically identical), but its style was modified to render properly in touch-based mobile devices.</span></span>

1. <span data-ttu-id="65df1-214">Selecione o **arquivo | Novo | Projeto** comando de menu.</span><span class="sxs-lookup"><span data-stu-id="65df1-214">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="65df1-215">No **novo projeto** caixa de diálogo, selecione o **Visual c# | Web** modelo no painel esquerdo da árvore e escolha o **aplicativo Web do ASP.NET MVC 4.**</span><span class="sxs-lookup"><span data-stu-id="65df1-215">In the **New Project** dialog, select the **Visual C# | Web** template on the left pane tree, and choose the **ASP.NET MVC 4 Web Application.**</span></span> <span data-ttu-id="65df1-216">Nomeie o projeto **PhotoGallery.Mobile**, selecione um local (ou deixe o padrão), selecione &quot;adicionar à solução&quot; e clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="65df1-216">Name the project **PhotoGallery.Mobile**, select a location (or leave the default), select &quot;Add to solution&quot; and click **OK**.</span></span>
2. <span data-ttu-id="65df1-217">No **novo projeto ASP.NET MVC 4** caixa de diálogo, selecione o **Mobile Application** modelo de projeto e clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="65df1-217">In the **New ASP.NET MVC 4 Project** dialog, select the **Mobile Application** project template and click **OK**.</span></span> <span data-ttu-id="65df1-218">Verifique se que você selecionou Razor como o mecanismo de exibição.</span><span class="sxs-lookup"><span data-stu-id="65df1-218">Make sure you have selected Razor as the view engine.</span></span>

    <span data-ttu-id="65df1-219">![Criar um novo aplicativo ASP.NET MVC 4 Mobile](whats-new-in-aspnet-mvc-4/_static/image11.png "criar um novo aplicativo ASP.NET MVC 4 Mobile")</span><span class="sxs-lookup"><span data-stu-id="65df1-219">![Creating a new ASP.NET MVC 4 Mobile Application](whats-new-in-aspnet-mvc-4/_static/image11.png "Creating a new ASP.NET MVC 4 Mobile Application")</span></span>

    <span data-ttu-id="65df1-220">*Criar um novo aplicativo ASP.NET MVC 4 Mobile*</span><span class="sxs-lookup"><span data-stu-id="65df1-220">*Creating a new ASP.NET MVC 4 Mobile Application*</span></span>
3. <span data-ttu-id="65df1-221">Agora é possível explorar a solução e check-out de alguns dos novos recursos introduzidos pelo modelo de solução do ASP.NET MVC 4 para dispositivos móveis:</span><span class="sxs-lookup"><span data-stu-id="65df1-221">Now you are able to explore the solution and check out some of the new features introduced by the ASP.NET MVC 4 solution template for mobile:</span></span>

    - <span data-ttu-id="65df1-222">**jQuery Mobile biblioteca**</span><span class="sxs-lookup"><span data-stu-id="65df1-222">**jQuery Mobile Library**</span></span>

        <span data-ttu-id="65df1-223">O modelo de projeto de aplicativo móvel inclui a biblioteca jQuery Mobile, que é uma biblioteca de software livre para compatibilidade com navegadores móveis.</span><span class="sxs-lookup"><span data-stu-id="65df1-223">The Mobile Application project template includes the jQuery Mobile library, which is an open source library for mobile browser compatibility.</span></span> <span data-ttu-id="65df1-224">o jQuery Mobile aplica aprimoramento progressivo para navegadores de dispositivos móveis que oferecem suporte a CSS e JavaScript.</span><span class="sxs-lookup"><span data-stu-id="65df1-224">jQuery Mobile applies progressive enhancement to mobile browsers that support CSS and JavaScript.</span></span> <span data-ttu-id="65df1-225">Aprimoramento progressivo permite que todos os navegadores exibir o conteúdo básico de uma página da web, enquanto ele só permite que os navegadores mais eficientes exibir o conteúdo.</span><span class="sxs-lookup"><span data-stu-id="65df1-225">Progressive enhancement enables all browsers to display the basic content of a web page, while it only enables the most powerful browsers to display the rich content.</span></span> <span data-ttu-id="65df1-226">Os arquivos JavaScript e CSS, incluídos no jQuery Mobile estilo, ajudam navegadores móveis para ajustar o conteúdo na tela sem fazer qualquer alteração na marcação da página.</span><span class="sxs-lookup"><span data-stu-id="65df1-226">The JavaScript and CSS files, included in the jQuery Mobile style, help mobile browsers to fit the content in the screen without making any change in the page markup.</span></span>

        ![jQuery-mobile-library-included-in-the-template](whats-new-in-aspnet-mvc-4/_static/image12.png)

        <span data-ttu-id="65df1-228">*biblioteca de móveis jQuery incluída no modelo*</span><span class="sxs-lookup"><span data-stu-id="65df1-228">*jQuery mobile library included in the template*</span></span>
    - <span data-ttu-id="65df1-229">**Marcação de HTML5 com base**</span><span class="sxs-lookup"><span data-stu-id="65df1-229">**HTML5 based markup**</span></span>

        ![Mobile-application-template-using-HTML5-markup](whats-new-in-aspnet-mvc-4/_static/image13.png)

        <span data-ttu-id="65df1-231">*Modelo de aplicativo móvel usando HTML5 marcação, (cshtml e cshtml)*</span><span class="sxs-lookup"><span data-stu-id="65df1-231">*Mobile application template using HTML5 markup, (Login.cshtml and index.cshtml)*</span></span>
4. <span data-ttu-id="65df1-232">Pressione **F5** para executar a solução.</span><span class="sxs-lookup"><span data-stu-id="65df1-232">Press **F5** to run the solution.</span></span>
5. <span data-ttu-id="65df1-233">Abra o **Windows Phone 7 emulador**.</span><span class="sxs-lookup"><span data-stu-id="65df1-233">Open the **Windows Phone 7 Emulator**.</span></span>
6. <span data-ttu-id="65df1-234">Na tela de início do telefone, abra o Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="65df1-234">In the phone start screen, open Internet Explorer.</span></span> <span data-ttu-id="65df1-235">Confira a URL onde o aplicativo de área de trabalho foi iniciado e navegue para a URL do telefone (por exemplo, `http://localhost:[PortNumber]/`).</span><span class="sxs-lookup"><span data-stu-id="65df1-235">Check out the URL where the desktop application started and browse to that URL from the phone (e.g. `http://localhost:[PortNumber]/`).</span></span>
7. <span data-ttu-id="65df1-236">Agora é possível inserir a página de logon ou check-out de sobre a página.</span><span class="sxs-lookup"><span data-stu-id="65df1-236">Now you are able to enter the login page or check out the about page.</span></span> <span data-ttu-id="65df1-237">Observe que o estilo do site se baseia o novo aplicativo do Metro para dispositivos móveis.</span><span class="sxs-lookup"><span data-stu-id="65df1-237">Notice that the style of the website is based on the new Metro app for mobile.</span></span> <span data-ttu-id="65df1-238">O modelo de projeto ASP.NET MVC 4 é exibido corretamente em dispositivos móveis, tornando-se de que todos os elementos da página são visíveis e habilitados.</span><span class="sxs-lookup"><span data-stu-id="65df1-238">The ASP.NET MVC 4 project template is correctly displayed on mobile devices, making sure all the elements of the page are visible and enabled.</span></span> <span data-ttu-id="65df1-239">Observe que os links no cabeçalho grandes o suficiente para ser clicado ou tocado.</span><span class="sxs-lookup"><span data-stu-id="65df1-239">Notice that the links on the header are big enough to be clicked or tapped.</span></span>

    <span data-ttu-id="65df1-240">![Páginas de modelo em um dispositivo móvel do projeto](whats-new-in-aspnet-mvc-4/_static/image14.png "projeto páginas de modelo em um dispositivo móvel")</span><span class="sxs-lookup"><span data-stu-id="65df1-240">![Project template pages in a mobile device](whats-new-in-aspnet-mvc-4/_static/image14.png "Project template pages in a mobile device")</span></span>

    <span data-ttu-id="65df1-241">*Páginas de modelo de projeto em um dispositivo móvel*</span><span class="sxs-lookup"><span data-stu-id="65df1-241">*Project template pages in a mobile device*</span></span>
8. <span data-ttu-id="65df1-242">O novo modelo também usa o **marca meta Viewport**.</span><span class="sxs-lookup"><span data-stu-id="65df1-242">The new template also uses the **Viewport meta tag**.</span></span> <span data-ttu-id="65df1-243">A maioria dos navegadores definem uma largura de uma janela do navegador virtual ou &quot;visor&quot;, que é maior do que a largura real do dispositivo móvel.</span><span class="sxs-lookup"><span data-stu-id="65df1-243">Most mobile browsers define a width for a virtual browser window or &quot;viewport&quot;, which is larger than the actual width of the mobile device.</span></span> <span data-ttu-id="65df1-244">Isso permite que navegadores móveis exibir a página da web inteira dentro a exibição virtual.</span><span class="sxs-lookup"><span data-stu-id="65df1-244">This enables mobile browsers to display the entire web page inside the virtual display.</span></span> <span data-ttu-id="65df1-245">O **marca meta Viewport** permite que os desenvolvedores da web definir a largura, altura e escala da área do navegador em dispositivos móveis **.**</span><span class="sxs-lookup"><span data-stu-id="65df1-245">The **Viewport meta tag** allows web developers to set the width, height and scale of the browser area on mobile devices **.**</span></span> <span data-ttu-id="65df1-246">O modelo do ASP.NET MVC 4 para aplicativos móveis define o visor para a largura do dispositivo (&quot;largura = dispositivo largura&quot;) no modelo de layout (*exibições \ compartilhadas\_cshtml*), de modo que todos os a páginas terão seu visor definida como a largura de tela do dispositivo.</span><span class="sxs-lookup"><span data-stu-id="65df1-246">The ASP.NET MVC 4 template for Mobile Applications sets the viewport to the device width (&quot;width=device-width&quot;) in the layout template (*Views\Shared\_Layout.cshtml*), so that all the pages will have their viewport set to the device screen width.</span></span> <span data-ttu-id="65df1-247">Observe que a marca meta Viewport não alterará o modo de exibição de navegador padrão.</span><span class="sxs-lookup"><span data-stu-id="65df1-247">Notice that the Viewport meta tag will not change the default browser view.</span></span>
9. <span data-ttu-id="65df1-248">Abra  **\_cshtml**, localizado no **exibições | Compartilhado** pasta e comente a marca meta do visor.</span><span class="sxs-lookup"><span data-stu-id="65df1-248">Open **\_Layout.cshtml**, located in the **Views | Shared** folder, and comment the Viewport meta tag.</span></span> <span data-ttu-id="65df1-249">Executar o aplicativo, se não estiver já aberto e conferir as diferenças.</span><span class="sxs-lookup"><span data-stu-id="65df1-249">Run the application, if not already opened, and check out the differences.</span></span>


~~~
[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample2.cshtml)]

![The site after commenting the viewport meta tag](whats-new-in-aspnet-mvc-4/_static/image15.png "The site after commenting the viewport meta tag")

*The site after commenting the viewport meta tag*
~~~
10. <span data-ttu-id="65df1-250">No Visual Studio, pressione **SHIFT** + **F5** para parar a depuração do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="65df1-250">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>
11. <span data-ttu-id="65df1-251">Remova a marca meta viewport.</span><span class="sxs-lookup"><span data-stu-id="65df1-251">Uncomment the viewport meta tag.</span></span>


~~~
[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample3.cshtml)]
~~~

<a id="Task_3_-_Using_Adaptive_Rendering"></a>
#### <a name="task-3---using-adaptive-rendering"></a><span data-ttu-id="65df1-252">Tarefa 3: usar a renderização adaptável</span><span class="sxs-lookup"><span data-stu-id="65df1-252">Task 3 - Using Adaptive Rendering</span></span>

<span data-ttu-id="65df1-253">Nesta tarefa, você aprenderá a outro método para renderizar uma página da Web corretamente em dispositivos móveis e navegadores da Web ao mesmo tempo sem nenhuma personalização.</span><span class="sxs-lookup"><span data-stu-id="65df1-253">In this task, you will learn another method to render a Web page correctly on mobile devices and Web browsers at the same time without any customization.</span></span> <span data-ttu-id="65df1-254">Você já usou marca meta Viewport com uma finalidade semelhante.</span><span class="sxs-lookup"><span data-stu-id="65df1-254">You have already used Viewport meta tag with a similar purpose.</span></span> <span data-ttu-id="65df1-255">Agora você atenderá outro método avançado: *processamento adaptável*.</span><span class="sxs-lookup"><span data-stu-id="65df1-255">Now you will meet another powerful method: *adaptive rendering*.</span></span>

<span data-ttu-id="65df1-256">Renderização adaptável é uma técnica que usa **consultas de mídia CSS3** para personalizar o estilo aplicado a uma página.</span><span class="sxs-lookup"><span data-stu-id="65df1-256">Adaptive rendering is a technique that uses **CSS3 media queries** to customize the style applied to a page.</span></span> <span data-ttu-id="65df1-257">As consultas de mídia definem condições dentro de uma folha de estilos, agrupando os estilos CSS em uma determinada condição.</span><span class="sxs-lookup"><span data-stu-id="65df1-257">Media queries define conditions inside a style sheet, grouping CSS styles under a certain condition.</span></span> <span data-ttu-id="65df1-258">Somente quando a condição for true, o estilo é aplicado aos objetos declarados.</span><span class="sxs-lookup"><span data-stu-id="65df1-258">Only when the condition is true, the style is applied to the declared objects.</span></span>

<span data-ttu-id="65df1-259">A flexibilidade fornecida pela técnica processamento adaptável permite que qualquer personalização para exibir o site em diferentes dispositivos.</span><span class="sxs-lookup"><span data-stu-id="65df1-259">The flexibility provided by the adaptive rendering technique enables any customization for displaying the site on different devices.</span></span> <span data-ttu-id="65df1-260">Você pode definir estilos quantos desejar em uma folha de estilos única sem escrever código de lógica para escolher o estilo.</span><span class="sxs-lookup"><span data-stu-id="65df1-260">You can define as many styles as you want on a single style sheet without writing logic code to choose the style.</span></span> <span data-ttu-id="65df1-261">Portanto, é uma maneira muito interessante de adaptar estilos de página, pois reduz a quantidade de código duplicado e lógica para fins de processamento.</span><span class="sxs-lookup"><span data-stu-id="65df1-261">Therefore, it is a very neat way of adapting page styles, as it reduces the amount of duplicated code and logic for rendering purposes.</span></span> <span data-ttu-id="65df1-262">Por outro lado, o consumo de largura de banda aumentaria, como o tamanho dos arquivos CSS pode aumentar um pouco.</span><span class="sxs-lookup"><span data-stu-id="65df1-262">On the other hand, bandwidth consumption would increase, as the size of your CSS files could grow marginally.</span></span>

<span data-ttu-id="65df1-263">Usando a técnica de renderização adaptável, o site será **exibido corretamente, independentemente do navegador.**</span><span class="sxs-lookup"><span data-stu-id="65df1-263">By using the adaptive rendering technique, your site will be **displayed properly, regardless of the browser.**</span></span> <span data-ttu-id="65df1-264">No entanto, você deve considerar se a largura de banda extra de carga é uma preocupação.</span><span class="sxs-lookup"><span data-stu-id="65df1-264">However, you should consider if the bandwidth extra load is a concern.</span></span>

> [!NOTE]
> <span data-ttu-id="65df1-265">O formato básico de uma consulta de mídia é: @media \[escopo: todos os | portáteis | imprimir | projeção | tela\] ([: valor da propriedade] e... [: valor da propriedade])</span><span class="sxs-lookup"><span data-stu-id="65df1-265">The basic format of a media query is: @media \[Scope: all | handheld | print | projection | screen\] ([property:value] and ... [property:value])</span></span>


<span data-ttu-id="65df1-266">Exemplos de consultas de mídia: &gt;  <strong>@media todas as e (largura máxima: 1000px) e (largura mínima: 700px) {}:</strong> para todas as resoluções entre 700px e 1000px.</span><span class="sxs-lookup"><span data-stu-id="65df1-266">Examples of media queries: &gt;<strong>@media all and (max-width: 1000px) and (min-width: 700px) {}:</strong> For all the resolutions between 700px and 1000px.</span></span>

> <span data-ttu-id="65df1-267"><strong>@media tela e (largura mínima: 400px) e (largura máxima: 700px) {...}:</strong> somente para telas.</span><span class="sxs-lookup"><span data-stu-id="65df1-267"><strong>@media screen and (min-width: 400px) and (max-width: 700px) { ... }:</strong> Only for screens.</span></span> <span data-ttu-id="65df1-268">A resolução deve ficar entre 400 e 700px.</span><span class="sxs-lookup"><span data-stu-id="65df1-268">The resolution must be between 400 and 700px.</span></span>
> 
> <span data-ttu-id="65df1-269"><strong>@media portáteis e (largura mínima: 20em), tela e (largura mínima: 20em) {...}:</strong> para telas e portáteis (móvel e dispositivos).</span><span class="sxs-lookup"><span data-stu-id="65df1-269"><strong>@media handheld and (min-width: 20em), screen and (min-width: 20em) { ... }:</strong> For handhelds(mobile and devices) and screens.</span></span> <span data-ttu-id="65df1-270">A largura mínima deve ser maior que 20em.</span><span class="sxs-lookup"><span data-stu-id="65df1-270">The minimum width must be greater than 20em.</span></span>
> 
> <span data-ttu-id="65df1-271">Você pode encontrar mais informações sobre isso no [site do W3C](http://www.w3.org/TR/css3-mediaqueries/).</span><span class="sxs-lookup"><span data-stu-id="65df1-271">You can find more information about this on the [W3C site](http://www.w3.org/TR/css3-mediaqueries/).</span></span>


<span data-ttu-id="65df1-272">Agora você explorará como funciona a renderização adaptável, melhorando a legibilidade do ASP.NET MVC 4 padrão modelo de site.</span><span class="sxs-lookup"><span data-stu-id="65df1-272">You will now explore how the adaptive rendering works, improving the readability of the ASP.NET MVC 4 default website template.</span></span>

1. <span data-ttu-id="65df1-273">Abra o **PhotoGallery.sln** solução que você criou na tarefa 1 e selecione o **Galeriadefotos** projeto.</span><span class="sxs-lookup"><span data-stu-id="65df1-273">Open the **PhotoGallery.sln** solution you have created at Task 1 and select the **PhotoGallery** project.</span></span> <span data-ttu-id="65df1-274">Pressione **F5** para executar a solução.</span><span class="sxs-lookup"><span data-stu-id="65df1-274">Press **F5** to run the solution.</span></span>
2. <span data-ttu-id="65df1-275">Redimensione a largura do navegador, configuração do windows para a metade ou para menos de um quarto de seu tamanho original.</span><span class="sxs-lookup"><span data-stu-id="65df1-275">Resize the browser's width, setting the windows to half or to less than a quarter of its original size.</span></span> <span data-ttu-id="65df1-276">Observe o que acontece com os itens no cabeçalho: alguns elementos não aparecerá na área visível do cabeçalho.</span><span class="sxs-lookup"><span data-stu-id="65df1-276">Notice what happens with the items in the header: Some elements will not appear in the visible area of the header.</span></span>
3. <span data-ttu-id="65df1-277">Abra <strong>Site.css</strong> arquivo no Visual Studio Gerenciador de soluções, localizado em <strong>conteúdo</strong> pasta do projeto.</span><span class="sxs-lookup"><span data-stu-id="65df1-277">Open <strong>Site.css</strong> file from the Visual Studio Solution explorer, located in <strong>Content</strong> project folder.</span></span> <span data-ttu-id="65df1-278">Pressione <strong>CTRL + F</strong> para abrir a pesquisa integrada do Visual Studio e gravar <strong>@media</strong> para localizar o <strong>consulta de mídia do CSS</strong>.</span><span class="sxs-lookup"><span data-stu-id="65df1-278">Press <strong>CTRL + F</strong> to open Visual Studio integrated search, and write <strong>@media</strong> to locate the <strong>CSS media query</strong>.</span></span>

    <span data-ttu-id="65df1-279">A condição de consulta de mídia definida neste modelo funciona desta forma: quando o tamanho da janela do navegador é abaixo **850 px**, as regras de CSS aplicadas são aquelas definidas neste bloco de mídia.</span><span class="sxs-lookup"><span data-stu-id="65df1-279">The media query condition defined in this template works in this way: When the browser's window size is below **850 px**, the CSS rules applied are the ones defined inside this media block.</span></span>

    <span data-ttu-id="65df1-280">![Localizando a consulta de mídia](whats-new-in-aspnet-mvc-4/_static/image16.png "Localizando a consulta de mídia")</span><span class="sxs-lookup"><span data-stu-id="65df1-280">![Locating the media query](whats-new-in-aspnet-mvc-4/_static/image16.png "Locating the media query")</span></span>

    <span data-ttu-id="65df1-281">*Localizando a consulta de mídia*</span><span class="sxs-lookup"><span data-stu-id="65df1-281">*Locating the media query*</span></span>
4. <span data-ttu-id="65df1-282">Substitua o valor do atributo de largura máxima definido 850 px com **10px**, para desativar o processamento adaptável e pressione **CTRL + S** para salvar as alterações.</span><span class="sxs-lookup"><span data-stu-id="65df1-282">Replace the max-width attribute value set in 850 px with **10px**, in order to disable the adaptive rendering, and press **CTRL + S** to save the changes.</span></span> <span data-ttu-id="65df1-283">Voltar para o navegador e pressione **CTRL + F5** para atualizar a página com as alterações feitas por você.</span><span class="sxs-lookup"><span data-stu-id="65df1-283">Return to the browser and press **CTRL + F5** to refresh the page with the changes you have made.</span></span> <span data-ttu-id="65df1-284">Observe as diferenças entre as duas páginas ao ajustar a largura da janela.</span><span class="sxs-lookup"><span data-stu-id="65df1-284">Notice the differences in both pages when adjusting the width of the window.</span></span>

    <span data-ttu-id="65df1-285">![À esquerda, a página está aplicando o @media estilo, no canto direito, o estilo for omitido](whats-new-in-aspnet-mvc-4/_static/image17.png "à esquerda, a página está aplicando o @media estilo, no canto direito, o estilo for omitido")</span><span class="sxs-lookup"><span data-stu-id="65df1-285">![In the left, the page is applying the @media style, in the right, the style is omitted](whats-new-in-aspnet-mvc-4/_static/image17.png "In the left, the page is applying the @media style, in the right, the style is omitted")</span></span>

    <span data-ttu-id="65df1-286"><em>À esquerda, a página está aplicando o @media estilo, no canto direito, o estilo for omitido</em></span><span class="sxs-lookup"><span data-stu-id="65df1-286"><em>In the left, the page is applying the @media style, in the right, the style is omitted</em></span></span>

    <span data-ttu-id="65df1-287">Agora, vamos conferir o que acontece em dispositivos móveis:</span><span class="sxs-lookup"><span data-stu-id="65df1-287">Now, let's check out what happens on mobile devices:</span></span>

    <span data-ttu-id="65df1-288">![À esquerda, a página está aplicando o @media estilo, no canto direito, o estilo for omitido](whats-new-in-aspnet-mvc-4/_static/image18.png "à esquerda, a página está aplicando o @media estilo, no canto direito, o estilo for omitido")</span><span class="sxs-lookup"><span data-stu-id="65df1-288">![In the left, the page is applying the @media style, in the right, the style is omitted](whats-new-in-aspnet-mvc-4/_static/image18.png "In the left, the page is applying the @media style, in the right, the style is omitted")</span></span>

    <span data-ttu-id="65df1-289"><em>À esquerda, a página está aplicando o @media estilo, no canto direito, o estilo for omitido</em></span><span class="sxs-lookup"><span data-stu-id="65df1-289"><em>In the left, the page is applying the @media style, in the right, the style is omitted</em></span></span>

    <span data-ttu-id="65df1-290">Embora você observará que as alterações quando a página é processada em um navegador da Web não são muito significativas, ao usar um dispositivo móvel as diferenças se tornam mais óbvias.</span><span class="sxs-lookup"><span data-stu-id="65df1-290">Although you will notice that the changes when the page is rendered in a Web browser are not very significant, when using a mobile device the differences become more obvious.</span></span> <span data-ttu-id="65df1-291">No lado esquerdo da imagem, podemos ver que o estilo personalizado aprimorada a legibilidade.</span><span class="sxs-lookup"><span data-stu-id="65df1-291">On the left side of the image, we can see that the custom style improved the readability.</span></span>

    <span data-ttu-id="65df1-292">Renderização adaptável pode ser usada em muitos mais cenários, tornando mais fácil aplicar o estilo para um site da Web e resolver problemas comuns de estilo com uma abordagem interessante condicional.</span><span class="sxs-lookup"><span data-stu-id="65df1-292">Adaptive rendering can be used in many more scenarios, making it easier to apply conditional styling to a Web site and solving common style issues with a neat approach.</span></span>

    <span data-ttu-id="65df1-293">A marca meta do visor e consultas de mídia CSS não são específicas para ASP.NET MVC 4, portanto você pode tirar proveito desses recursos em um aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="65df1-293">The Viewport meta tag and CSS media queries are not specific to ASP.NET MVC 4, so you can take advantage of these features in any web application.</span></span>
5. <span data-ttu-id="65df1-294">No Visual Studio, pressione **SHIFT** + **F5** para parar a depuração do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="65df1-294">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_the_Photo_Gallery_Web_Application"></a>
### <a name="exercise-2-creating-the-photo-gallery-web-application"></a><span data-ttu-id="65df1-295">Exercício 2: Criar o aplicativo Web de galeria de fotos</span><span class="sxs-lookup"><span data-stu-id="65df1-295">Exercise 2: Creating the Photo Gallery Web Application</span></span>

<span data-ttu-id="65df1-296">Neste exercício, você trabalhará em um aplicativo da Galeria de fotos para exibir fotos.</span><span class="sxs-lookup"><span data-stu-id="65df1-296">In this exercise, you will work on a Photo Gallery application to display photos.</span></span> <span data-ttu-id="65df1-297">Inicie com o modelo de projeto ASP.NET MVC 4 e, em seguida, você adicionará um recurso para recuperar fotos de um serviço e exibi-las na home page.</span><span class="sxs-lookup"><span data-stu-id="65df1-297">You will start with the ASP.NET MVC 4 project template, and then you will add a feature to retrieve photos from a service and display them in the home page.</span></span>

<span data-ttu-id="65df1-298">O exercício a seguir, você irá atualizar esta solução para aperfeiçoar a maneira como ele é exibido em dispositivos móveis.</span><span class="sxs-lookup"><span data-stu-id="65df1-298">In the following exercise, you will update this solution to enhance the way it is displayed on mobile devices.</span></span>

<a id="Task_1_-_Creating_a_Mock_Photo_Service"></a>
#### <a name="task-1---creating-a-mock-photo-service"></a><span data-ttu-id="65df1-299">Tarefa 1: Criando um serviço de simulação de fotos</span><span class="sxs-lookup"><span data-stu-id="65df1-299">Task 1 - Creating a Mock Photo Service</span></span>

<span data-ttu-id="65df1-300">Nesta tarefa, você criará uma simulação do serviço para recuperar o conteúdo que será exibido na Galeria de fotos.</span><span class="sxs-lookup"><span data-stu-id="65df1-300">In this task, you will create a mock of the photo service to retrieve the content that will be displayed in the gallery.</span></span> <span data-ttu-id="65df1-301">Para fazer isso, você adicionará um novo controlador que retornará apenas um arquivo JSON com os dados de cada foto.</span><span class="sxs-lookup"><span data-stu-id="65df1-301">To do this, you will add a new controller that will simply return a JSON file with the data of each photo.</span></span>

1. <span data-ttu-id="65df1-302">Abra **Visual Studio** se ainda não estiver aberto.</span><span class="sxs-lookup"><span data-stu-id="65df1-302">Open **Visual Studio** if not already opened.</span></span>
2. <span data-ttu-id="65df1-303">Selecione o **arquivo | Novo | Projeto** comando de menu.</span><span class="sxs-lookup"><span data-stu-id="65df1-303">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="65df1-304">No **novo projeto** caixa de diálogo, selecione o **Visual c# | Web** modelo no painel esquerdo da árvore e escolha **aplicativo Web do ASP.NET MVC 4.**</span><span class="sxs-lookup"><span data-stu-id="65df1-304">In the **New Project** dialog, select the **Visual C# | Web** template on the left pane tree, and choose **ASP.NET MVC 4 Web Application.**</span></span> <span data-ttu-id="65df1-305">Nomeie o projeto **Galeriadefotos**, selecione um local (ou deixe o padrão) e clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="65df1-305">Name the project **PhotoGallery**, select a location (or leave the default) and click **OK**.</span></span> <span data-ttu-id="65df1-306">Como alternativa, você pode continuar trabalhando de seu existente ASP.NET MVC 4 **aplicativo de Internet** solução de **Exercício 1** e passar para a próxima etapa.</span><span class="sxs-lookup"><span data-stu-id="65df1-306">Alternatively, you can continue working from your existing ASP.NET MVC 4 **Internet Application** solution from **Exercise 1** and skip the next step.</span></span>
3. <span data-ttu-id="65df1-307">No **novo projeto ASP.NET MVC 4** caixa de diálogo, selecione o **aplicativo de Internet** modelo de projeto e clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="65df1-307">In the **New ASP.NET MVC 4 Project** dialog box, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="65df1-308">Certifique-se de que ter selecionado como o mecanismo de exibição do Razor.</span><span class="sxs-lookup"><span data-stu-id="65df1-308">Make sure you have Razor selected as the View Engine.</span></span>
4. <span data-ttu-id="65df1-309">No **Solution Explorer**, com o botão direito do **aplicativo\_dados** pasta do seu projeto e selecione **adicionar | Item existente**.</span><span class="sxs-lookup"><span data-stu-id="65df1-309">In the **Solution Explorer**, right-click the **App\_Data** folder of your project, and select **Add | Existing Item**.</span></span> <span data-ttu-id="65df1-310">Navegue até o **Source\Assets\App\_dados** pasta deste laboratório e adicione o **Photos.json** arquivo.</span><span class="sxs-lookup"><span data-stu-id="65df1-310">Browse to the **Source\Assets\App\_Data** folder of this lab and add the **Photos.json** file.</span></span>
5. <span data-ttu-id="65df1-311">Criar um novo controlador com o nome **PhotoController**.</span><span class="sxs-lookup"><span data-stu-id="65df1-311">Create a new controller with the name **PhotoController**.</span></span> <span data-ttu-id="65df1-312">Para fazer isso, clique duas vezes no **controladores** pasta, vá para **adicionar** e selecione **controlador.**</span><span class="sxs-lookup"><span data-stu-id="65df1-312">To do this, right-click on the **Controllers** folder, go to **Add** and select **Controller.**</span></span> <span data-ttu-id="65df1-313">Complete o nome do controlador, deixe o **controlador MVC vazio** modelo e clique em **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="65df1-313">Complete the controller name, leave the **Empty MVC controller** template and click **Add**.</span></span>

    <span data-ttu-id="65df1-314">![Adicionando o PhotoController](whats-new-in-aspnet-mvc-4/_static/image19.png "adicionando o PhotoController")</span><span class="sxs-lookup"><span data-stu-id="65df1-314">![Adding the PhotoController](whats-new-in-aspnet-mvc-4/_static/image19.png "Adding the PhotoController")</span></span>

    <span data-ttu-id="65df1-315">*Adicionando o PhotoController*</span><span class="sxs-lookup"><span data-stu-id="65df1-315">*Adding the PhotoController*</span></span>
6. <span data-ttu-id="65df1-316">Substitua o **índice** método com o seguinte **galeria** ação e retornar o conteúdo do arquivo JSON que você recentemente adicionou ao projeto.</span><span class="sxs-lookup"><span data-stu-id="65df1-316">Replace the **Index** method with the following **Gallery** action, and return the content from the JSON file you have recently added to the project.</span></span>

    <span data-ttu-id="65df1-317">(Código de trecho - *ação de galeria do ASP.NET MVC 4 laboratório - Ex02 -*)</span><span class="sxs-lookup"><span data-stu-id="65df1-317">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Gallery Action*)</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample4.cs)]
~~~
7. <span data-ttu-id="65df1-318">Pressione **F5** para executar a solução e, em seguida, navegue até a URL a seguir para testar o serviço de foto fictícias: `http://localhost:[port]/photo/gallery` (o valor [port] depende da porta atual em que o aplicativo foi iniciado).</span><span class="sxs-lookup"><span data-stu-id="65df1-318">Press **F5** to run the solution, and then browse to the following URL in order to test the mocked photo service: `http://localhost:[port]/photo/gallery` (the [port] value depends on the current port where the application was launched).</span></span> <span data-ttu-id="65df1-319">A solicitação para essa URL deve recuperar o conteúdo a **Photos.json** arquivo.</span><span class="sxs-lookup"><span data-stu-id="65df1-319">The request to this URL should retrieve the content of the **Photos.json** file.</span></span>

    <span data-ttu-id="65df1-320">![Testando o serviço de foto fictícios](whats-new-in-aspnet-mvc-4/_static/image20.png "testando o serviço de foto fictícios")</span><span class="sxs-lookup"><span data-stu-id="65df1-320">![Testing the mocked photo service](whats-new-in-aspnet-mvc-4/_static/image20.png "Testing the mocked photo service")</span></span>

    <span data-ttu-id="65df1-321">*Testando o serviço de foto fictícios*</span><span class="sxs-lookup"><span data-stu-id="65df1-321">*Testing the mocked photo service*</span></span>

<span data-ttu-id="65df1-322">Em uma implementação real, você pode usar [ASP.NET Web API](../../../../web-api/index.md) para implementar o serviço de galeria de fotos.</span><span class="sxs-lookup"><span data-stu-id="65df1-322">In a real implementation you could use [ASP.NET Web API](../../../../web-api/index.md) to implement the Photo Gallery service.</span></span> <span data-ttu-id="65df1-323">API da Web do ASP.NET é uma estrutura que torna mais fácil criar serviços HTTP que alcançam uma ampla gama de clientes, incluindo navegadores e dispositivos móveis.</span><span class="sxs-lookup"><span data-stu-id="65df1-323">ASP.NET Web API is a framework that makes it easy to build HTTP services that reach a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="65df1-324">A API do ASP.NET Web é uma plataforma ideal para criar aplicativos com REST no .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="65df1-324">ASP.NET Web API is an ideal platform for building RESTful applications on the .NET Framework.</span></span>

<a id="Task_2_-_Displaying_the_Photo_Gallery"></a>
#### <a name="task-2---displaying-the-photo-gallery"></a><span data-ttu-id="65df1-325">Tarefa 2 - exibindo a Galeria de fotos</span><span class="sxs-lookup"><span data-stu-id="65df1-325">Task 2 - Displaying the Photo Gallery</span></span>

<span data-ttu-id="65df1-326">Nesta tarefa, você atualizará a página inicial para mostrar a Galeria de fotos usando o serviço fictícios que você criou na primeira tarefa deste exercício.</span><span class="sxs-lookup"><span data-stu-id="65df1-326">In this task, you will update the Home page to show the photo gallery by using the mocked service you created in the first task of this exercise.</span></span> <span data-ttu-id="65df1-327">Você adicionará os arquivos de modelo e atualizar os modos de exibição de galeria.</span><span class="sxs-lookup"><span data-stu-id="65df1-327">You will add model files and update the gallery views.</span></span>

1. <span data-ttu-id="65df1-328">No Visual Studio, pressione **SHIFT** + **F5** para parar a depuração do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="65df1-328">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>
2. <span data-ttu-id="65df1-329">Criar o **foto** classe no **modelos** pasta.</span><span class="sxs-lookup"><span data-stu-id="65df1-329">Create the **Photo** class in the **Models** folder.</span></span> <span data-ttu-id="65df1-330">Para fazer isso, clique duas vezes no **modelos** pasta, selecione **adicionar** e clique em **classe**.</span><span class="sxs-lookup"><span data-stu-id="65df1-330">To do this, right-click on the **Models** folder, select **Add** and click **Class**.</span></span> <span data-ttu-id="65df1-331">Em seguida, defina o nome como **Photo.cs** e clique em **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="65df1-331">Then, set the name to **Photo.cs** and click **Add**.</span></span>
3. <span data-ttu-id="65df1-332">Adicionar os seguintes membros para o **foto** classe.</span><span class="sxs-lookup"><span data-stu-id="65df1-332">Add the following members to the **Photo** class.</span></span>

    <span data-ttu-id="65df1-333">(Código de trecho - *modelo foto do ASP.NET MVC 4 laboratório - Ex02 -*)</span><span class="sxs-lookup"><span data-stu-id="65df1-333">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Photo model*)</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample5.cs)]
~~~
4. <span data-ttu-id="65df1-334">Abra o arquivo **HomeController.cs** da pasta **Controladores**.</span><span class="sxs-lookup"><span data-stu-id="65df1-334">Open the **HomeController.cs** file from the **Controllers** folder.</span></span>
5. <span data-ttu-id="65df1-335">Adicione o seguinte usando instruções.</span><span class="sxs-lookup"><span data-stu-id="65df1-335">Add the following using statements.</span></span>

    <span data-ttu-id="65df1-336">(Código de trecho - *usos do ASP.NET MVC 4 laboratório - Ex02 - HomeController*)</span><span class="sxs-lookup"><span data-stu-id="65df1-336">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - HomeController Usings*)</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample6.cs)]
~~~
6. <span data-ttu-id="65df1-337">Atualização de **índice** ação a ser usada **HttpClient** para recuperar os dados da galeria e, em seguida, use o **JavaScriptSerializer** desserializá-lo para o modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="65df1-337">Update the **Index** action to use **HttpClient** to retrieve the gallery data, and then use the **JavaScriptSerializer** to deserialize it to the view model.</span></span>

    <span data-ttu-id="65df1-338">(Código de trecho - *ação de índice do ASP.NET MVC 4 laboratório - Ex02 -*)</span><span class="sxs-lookup"><span data-stu-id="65df1-338">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Index Action*)</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample7.cs)]
~~~
7. <span data-ttu-id="65df1-339">Abra o **cshtml** arquivo localizado sob o **Views\Home** pasta e substitua todo o conteúdo com o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="65df1-339">Open the **Index.cshtml** file located under the **Views\Home** folder and replace all the content with the following code.</span></span>

    <span data-ttu-id="65df1-340">Esse código percorre todas as fotos recuperadas do serviço e os exibe em uma lista não ordenada.</span><span class="sxs-lookup"><span data-stu-id="65df1-340">This code loops through all the photos retrieved from the service and displays them into an unordered list.</span></span>

    <span data-ttu-id="65df1-341">(Código de trecho - *lista de fotos do ASP.NET MVC 4 laboratório - Ex02 -*)</span><span class="sxs-lookup"><span data-stu-id="65df1-341">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Photo List*)</span></span>


~~~
[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample8.cshtml)]
~~~
8. <span data-ttu-id="65df1-342">No **Solution Explorer**, com o botão direito do **conteúdo** pasta do seu projeto e selecione **adicionar | Item existente**.</span><span class="sxs-lookup"><span data-stu-id="65df1-342">In the **Solution Explorer**, right-click the **Content** folder of your project, and select **Add | Existing Item**.</span></span> <span data-ttu-id="65df1-343">Navegue até o **Source\Assets\Content** pasta deste laboratório e adicione o **Site.css** arquivo.</span><span class="sxs-lookup"><span data-stu-id="65df1-343">Browse to the **Source\Assets\Content** folder of this lab and add the **Site.css** file.</span></span> <span data-ttu-id="65df1-344">Você precisará confirmar sua substituição.</span><span class="sxs-lookup"><span data-stu-id="65df1-344">You will have to confirm its replacement.</span></span> <span data-ttu-id="65df1-345">Se você tiver o **Site.css** arquivo aberto, você precisará confirmar para recarregar o arquivo também.</span><span class="sxs-lookup"><span data-stu-id="65df1-345">If you have the **Site.css** file open, you will have to confirm to reload the file also.</span></span>
9. <span data-ttu-id="65df1-346">Abra o Explorador de arquivos e copie todo o **fotos** pasta localizado sob o **Source\Assets** pasta deste laboratório para a pasta raiz do seu projeto no Gerenciador de soluções.</span><span class="sxs-lookup"><span data-stu-id="65df1-346">Open File Explorer and copy the entire **Photos** folder located under the **Source\Assets** folder of this lab to the root folder of your project in Solution Explorer.</span></span>
10. <span data-ttu-id="65df1-347">Execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="65df1-347">Run the application.</span></span> <span data-ttu-id="65df1-348">Agora você deve ver a home page exibe as fotos na Galeria.</span><span class="sxs-lookup"><span data-stu-id="65df1-348">You should now see the home page displaying the photos in the gallery.</span></span>

    <span data-ttu-id="65df1-349">![Galeria de fotos](whats-new-in-aspnet-mvc-4/_static/image21.png "Galeria de fotos")</span><span class="sxs-lookup"><span data-stu-id="65df1-349">![Photo Gallery](whats-new-in-aspnet-mvc-4/_static/image21.png "Photo Gallery")</span></span>

    <span data-ttu-id="65df1-350">*Galeria de fotos*</span><span class="sxs-lookup"><span data-stu-id="65df1-350">*Photo Gallery*</span></span>
11. <span data-ttu-id="65df1-351">No Visual Studio, pressione **SHIFT** + **F5** para parar a depuração do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="65df1-351">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Adding_support_for_mobile_devices"></a>
### <a name="exercise-3-adding-support-for-mobile-devices"></a><span data-ttu-id="65df1-352">Exercício 3: Adicionando suporte para dispositivos móveis</span><span class="sxs-lookup"><span data-stu-id="65df1-352">Exercise 3: Adding support for mobile devices</span></span>

<span data-ttu-id="65df1-353">Uma das atualizações de chave no ASP.NET MVC 4 é o suporte para desenvolvimento móvel.</span><span class="sxs-lookup"><span data-stu-id="65df1-353">One of the key updates in ASP.NET MVC 4 is the support for mobile development.</span></span> <span data-ttu-id="65df1-354">Neste exercício, você irá explorar os novos recursos do ASP.NET MVC 4 para aplicativos móveis ao estender a solução Galeriadefotos que você criou no exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="65df1-354">In this exercise, you will explore ASP.NET MVC 4 new features for mobile applications by extending the PhotoGallery solution you have created in the previous exercise.</span></span>

<a id="Task_1_-_Installing_jQuery_Mobile_in_an_ASPNET_MVC_4_Application"></a>
#### <a name="task-1---installing-jquery-mobile-in-an-aspnet-mvc-4-application"></a><span data-ttu-id="65df1-355">Tarefa 1: instalar o jQuery Mobile em um aplicativo do ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="65df1-355">Task 1 - Installing jQuery Mobile in an ASP.NET MVC 4 Application</span></span>

1. <span data-ttu-id="65df1-356">Abra o **começar** solução localizado em **fonte/Ex3-MobileSupport/Begin/** pasta.</span><span class="sxs-lookup"><span data-stu-id="65df1-356">Open the **Begin** solution located at **Source/Ex3-MobileSupport/Begin/** folder.</span></span> <span data-ttu-id="65df1-357">Caso contrário, você pode continuar usando o **final** solução obtido executando o exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="65df1-357">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="65df1-358">Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="65df1-358">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="65df1-359">Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="65df1-359">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="65df1-360">No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="65df1-360">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="65df1-361">Por fim, compile a solução clicando **criar** | **compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="65df1-361">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="65df1-362">Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="65df1-362">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="65df1-363">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="65df1-363">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="65df1-364">É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.</span><span class="sxs-lookup"><span data-stu-id="65df1-364">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="65df1-365">Abra o **Package Manager Console** clicando o **ferramentas** &gt; **Gerenciador de biblioteca de pacote** &gt; **Package Manager Console** opção de menu.</span><span class="sxs-lookup"><span data-stu-id="65df1-365">Open the **Package Manager Console** by clicking the **Tools** &gt; **Library Package Manager** &gt; **Package Manager Console** menu option.</span></span>

    <span data-ttu-id="65df1-366">![Abrindo o NuGet Package Manager Console](whats-new-in-aspnet-mvc-4/_static/image22.png "abrindo o NuGet Package Manager Console")</span><span class="sxs-lookup"><span data-stu-id="65df1-366">![Opening the NuGet Package Manager Console](whats-new-in-aspnet-mvc-4/_static/image22.png "Opening the NuGet Package Manager Console")</span></span>

    <span data-ttu-id="65df1-367">*Abrindo o Console do Gerenciador de pacote do NuGet*</span><span class="sxs-lookup"><span data-stu-id="65df1-367">*Opening the NuGet Package Manager Console*</span></span>
3. <span data-ttu-id="65df1-368">No Console do Gerenciador de pacotes, execute o seguinte comando para instalar o **jQuery.Mobile.MVC** pacote.</span><span class="sxs-lookup"><span data-stu-id="65df1-368">In the Package Manager Console run the following command to install the **jQuery.Mobile.MVC** package.</span></span>

    <span data-ttu-id="65df1-369">jQuery Mobile é uma biblioteca de código aberto para a criação de interface da web otimizada para toque.</span><span class="sxs-lookup"><span data-stu-id="65df1-369">jQuery Mobile is an open source library for building touch-optimized web UI.</span></span> <span data-ttu-id="65df1-370">O pacote jQuery.Mobile.MVC NuGet inclui auxiliares para usar o jQuery Mobile com um aplicativo ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="65df1-370">The jQuery.Mobile.MVC NuGet package includes helpers to use jQuery Mobile with an ASP.NET MVC 4 application.</span></span>

    > [!NOTE]
    > <span data-ttu-id="65df1-371">Ao executar o comando a seguir, você irá ser baixando a biblioteca de jQuery.Mobile.MVC do Nuget.</span><span class="sxs-lookup"><span data-stu-id="65df1-371">By running the following command, you will be downloading the jQuery.Mobile.MVC library from Nuget.</span></span>

    <span data-ttu-id="65df1-372">PM</span><span class="sxs-lookup"><span data-stu-id="65df1-372">PM</span></span>

    [!code-powershell[Main](whats-new-in-aspnet-mvc-4/samples/sample9.ps1)]

    <span data-ttu-id="65df1-373">Esse comando instala o jQuery Mobile e alguns arquivos auxiliares, incluindo o seguinte:</span><span class="sxs-lookup"><span data-stu-id="65df1-373">This command installs jQuery Mobile and some helper files, including the following:</span></span>

    - <span data-ttu-id="65df1-374">**Exibições/compartilhadas/\_Layout.Mobile.cshtml**: é um layout com base em Mobile jQuery otimizado para uma tela menor.</span><span class="sxs-lookup"><span data-stu-id="65df1-374">**Views/Shared/\_Layout.Mobile.cshtml**: is a jQuery Mobile-based layout optimized for a smaller screen.</span></span> <span data-ttu-id="65df1-375">Quando o site recebe uma solicitação de um navegador móvel, ele substituirá o layout original (\_cshtml) com este.</span><span class="sxs-lookup"><span data-stu-id="65df1-375">When the website receives a request from a mobile browser, it will replace the original layout (\_Layout.cshtml) with this one.</span></span>
    - <span data-ttu-id="65df1-376">Um componente de alternância de exibição: consiste de **exibições/compartilhadas/\_ViewSwitcher.cshtml** exibição parcial e o **ViewSwitcherController.cs** controlador.</span><span class="sxs-lookup"><span data-stu-id="65df1-376">A view-switcher component: consists of the **Views/Shared/\_ViewSwitcher.cshtml** partial view and the **ViewSwitcherController.cs** controller.</span></span> <span data-ttu-id="65df1-377">Este componente exibirá um link em navegadores de dispositivos móveis para permitir que os usuários alternar para a versão da área de trabalho da página.</span><span class="sxs-lookup"><span data-stu-id="65df1-377">This component will show a link on mobile browsers to enable users to switch to the desktop version of the page.</span></span>  
        <span data-ttu-id="65df1-378">![Projeto da Galeria de fotos com suporte móvel](whats-new-in-aspnet-mvc-4/_static/image23.png "projeto da Galeria de fotos com suporte móvel")</span><span class="sxs-lookup"><span data-stu-id="65df1-378">![Photo Gallery project with mobile support](whats-new-in-aspnet-mvc-4/_static/image23.png "Photo Gallery project with mobile support")</span></span>

        <span data-ttu-id="65df1-379">*Projeto da Galeria de fotos com suporte móvel*</span><span class="sxs-lookup"><span data-stu-id="65df1-379">*Photo Gallery project with mobile support*</span></span>
4. <span data-ttu-id="65df1-380">Registre os pacotes móveis.</span><span class="sxs-lookup"><span data-stu-id="65df1-380">Register the Mobile bundles.</span></span> <span data-ttu-id="65df1-381">Para fazer isso, abra o **Global.asax.cs** e adicione a linha a seguir.</span><span class="sxs-lookup"><span data-stu-id="65df1-381">To do this, open the **Global.asax.cs** file and add the following line.</span></span>

    <span data-ttu-id="65df1-382">(Código de trecho - *pacotes móvel do ASP.NET MVC 4 laboratório - Ex03 - Register*)</span><span class="sxs-lookup"><span data-stu-id="65df1-382">(Code Snippet - *ASP.NET MVC 4 Lab - Ex03 - Register Mobile Bundles*)</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample10.cs)]
~~~
5. <span data-ttu-id="65df1-383">Execute o aplicativo usando um navegador da web da área de trabalho.</span><span class="sxs-lookup"><span data-stu-id="65df1-383">Run the application using a desktop web browser.</span></span>
6. <span data-ttu-id="65df1-384">Abra o **emulador do Windows Phone 7,** localizado em **Menu Iniciar | Todos os programas | Windows Phone SDK 7.1 | Emulador do Windows Phone.**</span><span class="sxs-lookup"><span data-stu-id="65df1-384">Open the **Windows Phone 7 Emulator,** located in **Start Menu | All Programs | Windows Phone SDK 7.1 | Windows Phone Emulator.**</span></span>
7. <span data-ttu-id="65df1-385">Na tela de início do telefone, abra o Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="65df1-385">In the phone start screen, open Internet Explorer.</span></span> <span data-ttu-id="65df1-386">Verifique a URL onde o aplicativo é iniciado e navegue até essa URL com o navegador do telefone (por exemplo, `http://localhost:[PortNumber]/`).</span><span class="sxs-lookup"><span data-stu-id="65df1-386">Check out the URL where the application started and browse to that URL with the phone browser (e.g. `http://localhost:[PortNumber]/`).</span></span>

    <span data-ttu-id="65df1-387">Você observará que seu aplicativo terá uma aparência diferente no emulador do Windows Phone, como o jQuery.Mobile.MVC criou novos ativos em seu projeto que mostram exibições otimizadas para dispositivos móveis.</span><span class="sxs-lookup"><span data-stu-id="65df1-387">You will notice that your application will look different in the Windows Phone emulator, as the jQuery.Mobile.MVC has created new assets in your project that show views optimized for mobile devices.</span></span>

    <span data-ttu-id="65df1-388">Observe a mensagem na parte superior do telefone, mostrando o link que alterna para o modo de exibição da área de trabalho.</span><span class="sxs-lookup"><span data-stu-id="65df1-388">Notice the message at the top of the phone, showing the link that switches to the Desktop view.</span></span> <span data-ttu-id="65df1-389">Além disso, o  **\_Layout.Mobile.cshtml** layout que foi criado com o pacote que você instalou incluem um layout diferente no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="65df1-389">Additionally, the **\_Layout.Mobile.cshtml** layout that was created by the package you have installed is including a different layout in the application.</span></span>

    > [!NOTE]
    > <span data-ttu-id="65df1-390">Até agora, há um link para voltar ao modo de exibição móvel.</span><span class="sxs-lookup"><span data-stu-id="65df1-390">So far, there is no link to get back to mobile view.</span></span> <span data-ttu-id="65df1-391">Ele será incluído em versões posteriores.</span><span class="sxs-lookup"><span data-stu-id="65df1-391">It will be included in later versions.</span></span>

    <span data-ttu-id="65df1-392">![Modo de exibição móvel da página inicial da Galeria de fotos](whats-new-in-aspnet-mvc-4/_static/image24.png "exibição móvel da página inicial da Galeria de fotos")</span><span class="sxs-lookup"><span data-stu-id="65df1-392">![Mobile view of the Photo Gallery Home page](whats-new-in-aspnet-mvc-4/_static/image24.png "Mobile view of the Photo Gallery Home page")</span></span>

    <span data-ttu-id="65df1-393">*Modo de exibição móvel da página inicial da Galeria de fotos*</span><span class="sxs-lookup"><span data-stu-id="65df1-393">*Mobile view of the Photo Gallery Home page*</span></span>
8. <span data-ttu-id="65df1-394">No Visual Studio, pressione **SHIFT** + **F5** para parar a depuração do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="65df1-394">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>

<a id="Task_2_-_Creating_Mobile_Views"></a>
#### <a name="task-2---creating-mobile-views"></a><span data-ttu-id="65df1-395">Tarefa 2: Criando modos de exibição móvel</span><span class="sxs-lookup"><span data-stu-id="65df1-395">Task 2 - Creating Mobile Views</span></span>

<span data-ttu-id="65df1-396">Nesta tarefa, você criará uma versão móvel do modo de exibição de índice com conteúdo adaptado para appareance melhor em dispositivos móveis.</span><span class="sxs-lookup"><span data-stu-id="65df1-396">In this task, you will create a mobile version of the index view with content adapted for better appareance in mobile devices.</span></span>

1. <span data-ttu-id="65df1-397">Copie o **Views\Home\Index.cshtml** exibir e cole-o para criar uma cópia, renomeie o novo arquivo **Index.Mobile.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="65df1-397">Copy the **Views\Home\Index.cshtml** view and paste it to create a copy, rename the new file to **Index.Mobile.cshtml**.</span></span>
2. <span data-ttu-id="65df1-398">Abra o novo criado **Index.Mobile.cshtml** exibir e substituir o &lt;ul&gt; marca com este código.</span><span class="sxs-lookup"><span data-stu-id="65df1-398">Open the new created **Index.Mobile.cshtml** view and replace the existing &lt;ul&gt; tag with this code.</span></span> <span data-ttu-id="65df1-399">Ao fazer isso, você atualizará o &lt;ul&gt; marca com anotações de dados móveis jQuery para usar os temas móveis do jQuery.</span><span class="sxs-lookup"><span data-stu-id="65df1-399">By doing this, you will be updating the &lt;ul&gt; tag with jQuery Mobile data annotations to use the mobile themes from jQuery.</span></span>


~~~
[!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample11.html)]

> [!NOTE] 
> 
> Notice that:
> 
> - The **data-role** attribute set to **listview** will render the list using the listview styles.
> 
> - The **data-inset** attribute set to true will show the list with rounded border and margin.
> 
> - The **data-filter** attribute set to **true** will generate a search box.
> 
> You can learn more about jQuery Mobile conventions in the project documentation: [[http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)
~~~
3. <span data-ttu-id="65df1-400">Pressione **CTRL + S** para salvar as alterações.</span><span class="sxs-lookup"><span data-stu-id="65df1-400">Press **CTRL + S** to save the changes.</span></span>
4. <span data-ttu-id="65df1-401">Alterne para o **emulador do Windows Phone** e atualizar o site.</span><span class="sxs-lookup"><span data-stu-id="65df1-401">Switch to the **Windows Phone Emulator** and refresh the site.</span></span> <span data-ttu-id="65df1-402">Observe a nova aparência da lista da galeria, bem como a nova caixa de pesquisa localizada na parte superior.</span><span class="sxs-lookup"><span data-stu-id="65df1-402">Notice the new look and feel of the gallery list, as well as the new search box located on the top.</span></span> <span data-ttu-id="65df1-403">Em seguida, digite uma palavra na caixa de pesquisa (por exemplo, **Tulips**) para iniciar uma pesquisa na Galeria de fotos.</span><span class="sxs-lookup"><span data-stu-id="65df1-403">Then, type a word in the search box (for instance, **Tulips**) to start a search in the photo gallery.</span></span>

    <span data-ttu-id="65df1-404">![Galeria usando o estilo de listview com filtragem](whats-new-in-aspnet-mvc-4/_static/image25.png "galeria usando o estilo de listview com filtragem")</span><span class="sxs-lookup"><span data-stu-id="65df1-404">![Gallery using listview style with filtering](whats-new-in-aspnet-mvc-4/_static/image25.png "Gallery using listview style with filtering")</span></span>

    <span data-ttu-id="65df1-405">*Usando o estilo de listview com a filtragem de galeria*</span><span class="sxs-lookup"><span data-stu-id="65df1-405">*Gallery using listview style with filtering*</span></span>

    <span data-ttu-id="65df1-406">Para resumir, você usou a receita mobilizador de exibição para criar uma cópia do modo de exibição de índice com o &quot;móvel&quot; sufixo.</span><span class="sxs-lookup"><span data-stu-id="65df1-406">To summarize, you have used the View Mobilizer recipe to create a copy of the Index view with the &quot;mobile&quot; suffix.</span></span> <span data-ttu-id="65df1-407">Este sufixo indica ao ASP.NET MVC 4 que cada solicitação gerada a partir de um dispositivo móvel usará esta cópia do índice.</span><span class="sxs-lookup"><span data-stu-id="65df1-407">This suffix indicates to ASP.NET MVC 4 that every request generated from a mobile device will use this copy of the index.</span></span> <span data-ttu-id="65df1-408">Além disso, você atualizou a versão móvel do modo de exibição de índice para usar o jQuery Mobile para melhorar a aparência e funcionalidade do site em dispositivos móveis.</span><span class="sxs-lookup"><span data-stu-id="65df1-408">Additionally, you have updated the mobile version of the Index view to use jQuery Mobile for enhancing the site look and feel in mobile devices.</span></span>
5. <span data-ttu-id="65df1-409">Volte para o Visual Studio e abra **Site.Mobile.css** localizado sob o **conteúdo** pasta.</span><span class="sxs-lookup"><span data-stu-id="65df1-409">Go back to Visual Studio and open **Site.Mobile.css** located under the **Content** folder.</span></span>
6. <span data-ttu-id="65df1-410">Corrija o posicionamento do título da foto para mostrá-la no lado direito da imagem.</span><span class="sxs-lookup"><span data-stu-id="65df1-410">Fix the positioning of the photo title to make it show at the right side of the image.</span></span> <span data-ttu-id="65df1-411">Para fazer isso, adicione o seguinte código para o **Site.Mobile.css** arquivo.</span><span class="sxs-lookup"><span data-stu-id="65df1-411">To do this, add the following code to the **Site.Mobile.css** file.</span></span>

    <span data-ttu-id="65df1-412">CSS</span><span class="sxs-lookup"><span data-stu-id="65df1-412">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-mvc-4/samples/sample12.css)]
7. <span data-ttu-id="65df1-413">Pressione **CTRL + S** para salvar as alterações.</span><span class="sxs-lookup"><span data-stu-id="65df1-413">Press **CTRL + S** to save the changes.</span></span>
8. <span data-ttu-id="65df1-414">Volte para o **emulador do Windows Phone** e atualizar o site.</span><span class="sxs-lookup"><span data-stu-id="65df1-414">Switch back to the **Windows Phone Emulator** and refresh the site.</span></span> <span data-ttu-id="65df1-415">Observe que o título de foto corretamente é posicionado agora.</span><span class="sxs-lookup"><span data-stu-id="65df1-415">Notice the photo title is properly positioned now.</span></span>

    <span data-ttu-id="65df1-416">![Título posicionado no lado direito da imagem](whats-new-in-aspnet-mvc-4/_static/image26.png "título posicionado no lado direito da imagem")</span><span class="sxs-lookup"><span data-stu-id="65df1-416">![Title positioned on the right side of the image](whats-new-in-aspnet-mvc-4/_static/image26.png "Title positioned on the right side of the image")</span></span>

    <span data-ttu-id="65df1-417">*Título posicionado no lado direito da imagem*</span><span class="sxs-lookup"><span data-stu-id="65df1-417">*Title positioned on the right side of the image*</span></span>

<a id="Task_3_-_jQuery_Mobile_Themes"></a>
#### <a name="task-3---jquery-mobile-themes"></a><span data-ttu-id="65df1-418">Tarefa 3 - jQuery Mobile temas</span><span class="sxs-lookup"><span data-stu-id="65df1-418">Task 3 - jQuery Mobile Themes</span></span>

<span data-ttu-id="65df1-419">Cada layout e o widget em jQuery Mobile foi projetada para um novo orientada a objeto CSS do framework que seja possível aplicar um tema concluída design visual unificada para sites e aplicativos.</span><span class="sxs-lookup"><span data-stu-id="65df1-419">Every layout and widget in jQuery Mobile is designed around a new object-oriented CSS framework that makes it possible to apply a complete unified visual design theme to sites and applications.</span></span>

<span data-ttu-id="65df1-420">padrão do jQuery Mobile tema inclui 5 amostras que recebem letras (a, b, c, r, e) para referência rápida.</span><span class="sxs-lookup"><span data-stu-id="65df1-420">jQuery Mobile's default Theme includes 5 swatches that are given letters (a, b, c, d, e) for quick reference.</span></span>

<span data-ttu-id="65df1-421">Nesta tarefa, você atualizará o layout móvel para usar um tema diferente do padrão.</span><span class="sxs-lookup"><span data-stu-id="65df1-421">In this task, you will update the mobile layout to use a different theme than the default.</span></span>

1. <span data-ttu-id="65df1-422">Alterne para o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="65df1-422">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="65df1-423">Abra o  **\_Layout.Mobile.cshtml** arquivo localizado em **exibições \ compartilhadas**.</span><span class="sxs-lookup"><span data-stu-id="65df1-423">Open the **\_Layout.Mobile.cshtml** file located in **Views\Shared**.</span></span>
3. <span data-ttu-id="65df1-424">Localizar o elemento div com a função de dados definida como &quot;página&quot; e atualize o **dados tema** para &quot; **e**&quot;.</span><span class="sxs-lookup"><span data-stu-id="65df1-424">Find the div element with the data-role set to &quot;page&quot; and update the **data-theme** to &quot;**e**&quot;.</span></span>


~~~
[!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample13.html)]
~~~
4. <span data-ttu-id="65df1-425">Pressione **CTRL + S** para salvar as alterações.</span><span class="sxs-lookup"><span data-stu-id="65df1-425">Press **CTRL + S** to save the changes.</span></span>
5. <span data-ttu-id="65df1-426">Atualizar o site no **emulador do Windows Phone** e observe o novo esquema de cores.</span><span class="sxs-lookup"><span data-stu-id="65df1-426">Refresh the site in the **Windows Phone Emulator** and notice the new colors scheme.</span></span>

    <span data-ttu-id="65df1-427">![Layout para dispositivos móveis com um esquema de cores diferentes](whats-new-in-aspnet-mvc-4/_static/image27.png "layout com um esquema de cores diferentes para dispositivos móveis")</span><span class="sxs-lookup"><span data-stu-id="65df1-427">![Mobile layout with a different color scheme](whats-new-in-aspnet-mvc-4/_static/image27.png "Mobile layout with a different color scheme")</span></span>

    <span data-ttu-id="65df1-428">*Layout para dispositivos móveis com um esquema de cores diferentes*</span><span class="sxs-lookup"><span data-stu-id="65df1-428">*Mobile layout with a different color scheme*</span></span>

<a id="Task_4_-_Using_the_View-Switcher_Component_and_the_Browser_Overriding_Features"></a>
#### <a name="task-4---using-the-view-switcher-component-and-the-browser-overriding-features"></a><span data-ttu-id="65df1-429">Tarefa 4 - usando o componente de alternância de exibição e o navegador substituindo recursos</span><span class="sxs-lookup"><span data-stu-id="65df1-429">Task 4 - Using the View-Switcher Component and the Browser Overriding Features</span></span>

<span data-ttu-id="65df1-430">É uma convenção mobile otimizado páginas da web para adicionar um link cujo texto é algo como o modo de exibição da área de trabalho ou modo completo do site que permite aos usuários a alternar para uma versão de área de trabalho da página.</span><span class="sxs-lookup"><span data-stu-id="65df1-430">A convention for mobile-optimized web pages is to add a link whose text is something like Desktop view or Full site mode that lets users switch to a desktop version of the page.</span></span> <span data-ttu-id="65df1-431">O pacote jQuery.Mobile.MVC inclui um exemplo **alternância de exibição** componente para essa finalidade usada no  **\_Layout.Mobile.cshtml** exibição.</span><span class="sxs-lookup"><span data-stu-id="65df1-431">The jQuery.Mobile.MVC package includes a sample **view-switcher** component for this purpose used in the **\_Layout.Mobile.cshtml** view.</span></span>

<span data-ttu-id="65df1-432">![Link para alternar para modo de área de trabalho](whats-new-in-aspnet-mvc-4/_static/image28.png "Link para alternar para modo de área de trabalho")</span><span class="sxs-lookup"><span data-stu-id="65df1-432">![Link to switch to Desktop View](whats-new-in-aspnet-mvc-4/_static/image28.png "Link to switch to Desktop View")</span></span>

<span data-ttu-id="65df1-433">*Para alternar para modo de exibição da área de trabalho*</span><span class="sxs-lookup"><span data-stu-id="65df1-433">*Link to switch to Desktop View*</span></span>

<span data-ttu-id="65df1-434">A alternância de exibição usa um novo recurso chamado **navegador substituindo**.</span><span class="sxs-lookup"><span data-stu-id="65df1-434">The view switcher uses a new feature called **Browser Overriding**.</span></span> <span data-ttu-id="65df1-435">Esse recurso permite que seu aplicativo lide com solicitações como se estivessem vindo de um navegador diferente (agente do usuário) que eles realmente são provenientes.</span><span class="sxs-lookup"><span data-stu-id="65df1-435">This feature lets your application treat requests as if they were coming from a different browser (user agent) than the one they are actually coming from.</span></span>

<span data-ttu-id="65df1-436">Nesta tarefa, você irá explorar a implementação de exemplo de um alternador de exibição adicionado jQuery.Mobile.MVC e o novo navegador substituindo recursos no ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="65df1-436">In this task, you will explore the sample implementation of a view-switcher added by jQuery.Mobile.MVC and the new browser overriding features in ASP.NET MVC 4.</span></span>

1. <span data-ttu-id="65df1-437">Alterne para o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="65df1-437">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="65df1-438">Abra o  **\_Layout.Mobile.cshtml** exibição localizado sob o **exibições \ compartilhadas** pasta e observe o componente de alternância de exibição que está sendo referenciado como uma exibição parcial.</span><span class="sxs-lookup"><span data-stu-id="65df1-438">Open the **\_Layout.Mobile.cshtml** view located under the **Views\Shared** folder and notice the view-switcher component being referenced as a partial view.</span></span>

    <span data-ttu-id="65df1-439">![Layout para dispositivos móveis usando o componente de alternância de exibição](whats-new-in-aspnet-mvc-4/_static/image29.png "layout para dispositivos móveis usando o componente de alternância de exibição")</span><span class="sxs-lookup"><span data-stu-id="65df1-439">![Mobile layout using View-Switcher component](whats-new-in-aspnet-mvc-4/_static/image29.png "Mobile layout using View-Switcher component")</span></span>

    <span data-ttu-id="65df1-440">*Layout para dispositivos móveis usando o componente de alternância de exibição*</span><span class="sxs-lookup"><span data-stu-id="65df1-440">*Mobile layout using View-Switcher component*</span></span>
3. <span data-ttu-id="65df1-441">Abra o  **\_ViewSwitcher.cshtml** exibição parcial.</span><span class="sxs-lookup"><span data-stu-id="65df1-441">Open the **\_ViewSwitcher.cshtml** partial view.</span></span>

    <span data-ttu-id="65df1-442">A exibição parcial usa o novo método **ViewContext.HttpContext.GetOverriddenBrowser()** para determinar a origem da solicitação da web e mostrar o link correspondente para alternar entre os modos de exibição do Desktop ou Mobile.</span><span class="sxs-lookup"><span data-stu-id="65df1-442">The partial view uses the new method **ViewContext.HttpContext.GetOverriddenBrowser()** to determine the origin of the web request and show the corresponding link to switch either to the Desktop or Mobile views.</span></span>

    <span data-ttu-id="65df1-443">O **GetOverridenBrowser** método retorna um **HttpBrowserCapabilitiesBase** instância que corresponde ao agente do usuário definido atualmente para a solicitação (reais ou substituído).</span><span class="sxs-lookup"><span data-stu-id="65df1-443">The **GetOverridenBrowser** method returns an **HttpBrowserCapabilitiesBase** instance that corresponds to the user agent currently set for the request (actual or overridden).</span></span> <span data-ttu-id="65df1-444">Você pode usar esse valor para obter as propriedades como **IsMobileDevice**.</span><span class="sxs-lookup"><span data-stu-id="65df1-444">You can use this value to get properties such as **IsMobileDevice**.</span></span>

    <span data-ttu-id="65df1-445">![Exibição parcial ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image30.png "exibição parcial ViewSwitcher")</span><span class="sxs-lookup"><span data-stu-id="65df1-445">![ViewSwitcher partial view](whats-new-in-aspnet-mvc-4/_static/image30.png "ViewSwitcher partial view")</span></span>

    <span data-ttu-id="65df1-446">*Exibição parcial ViewSwitcher*</span><span class="sxs-lookup"><span data-stu-id="65df1-446">*ViewSwitcher partial view*</span></span>
4. <span data-ttu-id="65df1-447">Abra o **ViewSwitcherController.cs** classe localizado no **controladores** pasta.</span><span class="sxs-lookup"><span data-stu-id="65df1-447">Open the **ViewSwitcherController.cs** class located in the **Controllers** folder.</span></span> <span data-ttu-id="65df1-448">Check-out ação SwitchView é chamado por um link no componente ViewSwitcher e observe os novos métodos de HttpContext.</span><span class="sxs-lookup"><span data-stu-id="65df1-448">Check out that SwitchView action is called by the link in the ViewSwitcher component, and notice the new HttpContext methods.</span></span>

    - <span data-ttu-id="65df1-449">O **HttpContext.ClearOverridenBrowser()** método remove qualquer agente de usuário substituído para a solicitação atual.</span><span class="sxs-lookup"><span data-stu-id="65df1-449">The **HttpContext.ClearOverridenBrowser()** method removes any overridden user agent for the current request.</span></span>
    - <span data-ttu-id="65df1-450">O **HttpContext.SetOverridenBrowser()** método substitui o valor do agente de usuário real da solicitação usando o agente do usuário especificado.</span><span class="sxs-lookup"><span data-stu-id="65df1-450">The **HttpContext.SetOverridenBrowser()** method overrides the request's actual user agent value using the specified user agent.</span></span>  
        <span data-ttu-id="65df1-451">![Controlador ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher controlador")</span><span class="sxs-lookup"><span data-stu-id="65df1-451">![ViewSwitcher Controller](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher Controller")</span></span>  
<span data-ttu-id="65df1-452">*Controlador ViewSwitcher*</span><span class="sxs-lookup"><span data-stu-id="65df1-452">*ViewSwitcher Controller*</span></span>

        <span data-ttu-id="65df1-453">Substituição do navegador é dos principais recursos do ASP.NET MVC 4, que também está disponível mesmo se você não instalar o pacote jQuery.Mobile.MVC.</span><span class="sxs-lookup"><span data-stu-id="65df1-453">Browser Overriding is a core feature of ASP.NET MVC 4, which is also available even if you do not install the jQuery.Mobile.MVC package.</span></span> <span data-ttu-id="65df1-454">No entanto, esse recurso afeta somente exibição, layout e exibição parcial, e ele não afeta os recursos que dependem do objeto Request. browser.</span><span class="sxs-lookup"><span data-stu-id="65df1-454">However, this feature affects only view, layout, and partial-view, and it does not affect any of the features that depend on the Request.Browser object.</span></span>

<a id="Task_5_-_Adding_the_View-Switcher_in_the_Desktop_View"></a>
#### <a name="task-5---adding-the-view-switcher-in-the-desktop-view"></a><span data-ttu-id="65df1-455">Tarefa 5: adicionar a alternância de exibição na exibição da área de trabalho</span><span class="sxs-lookup"><span data-stu-id="65df1-455">Task 5 - Adding the View-Switcher in the Desktop View</span></span>

<span data-ttu-id="65df1-456">Nesta tarefa, você atualizará o layout da área de trabalho para incluir a alternância de exibição.</span><span class="sxs-lookup"><span data-stu-id="65df1-456">In this task, you will update the desktop layout to include the view-switcher.</span></span> <span data-ttu-id="65df1-457">Isso permitirá que os usuários móveis voltar para o modo de exibição móvel ao procurar o modo de exibição da área de trabalho.</span><span class="sxs-lookup"><span data-stu-id="65df1-457">This will allow mobile users to go back to the mobile view when browsing the desktop view.</span></span>

1. <span data-ttu-id="65df1-458">Atualizar o site no **emulador do Windows Phone**.</span><span class="sxs-lookup"><span data-stu-id="65df1-458">Refresh the site in the **Windows Phone Emulator**.</span></span>
2. <span data-ttu-id="65df1-459">Clique no **exibição da área de trabalho** link na parte superior da Galeria.</span><span class="sxs-lookup"><span data-stu-id="65df1-459">Click on the **Desktop view** link at the top of the gallery.</span></span> <span data-ttu-id="65df1-460">Observe que não há nenhuma alternância de exibição na exibição da área de trabalho para permitir que você retornar à exibição móvel.</span><span class="sxs-lookup"><span data-stu-id="65df1-460">Notice there is no view-switcher in the desktop view to allow you return to the mobile view.</span></span>
3. <span data-ttu-id="65df1-461">Volte para o Visual Studio e abra o  **\_cshtml** exibição.</span><span class="sxs-lookup"><span data-stu-id="65df1-461">Go back to Visual Studio and open the **\_Layout.cshtml** view.</span></span>
4. <span data-ttu-id="65df1-462">Localize a seção de login e inserir uma chamada para renderizar o  **\_ViewSwitcher** exibição parcial abaixo o  **\_LogOnPartial** exibição parcial.</span><span class="sxs-lookup"><span data-stu-id="65df1-462">Find the login section and insert a call to render the **\_ViewSwitcher** partial view below the **\_LogOnPartial** partial view.</span></span> <span data-ttu-id="65df1-463">Em seguida, pressione **CTRL + S** para salvar as alterações.</span><span class="sxs-lookup"><span data-stu-id="65df1-463">Then, press **CTRL + S** to save the changes.</span></span>


~~~
[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample14.cshtml)]
~~~
5. <span data-ttu-id="65df1-464">Pressione **CTRL + S** para salvar as alterações.</span><span class="sxs-lookup"><span data-stu-id="65df1-464">Press **CTRL + S** to save the changes.</span></span>
6. <span data-ttu-id="65df1-465">Atualize a página no emulador do Windows Phone e clique duas vezes na tela para aumentar o zoom.</span><span class="sxs-lookup"><span data-stu-id="65df1-465">Refresh the page in the Windows Phone Emulator and double-click the screen to zoom in.</span></span> <span data-ttu-id="65df1-466">Observe que a página inicial agora mostra o **exibição móvel** link que alterna de móveis para o modo de área de trabalho.</span><span class="sxs-lookup"><span data-stu-id="65df1-466">Notice that the home page now shows the **Mobile view** link that switches from mobile to desktop view.</span></span>

    <span data-ttu-id="65df1-467">![Exibir alternador renderizado no modo de exibição da área de trabalho](whats-new-in-aspnet-mvc-4/_static/image32.png "alternância de exibição renderizado no modo de exibição da área de trabalho")</span><span class="sxs-lookup"><span data-stu-id="65df1-467">![View Switcher rendered in desktop view](whats-new-in-aspnet-mvc-4/_static/image32.png "View Switcher rendered in desktop view")</span></span>

    <span data-ttu-id="65df1-468">*Alternância de exibição renderizada no modo de exibição da área de trabalho*</span><span class="sxs-lookup"><span data-stu-id="65df1-468">*View Switcher rendered in desktop view*</span></span>
7. <span data-ttu-id="65df1-469">Alterne para o modo de exibição móvel novamente e navegue até <strong>sobre</strong> página (http://localhost[porta] / Home/sobre).</span><span class="sxs-lookup"><span data-stu-id="65df1-469">Switch to the Mobile view again and browse to <strong>About</strong> page (http://localhost[port]/Home/About).</span></span> <span data-ttu-id="65df1-470">Observe que, mesmo se você não criou uma exibição About.Mobile.cshtml, a página sobre é exibida usando o layout para dispositivos móveis (\_Layout.Mobile.cshtml).</span><span class="sxs-lookup"><span data-stu-id="65df1-470">Notice that, even if you haven't created an About.Mobile.cshtml view, the About page is displayed using the mobile layout (\_Layout.Mobile.cshtml).</span></span>

    <span data-ttu-id="65df1-471">![Sobre a página](whats-new-in-aspnet-mvc-4/_static/image33.png "sobre a página")</span><span class="sxs-lookup"><span data-stu-id="65df1-471">![About page](whats-new-in-aspnet-mvc-4/_static/image33.png "About page")</span></span>

    <span data-ttu-id="65df1-472">*Sobre a página*</span><span class="sxs-lookup"><span data-stu-id="65df1-472">*About page*</span></span>
8. <span data-ttu-id="65df1-473">Por fim, abra o site em um navegador da Web da área de trabalho.</span><span class="sxs-lookup"><span data-stu-id="65df1-473">Finally, open the site in a desktop Web browser.</span></span> <span data-ttu-id="65df1-474">Observe que nenhuma das atualizações do anteriores afetou a exibição da área de trabalho.</span><span class="sxs-lookup"><span data-stu-id="65df1-474">Notice that none of the previous updates has affected the desktop view.</span></span>

    <span data-ttu-id="65df1-475">![Exibição de área de trabalho Galeriadefotos](whats-new-in-aspnet-mvc-4/_static/image34.png "Galeriadefotos exibição de área de trabalho")</span><span class="sxs-lookup"><span data-stu-id="65df1-475">![PhotoGallery desktop view](whats-new-in-aspnet-mvc-4/_static/image34.png "PhotoGallery desktop view")</span></span>

    <span data-ttu-id="65df1-476">*Exibição de área de trabalho Galeriadefotos*</span><span class="sxs-lookup"><span data-stu-id="65df1-476">*PhotoGallery desktop view*</span></span>

<a id="Task_6_-_Creating_New_Display_Modes"></a>
#### <a name="task-6---creating-new-display-modes"></a><span data-ttu-id="65df1-477">Tarefa 6: criar novos modos de exibição</span><span class="sxs-lookup"><span data-stu-id="65df1-477">Task 6 - Creating New Display Modes</span></span>

<span data-ttu-id="65df1-478">O novo recurso de modos de exibição permite que um aplicativo selecione exibições dependendo do navegador que está gerando a solicitação.</span><span class="sxs-lookup"><span data-stu-id="65df1-478">The new display modes feature lets an application select views depending on the browser that is generating the request.</span></span> <span data-ttu-id="65df1-479">Por exemplo, se um navegador desktop solicitar a página inicial, o aplicativo retornará o **Views\Home\Index.cshtml** modelo.</span><span class="sxs-lookup"><span data-stu-id="65df1-479">For example, if a desktop browser requests the Home page, the application will return the **Views\Home\Index.cshtml** template.</span></span> <span data-ttu-id="65df1-480">Em seguida, se um navegador móvel solicitar a página inicial, o aplicativo retornará o **Views\Home\Index.mobile.cshtml** modelo.</span><span class="sxs-lookup"><span data-stu-id="65df1-480">Then, if a mobile browser requests the Home page, the application will return the **Views\Home\Index.mobile.cshtml** template.</span></span>

<span data-ttu-id="65df1-481">Nesta tarefa, você criará um layout personalizado para os dispositivos iPhone e você precisará simular solicitações de dispositivos iPhone.</span><span class="sxs-lookup"><span data-stu-id="65df1-481">In this task, you will create a customized layout for iPhone devices, and you will have to simulate requests from iPhone devices.</span></span> <span data-ttu-id="65df1-482">Para fazer isso, você pode usar qualquer um emulador/simulador de iPhone (como [Electric simulador de móveis](http://www.electricplum.com/)) ou um navegador com os complementos que modificam o agente do usuário.</span><span class="sxs-lookup"><span data-stu-id="65df1-482">To do this, you can use either an iPhone emulator/simulator (like [Electric Mobile Simulator](http://www.electricplum.com/)) or a browser with add-ons that modify the user agent.</span></span> <span data-ttu-id="65df1-483">Para obter instruções sobre como definir a cadeia de caracteres de agente do usuário em um navegador Safari emular um iPhone, consulte [como permitir que o Safari fingem é IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) no blog de David Alison.</span><span class="sxs-lookup"><span data-stu-id="65df1-483">For instructions on how to set the user agent string in an Safari browser to emulate an iPhone, see [How to let Safari pretend it's IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) in David Alison's blog.</span></span>

<span data-ttu-id="65df1-484">**Observe que essa tarefa é opcional e você pode continuar em todo o laboratório sem executá-la.**</span><span class="sxs-lookup"><span data-stu-id="65df1-484">**Notice that this task is optional and you can continue throughout the lab without executing it.**</span></span>

1. <span data-ttu-id="65df1-485">No Visual Studio, pressione **SHIFT** + **F5** para parar a depuração do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="65df1-485">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>
2. <span data-ttu-id="65df1-486">Abra **Global.asax.cs** e adicione o seguinte usando a instrução.</span><span class="sxs-lookup"><span data-stu-id="65df1-486">Open **Global.asax.cs** and add the following using statement.</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample15.cs)]
~~~
3. <span data-ttu-id="65df1-487">Adicione o seguinte código realçado no aplicativo\_método Start.</span><span class="sxs-lookup"><span data-stu-id="65df1-487">Add the following highlighted code into the Application\_Start method.</span></span>

    <span data-ttu-id="65df1-488">(Código de trecho - *ASP.NET MVC 4 laboratório - Ex03 - iPhone DisplayMode*)</span><span class="sxs-lookup"><span data-stu-id="65df1-488">(Code Snippet - *ASP.NET MVC 4 Lab - Ex03 - iPhone DisplayMode*)</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample16.cs)]

You have registered a new **DefaultDisplayMode named &quot;iPhone&quot;**, within the static **DisplayModeProvider.Instance.Modes** static list, that will be matched against each incoming request. If the incoming request contains the string &quot;iPhone&quot;, ASP.NET MVC will find the views whose name contain the &quot;iPhone&quot; suffix. The 0 parameter indicates how specific is the new mode; for instance, this view is more specific than the general &quot;.mobile&quot; rule that matches requests from mobile devices.

After this code runs, when an iPhone browser generates a request, your application will use the **Views\Shared\\_Layout.iPhone.cshtml** layout you will create in the next steps.

> [!NOTE]
> This way of testing the request for iPhone has been simplified for demo purposes and might not work as expected for every iPhone user agent string (for example test is case sensitive).
~~~
4. <span data-ttu-id="65df1-489">Criar uma cópia do  **\_Layout.Mobile.cshtml** arquivo o **exibições \ compartilhadas** pasta e renomeie a cópia de &quot; **\_Layout.iPhone.csthml**&quot;.</span><span class="sxs-lookup"><span data-stu-id="65df1-489">Create a copy of the **\_Layout.Mobile.cshtml** file in the **Views\Shared** folder and rename the copy to &quot;**\_Layout.iPhone.csthml**&quot;.</span></span>
5. <span data-ttu-id="65df1-490">Abra  **\_Layout.iPhone.csthml** criado na etapa anterior.</span><span class="sxs-lookup"><span data-stu-id="65df1-490">Open **\_Layout.iPhone.csthml** you created in the previous step.</span></span>
6. <span data-ttu-id="65df1-491">Localizar o elemento div com o atributo de dados de função definido como **página** e altere o **dados tema** atributo &quot; **um**&quot;.</span><span class="sxs-lookup"><span data-stu-id="65df1-491">Find the div element with the data-role attribute set to **page** and change the **data-theme** attribute to &quot;**a**&quot;.</span></span>


~~~
[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample17.cshtml)]

Now you have 3 layouts in your ASP.NET MVC 4 application:

1. **\_Layout.cshtml**: default layout used for desktop browsers.
2. **\_Layout.mobile.cshtml**: default layout used for mobile devices.
3. **\_Layout.iPhone.cshtml**: specific layout for iPhone devices, using a different color scheme to differentiate from \_Layout.mobile.cshtml.
~~~
7. <span data-ttu-id="65df1-492">Pressione **F5** para executar o aplicativo e navegue até o site no **emulador do Windows Phone**.</span><span class="sxs-lookup"><span data-stu-id="65df1-492">Press **F5** to run the application and browse the site in the **Windows Phone Emulator**.</span></span>
8. <span data-ttu-id="65df1-493">Abra um **simulador de iPhone** (consulte [Apêndice C](#AppendixC) para obter instruções sobre como instalar e configurar um simulador de iPhone) e navegue até o site muito.</span><span class="sxs-lookup"><span data-stu-id="65df1-493">Open an **iPhone simulator** (see [Appendix C](#AppendixC) for instructions on how to install and configure an iPhone simulator), and browse to the site too.</span></span> <span data-ttu-id="65df1-494">Observe que cada telefone está usando o modelo específico.</span><span class="sxs-lookup"><span data-stu-id="65df1-494">Notice that each phone is using the specific template.</span></span>

    ![Using-different-views-for-each-mobile-device2](whats-new-in-aspnet-mvc-4/_static/image35.png)

    <span data-ttu-id="65df1-496">*Usando modos de exibição diferentes para cada dispositivo móvel*</span><span class="sxs-lookup"><span data-stu-id="65df1-496">*Using different views for each mobile device*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Using_Asynchronous_Controllers"></a>
### <a name="exercise-4-using-asynchronous-controllers"></a><span data-ttu-id="65df1-497">Exercício 4: Usando controladores assíncronos</span><span class="sxs-lookup"><span data-stu-id="65df1-497">Exercise 4: Using Asynchronous Controllers</span></span>

<span data-ttu-id="65df1-498">Microsoft .NET Framework 4.5 apresenta novos recursos de idioma em c# e Visual Basic para fornecer uma nova base de assincronia na programação .NET.</span><span class="sxs-lookup"><span data-stu-id="65df1-498">Microsoft .NET Framework 4.5 introduces new language features in C# and Visual Basic to provide a new foundation for asynchrony in .NET programming.</span></span> <span data-ttu-id="65df1-499">Essa nova base torna programação assíncrona semelhantes e sobre tão simples quanto - programação síncrona.</span><span class="sxs-lookup"><span data-stu-id="65df1-499">This new foundation makes asynchronous programming similar to - and about as straightforward as - synchronous programming.</span></span> <span data-ttu-id="65df1-500">Agora é possível gravar os métodos de ação assíncrono no ASP.NET MVC 4 usando o **AsyncController** classe.</span><span class="sxs-lookup"><span data-stu-id="65df1-500">You are now able to write asynchronous action methods in ASP.NET MVC 4 by using the **AsyncController** class.</span></span> <span data-ttu-id="65df1-501">Você pode usar métodos de ação assíncrono de longa duração, CPU não associado a solicitações.</span><span class="sxs-lookup"><span data-stu-id="65df1-501">You can use asynchronous action methods for long-running, non-CPU bound requests.</span></span> <span data-ttu-id="65df1-502">Isso evita que o servidor Web da execução de trabalho enquanto a solicitação está sendo processada de bloqueio.</span><span class="sxs-lookup"><span data-stu-id="65df1-502">This avoids blocking the Web server from performing work while the request is being processed.</span></span> <span data-ttu-id="65df1-503">A classe AsyncController normalmente é usada para chamadas de serviço Web de longa execução.</span><span class="sxs-lookup"><span data-stu-id="65df1-503">The AsyncController class is typically used for long-running Web service calls.</span></span>

<span data-ttu-id="65df1-504">Este exercício explica os conceitos básicos de uma operação assíncrona no ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="65df1-504">This exercise explains the basics of asynchronous operation in ASP.NET MVC 4.</span></span> <span data-ttu-id="65df1-505">Se você quiser se aprofundar mais, confira o seguinte artigo: [[https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)</span><span class="sxs-lookup"><span data-stu-id="65df1-505">If you want a deeper dive, you can check out the following article: [[https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)</span></span>

<a id="Task_1_-_Implementing_an_Asynchronous_Controller"></a>
#### <a name="task-1---implementing-an-asynchronous-controller"></a><span data-ttu-id="65df1-506">Tarefa 1: Implementando um controlador assíncrono</span><span class="sxs-lookup"><span data-stu-id="65df1-506">Task 1 - Implementing an Asynchronous Controller</span></span>

1. <span data-ttu-id="65df1-507">Abra o **começar** solução localizado em **fonte/Ex4-Async/Begin/** pasta.</span><span class="sxs-lookup"><span data-stu-id="65df1-507">Open the **Begin** solution located at **Source/Ex4-Async/Begin/** folder.</span></span> <span data-ttu-id="65df1-508">Caso contrário, você pode continuar usando o **final** solução obtido executando o exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="65df1-508">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="65df1-509">Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="65df1-509">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="65df1-510">Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="65df1-510">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="65df1-511">No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="65df1-511">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="65df1-512">Por fim, compile a solução clicando **criar** | **compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="65df1-512">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="65df1-513">Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="65df1-513">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="65df1-514">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="65df1-514">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="65df1-515">É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.</span><span class="sxs-lookup"><span data-stu-id="65df1-515">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="65df1-516">Abra o **HomeController** classe o **controladores** pasta.</span><span class="sxs-lookup"><span data-stu-id="65df1-516">Open the **HomeController.cs** class from the **Controllers** folder.</span></span>
3. <span data-ttu-id="65df1-517">Adicione o seguinte usando a instrução.</span><span class="sxs-lookup"><span data-stu-id="65df1-517">Add the following using statement.</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample18.cs)]
~~~
4. <span data-ttu-id="65df1-518">Atualização de **HomeController** classe para herdar de **AsyncController**.</span><span class="sxs-lookup"><span data-stu-id="65df1-518">Update the **HomeController** class to inherit from **AsyncController**.</span></span> <span data-ttu-id="65df1-519">Controladores que derivam de AsyncController habilitar o ASP.NET processar solicitações assíncronas e ainda pode métodos de ação síncrono do serviço.</span><span class="sxs-lookup"><span data-stu-id="65df1-519">Controllers that derive from AsyncController enable ASP.NET to process asynchronous requests, and they can still service synchronous action methods.</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample19.cs)]
~~~
5. <span data-ttu-id="65df1-520">Adicionar o **async** palavra-chave para o **índice** método e o tipo de retorno **tarefa&lt;ActionResult&gt;**.</span><span class="sxs-lookup"><span data-stu-id="65df1-520">Add the **async** keyword to the **Index** method and make it return the type **Task&lt;ActionResult&gt;**.</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample20.cs)]

> [!NOTE]
> The **async** keyword is one of the new keywords the .NET Framework 4.5 provides; it tells the compiler that this method contains asynchronous code. A **Task** object represents an asynchronous operation that may complete at some point in the future.
~~~
6. <span data-ttu-id="65df1-521">Substitua o **cliente. GetAsync()** chamada com a versão assíncrona completa usando palavra-chave await conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="65df1-521">Replace the **client.GetAsync()** call with the full async version using await keyword as shown below.</span></span>

    <span data-ttu-id="65df1-522">(Código de trecho - *GetAsync do ASP.NET MVC 4 laboratório - Ex04 -*)</span><span class="sxs-lookup"><span data-stu-id="65df1-522">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - GetAsync*)</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample21.cs)]

> [!NOTE]
> In the previous version, you were using the **Result** property from the **Task** object to block the thread until the result is returned (sync version).
> 
> Adding the **await** keyword tells the compiler to asynchronously wait for the task returned from the method call. This means that the rest of the code will be executed as a callback only after the awaited method completes. Another thing to notice is that you do not need to change your try-catch block in order to make this work: the exceptions that happen in background or in foreground will still be caught without any extra work using a handler provided by the framework.
~~~
7. <span data-ttu-id="65df1-523">Alterar o código para continuar com a implementação assíncrona, substituindo as linhas com o novo código, conforme mostrado abaixo</span><span class="sxs-lookup"><span data-stu-id="65df1-523">Change the code to continue with the asynchronous implementation by replacing the lines with the new code as shown below</span></span>

    <span data-ttu-id="65df1-524">(Código de trecho - *ReadAsStringAsync do ASP.NET MVC 4 laboratório - Ex04 -*)</span><span class="sxs-lookup"><span data-stu-id="65df1-524">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - ReadAsStringAsync*)</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample22.cs)]
~~~
8. <span data-ttu-id="65df1-525">Execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="65df1-525">Run the application.</span></span> <span data-ttu-id="65df1-526">Você não observará nenhuma alteração principal, mas seu código não impedirá que um thread do pool de threads, tornando um melhor uso dos recursos do servidor e melhorar o desempenho.</span><span class="sxs-lookup"><span data-stu-id="65df1-526">You will notice no major changes, but your code will not block a thread from the thread pool making a better usage of the server resources and improving performance.</span></span>

    > [!NOTE]
    > <span data-ttu-id="65df1-527">Você pode aprender mais sobre os novos recursos de programação assíncronos no laboratório &quot; **programação assíncrona no .NET 4.5 com c# e Visual Basic** &quot; incluído no Kit de treinamento do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="65df1-527">You can learn more about the new asynchronous programming features in the lab &quot;**Asynchronous Programming in .NET 4.5 with C# and Visual Basic**&quot; included in the Visual Studio Training Kit.</span></span>

<a id="Task_2_-_Handling_Time-Outs_with_Cancellation_Tokens"></a>
#### <a name="task-2---handling-time-outs-with-cancellation-tokens"></a><span data-ttu-id="65df1-528">Tarefa 2 - tempos limite de tratamento com Tokens de cancelamento</span><span class="sxs-lookup"><span data-stu-id="65df1-528">Task 2 - Handling Time-Outs with Cancellation Tokens</span></span>

<span data-ttu-id="65df1-529">Métodos de ação assíncrono que retornam instâncias de tarefa também podem dar suporte a tempos limite.</span><span class="sxs-lookup"><span data-stu-id="65df1-529">Asynchronous action methods that return Task instances can also support time-outs.</span></span> <span data-ttu-id="65df1-530">Nesta tarefa, você atualizará o código do método de índice para lidar com um cenário de tempo limite usando um token de cancelamento.</span><span class="sxs-lookup"><span data-stu-id="65df1-530">In this task, you will update the Index method code to handle a time-out scenario using a cancellation token.</span></span>

1. <span data-ttu-id="65df1-531">Volte para o Visual Studio e pressione **SHIFT + F5** para parar a depuração.</span><span class="sxs-lookup"><span data-stu-id="65df1-531">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>
2. <span data-ttu-id="65df1-532">Adicione o seguinte usando a instrução de **HomeController** arquivo.</span><span class="sxs-lookup"><span data-stu-id="65df1-532">Add the following using statement to the **HomeController.cs** file.</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample23.cs)]
~~~
3. <span data-ttu-id="65df1-533">Atualizar a ação de índice para receber um **CancellationToken** argumento.</span><span class="sxs-lookup"><span data-stu-id="65df1-533">Update the Index action to receive a **CancellationToken** argument.</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample24.cs)]
~~~
4. <span data-ttu-id="65df1-534">Atualização de **GetAsync** chamada para passar o token de cancelamento.</span><span class="sxs-lookup"><span data-stu-id="65df1-534">Update the **GetAsync** call to pass the cancellation token.</span></span>

    <span data-ttu-id="65df1-535">(Código de trecho - *ASP.NET MVC 4 laboratório - Ex04 - SendAsync com CancellationToken*)</span><span class="sxs-lookup"><span data-stu-id="65df1-535">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - SendAsync with CancellationToken*)</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample25.cs)]
~~~
5. <span data-ttu-id="65df1-536">Decore o *índice* método com um **AsyncTimeout** atributo definido como 500 milissegundos e um **HandleError** atributo configurado para tratar  **TaskCanceledException** redirecionando para um **TimedOut** exibição.</span><span class="sxs-lookup"><span data-stu-id="65df1-536">Decorate the *Index* method with an **AsyncTimeout** attribute set to 500 milliseconds and a **HandleError** attribute configured to handle **TaskCanceledException** by redirecting to a **TimedOut** view.</span></span>

    <span data-ttu-id="65df1-537">(Código de trecho - *atributos do ASP.NET MVC 4 laboratório - Ex04 -*)</span><span class="sxs-lookup"><span data-stu-id="65df1-537">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - Attributes*)</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample26.cs)]
~~~
6. <span data-ttu-id="65df1-538">Abra o **PhotoController** classe e atualizar o **galeria** método para atrasar os milissegundos de execução 1000 (1 segundo) para simular uma tarefa demorada.</span><span class="sxs-lookup"><span data-stu-id="65df1-538">Open the **PhotoController** class and update the **Gallery** method to delay the execution 1000 miliseconds (1 second) to simulate a long running task.</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample27.cs)]
~~~
7. <span data-ttu-id="65df1-539">Abra o **Web. config** de arquivos e habilitar os erros personalizados, adicionando o seguinte elemento.</span><span class="sxs-lookup"><span data-stu-id="65df1-539">Open the **Web.config** file and enable custom errors by adding the following element.</span></span>


~~~
[!code-xml[Main](whats-new-in-aspnet-mvc-4/samples/sample28.xml)]
~~~
8. <span data-ttu-id="65df1-540">Criar uma nova exibição em **exibições \ compartilhadas** chamado **TimedOut** e use o layout padrão.</span><span class="sxs-lookup"><span data-stu-id="65df1-540">Create a new view in **Views\Shared** named **TimedOut** and use the default layout.</span></span> <span data-ttu-id="65df1-541">No Gerenciador de soluções, clique com botão direito do **exibições \ compartilhadas** pasta e selecione **adicionar | Exibição**.</span><span class="sxs-lookup"><span data-stu-id="65df1-541">In the Solution Explorer, right-click the **Views\Shared** folder and select **Add | View**.</span></span>

    <span data-ttu-id="65df1-542">![Usando modos de exibição diferentes para cada dispositivo móvel](whats-new-in-aspnet-mvc-4/_static/image36.png "usando diferentes modos de exibição para cada dispositivo móvel")</span><span class="sxs-lookup"><span data-stu-id="65df1-542">![Using different views for each mobile device](whats-new-in-aspnet-mvc-4/_static/image36.png "Using different views for each mobile device")</span></span>

    <span data-ttu-id="65df1-543">*Usando modos de exibição diferentes para cada dispositivo móvel*</span><span class="sxs-lookup"><span data-stu-id="65df1-543">*Using different views for each mobile device*</span></span>
9. <span data-ttu-id="65df1-544">Atualização de **TimedOut** exibir conteúdo, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="65df1-544">Update the **TimedOut** view content as shown below.</span></span>


~~~
[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample29.cshtml)]
~~~
10. <span data-ttu-id="65df1-545">Execute o aplicativo e navegue até a URL raiz.</span><span class="sxs-lookup"><span data-stu-id="65df1-545">Run the application and navigate to the root URL.</span></span> <span data-ttu-id="65df1-546">Como você adicionou um **Sleep** de 1000 milissegundos, você obterá um erro de tempo limite, gerado pelo **AsyncTimeout** de atributos e catch pelo **HandleError** atributo.</span><span class="sxs-lookup"><span data-stu-id="65df1-546">As you have added a **Thread.Sleep** of 1000 milliseconds, you will get a time-out error, generated by the **AsyncTimeout** attribute and catch by the **HandleError** attribute.</span></span>

    <span data-ttu-id="65df1-547">![Exceção de tempo limite tratada](whats-new-in-aspnet-mvc-4/_static/image37.png "tratada de exceção de tempo limite")</span><span class="sxs-lookup"><span data-stu-id="65df1-547">![Time-out exception handled](whats-new-in-aspnet-mvc-4/_static/image37.png "Time-out exception handled")</span></span>

    <span data-ttu-id="65df1-548">*Exceção de tempo limite tratada*</span><span class="sxs-lookup"><span data-stu-id="65df1-548">*Time-out exception handled*</span></span>

> [!NOTE]
> <span data-ttu-id="65df1-549">Além disso, você pode implantar esse aplicativo para Windows Azure Web Sites a seguir [apêndice d: publicação um aplicativo ASP.NET MVC 4 usando a implantação da Web](#AppendixD).</span><span class="sxs-lookup"><span data-stu-id="65df1-549">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix D: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixD).</span></span>


<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="65df1-550">Resumo</span><span class="sxs-lookup"><span data-stu-id="65df1-550">Summary</span></span>

<span data-ttu-id="65df1-551">Neste prático laboratório, você tenha observado alguns dos novos recursos residente no ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="65df1-551">In this hands-on-lab, you've observed some of the new features resident in ASP.NET MVC 4.</span></span> <span data-ttu-id="65df1-552">Foram discutidos os seguintes conceitos:</span><span class="sxs-lookup"><span data-stu-id="65df1-552">The following concepts have been discussed:</span></span>

- <span data-ttu-id="65df1-553">Aproveitar os aprimoramentos para o ASP.NET MVC projeto modelos, incluindo o novo modelo de projeto de aplicativo móvel</span><span class="sxs-lookup"><span data-stu-id="65df1-553">Take advantage of the enhancements to the ASP.NET MVC project templates-including the new mobile application project template</span></span>
- <span data-ttu-id="65df1-554">Use o atributo de visor do HTML5 e consultas de mídia CSS para melhorar a exibição em dispositivos móveis</span><span class="sxs-lookup"><span data-stu-id="65df1-554">Use the HTML5 viewport attribute and CSS media queries to improve the display on mobile devices</span></span>
- <span data-ttu-id="65df1-555">Use o jQuery Mobile para aprimoramentos progressivos e para a criação de interface da web otimizada para toque</span><span class="sxs-lookup"><span data-stu-id="65df1-555">Use jQuery Mobile for progressive enhancements and for building touch-optimized web UI</span></span>
- <span data-ttu-id="65df1-556">Criar exibições específicas para dispositivos móveis</span><span class="sxs-lookup"><span data-stu-id="65df1-556">Create mobile-specific views</span></span>
- <span data-ttu-id="65df1-557">Use o componente de alternância de exibição para alternar entre modos de exibição móveis e área de trabalho do aplicativo</span><span class="sxs-lookup"><span data-stu-id="65df1-557">Use the view-switcher component to toggle between mobile and desktop views in the application</span></span>
- <span data-ttu-id="65df1-558">Criar controladores assíncronos usando o suporte da tarefa</span><span class="sxs-lookup"><span data-stu-id="65df1-558">Create asynchronous controllers using task support</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a><span data-ttu-id="65df1-559">Apêndice a: usar trechos de código</span><span class="sxs-lookup"><span data-stu-id="65df1-559">Appendix A: Using Code Snippets</span></span>

<span data-ttu-id="65df1-560">Com trechos de código, você tem todo o código que é necessário ao seu alcance.</span><span class="sxs-lookup"><span data-stu-id="65df1-560">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="65df1-561">O documento de laboratório informará exatamente quando você pode usá-los, conforme mostrado na figura a seguir.</span><span class="sxs-lookup"><span data-stu-id="65df1-561">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="65df1-562">![Usando trechos de código do Visual Studio para inserir o código no seu projeto](whats-new-in-aspnet-mvc-4/_static/image38.png "trechos de código com o Visual Studio para inserir código em seu projeto")</span><span class="sxs-lookup"><span data-stu-id="65df1-562">![Using Visual Studio code snippets to insert code into your project](whats-new-in-aspnet-mvc-4/_static/image38.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="65df1-563">*Usando trechos de código do Visual Studio para inserir código em seu projeto*</span><span class="sxs-lookup"><span data-stu-id="65df1-563">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="65df1-564">***Para adicionar um trecho de código usando o teclado (apenas c#)***</span><span class="sxs-lookup"><span data-stu-id="65df1-564">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="65df1-565">Posicione o cursor onde você deseja inserir o código.</span><span class="sxs-lookup"><span data-stu-id="65df1-565">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="65df1-566">Comece a digitar o nome do trecho (sem espaços ou hifens).</span><span class="sxs-lookup"><span data-stu-id="65df1-566">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="65df1-567">Observe como IntelliSense exibe os nomes dos trechos de código correspondentes.</span><span class="sxs-lookup"><span data-stu-id="65df1-567">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="65df1-568">Selecione o trecho correto (ou continue digitando até que o nome do trecho de código inteiro é selecionado).</span><span class="sxs-lookup"><span data-stu-id="65df1-568">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="65df1-569">Pressione a tecla Tab duas vezes para inserir o trecho de código no local do cursor.</span><span class="sxs-lookup"><span data-stu-id="65df1-569">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="65df1-570">![Comece a digitar o nome do fragmento](whats-new-in-aspnet-mvc-4/_static/image39.png "comece a digitar o nome do trecho de código")</span><span class="sxs-lookup"><span data-stu-id="65df1-570">![Start typing the snippet name](whats-new-in-aspnet-mvc-4/_static/image39.png "Start typing the snippet name")</span></span>

<span data-ttu-id="65df1-571">*Comece a digitar o nome do trecho de código*</span><span class="sxs-lookup"><span data-stu-id="65df1-571">*Start typing the snippet name*</span></span>

<span data-ttu-id="65df1-572">![Pressione Tab para selecionar o trecho de código realçado](whats-new-in-aspnet-mvc-4/_static/image40.png "pressione Tab para selecionar o trecho de código realçado")</span><span class="sxs-lookup"><span data-stu-id="65df1-572">![Press Tab to select the highlighted snippet](whats-new-in-aspnet-mvc-4/_static/image40.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="65df1-573">*Pressione Tab para selecionar o trecho de código realçado*</span><span class="sxs-lookup"><span data-stu-id="65df1-573">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="65df1-574">![Pressione Tab novamente e o trecho expandirá](whats-new-in-aspnet-mvc-4/_static/image41.png "expandirá pressione Tab novamente e o trecho de código")</span><span class="sxs-lookup"><span data-stu-id="65df1-574">![Press Tab again and the snippet will expand](whats-new-in-aspnet-mvc-4/_static/image41.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="65df1-575">*Pressione Tab novamente e o trecho de código serão expandida*</span><span class="sxs-lookup"><span data-stu-id="65df1-575">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="65df1-576">***Para adicionar um trecho de código usando o mouse (c#, Visual Basic e XML)***</span><span class="sxs-lookup"><span data-stu-id="65df1-576">***To add a code snippet using the mouse (C#, Visual Basic and XML)***</span></span>

1. <span data-ttu-id="65df1-577">Clique com botão direito em que você deseja inserir o trecho de código.</span><span class="sxs-lookup"><span data-stu-id="65df1-577">Right-click where you want to insert the code snippet.</span></span>
2. <span data-ttu-id="65df1-578">Selecione **Inserir trecho** seguido por **Meus trechos de código**.</span><span class="sxs-lookup"><span data-stu-id="65df1-578">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
3. <span data-ttu-id="65df1-579">Selecione o trecho relevante na lista, clicando nele.</span><span class="sxs-lookup"><span data-stu-id="65df1-579">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="65df1-580">![Com o botão direito em que você deseja inserir o trecho de código e selecione Inserir trecho](whats-new-in-aspnet-mvc-4/_static/image42.png "com o botão direito em que você deseja inserir o trecho de código e selecione Inserir trecho")</span><span class="sxs-lookup"><span data-stu-id="65df1-580">![Right-click where you want to insert the code snippet and select Insert Snippet](whats-new-in-aspnet-mvc-4/_static/image42.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="65df1-581">*Clique com botão direito em que você deseja inserir o trecho de código e selecione Inserir trecho*</span><span class="sxs-lookup"><span data-stu-id="65df1-581">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="65df1-582">![Selecione o trecho relevante na lista, clicando nele](whats-new-in-aspnet-mvc-4/_static/image43.png "escolher o trecho relevante na lista, clicando nele")</span><span class="sxs-lookup"><span data-stu-id="65df1-582">![Pick the relevant snippet from the list, by clicking on it](whats-new-in-aspnet-mvc-4/_static/image43.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="65df1-583">*Selecione o trecho relevante na lista, clicando nele*</span><span class="sxs-lookup"><span data-stu-id="65df1-583">*Pick the relevant snippet from the list, by clicking on it*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="65df1-584">Apêndice b: instalar o Visual Studio Express 2012 para Web</span><span class="sxs-lookup"><span data-stu-id="65df1-584">Appendix B: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="65df1-585">Você pode instalar **Microsoft Visual Studio Express 2012 para Web** ou outro &quot;Express&quot; versão usando o **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="65df1-585">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="65df1-586">As instruções a seguir guiá-lo pelas etapas necessárias para instalar o *Visual studio Express 2012 para Web* usando *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="65df1-586">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="65df1-587">Vá para [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="65df1-587">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="65df1-588">Como alternativa, se você já tiver instalado o Web Platform Installer, você pode abri-la e procure o produto &quot; <em>Visual Studio Express 2012 para Web com o SDK do Windows Azure</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="65df1-588">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="65df1-589">Clique em **instalar agora**.</span><span class="sxs-lookup"><span data-stu-id="65df1-589">Click on **Install Now**.</span></span> <span data-ttu-id="65df1-590">Se você não tem **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.</span><span class="sxs-lookup"><span data-stu-id="65df1-590">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="65df1-591">Uma vez **Web Platform Installer** é aberto, clique em **instalar** para iniciar a instalação.</span><span class="sxs-lookup"><span data-stu-id="65df1-591">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="65df1-592">![Instalar Visual Studio Express](whats-new-in-aspnet-mvc-4/_static/image44.png "instalar Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="65df1-592">![Install Visual Studio Express](whats-new-in-aspnet-mvc-4/_static/image44.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="65df1-593">*Instalar Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="65df1-593">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="65df1-594">Leia os termos e licenças de todos os produtos e clique em **aceito** para continuar.</span><span class="sxs-lookup"><span data-stu-id="65df1-594">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Aceitar os termos de licença](whats-new-in-aspnet-mvc-4/_static/image45.png)

    <span data-ttu-id="65df1-596">*Aceitar os termos de licença*</span><span class="sxs-lookup"><span data-stu-id="65df1-596">*Accepting the license terms*</span></span>
5. <span data-ttu-id="65df1-597">Aguarde até que o processo de download e instalação seja concluída.</span><span class="sxs-lookup"><span data-stu-id="65df1-597">Wait until the downloading and installation process completes.</span></span>

    ![Progresso da instalação](whats-new-in-aspnet-mvc-4/_static/image46.png)

    <span data-ttu-id="65df1-599">*Progresso da instalação*</span><span class="sxs-lookup"><span data-stu-id="65df1-599">*Installation progress*</span></span>
6. <span data-ttu-id="65df1-600">Quando a instalação for concluída, clique em **concluir**.</span><span class="sxs-lookup"><span data-stu-id="65df1-600">When the installation completes, click **Finish**.</span></span>

    ![Instalação concluída](whats-new-in-aspnet-mvc-4/_static/image47.png)

    <span data-ttu-id="65df1-602">*Instalação concluída*</span><span class="sxs-lookup"><span data-stu-id="65df1-602">*Installation completed*</span></span>
7. <span data-ttu-id="65df1-603">Clique em **saída** para fechar o Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="65df1-603">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="65df1-604">Para abrir o Visual Studio Express para Web, vá para o **iniciar** tela e comece a escrever &quot; **VS Express**&quot;, em seguida, clique no **VS Express para Web** lado a lado.</span><span class="sxs-lookup"><span data-stu-id="65df1-604">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express para o bloco de Web](whats-new-in-aspnet-mvc-4/_static/image48.png)

    <span data-ttu-id="65df1-606">*VS Express para o bloco de Web*</span><span class="sxs-lookup"><span data-stu-id="65df1-606">*VS Express for Web tile*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Installing_WebMatrix_2_and_iPhone_Simulator"></a>
## <a name="appendix-c-installing-webmatrix-2-and-iphone-simulator"></a><span data-ttu-id="65df1-607">Apêndice c: Instalando o WebMatrix 2 e o simulador de iPhone</span><span class="sxs-lookup"><span data-stu-id="65df1-607">Appendix C: Installing WebMatrix 2 and iPhone Simulator</span></span>

<span data-ttu-id="65df1-608">Para executar o seu site em um dispositivo simulado iPhone, você pode usar a extensão do WebMatrix &quot;Electric simulador Mobile para iPhone&quot;.</span><span class="sxs-lookup"><span data-stu-id="65df1-608">To run your site in a simulated iPhone device you can use the WebMatrix extension &quot;Electric Mobile Simulator for the iPhone&quot;.</span></span> <span data-ttu-id="65df1-609">Além disso, você pode configurar a mesma extensão para executar o simulador do Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="65df1-609">Also, you can configure the same extension to run the simulator from Visual Studio 2012.</span></span>

<a id="ApxCTask1"></a>

<a id="Task_1_-_Installing_WebMatrix_2"></a>
#### <a name="task-1---installing-webmatrix-2"></a><span data-ttu-id="65df1-610">Tarefa 1: instalar o WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="65df1-610">Task 1 - Installing WebMatrix 2</span></span>

1. <span data-ttu-id="65df1-611">Vá para [ [ https://go.microsoft.com/?linkid=9809776 ](https://go.microsoft.com/?linkid=9809776) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="65df1-611">Go to [[https://go.microsoft.com/?linkid=9809776](https://go.microsoft.com/?linkid=9809776)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="65df1-612">Como alternativa, se você já tiver instalado o Web Platform Installer, você pode abri-la e procure o produto &quot; <em>o WebMatrix 2</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="65df1-612">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>WebMatrix 2</em>&quot;.</span></span>
2. <span data-ttu-id="65df1-613">Clique em **instalar agora**.</span><span class="sxs-lookup"><span data-stu-id="65df1-613">Click on **Install Now**.</span></span> <span data-ttu-id="65df1-614">Se você não tem **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.</span><span class="sxs-lookup"><span data-stu-id="65df1-614">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="65df1-615">Uma vez **Web Platform Installer** é aberto, clique em **instalar** para iniciar a instalação.</span><span class="sxs-lookup"><span data-stu-id="65df1-615">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="65df1-616">![Instalar o WebMatrix 2](whats-new-in-aspnet-mvc-4/_static/image49.png "instalar o WebMatrix 2")</span><span class="sxs-lookup"><span data-stu-id="65df1-616">![Install WebMatrix 2](whats-new-in-aspnet-mvc-4/_static/image49.png "Install WebMatrix 2")</span></span>

    <span data-ttu-id="65df1-617">*Instalar o WebMatrix 2*</span><span class="sxs-lookup"><span data-stu-id="65df1-617">*Install WebMatrix 2*</span></span>
4. <span data-ttu-id="65df1-618">Leia os termos e licenças de todos os produtos e clique em **aceito** para continuar.</span><span class="sxs-lookup"><span data-stu-id="65df1-618">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    <span data-ttu-id="65df1-619">![Aceitar os termos de licença](whats-new-in-aspnet-mvc-4/_static/image50.png "aceitar os termos de licença")</span><span class="sxs-lookup"><span data-stu-id="65df1-619">![Accepting the license terms](whats-new-in-aspnet-mvc-4/_static/image50.png "Accepting the license terms")</span></span>

    <span data-ttu-id="65df1-620">*Aceitar os termos de licença*</span><span class="sxs-lookup"><span data-stu-id="65df1-620">*Accepting the license terms*</span></span>
5. <span data-ttu-id="65df1-621">Aguarde até que o processo de download e instalação seja concluída.</span><span class="sxs-lookup"><span data-stu-id="65df1-621">Wait until the downloading and installation process completes.</span></span>

    <span data-ttu-id="65df1-622">![Progresso da instalação](whats-new-in-aspnet-mvc-4/_static/image51.png "progresso da instalação")</span><span class="sxs-lookup"><span data-stu-id="65df1-622">![Installation progress](whats-new-in-aspnet-mvc-4/_static/image51.png "Installation progress")</span></span>

    <span data-ttu-id="65df1-623">*Progresso da instalação*</span><span class="sxs-lookup"><span data-stu-id="65df1-623">*Installation progress*</span></span>
6. <span data-ttu-id="65df1-624">Quando a instalação for concluída, clique em **concluir**.</span><span class="sxs-lookup"><span data-stu-id="65df1-624">When the installation completes, click **Finish**.</span></span>

    <span data-ttu-id="65df1-625">![Instalação concluída](whats-new-in-aspnet-mvc-4/_static/image52.png "instalação concluída")</span><span class="sxs-lookup"><span data-stu-id="65df1-625">![Installation completed](whats-new-in-aspnet-mvc-4/_static/image52.png "Installation completed")</span></span>

    <span data-ttu-id="65df1-626">*Instalação concluída*</span><span class="sxs-lookup"><span data-stu-id="65df1-626">*Installation completed*</span></span>
7. <span data-ttu-id="65df1-627">Clique em **saída** para fechar o Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="65df1-627">Click **Exit** to close Web Platform Installer.</span></span>

<a id="ApxCTask2"></a>

<a id="Task_2_-_Installing_the_iPhone_Simulator_Extension"></a>
#### <a name="task-2---installing-the-iphone-simulator-extension"></a><span data-ttu-id="65df1-628">Tarefa 2 - instalar o extensão do simulador de iPhone</span><span class="sxs-lookup"><span data-stu-id="65df1-628">Task 2 - Installing the iPhone Simulator Extension</span></span>

1. <span data-ttu-id="65df1-629">Executar **WebMatrix** e abrir qualquer site existente ou criar um novo.</span><span class="sxs-lookup"><span data-stu-id="65df1-629">Run **WebMatrix** and open any existing Web site or create a new one.</span></span>
2. <span data-ttu-id="65df1-630">Clique o **executar** botão do **início** de faixa de opções e selecione **adicionar novo**.</span><span class="sxs-lookup"><span data-stu-id="65df1-630">Click the **Run** button from the **Home** ribbon and select **Add new**.</span></span>

    <span data-ttu-id="65df1-631">![Adicionar nova extensão do WebMatrix](whats-new-in-aspnet-mvc-4/_static/image53.png "adicionar nova extensão do WebMatrix")</span><span class="sxs-lookup"><span data-stu-id="65df1-631">![Adding new WebMatrix extension](whats-new-in-aspnet-mvc-4/_static/image53.png "Adding new WebMatrix extension")</span></span>

    <span data-ttu-id="65df1-632">*Adicionar nova extensão do WebMatrix*</span><span class="sxs-lookup"><span data-stu-id="65df1-632">*Adding new WebMatrix extension*</span></span>
3. <span data-ttu-id="65df1-633">Selecione **iPhone simulador** e clique em **instalar**.</span><span class="sxs-lookup"><span data-stu-id="65df1-633">Select **iPhone Simulator** and click **Install**.</span></span>

    <span data-ttu-id="65df1-634">![Procurando extensões WebMatrix](whats-new-in-aspnet-mvc-4/_static/image54.png "extensões de pesquisa no WebMatrix")</span><span class="sxs-lookup"><span data-stu-id="65df1-634">![Browsing WebMatrix extensions](whats-new-in-aspnet-mvc-4/_static/image54.png "Browsing WebMatrix extensions")</span></span>

    <span data-ttu-id="65df1-635">*Procurando extensões do WebMatrix*</span><span class="sxs-lookup"><span data-stu-id="65df1-635">*Browsing WebMatrix extensions*</span></span>
4. <span data-ttu-id="65df1-636">Nos detalhes do pacote, clique em **instalar** para continuar com a instalação da extensão.</span><span class="sxs-lookup"><span data-stu-id="65df1-636">In the package details, click **Install** to continue with the extension installation.</span></span>

    <span data-ttu-id="65df1-637">![extensão do simulador de iPhone](whats-new-in-aspnet-mvc-4/_static/image55.png "extensão simulador de iPhone")</span><span class="sxs-lookup"><span data-stu-id="65df1-637">![iPhone Simulator extension](whats-new-in-aspnet-mvc-4/_static/image55.png "iPhone Simulator extension")</span></span>

    <span data-ttu-id="65df1-638">*extensão do simulador de iPhone*</span><span class="sxs-lookup"><span data-stu-id="65df1-638">*iPhone Simulator extension*</span></span>
5. <span data-ttu-id="65df1-639">Leia e aceite o EULA de extensão.</span><span class="sxs-lookup"><span data-stu-id="65df1-639">Read and accept the extension EULA.</span></span>

    <span data-ttu-id="65df1-640">![Extensão do WebMatrix EULA](whats-new-in-aspnet-mvc-4/_static/image56.png "EULA de extensão do WebMatrix")</span><span class="sxs-lookup"><span data-stu-id="65df1-640">![WebMatrix extension EULA](whats-new-in-aspnet-mvc-4/_static/image56.png "WebMatrix extension EULA")</span></span>

    <span data-ttu-id="65df1-641">*Extensão do WebMatrix EULA*</span><span class="sxs-lookup"><span data-stu-id="65df1-641">*WebMatrix extension EULA*</span></span>
6. <span data-ttu-id="65df1-642">Agora, você pode executar o site do WebMatrix usando o opção de simulador de iPhone.</span><span class="sxs-lookup"><span data-stu-id="65df1-642">Now, you can run your Web site from WebMatrix using the iPhone Simulator option.</span></span>

    <span data-ttu-id="65df1-643">![Executar usando iPhone](whats-new-in-aspnet-mvc-4/_static/image57.png "executados usando iPhone")</span><span class="sxs-lookup"><span data-stu-id="65df1-643">![Run using iPhone](whats-new-in-aspnet-mvc-4/_static/image57.png "Run using iPhone")</span></span>

    <span data-ttu-id="65df1-644">*Executar usando iPhone*</span><span class="sxs-lookup"><span data-stu-id="65df1-644">*Run using iPhone*</span></span>

<a id="ApxCTask3"></a>

<a id="Task_3_-_Configuring_Visual_Studio_2012_to_run_iPhone_Simulator"></a>
#### <a name="task-3---configuring-visual-studio-2012-to-run-iphone-simulator"></a><span data-ttu-id="65df1-645">Tarefa 3: configurar o Visual Studio 2012 para executar o simulador de iPhone</span><span class="sxs-lookup"><span data-stu-id="65df1-645">Task 3 - Configuring Visual Studio 2012 to run iPhone Simulator</span></span>

1. <span data-ttu-id="65df1-646">Abra **Visual Studio 2012** e abrir qualquer site da Web ou criar um novo projeto.</span><span class="sxs-lookup"><span data-stu-id="65df1-646">Open **Visual Studio 2012** and open any Web site or create a new project.</span></span>
2. <span data-ttu-id="65df1-647">Clique na seta para baixo no botão Executar e selecione **procurar com**.</span><span class="sxs-lookup"><span data-stu-id="65df1-647">Click the down arrow from the Run button and select **Browse with**.</span></span>

    <span data-ttu-id="65df1-648">![Procurar com](whats-new-in-aspnet-mvc-4/_static/image58.png "procurar com")</span><span class="sxs-lookup"><span data-stu-id="65df1-648">![Browse with](whats-new-in-aspnet-mvc-4/_static/image58.png "Browse with")</span></span>

    <span data-ttu-id="65df1-649">*Procurar com*</span><span class="sxs-lookup"><span data-stu-id="65df1-649">*Browse with*</span></span>
3. <span data-ttu-id="65df1-650">No &quot;procurar com&quot; caixa de diálogo, clique em **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="65df1-650">In the &quot;Browse With&quot; dialog, click **Add**.</span></span>
4. <span data-ttu-id="65df1-651">No &quot;adicionar programa&quot; caixa de diálogo, use os seguintes valores:</span><span class="sxs-lookup"><span data-stu-id="65df1-651">In the &quot;Add Program&quot; dialog, use the following values:</span></span>

   - <span data-ttu-id="65df1-652"><strong>Programa</strong>: C:\Users\*{CurrentUser}<em>\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe * (adequadamente o caminho de atualização)</em></span><span class="sxs-lookup"><span data-stu-id="65df1-652"><strong>Program</strong>: C:\Users\*{CurrentUser}<em>\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe *(update the path accordingly)</em></span></span>
   - <span data-ttu-id="65df1-653">**Argumentos**: &quot;1&quot;</span><span class="sxs-lookup"><span data-stu-id="65df1-653">**Arguments**: &quot;1&quot;</span></span>
   - <span data-ttu-id="65df1-654">**Nome amigável**: simulador de iPhone</span><span class="sxs-lookup"><span data-stu-id="65df1-654">**Friendly name**: iPhone Simulator</span></span>

     <span data-ttu-id="65df1-655">![Adicionar programa](whats-new-in-aspnet-mvc-4/_static/image59.png "adicionar programa")</span><span class="sxs-lookup"><span data-stu-id="65df1-655">![Add program](whats-new-in-aspnet-mvc-4/_static/image59.png "Add program")</span></span>

     <span data-ttu-id="65df1-656">*Adicionar programa para procurar com*</span><span class="sxs-lookup"><span data-stu-id="65df1-656">*Add program to browse with*</span></span>
5. <span data-ttu-id="65df1-657">Clique em **Okey** e feche as caixas de diálogo.</span><span class="sxs-lookup"><span data-stu-id="65df1-657">Click **OK** and close the dialogs.</span></span>
6. <span data-ttu-id="65df1-658">Agora é possível executar seus aplicativos Web no iPhone simulador do Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="65df1-658">Now you are able to run your Web applications in the iPhone simulator from Visual Studio 2012.</span></span>

    <span data-ttu-id="65df1-659">![Procurar com iPhone simulador](whats-new-in-aspnet-mvc-4/_static/image60.png "procurar com simulador de iPhone")</span><span class="sxs-lookup"><span data-stu-id="65df1-659">![Browse with iPhone Simulator](whats-new-in-aspnet-mvc-4/_static/image60.png "Browse with iPhone Simulator")</span></span>

    <span data-ttu-id="65df1-660">*Procurar com simulador de iPhone*</span><span class="sxs-lookup"><span data-stu-id="65df1-660">*Browse with iPhone Simulator*</span></span>

<a id="AppendixD"></a>

<a id="Appendix_D_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-d-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="65df1-661">Apêndice d: publicar um aplicativo ASP.NET MVC 4 usando a implantação da Web</span><span class="sxs-lookup"><span data-stu-id="65df1-661">Appendix D: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="65df1-662">Este apêndice mostram como criar um novo site do Portal de gerenciamento do Windows Azure e publicar o aplicativo que você obteve seguindo o laboratório, aproveitando o recurso de publicação de implantação da Web fornecido pelo Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="65df1-662">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxDTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="65df1-663">Tarefa 1: criar um novo Site do Windows Azure Portal</span><span class="sxs-lookup"><span data-stu-id="65df1-663">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="65df1-664">Vá para o [Portal de gerenciamento do Windows Azure](https://manage.windowsazure.com/) e entre usando as credenciais da Microsoft associadas à sua assinatura.</span><span class="sxs-lookup"><span data-stu-id="65df1-664">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="65df1-665">Com o Windows Azure, você pode hospedar 10 Sites da Web ASP.NET gratuitamente e, em seguida, dimensione conforme seu tráfego cresce.</span><span class="sxs-lookup"><span data-stu-id="65df1-665">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="65df1-666">Você pode inscrever [aqui](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="65df1-666">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="65df1-667">![Faça logon no portal do Windows Azure](whats-new-in-aspnet-mvc-4/_static/image61.png "faça logon no portal do Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="65df1-667">![Log on to Windows Azure portal](whats-new-in-aspnet-mvc-4/_static/image61.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="65df1-668">*Faça logon no Portal de gerenciamento do Azure do Windows*</span><span class="sxs-lookup"><span data-stu-id="65df1-668">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="65df1-669">Clique em **novo** na barra de comandos.</span><span class="sxs-lookup"><span data-stu-id="65df1-669">Click **New** on the command bar.</span></span>

    <span data-ttu-id="65df1-670">![Criando um novo Site](whats-new-in-aspnet-mvc-4/_static/image62.png "criar um novo Site")</span><span class="sxs-lookup"><span data-stu-id="65df1-670">![Creating a new Web Site](whats-new-in-aspnet-mvc-4/_static/image62.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="65df1-671">*Criando um novo Site*</span><span class="sxs-lookup"><span data-stu-id="65df1-671">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="65df1-672">Clique em **de computação** | **Site da Web**.</span><span class="sxs-lookup"><span data-stu-id="65df1-672">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="65df1-673">Em seguida, selecione **criação rápida** opção.</span><span class="sxs-lookup"><span data-stu-id="65df1-673">Then select **Quick Create** option.</span></span> <span data-ttu-id="65df1-674">Forneça uma URL disponível para o novo site e clique em **Criar Site**.</span><span class="sxs-lookup"><span data-stu-id="65df1-674">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="65df1-675">Um Site do Windows Azure é o host para um aplicativo web em execução na nuvem que você pode controlar e gerenciar.</span><span class="sxs-lookup"><span data-stu-id="65df1-675">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="65df1-676">A opção criação rápida permite que você implante um aplicativo web concluído ao Site do Windows Azure de fora do portal.</span><span class="sxs-lookup"><span data-stu-id="65df1-676">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="65df1-677">Ele não inclui etapas para configurar um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="65df1-677">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="65df1-678">![Criando um novo Site usando a criação rápida](whats-new-in-aspnet-mvc-4/_static/image63.png "criar um novo Site usando a criação rápida")</span><span class="sxs-lookup"><span data-stu-id="65df1-678">![Creating a new Web Site using Quick Create](whats-new-in-aspnet-mvc-4/_static/image63.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="65df1-679">*Criando um novo Site usando a criação rápida*</span><span class="sxs-lookup"><span data-stu-id="65df1-679">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="65df1-680">Aguarde até que o novo **Site da Web** é criado.</span><span class="sxs-lookup"><span data-stu-id="65df1-680">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="65df1-681">Depois que o Site da Web é criado, clique no link sob a **URL** coluna.</span><span class="sxs-lookup"><span data-stu-id="65df1-681">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="65df1-682">Verifique se o novo Site da Web está funcionando.</span><span class="sxs-lookup"><span data-stu-id="65df1-682">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="65df1-683">![Navegando para o novo site](whats-new-in-aspnet-mvc-4/_static/image64.png "navegando para o novo site")</span><span class="sxs-lookup"><span data-stu-id="65df1-683">![Browsing to the new web site](whats-new-in-aspnet-mvc-4/_static/image64.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="65df1-684">*Navegando para o novo site*</span><span class="sxs-lookup"><span data-stu-id="65df1-684">*Browsing to the new web site*</span></span>

    <span data-ttu-id="65df1-685">![Site da Web em execução](whats-new-in-aspnet-mvc-4/_static/image65.png "site em execução")</span><span class="sxs-lookup"><span data-stu-id="65df1-685">![Web site running](whats-new-in-aspnet-mvc-4/_static/image65.png "Web site running")</span></span>

    <span data-ttu-id="65df1-686">*Site da Web em execução*</span><span class="sxs-lookup"><span data-stu-id="65df1-686">*Web site running*</span></span>
6. <span data-ttu-id="65df1-687">Acesse o portal e clique no nome do site sob o **nome** coluna para exibir as páginas de gerenciamento.</span><span class="sxs-lookup"><span data-stu-id="65df1-687">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="65df1-688">![Abrir as páginas de gerenciamento do site da web](whats-new-in-aspnet-mvc-4/_static/image66.png "abrir páginas de gerenciamento do site")</span><span class="sxs-lookup"><span data-stu-id="65df1-688">![Opening the web site management pages](whats-new-in-aspnet-mvc-4/_static/image66.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="65df1-689">*Abrir as páginas de gerenciamento do Site da Web*</span><span class="sxs-lookup"><span data-stu-id="65df1-689">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="65df1-690">No **painel** página no **visão rápida** seção, clique o **baixar perfil de publicação** link.</span><span class="sxs-lookup"><span data-stu-id="65df1-690">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="65df1-691">O *perfil de publicação* contém todas as informações necessárias para publicar um aplicativo web para um site do Windows Azure para cada método de publicação habilitado.</span><span class="sxs-lookup"><span data-stu-id="65df1-691">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="65df1-692">O perfil de publicação contém as URLs, as credenciais do usuário e cadeias de caracteres de banco de dados necessárias para se conectar e autenticar cada um dos pontos de extremidade para o qual um método de publicação está habilitado.</span><span class="sxs-lookup"><span data-stu-id="65df1-692">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="65df1-693">**Microsoft WebMatrix 2**, **Microsoft Visual Studio para Web Express** e **Microsoft Visual Studio 2012** oferecem suporte à leitura perfis de publicação para automatizar a configuração desses programas para publicação de aplicativos web para sites do Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="65df1-693">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="65df1-694">![Baixando o site da web de perfil de publicação](whats-new-in-aspnet-mvc-4/_static/image67.png "baixando o site da web de perfil de publicação")</span><span class="sxs-lookup"><span data-stu-id="65df1-694">![Downloading the web site publish profile](whats-new-in-aspnet-mvc-4/_static/image67.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="65df1-695">*Baixando o Site da Web de perfil de publicação*</span><span class="sxs-lookup"><span data-stu-id="65df1-695">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="65df1-696">Baixe o arquivo de perfil de publicação para um local conhecido.</span><span class="sxs-lookup"><span data-stu-id="65df1-696">Download the publish profile file to a known location.</span></span> <span data-ttu-id="65df1-697">Mais neste exercício, você verá como usar esse arquivo para publicar um aplicativo web para um Windows Azure Web Sites do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="65df1-697">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="65df1-698">![Salvando o arquivo de perfil de publicação](whats-new-in-aspnet-mvc-4/_static/image68.png "salvar o perfil de publicação")</span><span class="sxs-lookup"><span data-stu-id="65df1-698">![Saving the publish profile file](whats-new-in-aspnet-mvc-4/_static/image68.png "Saving the publish profile")</span></span>

    <span data-ttu-id="65df1-699">*Salvando o arquivo de perfil de publicação*</span><span class="sxs-lookup"><span data-stu-id="65df1-699">*Saving the publish profile file*</span></span>

<a id="ApxDTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="65df1-700">Tarefa 2 – Configurando o servidor de banco de dados</span><span class="sxs-lookup"><span data-stu-id="65df1-700">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="65df1-701">Se seu aplicativo utiliza o SQL Server bancos de dados que você precisará criar um servidor de banco de dados SQL.</span><span class="sxs-lookup"><span data-stu-id="65df1-701">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="65df1-702">Se você deseja implantar um aplicativo simples que não usa o SQL Server, você pode ignorar esta tarefa.</span><span class="sxs-lookup"><span data-stu-id="65df1-702">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="65df1-703">Você precisará de um servidor de banco de dados SQL para armazenar o banco de dados do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="65df1-703">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="65df1-704">Você pode exibir os servidores de banco de dados SQL de sua assinatura no portal de gerenciamento do Windows Azure em **bancos de dados Sql** | **servidores** | **do servidor Painel**.</span><span class="sxs-lookup"><span data-stu-id="65df1-704">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="65df1-705">Se você não tiver um servidor criado, você pode criar um usando o **adicionar** botão na barra de comandos.</span><span class="sxs-lookup"><span data-stu-id="65df1-705">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="65df1-706">Anote o **nome do servidor e o URL, o nome de logon de administrador e senha**, pois você usará nas próximas tarefas.</span><span class="sxs-lookup"><span data-stu-id="65df1-706">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="65df1-707">Não crie o banco de dados, como ele será criado em um estágio posterior.</span><span class="sxs-lookup"><span data-stu-id="65df1-707">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="65df1-708">![Painel do servidor de banco de dados SQL](whats-new-in-aspnet-mvc-4/_static/image69.png "painel de banco de dados do SQL Server")</span><span class="sxs-lookup"><span data-stu-id="65df1-708">![SQL Database Server Dashboard](whats-new-in-aspnet-mvc-4/_static/image69.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="65df1-709">*Painel de banco de dados do SQL Server*</span><span class="sxs-lookup"><span data-stu-id="65df1-709">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="65df1-710">A próxima tarefa, você testará a conexão de banco de dados do Visual Studio, por esse motivo, você precisa incluir seu endereço IP local na lista do servidor de **endereços IP permitidos**.</span><span class="sxs-lookup"><span data-stu-id="65df1-710">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="65df1-711">Para fazer isso, clique em **configurar**, selecione o endereço IP do **endereço IP do cliente atual** e cole-o no **endereço IP inicial** e **oendereçoIPfinal** caixas de texto e clique no ![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png) botão.</span><span class="sxs-lookup"><span data-stu-id="65df1-711">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png) button.</span></span>

    ![Adicionar endereço IP do cliente](whats-new-in-aspnet-mvc-4/_static/image71.png)

    <span data-ttu-id="65df1-713">*Adicionar endereço IP do cliente*</span><span class="sxs-lookup"><span data-stu-id="65df1-713">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="65df1-714">Uma vez o **endereço IP do cliente** é adicionado para os endereços IP permitidos, clique em **salvar** para confirmar as alterações.</span><span class="sxs-lookup"><span data-stu-id="65df1-714">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Confirmar alterações](whats-new-in-aspnet-mvc-4/_static/image72.png)

    <span data-ttu-id="65df1-716">*Confirmar alterações*</span><span class="sxs-lookup"><span data-stu-id="65df1-716">*Confirm Changes*</span></span>

<a id="ApxDTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="65df1-717">Tarefa 3: publicar um aplicativo ASP.NET MVC 4 usando a implantação da Web</span><span class="sxs-lookup"><span data-stu-id="65df1-717">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="65df1-718">Volte para a solução do ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="65df1-718">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="65df1-719">No **Solution Explorer**, com o botão direito no projeto de site da web e selecione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="65df1-719">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="65df1-720">![Publicar o aplicativo](whats-new-in-aspnet-mvc-4/_static/image73.png "a publicação do aplicativo")</span><span class="sxs-lookup"><span data-stu-id="65df1-720">![Publishing the Application](whats-new-in-aspnet-mvc-4/_static/image73.png "Publishing the Application")</span></span>

    <span data-ttu-id="65df1-721">*Publicar o site*</span><span class="sxs-lookup"><span data-stu-id="65df1-721">*Publishing the web site*</span></span>
2. <span data-ttu-id="65df1-722">Importe o perfil de publicação que você salvou na primeira tarefa.</span><span class="sxs-lookup"><span data-stu-id="65df1-722">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="65df1-723">![Importar o perfil de publicação](whats-new-in-aspnet-mvc-4/_static/image74.png "importar o perfil de publicação")</span><span class="sxs-lookup"><span data-stu-id="65df1-723">![Importing the publish profile](whats-new-in-aspnet-mvc-4/_static/image74.png "Importing the publish profile")</span></span>

    <span data-ttu-id="65df1-724">*Importar o perfil de publicação*</span><span class="sxs-lookup"><span data-stu-id="65df1-724">*Importing publish profile*</span></span>
3. <span data-ttu-id="65df1-725">Clique em **validar Conexão**.</span><span class="sxs-lookup"><span data-stu-id="65df1-725">Click **Validate Connection**.</span></span> <span data-ttu-id="65df1-726">Quando a validação estiver concluída, clique em **próximo**.</span><span class="sxs-lookup"><span data-stu-id="65df1-726">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="65df1-727">A validação é concluída depois que aparecer uma marca de seleção verde ao lado do botão Validar Conexão.</span><span class="sxs-lookup"><span data-stu-id="65df1-727">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="65df1-728">![Validando a conexão](whats-new-in-aspnet-mvc-4/_static/image75.png "validação de conexão")</span><span class="sxs-lookup"><span data-stu-id="65df1-728">![Validating connection](whats-new-in-aspnet-mvc-4/_static/image75.png "Validating connection")</span></span>

    <span data-ttu-id="65df1-729">*Validação de conexão*</span><span class="sxs-lookup"><span data-stu-id="65df1-729">*Validating connection*</span></span>
4. <span data-ttu-id="65df1-730">No **configurações** página no **bancos de dados** seção, clique no botão ao lado da caixa de texto da sua conexão de banco de dados (ou seja, **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="65df1-730">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="65df1-731">![Configuração de implantação da Web](whats-new-in-aspnet-mvc-4/_static/image76.png "configuração de implantação da Web")</span><span class="sxs-lookup"><span data-stu-id="65df1-731">![Web deploy configuration](whats-new-in-aspnet-mvc-4/_static/image76.png "Web deploy configuration")</span></span>

    <span data-ttu-id="65df1-732">*Configuração de implantação da Web*</span><span class="sxs-lookup"><span data-stu-id="65df1-732">*Web deploy configuration*</span></span>
5. <span data-ttu-id="65df1-733">Configure a conexão de banco de dados da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="65df1-733">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="65df1-734">No **nome do servidor** digite sua URL de servidor de banco de dados SQL usando o *tcp:* prefixo.</span><span class="sxs-lookup"><span data-stu-id="65df1-734">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="65df1-735">Em **nome de usuário** digite seu nome de logon de administrador do servidor.</span><span class="sxs-lookup"><span data-stu-id="65df1-735">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="65df1-736">Em **senha** digite sua senha de logon de administrador do servidor.</span><span class="sxs-lookup"><span data-stu-id="65df1-736">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="65df1-737">Digite um novo nome de banco de dados, por exemplo: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="65df1-737">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="65df1-738">![Configurando a cadeia de caracteres de conexão de destino](whats-new-in-aspnet-mvc-4/_static/image77.png "Configurando a cadeia de caracteres de conexão de destino")</span><span class="sxs-lookup"><span data-stu-id="65df1-738">![Configuring destination connection string](whats-new-in-aspnet-mvc-4/_static/image77.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="65df1-739">*Configurando a cadeia de caracteres de conexão de destino*</span><span class="sxs-lookup"><span data-stu-id="65df1-739">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="65df1-740">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="65df1-740">Then click **OK**.</span></span> <span data-ttu-id="65df1-741">Quando solicitado a criar o banco de dados, clique em **Sim**.</span><span class="sxs-lookup"><span data-stu-id="65df1-741">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="65df1-742">![Criando o banco de dados](whats-new-in-aspnet-mvc-4/_static/image78.png "criar a cadeia de caracteres do banco de dados")</span><span class="sxs-lookup"><span data-stu-id="65df1-742">![Creating the database](whats-new-in-aspnet-mvc-4/_static/image78.png "Creating the database string")</span></span>

    <span data-ttu-id="65df1-743">*Criando o banco de dados*</span><span class="sxs-lookup"><span data-stu-id="65df1-743">*Creating the database*</span></span>
7. <span data-ttu-id="65df1-744">A cadeia de caracteres de conexão que você usará para se conectar ao banco de dados SQL no Windows Azure é mostrada na caixa de texto de Conexão padrão.</span><span class="sxs-lookup"><span data-stu-id="65df1-744">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="65df1-745">Clique em **Avançar**.</span><span class="sxs-lookup"><span data-stu-id="65df1-745">Then click **Next**.</span></span>

    <span data-ttu-id="65df1-746">![Cadeia de caracteres de Conexão que aponta para o banco de dados SQL](whats-new-in-aspnet-mvc-4/_static/image79.png "cadeia de caracteres de Conexão que aponta para o banco de dados SQL")</span><span class="sxs-lookup"><span data-stu-id="65df1-746">![Connection string pointing to SQL Database](whats-new-in-aspnet-mvc-4/_static/image79.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="65df1-747">*Cadeia de caracteres de Conexão que aponta para o banco de dados SQL*</span><span class="sxs-lookup"><span data-stu-id="65df1-747">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="65df1-748">No **visualização** , clique em **publicar**.</span><span class="sxs-lookup"><span data-stu-id="65df1-748">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="65df1-749">![Publicar o aplicativo web](whats-new-in-aspnet-mvc-4/_static/image80.png "publicar o aplicativo web")</span><span class="sxs-lookup"><span data-stu-id="65df1-749">![Publishing the web application](whats-new-in-aspnet-mvc-4/_static/image80.png "Publishing the web application")</span></span>

    <span data-ttu-id="65df1-750">*Publicar o aplicativo web*</span><span class="sxs-lookup"><span data-stu-id="65df1-750">*Publishing the web application*</span></span>
9. <span data-ttu-id="65df1-751">Quando termina o processo de publicação, o navegador padrão abrirá o site da web publicado.</span><span class="sxs-lookup"><span data-stu-id="65df1-751">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="65df1-752">![Aplicativo publicado no Windows Azure](whats-new-in-aspnet-mvc-4/_static/image81.png "aplicativo publicado no Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="65df1-752">![Application published to Windows Azure](whats-new-in-aspnet-mvc-4/_static/image81.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="65df1-753">*Aplicativos publicados no Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="65df1-753">*Application published to Windows Azure*</span></span>
