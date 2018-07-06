---
uid: visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
title: 'Laboratório prático: One ASP.NET: a integração do ASP.NET Web Forms, MVC e API da Web | Microsoft Docs'
author: rick-anderson
description: O ASP.NET é uma estrutura para criar sites da Web, aplicativos e serviços usando tecnologias especializadas, como MVC, API da Web e outros. Com a expansão h do ASP.NET...
ms.author: aspnetcontent
ms.date: 07/16/2014
ms.assetid: 4fe2558d-67cc-4d12-a5c1-6fb9f6f16137
msc.legacyurl: /visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
msc.type: authoredcontent
ms.openlocfilehash: 276207a6e7d2388ce53778928665c35de9327318
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837299"
---
<a name="hands-on-lab-one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api"></a><span data-ttu-id="22517-104">Laboratório prático: One ASP.NET: a integração do ASP.NET Web Forms, MVC e API da Web</span><span class="sxs-lookup"><span data-stu-id="22517-104">Hands On Lab: One ASP.NET: Integrating ASP.NET Web Forms, MVC and Web API</span></span>
====================
<span data-ttu-id="22517-105">por [Web Camps equipe](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="22517-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="22517-106">Baixe o Kit de treinamento do Web Camps</span><span class="sxs-lookup"><span data-stu-id="22517-106">Download Web Camps Training Kit</span></span>](http://aka.ms/webcamps-training-kit)

> <span data-ttu-id="22517-107">O ASP.NET é uma estrutura para criar sites da Web, aplicativos e serviços usando tecnologias especializadas, como MVC, API da Web e outros.</span><span class="sxs-lookup"><span data-stu-id="22517-107">ASP.NET is a framework for building Web sites, apps and services using specialized technologies such as MVC, Web API and others.</span></span> <span data-ttu-id="22517-108">Com a expansão do ASP.NET tem visto desde sua criação e o expresso precisa ter essas tecnologias integradas, houve esforços recentes no trabalho em direção **One ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="22517-108">With the expansion ASP.NET has seen since its creation and the expressed need to have these technologies integrated, there have been recent efforts in working towards **One ASP.NET**.</span></span>
> 
> <span data-ttu-id="22517-109">Visual Studio 2013 apresenta um novo sistema de projeto unificado que permite criar um aplicativo e usar todas as tecnologias do ASP.NET em um único projeto.</span><span class="sxs-lookup"><span data-stu-id="22517-109">Visual Studio 2013 introduces a new unified project system which lets you build an application and use all the ASP.NET technologies in one project.</span></span> <span data-ttu-id="22517-110">Esse recurso elimina a necessidade de escolher uma tecnologia no início de um projeto e um pen drive com ele e, em vez disso, incentiva o uso de várias estruturas do ASP.NET dentro de um projeto.</span><span class="sxs-lookup"><span data-stu-id="22517-110">This feature eliminates the need to pick one technology at the start of a project and stick with it, and instead encourages the use of multiple ASP.NET frameworks within one project.</span></span>
> 
> <span data-ttu-id="22517-111">Todo o código de exemplo e trechos de código são incluídos no Web Camps treinamento Kit, disponível em [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="22517-111">All sample code and snippets are included in the Web Camps Training Kit, available at [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span></span>


<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="22517-112">Visão geral</span><span class="sxs-lookup"><span data-stu-id="22517-112">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="22517-113">Objetivos</span><span class="sxs-lookup"><span data-stu-id="22517-113">Objectives</span></span>

<span data-ttu-id="22517-114">Neste laboratório prático, você aprenderá como:</span><span class="sxs-lookup"><span data-stu-id="22517-114">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="22517-115">Criar um site com base nas **One ASP.NET** tipo de projeto</span><span class="sxs-lookup"><span data-stu-id="22517-115">Create a Web site based on the **One ASP.NET** project type</span></span>
- <span data-ttu-id="22517-116">Usar outra **ASP.NET** estruturas como **MVC** e **API da Web** no mesmo projeto</span><span class="sxs-lookup"><span data-stu-id="22517-116">Use different **ASP.NET** frameworks like **MVC** and **Web API** in the same project</span></span>
- <span data-ttu-id="22517-117">Identificar os principais componentes de um **ASP.NET** aplicativo</span><span class="sxs-lookup"><span data-stu-id="22517-117">Identify the main components of an **ASP.NET** application</span></span>
- <span data-ttu-id="22517-118">Aproveite as **Scaffolding do ASP.NET** framework para criar automaticamente os controladores e exibições para executar operações CRUD com base em suas classes de modelo</span><span class="sxs-lookup"><span data-stu-id="22517-118">Take advantage of the **ASP.NET Scaffolding** framework to automatically create Controllers and Views to perform CRUD operations based on your model classes</span></span>
- <span data-ttu-id="22517-119">Expor o mesmo conjunto de informações em formatos de máquina - e legível por humanos usando a ferramenta certa para cada trabalho</span><span class="sxs-lookup"><span data-stu-id="22517-119">Expose the same set of information in machine- and human-readable formats using the right tool for each job</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="22517-120">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="22517-120">Prerequisites</span></span>

<span data-ttu-id="22517-121">O exemplo a seguir é necessário para concluir este laboratório prático:</span><span class="sxs-lookup"><span data-stu-id="22517-121">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="22517-122">[O Visual Studio Express 2013 para Web](https://www.microsoft.com/visualstudio/) ou maior</span><span class="sxs-lookup"><span data-stu-id="22517-122">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="22517-123">Visual Studio 2013 Atualização 1</span><span class="sxs-lookup"><span data-stu-id="22517-123">Visual Studio 2013 Update 1</span></span>](https://go.microsoft.com/fwlink/?LinkId=301714)

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="22517-124">Configuração</span><span class="sxs-lookup"><span data-stu-id="22517-124">Setup</span></span>

<span data-ttu-id="22517-125">Para executar os exercícios neste laboratório prático, você precisará configurar seu ambiente pela primeira vez.</span><span class="sxs-lookup"><span data-stu-id="22517-125">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="22517-126">Abra o Windows Explorer e navegue para o laboratório **origem** pasta.</span><span class="sxs-lookup"><span data-stu-id="22517-126">Open Windows Explorer and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="22517-127">Clique duas vezes em **Setup. cmd** e selecione **executar como administrador** para iniciar o processo de instalação que irá configurar seu ambiente e instalem os trechos de código do Visual Studio para este laboratório.</span><span class="sxs-lookup"><span data-stu-id="22517-127">Right-click on **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="22517-128">Se a caixa de diálogo controle de conta de usuário for mostrada, confirme a ação para continuar.</span><span class="sxs-lookup"><span data-stu-id="22517-128">If the User Account Control dialog box is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="22517-129">Verifique se que você tiver marcado todas as dependências para este laboratório antes de executar a instalação.</span><span class="sxs-lookup"><span data-stu-id="22517-129">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="22517-130">Usando os trechos de código</span><span class="sxs-lookup"><span data-stu-id="22517-130">Using the Code Snippets</span></span>

<span data-ttu-id="22517-131">Em todo o documento de laboratório, você será instruído a inserir blocos de código.</span><span class="sxs-lookup"><span data-stu-id="22517-131">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="22517-132">Para sua conveniência, a maioria desse código é fornecido como o Visual Studio trechos de código que pode ser acessada de dentro do Visual Studio 2013 para evitar ter que adicioná-lo manualmente.</span><span class="sxs-lookup"><span data-stu-id="22517-132">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="22517-133">Cada exercício é acompanhado por uma solução inicial localizada na **começar** pasta do exercício que permite que você siga cada exercício independentemente dos outros.</span><span class="sxs-lookup"><span data-stu-id="22517-133">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="22517-134">Esteja ciente de que os trechos de código são adicionados durante um exercício estão ausentes desses iniciando soluções e podem não funcionar até concluir o exercício.</span><span class="sxs-lookup"><span data-stu-id="22517-134">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="22517-135">Dentro do código-fonte para um exercício, você também encontrará uma **final** pasta que contém uma solução do Visual Studio com o código que é o resultado de concluir as etapas no exercício correspondente.</span><span class="sxs-lookup"><span data-stu-id="22517-135">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="22517-136">Você pode usar essas soluções como uma diretriz se você precisar de ajuda adicional ao trabalhar com este laboratório prático.</span><span class="sxs-lookup"><span data-stu-id="22517-136">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


* * *

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="22517-137">Exercícios</span><span class="sxs-lookup"><span data-stu-id="22517-137">Exercises</span></span>

<span data-ttu-id="22517-138">Este laboratório prático inclui os seguintes exercícios:</span><span class="sxs-lookup"><span data-stu-id="22517-138">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="22517-139">Criar um novo projeto de formulários da Web</span><span class="sxs-lookup"><span data-stu-id="22517-139">Creating a New Web Forms Project</span></span>](#Exercise1)
2. [<span data-ttu-id="22517-140">Criando um controlador MVC usando Scaffolding</span><span class="sxs-lookup"><span data-stu-id="22517-140">Creating an MVC Controller Using Scaffolding</span></span>](#Exercise2)
3. [<span data-ttu-id="22517-141">Criando um controlador de API da Web usando o Scaffolding</span><span class="sxs-lookup"><span data-stu-id="22517-141">Creating a Web API Controller Using Scaffolding</span></span>](#Exercise3)

<span data-ttu-id="22517-142">Tempo estimado para concluir este laboratório: **60 minutos**</span><span class="sxs-lookup"><span data-stu-id="22517-142">Estimated time to complete this lab: **60 minutes**</span></span>

> [!NOTE]
> <span data-ttu-id="22517-143">Quando você inicia o Visual Studio pela primeira vez, você deve selecionar uma das coleções de configurações predefinidas.</span><span class="sxs-lookup"><span data-stu-id="22517-143">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="22517-144">Cada coleção predefinida foi projetada para corresponder a um estilo de desenvolvimento específico e determina o comportamento do editor, layouts de janela, trechos de código IntelliSense e opções da caixa de diálogo.</span><span class="sxs-lookup"><span data-stu-id="22517-144">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="22517-145">Os procedimentos neste laboratório descrevem as ações necessárias para realizar uma determinada tarefa no Visual Studio ao usar o **configurações gerais de desenvolvimento** coleção.</span><span class="sxs-lookup"><span data-stu-id="22517-145">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="22517-146">Se você escolher uma coleção de configurações diferentes para seu ambiente de desenvolvimento, pode haver diferenças nas etapas que você deve levar em conta.</span><span class="sxs-lookup"><span data-stu-id="22517-146">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-new-web-forms-project"></a><span data-ttu-id="22517-147">Exercício 1: Criar um novo projeto de formulários da Web</span><span class="sxs-lookup"><span data-stu-id="22517-147">Exercise 1: Creating a New Web Forms Project</span></span>

<span data-ttu-id="22517-148">Neste exercício, você criará um novo site de Web Forms no Visual Studio 2013 usando o **One ASP.NET** unificada de experiência de projeto, o que permitirá que você integre facilmente os componentes do Web Forms, MVC e API da Web no mesmo aplicativo.</span><span class="sxs-lookup"><span data-stu-id="22517-148">In this exercise you will create a new Web Forms site in Visual Studio 2013 using the **One ASP.NET** unified project experience, which will allow you to easily integrate Web Forms, MVC and Web API components in the same application.</span></span> <span data-ttu-id="22517-149">Você irá explorar a solução gerada e identificar suas partes, e por fim, você verá o site da Web em ação.</span><span class="sxs-lookup"><span data-stu-id="22517-149">You will then explore the generated solution and identify its parts, and finally you will see the Web site in action.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-a-new-site-using-the-one-aspnet-experience"></a><span data-ttu-id="22517-150">Tarefa 1 – criar um novo Site usando uma experiência de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="22517-150">Task 1 – Creating a New Site Using the One ASP.NET Experience</span></span>

<span data-ttu-id="22517-151">Nesta tarefa, você começará criando um novo site no Visual Studio com base nas **One ASP.NET** tipo de projeto.</span><span class="sxs-lookup"><span data-stu-id="22517-151">In this task you will start creating a new Web site in Visual Studio based on the **One ASP.NET** project type.</span></span> <span data-ttu-id="22517-152">**One ASP.NET** unifica todas as tecnologias do ASP.NET e lhe dá a opção de misturar e combiná-los conforme desejado.</span><span class="sxs-lookup"><span data-stu-id="22517-152">**One ASP.NET** unifies all ASP.NET technologies and gives you the option to mix and match them as desired.</span></span> <span data-ttu-id="22517-153">Você, em seguida, reconhecerá os diferentes componentes do Web Forms, MVC e API da Web que vivem lado a lado dentro de seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="22517-153">You will then recognize the different components of Web Forms, MVC and Web API that live side by side within your application.</span></span>

1. <span data-ttu-id="22517-154">Abra **Visual Studio Express 2013 para Web** e selecione **arquivo | Novo projeto...**  para iniciar uma nova solução.</span><span class="sxs-lookup"><span data-stu-id="22517-154">Open **Visual Studio Express 2013 for Web** and select **File | New Project...** to start a new solution.</span></span>

    ![Criando um novo projeto](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image1.png)

    <span data-ttu-id="22517-156">*Criar um novo projeto*</span><span class="sxs-lookup"><span data-stu-id="22517-156">*Creating a New Project*</span></span>
2. <span data-ttu-id="22517-157">No **novo projeto** caixa de diálogo, selecione **aplicativo Web ASP.NET** sob o **Visual c# | Web** guia e verifique se **.NET Framework 4.5** está selecionado.</span><span class="sxs-lookup"><span data-stu-id="22517-157">In the **New Project** dialog box, select **ASP.NET Web Application** under the **Visual C# | Web** tab, and make sure **.NET Framework 4.5** is selected.</span></span> <span data-ttu-id="22517-158">Nomeie o projeto *MyHybridSite*, escolha um **local** e clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="22517-158">Name the project *MyHybridSite*, choose a **Location** and click **OK**.</span></span>

    ![Novo projeto de aplicativo Web ASP.NET](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image2.png)

    <span data-ttu-id="22517-160">*Criar um novo projeto de aplicativo Web ASP.NET*</span><span class="sxs-lookup"><span data-stu-id="22517-160">*Creating a new ASP.NET Web Application project*</span></span>
3. <span data-ttu-id="22517-161">No **novo projeto ASP.NET** caixa de diálogo, selecione o **Web Forms** modelo e selecione o **MVC** e **API da Web** opções.</span><span class="sxs-lookup"><span data-stu-id="22517-161">In the **New ASP.NET Project** dialog box, select the **Web Forms** template and select the **MVC** and **Web API** options.</span></span> <span data-ttu-id="22517-162">Além disso, certifique-se de que o **autenticação** opção for definida como **contas de usuário individuais**.</span><span class="sxs-lookup"><span data-stu-id="22517-162">Also, make sure that the **Authentication** option is set to **Individual User Accounts**.</span></span> <span data-ttu-id="22517-163">Clique em **OK** para continuar.</span><span class="sxs-lookup"><span data-stu-id="22517-163">Click **OK** to continue.</span></span>

    ![Criar um novo projeto com o modelo de Web Forms, incluindo componentes de API da Web e o MVC](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image3.png)

    <span data-ttu-id="22517-165">*Criar um novo projeto com o modelo de Web Forms, incluindo componentes de API da Web e o MVC*</span><span class="sxs-lookup"><span data-stu-id="22517-165">*Creating a new project with the Web Forms template, including Web API and MVC components*</span></span>
4. <span data-ttu-id="22517-166">Agora você pode explorar a estrutura da solução gerada.</span><span class="sxs-lookup"><span data-stu-id="22517-166">You can now explore the structure of the generated solution.</span></span>

    ![Explorar a solução gerada](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image4.png)

    <span data-ttu-id="22517-168">*Explorar a solução gerada*</span><span class="sxs-lookup"><span data-stu-id="22517-168">*Exploring the generated solution*</span></span>

    1. <span data-ttu-id="22517-169">**Conta:** essa pasta contém as páginas de formulário da Web para se registrar, faça logon no e gerenciar contas de usuário do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="22517-169">**Account:** This folder contains the Web Form pages to register, log in to and manage the application's user accounts.</span></span> <span data-ttu-id="22517-170">Essa pasta é adicionada quando o **contas de usuário individuais** opção de autenticação é selecionada durante a configuração do modelo de projeto Web Forms.</span><span class="sxs-lookup"><span data-stu-id="22517-170">This folder is added when the **Individual User Accounts** authentication option is selected during the configuration of the Web Forms project template.</span></span>
    2. <span data-ttu-id="22517-171">**Modelos:** essa pasta contém as classes que representam os dados do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="22517-171">**Models:** This folder will contain the classes that represent your application data.</span></span>
    3. <span data-ttu-id="22517-172">**Controladores** e **modos de exibição**: essas pastas são necessárias para o **ASP.NET MVC** e **API Web ASP.NET** componentes.</span><span class="sxs-lookup"><span data-stu-id="22517-172">**Controllers** and **Views**: These folders are required for the **ASP.NET MVC** and **ASP.NET Web API** components.</span></span> <span data-ttu-id="22517-173">Você irá explorar as tecnologias MVC e API da Web nos próximos exercícios.</span><span class="sxs-lookup"><span data-stu-id="22517-173">You will explore the MVC and Web API technologies in the next exercises.</span></span>
    4. <span data-ttu-id="22517-174">O **default. aspx**, **Contact** e **About** arquivos são predefinidas páginas de formulário da Web que você pode usar como ponto de partida para criar as páginas específicas para seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="22517-174">The **Default.aspx**, **Contact.aspx** and **About.aspx** files are pre-defined Web Form pages that you can use as starting points to build the pages specific to your application.</span></span> <span data-ttu-id="22517-175">A lógica de programação desses arquivos reside em um arquivo separado, conhecido como o &quot;de lógica&quot; arquivo, que tem um &quot;. aspx&quot; ou &quot;. aspx.cs&quot; extensão (dependendo do idioma usado).</span><span class="sxs-lookup"><span data-stu-id="22517-175">The programming logic of those files resides in a separate file referred to as the &quot;code-behind&quot; file, which has an &quot;.aspx.vb&quot; or &quot;.aspx.cs&quot; extension (depending on the language used).</span></span> <span data-ttu-id="22517-176">A lógica de lógica é executado no servidor e dinamicamente produz a saída HTML para a sua página.</span><span class="sxs-lookup"><span data-stu-id="22517-176">The code-behind logic runs on the server and dynamically produces the HTML output for your page.</span></span>
    5. <span data-ttu-id="22517-177">O **Master** e **Site.Mobile.Master** páginas definem a aparência e o comportamento padrão de todas as páginas no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="22517-177">The **Site.Master** and **Site.Mobile.Master** pages define the look and feel and the standard behavior of all the pages in the application.</span></span>
5. <span data-ttu-id="22517-178">Clique duas vezes o **default. aspx** arquivo para explorar o conteúdo da página.</span><span class="sxs-lookup"><span data-stu-id="22517-178">Double-click the **Default.aspx** file to explore the content of the page.</span></span>

    ![Explorando a página Default. aspx](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image5.png)

    <span data-ttu-id="22517-180">*Explorando a página Default. aspx*</span><span class="sxs-lookup"><span data-stu-id="22517-180">*Exploring the Default.aspx page*</span></span>

    > [!NOTE]
    > <span data-ttu-id="22517-181">O **página** diretiva na parte superior do arquivo define os atributos de página de Web Forms.</span><span class="sxs-lookup"><span data-stu-id="22517-181">The **Page** directive at the top of the file defines the attributes of the Web Forms page.</span></span> <span data-ttu-id="22517-182">Por exemplo, o **MasterPageFile** atributo especifica o caminho para o mestre de página – nesse caso, o *Master* página – e o **Inherits** atributo define o classe code-behind da página herdar.</span><span class="sxs-lookup"><span data-stu-id="22517-182">For example, the **MasterPageFile** attribute specifies the path to the master page -in this case, the *Site.Master* page- and the **Inherits** attribute defines the code-behind class for the page to inherit.</span></span> <span data-ttu-id="22517-183">Essa classe está localizada no arquivo de determinado pelo **code-behind** atributo.</span><span class="sxs-lookup"><span data-stu-id="22517-183">This class is located in the file determined by the **CodeBehind** attribute.</span></span>
    > 
    > <span data-ttu-id="22517-184">O **asp: Content** controle mantém o conteúdo real da página (texto, marcação e controles) e é mapeado para um **asp: ContentPlaceHolder** controle na página mestra.</span><span class="sxs-lookup"><span data-stu-id="22517-184">The **asp:Content** control holds the actual content of the page (text, markup and controls) and is mapped to a **asp:ContentPlaceHolder** control on the master page.</span></span> <span data-ttu-id="22517-185">Nesse caso, o conteúdo da página será renderizado dentro de *MainContent* controle definido na *Master* página.</span><span class="sxs-lookup"><span data-stu-id="22517-185">In this case, the page content will be rendered inside the *MainContent* control defined in the *Site.Master* page.</span></span>
6. <span data-ttu-id="22517-186">Expanda o **App\_começar** pasta e observe que o **WebApiConfig.cs** arquivo.</span><span class="sxs-lookup"><span data-stu-id="22517-186">Expand the **App\_Start** folder and notice the **WebApiConfig.cs** file.</span></span> <span data-ttu-id="22517-187">Visual Studio incluído esse arquivo na solução gerada porque você incluiu a API da Web ao configurar seu projeto com o modelo do One ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="22517-187">Visual Studio included that file in the generated solution because you included Web API when configuring your project with the One ASP.NET template.</span></span>
7. <span data-ttu-id="22517-188">Abra o **WebApiConfig.cs** arquivo.</span><span class="sxs-lookup"><span data-stu-id="22517-188">Open the **WebApiConfig.cs** file.</span></span> <span data-ttu-id="22517-189">No *WebApiConfig* classe, você encontrará a configuração associada com a API da Web, que mapeia HTTP roteia para **controladores da API Web**.</span><span class="sxs-lookup"><span data-stu-id="22517-189">In the *WebApiConfig* class you will find the configuration associated with Web API, which maps HTTP routes to **Web API controllers**.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample1.cs)]
8. <span data-ttu-id="22517-190">Abra o **RouteConfig.cs** arquivo.</span><span class="sxs-lookup"><span data-stu-id="22517-190">Open the **RouteConfig.cs** file.</span></span> <span data-ttu-id="22517-191">Dentro de *RegisterRoutes* método, você encontrará a configuração associada ao MVC, que mapeia as rotas HTTP para **controladores MVC**.</span><span class="sxs-lookup"><span data-stu-id="22517-191">Inside the *RegisterRoutes* method you will find the configuration associated with MVC, which maps HTTP routes to **MVC controllers**.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample2.cs)]

<a id="Ex1Task2"></a>
#### <a name="task-2--running-the-solution"></a><span data-ttu-id="22517-192">Tarefa 2 – executar a solução</span><span class="sxs-lookup"><span data-stu-id="22517-192">Task 2 – Running the Solution</span></span>

<span data-ttu-id="22517-193">Nesta tarefa você executar a solução gerada, explorar o aplicativo e alguns de seus recursos, como a reconfiguração de URL e autenticação interna.</span><span class="sxs-lookup"><span data-stu-id="22517-193">In this task you will run the generated solution, explore the app and some of its features, like URL rewriting and built-in authentication.</span></span>

1. <span data-ttu-id="22517-194">Para executar a solução, pressione **F5** ou clique no **iniciar** botão localizado na barra de ferramentas.</span><span class="sxs-lookup"><span data-stu-id="22517-194">To run the solution, press **F5** or click the **Start** button located on the toolbar.</span></span> <span data-ttu-id="22517-195">Home page do aplicativo deve abrir no navegador.</span><span class="sxs-lookup"><span data-stu-id="22517-195">The application home page should open in the browser.</span></span>

    ![Executar a solução](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image6.png)
2. <span data-ttu-id="22517-197">Verifique se que as páginas de formulários da Web estão sendo invocadas.</span><span class="sxs-lookup"><span data-stu-id="22517-197">Verify that the Web Forms pages are being invoked.</span></span> <span data-ttu-id="22517-198">Para fazer isso, acrescente **/contact.aspx** para a URL na barra de endereços e pressione **Enter**.</span><span class="sxs-lookup"><span data-stu-id="22517-198">To do this, append **/contact.aspx** to the URL in the address bar and press **Enter**.</span></span>

    ![URLs amigáveis](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image7.png)

    <span data-ttu-id="22517-200">*URLs amigáveis*</span><span class="sxs-lookup"><span data-stu-id="22517-200">*Friendly URLs*</span></span>

    > [!NOTE]
    > <span data-ttu-id="22517-201">Como você pode ver, a URL é alterada para **/entre em contato com**.</span><span class="sxs-lookup"><span data-stu-id="22517-201">As you can see, the URL changes to **/contact**.</span></span> <span data-ttu-id="22517-202">A partir **ASP.NET 4**, os recursos de roteamento de URL foram adicionados a formulários da Web, para que você possa escrever, como URLs *[ http://www.mysite.com/products/software ](http://www.mysite.com/products/software)* em vez de  *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*.</span><span class="sxs-lookup"><span data-stu-id="22517-202">Starting from **ASP.NET 4**, URL routing capabilities were added to Web Forms, so you can write URLs like *[http://www.mysite.com/products/software](http://www.mysite.com/products/software)* instead of *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*.</span></span> <span data-ttu-id="22517-203">Para obter mais informações, consulte [roteamento de URL](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).</span><span class="sxs-lookup"><span data-stu-id="22517-203">For more information refer to [URL Routing](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).</span></span>
3. <span data-ttu-id="22517-204">Agora, você explorará o fluxo de autenticação integrado ao aplicativo.</span><span class="sxs-lookup"><span data-stu-id="22517-204">You will now explore the authentication flow integrated into the application.</span></span> <span data-ttu-id="22517-205">Para fazer isso, clique em **registrar** no canto superior direito da página.</span><span class="sxs-lookup"><span data-stu-id="22517-205">To do this, click **Register** in the upper-right corner of the page.</span></span>

    ![Registrar um novo usuário](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image8.png)

    <span data-ttu-id="22517-207">*Registrar um novo usuário*</span><span class="sxs-lookup"><span data-stu-id="22517-207">*Registering a new user*</span></span>
4. <span data-ttu-id="22517-208">No **registre** , insira um **nome de usuário** e **senha**e, em seguida, clique em **registrar**.</span><span class="sxs-lookup"><span data-stu-id="22517-208">In the **Register** page, enter a **User name** and **Password**, and then click **Register**.</span></span>

    ![Página de registro](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image9.png)

    <span data-ttu-id="22517-210">*Página de registro*</span><span class="sxs-lookup"><span data-stu-id="22517-210">*Register page*</span></span>
5. <span data-ttu-id="22517-211">O aplicativo registra a nova conta e o usuário é autenticado.</span><span class="sxs-lookup"><span data-stu-id="22517-211">The application registers the new account, and the user is authenticated.</span></span>

    ![Usuário autenticado](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image10.png)

    <span data-ttu-id="22517-213">*Usuário autenticado*</span><span class="sxs-lookup"><span data-stu-id="22517-213">*User authenticated*</span></span>
6. <span data-ttu-id="22517-214">Volte para o Visual Studio e pressione **SHIFT + F5** para parar a depuração.</span><span class="sxs-lookup"><span data-stu-id="22517-214">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-creating-an-mvc-controller-using-scaffolding"></a><span data-ttu-id="22517-215">Exercício 2: Criando um controlador MVC usando Scaffolding</span><span class="sxs-lookup"><span data-stu-id="22517-215">Exercise 2: Creating an MVC Controller Using Scaffolding</span></span>

<span data-ttu-id="22517-216">Neste exercício você aproveitará o framework de Scaffolding do ASP.NET fornecido pelo Visual Studio para criar um controlador ASP.NET MVC 5 com ações e exibições do Razor para realizar operações CRUD, sem escrever uma única linha de código.</span><span class="sxs-lookup"><span data-stu-id="22517-216">In this exercise you will take advantage of the ASP.NET Scaffolding framework provided by Visual Studio to create an ASP.NET MVC 5 controller with actions and Razor views to perform CRUD operations, without writing a single line of code.</span></span> <span data-ttu-id="22517-217">O processo de scaffolding usará o Entity Framework Code First para gerar o contexto de dados e o esquema de banco de dados no banco de dados SQL.</span><span class="sxs-lookup"><span data-stu-id="22517-217">The scaffolding process will use Entity Framework Code First to generate the data context and the database schema in the SQL database.</span></span>

<span data-ttu-id="22517-218">**Sobre o Entity Framework Code First**</span><span class="sxs-lookup"><span data-stu-id="22517-218">**About Entity Framework Code First**</span></span>

<span data-ttu-id="22517-219">Entity Framework (EF) é um mapeador relacional de objeto (ORM) que permite que você crie aplicativos de acesso de dados por meio da programação com um modelo de aplicativo conceitual em vez de programação diretamente usando um esquema de armazenamento relacional.</span><span class="sxs-lookup"><span data-stu-id="22517-219">Entity Framework (EF) is an object-relational mapper (ORM) that enables you to create data access applications by programming with a conceptual application model instead of programming directly using a relational storage schema.</span></span>

<span data-ttu-id="22517-220">O fluxo de trabalho de modelagem do Entity Framework Code First permite que você use suas próprias classes de domínio para representar o modelo de EF depende ao executar a consulta, funções de controle de alterações e atualização.</span><span class="sxs-lookup"><span data-stu-id="22517-220">The Entity Framework Code First modeling workflow allows you to use your own domain classes to represent the model that EF relies on when performing querying, change-tracking and updating functions.</span></span> <span data-ttu-id="22517-221">Usando o fluxo de trabalho de desenvolvimento Code First, você precisa iniciar seu aplicativo criando um banco de dados ou especificando um esquema.</span><span class="sxs-lookup"><span data-stu-id="22517-221">Using the Code First development workflow, you do not need to begin your application by creating a database or specifying a schema.</span></span> <span data-ttu-id="22517-222">Em vez disso, você pode escrever classes padrão do .NET que definem os objetos de modelo de domínio mais apropriados para seu aplicativo, e o Entity Framework criará o banco de dados para você.</span><span class="sxs-lookup"><span data-stu-id="22517-222">Instead, you can write standard .NET classes that define the most appropriate domain model objects for your application, and Entity Framework will create the database for you.</span></span>

> [!NOTE]
> <span data-ttu-id="22517-223">Você pode aprender mais sobre o Entity Framework [aqui](../../../entity-framework.md).</span><span class="sxs-lookup"><span data-stu-id="22517-223">You can learn more about Entity Framework [here](../../../entity-framework.md).</span></span>


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-new-model"></a><span data-ttu-id="22517-224">Tarefa 1 – criar um novo modelo</span><span class="sxs-lookup"><span data-stu-id="22517-224">Task 1 – Creating a New Model</span></span>

<span data-ttu-id="22517-225">Agora você definirá uma **pessoa** classe, que será o modelo usado pelo processo de scaffolding para criar o controlador MVC e os modos de exibição.</span><span class="sxs-lookup"><span data-stu-id="22517-225">You will now define a **Person** class, which will be the model used by the scaffolding process to create the MVC controller and the views.</span></span> <span data-ttu-id="22517-226">Você começará criando um **pessoa** classe de modelo e as operações CRUD no controlador serão criada automaticamente usando os recursos de scaffolding.</span><span class="sxs-lookup"><span data-stu-id="22517-226">You will start by creating a **Person** model class, and the CRUD operations in the controller will be automatically created using scaffolding features.</span></span>

1. <span data-ttu-id="22517-227">Abra **Visual Studio Express 2013 para Web** e o **MyHybridSite.sln** solução localizada no **origem/o Ex2-MvcScaffolding/início** pasta.</span><span class="sxs-lookup"><span data-stu-id="22517-227">Open **Visual Studio Express 2013 for Web** and the **MyHybridSite.sln** solution located in the **Source/Ex2-MvcScaffolding/Begin** folder.</span></span> <span data-ttu-id="22517-228">Como alternativa, você pode continuar com a solução que você obteve no exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="22517-228">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>
2. <span data-ttu-id="22517-229">No **Gerenciador de soluções**, clique com botão direito a **modelos** pasta da **MyHybridSite** do projeto e selecione **adicionar | Classe...** .</span><span class="sxs-lookup"><span data-stu-id="22517-229">In **Solution Explorer**, right-click the **Models** folder of the **MyHybridSite** project and select **Add | Class...**.</span></span>

    ![Adicionando a classe de modelo da pessoa](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image11.png)

    <span data-ttu-id="22517-231">*Adicionando a classe de modelo da pessoa*</span><span class="sxs-lookup"><span data-stu-id="22517-231">*Adding the Person model class*</span></span>
3. <span data-ttu-id="22517-232">No **Adicionar Novo Item** caixa de diálogo, nomeie o arquivo *Person.cs* e clique em **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="22517-232">In the **Add New Item** dialog box, name the file *Person.cs* and click **Add**.</span></span>

    ![Criando a classe de modelo da pessoa](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image12.png)

    <span data-ttu-id="22517-234">*Criando a classe de modelo da pessoa*</span><span class="sxs-lookup"><span data-stu-id="22517-234">*Creating the Person model class*</span></span>
4. <span data-ttu-id="22517-235">Substitua o conteúdo a **Person.cs** arquivo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="22517-235">Replace the content of the **Person.cs** file with the following code.</span></span> <span data-ttu-id="22517-236">Pressione **CTRL + S** para salvar as alterações.</span><span class="sxs-lookup"><span data-stu-id="22517-236">Press **CTRL + S** to save the changes.</span></span>

    <span data-ttu-id="22517-237">(Código de trecho de código – *PersonClass BringingTogetherOneAspNet - o Ex2 -*)</span><span class="sxs-lookup"><span data-stu-id="22517-237">(Code Snippet - *BringingTogetherOneAspNet - Ex2 - PersonClass*)</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample3.cs)]
5. <span data-ttu-id="22517-238">No **Gerenciador de soluções**, com o botão direito do **MyHybridSite** do projeto e selecione **Build**, ou pressione **CTRL + SHIFT + B** para compilar o projeto.</span><span class="sxs-lookup"><span data-stu-id="22517-238">In **Solution Explorer**, right-click the **MyHybridSite** project and select **Build**, or press **CTRL + SHIFT + B** to build the project.</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-an-mvc-controller"></a><span data-ttu-id="22517-239">Tarefa 2 – criar um controlador MVC</span><span class="sxs-lookup"><span data-stu-id="22517-239">Task 2 – Creating an MVC Controller</span></span>

<span data-ttu-id="22517-240">Agora que o **pessoa** modelo é criado, você usará o scaffolding do ASP.NET MVC com o Entity Framework para criar as ações do controlador CRUD e exibições para **pessoa**.</span><span class="sxs-lookup"><span data-stu-id="22517-240">Now that the **Person** model is created, you will use ASP.NET MVC scaffolding with Entity Framework to create the CRUD controller actions and views for **Person**.</span></span>

1. <span data-ttu-id="22517-241">No **Gerenciador de soluções**, clique com botão direito o **controladores** pasta do **MyHybridSite** do projeto e selecione **adicionar | Novo Item com Scaffold...** .</span><span class="sxs-lookup"><span data-stu-id="22517-241">In **Solution Explorer**, right-click the **Controllers** folder of the **MyHybridSite** project and select **Add | New Scaffolded Item...**.</span></span>

    ![Criando um novo controlador gerado por scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image13.png)

    <span data-ttu-id="22517-243">*Criando um novo controlador geradas por scaffolding*</span><span class="sxs-lookup"><span data-stu-id="22517-243">*Creating a new Scaffolded Controller*</span></span>
2. <span data-ttu-id="22517-244">No **adicionar Scaffold** caixa de diálogo, selecione **controlador MVC 5 com modos de exibição usando o Entity Framework** e, em seguida, clique em **adicionar.**</span><span class="sxs-lookup"><span data-stu-id="22517-244">In the **Add Scaffold** dialog box, select **MVC 5 Controller with views, using Entity Framework** and then click **Add.**</span></span>

    ![Selecionar controlador MVC 5 com modos de exibição e o Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image14.png)

    <span data-ttu-id="22517-246">*Selecionar controlador MVC 5 com modos de exibição e o Entity Framework*</span><span class="sxs-lookup"><span data-stu-id="22517-246">*Selecting MVC 5 Controller with views and Entity Framework*</span></span>
3. <span data-ttu-id="22517-247">Definir *MvcPersonController* como o **nome do controlador**, selecione o **usar ações do controlador assíncrono** opção e selecione **pessoa (MyHybridSite.Models)**  como o **classe de modelo**.</span><span class="sxs-lookup"><span data-stu-id="22517-247">Set *MvcPersonController* as the **Controller name**, select the **Use async controller actions** option and select **Person (MyHybridSite.Models)** as the **Model class**.</span></span>

    ![Adicionando um controlador MVC com scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image15.png)

    <span data-ttu-id="22517-249">*Adicionando um controlador MVC com scaffolding*</span><span class="sxs-lookup"><span data-stu-id="22517-249">*Adding an MVC controller with scaffolding*</span></span>
4. <span data-ttu-id="22517-250">Sob **classe de contexto de dados**, clique em **novo contexto de dados...** .</span><span class="sxs-lookup"><span data-stu-id="22517-250">Under **Data context class**, click **New data context...**.</span></span>

    ![Criando um novo contexto de dados](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image16.png)

    <span data-ttu-id="22517-252">*Criando um novo contexto de dados*</span><span class="sxs-lookup"><span data-stu-id="22517-252">*Creating a new data context*</span></span>
5. <span data-ttu-id="22517-253">No **novo contexto de dados** caixa de diálogo, o nome do novo contexto de dados *PersonContext* e clique em **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="22517-253">In the **New Data Context** dialog box, name the new data context *PersonContext* and click **Add**.</span></span>

    ![Criar o novo PersonContext](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image17.png)

    <span data-ttu-id="22517-255">*Criar o novo tipo de PersonContext*</span><span class="sxs-lookup"><span data-stu-id="22517-255">*Creating the new PersonContext type*</span></span>
6. <span data-ttu-id="22517-256">Clique em **Add** para criar o novo controlador para **pessoa** com scaffolding.</span><span class="sxs-lookup"><span data-stu-id="22517-256">Click **Add** to create the new controller for **Person** with scaffolding.</span></span> <span data-ttu-id="22517-257">Visual Studio, em seguida, irá gerar as ações do controlador, o contexto de dados de pessoa e as exibições do Razor.</span><span class="sxs-lookup"><span data-stu-id="22517-257">Visual Studio will then generate the controller actions, the Person data context and the Razor views.</span></span>

    ![Depois de criar o controlador MVC com scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image18.png)

    <span data-ttu-id="22517-259">*Depois de criar o controlador MVC com scaffolding*</span><span class="sxs-lookup"><span data-stu-id="22517-259">*After creating the MVC controller with scaffolding*</span></span>
7. <span data-ttu-id="22517-260">Abra o **MvcPersonController.cs** do arquivo na **controladores** pasta.</span><span class="sxs-lookup"><span data-stu-id="22517-260">Open the **MvcPersonController.cs** file in the **Controllers** folder.</span></span> <span data-ttu-id="22517-261">Observe que os métodos de ação CRUD foram gerados automaticamente.</span><span class="sxs-lookup"><span data-stu-id="22517-261">Notice that the CRUD action methods have been generated automatically.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample4.cs)]

    > [!NOTE]
    > <span data-ttu-id="22517-262">Selecionando o **usar ações do controlador assíncrono** opções de caixa de seleção no scaffolding nas etapas anteriores, o Visual Studio gera os métodos de ação assíncrono para todas as ações que envolvem o acesso ao contexto de dados de pessoa.</span><span class="sxs-lookup"><span data-stu-id="22517-262">By selecting the **Use async controller actions** check box from the scaffolding options in the previous steps, Visual Studio generates asynchronous action methods for all actions that involve access to the Person data context.</span></span> <span data-ttu-id="22517-263">É recomendável que você usar os métodos de ação assíncrono de longa execução, não-CPU associado a solicitações para evitar o bloqueio do servidor Web de executar o trabalho enquanto a solicitação está sendo processada.</span><span class="sxs-lookup"><span data-stu-id="22517-263">It is recommended that you use asynchronous action methods for long-running, non-CPU bound requests to avoid blocking the Web server from performing work while the request is being processed.</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3--running-the-solution"></a><span data-ttu-id="22517-264">Tarefa 3 – executar a solução</span><span class="sxs-lookup"><span data-stu-id="22517-264">Task 3 – Running the Solution</span></span>

<span data-ttu-id="22517-265">Nesta tarefa, você executará a solução novamente para verificar se os modos de exibição para **pessoa** estão funcionando conforme o esperado.</span><span class="sxs-lookup"><span data-stu-id="22517-265">In this task, you will run the solution again to verify that the views for **Person** are working as expected.</span></span> <span data-ttu-id="22517-266">Você adicionará uma nova pessoa para verificar que ele é salvo com êxito no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="22517-266">You will add a new person to verify that it is successfully saved to the database.</span></span>

1. <span data-ttu-id="22517-267">Pressione **F5** para executar a solução.</span><span class="sxs-lookup"><span data-stu-id="22517-267">Press **F5** to run the solution.</span></span>
2. <span data-ttu-id="22517-268">Navegue até **/MvcPerson**.</span><span class="sxs-lookup"><span data-stu-id="22517-268">Navigate to **/MvcPerson**.</span></span> <span data-ttu-id="22517-269">O modo de exibição gerados automaticamente que mostra a lista de pessoas deve aparecer.</span><span class="sxs-lookup"><span data-stu-id="22517-269">The scaffolded view that shows the list of people should appear.</span></span>
3. <span data-ttu-id="22517-270">Clique em **criar novo** para adicionar uma nova pessoa.</span><span class="sxs-lookup"><span data-stu-id="22517-270">Click **Create New** to add a new person.</span></span>

    ![Navegando até as exibições do MVC gerado por scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image19.png)

    <span data-ttu-id="22517-272">*Navegando até as exibições do MVC gerado por scaffolding*</span><span class="sxs-lookup"><span data-stu-id="22517-272">*Navigating to the scaffolded MVC views*</span></span>
4. <span data-ttu-id="22517-273">No **Create** exibir, forneça um **nome** e um **idade** para a pessoa e clique em **criar**.</span><span class="sxs-lookup"><span data-stu-id="22517-273">In the **Create** view, provide a **Name** and an **Age** for the person, and click **Create**.</span></span>

    ![Adicionando uma nova pessoa](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image20.png)

    <span data-ttu-id="22517-275">*Adicionando uma nova pessoa*</span><span class="sxs-lookup"><span data-stu-id="22517-275">*Adding a new person*</span></span>
5. <span data-ttu-id="22517-276">Nova pessoa é adicionada à lista.</span><span class="sxs-lookup"><span data-stu-id="22517-276">The new person is added to the list.</span></span> <span data-ttu-id="22517-277">Na lista de elementos, clique em **detalhes** para exibir o modo de exibição de detalhes da pessoa.</span><span class="sxs-lookup"><span data-stu-id="22517-277">In the element list, click **Details** to display the person's details view.</span></span> <span data-ttu-id="22517-278">Em seguida, nos **detalhes** exibir, clique em **voltar para a lista** para voltar à exibição de lista.</span><span class="sxs-lookup"><span data-stu-id="22517-278">Then, in the **Details** view, click **Back to List** to go back to the list view.</span></span>

    ![Modo de exibição de detalhes da pessoa](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image21.png)

    <span data-ttu-id="22517-280">*Modo de exibição de detalhes da pessoa*</span><span class="sxs-lookup"><span data-stu-id="22517-280">*Person's details view*</span></span>
6. <span data-ttu-id="22517-281">Clique o **excluir** link para excluir a pessoa.</span><span class="sxs-lookup"><span data-stu-id="22517-281">Click the **Delete** link to delete the person.</span></span> <span data-ttu-id="22517-282">No **exclua** exibir, clique em **excluir** para confirmar a operação.</span><span class="sxs-lookup"><span data-stu-id="22517-282">In the **Delete** view, click **Delete** to confirm the operation.</span></span>

    ![Exclusão de uma pessoa](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image22.png)

    <span data-ttu-id="22517-284">*Exclusão de uma pessoa*</span><span class="sxs-lookup"><span data-stu-id="22517-284">*Deleting a person*</span></span>
7. <span data-ttu-id="22517-285">Volte para o Visual Studio e pressione **SHIFT + F5** para parar a depuração.</span><span class="sxs-lookup"><span data-stu-id="22517-285">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>

<a id="Exercise3"></a>
### <a name="exercise-3-creating-a-web-api-controller-using-scaffolding"></a><span data-ttu-id="22517-286">Exercício 3: Criar um controlador da API da Web usando o Scaffolding</span><span class="sxs-lookup"><span data-stu-id="22517-286">Exercise 3: Creating a Web API Controller Using Scaffolding</span></span>

<span data-ttu-id="22517-287">A estrutura da API Web é parte da pilha do ASP.NET e projetado para facilitar a implementação dos serviços HTTP, geralmente enviando e recebendo dados JSON ou XML formatado por meio de uma API RESTful.</span><span class="sxs-lookup"><span data-stu-id="22517-287">The Web API framework is part of the ASP.NET Stack and designed to make implementing HTTP services easier, generally sending and receiving JSON- or XML-formatted data through a RESTful API.</span></span>

<span data-ttu-id="22517-288">Neste exercício, você usará Scaffolding do ASP.NET novamente para gerar um controlador de API da Web.</span><span class="sxs-lookup"><span data-stu-id="22517-288">In this exercise, you will use ASP.NET Scaffolding again to generate a Web API controller.</span></span> <span data-ttu-id="22517-289">Você usará o mesmo **pessoa** e **PersonContext** classes do exercício anterior para fornecer os mesmos dados person no formato JSON.</span><span class="sxs-lookup"><span data-stu-id="22517-289">You will use the same **Person** and **PersonContext** classes from the previous exercise to provide the same person data in JSON format.</span></span> <span data-ttu-id="22517-290">Você verá como é possível expor os mesmos recursos de maneiras diferentes dentro do mesmo aplicativo ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="22517-290">You will see how you can expose the same resources in different ways within the same ASP.NET application.</span></span>

<a id="Ex3Task1"></a>
#### <a name="task-1--creating-a-web-api-controller"></a><span data-ttu-id="22517-291">Tarefa 1 – criar um controlador da API da Web</span><span class="sxs-lookup"><span data-stu-id="22517-291">Task 1 – Creating a Web API Controller</span></span>

<span data-ttu-id="22517-292">Nesta tarefa, você criará uma nova **controlador da API Web** que irá expor os dados da pessoa em um formato consumível de máquina, como JSON.</span><span class="sxs-lookup"><span data-stu-id="22517-292">In this task you will create a new **Web API Controller** that will expose the person data in a machine-consumable format like JSON.</span></span>

1. <span data-ttu-id="22517-293">Se ainda não estiver aberto, abra **Visual Studio Express 2013 para Web** e abra o **MyHybridSite.sln** solução localizada no **origem/Ex3-WebAPI/início** pasta.</span><span class="sxs-lookup"><span data-stu-id="22517-293">If not already opened, open **Visual Studio Express 2013 for Web** and open the **MyHybridSite.sln** solution located in the **Source/Ex3-WebAPI/Begin** folder.</span></span> <span data-ttu-id="22517-294">Como alternativa, você pode continuar com a solução que você obteve no exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="22517-294">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>

    > [!NOTE]
    > <span data-ttu-id="22517-295">Se você iniciar com a solução de início do Exercício 3, pressione **CTRL + SHIFT + B** para compilar a solução.</span><span class="sxs-lookup"><span data-stu-id="22517-295">If you start with the Begin solution from Exercise 3, press **CTRL + SHIFT + B** to build the solution.</span></span>
2. <span data-ttu-id="22517-296">No **Gerenciador de soluções**, clique com botão direito o **controladores** pasta do **MyHybridSite** do projeto e selecione **adicionar | Novo Item com Scaffold...** .</span><span class="sxs-lookup"><span data-stu-id="22517-296">In **Solution Explorer**, right-click the **Controllers** folder of the **MyHybridSite** project and select **Add | New Scaffolded Item...**.</span></span>

    ![Criando um novo controlador gerado por scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image23.png)

    <span data-ttu-id="22517-298">*Criando um novo controlador gerado por scaffolding*</span><span class="sxs-lookup"><span data-stu-id="22517-298">*Creating a new scaffolded Controller*</span></span>
3. <span data-ttu-id="22517-299">No **adicionar Scaffold** caixa de diálogo, selecione **API da Web** no painel esquerdo, em seguida, **controlador Web API 2 com ações, usando o Entity Framework** no painel central e, em seguida, clique em  **Adicione.**</span><span class="sxs-lookup"><span data-stu-id="22517-299">In the **Add Scaffold** dialog box, select **Web API** in the left pane, then **Web API 2 Controller with actions, using Entity Framework** in the middle pane and then click **Add.**</span></span>

    <span data-ttu-id="22517-300">![Selecionar controlador Web API 2 com ações e o Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "selecionar controlador Web API 2 com ações e o Entity Framework")</span><span class="sxs-lookup"><span data-stu-id="22517-300">![Selecting Web API 2 Controller with actions and Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "Selecting Web API 2 Controller with actions and Entity Framework")</span></span>

    <span data-ttu-id="22517-301">*Selecionar controlador Web API 2 com ações e o Entity Framework*</span><span class="sxs-lookup"><span data-stu-id="22517-301">*Selecting Web API 2 Controller with actions and Entity Framework*</span></span>
4. <span data-ttu-id="22517-302">Definir *ApiPersonController* como o **nome do controlador**, selecione o **usar ações do controlador assíncrono** opção e selecione **pessoa (MyHybridSite.Models)**  e **PersonContext (MyHybridSite.Models)** como o **modelo** e **contexto de dados** classes, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="22517-302">Set *ApiPersonController* as the **Controller name**, select the **Use async controller actions** option and select **Person (MyHybridSite.Models)** and **PersonContext (MyHybridSite.Models)** as the **Model** and **Data context** classes respectively.</span></span> <span data-ttu-id="22517-303">Em seguida, clique em **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="22517-303">Then click **Add**.</span></span>

    <span data-ttu-id="22517-304">![Adicionando um controlador de API da Web com o scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "adicionando um controlador de API da Web com scaffolding")</span><span class="sxs-lookup"><span data-stu-id="22517-304">![Adding a Web API Controller with scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "Adding a Web API controller with scaffolding")</span></span>

    <span data-ttu-id="22517-305">*Adicionando um controlador de API da Web com scaffolding*</span><span class="sxs-lookup"><span data-stu-id="22517-305">*Adding a Web API controller with scaffolding*</span></span>
5. <span data-ttu-id="22517-306">Visual Studio, em seguida, irá gerar o **ApiPersonController** classe com as quatro ações CRUD para trabalhar com seus dados.</span><span class="sxs-lookup"><span data-stu-id="22517-306">Visual Studio will then generate the **ApiPersonController** class with the four CRUD actions to work with your data.</span></span>

    <span data-ttu-id="22517-307">![Depois de criar o controlador da API Web com scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "depois de criar o controlador da API Web com scaffolding")</span><span class="sxs-lookup"><span data-stu-id="22517-307">![After creating the Web API controller with scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "After creating the Web API controller with scaffolding")</span></span>

    <span data-ttu-id="22517-308">*Depois de criar o controlador da API Web com scaffolding*</span><span class="sxs-lookup"><span data-stu-id="22517-308">*After creating the Web API controller with scaffolding*</span></span>
6. <span data-ttu-id="22517-309">Abra o **ApiPersonController.cs** do arquivo e inspecione o *GetPeople* método de ação.</span><span class="sxs-lookup"><span data-stu-id="22517-309">Open the **ApiPersonController.cs** file and inspect the *GetPeople* action method.</span></span> <span data-ttu-id="22517-310">Este método consulta o campo de banco de dados do **PersonContext** tipo para as pessoas de obtenção de dados.</span><span class="sxs-lookup"><span data-stu-id="22517-310">This method queries the db field of **PersonContext** type in order to get the people data.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample5.cs)]
7. <span data-ttu-id="22517-311">Agora, observe o comentário acima da definição de método.</span><span class="sxs-lookup"><span data-stu-id="22517-311">Now notice the comment above the method definition.</span></span> <span data-ttu-id="22517-312">Ele fornece o URI que expõe esta ação que você usará na próxima tarefa.</span><span class="sxs-lookup"><span data-stu-id="22517-312">It provides the URI that exposes this action which you will use in the next task.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample6.cs)]

    > [!NOTE]
    > <span data-ttu-id="22517-313">Por padrão, a API da Web está configurada para capturar as consultas para o */API* caminho para evitar colisões com controladores MVC.</span><span class="sxs-lookup"><span data-stu-id="22517-313">By default, Web API is configured to catch the queries to the */api* path to avoid collisions with MVC controllers.</span></span> <span data-ttu-id="22517-314">Se você precisar alterar essa configuração, consulte [roteamento na API Web ASP.NET](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="22517-314">If you need to change this setting, refer to [Routing in ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

<a id="Ex3Task2"></a>
#### <a name="task-2--running-the-solution"></a><span data-ttu-id="22517-315">Tarefa 2 – executar a solução</span><span class="sxs-lookup"><span data-stu-id="22517-315">Task 2 – Running the Solution</span></span>

<span data-ttu-id="22517-316">Nesta tarefa, você usará o Internet Explorer **ferramentas de desenvolvedor F12** para inspecionar a resposta completa do controlador de API da Web.</span><span class="sxs-lookup"><span data-stu-id="22517-316">In this task you will use the Internet Explorer **F12 developer tools** to inspect the full response from the Web API controller.</span></span> <span data-ttu-id="22517-317">Você verá como você pode capturar o tráfego de rede para obter mais informações sobre os dados do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="22517-317">You will see how you can capture network traffic to get more insight into your application data.</span></span>

> [!NOTE]
> <span data-ttu-id="22517-318">Certifique-se de que **Internet Explorer** está selecionado na **iniciar** botão localizado na barra de ferramentas do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="22517-318">Make sure that **Internet Explorer** is selected in the **Start** button located on the Visual Studio toolbar.</span></span>
> 
> ![Opção do Internet Explorer](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image27.png)
> 
> <span data-ttu-id="22517-320">O **ferramentas de desenvolvedor F12** têm um amplo conjunto de funcionalidade que não é abordada neste laboratório prático.</span><span class="sxs-lookup"><span data-stu-id="22517-320">The **F12 developer tools** have a wide set of functionality that is not covered in this hands-on-lab.</span></span> <span data-ttu-id="22517-321">Se você quiser saber mais sobre isso, consulte [usando as ferramentas de desenvolvedor F12](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).</span><span class="sxs-lookup"><span data-stu-id="22517-321">If you want to learn more about it, refer to [Using the F12 developer tools](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).</span></span>


1. <span data-ttu-id="22517-322">Pressione **F5** para executar a solução.</span><span class="sxs-lookup"><span data-stu-id="22517-322">Press **F5** to run the solution.</span></span>

    > [!NOTE]
    > <span data-ttu-id="22517-323">Para seguir essa tarefa corretamente, seu aplicativo precisa ter dados.</span><span class="sxs-lookup"><span data-stu-id="22517-323">In order to follow this task correctly, your application needs to have data.</span></span> <span data-ttu-id="22517-324">Se seu banco de dados estiver vazio, você pode voltar para a tarefa 3 no Exercício 2 e siga as etapas sobre como criar uma nova pessoa usando as exibições do MVC.</span><span class="sxs-lookup"><span data-stu-id="22517-324">If your database is empty, you can go back to Task 3 in Exercise 2 and follow the steps on how to create a new person using the MVC views.</span></span>
2. <span data-ttu-id="22517-325">No navegador, pressione **F12** para abrir o **ferramentas de desenvolvedor** painel.</span><span class="sxs-lookup"><span data-stu-id="22517-325">In the browser, press **F12** to open the **Developer Tools** panel.</span></span> <span data-ttu-id="22517-326">Pressione **CTRL** + **4** ou clique no **rede** ícone e, em seguida, clique botão de seta verde para começar a capturar o tráfego de rede.</span><span class="sxs-lookup"><span data-stu-id="22517-326">Press **CTRL** + **4** or click the **Network** icon, and then click the green arrow button to begin capturing network traffic.</span></span>

    <span data-ttu-id="22517-327">![Iniciando captura de rede da API Web](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "captura de rede iniciando a API da Web")</span><span class="sxs-lookup"><span data-stu-id="22517-327">![Initiating Web API network capture](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "Initiating Web API network capture")</span></span>

    <span data-ttu-id="22517-328">*Iniciando captura de rede de API da Web*</span><span class="sxs-lookup"><span data-stu-id="22517-328">*Initiating Web API network capture*</span></span>
3. <span data-ttu-id="22517-329">Acrescente **api/ApiPerson** para a URL na barra de endereços do navegador.</span><span class="sxs-lookup"><span data-stu-id="22517-329">Append **api/ApiPerson** to the URL in the browser's address bar.</span></span> <span data-ttu-id="22517-330">Agora, você inspecionará os detalhes da resposta do **ApiPersonController**.</span><span class="sxs-lookup"><span data-stu-id="22517-330">You will now inspect the details of the response from the **ApiPersonController**.</span></span>

    <span data-ttu-id="22517-331">![Recuperando dados da pessoa por meio da API Web](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "recuperando dados da pessoa por meio da API da Web")</span><span class="sxs-lookup"><span data-stu-id="22517-331">![Retrieving person data through Web API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "Retrieving person data through Web API")</span></span>

    <span data-ttu-id="22517-332">*Recuperando dados da pessoa por meio da API da Web*</span><span class="sxs-lookup"><span data-stu-id="22517-332">*Retrieving person data through Web API*</span></span>

    > [!NOTE]
    > <span data-ttu-id="22517-333">Quando o download for concluído, você precisará fazer uma ação com o arquivo baixado.</span><span class="sxs-lookup"><span data-stu-id="22517-333">Once the download finishes, you will be prompted to make an action with the downloaded file.</span></span> <span data-ttu-id="22517-334">Deixe a caixa de diálogo aberta para que seja possível assistir o conteúdo da resposta por meio da janela de ferramenta de desenvolvedores.</span><span class="sxs-lookup"><span data-stu-id="22517-334">Leave the dialog box open in order to be able to watch the response content through the Developers Tool window.</span></span>
4. <span data-ttu-id="22517-335">Agora você irá inspecionar o corpo da resposta.</span><span class="sxs-lookup"><span data-stu-id="22517-335">Now you will inspect the body of the response.</span></span> <span data-ttu-id="22517-336">Para fazer isso, clique o **detalhes** guia e, em seguida, clique em **corpo da resposta**.</span><span class="sxs-lookup"><span data-stu-id="22517-336">To do this, click the **Details** tab and then click **Response body**.</span></span> <span data-ttu-id="22517-337">Você pode verificar que os dados baixados são uma lista de objetos com as propriedades **identificação**, **nome** e **idade** que correspondam ao **pessoa** classe.</span><span class="sxs-lookup"><span data-stu-id="22517-337">You can check that the downloaded data is a list of objects with the properties **Id**, **Name** and **Age** that correspond to the **Person** class.</span></span>

    <span data-ttu-id="22517-338">![Corpo de resposta da API da Web de exibindo](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "visualização de corpo de resposta da API da Web")</span><span class="sxs-lookup"><span data-stu-id="22517-338">![Viewing Web API Response Body](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "Viewing Web API Response Body")</span></span>

    <span data-ttu-id="22517-339">*Corpo de resposta da API da Web exibindo*</span><span class="sxs-lookup"><span data-stu-id="22517-339">*Viewing Web API Response Body*</span></span>

<a id="Ex3Task3"></a>
#### <a name="task-3--adding-web-api-help-pages"></a><span data-ttu-id="22517-340">Tarefa 3 – adicionar páginas de Ajuda da API da Web</span><span class="sxs-lookup"><span data-stu-id="22517-340">Task 3 – Adding Web API Help Pages</span></span>

<span data-ttu-id="22517-341">Quando você cria uma API da Web, é útil criar uma página de ajuda para que outros desenvolvedores saibam como chamar sua API.</span><span class="sxs-lookup"><span data-stu-id="22517-341">When you create a Web API, it is useful to create a help page so that other developers will know how to call your API.</span></span> <span data-ttu-id="22517-342">Você pode criar e atualizar as páginas de documentação manualmente, mas é melhor gerar automaticamente-los para evitar ter que fazer o trabalho de manutenção.</span><span class="sxs-lookup"><span data-stu-id="22517-342">You could create and update the documentation pages manually, but it is better to auto-generate them to avoid having to do maintenance work.</span></span> <span data-ttu-id="22517-343">Nesta tarefa, você usará um pacote do Nuget para gerar automaticamente as páginas de Ajuda da API da Web à solução.</span><span class="sxs-lookup"><span data-stu-id="22517-343">In this task you will use a Nuget package to automatically generate Web API help pages to the solution.</span></span>

1. <span data-ttu-id="22517-344">Dos **ferramentas** menu no Visual Studio, selecione **Gerenciador de pacotes de biblioteca**e, em seguida, clique em **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="22517-344">From the **Tools** menu in Visual Studio, select **Library Package Manager**, and then click **Package Manager Console**.</span></span>
2. <span data-ttu-id="22517-345">No **Package Manager Console** janela, execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="22517-345">In the **Package Manager Console** window, execute the following command:</span></span>

    [!code-powershell[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample7.ps1)]

    > [!NOTE]
    > <span data-ttu-id="22517-346">O **Microsoft.AspNet.WebApi.HelpPage** pacote instala os assemblies necessários e adiciona as exibições do MVC para as páginas de ajuda na **áreas/HelpPage** pasta.</span><span class="sxs-lookup"><span data-stu-id="22517-346">The **Microsoft.AspNet.WebApi.HelpPage** package installs the necessary assemblies and adds MVC Views for the help pages under the **Areas/HelpPage** folder.</span></span>

    <span data-ttu-id="22517-347">![Área de HelpPage](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage área")</span><span class="sxs-lookup"><span data-stu-id="22517-347">![HelpPage Area](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage Area")</span></span>

    <span data-ttu-id="22517-348">*Área de HelpPage*</span><span class="sxs-lookup"><span data-stu-id="22517-348">*HelpPage Area*</span></span>
3. <span data-ttu-id="22517-349">Por padrão, a Ajuda páginas têm cadeias de caracteres de espaço reservado para a documentação.</span><span class="sxs-lookup"><span data-stu-id="22517-349">By default, the help pages have placeholder strings for documentation.</span></span> <span data-ttu-id="22517-350">Você pode usar comentários da documentação XML para criar a documentação.</span><span class="sxs-lookup"><span data-stu-id="22517-350">You can use XML documentation comments to create the documentation.</span></span> <span data-ttu-id="22517-351">Para habilitar esse recurso, abra o **HelpPageConfig.cs** arquivos localizados na **HelpPage/aplicativo/áreas\_iniciar** pasta e remova a seguinte linha:</span><span class="sxs-lookup"><span data-stu-id="22517-351">To enable this feature, open the **HelpPageConfig.cs** file located in the **Areas/HelpPage/App\_Start** folder and uncomment the following line:</span></span>

    [!code-javascript[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample8.js)]
4. <span data-ttu-id="22517-352">Na **Gerenciador de soluções**, clique com botão direito no projeto **MyHybridSite**, selecione **propriedades** e clique no **Build** guia.</span><span class="sxs-lookup"><span data-stu-id="22517-352">In **Solution Explorer**, right-click the project **MyHybridSite**, select **Properties** and click the **Build** tab.</span></span>

    <span data-ttu-id="22517-353">![Compile a guia](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "seção de Build")</span><span class="sxs-lookup"><span data-stu-id="22517-353">![Build tab](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "Build section")</span></span>

    <span data-ttu-id="22517-354">*Guia compilação*</span><span class="sxs-lookup"><span data-stu-id="22517-354">*Build tab*</span></span>
5. <span data-ttu-id="22517-355">Sob **saída**, selecione **arquivo de documentação XML**.</span><span class="sxs-lookup"><span data-stu-id="22517-355">Under **Output**, select **XML documentation file**.</span></span> <span data-ttu-id="22517-356">Na caixa de edição, digite **App\_Data/XmlDocument.xml**.</span><span class="sxs-lookup"><span data-stu-id="22517-356">In the edit box, type **App\_Data/XmlDocument.xml**.</span></span>

    <span data-ttu-id="22517-357">![Saída da seção na guia Build](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "seção na guia build de saída")</span><span class="sxs-lookup"><span data-stu-id="22517-357">![Output section in Build tab](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "Output section in build tab")</span></span>

    <span data-ttu-id="22517-358">*Seção de saída na guia Build*</span><span class="sxs-lookup"><span data-stu-id="22517-358">*Output section in Build tab*</span></span>
6. <span data-ttu-id="22517-359">Pressione **CTRL** + **S** para salvar as alterações.</span><span class="sxs-lookup"><span data-stu-id="22517-359">Press **CTRL** + **S** to save the changes.</span></span>
7. <span data-ttu-id="22517-360">Abra o **ApiPersonController.cs** do arquivo da **controladores** pasta.</span><span class="sxs-lookup"><span data-stu-id="22517-360">Open the **ApiPersonController.cs** file from the **Controllers** folder.</span></span>
8. <span data-ttu-id="22517-361">Insira uma nova linha entre o *GetPeople* assinatura de método e o */ / obter api/ApiPerson* de comentário e, em seguida, digite três barras "/".</span><span class="sxs-lookup"><span data-stu-id="22517-361">Enter a new line between the *GetPeople* method signature and the *// GET api/ApiPerson* comment, and then type three forward slashes.</span></span>

    > [!NOTE]
    > <span data-ttu-id="22517-362">Visual Studio insere automaticamente os elementos XML que definem a documentação do método.</span><span class="sxs-lookup"><span data-stu-id="22517-362">Visual Studio automatically inserts the XML elements which define the method documentation.</span></span>
9. <span data-ttu-id="22517-363">Adicionar um texto de resumo e o valor retornado para o *GetPeople* método.</span><span class="sxs-lookup"><span data-stu-id="22517-363">Add a summary text and the return value for the *GetPeople* method.</span></span> <span data-ttu-id="22517-364">Ele deverá ter a seguinte aparência.</span><span class="sxs-lookup"><span data-stu-id="22517-364">It should look like the following.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample9.cs)]
10. <span data-ttu-id="22517-365">Pressione **F5** para executar a solução.</span><span class="sxs-lookup"><span data-stu-id="22517-365">Press **F5** to run the solution.</span></span>
11. <span data-ttu-id="22517-366">Acrescente **/Help** para a URL na barra de endereços para navegar até a página de Ajuda.</span><span class="sxs-lookup"><span data-stu-id="22517-366">Append **/help** to the URL in the address bar to browse to the help page.</span></span>

    <span data-ttu-id="22517-367">![Página de Ajuda da API Web ASP.NET](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "página de Ajuda da API Web ASP.NET")</span><span class="sxs-lookup"><span data-stu-id="22517-367">![ASP.NET Web API Help Page](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "ASP.NET Web API Help Page")</span></span>

    <span data-ttu-id="22517-368">*Página de Ajuda da API Web ASP.NET*</span><span class="sxs-lookup"><span data-stu-id="22517-368">*ASP.NET Web API Help Page*</span></span>

    > [!NOTE]
    > <span data-ttu-id="22517-369">O conteúdo principal da página é uma tabela de APIs, agrupadas pelo controlador.</span><span class="sxs-lookup"><span data-stu-id="22517-369">The main content of the page is a table of APIs, grouped by controller.</span></span> <span data-ttu-id="22517-370">As entradas da tabela são geradas dinamicamente, usando o **IApiExplorer** interface.</span><span class="sxs-lookup"><span data-stu-id="22517-370">The table entries are generated dynamically, using the **IApiExplorer** interface.</span></span> <span data-ttu-id="22517-371">Se você adicionar ou atualizar um controlador de API, a tabela será automaticamente atualizada na próxima vez que você compila o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="22517-371">If you add or update an API controller, the table will be automatically updated the next time you build the application.</span></span>
    > 
    > <span data-ttu-id="22517-372">O **API** coluna lista o método HTTP e o URI relativo.</span><span class="sxs-lookup"><span data-stu-id="22517-372">The **API** column lists the HTTP method and relative URI.</span></span> <span data-ttu-id="22517-373">O **descrição** coluna contém informações que foram extraídas de documentação do método.</span><span class="sxs-lookup"><span data-stu-id="22517-373">The **Description** column contains information that has been extracted from the method's documentation.</span></span>
12. <span data-ttu-id="22517-374">Observe que a descrição que você adicionou acima da definição de método é exibida na coluna Descrição.</span><span class="sxs-lookup"><span data-stu-id="22517-374">Note that the description you added above the method definition is displayed in the description column.</span></span>

    <span data-ttu-id="22517-375">![Descrição do método de API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "descrição do método de API")</span><span class="sxs-lookup"><span data-stu-id="22517-375">![API method description](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "API method description")</span></span>

    <span data-ttu-id="22517-376">*Descrição do método de API*</span><span class="sxs-lookup"><span data-stu-id="22517-376">*API method description*</span></span>
13. <span data-ttu-id="22517-377">Clique em um dos métodos da API para navegar até uma página com informações mais detalhadas, incluindo os corpos de resposta de exemplo.</span><span class="sxs-lookup"><span data-stu-id="22517-377">Click one of the API methods to navigate to a page with more detailed information, including sample response bodies.</span></span>

    <span data-ttu-id="22517-378">![Página informações detalhadas](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "página de informações de detalhe")</span><span class="sxs-lookup"><span data-stu-id="22517-378">![Detail Information page](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "Detail Information page")</span></span>

    <span data-ttu-id="22517-379">*Página de informações detalhadas*</span><span class="sxs-lookup"><span data-stu-id="22517-379">*Detailed information page*</span></span>

* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="22517-380">Resumo</span><span class="sxs-lookup"><span data-stu-id="22517-380">Summary</span></span>

<span data-ttu-id="22517-381">Ao concluir este laboratório prático que você aprendeu como:</span><span class="sxs-lookup"><span data-stu-id="22517-381">By completing this hands-on lab you have learned how to:</span></span>

- <span data-ttu-id="22517-382">Criar um novo aplicativo Web usando uma experiência de ASP.NET no Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="22517-382">Create a new Web application using the One ASP.NET Experience in Visual Studio 2013</span></span>
- <span data-ttu-id="22517-383">Integrar várias tecnologias do ASP.NET em um único projeto</span><span class="sxs-lookup"><span data-stu-id="22517-383">Integrate multiple ASP.NET technologies into one single project</span></span>
- <span data-ttu-id="22517-384">Gerar exibições e controladores MVC de suas classes de modelo usando o Scaffolding do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="22517-384">Generate MVC controllers and views from your model classes using ASP.NET Scaffolding</span></span>
- <span data-ttu-id="22517-385">Gerar controladores da API Web, que usam recursos como a programação assíncrona e acesso a dados por meio do Entity Framework</span><span class="sxs-lookup"><span data-stu-id="22517-385">Generate Web API controllers, which use features such as Async Programming and data access through Entity Framework</span></span>
- <span data-ttu-id="22517-386">Gerar automaticamente as páginas de Ajuda da API da Web para seus controladores</span><span class="sxs-lookup"><span data-stu-id="22517-386">Automatically generate Web API Help Pages for your controllers</span></span>
