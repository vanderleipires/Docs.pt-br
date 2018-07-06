---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-2
title: Adicionar modelos e controladores | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: 88908ff8-51a9-40eb-931c-a8139128b680
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-2
msc.type: authoredcontent
ms.openlocfilehash: 31e0fbf65c74a18677588cfa412dc34f7b25f78f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37806352"
---
<a name="add-models-and-controllers"></a><span data-ttu-id="0583f-102">Adicionar modelos e controladores</span><span class="sxs-lookup"><span data-stu-id="0583f-102">Add Models and Controllers</span></span>
====================
<span data-ttu-id="0583f-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="0583f-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="0583f-104">Baixe o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="0583f-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="0583f-105">Nesta seção, você irá adicionar classes de modelo que definem as entidades de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="0583f-105">In this section, you will add model classes that define the database entities.</span></span> <span data-ttu-id="0583f-106">Em seguida, você irá adicionar controladores da API Web que executam operações CRUD nessas entidades.</span><span class="sxs-lookup"><span data-stu-id="0583f-106">Then you will add Web API controllers that perform CRUD operations on those entities.</span></span>

## <a name="add-model-classes"></a><span data-ttu-id="0583f-107">Adicionar Classes de modelo</span><span class="sxs-lookup"><span data-stu-id="0583f-107">Add Model Classes</span></span>

<span data-ttu-id="0583f-108">Neste tutorial, vamos criar o banco de dados usando a abordagem de "Code First" para o Entity Framework (EF).</span><span class="sxs-lookup"><span data-stu-id="0583f-108">In this tutorial, we'll create the database by using the "Code First" approach to Entity Framework (EF).</span></span> <span data-ttu-id="0583f-109">Com o Code First, escrever classes em c# que correspondem às tabelas de banco de dados e o EF cria o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="0583f-109">With Code First, you write C# classes that correspond to database tables, and EF creates the database.</span></span> <span data-ttu-id="0583f-110">(Para obter mais informações, consulte [abordagens de desenvolvimento do Entity Framework](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)</span><span class="sxs-lookup"><span data-stu-id="0583f-110">(For more information, see [Entity Framework Development Approaches](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)</span></span>

<span data-ttu-id="0583f-111">Vamos começar definindo nossos objetos de domínio como POCOs (objetos CLR do BOM e velho).</span><span class="sxs-lookup"><span data-stu-id="0583f-111">We start by defining our domain objects as POCOs (plain-old CLR objects).</span></span> <span data-ttu-id="0583f-112">Vamos criar POCOs a seguir:</span><span class="sxs-lookup"><span data-stu-id="0583f-112">We will create the following POCOs:</span></span>

- <span data-ttu-id="0583f-113">Autor</span><span class="sxs-lookup"><span data-stu-id="0583f-113">Author</span></span>
- <span data-ttu-id="0583f-114">Livro</span><span class="sxs-lookup"><span data-stu-id="0583f-114">Book</span></span>

<span data-ttu-id="0583f-115">No Gerenciador de soluções, clique com botão direito na pasta modelos.</span><span class="sxs-lookup"><span data-stu-id="0583f-115">In Solution Explorer, right click the Models folder.</span></span> <span data-ttu-id="0583f-116">Selecione **Add**, em seguida, selecione **classe**.</span><span class="sxs-lookup"><span data-stu-id="0583f-116">Select **Add**, then select **Class**.</span></span> <span data-ttu-id="0583f-117">Nomeie a classe `Author`.</span><span class="sxs-lookup"><span data-stu-id="0583f-117">Name the class `Author`.</span></span>

![](part-2/_static/image1.png)

<span data-ttu-id="0583f-118">Substitua todo o código clichê em Author.cs com o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="0583f-118">Replace all of the boilerplate code in Author.cs with the following code.</span></span>

[!code-csharp[Main](part-2/samples/sample1.cs)]

<span data-ttu-id="0583f-119">Adicione outra classe denominada `Book`, com o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="0583f-119">Add another class named `Book`, with the following code.</span></span>

[!code-csharp[Main](part-2/samples/sample2.cs)]

<span data-ttu-id="0583f-120">Entity Framework usará esses modelos para criar tabelas de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="0583f-120">Entity Framework will use these models to create database tables.</span></span> <span data-ttu-id="0583f-121">Para cada modelo, o `Id` propriedade tornam-se a coluna de chave primária da tabela de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="0583f-121">For each model, the `Id` property will become the primary key column of the database table.</span></span>

<span data-ttu-id="0583f-122">Na classe do livro, o `AuthorId` define uma chave estrangeira para a `Author` tabela.</span><span class="sxs-lookup"><span data-stu-id="0583f-122">In the Book class, the `AuthorId` defines a foreign key into the `Author` table.</span></span> <span data-ttu-id="0583f-123">(Para simplificar, estou pressupondo que cada livro tem o autor de um único.) A classe de livro também contém uma propriedade de navegação para o relacionado `Author`.</span><span class="sxs-lookup"><span data-stu-id="0583f-123">(For simplicity, I'm assuming that each book has a single author.) The book class also contains a navigation property to the related `Author`.</span></span> <span data-ttu-id="0583f-124">Você pode usar a propriedade de navegação para acessar o relacionados `Author` no código.</span><span class="sxs-lookup"><span data-stu-id="0583f-124">You can use the navigation property to access the related `Author` in code.</span></span> <span data-ttu-id="0583f-125">Posso falar mais sobre as propriedades de navegação na parte 4, [tratamento de relações de entidade](part-4.md).</span><span class="sxs-lookup"><span data-stu-id="0583f-125">I say more about navigation properties in part 4, [Handling Entity Relations](part-4.md).</span></span>

## <a name="add-web-api-controllers"></a><span data-ttu-id="0583f-126">Adicionar controladores de API da Web</span><span class="sxs-lookup"><span data-stu-id="0583f-126">Add Web API Controllers</span></span>

<span data-ttu-id="0583f-127">Nesta seção, vamos adicionar controladores da API Web que dão suporte a operações CRUD (criar, ler, atualizar e excluir).</span><span class="sxs-lookup"><span data-stu-id="0583f-127">In this section, we'll add Web API controllers that support CRUD operations (create, read, update, and delete).</span></span> <span data-ttu-id="0583f-128">Os controladores usará o Entity Framework para se comunicar com a camada de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="0583f-128">The controllers will use Entity Framework to communicate with the database layer.</span></span>

<span data-ttu-id="0583f-129">Primeiro, você pode excluir o arquivo Controllers/ValuesController.cs.</span><span class="sxs-lookup"><span data-stu-id="0583f-129">First, you can delete the file Controllers/ValuesController.cs.</span></span> <span data-ttu-id="0583f-130">Esse arquivo contém um controlador de API Web de exemplo, mas ele não é necessário para este tutorial.</span><span class="sxs-lookup"><span data-stu-id="0583f-130">This file contains an example Web API controller, but you don't need it for this tutorial.</span></span>

![](part-2/_static/image2.png)

<span data-ttu-id="0583f-131">Em seguida, compile o projeto.</span><span class="sxs-lookup"><span data-stu-id="0583f-131">Next, build the project.</span></span> <span data-ttu-id="0583f-132">O scaffolding de API da Web usa a reflexão para encontrar as classes de modelo, portanto, ele precisa que o assembly compilado.</span><span class="sxs-lookup"><span data-stu-id="0583f-132">The Web API scaffolding uses reflection to find the model classes, so it needs the compiled assembly.</span></span>

<span data-ttu-id="0583f-133">No Gerenciador de soluções, clique com botão direito na pasta controladores.</span><span class="sxs-lookup"><span data-stu-id="0583f-133">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="0583f-134">Selecione **Add**, em seguida, selecione **controlador**.</span><span class="sxs-lookup"><span data-stu-id="0583f-134">Select **Add**, then select **Controller**.</span></span>

![](part-2/_static/image3.png)

<span data-ttu-id="0583f-135">No **adicionar Scaffold** caixa de diálogo, selecione "Web API 2 Controller com ações, usando o Entity Framework".</span><span class="sxs-lookup"><span data-stu-id="0583f-135">In the **Add Scaffold** dialog, select "Web API 2 Controller with actions, using Entity Framework".</span></span> <span data-ttu-id="0583f-136">Clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="0583f-136">Click **Add**.</span></span>

![](part-2/_static/image4.png)

<span data-ttu-id="0583f-137">No **Adicionar controlador** caixa de diálogo, faça o seguinte:</span><span class="sxs-lookup"><span data-stu-id="0583f-137">In the **Add Controller** dialog, do the following:</span></span>

1. <span data-ttu-id="0583f-138">No **classe de modelo** lista suspensa, selecione o `Author` classe.</span><span class="sxs-lookup"><span data-stu-id="0583f-138">In the **Model class** dropdown, select the `Author` class.</span></span> <span data-ttu-id="0583f-139">(Se você não vi-lo listado na lista suspensa, verifique se que você compilou o projeto.)</span><span class="sxs-lookup"><span data-stu-id="0583f-139">(If you don't see it listed in the dropdown, make sure that you built the project.)</span></span>
2. <span data-ttu-id="0583f-140">Verifique se "Use async controller actions".</span><span class="sxs-lookup"><span data-stu-id="0583f-140">Check "Use async controller actions".</span></span>
3. <span data-ttu-id="0583f-141">Deixe o nome do controlador como &quot;AuthorsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="0583f-141">Leave the controller name as &quot;AuthorsController&quot;.</span></span>
4. <span data-ttu-id="0583f-142">Clique em mais (+) ao lado de botão **classe de contexto de dados**.</span><span class="sxs-lookup"><span data-stu-id="0583f-142">Click plus (+) button next to **Data Context Class**.</span></span>

![](part-2/_static/image5.png)

<span data-ttu-id="0583f-143">No **novo contexto de dados** caixa de diálogo, deixe o nome padrão e clique em **Add**.</span><span class="sxs-lookup"><span data-stu-id="0583f-143">In the **New Data Context** dialog, leave the default name and click **Add**.</span></span>

![](part-2/_static/image6.png)

<span data-ttu-id="0583f-144">Clique em **Add** para concluir a **Adicionar controlador** caixa de diálogo.</span><span class="sxs-lookup"><span data-stu-id="0583f-144">Click **Add** to complete the **Add Controller** dialog.</span></span> <span data-ttu-id="0583f-145">A caixa de diálogo adiciona duas classes ao seu projeto:</span><span class="sxs-lookup"><span data-stu-id="0583f-145">The dialog adds two classes to your project:</span></span>

- <span data-ttu-id="0583f-146">`AuthorsController` define um controlador de API da Web.</span><span class="sxs-lookup"><span data-stu-id="0583f-146">`AuthorsController` defines a Web API controller.</span></span> <span data-ttu-id="0583f-147">O controlador implementa a API REST que os clientes usam para executar operações CRUD, na lista de autores.</span><span class="sxs-lookup"><span data-stu-id="0583f-147">The controller implements the REST API that clients use to perform CRUD operations on the list of authors.</span></span>
- <span data-ttu-id="0583f-148">`BookServiceContext` gerencia objetos de entidade durante o tempo de execução, que inclui a população de objetos com dados de um banco de dados, o controle de alterações e a persistência de dados para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="0583f-148">`BookServiceContext` manages entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span> <span data-ttu-id="0583f-149">Herda de `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="0583f-149">It inherits from `DbContext`.</span></span>

![](part-2/_static/image7.png)

<span data-ttu-id="0583f-150">Neste ponto, compile o projeto novamente.</span><span class="sxs-lookup"><span data-stu-id="0583f-150">At this point, build the project again.</span></span> <span data-ttu-id="0583f-151">Agora, percorra as mesmas etapas para adicionar um controlador de API para `Book` entidades.</span><span class="sxs-lookup"><span data-stu-id="0583f-151">Now go through the same steps to add an API controller for `Book` entities.</span></span> <span data-ttu-id="0583f-152">Desta vez, selecione `Book` para a classe de modelo e selecione existente `BookServiceContext` classe para a classe de contexto de dados.</span><span class="sxs-lookup"><span data-stu-id="0583f-152">This time, select `Book` for the model class, and select the existing `BookServiceContext` class for the data context class.</span></span> <span data-ttu-id="0583f-153">(Não crie um novo contexto de dados). Clique em **adicionar** para adicionar o controlador.</span><span class="sxs-lookup"><span data-stu-id="0583f-153">(Don't create a new data context.) Click **Add** to add the controller.</span></span>

![](part-2/_static/image8.png)

> [!div class="step-by-step"]
> <span data-ttu-id="0583f-154">[Anterior](part-1.md)
> [Próximo](part-3.md)</span><span class="sxs-lookup"><span data-stu-id="0583f-154">[Previous](part-1.md)
[Next](part-3.md)</span></span>
