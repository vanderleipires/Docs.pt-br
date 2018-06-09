---
uid: web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
title: Chamando a API da Web de um Windows Phone 8 aplicativo (c#) | Microsoft Docs
author: rmcmurray
description: Crie um cenário de ponta a ponta completo consiste em um aplicativo de API da Web ASP.NET que fornece um catálogo de livros de um aplicativo do Windows Phone 8.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/09/2013
ms.topic: article
ms.assetid: b9775f41-352a-4f82-baa6-23e95b342e20
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
msc.type: authoredcontent
ms.openlocfilehash: 7d0486b4cab85ffe77fda87d4b34dd3ec0a9e8fe
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2018
ms.locfileid: "30874220"
---
<a name="calling-web-api-from-a-windows-phone-8-application-c"></a><span data-ttu-id="593f4-103">Chamar a API da Web de um aplicativo do Windows Phone 8 (c#)</span><span class="sxs-lookup"><span data-stu-id="593f4-103">Calling Web API from a Windows Phone 8 Application (C#)</span></span>
====================
<span data-ttu-id="593f4-104">por [Robert McMurray](https://github.com/rmcmurray)</span><span class="sxs-lookup"><span data-stu-id="593f4-104">by [Robert McMurray](https://github.com/rmcmurray)</span></span>

<span data-ttu-id="593f4-105">Neste tutorial, você aprenderá como criar um cenário de ponta a ponta completo consiste em um aplicativo de API da Web ASP.NET que fornece um catálogo de livros de um aplicativo do Windows Phone 8.</span><span class="sxs-lookup"><span data-stu-id="593f4-105">In this tutorial, you will learn how to create a complete end-to-end scenario consisting of an ASP.NET Web API application that provides a catalog of books to a Windows Phone 8 application.</span></span>

### <a name="overview"></a><span data-ttu-id="593f4-106">Visão geral</span><span class="sxs-lookup"><span data-stu-id="593f4-106">Overview</span></span>

<span data-ttu-id="593f4-107">Os serviços rESTful como ASP.NET Web API simplificam a criação de aplicativos baseados em HTTP para os desenvolvedores abstraindo a arquitetura de aplicativos do lado do servidor e do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="593f4-107">RESTful services like ASP.NET Web API simplify the creation of HTTP-based applications for developers by abstracting the architecture for server-side and client-side applications.</span></span> <span data-ttu-id="593f4-108">Em vez de criar um protocolo proprietário em soquete para comunicação, os desenvolvedores de API da Web simplesmente precisam publicar os métodos HTTP necessários para seu aplicativo (por exemplo: GET, POST, PUT, DELETE), e os desenvolvedores de aplicativos de cliente só precisam consumir os métodos HTTP que são necessários para seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="593f4-108">Instead of creating a proprietary socket-based protocol for communication, Web API developers simply need to publish the requisite HTTP methods for their application, (for example: GET, POST, PUT, DELETE), and client application developers only need to consume the HTTP methods that are necessary for their application.</span></span>

<span data-ttu-id="593f4-109">Este tutorial de ponta a ponta, você aprenderá como usar a API da Web para criar os projetos a seguir:</span><span class="sxs-lookup"><span data-stu-id="593f4-109">In this end-to-end tutorial, you will learn how to use Web API to create the following projects:</span></span>

- <span data-ttu-id="593f4-110">No [primeira parte deste tutorial](#STEP1), você criará um aplicativo de API da Web ASP.NET que dá suporte a todas as operações de criação, leitura, atualização e exclusão (CRUD) para gerenciar um catálogo de endereços.</span><span class="sxs-lookup"><span data-stu-id="593f4-110">In the [first part of this tutorial](#STEP1), you will create an ASP.NET Web API application that supports all of the Create, Read, Update, and Delete (CRUD) operations to manage a book catalog.</span></span> <span data-ttu-id="593f4-111">Este aplicativo usará o [arquivo XML de exemplo (books.xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) do MSDN.</span><span class="sxs-lookup"><span data-stu-id="593f4-111">This application will use the [Sample XML File (books.xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) from MSDN.</span></span>
- <span data-ttu-id="593f4-112">No [segunda parte deste tutorial](#STEP2), você criará um aplicativo interativo do Windows Phone 8 que recupera os dados do seu aplicativo de API da Web.</span><span class="sxs-lookup"><span data-stu-id="593f4-112">In the [second part of this tutorial](#STEP2), you will create an interactive Windows Phone 8 application that retrieves the data from your Web API application.</span></span>

#### <a name="prerequisites"></a><span data-ttu-id="593f4-113">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="593f4-113">Prerequisites</span></span>

- <span data-ttu-id="593f4-114">Visual Studio 2013 com o Windows Phone 8 SDK instalado</span><span class="sxs-lookup"><span data-stu-id="593f4-114">Visual Studio 2013 with the Windows Phone 8 SDK installed</span></span>
- <span data-ttu-id="593f4-115">Windows 8 ou posterior em um sistema de 64 bits com Hyper-V instalado</span><span class="sxs-lookup"><span data-stu-id="593f4-115">Windows 8 or later on a 64-bit system with Hyper-V installed</span></span>
- <span data-ttu-id="593f4-116">Para obter uma lista de requisitos adicionais, consulte o *requisitos de sistema* seção o [Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471) página de download.</span><span class="sxs-lookup"><span data-stu-id="593f4-116">For a list of additional requirements, see the *System Requirements* section on the [Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471) download page.</span></span>

> [!NOTE]
> <span data-ttu-id="593f4-117">Se você for para testar a conectividade entre a API da Web e projetos do Windows Phone 8 no seu sistema local, você precisará seguir as instruções no *[conectando o emulador do Windows Phone 8 para aplicativos de API da Web em um Local Computador](https://go.microsoft.com/fwlink/?LinkId=324014)* artigo para configurar seu ambiente de teste.</span><span class="sxs-lookup"><span data-stu-id="593f4-117">If you are going to test the connectivity between Web API and Windows Phone 8 projects on your local system, you will need to follow the instructions in the *[Connecting the Windows Phone 8 Emulator to Web API Applications on a Local Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* article to set up your testing environment.</span></span>


<a id="STEP1"></a>
### <a name="step-1-creating-the-web-api-bookstore-project"></a><span data-ttu-id="593f4-118">Etapa 1: Criando o projeto de livraria API da Web</span><span class="sxs-lookup"><span data-stu-id="593f4-118">Step 1: Creating the Web API Bookstore Project</span></span>

<span data-ttu-id="593f4-119">É a primeira etapa deste tutorial de ponta a ponta criar um projeto de API da Web que oferece suporte a todas as operações CRUD; Observe que você irá adicionar o projeto de aplicativo do Windows Phone para esta solução [etapa 2](#STEP2) deste tutorial.</span><span class="sxs-lookup"><span data-stu-id="593f4-119">The first step of this end-to-end tutorial is to create a Web API project that supports all of the CRUD operations; note that you will add the Windows Phone application project to this solution in [Step 2](#STEP2) of this tutorial.</span></span>

1. <span data-ttu-id="593f4-120">Abra **Visual Studio 2013**.</span><span class="sxs-lookup"><span data-stu-id="593f4-120">Open **Visual Studio 2013**.</span></span>
2. <span data-ttu-id="593f4-121">Clique em **arquivo**, em seguida, **novo**e, em seguida, **projeto**.</span><span class="sxs-lookup"><span data-stu-id="593f4-121">Click **File**, then **New**, and then **Project**.</span></span>
3. <span data-ttu-id="593f4-122">Quando o **novo projeto** caixa de diálogo é exibida, expanda **instalado**, em seguida, **modelos**, em seguida, **Visual C#** e, em seguida, **Web**.</span><span class="sxs-lookup"><span data-stu-id="593f4-122">When the **New Project** dialog box is displayed, expand **Installed**, then **Templates**, then **Visual C#**, and then **Web**.</span></span>


   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image2.png)](calling-web-api-from-a-windows-phone-8-application/_static/image1.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                <span data-ttu-id="593f4-123">Clique na imagem para expandir</span><span class="sxs-lookup"><span data-stu-id="593f4-123">Click image to expand</span></span>                                                                |


4. <span data-ttu-id="593f4-124">Realçar **aplicativo Web ASP.NET**, digite **livraria** para o nome do projeto e clique **Okey**.</span><span class="sxs-lookup"><span data-stu-id="593f4-124">Highlight **ASP.NET Web Application**, enter **BookStore** for the project name, and then click **OK**.</span></span>
5. <span data-ttu-id="593f4-125">Quando o **novo projeto ASP.NET** caixa de diálogo for exibida, selecione o **API da Web** modelo e depois clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="593f4-125">When the **New ASP.NET Project** dialog box is displayed, select the **Web API** template, and then click **OK**.</span></span>


   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image4.png)](calling-web-api-from-a-windows-phone-8-application/_static/image3.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                <span data-ttu-id="593f4-126">Clique na imagem para expandir</span><span class="sxs-lookup"><span data-stu-id="593f4-126">Click image to expand</span></span>                                                                |


6. <span data-ttu-id="593f4-127">Quando o projeto de API da Web é aberto, remova o controlador de exemplo do projeto:</span><span class="sxs-lookup"><span data-stu-id="593f4-127">When the Web API project opens, remove the sample controller from the project:</span></span>

    1. <span data-ttu-id="593f4-128">Expanda o **controladores** pasta no Gerenciador de soluções.</span><span class="sxs-lookup"><span data-stu-id="593f4-128">Expand the **Controllers** folder in the solution explorer.</span></span>
    2. <span data-ttu-id="593f4-129">Clique com botão direito do **ValuesController.cs** de arquivo e, em seguida, clique em **excluir**.</span><span class="sxs-lookup"><span data-stu-id="593f4-129">Right-click the **ValuesController.cs** file, and then click **Delete**.</span></span>
    3. <span data-ttu-id="593f4-130">Clique em **Okey** quando for solicitado a confirmar a exclusão.</span><span class="sxs-lookup"><span data-stu-id="593f4-130">Click **OK** when prompted to confirm the deletion.</span></span>
7. <span data-ttu-id="593f4-131">Adicionar um arquivo de dados XML para o projeto de API da Web; Esse arquivo contém o conteúdo do catálogo livraria:</span><span class="sxs-lookup"><span data-stu-id="593f4-131">Add an XML data file to the Web API project; this file contains the contents of the bookstore catalog:</span></span>

   1. <span data-ttu-id="593f4-132">Clique com botão direito do **aplicativo\_dados** pasta no Gerenciador de soluções, em seguida, clique em **adicionar**e, em seguida, clique em **Novo Item**.</span><span class="sxs-lookup"><span data-stu-id="593f4-132">Right-click the **App\_Data** folder in the solution explorer, then click **Add**, and then click **New Item**.</span></span>
   2. <span data-ttu-id="593f4-133">Quando o **Adicionar Novo Item** caixa de diálogo é exibida, realce o **arquivo XML** modelo.</span><span class="sxs-lookup"><span data-stu-id="593f4-133">When the **Add New Item** dialog box is displayed, highlight the **XML File** template.</span></span>
   3. <span data-ttu-id="593f4-134">Nomeie o arquivo **Books.xml**e, em seguida, clique em **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="593f4-134">Name the file **Books.xml**, and then click **Add**.</span></span>
   4. <span data-ttu-id="593f4-135">Quando o **Books.xml** arquivo é aberto, substitua o código no arquivo com o XML de exemplo **books.xml** arquivo no MSDN:</span><span class="sxs-lookup"><span data-stu-id="593f4-135">When the **Books.xml** file is opened, replace the code in the file with the XML from the sample **books.xml** file on MSDN:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample1.xml)]
   5. <span data-ttu-id="593f4-136">Salve e feche o arquivo XML.</span><span class="sxs-lookup"><span data-stu-id="593f4-136">Save and close the XML file.</span></span>

8. <span data-ttu-id="593f4-137">Adicionar modelo livraria para o projeto de API da Web; Este modelo contém a lógica da criação, leitura, atualização e exclusão (CRUD) para o aplicativo livraria:</span><span class="sxs-lookup"><span data-stu-id="593f4-137">Add the bookstore model to the Web API project; this model contains the Create, Read, Update, and Delete (CRUD) logic for the bookstore application:</span></span>

   1. <span data-ttu-id="593f4-138">Clique com botão direito do **modelos** pasta no Gerenciador de soluções, em seguida, clique em **adicionar**e, em seguida, clique em **classe**.</span><span class="sxs-lookup"><span data-stu-id="593f4-138">Right-click the **Models** folder in the solution explorer, then click **Add**, and then click **Class**.</span></span>
   2. <span data-ttu-id="593f4-139">Quando o **Adicionar Novo Item** caixa de diálogo for exibida, nomeie o arquivo de classe **BookDetails.cs**e, em seguida, clique em **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="593f4-139">When the **Add New Item** dialog box is displayed, name the class file **BookDetails.cs**, and then click **Add**.</span></span>
   3. <span data-ttu-id="593f4-140">Quando o **BookDetails.cs** arquivo é aberto, substitua o código no arquivo com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="593f4-140">When the **BookDetails.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample2.cs)]
   4. <span data-ttu-id="593f4-141">Salve e feche o **BookDetails.cs** arquivo.</span><span class="sxs-lookup"><span data-stu-id="593f4-141">Save and close the **BookDetails.cs** file.</span></span>

9. <span data-ttu-id="593f4-142">Adicione o controlador livraria para o projeto de API da Web:</span><span class="sxs-lookup"><span data-stu-id="593f4-142">Add the bookstore controller to the Web API project:</span></span>

   1. <span data-ttu-id="593f4-143">Com o botão direito do **controladores** pasta no Gerenciador de soluções, em seguida, clique em **adicionar**e, em seguida, clique em **controlador**.</span><span class="sxs-lookup"><span data-stu-id="593f4-143">Right-click the **Controllers** folder in the solution explorer, then click **Add**, and then click **Controller**.</span></span>
   2. <span data-ttu-id="593f4-144">Quando o **adicionar Scaffold** caixa de diálogo é exibida, realce **controlador de 2 de API da Web - vazio**e, em seguida, clique em **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="593f4-144">When the **Add Scaffold** dialog box is displayed, highlight **Web API 2 Controller - Empty**, and then click **Add**.</span></span>
   3. <span data-ttu-id="593f4-145">Quando o **Adicionar controlador** caixa de diálogo é exibida, nomeie o controlador **BooksController**e, em seguida, clique em **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="593f4-145">When the **Add Controller** dialog box is displayed, name the controller **BooksController**, and then click **Add**.</span></span>
   4. <span data-ttu-id="593f4-146">Quando o **BooksController.cs** arquivo é aberto, substitua o código no arquivo com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="593f4-146">When the **BooksController.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample3.cs)]
   5. <span data-ttu-id="593f4-147">Salve e feche o **BooksController.cs** arquivo.</span><span class="sxs-lookup"><span data-stu-id="593f4-147">Save and close the **BooksController.cs** file.</span></span>

10. <span data-ttu-id="593f4-148">O aplicativo de API da Web para verificar se há erros de compilação.</span><span class="sxs-lookup"><span data-stu-id="593f4-148">Build the Web API application to check for errors.</span></span>

<a id="STEP2"></a>
### <a name="step-2-adding-the-windows-phone-8-bookstore-catalog-project"></a><span data-ttu-id="593f4-149">Etapa 2: Adicionar o projeto de catálogo do Windows Phone 8 livraria</span><span class="sxs-lookup"><span data-stu-id="593f4-149">Step 2: Adding the Windows Phone 8 Bookstore Catalog Project</span></span>

<span data-ttu-id="593f4-150">É a próxima etapa deste cenário de ponta a ponta criar o aplicativo de catálogo para o Windows Phone 8.</span><span class="sxs-lookup"><span data-stu-id="593f4-150">The next step of this end-to-end scenario is to create the catalog application for Windows Phone 8.</span></span> <span data-ttu-id="593f4-151">Este aplicativo usará o *Windows Phone Databound App* modelo para a interface do usuário padrão e usará o aplicativo de API da Web que você criou na [etapa 1](#STEP1) deste tutorial, como a fonte de dados.</span><span class="sxs-lookup"><span data-stu-id="593f4-151">This application will use the *Windows Phone Databound App* template for the default user interface, and it will use the Web API application that you created in [Step 1](#STEP1) of this tutorial as the data source.</span></span>

1. <span data-ttu-id="593f4-152">Com o botão direito do **livraria** solução no Gerenciador de soluções, em seguida, clique em **adicionar**e, em seguida, **novo projeto**.</span><span class="sxs-lookup"><span data-stu-id="593f4-152">Right-click the **BookStore** solution in the in the solution explorer, then click **Add**, and then **New Project**.</span></span>
2. <span data-ttu-id="593f4-153">Quando o **novo projeto** caixa de diálogo é exibida, expanda **instalado**, em seguida, **Visual c#** e, em seguida, **do Windows Phone**.</span><span class="sxs-lookup"><span data-stu-id="593f4-153">When the **New Project** dialog box is displayed, expand **Installed**, then **Visual C#**, and then **Windows Phone**.</span></span>
3. <span data-ttu-id="593f4-154">Realçar **Windows Phone Databound App**, digite **BookCatalog** para o nome e depois clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="593f4-154">Highlight **Windows Phone Databound App**, enter **BookCatalog** for the name, and then click **OK**.</span></span>
4. <span data-ttu-id="593f4-155">Adicionar o pacote NuGet do Json.NET para o **BookCatalog** projeto:</span><span class="sxs-lookup"><span data-stu-id="593f4-155">Add the Json.NET NuGet package to the **BookCatalog** project:</span></span>

    1. <span data-ttu-id="593f4-156">Clique com botão direito **referências** para o **BookCatalog** projeto no Gerenciador de soluções e, em seguida, clique em **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="593f4-156">Right-click **References** for the **BookCatalog** project in the solution explorer, and then click **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="593f4-157">Quando o **gerenciar pacotes NuGet** caixa de diálogo é exibida, expanda o **Online** seção e realçar **nuget.org**.</span><span class="sxs-lookup"><span data-stu-id="593f4-157">When the **Manage NuGet Packages** dialog box is displayed, expand the **Online** section, and highlight **nuget.org**.</span></span>
    3. <span data-ttu-id="593f4-158">Digite **Json.NET** na pesquisa de campo e clique no ícone de pesquisa.</span><span class="sxs-lookup"><span data-stu-id="593f4-158">Enter **Json.NET** in the search field and click the search icon.</span></span>
    4. <span data-ttu-id="593f4-159">Realçar **Json.NET** nos resultados da pesquisa e clique **instalar**.</span><span class="sxs-lookup"><span data-stu-id="593f4-159">Highlight **Json.NET** in the search results, and then click **Install**.</span></span>
    5. <span data-ttu-id="593f4-160">Quando a instalação for concluída, clique em **fechar**.</span><span class="sxs-lookup"><span data-stu-id="593f4-160">When the installation has completed, click **Close**.</span></span>
5. <span data-ttu-id="593f4-161">Adicionar o **BookDetails** modelo para o **BookCatalog** projeto; ele contém um modelo genérico da classe livraria:</span><span class="sxs-lookup"><span data-stu-id="593f4-161">Add the **BookDetails** model to the **BookCatalog** project; this contains a generic model of the bookstore class:</span></span>

   1. <span data-ttu-id="593f4-162">Clique com botão direito do **BookCatalog** projeto no Gerenciador de soluções, clique em **adicionar**e, em seguida, clique em **nova pasta**.</span><span class="sxs-lookup"><span data-stu-id="593f4-162">Right-click the **BookCatalog** project in the solution explorer, then click **Add**, and then click **New Folder**.</span></span>
   2. <span data-ttu-id="593f4-163">Nomeie a nova pasta **modelos**.</span><span class="sxs-lookup"><span data-stu-id="593f4-163">Name the new folder **Models**.</span></span>
   3. <span data-ttu-id="593f4-164">Clique com botão direito do **modelos** pasta no Gerenciador de soluções, em seguida, clique em **adicionar**e, em seguida, clique em **classe**.</span><span class="sxs-lookup"><span data-stu-id="593f4-164">Right-click the **Models** folder in the solution explorer, then click **Add**, and then click **Class**.</span></span>
   4. <span data-ttu-id="593f4-165">Quando o **Adicionar Novo Item** caixa de diálogo for exibida, nomeie o arquivo de classe **BookDetails.cs**e, em seguida, clique em **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="593f4-165">When the **Add New Item** dialog box is displayed, name the class file **BookDetails.cs**, and then click **Add**.</span></span>
   5. <span data-ttu-id="593f4-166">Quando o **BookDetails.cs** arquivo é aberto, substitua o código no arquivo com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="593f4-166">When the **BookDetails.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample4.cs)]
   6. <span data-ttu-id="593f4-167">Salve e feche o **BookDetails.cs** arquivo.</span><span class="sxs-lookup"><span data-stu-id="593f4-167">Save and close the **BookDetails.cs** file.</span></span>

6. <span data-ttu-id="593f4-168">Atualização de **MainViewModel.cs** classe para incluir a funcionalidade para se comunicar com o aplicativo de API da Web de livraria:</span><span class="sxs-lookup"><span data-stu-id="593f4-168">Update the **MainViewModel.cs** class to include the functionality to communicate with the BookStore Web API application:</span></span>

   1. <span data-ttu-id="593f4-169">Expanda o **ViewModels** pasta no Gerenciador de soluções e, em seguida, clique duas vezes o **MainViewModel.cs** arquivo.</span><span class="sxs-lookup"><span data-stu-id="593f4-169">Expand the **ViewModels** folder in the solution explorer, and then double-click the **MainViewModel.cs** file.</span></span>
   2. <span data-ttu-id="593f4-170">Quando o **MainViewModel.cs** arquivo é aberto, substitua o código no arquivo pelo seguinte; Observe que você precisa atualizar o valor da `apiUrl` constante com a URL real da sua API da Web:</span><span class="sxs-lookup"><span data-stu-id="593f4-170">When the **MainViewModel.cs** file is opened, replace the code in the file with the following; note that you will need to update the value of the `apiUrl` constant with the actual URL of your Web API:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample5.cs)]
   3. <span data-ttu-id="593f4-171">Salve e feche o **MainViewModel.cs** arquivo.</span><span class="sxs-lookup"><span data-stu-id="593f4-171">Save and close the **MainViewModel.cs** file.</span></span>

7. <span data-ttu-id="593f4-172">Atualização de **MainPage. XAML** arquivo para personalizar o nome do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="593f4-172">Update the **MainPage.xaml** file to customize the application name:</span></span>

   1. <span data-ttu-id="593f4-173">Clique duas vezes o **MainPage. XAML** arquivo no Gerenciador de soluções.</span><span class="sxs-lookup"><span data-stu-id="593f4-173">Double-click the **MainPage.xaml** file in the solution explorer.</span></span>
   2. <span data-ttu-id="593f4-174">Quando o **MainPage. XAML** arquivo é aberto, localize as seguintes linhas de código:</span><span class="sxs-lookup"><span data-stu-id="593f4-174">When the **MainPage.xaml** file is opened, locate the following lines of code:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample6.xml)]
   3. <span data-ttu-id="593f4-175">Substitua as linhas com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="593f4-175">Replace those lines with the following:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample7.xml)]
   4. <span data-ttu-id="593f4-176">Salve e feche o **MainPage. XAML** arquivo.</span><span class="sxs-lookup"><span data-stu-id="593f4-176">Save and close the **MainPage.xaml** file.</span></span>

8. <span data-ttu-id="593f4-177">Atualização de **DetailsPage.xaml** arquivo para personalizar itens exibidos:</span><span class="sxs-lookup"><span data-stu-id="593f4-177">Update the **DetailsPage.xaml** file to customize the displayed items:</span></span>

   1. <span data-ttu-id="593f4-178">Clique duas vezes o **DetailsPage.xaml** arquivo no Gerenciador de soluções.</span><span class="sxs-lookup"><span data-stu-id="593f4-178">Double-click the **DetailsPage.xaml** file in the solution explorer.</span></span>
   2. <span data-ttu-id="593f4-179">Quando o **DetailsPage.xaml** arquivo é aberto, localize as seguintes linhas de código:</span><span class="sxs-lookup"><span data-stu-id="593f4-179">When the **DetailsPage.xaml** file is opened, locate the following lines of code:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample8.xml)]
   3. <span data-ttu-id="593f4-180">Substitua as linhas com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="593f4-180">Replace those lines with the following:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample9.xml)]
   4. <span data-ttu-id="593f4-181">Salve e feche o **DetailsPage.xaml** arquivo.</span><span class="sxs-lookup"><span data-stu-id="593f4-181">Save and close the **DetailsPage.xaml** file.</span></span>

9. <span data-ttu-id="593f4-182">O aplicativo do Windows Phone para verificar se há erros de compilação.</span><span class="sxs-lookup"><span data-stu-id="593f4-182">Build the Windows Phone application to check for errors.</span></span>

### <a name="step-3-testing-the-end-to-end-solution"></a><span data-ttu-id="593f4-183">Etapa 3: Testar a solução de ponta a ponta</span><span class="sxs-lookup"><span data-stu-id="593f4-183">Step 3: Testing the End-to-End Solution</span></span>

<span data-ttu-id="593f4-184">Conforme mencionado no *pré-requisitos* seção deste tutorial, quando você está testando a conectividade entre a API da Web e Windows Phone 8 projetos em seu sistema local, você precisará seguir as instruções no *[ Conectando o emulador do Windows Phone 8 para aplicativos de API da Web em um computador Local](https://go.microsoft.com/fwlink/?LinkId=324014)* artigo para configurar seu ambiente de teste.</span><span class="sxs-lookup"><span data-stu-id="593f4-184">As mentioned in the *Prerequisites* section of this tutorial, when you are testing the connectivity between Web API and Windows Phone 8 projects on your local system, you will need to follow the instructions in the *[Connecting the Windows Phone 8 Emulator to Web API Applications on a Local Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* article to set up your testing environment.</span></span>

<span data-ttu-id="593f4-185">Depois que você tiver configurado o ambiente de teste, você precisará definir o aplicativo do Windows Phone como projeto de inicialização.</span><span class="sxs-lookup"><span data-stu-id="593f4-185">Once you have the testing environment configured, you will need to set the Windows Phone application as the startup project.</span></span> <span data-ttu-id="593f4-186">Para fazer isso, realce o **BookCatalog** aplicativo no Gerenciador de soluções e clique **definir como projeto de inicialização**:</span><span class="sxs-lookup"><span data-stu-id="593f4-186">To do so, highlight the **BookCatalog** application in the solution explorer, and then click **Set as StartUp Project**:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image6.png)](calling-web-api-from-a-windows-phone-8-application/_static/image5.png) |
| --- |
| <span data-ttu-id="593f4-187">Clique na imagem para expandir</span><span class="sxs-lookup"><span data-stu-id="593f4-187">Click image to expand</span></span> |

<span data-ttu-id="593f4-188">Quando você pressionar F5, o Visual Studio iniciará ambos os Windows Phone emulador, que exibe um &quot;Aguarde&quot; mensagem enquanto os dados de aplicativo são recuperados da sua API da Web:</span><span class="sxs-lookup"><span data-stu-id="593f4-188">When you press F5, Visual Studio will start both the Windows Phone Emulator, which will display a &quot;Please Wait&quot; message while the application data is retrieved from your Web API:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image8.png)](calling-web-api-from-a-windows-phone-8-application/_static/image7.png) |
| --- |
| <span data-ttu-id="593f4-189">Clique na imagem para expandir</span><span class="sxs-lookup"><span data-stu-id="593f4-189">Click image to expand</span></span> |

<span data-ttu-id="593f4-190">Se tudo for bem-sucedida, você deve ver o catálogo exibido:</span><span class="sxs-lookup"><span data-stu-id="593f4-190">If everything is successful, you should see the catalog displayed:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image10.png)](calling-web-api-from-a-windows-phone-8-application/_static/image9.png) |
| --- |
| <span data-ttu-id="593f4-191">Clique na imagem para expandir</span><span class="sxs-lookup"><span data-stu-id="593f4-191">Click image to expand</span></span> |

<span data-ttu-id="593f4-192">Se você tocar em qualquer título de livro, o aplicativo exibirá a descrição de catálogo:</span><span class="sxs-lookup"><span data-stu-id="593f4-192">If you tap on any book title, the application will display the book description:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image12.png)](calling-web-api-from-a-windows-phone-8-application/_static/image11.png) |
| --- |
| <span data-ttu-id="593f4-193">Clique na imagem para expandir</span><span class="sxs-lookup"><span data-stu-id="593f4-193">Click image to expand</span></span> |

<span data-ttu-id="593f4-194">Se o aplicativo não pode se comunicar com a API da Web, será exibida uma mensagem de erro:</span><span class="sxs-lookup"><span data-stu-id="593f4-194">If the application cannot communicate with your Web API, an error message will be displayed:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image14.png)](calling-web-api-from-a-windows-phone-8-application/_static/image13.png) |
| --- |
| <span data-ttu-id="593f4-195">Clique na imagem para expandir</span><span class="sxs-lookup"><span data-stu-id="593f4-195">Click image to expand</span></span> |

<span data-ttu-id="593f4-196">Se você tocar na mensagem de erro, todos os detalhes adicionais sobre o erro serão exibidos:</span><span class="sxs-lookup"><span data-stu-id="593f4-196">If you tap on the error message, any additional details about the error will be displayed:</span></span>


| [![](calling-web-api-from-a-windows-phone-8-application/_static/image16.png)](calling-web-api-from-a-windows-phone-8-application/_static/image15.png) |
|-------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                                 <span data-ttu-id="593f4-197">Clique na imagem para expandir</span><span class="sxs-lookup"><span data-stu-id="593f4-197">Click image to expand</span></span>                                                                 |

