---
uid: web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
title: Simulação do Entity Framework quando a API Web ASP.NET 2 de teste de unidade | Microsoft Docs
author: tfitzmac
description: Este guia e o aplicativo demonstram como criar testes de unidade para seu aplicativo de API Web 2 que usa o Entity Framework. Ele mostra como modificar o...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/13/2013
ms.topic: article
ms.assetid: cd844025-ccad-41ce-8694-595f1022a49f
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: f1ff2fda85a6d56a6bbb76b1bff740301ab0c70d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371863"
---
<a name="mocking-entity-framework-when-unit-testing-aspnet-web-api-2"></a><span data-ttu-id="aa3a5-104">Simulação do Entity Framework quando a API Web ASP.NET 2 de teste de unidade</span><span class="sxs-lookup"><span data-stu-id="aa3a5-104">Mocking Entity Framework when Unit Testing ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="aa3a5-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="aa3a5-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[<span data-ttu-id="aa3a5-106">Baixe o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="aa3a5-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-e2867d4d)

> <span data-ttu-id="aa3a5-107">Este guia e o aplicativo demonstram como criar testes de unidade para seu aplicativo de API Web 2 que usa o Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-107">This guidance and application demonstrate how to create unit tests for your Web API 2 application that uses the Entity Framework.</span></span> <span data-ttu-id="aa3a5-108">Ele mostra como modificar o controlador gerados automaticamente para habilitar passando um objeto de contexto para teste e como criar objetos de teste que funcionam com o Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-108">It shows how to modify the scaffolded controller to enable passing a context object for testing, and how to create test objects that work with Entity Framework.</span></span>
> 
> <span data-ttu-id="aa3a5-109">Para obter uma introdução ao teste de unidade com a API Web ASP.NET, consulte [Unit Testing com o ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="aa3a5-109">For an introduction to unit testing with ASP.NET Web API, see [Unit Testing with ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md).</span></span>
> 
> <span data-ttu-id="aa3a5-110">Este tutorial presume que você está familiarizado com os conceitos básicos da API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-110">This tutorial assumes you are familiar with the basic concepts of ASP.NET Web API.</span></span> <span data-ttu-id="aa3a5-111">Para um tutorial de Introdução, consulte [Introdução ao ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="aa3a5-111">For an introductory tutorial, see [Getting Started with ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="aa3a5-112">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="aa3a5-112">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="aa3a5-113">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="aa3a5-113">Visual Studio 2017</span></span>](https://www.visualstudio.com/vs/)
> - <span data-ttu-id="aa3a5-114">API Web 2</span><span class="sxs-lookup"><span data-stu-id="aa3a5-114">Web API 2</span></span>


## <a name="in-this-topic"></a><span data-ttu-id="aa3a5-115">Neste tópico</span><span class="sxs-lookup"><span data-stu-id="aa3a5-115">In this topic</span></span>

<span data-ttu-id="aa3a5-116">Esse tópico contém as seguintes seções:</span><span class="sxs-lookup"><span data-stu-id="aa3a5-116">This topic contains the following sections:</span></span>

- [<span data-ttu-id="aa3a5-117">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="aa3a5-117">Prerequisites</span></span>](#prereqs)
- [<span data-ttu-id="aa3a5-118">Baixar o código</span><span class="sxs-lookup"><span data-stu-id="aa3a5-118">Download code</span></span>](#download)
- [<span data-ttu-id="aa3a5-119">Criar aplicativo com o projeto de teste de unidade</span><span class="sxs-lookup"><span data-stu-id="aa3a5-119">Create application with unit test project</span></span>](#appwithunittest)
- [<span data-ttu-id="aa3a5-120">Criar a classe de modelo</span><span class="sxs-lookup"><span data-stu-id="aa3a5-120">Create the model class</span></span>](#modelclass)
- [<span data-ttu-id="aa3a5-121">Adicionar o controlador</span><span class="sxs-lookup"><span data-stu-id="aa3a5-121">Add the controller</span></span>](#controller)
- [<span data-ttu-id="aa3a5-122">Adicionar a injeção de dependência</span><span class="sxs-lookup"><span data-stu-id="aa3a5-122">Add dependency injection</span></span>](#dependency)
- [<span data-ttu-id="aa3a5-123">Instalar os pacotes NuGet no projeto de teste</span><span class="sxs-lookup"><span data-stu-id="aa3a5-123">Install NuGet packages in test project</span></span>](#testpackages)
- [<span data-ttu-id="aa3a5-124">Criar o contexto do teste</span><span class="sxs-lookup"><span data-stu-id="aa3a5-124">Create test context</span></span>](#testcontext)
- [<span data-ttu-id="aa3a5-125">Criar testes</span><span class="sxs-lookup"><span data-stu-id="aa3a5-125">Create tests</span></span>](#tests)
- [<span data-ttu-id="aa3a5-126">Executar testes</span><span class="sxs-lookup"><span data-stu-id="aa3a5-126">Run tests</span></span>](#runtests)

<span data-ttu-id="aa3a5-127">Se você já tiver concluído as etapas em [Unit Testing com o ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), você pode pular para a seção [adicionar o controlador](#controller).</span><span class="sxs-lookup"><span data-stu-id="aa3a5-127">If you have already completed the steps in [Unit Testing with ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), you can skip to the section [Add the controller](#controller).</span></span>

<a id="prereqs"></a>
## <a name="prerequisites"></a><span data-ttu-id="aa3a5-128">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="aa3a5-128">Prerequisites</span></span>

<span data-ttu-id="aa3a5-129">Edição do Visual Studio 2017 Community, Professional ou Enterprise</span><span class="sxs-lookup"><span data-stu-id="aa3a5-129">Visual Studio 2017 Community, Professional or Enterprise edition</span></span>

<a id="download"></a>
## <a name="download-code"></a><span data-ttu-id="aa3a5-130">Baixar o código</span><span class="sxs-lookup"><span data-stu-id="aa3a5-130">Download code</span></span>

<span data-ttu-id="aa3a5-131">Baixe o [projeto concluído](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span><span class="sxs-lookup"><span data-stu-id="aa3a5-131">Download the [completed project](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span></span> <span data-ttu-id="aa3a5-132">O projeto que pode ser baixado inclui código de teste de unidade para este tópico e para o [unidade de teste de API Web ASP.NET 2](unit-testing-with-aspnet-web-api.md) tópico.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-132">The downloadable project includes unit test code for this topic and for the [Unit Testing ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md) topic.</span></span>

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a><span data-ttu-id="aa3a5-133">Criar aplicativo com o projeto de teste de unidade</span><span class="sxs-lookup"><span data-stu-id="aa3a5-133">Create application with unit test project</span></span>

<span data-ttu-id="aa3a5-134">Você pode criar um projeto de teste de unidade durante a criação de seu aplicativo ou adicionar um projeto de teste de unidade para um aplicativo existente.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-134">You can either create a unit test project when creating your application or add a unit test project to an existing application.</span></span> <span data-ttu-id="aa3a5-135">Este tutorial mostra como criar um projeto de teste de unidade ao criar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-135">This tutorial shows creating a unit test project when creating the application.</span></span>

<span data-ttu-id="aa3a5-136">Criar um novo aplicativo de Web do ASP.NET chamado **StoreApp**.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-136">Create a new ASP.NET Web Application named **StoreApp**.</span></span>

<span data-ttu-id="aa3a5-137">Nas janelas do novo projeto ASP.NET, selecione o **vazio** modelo e adicionar pastas e os principais referências de API da Web.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-137">In the New ASP.NET Project windows, select the **Empty** template and add folders and core references for Web API.</span></span> <span data-ttu-id="aa3a5-138">Selecione o **adicionar testes de unidade** opção.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-138">Select the **Add unit tests** option.</span></span> <span data-ttu-id="aa3a5-139">O projeto de teste de unidade é automaticamente denominado **StoreApp.Tests**.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-139">The unit test project is automatically named **StoreApp.Tests**.</span></span> <span data-ttu-id="aa3a5-140">Você pode manter esse nome.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-140">You can keep this name.</span></span>

![Criar projeto de teste de unidade](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image1.png)

<span data-ttu-id="aa3a5-142">Depois de criar o aplicativo, você verá que ele contém dois projetos - **StoreApp** e **StoreApp.Tests**.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-142">After creating the application, you will see it contains two projects - **StoreApp** and **StoreApp.Tests**.</span></span>

<a id="modelclass"></a>
## <a name="create-the-model-class"></a><span data-ttu-id="aa3a5-143">Criar a classe de modelo</span><span class="sxs-lookup"><span data-stu-id="aa3a5-143">Create the model class</span></span>

<span data-ttu-id="aa3a5-144">Em seu projeto de StoreApp, adicione um arquivo de classe para o **modelos** pasta denominada **Product.cs**.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-144">In your StoreApp project, add a class file to the **Models** folder named **Product.cs**.</span></span> <span data-ttu-id="aa3a5-145">Substitua o conteúdo do arquivo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-145">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample1.cs)]

<span data-ttu-id="aa3a5-146">Compile a solução.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-146">Build the solution.</span></span>

<a id="controller"></a>
## <a name="add-the-controller"></a><span data-ttu-id="aa3a5-147">Adicionar o controlador</span><span class="sxs-lookup"><span data-stu-id="aa3a5-147">Add the controller</span></span>

<span data-ttu-id="aa3a5-148">Clique com botão direito na pasta controladores e selecione **Add** e **Novo Item de Scaffold**.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-148">Right-click the Controllers folder and select **Add** and **New Scaffolded Item**.</span></span> <span data-ttu-id="aa3a5-149">Selecione o controlador Web API 2 com ações, usando o Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-149">Select Web API 2 Controller with actions, using Entity Framework.</span></span>

![Adicionar novo controlador](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image2.png)

<span data-ttu-id="aa3a5-151">Defina os seguintes valores:</span><span class="sxs-lookup"><span data-stu-id="aa3a5-151">Set the following values:</span></span>

- <span data-ttu-id="aa3a5-152">Nome do controlador: **ProductController**</span><span class="sxs-lookup"><span data-stu-id="aa3a5-152">Controller name: **ProductController**</span></span>
- <span data-ttu-id="aa3a5-153">Classe de modelo: **produto**</span><span class="sxs-lookup"><span data-stu-id="aa3a5-153">Model class: **Product**</span></span>
- <span data-ttu-id="aa3a5-154">Classe de contexto de dados: [selecionar **novo contexto de dados** botão que preenche os valores vistos abaixo]</span><span class="sxs-lookup"><span data-stu-id="aa3a5-154">Data context class: [Select **New data context** button which fills in the values seen below]</span></span>

![Especifique o controlador](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image3.png)

<span data-ttu-id="aa3a5-156">Clique em **adicionar** para criar o controlador com o código gerado automaticamente.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-156">Click **Add** to create the controller with automatically-generated code.</span></span> <span data-ttu-id="aa3a5-157">O código inclui métodos para criar, recuperar, atualizar e excluir instâncias da classe de produto.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-157">The code includes methods for creating, retrieving, updating and deleting instances of the Product class.</span></span> <span data-ttu-id="aa3a5-158">O código a seguir mostra o método para adicionar um produto.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-158">The following code shows the method for add a Product.</span></span> <span data-ttu-id="aa3a5-159">Observe que o método retorna uma instância do **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-159">Notice that the method returns an instance of **IHttpActionResult**.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample2.cs)]

<span data-ttu-id="aa3a5-160">IHttpActionResult é um dos novos recursos na API Web 2, e ele simplifica o desenvolvimento de teste de unidade.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-160">IHttpActionResult is one of the new features in Web API 2, and it simplifies unit test development.</span></span>

<span data-ttu-id="aa3a5-161">Na próxima seção, você personalizará o código gerado para facilitar a passagem de objetos de teste para o controlador.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-161">In the next section, you will customize the generated code to facilitate passing test objects to the controller.</span></span>

<a id="dependency"></a>
## <a name="add-dependency-injection"></a><span data-ttu-id="aa3a5-162">Adicionar a injeção de dependência</span><span class="sxs-lookup"><span data-stu-id="aa3a5-162">Add dependency injection</span></span>

<span data-ttu-id="aa3a5-163">Atualmente, a classe ProductController é codificado para usar uma instância da classe StoreAppContext.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-163">Currently, the ProductController class is hard-coded to use an instance of the StoreAppContext class.</span></span> <span data-ttu-id="aa3a5-164">Você usará um padrão chamado injeção de dependência para modificar seu aplicativo e remover essa dependência embutido em código.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-164">You will use a pattern called dependency injection to modify your application and remove that hard-coded dependency.</span></span> <span data-ttu-id="aa3a5-165">Ao dividir essa dependência, você pode passar um objeto fictício durante o teste.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-165">By breaking this dependency, you can pass in a mock object when testing.</span></span>

<span data-ttu-id="aa3a5-166">Clique com botão direito do **modelos** pasta e adicione uma nova interface chamada **IStoreAppContext**.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-166">Right-click the **Models** folder, and add a new interface named **IStoreAppContext**.</span></span>

<span data-ttu-id="aa3a5-167">Substitua o código pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-167">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample3.cs)]

<span data-ttu-id="aa3a5-168">Abra o arquivo StoreAppContext.cs e faça as seguintes alterações realçadas.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-168">Open the StoreAppContext.cs file and make the following highlighted changes.</span></span> <span data-ttu-id="aa3a5-169">As alterações importantes a observar são:</span><span class="sxs-lookup"><span data-stu-id="aa3a5-169">The important changes to note are:</span></span>

- <span data-ttu-id="aa3a5-170">Classe StoreAppContext implementa a interface IStoreAppContext</span><span class="sxs-lookup"><span data-stu-id="aa3a5-170">StoreAppContext class implements IStoreAppContext interface</span></span>
- <span data-ttu-id="aa3a5-171">Método MarkAsModified é implementado</span><span class="sxs-lookup"><span data-stu-id="aa3a5-171">MarkAsModified method is implemented</span></span>


[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample4.cs?highlight=6,14-17)]

<span data-ttu-id="aa3a5-172">Abra o arquivo ProductController.cs.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-172">Open the ProductController.cs file.</span></span> <span data-ttu-id="aa3a5-173">Altere o código existente para coincidir com o código realçado.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-173">Change the existing code to match the highlighted code.</span></span> <span data-ttu-id="aa3a5-174">Essas alterações quebrar a dependência no StoreAppContext e habilitar a outras classes transmitir um objeto diferente para a classe de contexto.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-174">These changes break the dependency on StoreAppContext and enable other classes to pass in a different object for the context class.</span></span> <span data-ttu-id="aa3a5-175">Essa alteração permitirá que você passar um contexto de teste durante testes de unidade.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-175">This change will enable you to pass in a test context during unit tests.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample5.cs?highlight=4,7-12)]

<span data-ttu-id="aa3a5-176">Não há mais de uma alteração que você deve fazer no ProductController.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-176">There is one more change you must make in ProductController.</span></span> <span data-ttu-id="aa3a5-177">No **PutProduct** método, substitua a linha que define o estado da entidade como modificada com uma chamada ao método MarkAsModified.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-177">In the **PutProduct** method, replace the line that sets the entity state to modified with a call to the MarkAsModified method.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample6.cs?highlight=14-15)]

<span data-ttu-id="aa3a5-178">Compile a solução.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-178">Build the solution.</span></span>

<span data-ttu-id="aa3a5-179">Agora você está pronto para configurar o projeto de teste.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-179">You are now ready to set up the test project.</span></span>

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a><span data-ttu-id="aa3a5-180">Instalar os pacotes NuGet no projeto de teste</span><span class="sxs-lookup"><span data-stu-id="aa3a5-180">Install NuGet packages in test project</span></span>

<span data-ttu-id="aa3a5-181">Quando você usa o modelo vazio para criar um aplicativo, o projeto de teste de unidade (StoreApp.Tests) não inclui todos os pacotes NuGet instalados.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-181">When you use the Empty template to create an application, the unit test project (StoreApp.Tests) does not include any installed NuGet packages.</span></span> <span data-ttu-id="aa3a5-182">Outros modelos, como o modelo de API Web incluem alguns pacotes do NuGet no projeto de teste de unidade.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-182">Other templates, such as the Web API template, include some NuGet packages in the unit test project.</span></span> <span data-ttu-id="aa3a5-183">Para este tutorial, você deve incluir o packge do Entity Framework e o pacote Microsoft ASP.NET Web API 2 Core ao projeto de teste.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-183">For this tutorial, you must include the Entity Framework packge and the Microsoft ASP.NET Web API 2 Core package to the test project.</span></span>

<span data-ttu-id="aa3a5-184">Clique com botão direito no projeto StoreApp.Tests e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-184">Right-click the StoreApp.Tests project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="aa3a5-185">Você deve selecionar o projeto StoreApp.Tests para adicionar os pacotes para o projeto.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-185">You must select the StoreApp.Tests project to add the packages to that project.</span></span>

![Gerenciar pacotes](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image4.png)

<span data-ttu-id="aa3a5-187">Dos pacotes Online, localizar e instalar o pacote EntityFramework (versão 6.0 ou posterior).</span><span class="sxs-lookup"><span data-stu-id="aa3a5-187">From the Online packages, find and install the EntityFramework package (version 6.0 or later).</span></span> <span data-ttu-id="aa3a5-188">Se parecer que o pacote EntityFramework já está instalado, você talvez tenha selecionado o projeto StoreApp em vez do projeto StoreApp.Tests.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-188">If it appears that the EntityFramework package is already installed, you may have selected the StoreApp project instead of the StoreApp.Tests project.</span></span>

![Adicionar o Entity Framework](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image5.png)

<span data-ttu-id="aa3a5-190">Localizar e instalar o pacote do Microsoft ASP.NET Web API 2 Core.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-190">Find and install Microsoft ASP.NET Web API 2 Core package.</span></span>

![instalar o pacote de núcleo de api da web](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image6.png)

<span data-ttu-id="aa3a5-192">Feche a janela Gerenciar pacotes NuGet.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-192">Close the Manage NuGet Packages window.</span></span>

<a id="testcontext"></a>
## <a name="create-test-context"></a><span data-ttu-id="aa3a5-193">Criar o contexto do teste</span><span class="sxs-lookup"><span data-stu-id="aa3a5-193">Create test context</span></span>

<span data-ttu-id="aa3a5-194">Adicione uma classe chamada **TestDbSet** ao projeto de teste.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-194">Add a class named **TestDbSet** to the test project.</span></span> <span data-ttu-id="aa3a5-195">Esta classe serve como a classe base para seu conjunto de dados de teste.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-195">This class serves as the base class for your test data set.</span></span> <span data-ttu-id="aa3a5-196">Substitua o código pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-196">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample7.cs)]

<span data-ttu-id="aa3a5-197">Adicione uma classe chamada **TestProductDbSet** ao projeto de teste que contém o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-197">Add a class named **TestProductDbSet** to the test project which contains the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample8.cs)]

<span data-ttu-id="aa3a5-198">Adicione uma classe chamada **TestStoreAppContext** e substitua o código existente pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-198">Add a class named **TestStoreAppContext** and replace the existing code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample9.cs)]

<a id="tests"></a>
## <a name="create-tests"></a><span data-ttu-id="aa3a5-199">Criar testes</span><span class="sxs-lookup"><span data-stu-id="aa3a5-199">Create tests</span></span>

<span data-ttu-id="aa3a5-200">Por padrão, o seu projeto de teste inclui um arquivo de teste vazio chamado **UnitTest1.cs**.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-200">By default, your test project includes an empty test file named **UnitTest1.cs**.</span></span> <span data-ttu-id="aa3a5-201">Esse arquivo mostra os atributos que você usa para criar métodos de teste.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-201">This file shows the attributes you use to create test methods.</span></span> <span data-ttu-id="aa3a5-202">Para este tutorial, você pode excluir esse arquivo como você adicionará uma nova classe de teste.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-202">For this tutorial, you can delete this file because you will add a new test class.</span></span>

<span data-ttu-id="aa3a5-203">Adicione uma classe chamada **TestProductController** ao projeto de teste.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-203">Add a class named **TestProductController** to the test project.</span></span> <span data-ttu-id="aa3a5-204">Substitua o código pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-204">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample10.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a><span data-ttu-id="aa3a5-205">Executar testes</span><span class="sxs-lookup"><span data-stu-id="aa3a5-205">Run tests</span></span>

<span data-ttu-id="aa3a5-206">Agora você está pronto para executar os testes.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-206">You are now ready to run the tests.</span></span> <span data-ttu-id="aa3a5-207">Todos o método que são marcados com o **TestMethod** atributo será testado.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-207">All of the method that are marked with the **TestMethod** attribute will be tested.</span></span> <span data-ttu-id="aa3a5-208">Dos **teste** item de menu, execute os testes.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-208">From the **Test** menu item, run the tests.</span></span>

![executar testes](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image7.png)

<span data-ttu-id="aa3a5-210">Abra o **Gerenciador de testes** janela e observe os resultados dos testes.</span><span class="sxs-lookup"><span data-stu-id="aa3a5-210">Open the **Test Explorer** window, and notice the results of the tests.</span></span>

![resultados de testes](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image8.png)
