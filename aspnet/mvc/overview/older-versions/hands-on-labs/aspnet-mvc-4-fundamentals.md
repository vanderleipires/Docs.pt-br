---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: "Conceitos básicos do ASP.NET MVC 4 | Microsoft Docs"
author: rick-anderson
description: "Este laboratório prático baseia-se no repositório de música MVC (Model View Controller), um aplicativo tutorial que apresenta e explica passo a passo sobre como usar o ASP.NET MV..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: f93f51219403cd5aeca2dd3648444a84690c3d25
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/02/2018
---
# <a name="aspnet-mvc-4-fundamentals"></a><span data-ttu-id="966de-103">Conceitos básicos do ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="966de-103">ASP.NET MVC 4 Fundamentals</span></span>

<span data-ttu-id="966de-104">Por [Web Camps Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="966de-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="966de-105">Baixar o Kit de treinamento de Camps de Web</span><span class="sxs-lookup"><span data-stu-id="966de-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="966de-106">Este laboratório prático baseia-se no repositório de música MVC (Model View Controller), um aplicativo tutorial que apresenta e explica passo a passo sobre como usar o ASP.NET MVC e o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="966de-106">This Hands-On Lab is based on MVC (Model View Controller) Music Store, a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio.</span></span> <span data-ttu-id="966de-107">Em todo o laboratório, você aprenderá a simplicidade, ainda potência de usar essas tecnologias em conjunto.</span><span class="sxs-lookup"><span data-stu-id="966de-107">Throughout the lab you will learn the simplicity, yet power of using these technologies together.</span></span> <span data-ttu-id="966de-108">Você começará com um aplicativo simples e criará a ele até que você tenha um aplicativo de Web do ASP.NET MVC 4 totalmente funcional.</span><span class="sxs-lookup"><span data-stu-id="966de-108">You will start with a simple application and will build it until you have a fully functional ASP.NET MVC 4 Web Application.</span></span>

<span data-ttu-id="966de-109">Este laboratório funciona com o ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="966de-109">This Lab works with ASP.NET MVC 4.</span></span>

<span data-ttu-id="966de-110">Se você quiser explorar a versão do ASP.NET MVC 3 do tutorial aplicativo, você pode encontrar na [repositório de música MVC](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="966de-110">If you wish to explore the ASP.NET MVC 3 version of the tutorial application, you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="966de-111">Este laboratório prático pressupõe que o desenvolvedor tenha experiência em tecnologias de desenvolvimento na Web, como HTML e JavaScript.</span><span class="sxs-lookup"><span data-stu-id="966de-111">This Hands-On Lab assumes that the developer has experience in Web development technologies, such as HTML and JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="966de-112">Todo o código de exemplo e trechos de código são incluídos no Web Camps treinamento Kit, disponíveis em [versões Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="966de-112">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="966de-113">O projeto específico para este laboratório está disponível em [conceitos básicos do ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).</span><span class="sxs-lookup"><span data-stu-id="966de-113">The project specific to this lab is available at [ASP.NET MVC 4 Fundamentals](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).</span></span>

<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a><span data-ttu-id="966de-114">O aplicativo de repositório de música</span><span class="sxs-lookup"><span data-stu-id="966de-114">The Music Store application</span></span>

<span data-ttu-id="966de-115">O aplicativo web de repositório de música que será criado durante este laboratório consiste em três partes principais: compras, check-out e administração.</span><span class="sxs-lookup"><span data-stu-id="966de-115">The Music Store web application that will be built throughout this Lab comprises three main parts: shopping, checkout, and administration.</span></span> <span data-ttu-id="966de-116">Os visitantes poderão procurar álbuns por gênero, adicionar álbuns a seu carrinho, revise sua seleção e, por fim, vá para check-out ao fazer logon e concluir o pedido.</span><span class="sxs-lookup"><span data-stu-id="966de-116">Visitors will be able to browse albums by genre, add albums to their cart, review their selection and finally proceed to checkout to login and complete the order.</span></span> <span data-ttu-id="966de-117">Além disso, os administradores de armazenamento será capazes de gerenciar os álbuns disponíveis, bem como suas propriedades principais.</span><span class="sxs-lookup"><span data-stu-id="966de-117">Additionally, store administrators will be able to manage the available albums as well as their main properties.</span></span>

<span data-ttu-id="966de-118">![Telas de repositório de música](aspnet-mvc-4-fundamentals/_static/image1.png "telas do repositório de música")</span><span class="sxs-lookup"><span data-stu-id="966de-118">![Music Store screens](aspnet-mvc-4-fundamentals/_static/image1.png "Music Store screens")</span></span>

<span data-ttu-id="966de-119">*Telas de repositório de música*</span><span class="sxs-lookup"><span data-stu-id="966de-119">*Music Store screens*</span></span>

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a><span data-ttu-id="966de-120">ASP.NET MVC 4 Essentials</span><span class="sxs-lookup"><span data-stu-id="966de-120">ASP.NET MVC 4 Essentials</span></span>

<span data-ttu-id="966de-121">Aplicativo de repositório de música será criado usando **controlador MVC (Model View)**, um padrão de arquitetura que separa um aplicativo em três componentes principais:</span><span class="sxs-lookup"><span data-stu-id="966de-121">Music Store application will be built using **Model View Controller (MVC)**, an architectural pattern that separates an application into three main components:</span></span>

- <span data-ttu-id="966de-122">**Modelos de**: objetos de modelo são as partes do aplicativo que implementa a lógica do domínio.</span><span class="sxs-lookup"><span data-stu-id="966de-122">**Models**: Model objects are the parts of the application that implement the domain logic.</span></span> <span data-ttu-id="966de-123">Geralmente, os objetos de modelo também recuperam e armazenam o estado de modelo em um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="966de-123">Often, model objects also retrieve and store model state in a database.</span></span>
- <span data-ttu-id="966de-124">**Modos de exibição:** exibições são os componentes que exibem a interface do usuário do aplicativo (IU).</span><span class="sxs-lookup"><span data-stu-id="966de-124">**Views:** Views are the components that display the application's user interface (UI).</span></span> <span data-ttu-id="966de-125">Normalmente, essa interface do usuário é criado a partir de dados de modelo.</span><span class="sxs-lookup"><span data-stu-id="966de-125">Typically, this UI is created from the model data.</span></span> <span data-ttu-id="966de-126">Um exemplo seria a exibição de edição de álbuns que exibe as caixas de texto e uma lista suspensa com base no estado atual de um objeto de álbum.</span><span class="sxs-lookup"><span data-stu-id="966de-126">An example would be the edit view of Albums that displays text boxes and a drop-down list based on the current state of an Album object.</span></span>
- <span data-ttu-id="966de-127">**Controladores:** controladores são os componentes que lidar com a interação do usuário, manipulam o modelo e, por fim, selecionam um modo de exibição para renderizar a interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="966de-127">**Controllers:** Controllers are the components that handle user interaction, manipulate the model, and ultimately select a view to render the UI.</span></span> <span data-ttu-id="966de-128">Em um aplicativo MVC, a exibição mostra apenas informações; o controlador manipula e responde à entrada e à interação do usuário.</span><span class="sxs-lookup"><span data-stu-id="966de-128">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="966de-129">O padrão MVC ajuda você a criar aplicativos que separam os diferentes aspectos do aplicativo (lógica de entrada, lógica de negócios e lógica de interface do usuário), enquanto fornece um acoplamento fraco entre esses elementos.</span><span class="sxs-lookup"><span data-stu-id="966de-129">The MVC pattern helps you to create applications that separate the different aspects of the application (input logic, business logic, and UI logic), while providing a loose coupling between these elements.</span></span> <span data-ttu-id="966de-130">Essa separação ajuda a gerenciar a complexidade quando você cria um aplicativo, pois ele permite que você se concentre em um aspecto da implementação por vez.</span><span class="sxs-lookup"><span data-stu-id="966de-130">This separation helps you manage complexity when you build an application, as it allows you to focus on one aspect of the implementation at a time.</span></span> <span data-ttu-id="966de-131">Além disso, o padrão MVC torna fácil de testar aplicativos, incentivando também o uso de desenvolvimento controlado por testes (TDD) para a criação de aplicativos.</span><span class="sxs-lookup"><span data-stu-id="966de-131">In addition, the MVC pattern makes it easy to test applications, also encouraging the use of test-driven development (TDD) for creating applications.</span></span>

<span data-ttu-id="966de-132">O **ASP.NET MVC** framework fornece uma alternativa ao padrão Web Forms do ASP.NET para criar aplicativos Web baseados em ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="966de-132">The **ASP.NET MVC** framework provides an alternative to the ASP.NET Web Forms pattern for creating ASP.NET MVC-based Web applications.</span></span> <span data-ttu-id="966de-133">O **ASP.NET MVC** framework é uma estrutura de apresentação leve e altamente testável que (como aplicativos baseados em formulários da Web) está integrado aos recursos ASP.NET existentes, como páginas mestras e baseada em associação autenticação para que você obtenha todos os recursos do .NET framework core.</span><span class="sxs-lookup"><span data-stu-id="966de-133">The **ASP.NET MVC** framework is a lightweight, highly testable presentation framework that (as with Web-forms-based applications) is integrated with existing ASP.NET features, such as master pages and membership-based authentication so you get all the power of the core .NET framework.</span></span> <span data-ttu-id="966de-134">Isso é útil se você já estiver familiarizado com o ASP.NET Web Forms porque todas as bibliotecas que você já usa também estão disponíveis no ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="966de-134">This is useful if you are already familiar with ASP.NET Web Forms because all the libraries that you already use are available in ASP.NET MVC 4 as well.</span></span>

<span data-ttu-id="966de-135">Além disso, o acoplamento flexível entre os três componentes principais de um aplicativo MVC também promove o desenvolvimento paralelo.</span><span class="sxs-lookup"><span data-stu-id="966de-135">In addition, the loose coupling between the three main components of an MVC application also promotes parallel development.</span></span> <span data-ttu-id="966de-136">Por exemplo, um desenvolvedor pode trabalhar no modo de exibição, um segundo desenvolvedor pode trabalhar na lógica do controlador e um terceiro desenvolvedor pode se concentrar na lógica de negócios no modelo.</span><span class="sxs-lookup"><span data-stu-id="966de-136">For instance, one developer can work on the view, a second developer can work on the controller logic, and a third developer can focus on the business logic in the model.</span></span>

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="966de-137">Objetivos</span><span class="sxs-lookup"><span data-stu-id="966de-137">Objectives</span></span>

<span data-ttu-id="966de-138">Neste laboratório prático, você aprenderá como:</span><span class="sxs-lookup"><span data-stu-id="966de-138">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="966de-139">Criar um aplicativo ASP.NET MVC do zero com base no tutorial do aplicativo de repositório de música</span><span class="sxs-lookup"><span data-stu-id="966de-139">Create an ASP.NET MVC application from scratch based on the Music Store Application tutorial</span></span>
- <span data-ttu-id="966de-140">Adicionar controladores para lidar com URLs para a Home page do site e para navegação sua funcionalidade principal</span><span class="sxs-lookup"><span data-stu-id="966de-140">Add Controllers to handle URLs to the Home page of the site and for browsing its main functionality</span></span>
- <span data-ttu-id="966de-141">Adicionar um modo de exibição para personalizar o conteúdo exibido junto com seu estilo</span><span class="sxs-lookup"><span data-stu-id="966de-141">Add a View to customize the content displayed along with its style</span></span>
- <span data-ttu-id="966de-142">Adicionar classes de modelo para conter e gerenciar dados e domínio lógica</span><span class="sxs-lookup"><span data-stu-id="966de-142">Add Model classes to contain and manage data and domain logic</span></span>
- <span data-ttu-id="966de-143">Usar padrão de modelo de exibição para passar informações de ações do controlador para exibir modelos</span><span class="sxs-lookup"><span data-stu-id="966de-143">Use View Model pattern to pass information from controller actions to the view templates</span></span>
- <span data-ttu-id="966de-144">Explore o novo modelo de ASP.NET MVC 4 para aplicativos da internet</span><span class="sxs-lookup"><span data-stu-id="966de-144">Explore the ASP.NET MVC 4 new template for internet applications</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="966de-145">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="966de-145">Prerequisites</span></span>

<span data-ttu-id="966de-146">Você deve ter os seguintes itens para concluir este laboratório:</span><span class="sxs-lookup"><span data-stu-id="966de-146">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="966de-147">[O Visual Studio 2012 Express para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (leitura [Apêndice A](#AppendixA) para obter instruções sobre como instalá-lo)</span><span class="sxs-lookup"><span data-stu-id="966de-147">[Visual Studio 2012 Express for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (read [Appendix A](#AppendixA) for instructions on how to install it)</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="966de-148">Configuração</span><span class="sxs-lookup"><span data-stu-id="966de-148">Setup</span></span>

<span data-ttu-id="966de-149">**Instalando os trechos de código**</span><span class="sxs-lookup"><span data-stu-id="966de-149">**Installing Code Snippets**</span></span>

<span data-ttu-id="966de-150">Para sua conveniência, boa parte do código que você estiver gerenciando ao longo deste laboratório está disponível como trechos de código do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="966de-150">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="966de-151">Para instalar os trechos de código executar **.\Source\Setup\CodeSnippets.vsi** arquivo.</span><span class="sxs-lookup"><span data-stu-id="966de-151">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="966de-152">Se você não estiver familiarizado com os trechos de código do Visual Studio e aprender como usá-los, consulte o Apêndice deste documento &quot; [trechos de código usando do apêndice c:](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="966de-152">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="966de-153">Exercícios</span><span class="sxs-lookup"><span data-stu-id="966de-153">Exercises</span></span>

<span data-ttu-id="966de-154">Este laboratório prático é composto pelos exercícios a seguir:</span><span class="sxs-lookup"><span data-stu-id="966de-154">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="966de-155">Exercício 1: Criando o projeto de aplicativo Web do MusicStore ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="966de-155">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>](#Exercise1)
2. [<span data-ttu-id="966de-156">Exercício 2: Criando um controlador</span><span class="sxs-lookup"><span data-stu-id="966de-156">Exercise 2: Creating a Controller</span></span>](#Exercise2)
3. [<span data-ttu-id="966de-157">Exercício 3: Passar parâmetros para um controlador</span><span class="sxs-lookup"><span data-stu-id="966de-157">Exercise 3: Passing parameters to a Controller</span></span>](#Exercise3)
4. [<span data-ttu-id="966de-158">Exercício 4: Criar um modo de exibição</span><span class="sxs-lookup"><span data-stu-id="966de-158">Exercise 4: Creating a View</span></span>](#Exercise4)
5. [<span data-ttu-id="966de-159">Exercício 5: Criar um modelo de exibição</span><span class="sxs-lookup"><span data-stu-id="966de-159">Exercise 5: Creating a View Model</span></span>](#Exercise5)
6. [<span data-ttu-id="966de-160">Exercício 6: Usando parâmetros no modo de exibição</span><span class="sxs-lookup"><span data-stu-id="966de-160">Exercise 6: Using parameters in View</span></span>](#Exercise6)
7. [<span data-ttu-id="966de-161">Exercício 7: Visão geral do novo modelo do ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="966de-161">Exercise 7: A lap around ASP.NET MVC 4 New Template</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="966de-162">Cada exercício é acompanhado por um **final** pasta que contém a solução resultante, você deve obter depois de concluir os exercícios.</span><span class="sxs-lookup"><span data-stu-id="966de-162">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="966de-163">Você pode usar essa solução como um guia se você precisar trabalhar com os exercícios de ajuda adicional.</span><span class="sxs-lookup"><span data-stu-id="966de-163">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="966de-164">Tempo estimado para concluir este laboratório: **60 minutos**.</span><span class="sxs-lookup"><span data-stu-id="966de-164">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a><span data-ttu-id="966de-165">Exercício 1: Criando o projeto de aplicativo Web do MusicStore ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="966de-165">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>

<span data-ttu-id="966de-166">Neste exercício, você aprenderá como criar um aplicativo ASP.NET MVC no Visual Studio 2012 Express para Web, bem como sua organização pasta principal.</span><span class="sxs-lookup"><span data-stu-id="966de-166">In this exercise, you will learn how to create an ASP.NET MVC application in Visual Studio 2012 Express for Web as well as its main folder organization.</span></span> <span data-ttu-id="966de-167">Além disso, você aprenderá a adicionar um novo controlador e torná-lo a exibir uma cadeia de caracteres simple na página inicial do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="966de-167">Additionally, you will learn how to add a new Controller and make it display a simple string in the application's home page.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a><span data-ttu-id="966de-168">Tarefa 1: Criando o projeto de aplicativo Web ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="966de-168">Task 1 - Creating the ASP.NET MVC Web Application Project</span></span>

1. <span data-ttu-id="966de-169">Nesta tarefa, você criará um projeto de aplicativo do ASP.NET MVC vazio usando o modelo MVC Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="966de-169">In this task, you will create an empty ASP.NET MVC application project using the MVC Visual Studio template.</span></span> <span data-ttu-id="966de-170">Iniciar **VS Express para Web**.</span><span class="sxs-lookup"><span data-stu-id="966de-170">Start **VS Express for Web**.</span></span>
2. <span data-ttu-id="966de-171">No menu **Arquivo**, clique em **Novo Projeto**.</span><span class="sxs-lookup"><span data-stu-id="966de-171">On the **File** menu, click **New Project**.</span></span>
3. <span data-ttu-id="966de-172">No **novo projeto** select da caixa de diálogo de **aplicativo Web do ASP.NET MVC 4** tipo, localizado no projeto **Visual c#,** **Web** modelo lista.</span><span class="sxs-lookup"><span data-stu-id="966de-172">In the **New Project** dialog box select the **ASP.NET MVC 4 Web Application** project type, located under **Visual C#,** **Web** template list.</span></span>
4. <span data-ttu-id="966de-173">Alterar o **nome** para *MvcMusicStore*.</span><span class="sxs-lookup"><span data-stu-id="966de-173">Change the **Name** to *MvcMusicStore*.</span></span>
5. <span data-ttu-id="966de-174">Defina o local da solução dentro de um novo **começar** na pasta de origem deste exercício, por exemplo **\Source\Ex01-CreatingMusicStoreProject\Begin [seu-HOL-PATH]**.</span><span class="sxs-lookup"><span data-stu-id="966de-174">Set the location of the solution inside a new **Begin** folder in this Exercise's Source folder, for example **[YOUR-HOL-PATH]\Source\Ex01-CreatingMusicStoreProject\Begin**.</span></span> <span data-ttu-id="966de-175">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="966de-175">Click **OK**.</span></span>

    <span data-ttu-id="966de-176">![Criar caixa de diálogo Novo projeto](aspnet-mvc-4-fundamentals/_static/image2.png "criar caixa de diálogo Novo projeto")</span><span class="sxs-lookup"><span data-stu-id="966de-176">![Create New Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image2.png "Create New Project Dialog Box")</span></span>

    <span data-ttu-id="966de-177">*Criar caixa de diálogo Novo projeto*</span><span class="sxs-lookup"><span data-stu-id="966de-177">*Create New Project Dialog Box*</span></span>
6. <span data-ttu-id="966de-178">No **novo projeto ASP.NET MVC 4** select da caixa de diálogo de **básica** modelo e certifique-se de que o **mecanismo de exibição** selecionado é **Razor**.</span><span class="sxs-lookup"><span data-stu-id="966de-178">In the **New ASP.NET MVC 4 Project** dialog box select the **Basic** template and make sure that the **View engine** selected is **Razor**.</span></span> <span data-ttu-id="966de-179">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="966de-179">Click **OK**.</span></span>

    <span data-ttu-id="966de-180">![Nova caixa de diálogo do projeto ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image3.png "nova caixa de diálogo do projeto ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="966de-180">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image3.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="966de-181">*Nova caixa de diálogo do projeto ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="966de-181">*New ASP.NET MVC 4 Project Dialog Box*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a><span data-ttu-id="966de-182">Tarefa 2: explorar a estrutura da solução</span><span class="sxs-lookup"><span data-stu-id="966de-182">Task 2 - Exploring the Solution Structure</span></span>

<span data-ttu-id="966de-183">A estrutura ASP.NET MVC inclui um modelo de projeto do Visual Studio que ajuda você a criar aplicativos da Web que suporte o padrão MVC.</span><span class="sxs-lookup"><span data-stu-id="966de-183">The ASP.NET MVC framework includes a Visual Studio project template that helps you create Web applications supporting the MVC pattern.</span></span> <span data-ttu-id="966de-184">Este modelo cria um novo aplicativo Web do ASP.NET MVC com as pastas necessárias, modelos de item e as entradas do arquivo de configuração.</span><span class="sxs-lookup"><span data-stu-id="966de-184">This template creates a new ASP.NET MVC Web application with the required folders, item templates, and configuration-file entries.</span></span>

<span data-ttu-id="966de-185">Nesta tarefa, você examinará a estrutura da solução para compreender os elementos que estão envolvidos e suas relações.</span><span class="sxs-lookup"><span data-stu-id="966de-185">In this task, you will examine the solution structure to understand the elements that are involved and their relationships.</span></span> <span data-ttu-id="966de-186">As pastas a seguir estão incluídas no aplicativo do ASP.NET MVC porque a estrutura ASP.NET MVC por padrão usa uma &quot;convenção sobre a configuração&quot; abordagem e faz algumas suposições padrão com base em nomes de pasta convenções.</span><span class="sxs-lookup"><span data-stu-id="966de-186">The following folders are included in all the ASP.NET MVC application because the ASP.NET MVC framework by default uses a &quot;convention over configuration&quot; approach, and makes some default assumptions based on folder naming conventions.</span></span>

1. <span data-ttu-id="966de-187">Quando o projeto é criado, examine a estrutura de pasta que foi criada no Gerenciador de soluções, no lado direito:</span><span class="sxs-lookup"><span data-stu-id="966de-187">Once the project is created, review the folder structure that has been created in the Solution Explorer on the right side:</span></span>

    <span data-ttu-id="966de-188">![Estrutura de pasta do ASP.NET MVC no Gerenciador de soluções](aspnet-mvc-4-fundamentals/_static/image4.png "estrutura ASP.NET MVC pasta no Gerenciador de soluções")</span><span class="sxs-lookup"><span data-stu-id="966de-188">![ASP.NET MVC Folder structure in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image4.png "ASP.NET MVC Folder structure in Solution Explorer")</span></span>

    <span data-ttu-id="966de-189">*Estrutura de pasta do ASP.NET MVC no Gerenciador de soluções*</span><span class="sxs-lookup"><span data-stu-id="966de-189">*ASP.NET MVC Folder structure in Solution Explorer*</span></span>

    1. <span data-ttu-id="966de-190">**Controladores**.</span><span class="sxs-lookup"><span data-stu-id="966de-190">**Controllers**.</span></span> <span data-ttu-id="966de-191">Essa pasta contém as classes do controlador.</span><span class="sxs-lookup"><span data-stu-id="966de-191">This folder will contain the controller classes.</span></span> <span data-ttu-id="966de-192">Em um aplicativo MVC baseado, controladores são responsáveis por tratar a interação do usuário final, manipulando o modelo e, por fim, escolhendo um modo de exibição para renderizar a interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="966de-192">In an MVC based application, controllers are responsible for handling end user interaction, manipulating the model, and ultimately choosing a view to render the UI.</span></span>

        > [!NOTE]
        > <span data-ttu-id="966de-193">A estrutura do MVC requer que os nomes de todos os controladores para terminar com &quot;controlador&quot;-por exemplo, HomeController, LoginController ou ProductController.</span><span class="sxs-lookup"><span data-stu-id="966de-193">The MVC framework requires the names of all controllers to end with &quot;Controller&quot;-for example, HomeController, LoginController, or ProductController.</span></span>
    2. <span data-ttu-id="966de-194">**Modelos de**.</span><span class="sxs-lookup"><span data-stu-id="966de-194">**Models**.</span></span> <span data-ttu-id="966de-195">Essa pasta é fornecida para classes que representam o modelo de aplicativo para o aplicativo Web MVC.</span><span class="sxs-lookup"><span data-stu-id="966de-195">This folder is provided for classes that represent the application model for the MVC Web application.</span></span> <span data-ttu-id="966de-196">Isso geralmente inclui o código que define a lógica para interagir com o repositório de dados e objetos.</span><span class="sxs-lookup"><span data-stu-id="966de-196">This usually includes code that defines objects and the logic for interacting with the data store.</span></span> <span data-ttu-id="966de-197">Normalmente, os objetos de modelo real será em bibliotecas de classe separada.</span><span class="sxs-lookup"><span data-stu-id="966de-197">Typically, the actual model objects will be in separate class libraries.</span></span> <span data-ttu-id="966de-198">No entanto, quando você cria um novo aplicativo, você pode incluir classes e movê-los para as bibliotecas de classe separada posteriormente no ciclo de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="966de-198">However, when you create a new application, you might include classes and then move them into separate class libraries at a later point in the development cycle.</span></span>
    3. <span data-ttu-id="966de-199">**Modos de exibição**.</span><span class="sxs-lookup"><span data-stu-id="966de-199">**Views**.</span></span> <span data-ttu-id="966de-200">Essa pasta é o local recomendado para modos de exibição, os componentes responsáveis por exibir a interface do usuário do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="966de-200">This folder is the recommended location for views, the components responsible for displaying the application's user interface.</span></span> <span data-ttu-id="966de-201">Modos de exibição usam arquivos. aspx,. ascx,. cshtml e. master, além de outros arquivos que estão relacionados aos modos de exibição de renderização.</span><span class="sxs-lookup"><span data-stu-id="966de-201">Views use .aspx, .ascx, .cshtml and .master files, in addition to any other files that are related to rendering views.</span></span> <span data-ttu-id="966de-202">Pasta de modos de exibição contém uma pasta para cada controlador; a pasta é nomeada com o prefixo do nome do controlador.</span><span class="sxs-lookup"><span data-stu-id="966de-202">Views folder contains a folder for each controller; the folder is named with the controller-name prefix.</span></span> <span data-ttu-id="966de-203">Por exemplo, se você tiver um controlador nomeado **HomeController**, modos de exibição contém uma pasta chamada início.</span><span class="sxs-lookup"><span data-stu-id="966de-203">For example, if you have a controller named **HomeController**, the Views folder will contain a folder named Home.</span></span> <span data-ttu-id="966de-204">Por padrão, quando a estrutura ASP.NET MVC carrega uma exibição, ele procura um arquivo. aspx com o nome de exibição solicitada na pasta Views\controllerName (**exibições [ControllerName] [ação]. aspx**) ou (**exibições [ControllerName] [Ação] cshtml**) para modos de exibição do Razor.</span><span class="sxs-lookup"><span data-stu-id="966de-204">By default, when the ASP.NET MVC framework loads a view, it looks for an .aspx file with the requested view name in the Views\controllerName folder (**Views[ControllerName][Action].aspx**) or (**Views[ControllerName][Action].cshtml**) for Razor Views.</span></span>

    > [!NOTE]
    > <span data-ttu-id="966de-205">Além das pastas listadas anteriormente, um aplicativo MVC usa o **global. asax** padrões de arquivo para definir o roteamento de URL global e usa o **Web. config** arquivo para configurar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="966de-205">In addition to the folders listed previously, an MVC Web application uses the **Global.asax** file to set global URL routing defaults, and it uses the **Web.config** file to configure the application.</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a><span data-ttu-id="966de-206">Tarefa 3: adicionando um HomeController</span><span class="sxs-lookup"><span data-stu-id="966de-206">Task 3 - Adding a HomeController</span></span>

<span data-ttu-id="966de-207">Em aplicativos ASP.NET que não usam a estrutura do MVC, a interação do usuário é organizada em torno de páginas e gerando e manipulando eventos a partir dessas páginas.</span><span class="sxs-lookup"><span data-stu-id="966de-207">In ASP.NET applications that do not use the MVC framework, user interaction is organized around pages, and around raising and handling events from those pages.</span></span> <span data-ttu-id="966de-208">Por outro lado, a interação do usuário com aplicativos ASP.NET MVC é organizada em torno de controladores e seus métodos de ação.</span><span class="sxs-lookup"><span data-stu-id="966de-208">In contrast, user interaction with ASP.NET MVC applications is organized around controllers and their action methods.</span></span>

<span data-ttu-id="966de-209">Por outro lado, estrutura ASP.NET MVC mapeia URLs para classes que são chamados de controladores.</span><span class="sxs-lookup"><span data-stu-id="966de-209">On the other hand, ASP.NET MVC framework maps URLs to classes that are referred to as controllers.</span></span> <span data-ttu-id="966de-210">Controladores de processar solicitações de entrada, lidar com a entrada do usuário e interações, executar a lógica do aplicativo apropriada e determinar a resposta para enviar de volta para o cliente (exibição de HTML, baixar um arquivo, redirecionar para uma URL diferente, etc.).</span><span class="sxs-lookup"><span data-stu-id="966de-210">Controllers process incoming requests, handle user input and interactions, execute appropriate application logic and determine the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span> <span data-ttu-id="966de-211">No caso de exibição de HTML, uma classe de controlador normalmente chama um componente de visualização separada para gerar a marcação HTML para a solicitação.</span><span class="sxs-lookup"><span data-stu-id="966de-211">In the case of displaying HTML, a controller class typically calls a separate view component to generate the HTML markup for the request.</span></span> <span data-ttu-id="966de-212">Em um aplicativo MVC, a exibição mostra apenas informações; o controlador manipula e responde à entrada e à interação do usuário.</span><span class="sxs-lookup"><span data-stu-id="966de-212">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="966de-213">Nesta tarefa, você adicionará uma classe do controlador que tratará as URLs para a Home page do site do repositório de música.</span><span class="sxs-lookup"><span data-stu-id="966de-213">In this task, you will add a Controller class that will handle URLs to the Home page of the Music Store site.</span></span>

1. <span data-ttu-id="966de-214">Clique com botão direito **controladores** pasta no Gerenciador de soluções, selecione **adicionar** e **controlador** comando:</span><span class="sxs-lookup"><span data-stu-id="966de-214">Right-click **Controllers** folder within the Solution Explorer, select **Add** and then **Controller** command:</span></span>

    <span data-ttu-id="966de-215">![Adicionar um comando de controlador](aspnet-mvc-4-fundamentals/_static/image5.png "adicionar um comando de controlador")</span><span class="sxs-lookup"><span data-stu-id="966de-215">![Add a Controller Command](aspnet-mvc-4-fundamentals/_static/image5.png "Add a Controller Command")</span></span>

    <span data-ttu-id="966de-216">*Adicionar um comando de controlador*</span><span class="sxs-lookup"><span data-stu-id="966de-216">*Add Controller Command*</span></span>
2. <span data-ttu-id="966de-217">O **Adicionar controlador** caixa de diálogo é exibida.</span><span class="sxs-lookup"><span data-stu-id="966de-217">The **Add Controller** dialog appears.</span></span> <span data-ttu-id="966de-218">Nome do controlador *HomeController* e pressione **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="966de-218">Name the controller *HomeController* and press **Add**.</span></span>

    <span data-ttu-id="966de-219">![Controlador de caixa de diálogo Adicionar](aspnet-mvc-4-fundamentals/_static/image6.png "Adicionar caixa de diálogo do controlador")</span><span class="sxs-lookup"><span data-stu-id="966de-219">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image6.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="966de-220">*Adicionar caixa de diálogo do controlador*</span><span class="sxs-lookup"><span data-stu-id="966de-220">*Add Controller Dialog*</span></span>
3. <span data-ttu-id="966de-221">O arquivo **HomeController** é criado no **controladores** pasta.</span><span class="sxs-lookup"><span data-stu-id="966de-221">The file **HomeController.cs** is created in the **Controllers** folder.</span></span> <span data-ttu-id="966de-222">Para ter o **HomeController** retornar uma cadeia de caracteres em sua ação de índice, substitua o **índice** método com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="966de-222">In order to have the **HomeController** return a string on its Index action, replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="966de-223">(Código de trecho - *conceitos básicos do ASP.NET MVC 4 - Ex1 HomeController índice*)</span><span class="sxs-lookup"><span data-stu-id="966de-223">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex1 HomeController Index*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="966de-224">Tarefa 4 - executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="966de-224">Task 4 - Running the Application</span></span>

<span data-ttu-id="966de-225">Nesta tarefa, você experimentar o aplicativo em um navegador da web.</span><span class="sxs-lookup"><span data-stu-id="966de-225">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="966de-226">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="966de-226">Press **F5** to run the Application.</span></span> <span data-ttu-id="966de-227">O projeto é compilado e inicia de servidor Web do IIS local.</span><span class="sxs-lookup"><span data-stu-id="966de-227">The project is compiled and the local IIS Web Server starts.</span></span> <span data-ttu-id="966de-228">Local IIS Web Server automaticamente abrirá um navegador da web apontando para a URL do servidor Web.</span><span class="sxs-lookup"><span data-stu-id="966de-228">The local IIS Web Server will automatically open a web browser pointing to the URL of the Web server.</span></span>

    <span data-ttu-id="966de-229">![Aplicativo em execução em um navegador da web](aspnet-mvc-4-fundamentals/_static/image7.png "aplicativo executado em um navegador da web")</span><span class="sxs-lookup"><span data-stu-id="966de-229">![Application running in a web browser](aspnet-mvc-4-fundamentals/_static/image7.png "Application running in a web browser")</span></span>

    <span data-ttu-id="966de-230">*Aplicativo em execução em um navegador da web*</span><span class="sxs-lookup"><span data-stu-id="966de-230">*Application running in a web browser*</span></span>

    > [!NOTE]
    > <span data-ttu-id="966de-231">Servidor Web do IIS local executará o site em um número de porta livre aleatória.</span><span class="sxs-lookup"><span data-stu-id="966de-231">The local IIS Web Server will run the website on a random free port number.</span></span> <span data-ttu-id="966de-232">Na figura acima, o site está em execução no `http://localhost:50103/`, de modo que ele está usando a porta 50103.</span><span class="sxs-lookup"><span data-stu-id="966de-232">In the figure above, the site is running at `http://localhost:50103/`, so it's using port 50103.</span></span> <span data-ttu-id="966de-233">O número de porta pode variar.</span><span class="sxs-lookup"><span data-stu-id="966de-233">Your port number may vary.</span></span>
2. <span data-ttu-id="966de-234">Feche o navegador.</span><span class="sxs-lookup"><span data-stu-id="966de-234">Close the browser.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a><span data-ttu-id="966de-235">Exercício 2: Criando um controlador</span><span class="sxs-lookup"><span data-stu-id="966de-235">Exercise 2: Creating a Controller</span></span>

<span data-ttu-id="966de-236">Neste exercício, você aprenderá como atualizar o controlador para implementar a funcionalidade do aplicativo de repositório de música simple.</span><span class="sxs-lookup"><span data-stu-id="966de-236">In this exercise, you will learn how to update the controller to implement simple functionality of the Music Store application.</span></span> <span data-ttu-id="966de-237">Esse controlador definirá os métodos de ação para lidar com cada uma das seguintes solicitações específicas:</span><span class="sxs-lookup"><span data-stu-id="966de-237">That controller will define action methods to handle each of the following specific requests:</span></span>

- <span data-ttu-id="966de-238">Uma página de listagem de gêneros a música no repositório de música</span><span class="sxs-lookup"><span data-stu-id="966de-238">A listing page of the music genres in the Music Store</span></span>
- <span data-ttu-id="966de-239">Uma página de pesquisa que lista todos os álbuns de música para um determinado gênero</span><span class="sxs-lookup"><span data-stu-id="966de-239">A browse page that lists all of the music albums for a particular genre</span></span>
- <span data-ttu-id="966de-240">Uma página de detalhes que mostra informações sobre um álbum de música específico</span><span class="sxs-lookup"><span data-stu-id="966de-240">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="966de-241">Para o escopo deste exercício, essas ações simplesmente retornará uma cadeia de caracteres agora.</span><span class="sxs-lookup"><span data-stu-id="966de-241">For the scope of this exercise, those actions will simply return a string by now.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a><span data-ttu-id="966de-242">Tarefa 1: adicionar uma nova classe StoreController</span><span class="sxs-lookup"><span data-stu-id="966de-242">Task 1 - Adding a New StoreController Class</span></span>

<span data-ttu-id="966de-243">Nesta tarefa, você adicionará um novo controlador.</span><span class="sxs-lookup"><span data-stu-id="966de-243">In this task, you will add a new Controller.</span></span>

1. <span data-ttu-id="966de-244">Se não estiver aberto, inicie **VS Express para Web 2012**.</span><span class="sxs-lookup"><span data-stu-id="966de-244">If not already open, start **VS Express for Web 2012**.</span></span>
2. <span data-ttu-id="966de-245">No **arquivo** menu, escolha **Abrir projeto**.</span><span class="sxs-lookup"><span data-stu-id="966de-245">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="966de-246">Na caixa de diálogo Abrir projeto, vá para **Source\Ex02 CreatingAController\Begin**, selecione **Begin.sln** e clique em **abrir**.</span><span class="sxs-lookup"><span data-stu-id="966de-246">In the Open Project dialog, browse to **Source\Ex02-CreatingAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="966de-247">Como alternativa, você pode continuar com a solução que você obteve depois de concluir o exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="966de-247">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

    1. <span data-ttu-id="966de-248">Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="966de-248">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="966de-249">Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="966de-249">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="966de-250">No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="966de-250">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="966de-251">Por fim, compile a solução clicando **criar** | **compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="966de-251">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="966de-252">Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="966de-252">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="966de-253">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="966de-253">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="966de-254">É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.</span><span class="sxs-lookup"><span data-stu-id="966de-254">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="966de-255">Adicione o novo controlador.</span><span class="sxs-lookup"><span data-stu-id="966de-255">Add the new controller.</span></span> <span data-ttu-id="966de-256">Para fazer isso, clique com botão direito do **controladores** pasta no Gerenciador de soluções, selecione **adicionar** e, em seguida, o **controlador** comando.</span><span class="sxs-lookup"><span data-stu-id="966de-256">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="966de-257">Alterar o **nome do controlador** para *StoreController*e clique em **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="966de-257">Change the **Controller Name** to *StoreController*, and click **Add**.</span></span>

    <span data-ttu-id="966de-258">![Controlador de caixa de diálogo Adicionar](aspnet-mvc-4-fundamentals/_static/image8.png "Adicionar caixa de diálogo do controlador")</span><span class="sxs-lookup"><span data-stu-id="966de-258">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image8.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="966de-259">*Adicionar caixa de diálogo do controlador*</span><span class="sxs-lookup"><span data-stu-id="966de-259">*Add Controller Dialog*</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a><span data-ttu-id="966de-260">Tarefa 2: modificar ações do StoreController</span><span class="sxs-lookup"><span data-stu-id="966de-260">Task 2 - Modifying the StoreController's Actions</span></span>

<span data-ttu-id="966de-261">Nesta tarefa, você modificará os métodos do controlador que são chamados **ações**.</span><span class="sxs-lookup"><span data-stu-id="966de-261">In this task, you will modify the Controller methods that are called **actions**.</span></span> <span data-ttu-id="966de-262">Ações são responsáveis por gerenciar solicitações de URL e determinar o conteúdo que deve ser enviado de volta para o navegador ou o usuário que invocou a URL.</span><span class="sxs-lookup"><span data-stu-id="966de-262">Actions are responsible for handling URL requests and determining the content that should be sent back to the browser or user that invoked the URL.</span></span>

1. <span data-ttu-id="966de-263">O **StoreController** classe já tem um **índice** método.</span><span class="sxs-lookup"><span data-stu-id="966de-263">The **StoreController** class already has an **Index** method.</span></span> <span data-ttu-id="966de-264">Você usará-lo posteriormente neste laboratório para implementar a página que lista todos os gêneros do repositório de música.</span><span class="sxs-lookup"><span data-stu-id="966de-264">You will use it later in this Lab to implement the page that lists all genres of the music store.</span></span> <span data-ttu-id="966de-265">Por enquanto, basta substituir a **índice** método com o código a seguir que retorna uma cadeia de caracteres &quot;Olá do Store.Index()&quot;:</span><span class="sxs-lookup"><span data-stu-id="966de-265">For now, just replace the **Index** method with the following code that returns a string &quot;Hello from Store.Index()&quot;:</span></span>

    <span data-ttu-id="966de-266">(Código de trecho - *conceitos básicos do ASP.NET MVC 4 - o Ex2 StoreController índice*)</span><span class="sxs-lookup"><span data-stu-id="966de-266">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController Index*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
2. <span data-ttu-id="966de-267">Adicionar **procurar** e **detalhes** métodos.</span><span class="sxs-lookup"><span data-stu-id="966de-267">Add **Browse** and **Details** methods.</span></span> <span data-ttu-id="966de-268">Para fazer isso, adicione o seguinte código para o **StoreController**:</span><span class="sxs-lookup"><span data-stu-id="966de-268">To do this, add the following code to the **StoreController**:</span></span>

    <span data-ttu-id="966de-269">(Código de trecho - *conceitos básicos do ASP.NET MVC 4 - o Ex2 StoreController BrowseAndDetails*)</span><span class="sxs-lookup"><span data-stu-id="966de-269">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController BrowseAndDetails*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="966de-270">Tarefa 3: executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="966de-270">Task 3 - Running the Application</span></span>

<span data-ttu-id="966de-271">Nesta tarefa, você experimentar o aplicativo em um navegador da web.</span><span class="sxs-lookup"><span data-stu-id="966de-271">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="966de-272">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="966de-272">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="966de-273">O projeto é iniciado **início** página.</span><span class="sxs-lookup"><span data-stu-id="966de-273">The project starts in the **Home** page.</span></span> <span data-ttu-id="966de-274">Altere a URL para verificar a implementação de cada ação.</span><span class="sxs-lookup"><span data-stu-id="966de-274">Change the URL to verify each action's implementation.</span></span>

    1. <span data-ttu-id="966de-275">**/Store**.</span><span class="sxs-lookup"><span data-stu-id="966de-275">**/Store**.</span></span> <span data-ttu-id="966de-276">Você verá  **&quot;Olá do Store.Index()&quot;**.</span><span class="sxs-lookup"><span data-stu-id="966de-276">You will see **&quot;Hello from Store.Index()&quot;**.</span></span>
    2. <span data-ttu-id="966de-277">**/Store/Browse**.</span><span class="sxs-lookup"><span data-stu-id="966de-277">**/Store/Browse**.</span></span> <span data-ttu-id="966de-278">Você verá  **&quot;Olá do Store.Browse()&quot;**.</span><span class="sxs-lookup"><span data-stu-id="966de-278">You will see **&quot;Hello from Store.Browse()&quot;**.</span></span>
    3. <span data-ttu-id="966de-279">**/Store/Details**.</span><span class="sxs-lookup"><span data-stu-id="966de-279">**/Store/Details**.</span></span> <span data-ttu-id="966de-280">Você verá  **&quot;Olá do Store.Details()&quot;**.</span><span class="sxs-lookup"><span data-stu-id="966de-280">You will see **&quot;Hello from Store.Details()&quot;**.</span></span>

        <span data-ttu-id="966de-281">![Navegação StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "StoreBrowse de navegação")</span><span class="sxs-lookup"><span data-stu-id="966de-281">![Browsing StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "Browsing StoreBrowse")</span></span>

        <span data-ttu-id="966de-282">*Navegação /Store/Browse*</span><span class="sxs-lookup"><span data-stu-id="966de-282">*Browsing /Store/Browse*</span></span>
3. <span data-ttu-id="966de-283">Feche o navegador.</span><span class="sxs-lookup"><span data-stu-id="966de-283">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a><span data-ttu-id="966de-284">Exercício 3: Passar parâmetros para um controlador</span><span class="sxs-lookup"><span data-stu-id="966de-284">Exercise 3: Passing parameters to a Controller</span></span>

<span data-ttu-id="966de-285">Até agora, você tem sido retornando cadeias de caracteres constantes dos controladores de.</span><span class="sxs-lookup"><span data-stu-id="966de-285">Until now, you have been returning constant strings from the Controllers.</span></span> <span data-ttu-id="966de-286">Neste exercício, você aprenderá como passar parâmetros para um controlador usando a URL e querystring e, em seguida, fazer com que as ações de método responder com texto para o navegador.</span><span class="sxs-lookup"><span data-stu-id="966de-286">In this exercise you will learn how to pass parameters to a Controller using the URL and querystring, and then making the method actions respond with text to the browser.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a><span data-ttu-id="966de-287">Tarefa 1: Adicionar parâmetro gênero StoreController</span><span class="sxs-lookup"><span data-stu-id="966de-287">Task 1 - Adding Genre Parameter to StoreController</span></span>

<span data-ttu-id="966de-288">Nesta tarefa, você usará o **querystring** para enviar parâmetros para o **procurar** método de ação de **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="966de-288">In this task, you will use the **querystring** to send parameters to the **Browse** action method in the **StoreController**.</span></span>

1. <span data-ttu-id="966de-289">Se não estiver aberto, inicie **VS Express para Web**.</span><span class="sxs-lookup"><span data-stu-id="966de-289">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="966de-290">No **arquivo** menu, escolha **Abrir projeto**.</span><span class="sxs-lookup"><span data-stu-id="966de-290">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="966de-291">Na caixa de diálogo Abrir projeto, vá para **Source\Ex03 PassingParametersToAController\Begin**, selecione **Begin.sln** e clique em **abrir**.</span><span class="sxs-lookup"><span data-stu-id="966de-291">In the Open Project dialog, browse to **Source\Ex03-PassingParametersToAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="966de-292">Como alternativa, você pode continuar com a solução que você obteve depois de concluir o exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="966de-292">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

    1. <span data-ttu-id="966de-293">Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="966de-293">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="966de-294">Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="966de-294">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="966de-295">No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="966de-295">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="966de-296">Por fim, compile a solução clicando **criar** | **compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="966de-296">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="966de-297">Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="966de-297">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="966de-298">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="966de-298">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="966de-299">É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.</span><span class="sxs-lookup"><span data-stu-id="966de-299">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="966de-300">Abra **StoreController** classe.</span><span class="sxs-lookup"><span data-stu-id="966de-300">Open **StoreController** class.</span></span> <span data-ttu-id="966de-301">Para fazer isso, no **Solution Explorer**, expanda o **controladores** pasta e clique duas vezes em **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="966de-301">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
4. <span data-ttu-id="966de-302">Alterar o **procurar** método, adicionando um parâmetro de cadeia de caracteres para solicitar um gênero específico.</span><span class="sxs-lookup"><span data-stu-id="966de-302">Change the **Browse** method, adding a string parameter to request for a specific genre.</span></span> <span data-ttu-id="966de-303">ASP.NET MVC será automaticamente passar qualquer querystring ou parâmetros nomeados de postagem de formulário **gênero** para este método de ação quando invocado.</span><span class="sxs-lookup"><span data-stu-id="966de-303">ASP.NET MVC will automatically pass any querystring or form post parameters named **genre** to this action method when invoked.</span></span> <span data-ttu-id="966de-304">Para fazer isso, substitua o **procurar** método com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="966de-304">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="966de-305">(Código de trecho - *conceitos básicos do ASP.NET MVC 4 - Ex3 StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="966de-305">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController BrowseMethod*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

    > [!NOTE]
    > <span data-ttu-id="966de-306">Você está usando o **HttpUtility** método utilitário impede os usuários de injeção de Javascript no modo de exibição com um link como   **/repositório/procurar? Gênero =&lt;script&gt;window.location='[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;**.</span><span class="sxs-lookup"><span data-stu-id="966de-306">You are using the **HttpUtility.HtmlEncode** utility method to prevents users from injecting Javascript into the View with a link like **/Store/Browse?Genre=&lt;script&gt;window.location='[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;**.</span></span>
    > 
    > <span data-ttu-id="966de-307">Para obter mais explicações, visite [este artigo do msdn](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).</span><span class="sxs-lookup"><span data-stu-id="966de-307">For further explanation, please visit [this msdn article](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="966de-308">Tarefa 2: executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="966de-308">Task 2 - Running the Application</span></span>

<span data-ttu-id="966de-309">Nesta tarefa, você testar o aplicativo em um navegador da web e usará o **gênero** parâmetro.</span><span class="sxs-lookup"><span data-stu-id="966de-309">In this task, you will try out the Application in a web browser and use the **genre** parameter.</span></span>

1. <span data-ttu-id="966de-310">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="966de-310">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="966de-311">O projeto é iniciado **início** página.</span><span class="sxs-lookup"><span data-stu-id="966de-311">The project starts in the **Home** page.</span></span> <span data-ttu-id="966de-312">Altere a URL para   */armazenar/procurar? Gênero = Disco* para verificar se a ação recebe o parâmetro de gênero.</span><span class="sxs-lookup"><span data-stu-id="966de-312">Change the URL to */Store/Browse?Genre=Disco* to verify that the action receives the genre parameter.</span></span>

    <span data-ttu-id="966de-313">![Navegação StoreBrowseGenre = Disco](aspnet-mvc-4-fundamentals/_static/image10.png "navegação StoreBrowseGenre = Disco")</span><span class="sxs-lookup"><span data-stu-id="966de-313">![Browsing StoreBrowseGenre=Disco](aspnet-mvc-4-fundamentals/_static/image10.png "Browsing StoreBrowseGenre=Disco")</span></span>

    <span data-ttu-id="966de-314">*Navegação /Store/Browse? Gênero = Disco*</span><span class="sxs-lookup"><span data-stu-id="966de-314">*Browsing /Store/Browse?Genre=Disco*</span></span>
3. <span data-ttu-id="966de-315">Feche o navegador.</span><span class="sxs-lookup"><span data-stu-id="966de-315">Close the browser.</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a><span data-ttu-id="966de-316">Tarefa 3: adicionando um parâmetro de Id inserido na URL</span><span class="sxs-lookup"><span data-stu-id="966de-316">Task 3 - Adding an Id Parameter Embedded in the URL</span></span>

<span data-ttu-id="966de-317">Nesta tarefa, você usará o **URL** para passar um **Id** parâmetro para o **detalhes** método de ação a **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="966de-317">In this task, you will use the **URL** to pass an **Id** parameter to the **Details** action method of the **StoreController**.</span></span> <span data-ttu-id="966de-318">Padrão de ASP.NET MVC convenção de roteamento é tratar o segmento de URL após o nome do método de ação como um parâmetro denominado **Id**. Se seu método de ação tem um parâmetro chamado Id, em seguida, ASP.NET MVC automaticamente passará o segmento de URL para você como um parâmetro.</span><span class="sxs-lookup"><span data-stu-id="966de-318">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named **Id**. If your action method has parameter named Id, then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span> <span data-ttu-id="966de-319">Na URL **repositório/detalhes/5**, **Id** será interpretado como **5**.</span><span class="sxs-lookup"><span data-stu-id="966de-319">In the URL **Store/Details/5**, **Id** will be interpreted as **5**.</span></span>

1. <span data-ttu-id="966de-320">Alterar o **detalhes** método o **StoreController**, adicionando um **int** parâmetro chamado **id**. Para fazer isso, substitua o **detalhes** método com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="966de-320">Change the **Details** method of the **StoreController**, adding an **int** parameter called **id**. To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="966de-321">(Código de trecho - *conceitos básicos do ASP.NET MVC 4 - Ex3 StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="966de-321">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController DetailsMethod*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="966de-322">Tarefa 4 - executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="966de-322">Task 4 - Running the Application</span></span>

<span data-ttu-id="966de-323">Nesta tarefa, você testar o aplicativo em um navegador da web e usará o **Id** parâmetro.</span><span class="sxs-lookup"><span data-stu-id="966de-323">In this task, you will try out the Application in a web browser and use the **Id** parameter.</span></span>

1. <span data-ttu-id="966de-324">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="966de-324">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="966de-325">O projeto é iniciado **início** página.</span><span class="sxs-lookup"><span data-stu-id="966de-325">The project starts in the **Home** page.</span></span> <span data-ttu-id="966de-326">Altere a URL para */Store/Details/5* para verificar se a ação recebe o parâmetro de id.</span><span class="sxs-lookup"><span data-stu-id="966de-326">Change the URL to */Store/Details/5* to verify that the action receives the id parameter.</span></span>

    <span data-ttu-id="966de-327">![Navegação StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "StoreDetails5 de navegação")</span><span class="sxs-lookup"><span data-stu-id="966de-327">![Browsing StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "Browsing StoreDetails5")</span></span>

    <span data-ttu-id="966de-328">*Navegação /Store/Details/5*</span><span class="sxs-lookup"><span data-stu-id="966de-328">*Browsing /Store/Details/5*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a><span data-ttu-id="966de-329">Exercício 4: Criar um modo de exibição</span><span class="sxs-lookup"><span data-stu-id="966de-329">Exercise 4: Creating a View</span></span>

<span data-ttu-id="966de-330">Até você ter foi retornando cadeias de caracteres de ações do controlador.</span><span class="sxs-lookup"><span data-stu-id="966de-330">So far you have been returning strings from controller actions.</span></span> <span data-ttu-id="966de-331">Embora o que é uma maneira útil entender como funcionam os controladores, não é como seus aplicativos Web reais são criados.</span><span class="sxs-lookup"><span data-stu-id="966de-331">Although that is a useful way of understanding how controllers work, it is not how your real Web applications are built.</span></span> <span data-ttu-id="966de-332">Modos de exibição são os componentes que fornecem uma abordagem melhor para gerar HTML para o navegador com o uso de arquivos de modelo.</span><span class="sxs-lookup"><span data-stu-id="966de-332">Views are components that provide a better approach for generating HTML back to the browser with the use of template files.</span></span>

<span data-ttu-id="966de-333">Neste exercício, você aprenderá como adicionar uma página de layout mestre para configurar um modelo de conteúdo HTML comum, uma folha de estilos para aprimorar a aparência do site e, finalmente, um modelo de exibição para habilitar o HomeController retornar o HTML.</span><span class="sxs-lookup"><span data-stu-id="966de-333">In this exercise you will learn how to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel of the site and finally a View template to enable HomeController to return HTML.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-layoutcshtml"></a><span data-ttu-id="966de-334">Tarefa 1: modificando o arquivo \_cshtml</span><span class="sxs-lookup"><span data-stu-id="966de-334">Task 1 - Modifying the file \_layout.cshtml</span></span>

<span data-ttu-id="966de-335">O arquivo **~/Views/Shared/\_cshtml** permite que você configurar um modelo para HTML comuns para uso em todo o site.</span><span class="sxs-lookup"><span data-stu-id="966de-335">The file **~/Views/Shared/\_layout.cshtml** allows you to setup a template for common HTML to use across the entire website.</span></span> <span data-ttu-id="966de-336">Nesta tarefa, você adicionará uma página de layout mestre com um cabeçalho comuns com links para a área página inicial e o repositório.</span><span class="sxs-lookup"><span data-stu-id="966de-336">In this task you will add a layout master page with a common header with links to the Home page and Store area.</span></span>

1. <span data-ttu-id="966de-337">Se não estiver aberto, inicie **VS Express para Web**.</span><span class="sxs-lookup"><span data-stu-id="966de-337">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="966de-338">No **arquivo** menu, escolha **Abrir projeto**.</span><span class="sxs-lookup"><span data-stu-id="966de-338">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="966de-339">Na caixa de diálogo Abrir projeto, vá para **Source\Ex04 CreatingAView\Begin**, selecione **Begin.sln** e clique em **abrir**.</span><span class="sxs-lookup"><span data-stu-id="966de-339">In the Open Project dialog, browse to **Source\Ex04-CreatingAView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="966de-340">Como alternativa, você pode continuar com a solução que você obteve depois de concluir o exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="966de-340">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

    1. <span data-ttu-id="966de-341">Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="966de-341">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="966de-342">Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="966de-342">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="966de-343">No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="966de-343">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="966de-344">Por fim, compile a solução clicando **criar** | **compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="966de-344">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="966de-345">Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="966de-345">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="966de-346">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="966de-346">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="966de-347">É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.</span><span class="sxs-lookup"><span data-stu-id="966de-347">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="966de-348">O arquivo  **\_cshtml** contém o layout de contêiner HTML para todas as páginas no site.</span><span class="sxs-lookup"><span data-stu-id="966de-348">The file **\_layout.cshtml** contains the HTML container layout for all pages on the site.</span></span> <span data-ttu-id="966de-349">Ele inclui o  **&lt;html&gt;**  elemento para a resposta HTML, bem como o  **&lt;head&gt;**  e  **&lt;corpo&gt;**  elementos.</span><span class="sxs-lookup"><span data-stu-id="966de-349">It includes the **&lt;html&gt;** element for the HTML response, as well as the **&lt;head&gt;** and **&lt;body&gt;** elements.</span></span> <span data-ttu-id="966de-350">**@RenderBody()** em HTML corpo identificar regiões exibição modelos poderão ser preenchidos com conteúdo dinâmico.</span><span class="sxs-lookup"><span data-stu-id="966de-350">**@RenderBody()** within the HTML body identify regions that view templates will be able to fill in with dynamic content.</span></span>
<span data-ttu-id="966de-351">(C#)</span><span class="sxs-lookup"><span data-stu-id="966de-351">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. <span data-ttu-id="966de-352">Adicione um cabeçalho comuns com links para a área página inicial e o armazenamento em todas as páginas no site.</span><span class="sxs-lookup"><span data-stu-id="966de-352">Add a common header with links to the Home page and Store area on all pages in the site.</span></span> <span data-ttu-id="966de-353">Para fazer isso, adicione o seguinte código abaixo &lt;corpo&gt; instrução.</span><span class="sxs-lookup"><span data-stu-id="966de-353">In order to do that, add the following code below &lt;body&gt; statement.</span></span>
<span data-ttu-id="966de-354">(C#)</span><span class="sxs-lookup"><span data-stu-id="966de-354">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. <span data-ttu-id="966de-355">Inclua um div para renderizar a seção de corpo de cada página.</span><span class="sxs-lookup"><span data-stu-id="966de-355">Include a div to render the body section of each page.</span></span> <span data-ttu-id="966de-356">Substituir  **@RenderBody()** com o código a seguir higlighted: (c#)</span><span class="sxs-lookup"><span data-stu-id="966de-356">Replace **@RenderBody()** with the following higlighted code: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="966de-357">Você sabia?</span><span class="sxs-lookup"><span data-stu-id="966de-357">Did you know?</span></span> <span data-ttu-id="966de-358">O Visual Studio 2012 tem trechos de código que tornam mais fácil adicionar código de uso geral no HTML, arquivos de código e muito mais!</span><span class="sxs-lookup"><span data-stu-id="966de-358">Visual Studio 2012 has snippets that make it easy to add commonly used code in HTML, code files and more!</span></span> <span data-ttu-id="966de-359">Experimentar digitando  **&lt;div&gt;**  e pressionando **guia** duas vezes para inserir uma completa **div** marca.</span><span class="sxs-lookup"><span data-stu-id="966de-359">Try it out by typing **&lt;div&gt;** and pressing **TAB** twice to insert a complete **div** tag.</span></span>

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a><span data-ttu-id="966de-360">Tarefa 2 - adicionar folha de estilos CSS</span><span class="sxs-lookup"><span data-stu-id="966de-360">Task 2 - Adding CSS Stylesheet</span></span>

<span data-ttu-id="966de-361">O modelo de projeto vazio inclui um arquivo CSS muito simplificado que inclui apenas os estilos usados para exibir mensagens de validação e formas básicas.</span><span class="sxs-lookup"><span data-stu-id="966de-361">The empty project template includes a very streamlined CSS file which just includes styles used to display basic forms and validation messages.</span></span> <span data-ttu-id="966de-362">Você usará o CSS e imagens (potencialmente fornecidas por um designer) adicionais para melhorar a aparência do site.</span><span class="sxs-lookup"><span data-stu-id="966de-362">You will use additional CSS and images (potentially provided by a designer) in order to enhance the look and feel of the site.</span></span>

<span data-ttu-id="966de-363">Nesta tarefa, você adicionará uma folha de estilos CSS para definir os estilos do site.</span><span class="sxs-lookup"><span data-stu-id="966de-363">In this task, you will add a CSS stylesheet to define the styles of the site.</span></span>

1. <span data-ttu-id="966de-364">O arquivo CSS e imagens são incluídas na **Source\Assets\Content** pasta deste laboratório.</span><span class="sxs-lookup"><span data-stu-id="966de-364">The CSS file and images are included in the **Source\Assets\Content** folder of this Lab.</span></span> <span data-ttu-id="966de-365">Para adicioná-las para o aplicativo, arraste o conteúdo de um **Windows Explorer** janela para o **Solution Explorer** no Visual Studio Express para Web, conforme mostrado abaixo:</span><span class="sxs-lookup"><span data-stu-id="966de-365">In order to add them to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Studio Express for Web, as shown below:</span></span>

    <span data-ttu-id="966de-366">![Arrastando o conteúdo de estilo](aspnet-mvc-4-fundamentals/_static/image12.png "arrastando o conteúdo de estilo")</span><span class="sxs-lookup"><span data-stu-id="966de-366">![Dragging style contents](aspnet-mvc-4-fundamentals/_static/image12.png "Dragging style contents")</span></span>

    <span data-ttu-id="966de-367">*Arrastando o conteúdo de estilo*</span><span class="sxs-lookup"><span data-stu-id="966de-367">*Dragging style contents*</span></span>
2. <span data-ttu-id="966de-368">Uma caixa de diálogo de aviso será exibida, solicitando a confirmação substituir **Site.css** de arquivo e algumas imagens existentes.</span><span class="sxs-lookup"><span data-stu-id="966de-368">A warning dialog will appear, asking for confirmation to replace **Site.css** file and some existing images.</span></span> <span data-ttu-id="966de-369">Verificar **aplicar a todos os itens** e clique em **Sim**.</span><span class="sxs-lookup"><span data-stu-id="966de-369">Check **Apply to all items** and click **Yes**.</span></span>

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a><span data-ttu-id="966de-370">Tarefa 3: adicionando um modelo de exibição</span><span class="sxs-lookup"><span data-stu-id="966de-370">Task 3 - Adding a View Template</span></span>

<span data-ttu-id="966de-371">Nesta tarefa, você adicionará um modelo de exibição para gerar a resposta HTML que usará a página mestra layout e CSS adicionados neste exercício.</span><span class="sxs-lookup"><span data-stu-id="966de-371">In this task, you will add a View template to generate the HTML response that will use the layout master page and CSS added in this exercise.</span></span>

1. <span data-ttu-id="966de-372">Para usar um modelo de exibição durante a navegação da home page do site, primeiro será necessário indicar que em vez de retornar uma cadeia de caracteres, o **índice HomeController** método retornará um **exibição**.</span><span class="sxs-lookup"><span data-stu-id="966de-372">To use a View template when browsing the site's home page, you will first need to indicate that instead of returning a string, the **HomeController Index** method will return a **View**.</span></span> <span data-ttu-id="966de-373">Abra **HomeController** classe e altere seu **índice** método para retornar um **ActionResult**, e ainda retornar **View()**.</span><span class="sxs-lookup"><span data-stu-id="966de-373">Open **HomeController** class and change its **Index** method to return an **ActionResult**, and have it return **View()**.</span></span>

    <span data-ttu-id="966de-374">(Código de trecho - *conceitos básicos do ASP.NET MVC 4 - Ex4 HomeController índice*)</span><span class="sxs-lookup"><span data-stu-id="966de-374">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex4 HomeController Index*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
2. <span data-ttu-id="966de-375">Agora, você precisa adicionar um modelo de exibição apropriado.</span><span class="sxs-lookup"><span data-stu-id="966de-375">Now, you need to add an appropriate View template.</span></span> <span data-ttu-id="966de-376">Para fazer isso, **clique** dentro de **índice** método de ação e selecione **adicionar exibição**.</span><span class="sxs-lookup"><span data-stu-id="966de-376">To do this, **right-click** inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="966de-377">Isso abrirá o **adicionar exibição** caixa de diálogo.</span><span class="sxs-lookup"><span data-stu-id="966de-377">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="966de-378">![Adicionando uma exibição de dentro do método de índice](aspnet-mvc-4-fundamentals/_static/image13.png "adicionando uma exibição de dentro do método de índice")</span><span class="sxs-lookup"><span data-stu-id="966de-378">![Adding a View from within the Index method](aspnet-mvc-4-fundamentals/_static/image13.png "Adding a View from within the Index method")</span></span>

    <span data-ttu-id="966de-379">*Adicionando uma exibição de dentro do método de índice*</span><span class="sxs-lookup"><span data-stu-id="966de-379">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="966de-380">O **adicionar exibição** caixa de diálogo aparecerá gerar um arquivo de modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="966de-380">The **Add View** Dialog will appear to generate a View template file.</span></span> <span data-ttu-id="966de-381">Por padrão, esta caixa de diálogo popula previamente o nome do modelo de exibição para que ele corresponda o método de ação que irá usá-la.</span><span class="sxs-lookup"><span data-stu-id="966de-381">By default, this dialog pre-populates the name of the View template so that it matches the action method that will use it.</span></span> <span data-ttu-id="966de-382">Porque você usou o **adicionar modo de exibição** menu de contexto dentro de **índice** método de ação no HomeController, o **adicionar modo de exibição** caixa de diálogo possui um índice como o nome de exibição padrão.</span><span class="sxs-lookup"><span data-stu-id="966de-382">Because you used the **Add View** context menu within the **Index** action method within the HomeController, the **Add View** dialog has Index as the default view name.</span></span> <span data-ttu-id="966de-383">Clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="966de-383">Click **Add**.</span></span>

    <span data-ttu-id="966de-384">![Exibição de caixa de diálogo Adicionar](aspnet-mvc-4-fundamentals/_static/image14.png "exibição de caixa de diálogo Adicionar")</span><span class="sxs-lookup"><span data-stu-id="966de-384">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image14.png "Add View Dialog")</span></span>

    <span data-ttu-id="966de-385">*Adicionar caixa de diálogo de exibição*</span><span class="sxs-lookup"><span data-stu-id="966de-385">*Add View Dialog*</span></span>
4. <span data-ttu-id="966de-386">O Visual Studio gera um **cshtml** exibir modelo dentro do **Views\Home** pasta e, em seguida, abre.</span><span class="sxs-lookup"><span data-stu-id="966de-386">Visual Studio generates an **Index.cshtml** view template inside the **Views\Home** folder and then opens it.</span></span>

    <span data-ttu-id="966de-387">![Início de exibição do índice criado](aspnet-mvc-4-fundamentals/_static/image15.png "exibição Home índice criado")</span><span class="sxs-lookup"><span data-stu-id="966de-387">![Home Index view created](aspnet-mvc-4-fundamentals/_static/image15.png "Home Index view created")</span></span>

    <span data-ttu-id="966de-388">*Exibição de índice inicial criada*</span><span class="sxs-lookup"><span data-stu-id="966de-388">*Home Index view created*</span></span>

    > [!NOTE]
    > <span data-ttu-id="966de-389">nome e local do **cshtml** arquivo relevante e segue as convenções de nomenclatura padrão ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="966de-389">name and location of the **Index.cshtml** file is relevant and follows the default ASP.NET MVC naming conventions.</span></span>
    > 
    > <span data-ttu-id="966de-390">A pasta \Views\**início** corresponde ao nome de controlador (**início** controlador).</span><span class="sxs-lookup"><span data-stu-id="966de-390">The folder \Views\**Home** matches the controller name (**Home** Controller).</span></span> <span data-ttu-id="966de-391">O nome do modelo de exibição (**índice**), coincide com o método de ação do controlador que deseja exibir o modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="966de-391">The View template name (**Index**), matches the controller action method which will be displaying the View.</span></span>
    > 
    > <span data-ttu-id="966de-392">Dessa forma, o ASP.NET MVC evita a necessidade de especificar explicitamente o nome ou local de um modelo de exibição ao usar essa convenção de nomenclatura para retornar uma exibição.</span><span class="sxs-lookup"><span data-stu-id="966de-392">This way, ASP.NET MVC avoids having to explicitly specify the name or location of a View template when using this naming convention to return a View.</span></span>
5. <span data-ttu-id="966de-393">O modelo de exibição gerado se baseia o  **\_cshtml** modelo definido anteriormente.</span><span class="sxs-lookup"><span data-stu-id="966de-393">The generated View template is based on the **\_layout.cshtml** template earlier defined.</span></span> <span data-ttu-id="966de-394">Atualizar a propriedade ViewBag Title para **início**e alterar o conteúdo principal para **esta é a Home Page**, conforme mostrado no código a seguir:</span><span class="sxs-lookup"><span data-stu-id="966de-394">Update the ViewBag.Title property to **Home**, and change the main content to **This is the Home Page**, as shown in the code below:</span></span>


    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
6. <span data-ttu-id="966de-395">Selecione **MvcMusicStore** projeto no Gerenciador de soluções e pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="966de-395">Select **MvcMusicStore** project in the Solution Explorer and Press **F5** to run the Application.</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a><span data-ttu-id="966de-396">Tarefa 4: verificação</span><span class="sxs-lookup"><span data-stu-id="966de-396">Task 4: Verification</span></span>

<span data-ttu-id="966de-397">Para verificar que você executou corretamente todas as etapas no exercício anterior, faça o seguinte:</span><span class="sxs-lookup"><span data-stu-id="966de-397">In order to verify that you have correctly performed all the steps in the previous exercise, proceed as follows:</span></span>

<span data-ttu-id="966de-398">Com o aplicativo aberto em um navegador, você deve observar que:</span><span class="sxs-lookup"><span data-stu-id="966de-398">With the application opened in a browser, you should note that:</span></span>

1. <span data-ttu-id="966de-399">Método de ação de índice do HomeController encontrado e exibidos a **\Views\Home\Index.cshtml** exibir modelo, mesmo que o código chamado **retornar View()**, pois o modelo de exibição seguido de convenção de nomenclatura padrão.</span><span class="sxs-lookup"><span data-stu-id="966de-399">The HomeController's Index action method found and displayed the **\Views\Home\Index.cshtml** View template, even though the code called **return View()**, because the View template followed the standard naming convention.</span></span>
2. <span data-ttu-id="966de-400">A Home Page exibe a mensagem de boas-vinda definida dentro de **\Views\Home\Index.cshtml** modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="966de-400">The Home Page displays the welcome message defined within the **\Views\Home\Index.cshtml** view template.</span></span>
3. <span data-ttu-id="966de-401">A Home Page está usando o  **\_cshtml** modelo, e, portanto, a mensagem de boas-vinda está contida dentro do layout do site padrão HTML.</span><span class="sxs-lookup"><span data-stu-id="966de-401">The Home Page is using the **\_layout.cshtml** template, and so the welcome message is contained within the standard site HTML layout.</span></span>

    <span data-ttu-id="966de-402">![Índice de exibição usando o LayoutPage definido e o estilo de início](aspnet-mvc-4-fundamentals/_static/image16.png "índice visualização inicial usando o LayoutPage e o estilo definido")</span><span class="sxs-lookup"><span data-stu-id="966de-402">![Home Index View using the defined LayoutPage and style](aspnet-mvc-4-fundamentals/_static/image16.png "Home Index View using the defined LayoutPage and style")</span></span>

    <span data-ttu-id="966de-403">*Exibição de índice inicial usando o LayoutPage e o estilo definido*</span><span class="sxs-lookup"><span data-stu-id="966de-403">*Home Index View using the defined LayoutPage and style*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a><span data-ttu-id="966de-404">Exercício 5: Criar um modelo de exibição</span><span class="sxs-lookup"><span data-stu-id="966de-404">Exercise 5: Creating a View Model</span></span>

<span data-ttu-id="966de-405">Até agora, feitas seus modos de exibição exibem inserido no código HTML, mas para criar aplicativos web dinâmicos, o modelo de exibição deve receber informações do controlador.</span><span class="sxs-lookup"><span data-stu-id="966de-405">So far, you made your Views display hardcoded HTML, but, in order to create dynamic web applications, the View template should receive information from the Controller.</span></span> <span data-ttu-id="966de-406">Uma técnica comum a ser usado para essa finalidade é a **ViewModel** padrão, que permite que um controlador empacotar todas as informações necessárias para gerar a resposta HTML apropriada.</span><span class="sxs-lookup"><span data-stu-id="966de-406">One common technique to be used for that purpose is the **ViewModel** pattern, which allows a Controller to package up all the information needed to generate the appropriate HTML response.</span></span>

<span data-ttu-id="966de-407">Neste exercício, você aprenderá como criar uma classe ViewModel e adicionar as propriedades necessárias: o número de gêneros o armazenamento e uma lista desses gêneros.</span><span class="sxs-lookup"><span data-stu-id="966de-407">In this exercise, you will learn how to create a ViewModel class and add the required properties: the number of genres in the store and a list of those genres.</span></span> <span data-ttu-id="966de-408">Você também atualizará o StoreController para usar o ViewModel criado e por fim, você criará um novo modelo de exibição que exibirá as propriedades mencionadas na página.</span><span class="sxs-lookup"><span data-stu-id="966de-408">You will also update the StoreController to use the created ViewModel, and finally, you will create a new View template that will display the mentioned properties in the page.</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a><span data-ttu-id="966de-409">Tarefa 1: Criando uma classe ViewModel</span><span class="sxs-lookup"><span data-stu-id="966de-409">Task 1 - Creating a ViewModel Class</span></span>

<span data-ttu-id="966de-410">Nesta tarefa, você criará uma classe ViewModel implementa o cenário de listagem de gênero do repositório.</span><span class="sxs-lookup"><span data-stu-id="966de-410">In this task, you will create a ViewModel class that will implement the Store genre listing scenario.</span></span>

1. <span data-ttu-id="966de-411">Se não estiver aberto, inicie **VS Express para Web**.</span><span class="sxs-lookup"><span data-stu-id="966de-411">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="966de-412">No **arquivo** menu, escolha **Abrir projeto**.</span><span class="sxs-lookup"><span data-stu-id="966de-412">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="966de-413">Na caixa de diálogo Abrir projeto, vá para **Source\Ex05 CreatingAViewModel\Begin**, selecione **Begin.sln** e clique em **abrir**.</span><span class="sxs-lookup"><span data-stu-id="966de-413">In the Open Project dialog, browse to **Source\Ex05-CreatingAViewModel\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="966de-414">Como alternativa, você pode continuar com a solução que você obteve depois de concluir o exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="966de-414">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

    1. <span data-ttu-id="966de-415">Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="966de-415">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="966de-416">Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="966de-416">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="966de-417">No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="966de-417">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="966de-418">Por fim, compile a solução clicando **criar** | **compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="966de-418">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="966de-419">Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="966de-419">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="966de-420">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="966de-420">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="966de-421">É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.</span><span class="sxs-lookup"><span data-stu-id="966de-421">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="966de-422">Criar um **ViewModels** pasta para manter o ViewModel.</span><span class="sxs-lookup"><span data-stu-id="966de-422">Create a **ViewModels** folder to hold the ViewModel.</span></span> <span data-ttu-id="966de-423">Para fazer isso, clique com botão direito de nível superior **MvcMusicStore** projeto, selecione **adicionar** e **nova pasta**.</span><span class="sxs-lookup"><span data-stu-id="966de-423">To do this, right-click the top-level **MvcMusicStore** project, select **Add** and then **New Folder**.</span></span>

    <span data-ttu-id="966de-424">![Adicionando uma nova pasta](aspnet-mvc-4-fundamentals/_static/image17.png "adicionando uma nova pasta")</span><span class="sxs-lookup"><span data-stu-id="966de-424">![Adding a new folder](aspnet-mvc-4-fundamentals/_static/image17.png "Adding a new folder")</span></span>

    <span data-ttu-id="966de-425">*Adicionando uma nova pasta*</span><span class="sxs-lookup"><span data-stu-id="966de-425">*Adding a new folder*</span></span>
4. <span data-ttu-id="966de-426">O nome da pasta *ViewModels*.</span><span class="sxs-lookup"><span data-stu-id="966de-426">Name the folder *ViewModels*.</span></span>

    <span data-ttu-id="966de-427">![Pasta ViewModels no Gerenciador de soluções](aspnet-mvc-4-fundamentals/_static/image18.png "ViewModels pasta no Gerenciador de soluções")</span><span class="sxs-lookup"><span data-stu-id="966de-427">![ViewModels folder in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image18.png "ViewModels folder in Solution Explorer")</span></span>

    <span data-ttu-id="966de-428">*Pasta ViewModels no Gerenciador de soluções*</span><span class="sxs-lookup"><span data-stu-id="966de-428">*ViewModels folder in Solution Explorer*</span></span>
5. <span data-ttu-id="966de-429">Criar um **ViewModel** classe.</span><span class="sxs-lookup"><span data-stu-id="966de-429">Create a **ViewModel** class.</span></span> <span data-ttu-id="966de-430">Para fazer isso, clique duas vezes no **ViewModels** pasta criada recentemente, selecione **adicionar** e **Novo Item**.</span><span class="sxs-lookup"><span data-stu-id="966de-430">To do this, right-click on the **ViewModels** folder recently created, select **Add** and then **New Item**.</span></span> <span data-ttu-id="966de-431">Em **código**, escolha o **classe** item e nomeie o arquivo *StoreIndexViewModel.cs*, em seguida, clique em **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="966de-431">Under **Code**, choose the **Class** item and name the file *StoreIndexViewModel.cs*, then click **Add**.</span></span>

    <span data-ttu-id="966de-432">![Adicionando uma nova classe](aspnet-mvc-4-fundamentals/_static/image19.png "adicionando uma nova classe")</span><span class="sxs-lookup"><span data-stu-id="966de-432">![Adding a new Class](aspnet-mvc-4-fundamentals/_static/image19.png "Adding a new Class")</span></span>

    <span data-ttu-id="966de-433">*Adicionando uma nova classe*</span><span class="sxs-lookup"><span data-stu-id="966de-433">*Adding a new Class*</span></span>

    <span data-ttu-id="966de-434">![Criando classe StoreIndexViewModel](aspnet-mvc-4-fundamentals/_static/image20.png "StoreIndexViewModel criação de classe")</span><span class="sxs-lookup"><span data-stu-id="966de-434">![Creating StoreIndexViewModel class](aspnet-mvc-4-fundamentals/_static/image20.png "Creating StoreIndexViewModel class")</span></span>

    <span data-ttu-id="966de-435">*Criando classe StoreIndexViewModel*</span><span class="sxs-lookup"><span data-stu-id="966de-435">*Creating StoreIndexViewModel class*</span></span>

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a><span data-ttu-id="966de-436">Tarefa 2: Adicionando propriedades para a classe ViewModel</span><span class="sxs-lookup"><span data-stu-id="966de-436">Task 2 - Adding Properties to the ViewModel class</span></span>

<span data-ttu-id="966de-437">Há dois parâmetros a serem passados do StoreController para o modelo de exibição para gerar a resposta HTML esperada: o número de gêneros o armazenamento e uma lista desses gêneros.</span><span class="sxs-lookup"><span data-stu-id="966de-437">There are two parameters to be passed from the StoreController to the View template in order to generate the expected HTML response: the number of genres in the store and a list of those genres.</span></span>

<span data-ttu-id="966de-438">Nesta tarefa, você adicionará essas 2 propriedades para o **StoreIndexViewModel** classe: **NumberOfGenres** (um inteiro) e **gêneros** (uma lista de cadeias de caracteres).</span><span class="sxs-lookup"><span data-stu-id="966de-438">In this task, you will add those 2 properties to the **StoreIndexViewModel** class: **NumberOfGenres** (an integer) and **Genres** (a list of strings).</span></span>

1. <span data-ttu-id="966de-439">Adicionar **NumberOfGenres** e **gêneros** propriedades para o **StoreIndexViewModel** classe.</span><span class="sxs-lookup"><span data-stu-id="966de-439">Add **NumberOfGenres** and **Genres** properties to the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="966de-440">Para fazer isso, adicione as seguintes 2 linhas à definição de classe:</span><span class="sxs-lookup"><span data-stu-id="966de-440">To do this, add the following 2 lines to the class definition:</span></span>

    <span data-ttu-id="966de-441">(Código de trecho - *ASP.NET MVC 4 conceitos básicos - propriedades de Ex5 StoreIndexViewModel*)</span><span class="sxs-lookup"><span data-stu-id="966de-441">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel properties*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="966de-442">O **{get; definir;}**  notação utiliza do # recurso propriedades implementadas automaticamente.</span><span class="sxs-lookup"><span data-stu-id="966de-442">The **{ get; set; }** notation makes use of C#'s auto-implemented properties feature.</span></span> <span data-ttu-id="966de-443">Ele fornece os benefícios de uma propriedade sem a necessidade de nós declarar um campo existente.</span><span class="sxs-lookup"><span data-stu-id="966de-443">It provides the benefits of a property without requiring us to declare a backing field.</span></span>

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a><span data-ttu-id="966de-444">Tarefa 3: atualização StoreController para usar o StoreIndexViewModel</span><span class="sxs-lookup"><span data-stu-id="966de-444">Task 3 - Updating StoreController to use the StoreIndexViewModel</span></span>

<span data-ttu-id="966de-445">O **StoreIndexViewModel** classe encapsula as informações necessárias para passar de **StoreController**do **índice** método para um modelo de exibição para gerar uma resposta .</span><span class="sxs-lookup"><span data-stu-id="966de-445">The **StoreIndexViewModel** class encapsulates the information needed to pass from **StoreController**'s **Index** method to a View template in order to generate a response.</span></span>

<span data-ttu-id="966de-446">Nesta tarefa, você atualizará o **StoreController** para usar o **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="966de-446">In this task, you will update the **StoreController** to use the **StoreIndexViewModel**.</span></span>

1. <span data-ttu-id="966de-447">Abra **StoreController** classe.</span><span class="sxs-lookup"><span data-stu-id="966de-447">Open **StoreController** class.</span></span>

    <span data-ttu-id="966de-448">![Abrir classe StoreController](aspnet-mvc-4-fundamentals/_static/image21.png "StoreController de abertura de classe")</span><span class="sxs-lookup"><span data-stu-id="966de-448">![Opening StoreController class](aspnet-mvc-4-fundamentals/_static/image21.png "Opening StoreController class")</span></span>

    <span data-ttu-id="966de-449">*Classe de StoreController de abertura*</span><span class="sxs-lookup"><span data-stu-id="966de-449">*Opening StoreController class*</span></span>
2. <span data-ttu-id="966de-450">Para usar o **StoreIndexViewModel** classe o **StoreController**, adicione o seguinte namespace na parte superior do **StoreController** código:</span><span class="sxs-lookup"><span data-stu-id="966de-450">In order to use the **StoreIndexViewModel** class from the **StoreController**, add the following namespace at the top of the **StoreController** code:</span></span>

    <span data-ttu-id="966de-451">(Código de trecho - *ASP.NET MVC 4 conceitos básicos - StoreIndexViewModel Ex5 usando ViewModels*)</span><span class="sxs-lookup"><span data-stu-id="966de-451">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel using ViewModels*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
3. <span data-ttu-id="966de-452">Alterar o **StoreController**do **índice** método de ação para que ele cria e preenche uma **StoreIndexViewModel** de objeto e passa em um modelo de exibição para gere uma resposta HTML com ele.</span><span class="sxs-lookup"><span data-stu-id="966de-452">Change the **StoreController**'s **Index** action method so that it creates and populates a **StoreIndexViewModel** object and then passes it off to a View template to generate an HTML response with it.</span></span>

    > [!NOTE]
    > <span data-ttu-id="966de-453">Laboratório &quot;acesso a dados e modelos do ASP.NET MVC&quot; você escreverá código que recupera a lista de gêneros de armazenamento de um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="966de-453">In Lab &quot;ASP.NET MVC Models and Data Access&quot; you will write code that retrieves the list of store genres from a database.</span></span> <span data-ttu-id="966de-454">O código a seguir, você criará uma **lista** de gêneros dados fictícios que preencherão o **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="966de-454">In the following code, you will create a **List** of dummy data genres that will populate the **StoreIndexViewModel**.</span></span>
    > 
    > <span data-ttu-id="966de-455">Depois de criar e configurar o **StoreIndexViewModel** do objeto, ela será transmitida como um argumento para o **exibição** método.</span><span class="sxs-lookup"><span data-stu-id="966de-455">After creating and setting up the **StoreIndexViewModel** object, it will be passed as an argument to the **View** method.</span></span> <span data-ttu-id="966de-456">Isso indica que o modelo de exibição usará esse objeto para gerar uma resposta HTML com ele.</span><span class="sxs-lookup"><span data-stu-id="966de-456">This indicates that the View template will use that object to generate an HTML response with it.</span></span>
4. <span data-ttu-id="966de-457">Substitua o **índice** método com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="966de-457">Replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="966de-458">(Código de trecho - *ASP.NET MVC 4 conceitos básicos - método Ex5 StoreController índice*)</span><span class="sxs-lookup"><span data-stu-id="966de-458">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreController Index method*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

    > [!NOTE]
    > <span data-ttu-id="966de-459">Se você estiver familiarizado com c#, você pode presumir que usando **var** significa que o **viewModel** variável é associação tardia.</span><span class="sxs-lookup"><span data-stu-id="966de-459">If you're unfamiliar with C#, you may assume that using **var** means that the **viewModel** variable is late-bound.</span></span> <span data-ttu-id="966de-460">Que não está correta - o compilador c# está usando com base em atribuir à variável de inferência de tipo para determinar que **viewModel** é do tipo **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="966de-460">That's not correct - the C# compiler is using type-inference based on what you assign to the variable to determine that **viewModel** is of type **StoreIndexViewModel**.</span></span> <span data-ttu-id="966de-461">Além disso, ao compilar local **viewModel** variável como um **StoreIndexViewModel** tipo get verificação de tempo de compilação e suporte de editor de códigos do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="966de-461">Also, by compiling the local **viewModel** variable as a **StoreIndexViewModel** type you get compile-time checking and Visual Studio code-editor support.</span></span>

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a><span data-ttu-id="966de-462">Tarefa 4 - criar um modelo de exibição que usa StoreIndexViewModel</span><span class="sxs-lookup"><span data-stu-id="966de-462">Task 4 - Creating a View Template that Uses StoreIndexViewModel</span></span>

<span data-ttu-id="966de-463">Nesta tarefa, você criará um modelo de exibição que irá usar um objeto de StoreIndexViewModel passado do controlador para exibir uma lista de gêneros.</span><span class="sxs-lookup"><span data-stu-id="966de-463">In this task, you will create a View template that will use a StoreIndexViewModel object passed from the Controller to display a list of genres.</span></span>

1. <span data-ttu-id="966de-464">Antes de criar o novo modelo de exibição, vamos criar o projeto para que o **caixa de diálogo Adicionar modo de exibição** conhece o **StoreIndexViewModel** classe.</span><span class="sxs-lookup"><span data-stu-id="966de-464">Before creating the new View template, let's build the project so that the **Add View Dialog** knows about the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="966de-465">Compile o projeto selecionando o **criar** item de menu e, em seguida, **MvcMusicStore criar**.</span><span class="sxs-lookup"><span data-stu-id="966de-465">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="966de-466">![Compilar o projeto](aspnet-mvc-4-fundamentals/_static/image22.png "compilar o projeto")</span><span class="sxs-lookup"><span data-stu-id="966de-466">![Building the project](aspnet-mvc-4-fundamentals/_static/image22.png "Building the project")</span></span>

    <span data-ttu-id="966de-467">*Criar o projeto*</span><span class="sxs-lookup"><span data-stu-id="966de-467">*Building the project*</span></span>
2. <span data-ttu-id="966de-468">Crie um novo modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="966de-468">Create a new View template.</span></span> <span data-ttu-id="966de-469">Para fazer isso, clique dentro do **índice** método e selecione **adicionar exibição**.</span><span class="sxs-lookup"><span data-stu-id="966de-469">To do that, right-click inside the **Index** method and select **Add View**.</span></span>

    <span data-ttu-id="966de-470">![Adicionando uma exibição](aspnet-mvc-4-fundamentals/_static/image23.png "adicionando uma exibição")</span><span class="sxs-lookup"><span data-stu-id="966de-470">![Adding a View](aspnet-mvc-4-fundamentals/_static/image23.png "Adding a View")</span></span>

    <span data-ttu-id="966de-471">*Adicionando uma exibição*</span><span class="sxs-lookup"><span data-stu-id="966de-471">*Adding a View*</span></span>
3. <span data-ttu-id="966de-472">Porque o **caixa de diálogo Adicionar modo de exibição** foi invocado do **StoreController**, ele adicionará o modelo de exibição por padrão em um **\Views\Store\Index.cshtml** arquivo.</span><span class="sxs-lookup"><span data-stu-id="966de-472">Because the **Add View Dialog** was invoked from the **StoreController**, it will add the View template by default in a **\Views\Store\Index.cshtml** file.</span></span> <span data-ttu-id="966de-473">Verifique o **criar uma fortemente tipado-exibição** caixa de seleção e, em seguida, selecione **StoreIndexViewModel** como o **classe modelo**.</span><span class="sxs-lookup"><span data-stu-id="966de-473">Check the **Create a strongly-typed-view** checkbox and then select **StoreIndexViewModel** as the **Model class**.</span></span> <span data-ttu-id="966de-474">Além disso, certifique-se de que o mecanismo de exibição selecionado é **Razor**.</span><span class="sxs-lookup"><span data-stu-id="966de-474">Also, make sure that the View engine selected is **Razor**.</span></span> <span data-ttu-id="966de-475">Clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="966de-475">Click **Add**.</span></span>

    <span data-ttu-id="966de-476">![Exibição de caixa de diálogo Adicionar](aspnet-mvc-4-fundamentals/_static/image24.png "exibição de caixa de diálogo Adicionar")</span><span class="sxs-lookup"><span data-stu-id="966de-476">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image24.png "Add View Dialog")</span></span>

    <span data-ttu-id="966de-477">*Adicionar caixa de diálogo de exibição*</span><span class="sxs-lookup"><span data-stu-id="966de-477">*Add View Dialog*</span></span>

    <span data-ttu-id="966de-478">O **\Views\Store\Index.cshtml** exibir o arquivo de modelo é criado e aberto.</span><span class="sxs-lookup"><span data-stu-id="966de-478">The **\Views\Store\Index.cshtml** View template file is created and opened.</span></span> <span data-ttu-id="966de-479">Com base nas informações fornecidas para o **adicionar exibição** caixa de diálogo na última etapa, a exibição de modelo espera um **StoreIndexViewModel** instância como os dados a ser usado para gerar uma resposta HTML.</span><span class="sxs-lookup"><span data-stu-id="966de-479">Based on the information provided to the **Add View** dialog in the last step, the View template will expect a **StoreIndexViewModel** instance as the data to use to generate an HTML response.</span></span> <span data-ttu-id="966de-480">Você observará que o modelo herda um `ViewPage<musicstore.viewmodels.storeindexviewmodel>` em c#.</span><span class="sxs-lookup"><span data-stu-id="966de-480">You will notice that the template inherits a `ViewPage<musicstore.viewmodels.storeindexviewmodel>` in C#.</span></span>

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a><span data-ttu-id="966de-481">Tarefa 5: atualizando o modelo de exibição</span><span class="sxs-lookup"><span data-stu-id="966de-481">Task 5 - Updating the View Template</span></span>

<span data-ttu-id="966de-482">Nesta tarefa, você atualizará o modelo de exibição criado na última tarefa ao recuperar o número de gêneros e seus nomes dentro da página.</span><span class="sxs-lookup"><span data-stu-id="966de-482">In this task, you will update the View template created in the last task to retrieve the number of genres and their names within the page.</span></span>

> [!NOTE]
> <span data-ttu-id="966de-483">Você usará a sintaxe @ (conhecida como &quot;código novidades sobre&quot;) para executar o código dentro do modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="966de-483">You will use @ syntax (often referred to as &quot;code nuggets&quot;) to execute code within the View template.</span></span>


1. <span data-ttu-id="966de-484">No **cshtml** arquivo, dentro de **repositório** pasta, substitua o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="966de-484">In the **Index.cshtml** file, within the **Store** folder, replace its code with the following:</span></span>


    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample14.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="966de-485">Assim que terminar de digitar o período após a palavra **modelo**, Intellisense do Visual Studio mostrará uma lista de possíveis propriedades e métodos disponíveis.</span><span class="sxs-lookup"><span data-stu-id="966de-485">As soon as you finish typing the period after the word **Model**, Visual Studio's Intellisense will show a list of possible properties and methods to choose from.</span></span>
    > 
    > ![](aspnet-mvc-4-fundamentals/_static/image25.png)
    > 
    > <span data-ttu-id="966de-486">*Obter propriedades de modelo e os métodos com o IntelliSense do Visual Studio*</span><span class="sxs-lookup"><span data-stu-id="966de-486">*Getting Model properties and methods with Visual Studio's IntelliSense*</span></span>
    > 
    > <span data-ttu-id="966de-487">O **modelo** referências de propriedade de **StoreIndexViewModel** objeto que o controlador é passado para o modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="966de-487">The **Model** property references the **StoreIndexViewModel** object that the Controller passed to the View template.</span></span> <span data-ttu-id="966de-488">Isso significa que você pode acessar todos os dados passados do controlador para o modelo de exibição por meio de **modelo** propriedade e formate-o em uma resposta HTML apropriada dentro do modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="966de-488">This means that you can access all of the data passed from the Controller to the View template via the **Model** property, and format it into an appropriate HTML response within the View template.</span></span>
    > 
    > <span data-ttu-id="966de-489">Você pode selecionar apenas o **NumberOfGenres** lista de propriedade do Intellisense em vez de digitar-o e, em seguida, ele será o preenchimento automático-la pressionando o **tecla tab**.</span><span class="sxs-lookup"><span data-stu-id="966de-489">You can just select the **NumberOfGenres** property from the Intellisense list rather than typing it in and then it will auto-complete it by pressing the **tab key**.</span></span>
2. <span data-ttu-id="966de-490">Loop em relação à lista de gênero em **StoreIndexViewModel** e criar um HTML  **&lt;ul&gt;**  lista usando um **foreach** loop.</span><span class="sxs-lookup"><span data-stu-id="966de-490">Loop over the genre list in **StoreIndexViewModel** and create an HTML **&lt;ul&gt;** list using a **foreach** loop.</span></span>
<span data-ttu-id="966de-491">(C#)</span><span class="sxs-lookup"><span data-stu-id="966de-491">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. <span data-ttu-id="966de-492">Pressione **F5** para executar o aplicativo e procurar **/armazenamento**.</span><span class="sxs-lookup"><span data-stu-id="966de-492">Press **F5** to run the Application and browse **/Store**.</span></span> <span data-ttu-id="966de-493">Você verá a lista de gêneros passado a **StoreIndexViewModel** de objeto o **StoreController** para o modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="966de-493">You will see the list of genres passed in the **StoreIndexViewModel** object from the **StoreController** to the View template.</span></span>

    <span data-ttu-id="966de-494">![Modo de exibição exibe uma lista de gêneros](aspnet-mvc-4-fundamentals/_static/image26.png "exibição exibindo uma lista de gêneros")</span><span class="sxs-lookup"><span data-stu-id="966de-494">![View displaying a list of genres](aspnet-mvc-4-fundamentals/_static/image26.png "View displaying a list of genres")</span></span>

    <span data-ttu-id="966de-495">*Exibindo uma lista de gêneros de exibição*</span><span class="sxs-lookup"><span data-stu-id="966de-495">*View displaying a list of genres*</span></span>
4. <span data-ttu-id="966de-496">Feche o navegador.</span><span class="sxs-lookup"><span data-stu-id="966de-496">Close the browser.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a><span data-ttu-id="966de-497">Exercício 6: Usando parâmetros no modo de exibição</span><span class="sxs-lookup"><span data-stu-id="966de-497">Exercise 6: Using Parameters in View</span></span>

<span data-ttu-id="966de-498">No Exercício 3, você aprendeu como passar parâmetros para o controlador.</span><span class="sxs-lookup"><span data-stu-id="966de-498">In Exercise 3 you learned how to pass parameters to the Controller.</span></span> <span data-ttu-id="966de-499">Neste exercício, você aprenderá como usar esses parâmetros no modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="966de-499">In this exercise, you will learn how to use those parameters in the View template.</span></span> <span data-ttu-id="966de-500">Para essa finalidade, você vai ser introduzido pela primeira vez para classes de modelo que ajudarão você a gerenciar sua lógica de dados e de domínio.</span><span class="sxs-lookup"><span data-stu-id="966de-500">For that purpose, you will be introduced first to Model classes that will help you to manage your data and domain logic.</span></span> <span data-ttu-id="966de-501">Além disso, você aprenderá como criar links para páginas dentro do aplicativo ASP.NET MVC sem se preocupar de coisas como a codificação de caminhos de URL.</span><span class="sxs-lookup"><span data-stu-id="966de-501">Additionally, you will learn how to create links to pages inside the ASP.NET MVC application without worrying of things like URL paths encoding.</span></span>

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a><span data-ttu-id="966de-502">Tarefa 1: Adicionar Classes de modelo</span><span class="sxs-lookup"><span data-stu-id="966de-502">Task 1 - Adding Model Classes</span></span>

<span data-ttu-id="966de-503">Diferentemente de ViewModels, que são criados apenas para passar informações do controlador para o modo de exibição, classes de modelo são criados para conter e gerenciar a lógica de dados e o domínio.</span><span class="sxs-lookup"><span data-stu-id="966de-503">Unlike ViewModels, which are created just to pass information from the Controller to the View, Model classes are built to contain and manage data and domain logic.</span></span> <span data-ttu-id="966de-504">Nesta tarefa, você adicionará duas classes de modelo para representar esses conceitos: **gênero** e **álbum**.</span><span class="sxs-lookup"><span data-stu-id="966de-504">In this task you will add two model classes to represent these concepts: **Genre** and **Album**.</span></span>

1. <span data-ttu-id="966de-505">Se não estiver aberto, inicie **VS Express para Web**</span><span class="sxs-lookup"><span data-stu-id="966de-505">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="966de-506">No **arquivo** menu, escolha **Abrir projeto**.</span><span class="sxs-lookup"><span data-stu-id="966de-506">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="966de-507">Na caixa de diálogo Abrir projeto, vá para **Source\Ex06 UsingParametersInView\Begin**, selecione **Begin.sln** e clique em **abrir**.</span><span class="sxs-lookup"><span data-stu-id="966de-507">In the Open Project dialog, browse to **Source\Ex06-UsingParametersInView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="966de-508">Como alternativa, você pode continuar com a solução que você obteve depois de concluir o exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="966de-508">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

    1. <span data-ttu-id="966de-509">Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="966de-509">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="966de-510">Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="966de-510">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="966de-511">No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="966de-511">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="966de-512">Por fim, compile a solução clicando **criar** | **compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="966de-512">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="966de-513">Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="966de-513">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="966de-514">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="966de-514">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="966de-515">É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.</span><span class="sxs-lookup"><span data-stu-id="966de-515">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="966de-516">Adicionar um **gênero** classe de modelo.</span><span class="sxs-lookup"><span data-stu-id="966de-516">Add a **Genre** Model class.</span></span> <span data-ttu-id="966de-517">Para fazer isso, clique o **modelos** pasta o **Gerenciador de soluções**, selecione **adicionar** e, em seguida, o **Novo Item** opção.</span><span class="sxs-lookup"><span data-stu-id="966de-517">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="966de-518">Em **código**, escolha o **classe** item e nomeie o arquivo *Genre.cs*, em seguida, clique em **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="966de-518">Under **Code**, choose the **Class** item and name the file *Genre.cs*, then click **Add**.</span></span>

    <span data-ttu-id="966de-519">![Adicionando uma classe](aspnet-mvc-4-fundamentals/_static/image27.png "adicionando uma classe")</span><span class="sxs-lookup"><span data-stu-id="966de-519">![Adding a class](aspnet-mvc-4-fundamentals/_static/image27.png "Adding a class")</span></span>

    <span data-ttu-id="966de-520">*Adicionar um novo item*</span><span class="sxs-lookup"><span data-stu-id="966de-520">*Adding a new item*</span></span>

    <span data-ttu-id="966de-521">![Adicionar classe de modelo de gênero](aspnet-mvc-4-fundamentals/_static/image28.png "Adicionar classe de modelo de gênero")</span><span class="sxs-lookup"><span data-stu-id="966de-521">![Add Genre Model Class](aspnet-mvc-4-fundamentals/_static/image28.png "Add Genre Model Class")</span></span>

    <span data-ttu-id="966de-522">*Adicionar classe de modelo de gênero*</span><span class="sxs-lookup"><span data-stu-id="966de-522">*Add Genre Model Class*</span></span>
4. <span data-ttu-id="966de-523">Adicionar um **nome** propriedade para a classe de gênero.</span><span class="sxs-lookup"><span data-stu-id="966de-523">Add a **Name** property to the Genre class.</span></span> <span data-ttu-id="966de-524">Para fazer isso, adicione o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="966de-524">To do this, add the following code:</span></span>

    <span data-ttu-id="966de-525">(Código de trecho - *conceitos básicos do ASP.NET MVC 4 - gênero Ex6*)</span><span class="sxs-lookup"><span data-stu-id="966de-525">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Genre*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
5. <span data-ttu-id="966de-526">Siga o mesmo procedimento como antes, adicione um **álbum** classe.</span><span class="sxs-lookup"><span data-stu-id="966de-526">Following the same procedure as before, add an **Album** class.</span></span> <span data-ttu-id="966de-527">Para fazer isso, clique o **modelos** pasta o **Gerenciador de soluções**, selecione **adicionar** e, em seguida, o **Novo Item** opção.</span><span class="sxs-lookup"><span data-stu-id="966de-527">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="966de-528">Em **código**, escolha o **classe** item e nomeie o arquivo *Album.cs*, em seguida, clique em **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="966de-528">Under **Code**, choose the **Class** item and name the file *Album.cs*, then click **Add**.</span></span>
6. <span data-ttu-id="966de-529">Adicione duas propriedades à classe álbum: **gênero** e **título**.</span><span class="sxs-lookup"><span data-stu-id="966de-529">Add two properties to the Album class: **Genre** and **Title**.</span></span> <span data-ttu-id="966de-530">Para fazer isso, adicione o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="966de-530">To do this, add the following code:</span></span>

    <span data-ttu-id="966de-531">(Código de trecho - *conceitos básicos do ASP.NET MVC 4 - álbum Ex6*)</span><span class="sxs-lookup"><span data-stu-id="966de-531">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Album*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a><span data-ttu-id="966de-532">Tarefa 2: adicionando um StoreBrowseViewModel</span><span class="sxs-lookup"><span data-stu-id="966de-532">Task 2 - Adding a StoreBrowseViewModel</span></span>

<span data-ttu-id="966de-533">Um **StoreBrowseViewModel** será usado nessa tarefa para mostrar os álbuns que correspondem a um gênero selecionado.</span><span class="sxs-lookup"><span data-stu-id="966de-533">A **StoreBrowseViewModel** will be used in this task to show the Albums that match a selected Genre.</span></span> <span data-ttu-id="966de-534">Nesta tarefa, você criará essa classe e, em seguida, adiciona duas propriedades para lidar com o **gênero** e sua **álbum**da lista.</span><span class="sxs-lookup"><span data-stu-id="966de-534">In this task, you will create this class and then add two properties to handle the **Genre** and its **Album**'s List.</span></span>

1. <span data-ttu-id="966de-535">Adicionar um **StoreBrowseViewModel** classe.</span><span class="sxs-lookup"><span data-stu-id="966de-535">Add a **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="966de-536">Para fazer isso, clique o **ViewModels** pasta o **Solution Explorer**, selecione **adicionar** e, em seguida, o **Novo Item** opção.</span><span class="sxs-lookup"><span data-stu-id="966de-536">To do this, right-click the **ViewModels** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="966de-537">Em **código**, escolha o **classe** item e nomeie o arquivo *StoreBrowseViewModel.cs*, em seguida, clique em **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="966de-537">Under **Code**, choose the **Class** item and name the file *StoreBrowseViewModel.cs*, then click **Add**.</span></span>
2. <span data-ttu-id="966de-538">Adicione uma referência para os modelos no **StoreBrowseViewModel** classe.</span><span class="sxs-lookup"><span data-stu-id="966de-538">Add a reference to the Models in **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="966de-539">Para fazer isso, adicione o seguinte usando o namespace:</span><span class="sxs-lookup"><span data-stu-id="966de-539">To do this, add the following using namespace:</span></span>

    <span data-ttu-id="966de-540">(Código de trecho - *conceitos básicos do ASP.NET MVC 4 - Ex6 UsingModel*)</span><span class="sxs-lookup"><span data-stu-id="966de-540">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModel*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
3. <span data-ttu-id="966de-541">Adicionar duas propriedades para **StoreBrowseViewModel** classe: **gênero** e **álbuns**.</span><span class="sxs-lookup"><span data-stu-id="966de-541">Add two properties to **StoreBrowseViewModel** class: **Genre** and **Albums**.</span></span> <span data-ttu-id="966de-542">Para fazer isso, adicione o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="966de-542">To do this, add the following code:</span></span>

    <span data-ttu-id="966de-543">(Código de trecho - *conceitos básicos do ASP.NET MVC 4 - Ex6 ModelProperties*)</span><span class="sxs-lookup"><span data-stu-id="966de-543">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 ModelProperties*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

    > [!NOTE]
    > <span data-ttu-id="966de-544">O que é **lista&lt;álbum&gt;**  ?: esta definição está usando o **lista&lt;T&gt;**  tipo, onde **T** restringe o quais elementos deste tipo **lista** pertencem e, nesse caso **álbum** (ou qualquer um de seus descendentes).</span><span class="sxs-lookup"><span data-stu-id="966de-544">What is **List&lt;Album&gt;** ?: This definition is using the **List&lt;T&gt;** type, where **T** constrains the type to which elements of this **List** belong to, in this case **Album** (or any of its descendants).</span></span>
    > 
    > <span data-ttu-id="966de-545">Essa capacidade de criar classes e métodos que adiar a especificação de um ou mais tipos até que a classe ou método é declarado e instanciado pelo código do cliente é um recurso da linguagem c# chamado **genéricos**.</span><span class="sxs-lookup"><span data-stu-id="966de-545">This ability to design classes and methods that defer the specification of one or more types until the class or method is declared and instantiated by client code is a feature of the C# language called **Generics**.</span></span>
    > 
    > <span data-ttu-id="966de-546">**Lista&lt;T&gt;**  é o equivalente genérico de **ArrayList** digite e está disponível na **Generic** namespace.</span><span class="sxs-lookup"><span data-stu-id="966de-546">**List&lt;T&gt;** is the generic equivalent of the **ArrayList** type and is available in the **System.Collections.Generic** namespace.</span></span> <span data-ttu-id="966de-547">Um dos benefícios do uso de **genéricos** é que, como o tipo é especificado, você não precisa cuidar do tipo de verificação de operações como converter os elementos em **álbum** como você faria com um **ArrayList**.</span><span class="sxs-lookup"><span data-stu-id="966de-547">One of the benefits of using **generics** is that since the type is specified, you do not need to take care of type checking operations such as casting the elements into **Album** as you would do with an **ArrayList**.</span></span>

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a><span data-ttu-id="966de-548">Tarefa 3: usando o novo ViewModel no StoreController</span><span class="sxs-lookup"><span data-stu-id="966de-548">Task 3 - Using the New ViewModel in the StoreController</span></span>

<span data-ttu-id="966de-549">Nesta tarefa, você modificará o **StoreController**do **procurar** e **detalhes** métodos de ação para usar o novo **StoreBrowseViewModel** .</span><span class="sxs-lookup"><span data-stu-id="966de-549">In this task, you will modify the **StoreController**'s **Browse** and **Details** action methods to use the new **StoreBrowseViewModel**.</span></span>

1. <span data-ttu-id="966de-550">Adicione uma referência para a pasta de modelos em **StoreController** classe.</span><span class="sxs-lookup"><span data-stu-id="966de-550">Add a reference to the Models folder in **StoreController** class.</span></span> <span data-ttu-id="966de-551">Para fazer isso, expanda o **controladores** pasta o **Solution Explorer** e abra o **StoreController** classe.</span><span class="sxs-lookup"><span data-stu-id="966de-551">To do this, expand the **Controllers** folder in the **Solution Explorer** and open the **StoreController** class.</span></span> <span data-ttu-id="966de-552">Em seguida, adicione o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="966de-552">Then add the following code:</span></span>

    <span data-ttu-id="966de-553">(Código de trecho - *conceitos básicos do ASP.NET MVC 4 - Ex6 UsingModelInController*)</span><span class="sxs-lookup"><span data-stu-id="966de-553">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModelInController*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
2. <span data-ttu-id="966de-554">Substitua o **procurar** método de ação para usar o **StoreViewBrowseController** classe.</span><span class="sxs-lookup"><span data-stu-id="966de-554">Replace the **Browse** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="966de-555">Você criará um gênero e dois novos objetos álbuns com dados fictícios (no próximo laboratório prático que consumirá dados reais de um banco de dados).</span><span class="sxs-lookup"><span data-stu-id="966de-555">You will create a Genre and two new Albums objects with dummy data (in the next Hands-on Lab you will consume real data from a database).</span></span> <span data-ttu-id="966de-556">Para fazer isso, substitua o **procurar** método com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="966de-556">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="966de-557">(Código de trecho - *conceitos básicos do ASP.NET MVC 4 - Ex6 BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="966de-557">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 BrowseMethod*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
3. <span data-ttu-id="966de-558">Substitua o **detalhes** método de ação para usar o **StoreViewBrowseController** classe.</span><span class="sxs-lookup"><span data-stu-id="966de-558">Replace the **Details** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="966de-559">Você criará um novo **álbum** objeto a ser retornado para o **exibição**.</span><span class="sxs-lookup"><span data-stu-id="966de-559">You will create a new **Album** object to be returned to the **View**.</span></span> <span data-ttu-id="966de-560">Para fazer isso, substitua o **detalhes** método com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="966de-560">To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="966de-561">(Código de trecho - *conceitos básicos do ASP.NET MVC 4 - Ex6 DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="966de-561">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 DetailsMethod*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a><span data-ttu-id="966de-562">Tarefa 4: adicionando um modelo de exibição de navegação</span><span class="sxs-lookup"><span data-stu-id="966de-562">Task 4 - Adding a Browse View Template</span></span>

<span data-ttu-id="966de-563">Nesta tarefa, você adicionará um **procurar** exibição para mostrar os álbuns encontrados para um gênero específico.</span><span class="sxs-lookup"><span data-stu-id="966de-563">In this task, you will add a **Browse** View to show the Albums found for a specific Genre.</span></span>

1. <span data-ttu-id="966de-564">Antes de criar o novo modelo de exibição, você deve compilar o projeto para que o **adicionar exibição** diálogo conhece o **ViewModel** classe a ser usada.</span><span class="sxs-lookup"><span data-stu-id="966de-564">Before creating the new View template, you should build the project so that the **Add View** Dialog knows about the **ViewModel** class to use.</span></span> <span data-ttu-id="966de-565">Compile o projeto selecionando o **criar** item de menu e, em seguida, **MvcMusicStore criar**.</span><span class="sxs-lookup"><span data-stu-id="966de-565">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>
2. <span data-ttu-id="966de-566">Adicionar um **procurar** exibição.</span><span class="sxs-lookup"><span data-stu-id="966de-566">Add a **Browse** View.</span></span> <span data-ttu-id="966de-567">Para fazer isso, clique no **procurar** método de ação de **StoreController** e clique em **adicionar modo de exibição**.</span><span class="sxs-lookup"><span data-stu-id="966de-567">To do this, right-click in the **Browse** action method of the **StoreController** and click **Add View**.</span></span>
3. <span data-ttu-id="966de-568">No **adicionar exibição** caixa de diálogo caixa, verifique se o nome de exibição **procurar**.</span><span class="sxs-lookup"><span data-stu-id="966de-568">In the **Add View** dialog box, verify that the View Name is **Browse**.</span></span> <span data-ttu-id="966de-569">Verifique o **criar uma exibição fortemente tipada** caixa de seleção e selecione **StoreBrowseViewModel** do **classe modelo** lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="966de-569">Check the **Create a strongly-typed view** checkbox and select **StoreBrowseViewModel** from the **Model class** dropdown.</span></span> <span data-ttu-id="966de-570">Deixe os outros campos com seus valores padrão.</span><span class="sxs-lookup"><span data-stu-id="966de-570">Leave the other fields with their default value.</span></span> <span data-ttu-id="966de-571">Em seguida, clique em **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="966de-571">Then click **Add**.</span></span>

    <span data-ttu-id="966de-572">![Adicionando uma exibição de navegação](aspnet-mvc-4-fundamentals/_static/image29.png "adicionando uma exibição de navegação")</span><span class="sxs-lookup"><span data-stu-id="966de-572">![Adding a Browse View](aspnet-mvc-4-fundamentals/_static/image29.png "Adding a Browse View")</span></span>

    <span data-ttu-id="966de-573">*Adicionar um modo de procura*</span><span class="sxs-lookup"><span data-stu-id="966de-573">*Adding a Browse View*</span></span>
4. <span data-ttu-id="966de-574">Modificar o **Browse.cshtml** para exibir informações do gênero, acessando o **StoreBrowseViewModel** objeto que é passado para o modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="966de-574">Modify the **Browse.cshtml** to display the Genre's information, accessing the **StoreBrowseViewModel** object that is passed to the view template.</span></span> <span data-ttu-id="966de-575">Para fazer isso, substitua o conteúdo a seguir: (c#)</span><span class="sxs-lookup"><span data-stu-id="966de-575">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="966de-576">Tarefa 5: executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="966de-576">Task 5 - Running the Application</span></span>

<span data-ttu-id="966de-577">Nesta tarefa, você testará que o **procurar** método recupera álbuns do **procurar** ação do método.</span><span class="sxs-lookup"><span data-stu-id="966de-577">In this task, you will test that the **Browse** method retrieves Albums from the **Browse** method action.</span></span>

1. <span data-ttu-id="966de-578">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="966de-578">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="966de-579">O projeto é iniciado na Home page.</span><span class="sxs-lookup"><span data-stu-id="966de-579">The project starts in the Home page.</span></span> <span data-ttu-id="966de-580">Altere a URL para   **/armazenar/procurar? Gênero = Disco** e verificar se a ação retorna dois álbuns.</span><span class="sxs-lookup"><span data-stu-id="966de-580">Change the URL to **/Store/Browse?Genre=Disco** to verify that the action returns two Albums.</span></span>

    <span data-ttu-id="966de-581">![Navegação álbuns de Disco de armazenamento](aspnet-mvc-4-fundamentals/_static/image30.png "navegação álbuns de Disco de armazenamento")</span><span class="sxs-lookup"><span data-stu-id="966de-581">![Browsing Store Disco Albums](aspnet-mvc-4-fundamentals/_static/image30.png "Browsing Store Disco Albums")</span></span>

    <span data-ttu-id="966de-582">*Navegação álbuns de Disco de armazenamento*</span><span class="sxs-lookup"><span data-stu-id="966de-582">*Browsing Store Disco Albums*</span></span>

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a><span data-ttu-id="966de-583">Tarefa 6 - exibindo informações sobre um álbum específico</span><span class="sxs-lookup"><span data-stu-id="966de-583">Task 6 - Displaying information About a Specific Album</span></span>

<span data-ttu-id="966de-584">Nesta tarefa, você implementará o **detalhes do repositório** para exibir informações sobre um álbum específico.</span><span class="sxs-lookup"><span data-stu-id="966de-584">In this task, you will implement the **Store/Details** view to display information about a specific album.</span></span> <span data-ttu-id="966de-585">Neste laboratório prático, tudo o que você irá exibir sobre o álbum já está contido no **exibição** modelo.</span><span class="sxs-lookup"><span data-stu-id="966de-585">In this Hands-on Lab, everything you will display about the album is already contained in the **View** template.</span></span> <span data-ttu-id="966de-586">Assim, em vez de criar um **StoreDetailsViewModel** classe, você usará o atual **StoreBrowseViewModel** modelo passando o álbum a ele.</span><span class="sxs-lookup"><span data-stu-id="966de-586">So, instead of creating a **StoreDetailsViewModel** class, you will use the current **StoreBrowseViewModel** template passing the Album to it.</span></span>

1. <span data-ttu-id="966de-587">Feche o navegador se necessário, para retornar à janela do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="966de-587">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="966de-588">Adicionar um novo **detalhes** exibir para o **StoreController**do **detalhes** método de ação.</span><span class="sxs-lookup"><span data-stu-id="966de-588">Add a new **Details** view for the **StoreController**'s **Details** action method.</span></span> <span data-ttu-id="966de-589">Para fazer isso, clique com botão direito do **detalhes** método o **StoreController** classe e clique em **adicionar modo de exibição**.</span><span class="sxs-lookup"><span data-stu-id="966de-589">To do this, right-click the **Details** method in the **StoreController** class and click **Add View**.</span></span>
2. <span data-ttu-id="966de-590">No **adicionar exibição** caixa de diálogo, verifique o **nome de exibição** é **detalhes**.</span><span class="sxs-lookup"><span data-stu-id="966de-590">In the **Add View** dialog, verify that the **View Name** is **Details**.</span></span> <span data-ttu-id="966de-591">Verifique o **criar uma exibição fortemente tipada** caixa de seleção e selecione **álbum** do **classe modelo** lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="966de-591">Check the **Create a strongly-typed view** checkbox and select **Album** from the **Model class** drop-down.</span></span> <span data-ttu-id="966de-592">Deixe os outros campos com seus valores padrão.</span><span class="sxs-lookup"><span data-stu-id="966de-592">Leave the other fields with their default value.</span></span> <span data-ttu-id="966de-593">Em seguida, clique em **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="966de-593">Then click **Add**.</span></span> <span data-ttu-id="966de-594">Isso irá criar e abrir um **\Views\Store\Details.cshtml** arquivo.</span><span class="sxs-lookup"><span data-stu-id="966de-594">This will create and open a **\Views\Store\Details.cshtml** file.</span></span>

    <span data-ttu-id="966de-595">![Adicionando uma exibição de detalhes](aspnet-mvc-4-fundamentals/_static/image31.png "adicionando uma exibição de detalhes")</span><span class="sxs-lookup"><span data-stu-id="966de-595">![Adding a Details View](aspnet-mvc-4-fundamentals/_static/image31.png "Adding a Details View")</span></span>

    <span data-ttu-id="966de-596">*Adicionando uma exibição de detalhes*</span><span class="sxs-lookup"><span data-stu-id="966de-596">*Adding a Details View*</span></span>
3. <span data-ttu-id="966de-597">Modificar o **Details.cshtml** arquivo para exibir informações do álbum, acessando o **álbum** objeto que é passado para o modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="966de-597">Modify the **Details.cshtml** file to display the Album's information, accessing the **Album** object that is passed to the view template.</span></span> <span data-ttu-id="966de-598">Para fazer isso, substitua o conteúdo a seguir: (c#)</span><span class="sxs-lookup"><span data-stu-id="966de-598">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="966de-599">Tarefa 7: executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="966de-599">Task 7 - Running the Application</span></span>

<span data-ttu-id="966de-600">Nesta tarefa, você testará que o **detalhes** exibição recupera informações do álbum do **detalhes ação** método.</span><span class="sxs-lookup"><span data-stu-id="966de-600">In this task, you will test that the **Details** View retrieves Album's information from the **Details action** method.</span></span>

1. <span data-ttu-id="966de-601">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="966de-601">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="966de-602">O projeto é iniciado **início** página.</span><span class="sxs-lookup"><span data-stu-id="966de-602">The project starts in the **Home** page.</span></span> <span data-ttu-id="966de-603">Altere a URL para **/Store/Details/5** para verificar as informações do álbum.</span><span class="sxs-lookup"><span data-stu-id="966de-603">Change the URL to **/Store/Details/5** to verify the album's information.</span></span>

    <span data-ttu-id="966de-604">![Procurando detalhes álbuns](aspnet-mvc-4-fundamentals/_static/image32.png "navegação álbuns detalhes")</span><span class="sxs-lookup"><span data-stu-id="966de-604">![Browsing Albums Detail](aspnet-mvc-4-fundamentals/_static/image32.png "Browsing Albums Detail")</span></span>

    <span data-ttu-id="966de-605">*Detalhes do álbum de navegação*</span><span class="sxs-lookup"><span data-stu-id="966de-605">*Browsing Album's Detail*</span></span>

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a><span data-ttu-id="966de-606">Tarefa 8 - adicionando Links entre páginas</span><span class="sxs-lookup"><span data-stu-id="966de-606">Task 8 - Adding Links Between Pages</span></span>

<span data-ttu-id="966de-607">Nesta tarefa, você adicionará um link na exibição de repositório para ter um link em cada nome de gênero ao apropriado **repositório/procurar** URL.</span><span class="sxs-lookup"><span data-stu-id="966de-607">In this task, you will add a link in the Store View to have a link in every Genre name to the appropriate **/Store/Browse** URL.</span></span> <span data-ttu-id="966de-608">Dessa forma, quando você clica em um gênero, por exemplo **Disco**, ele navegará para **repositório/procurar? gênero = Disco** URL.</span><span class="sxs-lookup"><span data-stu-id="966de-608">This way, when you click on a Genre, for instance **Disco**, it will navigate to **/Store/Browse?genre=Disco** URL.</span></span>

1. <span data-ttu-id="966de-609">Feche o navegador se necessário, para retornar à janela do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="966de-609">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="966de-610">Atualização de **índice** página para adicionar um link para o **procurar** página.</span><span class="sxs-lookup"><span data-stu-id="966de-610">Update the **Index** page to add a link to the **Browse** page.</span></span> <span data-ttu-id="966de-611">Para fazer isso, no **Gerenciador de soluções** expandir o **exibições** pasta, o **repositório** pasta e clique duas vezes o **cshtml** página.</span><span class="sxs-lookup"><span data-stu-id="966de-611">To do this, in the **Solution Explorer** expand the **Views** folder, then the **Store** folder and double-click the **Index.cshtml** page.</span></span>
2. <span data-ttu-id="966de-612">Adicione um link para o modo de procura que indica o gênero selecionado.</span><span class="sxs-lookup"><span data-stu-id="966de-612">Add a link to the Browse view indicating the genre selected.</span></span> <span data-ttu-id="966de-613">Para fazer isso, substitua o seguinte código dentro de  **&lt;li&gt;**  marcas: (c#)</span><span class="sxs-lookup"><span data-stu-id="966de-613">To do this, replace the following highlighted code within the **&lt;li&gt;** tags: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="966de-614">outra abordagem seria possível vincular diretamente para a página, com um código semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="966de-614">another approach would be linking directly to the page, with a code like the following:</span></span>
    > 
    > <span data-ttu-id="966de-615">&lt;a href=&quot;/Store/Browse?genre=@genreName&quot;&gt;@genreName&lt;/a&gt;</span><span class="sxs-lookup"><span data-stu-id="966de-615">&lt;a href=&quot;/Store/Browse?genre=@genreName&quot;&gt;@genreName&lt;/a&gt;</span></span>
    > 
    > <span data-ttu-id="966de-616">Embora essa abordagem funciona, depende de uma cadeia de caracteres codificada.</span><span class="sxs-lookup"><span data-stu-id="966de-616">Although this approach works, it depends on a hardcoded string.</span></span> <span data-ttu-id="966de-617">Se você renomear o controlador mais tarde, você precisará alterar essa instrução manualmente.</span><span class="sxs-lookup"><span data-stu-id="966de-617">If you later rename the Controller, you will have to change this instruction manually.</span></span> <span data-ttu-id="966de-618">Uma alternativa melhor é usar um **auxiliar HTML** método.</span><span class="sxs-lookup"><span data-stu-id="966de-618">A better alternative is to use an **HTML Helper** method.</span></span> <span data-ttu-id="966de-619">O ASP.NET MVC inclui um método auxiliar HTML que está disponível para essas tarefas.</span><span class="sxs-lookup"><span data-stu-id="966de-619">ASP.NET MVC includes an HTML Helper method which is available for such tasks.</span></span> <span data-ttu-id="966de-620">O **Html.ActionLink()** método auxiliar torna mais fácil criar HTML  **&lt;um&gt;**  links, certificando-se os caminhos de URL corretamente são codificados de URL.</span><span class="sxs-lookup"><span data-stu-id="966de-620">The **Html.ActionLink()** helper method makes it easy to build HTML **&lt;a&gt;** links, making sure URL paths are properly URL encoded.</span></span>
    > 
    > <span data-ttu-id="966de-621">Htlm.ActionLink tem várias sobrecargas.</span><span class="sxs-lookup"><span data-stu-id="966de-621">Htlm.ActionLink has several overloads.</span></span> <span data-ttu-id="966de-622">Neste exercício você usará um que usa três parâmetros:</span><span class="sxs-lookup"><span data-stu-id="966de-622">In this exercise you will use one that takes three parameters:</span></span>
    > 
    > 1. <span data-ttu-id="966de-623">Texto do link, que exibe o nome de gênero</span><span class="sxs-lookup"><span data-stu-id="966de-623">Link text, which will display the Genre name</span></span>
    > 2. <span data-ttu-id="966de-624">Nome de ação do controlador (**procurar**)</span><span class="sxs-lookup"><span data-stu-id="966de-624">Controller action name (**Browse**)</span></span>
    > 3. <span data-ttu-id="966de-625">Valores de parâmetro, especificando o nome de rota (**gênero**) e o valor (**gênero**)</span><span class="sxs-lookup"><span data-stu-id="966de-625">Route parameter values, specifying both the name (**Genre**) and the value (**Genre name**)</span></span>

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a><span data-ttu-id="966de-626">Tarefa 9 - executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="966de-626">Task 9 - Running the Application</span></span>

<span data-ttu-id="966de-627">Nesta tarefa, você testará que cada gênero é exibido com um link para as **repositório/procurar** URL.</span><span class="sxs-lookup"><span data-stu-id="966de-627">In this task, you will test that each Genre is displayed with a link to the appropriate **/Store/Browse** URL.</span></span>

1. <span data-ttu-id="966de-628">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="966de-628">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="966de-629">O projeto é iniciado na Home page.</span><span class="sxs-lookup"><span data-stu-id="966de-629">The project starts in the Home page.</span></span> <span data-ttu-id="966de-630">Altere a URL para **/armazenamento** para verificar se cada gênero contém links para as **repositório/procurar** URL.</span><span class="sxs-lookup"><span data-stu-id="966de-630">Change the URL to **/Store** to verify that each Genre links to the appropriate **/Store/Browse** URL.</span></span>

    <span data-ttu-id="966de-631">![Navegação gêneros com links para a página Procurar](aspnet-mvc-4-fundamentals/_static/image33.png "gêneros de navegação com links para a página de pesquisa")</span><span class="sxs-lookup"><span data-stu-id="966de-631">![Browsing Genres with links to Browse page](aspnet-mvc-4-fundamentals/_static/image33.png "Browsing Genres with links to Browse page")</span></span>

    <span data-ttu-id="966de-632">*Navegação gêneros com links para a página de pesquisa*</span><span class="sxs-lookup"><span data-stu-id="966de-632">*Browsing Genres with links to Browse page*</span></span>

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a><span data-ttu-id="966de-633">Tarefa 10 - usando a coleção de ViewModel dinâmico para passar valores</span><span class="sxs-lookup"><span data-stu-id="966de-633">Task 10 - Using Dynamic ViewModel Collection to Pass Values</span></span>

<span data-ttu-id="966de-634">Nesta tarefa, você aprenderá a um método simple e eficiente para transmitir valores entre o controlador e o modo de exibição sem fazer alterações no modelo.</span><span class="sxs-lookup"><span data-stu-id="966de-634">In this task, you will learn a simple and powerful method to pass values between the Controller and the View without making any changes in the Model.</span></span> <span data-ttu-id="966de-635">ASP.NET MVC 4 fornece a coleção &quot;ViewModel&quot;, que pode ser atribuído a qualquer valor dinâmico e acessado dentro de controladores e modos de exibição.</span><span class="sxs-lookup"><span data-stu-id="966de-635">ASP.NET MVC 4 provides the collection &quot;ViewModel&quot;, which can be assigned to any dynamic value and accessed inside controllers and views as well.</span></span>

<span data-ttu-id="966de-636">Agora você usará o conjunto dinâmico ViewBag para passar uma lista de &quot; **contêm gêneros** &quot; do controlador para o modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="966de-636">You will now use the ViewBag dynamic collection to pass a list of &quot;**Starred genres**&quot; from the controller to the view.</span></span> <span data-ttu-id="966de-637">O modo de exibição do índice de repositório acessará a **ViewModel** e exibir as informações.</span><span class="sxs-lookup"><span data-stu-id="966de-637">The Store Index view will access to **ViewModel** and display the information.</span></span>

1. <span data-ttu-id="966de-638">Feche o navegador se necessário, para retornar à janela do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="966de-638">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="966de-639">Abra **StoreController.cs** e modificar **índice** método para criar uma lista de contêm gêneros ViewModel coleção:</span><span class="sxs-lookup"><span data-stu-id="966de-639">Open **StoreController.cs** and modify **Index** method to create a list of starred genres into ViewModel collection :</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

    > [!NOTE]
    > <span data-ttu-id="966de-640">Você também pode usar a sintaxe **ViewBag [&quot;Starred&quot;]** para acessar as propriedades.</span><span class="sxs-lookup"><span data-stu-id="966de-640">You could also use the syntax **ViewBag[&quot;Starred&quot;]** to access the properties.</span></span>
2. <span data-ttu-id="966de-641">O ícone de estrela  **&quot;starred.png&quot;**  está incluído no **Source\Assets\Images** pasta deste laboratório.</span><span class="sxs-lookup"><span data-stu-id="966de-641">The star icon **&quot;starred.png&quot;** is included in the **Source\Assets\Images** folder of this lab.</span></span> <span data-ttu-id="966de-642">Para adicioná-lo para o aplicativo, arraste o conteúdo de um **Windows Explorer** janela para o **Solution Explorer** no Visual Web Developer Express, conforme mostrado abaixo:</span><span class="sxs-lookup"><span data-stu-id="966de-642">In order to add it to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Web Developer Express, as shown below:</span></span>

    <span data-ttu-id="966de-643">![Imagem de estrela adicionando à solução](aspnet-mvc-4-fundamentals/_static/image34.png "adicionando imagem de estrela para a solução")</span><span class="sxs-lookup"><span data-stu-id="966de-643">![Adding star image to the solution](aspnet-mvc-4-fundamentals/_static/image34.png "Adding star image to the solution")</span></span>

    <span data-ttu-id="966de-644">*Adicionar imagem de estrela para a solução*</span><span class="sxs-lookup"><span data-stu-id="966de-644">*Adding star image to the solution*</span></span>
3. <span data-ttu-id="966de-645">Abra a exibição **Store/Index.cshtml** e modificar o conteúdo.</span><span class="sxs-lookup"><span data-stu-id="966de-645">Open the view **Store/Index.cshtml** and modify the content.</span></span> <span data-ttu-id="966de-646">Você lerá o &quot;contêm&quot; propriedade o **ViewBag** coleção e pergunte se o nome de gênero atual está na lista.</span><span class="sxs-lookup"><span data-stu-id="966de-646">You will read the &quot;starred&quot; property in the **ViewBag** collection, and ask if the current genre name is in the list.</span></span> <span data-ttu-id="966de-647">Nesse caso, você mostrará um ícone de estrela direita para o link de gênero.</span><span class="sxs-lookup"><span data-stu-id="966de-647">In that case you will show a star icon right to the genre link.</span></span>
<span data-ttu-id="966de-648">(C#)</span><span class="sxs-lookup"><span data-stu-id="966de-648">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a><span data-ttu-id="966de-649">Tarefa 11 - executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="966de-649">Task 11 - Running the Application</span></span>

<span data-ttu-id="966de-650">Nesta tarefa, você testará os gêneros com estrelas exibem um ícone de estrela.</span><span class="sxs-lookup"><span data-stu-id="966de-650">In this task, you will test that the starred genres display a star icon.</span></span>

1. <span data-ttu-id="966de-651">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="966de-651">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="966de-652">O projeto é iniciado **início** página.</span><span class="sxs-lookup"><span data-stu-id="966de-652">The project starts in the **Home** page.</span></span> <span data-ttu-id="966de-653">Altere a URL para **/armazenamento** para verificar se cada gênero em destaque tem o rótulo respecting:</span><span class="sxs-lookup"><span data-stu-id="966de-653">Change the URL to **/Store** to verify that each featured genre has the respecting label:</span></span>

    <span data-ttu-id="966de-654">![Navegação gêneros com elementos com estrelas](aspnet-mvc-4-fundamentals/_static/image35.png "gêneros navegação com elementos com estrelas")</span><span class="sxs-lookup"><span data-stu-id="966de-654">![Browsing Genres with starred elements](aspnet-mvc-4-fundamentals/_static/image35.png "Browsing Genres with starred elements")</span></span>

    <span data-ttu-id="966de-655">*Navegação gêneros com elementos com estrelas*</span><span class="sxs-lookup"><span data-stu-id="966de-655">*Browsing Genres with starred elements*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a><span data-ttu-id="966de-656">Exercício 7: Visão geral do novo modelo do ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="966de-656">Exercise 7: A lap around ASP.NET MVC 4 new template</span></span>

<span data-ttu-id="966de-657">Neste exercício, você irá explorar os aprimoramentos nos modelos de projeto ASP.NET MVC 4, levando uma aparência recursos mais relevantes do novo modelo.</span><span class="sxs-lookup"><span data-stu-id="966de-657">In this exercise, you will explore the enhancements in the ASP.NET MVC 4 project templates, taking a look at the most relevant features of the new template.</span></span>

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a><span data-ttu-id="966de-658">Tarefa 1: Explorando o modelo de aplicativo de Internet do ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="966de-658">Task 1: Exploring the ASP.NET MVC 4 Internet Application Template</span></span>

1. <span data-ttu-id="966de-659">Se não estiver aberto, inicie **VS Express para Web**</span><span class="sxs-lookup"><span data-stu-id="966de-659">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="966de-660">Selecione o **arquivo | Novo | Projeto** comando de menu.</span><span class="sxs-lookup"><span data-stu-id="966de-660">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="966de-661">No **novo projeto** caixa de diálogo, selecione o **Visual c# | Web** modelo no painel esquerdo da árvore e escolha o **aplicativo Web do ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="966de-661">In the **New Project** dialog, select the **Visual C#|Web** template on the left pane tree, and choose the **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="966de-662">**Nome** o projeto *MusicStore* e atualize o **nome da solução** para *começar*, em seguida, selecione um local (ou deixe o padrão) e clique em **Okey** .</span><span class="sxs-lookup"><span data-stu-id="966de-662">**Name** the project *MusicStore* and update the **solution name** to *Begin*, then select a location (or leave the default) and click **OK**.</span></span>

    <span data-ttu-id="966de-663">![Criar um novo projeto ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image36.png "criar um novo projeto ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="966de-663">![Creating a new ASP.NET MVC 4 Project](aspnet-mvc-4-fundamentals/_static/image36.png "Creating a new ASP.NET MVC 4 Project")</span></span>

    <span data-ttu-id="966de-664">*Criar um novo projeto ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="966de-664">*Creating a new ASP.NET MVC 4 Project*</span></span>
3. <span data-ttu-id="966de-665">No **novo projeto ASP.NET MVC 4** caixa de diálogo, selecione o **aplicativo de Internet** modelo de projeto e clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="966de-665">In the **New ASP.NET MVC 4 Project** dialog, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="966de-666">Observe que você pode selecionar Razor ou ASPX como o mecanismo de exibição.</span><span class="sxs-lookup"><span data-stu-id="966de-666">Notice you can select either Razor or ASPX as the view engine.</span></span>

    <span data-ttu-id="966de-667">![Criar um novo aplicativo ASP.NET MVC 4 Internet](aspnet-mvc-4-fundamentals/_static/image37.png "criar um novo aplicativo ASP.NET MVC 4 da Internet")</span><span class="sxs-lookup"><span data-stu-id="966de-667">![Creating a new ASP.NET MVC 4 Internet Application](aspnet-mvc-4-fundamentals/_static/image37.png "Creating a new ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="966de-668">*Criar um novo aplicativo ASP.NET MVC 4 da Internet*</span><span class="sxs-lookup"><span data-stu-id="966de-668">*Creating a new ASP.NET MVC 4 Internet Application*</span></span>

    > [!NOTE]
    > <span data-ttu-id="966de-669">A sintaxe do Razor foi introduzida no ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="966de-669">Razor syntax has been introduced in ASP.NET MVC 3.</span></span> <span data-ttu-id="966de-670">Seu objetivo é minimizar o número de caracteres e pressionamentos de tecla necessários em um arquivo, permitindo um rápido e fluido fluxo de trabalho de codificação.</span><span class="sxs-lookup"><span data-stu-id="966de-670">Its goal is to minimize the number of characters and keystrokes required in a file, enabling a fast and fluid coding workflow.</span></span> <span data-ttu-id="966de-671">Aproveita o Razor existente C# /VB (ou outro) habilidades de linguagem e fornece uma sintaxe de marcação de modelo que permite que um fluxo de trabalho de construção awesome HTML.</span><span class="sxs-lookup"><span data-stu-id="966de-671">Razor leverages existing C#/VB (or other) language skills and delivers a template markup syntax that enables an awesome HTML construction workflow.</span></span>
4. <span data-ttu-id="966de-672">Pressione **F5** para executar a solução e ver o modelo renovado.</span><span class="sxs-lookup"><span data-stu-id="966de-672">Press **F5** to run the solution and see the renewed template.</span></span> <span data-ttu-id="966de-673">Você pode conferir os recursos a seguir:</span><span class="sxs-lookup"><span data-stu-id="966de-673">You can check out the following features:</span></span>

    1. <span data-ttu-id="966de-674">**Modelos de estilo moderno**</span><span class="sxs-lookup"><span data-stu-id="966de-674">**Modern-style templates**</span></span>

        <span data-ttu-id="966de-675">Os modelos foram renovados, fornecendo mais estilos aparência moderna.</span><span class="sxs-lookup"><span data-stu-id="966de-675">The templates have been renewed, providing more modern-looking styles.</span></span>

        <span data-ttu-id="966de-676">![Modelos do ASP.NET MVC 4 remodelado](aspnet-mvc-4-fundamentals/_static/image38.png "remodelado modelos do ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="966de-676">![ASP.NET MVC 4 restyled templates](aspnet-mvc-4-fundamentals/_static/image38.png "ASP.NET MVC 4 restyled templates")</span></span>

        <span data-ttu-id="966de-677">*Modelos do ASP.NET MVC 4 remodelado*</span><span class="sxs-lookup"><span data-stu-id="966de-677">*ASP.NET MVC 4 restyled templates*</span></span>
    2. <span data-ttu-id="966de-678">**Renderização adaptável**</span><span class="sxs-lookup"><span data-stu-id="966de-678">**Adaptive Rendering**</span></span>

        <span data-ttu-id="966de-679">Check-out de redimensionar a janela do navegador e observe como o layout de página adapta dinamicamente para o novo tamanho da janela.</span><span class="sxs-lookup"><span data-stu-id="966de-679">Check out resizing the browser window and notice how the page layout dynamically adapts to the new window size.</span></span> <span data-ttu-id="966de-680">Esses modelos de usam a técnica de renderização adaptável para processar corretamente em plataformas móveis e desktop sem nenhuma personalização.</span><span class="sxs-lookup"><span data-stu-id="966de-680">These templates use the adaptive rendering technique to render properly in both desktop and mobile platforms without any customization.</span></span>

        <span data-ttu-id="966de-681">![Modelo de projeto ASP.NET MVC 4 em tamanhos diferentes do navegador](aspnet-mvc-4-fundamentals/_static/image39.png "modelo de projeto ASP.NET MVC 4 em tamanhos diferentes do navegador")</span><span class="sxs-lookup"><span data-stu-id="966de-681">![ASP.NET MVC 4 project template in different browser sizes](aspnet-mvc-4-fundamentals/_static/image39.png "ASP.NET MVC 4 project template in different browser sizes")</span></span>

        <span data-ttu-id="966de-682">*Modelo de projeto ASP.NET MVC 4 em tamanhos diferentes do navegador*</span><span class="sxs-lookup"><span data-stu-id="966de-682">*ASP.NET MVC 4 project template in different browser sizes*</span></span>
5. <span data-ttu-id="966de-683">Feche o navegador para interromper o depurador e retornar ao Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="966de-683">Close the browser to stop the debugger and return to Visual Studio.</span></span>
6. <span data-ttu-id="966de-684">Agora você já pode explorar a solução e check-out de alguns dos novos recursos introduzidos pelo ASP.NET MVC 4 no modelo de projeto.</span><span class="sxs-lookup"><span data-stu-id="966de-684">Now you are able to explore the solution and check out some of the new features introduced by ASP.NET MVC 4 in the project template.</span></span>

    <span data-ttu-id="966de-685">![ASP.NET MVC4-internet---modelo de projeto aplicativo](aspnet-mvc-4-fundamentals/_static/image40.png "o modelo de projeto de aplicativo de Internet do ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="966de-685">![ASP.NET MVC4-internet-application-project-template](aspnet-mvc-4-fundamentals/_static/image40.png "The ASP.NET MVC 4 Internet Application Project Template")</span></span>

    <span data-ttu-id="966de-686">*O modelo de projeto de aplicativo de Internet do ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="966de-686">*The ASP.NET MVC 4 Internet Application Project Template*</span></span>

    1. <span data-ttu-id="966de-687">**Marcação de HTML5**</span><span class="sxs-lookup"><span data-stu-id="966de-687">**HTML5 markup**</span></span>

        <span data-ttu-id="966de-688">Procurar exibições de modelo para descobrir a marcação novo tema, por exemplo aberta **About.cshtml** exibir em **início** pasta.</span><span class="sxs-lookup"><span data-stu-id="966de-688">Browse template views to find out the new theme markup, for example open **About.cshtml** view within **Home** folder.</span></span>

        <span data-ttu-id="966de-689">![Novo modelo, usando marcação Razor e HTML5](aspnet-mvc-4-fundamentals/_static/image41.png "novo modelo, usando marcação Razor e HTML5")</span><span class="sxs-lookup"><span data-stu-id="966de-689">![New template, using Razor and HTML5 markup](aspnet-mvc-4-fundamentals/_static/image41.png "New template, using Razor and HTML5 markup")</span></span>

        <span data-ttu-id="966de-690">*Novo modelo, usando marcação Razor e HTML5*</span><span class="sxs-lookup"><span data-stu-id="966de-690">*New template, using Razor and HTML5 markup*</span></span>
    2. <span data-ttu-id="966de-691">**Bibliotecas JavaScript incluídas**</span><span class="sxs-lookup"><span data-stu-id="966de-691">**JavaScript libraries included**</span></span>

        1. <span data-ttu-id="966de-692">**jQuery**: jQuery simplifica o desvio de documento HTML, manipulação de eventos, animação e interações de Ajax.</span><span class="sxs-lookup"><span data-stu-id="966de-692">**jQuery**: jQuery simplifies HTML document traversing, event handling, animating, and Ajax interactions.</span></span>
        2. <span data-ttu-id="966de-693">**jQuery UI**: esta biblioteca fornece abstrações de interação de baixo nível avançado de animações, efeitos e widgets de um tema, criadas sobre a biblioteca JavaScript jQuery.</span><span class="sxs-lookup"><span data-stu-id="966de-693">**jQuery UI**: This library provides abstractions for low-level interaction and animation, advanced effects and themeable widgets, built on top of the jQuery JavaScript Library.</span></span>

            > [!NOTE]
            > <span data-ttu-id="966de-694">Você pode aprender sobre jQuery e jQuery UI em [ [http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span><span class="sxs-lookup"><span data-stu-id="966de-694">You can learn about jQuery and jQuery UI in [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span></span>
        3. <span data-ttu-id="966de-695">**KnockoutJS**: modelo de padrão do ASP.NET MVC 4 agora inclui **KnockoutJS**, uma estrutura de JavaScript MVVM que permite que você crie aplicativos web altamente responsivo e avançado usando JavaScript e HTML.</span><span class="sxs-lookup"><span data-stu-id="966de-695">**KnockoutJS**: The ASP.NET MVC 4 default template now includes **KnockoutJS**, a JavaScript MVVM framework that lets you create rich and highly responsive web applications using JavaScript and HTML.</span></span> <span data-ttu-id="966de-696">Como no ASP.NET MVC 3, jQuery e jQuery bibliotecas de interface do usuário também estão incluídas no ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="966de-696">Like in ASP.NET MVC 3, jQuery and jQuery UI libraries are also included in ASP.NET MVC 4.</span></span>

            > [!NOTE]
            > <span data-ttu-id="966de-697">Você pode obter mais informações sobre a biblioteca KnockOutJS neste link: [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).</span><span class="sxs-lookup"><span data-stu-id="966de-697">You can get more information about KnockOutJS library in this link: [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).</span></span>
        4. <span data-ttu-id="966de-698">**Modernizr**: esta biblioteca é executado automaticamente, tornando o seu site compatível com navegadores mais antigos com tecnologias de HTML5 e CSS3.</span><span class="sxs-lookup"><span data-stu-id="966de-698">**Modernizr**: This library runs automatically, making your site compatible with older browsers when using HTML5 and CSS3 technologies.</span></span>

            > [!NOTE]
            > <span data-ttu-id="966de-699">Você pode obter mais informações sobre a biblioteca Modernizr presentes neste link: [http://www.modernizr.com/](http://www.modernizr.com/).</span><span class="sxs-lookup"><span data-stu-id="966de-699">You can get more information about Modernizr library in this link: [http://www.modernizr.com/](http://www.modernizr.com/).</span></span>
    3. <span data-ttu-id="966de-700">**SimpleMembership incluído na solução**</span><span class="sxs-lookup"><span data-stu-id="966de-700">**SimpleMembership included in the solution**</span></span>

        <span data-ttu-id="966de-701">SimpleMembership foi projetado como uma substituição para o sistema de provedor de função ASP.NET e associação anterior.</span><span class="sxs-lookup"><span data-stu-id="966de-701">SimpleMembership has been designed as a replacement for the previous ASP.NET Role and Membership provider system.</span></span> <span data-ttu-id="966de-702">Ele tem muitos recursos novos que facilitam para o desenvolvedor de páginas da web seguras de forma mais flexível.</span><span class="sxs-lookup"><span data-stu-id="966de-702">It has many new features that make it easier for the developer to secure web pages in a more flexible way.</span></span>

        <span data-ttu-id="966de-703">O modelo de Internet já configurou algumas coisas a integrar SimpleMembership, por exemplo, o AccountController está preparado para usar OAuthWebSecurity (para o registro de conta OAuth, login, gerenciamento, etc.) e segurança da Web.</span><span class="sxs-lookup"><span data-stu-id="966de-703">The Internet template already has set up a few things to integrate SimpleMembership, for example, the AccountController is prepared to use OAuthWebSecurity (for OAuth account registration, login, management, etc.) and Web Security.</span></span>

        <span data-ttu-id="966de-704">![SimpleMembership incluídos na solução](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership incluídos na solução")</span><span class="sxs-lookup"><span data-stu-id="966de-704">![SimpleMembership Included in the solution](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership Included in the solution")</span></span>

        <span data-ttu-id="966de-705">*SimpleMembership incluídos na solução*</span><span class="sxs-lookup"><span data-stu-id="966de-705">*SimpleMembership Included in the solution*</span></span>

        > [!NOTE]
        > <span data-ttu-id="966de-706">Para obter mais informações sobre [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) no MSDN.</span><span class="sxs-lookup"><span data-stu-id="966de-706">Find more information about [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) in MSDN.</span></span>

> [!NOTE]
> <span data-ttu-id="966de-707">Além disso, você pode implantar esse aplicativo para Windows Azure Web Sites a seguir [apêndice b: publicação um aplicativo ASP.NET MVC 4 usando a implantação da Web](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="966de-707">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="966de-708">Resumo</span><span class="sxs-lookup"><span data-stu-id="966de-708">Summary</span></span>

<span data-ttu-id="966de-709">Ao concluir este laboratório prático, você aprendeu os fundamentos do ASP.NET MVC:</span><span class="sxs-lookup"><span data-stu-id="966de-709">By completing this Hands-On Lab you have learned the fundamentals of ASP.NET MVC:</span></span>

- <span data-ttu-id="966de-710">Os elementos principais de um aplicativo MVC e como eles interagem</span><span class="sxs-lookup"><span data-stu-id="966de-710">The core elements of an MVC application and how they interact</span></span>
- <span data-ttu-id="966de-711">Como criar um aplicativo ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="966de-711">How to create an ASP.NET MVC Application</span></span>
- <span data-ttu-id="966de-712">Como adicionar e configurar controladores para lidar com os parâmetros passados a URL e querystring</span><span class="sxs-lookup"><span data-stu-id="966de-712">How to add and configure Controllers to handle parameters passed through the URL and querystring</span></span>
- <span data-ttu-id="966de-713">Como adicionar uma página de layout mestre para configurar um modelo de conteúdo HTML comum, uma folha de estilos para aprimorar a aparência e a um modelo de exibição para exibir o conteúdo HTML</span><span class="sxs-lookup"><span data-stu-id="966de-713">How to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel and a View template to display HTML content</span></span>
- <span data-ttu-id="966de-714">Como usar o padrão de ViewModel para passar propriedades para o modelo de exibição para exibir informações dinâmicas</span><span class="sxs-lookup"><span data-stu-id="966de-714">How to use the ViewModel pattern for passing properties to the View template to display dynamic information</span></span>
- <span data-ttu-id="966de-715">Como usar parâmetros passados para os controladores no modelo de exibição</span><span class="sxs-lookup"><span data-stu-id="966de-715">How to use parameters passed to Controllers in the View template</span></span>
- <span data-ttu-id="966de-716">Como adicionar links para páginas dentro do aplicativo ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="966de-716">How to add links to pages inside the ASP.NET MVC application</span></span>
- <span data-ttu-id="966de-717">Como adicionar e usar propriedades dinâmicas em uma exibição</span><span class="sxs-lookup"><span data-stu-id="966de-717">How to add and use dynamic properties in a View</span></span>
- <span data-ttu-id="966de-718">Os aprimoramentos nos modelos de projeto do ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="966de-718">The enhancements in the ASP.NET MVC 4 project templates</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="966de-719">Apêndice a: instalar o Visual Studio Express 2012 para Web</span><span class="sxs-lookup"><span data-stu-id="966de-719">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="966de-720">Você pode instalar **Microsoft Visual Studio Express 2012 para Web** ou outro &quot;Express&quot; versão usando o  **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="966de-720">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="966de-721">As instruções a seguir guiá-lo pelas etapas necessárias para instalar o *Visual studio Express 2012 para Web* usando *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="966de-721">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="966de-722">Vá para [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="966de-722">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="966de-723">Como alternativa, se você já tiver instalado o Web Platform Installer, você pode abri-la e procure o produto &quot; *Visual Studio Express 2012 para Web com o SDK do Windows Azure*&quot;.</span><span class="sxs-lookup"><span data-stu-id="966de-723">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="966de-724">Clique em **instalar agora**.</span><span class="sxs-lookup"><span data-stu-id="966de-724">Click on **Install Now**.</span></span> <span data-ttu-id="966de-725">Se você não tem **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.</span><span class="sxs-lookup"><span data-stu-id="966de-725">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="966de-726">Uma vez **Web Platform Installer** é aberto, clique em **instalar** para iniciar a instalação.</span><span class="sxs-lookup"><span data-stu-id="966de-726">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="966de-727">![Instalar Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "instalar Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="966de-727">![Install Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="966de-728">*Instalar Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="966de-728">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="966de-729">Leia os termos e licenças de todos os produtos e clique em **aceito** para continuar.</span><span class="sxs-lookup"><span data-stu-id="966de-729">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Aceitar os termos de licença](aspnet-mvc-4-fundamentals/_static/image44.png)

    <span data-ttu-id="966de-731">*Aceitar os termos de licença*</span><span class="sxs-lookup"><span data-stu-id="966de-731">*Accepting the license terms*</span></span>
5. <span data-ttu-id="966de-732">Aguarde até que o processo de download e instalação seja concluída.</span><span class="sxs-lookup"><span data-stu-id="966de-732">Wait until the downloading and installation process completes.</span></span>

    ![Progresso da instalação](aspnet-mvc-4-fundamentals/_static/image45.png)

    <span data-ttu-id="966de-734">*Progresso da instalação*</span><span class="sxs-lookup"><span data-stu-id="966de-734">*Installation progress*</span></span>
6. <span data-ttu-id="966de-735">Quando a instalação for concluída, clique em **concluir**.</span><span class="sxs-lookup"><span data-stu-id="966de-735">When the installation completes, click **Finish**.</span></span>

    ![Instalação concluída](aspnet-mvc-4-fundamentals/_static/image46.png)

    <span data-ttu-id="966de-737">*Instalação concluída*</span><span class="sxs-lookup"><span data-stu-id="966de-737">*Installation completed*</span></span>
7. <span data-ttu-id="966de-738">Clique em **saída** para fechar o Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="966de-738">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="966de-739">Para abrir o Visual Studio Express para Web, vá para o **iniciar** tela e comece a escrever &quot; **VS Express**&quot;, em seguida, clique no **VS Express para Web** lado a lado.</span><span class="sxs-lookup"><span data-stu-id="966de-739">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express para o bloco de Web](aspnet-mvc-4-fundamentals/_static/image47.png)

    <span data-ttu-id="966de-741">*VS Express para o bloco de Web*</span><span class="sxs-lookup"><span data-stu-id="966de-741">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="966de-742">Apêndice b: publicar um aplicativo ASP.NET MVC 4 usando a implantação da Web</span><span class="sxs-lookup"><span data-stu-id="966de-742">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="966de-743">Este apêndice mostram como criar um novo site do Portal de gerenciamento do Windows Azure e publicar o aplicativo que você obteve seguindo o laboratório, aproveitando o recurso de publicação de implantação da Web fornecido pelo Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="966de-743">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="966de-744">Tarefa 1: criar um novo Site do Windows Azure Portal</span><span class="sxs-lookup"><span data-stu-id="966de-744">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="966de-745">Vá para o [Portal de gerenciamento do Windows Azure](https://manage.windowsazure.com/) e entre usando as credenciais da Microsoft associadas à sua assinatura.</span><span class="sxs-lookup"><span data-stu-id="966de-745">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="966de-746">Com o Windows Azure, você pode hospedar 10 Sites da Web ASP.NET gratuitamente e, em seguida, dimensione conforme seu tráfego cresce.</span><span class="sxs-lookup"><span data-stu-id="966de-746">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="966de-747">Você pode inscrever [aqui](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="966de-747">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="966de-748">![Faça logon no portal do Windows Azure](aspnet-mvc-4-fundamentals/_static/image48.png "faça logon no portal do Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="966de-748">![Log on to Windows Azure portal](aspnet-mvc-4-fundamentals/_static/image48.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="966de-749">*Faça logon no Portal de gerenciamento do Azure do Windows*</span><span class="sxs-lookup"><span data-stu-id="966de-749">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="966de-750">Clique em **novo** na barra de comandos.</span><span class="sxs-lookup"><span data-stu-id="966de-750">Click **New** on the command bar.</span></span>

    <span data-ttu-id="966de-751">![Criando um novo Site](aspnet-mvc-4-fundamentals/_static/image49.png "criar um novo Site")</span><span class="sxs-lookup"><span data-stu-id="966de-751">![Creating a new Web Site](aspnet-mvc-4-fundamentals/_static/image49.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="966de-752">*Criando um novo Site*</span><span class="sxs-lookup"><span data-stu-id="966de-752">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="966de-753">Clique em **de computação** | **Site da Web**.</span><span class="sxs-lookup"><span data-stu-id="966de-753">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="966de-754">Em seguida, selecione **criação rápida** opção.</span><span class="sxs-lookup"><span data-stu-id="966de-754">Then select **Quick Create** option.</span></span> <span data-ttu-id="966de-755">Forneça uma URL disponível para o novo site e clique em **Criar Site**.</span><span class="sxs-lookup"><span data-stu-id="966de-755">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="966de-756">Um Site do Windows Azure é o host para um aplicativo web em execução na nuvem que você pode controlar e gerenciar.</span><span class="sxs-lookup"><span data-stu-id="966de-756">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="966de-757">A opção criação rápida permite que você implante um aplicativo web concluído ao Site do Windows Azure de fora do portal.</span><span class="sxs-lookup"><span data-stu-id="966de-757">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="966de-758">Ele não inclui etapas para configurar um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="966de-758">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="966de-759">![Criando um novo Site usando a criação rápida](aspnet-mvc-4-fundamentals/_static/image50.png "criar um novo Site usando a criação rápida")</span><span class="sxs-lookup"><span data-stu-id="966de-759">![Creating a new Web Site using Quick Create](aspnet-mvc-4-fundamentals/_static/image50.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="966de-760">*Criando um novo Site usando a criação rápida*</span><span class="sxs-lookup"><span data-stu-id="966de-760">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="966de-761">Aguarde até que o novo **Site da Web** é criado.</span><span class="sxs-lookup"><span data-stu-id="966de-761">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="966de-762">Depois que o Site da Web é criado, clique no link sob a **URL** coluna.</span><span class="sxs-lookup"><span data-stu-id="966de-762">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="966de-763">Verifique se o novo Site da Web está funcionando.</span><span class="sxs-lookup"><span data-stu-id="966de-763">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="966de-764">![Navegando para o novo site](aspnet-mvc-4-fundamentals/_static/image51.png "navegando para o novo site")</span><span class="sxs-lookup"><span data-stu-id="966de-764">![Browsing to the new web site](aspnet-mvc-4-fundamentals/_static/image51.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="966de-765">*Navegando para o novo site*</span><span class="sxs-lookup"><span data-stu-id="966de-765">*Browsing to the new web site*</span></span>

    <span data-ttu-id="966de-766">![Site da Web em execução](aspnet-mvc-4-fundamentals/_static/image52.png "site em execução")</span><span class="sxs-lookup"><span data-stu-id="966de-766">![Web site running](aspnet-mvc-4-fundamentals/_static/image52.png "Web site running")</span></span>

    <span data-ttu-id="966de-767">*Site da Web em execução*</span><span class="sxs-lookup"><span data-stu-id="966de-767">*Web site running*</span></span>
6. <span data-ttu-id="966de-768">Acesse o portal e clique no nome do site sob o **nome** coluna para exibir as páginas de gerenciamento.</span><span class="sxs-lookup"><span data-stu-id="966de-768">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="966de-769">![Abrir as páginas de gerenciamento do site da web](aspnet-mvc-4-fundamentals/_static/image53.png "abrir páginas de gerenciamento do site")</span><span class="sxs-lookup"><span data-stu-id="966de-769">![Opening the web site management pages](aspnet-mvc-4-fundamentals/_static/image53.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="966de-770">*Abrir as páginas de gerenciamento do Site da Web*</span><span class="sxs-lookup"><span data-stu-id="966de-770">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="966de-771">No **painel** página no **visão rápida** seção, clique o **baixar perfil de publicação** link.</span><span class="sxs-lookup"><span data-stu-id="966de-771">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="966de-772">O *perfil de publicação* contém todas as informações necessárias para publicar um aplicativo web para um site do Windows Azure para cada método de publicação habilitado.</span><span class="sxs-lookup"><span data-stu-id="966de-772">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="966de-773">O perfil de publicação contém as URLs, as credenciais do usuário e cadeias de caracteres de banco de dados necessárias para se conectar e autenticar cada um dos pontos de extremidade para o qual um método de publicação está habilitado.</span><span class="sxs-lookup"><span data-stu-id="966de-773">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="966de-774">**Microsoft WebMatrix 2**, **Microsoft Visual Studio para Web Express** e **Microsoft Visual Studio 2012** oferecem suporte à leitura perfis de publicação para automatizar a configuração desses programas para publicação de aplicativos web para sites do Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="966de-774">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="966de-775">![Baixando o site da web de perfil de publicação](aspnet-mvc-4-fundamentals/_static/image54.png "baixando o site da web de perfil de publicação")</span><span class="sxs-lookup"><span data-stu-id="966de-775">![Downloading the web site publish profile](aspnet-mvc-4-fundamentals/_static/image54.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="966de-776">*Baixando o Site da Web de perfil de publicação*</span><span class="sxs-lookup"><span data-stu-id="966de-776">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="966de-777">Baixe o arquivo de perfil de publicação para um local conhecido.</span><span class="sxs-lookup"><span data-stu-id="966de-777">Download the publish profile file to a known location.</span></span> <span data-ttu-id="966de-778">Mais neste exercício, você verá como usar esse arquivo para publicar um aplicativo web para um Windows Azure Web Sites do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="966de-778">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="966de-779">![Salvando o arquivo de perfil de publicação](aspnet-mvc-4-fundamentals/_static/image55.png "salvar o perfil de publicação")</span><span class="sxs-lookup"><span data-stu-id="966de-779">![Saving the publish profile file](aspnet-mvc-4-fundamentals/_static/image55.png "Saving the publish profile")</span></span>

    <span data-ttu-id="966de-780">*Salvando o arquivo de perfil de publicação*</span><span class="sxs-lookup"><span data-stu-id="966de-780">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="966de-781">Tarefa 2 – Configurando o servidor de banco de dados</span><span class="sxs-lookup"><span data-stu-id="966de-781">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="966de-782">Se seu aplicativo utiliza o SQL Server bancos de dados que você precisará criar um servidor de banco de dados SQL.</span><span class="sxs-lookup"><span data-stu-id="966de-782">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="966de-783">Se você deseja implantar um aplicativo simples que não usa o SQL Server, você pode ignorar esta tarefa.</span><span class="sxs-lookup"><span data-stu-id="966de-783">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="966de-784">Você precisará de um servidor de banco de dados SQL para armazenar o banco de dados do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="966de-784">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="966de-785">Você pode exibir os servidores de banco de dados SQL de sua assinatura no portal de gerenciamento do Windows Azure em **bancos de dados Sql** | **servidores** | **do servidor Painel**.</span><span class="sxs-lookup"><span data-stu-id="966de-785">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="966de-786">Se você não tiver um servidor criado, você pode criar um usando o **adicionar** botão na barra de comandos.</span><span class="sxs-lookup"><span data-stu-id="966de-786">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="966de-787">Anote o **nome do servidor e o URL, o nome de logon de administrador e senha**, pois você usará nas próximas tarefas.</span><span class="sxs-lookup"><span data-stu-id="966de-787">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="966de-788">Não crie o banco de dados, como ele será criado em um estágio posterior.</span><span class="sxs-lookup"><span data-stu-id="966de-788">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="966de-789">![Painel do servidor de banco de dados SQL](aspnet-mvc-4-fundamentals/_static/image56.png "painel de banco de dados do SQL Server")</span><span class="sxs-lookup"><span data-stu-id="966de-789">![SQL Database Server Dashboard](aspnet-mvc-4-fundamentals/_static/image56.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="966de-790">*Painel de banco de dados do SQL Server*</span><span class="sxs-lookup"><span data-stu-id="966de-790">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="966de-791">A próxima tarefa, você testará a conexão de banco de dados do Visual Studio, por esse motivo, você precisa incluir seu endereço IP local na lista do servidor de **endereços IP permitidos**.</span><span class="sxs-lookup"><span data-stu-id="966de-791">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="966de-792">Para fazer isso, clique em **configurar**, selecione o endereço IP do **endereço IP do cliente atual** e cole-o no **endereço IP inicial** e **oendereçoIPfinal** caixas de texto e clique no ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) botão.</span><span class="sxs-lookup"><span data-stu-id="966de-792">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) button.</span></span>

    ![Adicionar endereço IP do cliente](aspnet-mvc-4-fundamentals/_static/image58.png)

    <span data-ttu-id="966de-794">*Adicionar endereço IP do cliente*</span><span class="sxs-lookup"><span data-stu-id="966de-794">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="966de-795">Uma vez o **endereço IP do cliente** é adicionado para os endereços IP permitidos, clique em **salvar** para confirmar as alterações.</span><span class="sxs-lookup"><span data-stu-id="966de-795">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Confirmar alterações](aspnet-mvc-4-fundamentals/_static/image59.png)

    <span data-ttu-id="966de-797">*Confirmar alterações*</span><span class="sxs-lookup"><span data-stu-id="966de-797">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="966de-798">Tarefa 3: publicar um aplicativo ASP.NET MVC 4 usando a implantação da Web</span><span class="sxs-lookup"><span data-stu-id="966de-798">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="966de-799">Volte para a solução do ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="966de-799">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="966de-800">No **Solution Explorer**, com o botão direito no projeto de site da web e selecione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="966de-800">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="966de-801">![Publicar o aplicativo](aspnet-mvc-4-fundamentals/_static/image60.png "a publicação do aplicativo")</span><span class="sxs-lookup"><span data-stu-id="966de-801">![Publishing the Application](aspnet-mvc-4-fundamentals/_static/image60.png "Publishing the Application")</span></span>

    <span data-ttu-id="966de-802">*Publicar o site*</span><span class="sxs-lookup"><span data-stu-id="966de-802">*Publishing the web site*</span></span>
2. <span data-ttu-id="966de-803">Importe o perfil de publicação que você salvou na primeira tarefa.</span><span class="sxs-lookup"><span data-stu-id="966de-803">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="966de-804">![Importar o perfil de publicação](aspnet-mvc-4-fundamentals/_static/image61.png "importar o perfil de publicação")</span><span class="sxs-lookup"><span data-stu-id="966de-804">![Importing the publish profile](aspnet-mvc-4-fundamentals/_static/image61.png "Importing the publish profile")</span></span>

    <span data-ttu-id="966de-805">*Importar o perfil de publicação*</span><span class="sxs-lookup"><span data-stu-id="966de-805">*Importing publish profile*</span></span>
3. <span data-ttu-id="966de-806">Clique em **validar Conexão**.</span><span class="sxs-lookup"><span data-stu-id="966de-806">Click **Validate Connection**.</span></span> <span data-ttu-id="966de-807">Quando a validação estiver concluída, clique em **próximo**.</span><span class="sxs-lookup"><span data-stu-id="966de-807">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="966de-808">A validação é concluída depois que aparecer uma marca de seleção verde ao lado do botão Validar Conexão.</span><span class="sxs-lookup"><span data-stu-id="966de-808">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="966de-809">![Validando a conexão](aspnet-mvc-4-fundamentals/_static/image62.png "validação de conexão")</span><span class="sxs-lookup"><span data-stu-id="966de-809">![Validating connection](aspnet-mvc-4-fundamentals/_static/image62.png "Validating connection")</span></span>

    <span data-ttu-id="966de-810">*Validação de conexão*</span><span class="sxs-lookup"><span data-stu-id="966de-810">*Validating connection*</span></span>
4. <span data-ttu-id="966de-811">No **configurações** página no **bancos de dados** seção, clique no botão ao lado da caixa de texto da sua conexão de banco de dados (ou seja, **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="966de-811">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="966de-812">![Configuração de implantação da Web](aspnet-mvc-4-fundamentals/_static/image63.png "configuração de implantação da Web")</span><span class="sxs-lookup"><span data-stu-id="966de-812">![Web deploy configuration](aspnet-mvc-4-fundamentals/_static/image63.png "Web deploy configuration")</span></span>

    <span data-ttu-id="966de-813">*Configuração de implantação da Web*</span><span class="sxs-lookup"><span data-stu-id="966de-813">*Web deploy configuration*</span></span>
5. <span data-ttu-id="966de-814">Configure a conexão de banco de dados da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="966de-814">Configure the database connection as follows:</span></span>

    - <span data-ttu-id="966de-815">No **nome do servidor** digite sua URL de servidor de banco de dados SQL usando o *tcp:* prefixo.</span><span class="sxs-lookup"><span data-stu-id="966de-815">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
    - <span data-ttu-id="966de-816">Em **nome de usuário** digite seu nome de logon de administrador do servidor.</span><span class="sxs-lookup"><span data-stu-id="966de-816">In **User name** type your server administrator login name.</span></span>
    - <span data-ttu-id="966de-817">Em **senha** digite sua senha de logon de administrador do servidor.</span><span class="sxs-lookup"><span data-stu-id="966de-817">In **Password** type your server administrator login password.</span></span>
    - <span data-ttu-id="966de-818">Digite um novo nome de banco de dados, por exemplo: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="966de-818">Type a new database name, for example: *MVC4SampleDB*.</span></span>

    <span data-ttu-id="966de-819">![Configurando a cadeia de caracteres de conexão de destino](aspnet-mvc-4-fundamentals/_static/image64.png "Configurando a cadeia de caracteres de conexão de destino")</span><span class="sxs-lookup"><span data-stu-id="966de-819">![Configuring destination connection string](aspnet-mvc-4-fundamentals/_static/image64.png "Configuring destination connection string")</span></span>

    <span data-ttu-id="966de-820">*Configurando a cadeia de caracteres de conexão de destino*</span><span class="sxs-lookup"><span data-stu-id="966de-820">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="966de-821">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="966de-821">Then click **OK**.</span></span> <span data-ttu-id="966de-822">Quando solicitado a criar o banco de dados, clique em **Sim**.</span><span class="sxs-lookup"><span data-stu-id="966de-822">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="966de-823">![Criando o banco de dados](aspnet-mvc-4-fundamentals/_static/image65.png "criar a cadeia de caracteres do banco de dados")</span><span class="sxs-lookup"><span data-stu-id="966de-823">![Creating the database](aspnet-mvc-4-fundamentals/_static/image65.png "Creating the database string")</span></span>

    <span data-ttu-id="966de-824">*Criando o banco de dados*</span><span class="sxs-lookup"><span data-stu-id="966de-824">*Creating the database*</span></span>
7. <span data-ttu-id="966de-825">A cadeia de caracteres de conexão que você usará para se conectar ao banco de dados SQL no Windows Azure é mostrada na caixa de texto de Conexão padrão.</span><span class="sxs-lookup"><span data-stu-id="966de-825">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="966de-826">Clique em **Avançar**.</span><span class="sxs-lookup"><span data-stu-id="966de-826">Then click **Next**.</span></span>

    <span data-ttu-id="966de-827">![Cadeia de caracteres de Conexão que aponta para o banco de dados SQL](aspnet-mvc-4-fundamentals/_static/image66.png "cadeia de caracteres de Conexão que aponta para o banco de dados SQL")</span><span class="sxs-lookup"><span data-stu-id="966de-827">![Connection string pointing to SQL Database](aspnet-mvc-4-fundamentals/_static/image66.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="966de-828">*Cadeia de caracteres de Conexão que aponta para o banco de dados SQL*</span><span class="sxs-lookup"><span data-stu-id="966de-828">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="966de-829">No **visualização** , clique em **publicar**.</span><span class="sxs-lookup"><span data-stu-id="966de-829">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="966de-830">![Publicar o aplicativo web](aspnet-mvc-4-fundamentals/_static/image67.png "publicar o aplicativo web")</span><span class="sxs-lookup"><span data-stu-id="966de-830">![Publishing the web application](aspnet-mvc-4-fundamentals/_static/image67.png "Publishing the web application")</span></span>

    <span data-ttu-id="966de-831">*Publicar o aplicativo web*</span><span class="sxs-lookup"><span data-stu-id="966de-831">*Publishing the web application*</span></span>
9. <span data-ttu-id="966de-832">Quando termina o processo de publicação, o navegador padrão abrirá o site da web publicado.</span><span class="sxs-lookup"><span data-stu-id="966de-832">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="966de-833">![Aplicativo publicado no Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "aplicativo publicado no Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="966de-833">![Application published to Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="966de-834">*Aplicativos publicados no Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="966de-834">*Application published to Windows Azure*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="966de-835">Apêndice c: usar trechos de código</span><span class="sxs-lookup"><span data-stu-id="966de-835">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="966de-836">Com trechos de código, você tem todo o código que é necessário ao seu alcance.</span><span class="sxs-lookup"><span data-stu-id="966de-836">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="966de-837">O documento de laboratório informará exatamente quando você pode usá-los, conforme mostrado na figura a seguir.</span><span class="sxs-lookup"><span data-stu-id="966de-837">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="966de-838">![Usando trechos de código do Visual Studio para inserir o código no seu projeto](aspnet-mvc-4-fundamentals/_static/image69.png "trechos de código com o Visual Studio para inserir código em seu projeto")</span><span class="sxs-lookup"><span data-stu-id="966de-838">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-fundamentals/_static/image69.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="966de-839">*Usando trechos de código do Visual Studio para inserir código em seu projeto*</span><span class="sxs-lookup"><span data-stu-id="966de-839">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="966de-840">***Para adicionar um trecho de código usando o teclado (apenas c#)***</span><span class="sxs-lookup"><span data-stu-id="966de-840">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="966de-841">Posicione o cursor onde você deseja inserir o código.</span><span class="sxs-lookup"><span data-stu-id="966de-841">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="966de-842">Comece a digitar o nome do trecho (sem espaços ou hifens).</span><span class="sxs-lookup"><span data-stu-id="966de-842">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="966de-843">Observe como IntelliSense exibe os nomes dos trechos de código correspondentes.</span><span class="sxs-lookup"><span data-stu-id="966de-843">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="966de-844">Selecione o trecho correto (ou continue digitando até que o nome do trecho de código inteiro é selecionado).</span><span class="sxs-lookup"><span data-stu-id="966de-844">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="966de-845">Pressione a tecla Tab duas vezes para inserir o trecho de código no local do cursor.</span><span class="sxs-lookup"><span data-stu-id="966de-845">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="966de-846">![Comece a digitar o nome do fragmento](aspnet-mvc-4-fundamentals/_static/image70.png "comece a digitar o nome do trecho de código")</span><span class="sxs-lookup"><span data-stu-id="966de-846">![Start typing the snippet name](aspnet-mvc-4-fundamentals/_static/image70.png "Start typing the snippet name")</span></span>

<span data-ttu-id="966de-847">*Comece a digitar o nome do trecho de código*</span><span class="sxs-lookup"><span data-stu-id="966de-847">*Start typing the snippet name*</span></span>

<span data-ttu-id="966de-848">![Pressione Tab para selecionar o trecho de código realçado](aspnet-mvc-4-fundamentals/_static/image71.png "pressione Tab para selecionar o trecho de código realçado")</span><span class="sxs-lookup"><span data-stu-id="966de-848">![Press Tab to select the highlighted snippet](aspnet-mvc-4-fundamentals/_static/image71.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="966de-849">*Pressione Tab para selecionar o trecho de código realçado*</span><span class="sxs-lookup"><span data-stu-id="966de-849">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="966de-850">![Pressione Tab novamente e o trecho expandirá](aspnet-mvc-4-fundamentals/_static/image72.png "expandirá pressione Tab novamente e o trecho de código")</span><span class="sxs-lookup"><span data-stu-id="966de-850">![Press Tab again and the snippet will expand](aspnet-mvc-4-fundamentals/_static/image72.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="966de-851">*Pressione Tab novamente e o trecho de código serão expandida*</span><span class="sxs-lookup"><span data-stu-id="966de-851">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="966de-852">***Para adicionar um trecho de código usando o mouse (c#, Visual Basic e XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="966de-852">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="966de-853">Clique com botão direito em que você deseja inserir o trecho de código.</span><span class="sxs-lookup"><span data-stu-id="966de-853">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="966de-854">Selecione **Inserir trecho** seguido por **Meus trechos de código**.</span><span class="sxs-lookup"><span data-stu-id="966de-854">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="966de-855">Selecione o trecho relevante na lista, clicando nele.</span><span class="sxs-lookup"><span data-stu-id="966de-855">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="966de-856">![Com o botão direito em que você deseja inserir o trecho de código e selecione Inserir trecho](aspnet-mvc-4-fundamentals/_static/image73.png "com o botão direito em que você deseja inserir o trecho de código e selecione Inserir trecho")</span><span class="sxs-lookup"><span data-stu-id="966de-856">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-fundamentals/_static/image73.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="966de-857">*Clique com botão direito em que você deseja inserir o trecho de código e selecione Inserir trecho*</span><span class="sxs-lookup"><span data-stu-id="966de-857">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="966de-858">![Selecione o trecho relevante na lista, clicando nele](aspnet-mvc-4-fundamentals/_static/image74.png "escolher o trecho relevante na lista, clicando nele")</span><span class="sxs-lookup"><span data-stu-id="966de-858">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-fundamentals/_static/image74.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="966de-859">*Selecione o trecho relevante na lista, clicando nele*</span><span class="sxs-lookup"><span data-stu-id="966de-859">*Pick the relevant snippet from the list, by clicking on it*</span></span>
