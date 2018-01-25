---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-3
title: "Use as migrações do Code First para propagar o banco de dados | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 76e2013a-65b7-488c-834d-9448ecea378e
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-3
msc.type: authoredcontent
ms.openlocfilehash: 1ca627397f0f100d13388f9afc27ff481886e098
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="use-code-first-migrations-to-seed-the-database"></a><span data-ttu-id="5b7c8-102">Use as migrações do Code First para propagar o banco de dados</span><span class="sxs-lookup"><span data-stu-id="5b7c8-102">Use Code First Migrations to Seed the Database</span></span>
====================
<span data-ttu-id="5b7c8-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="5b7c8-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="5b7c8-104">Baixe o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="5b7c8-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="5b7c8-105">Nesta seção, você usará [migrações do Code First](https://msdn.microsoft.com/data/jj591621) no EF para propagar o banco de dados de teste.</span><span class="sxs-lookup"><span data-stu-id="5b7c8-105">In this section, you will use [Code First Migrations](https://msdn.microsoft.com/data/jj591621) in EF to seed the database with test data.</span></span>

<span data-ttu-id="5b7c8-106">Do **ferramentas** menu, selecione **Gerenciador de biblioteca de pacote**, em seguida, selecione **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="5b7c8-106">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="5b7c8-107">Na janela do Console do Gerenciador de pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="5b7c8-107">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](part-3/samples/sample1.cmd)]

<span data-ttu-id="5b7c8-108">Este comando adiciona uma pasta chamada migrações ao seu projeto, além de um arquivo de código chamado Configuration.cs na pasta de migrações.</span><span class="sxs-lookup"><span data-stu-id="5b7c8-108">This command adds a folder named Migrations to your project, plus a code file named Configuration.cs in the Migrations folder.</span></span>

![](part-3/_static/image1.png)

<span data-ttu-id="5b7c8-109">Abra o arquivo Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="5b7c8-109">Open the Configuration.cs file.</span></span> <span data-ttu-id="5b7c8-110">Adicione o seguinte **usando** instrução.</span><span class="sxs-lookup"><span data-stu-id="5b7c8-110">Add the following **using** statement.</span></span>

[!code-csharp[Main](part-3/samples/sample2.cs)]

<span data-ttu-id="5b7c8-111">Em seguida, adicione o seguinte código para o **Configuration.Seed** método:</span><span class="sxs-lookup"><span data-stu-id="5b7c8-111">Then add the following code to the **Configuration.Seed** method:</span></span>

[!code-csharp[Main](part-3/samples/sample3.cs)]

<span data-ttu-id="5b7c8-112">Na janela do Console do Gerenciador de pacotes, digite os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="5b7c8-112">In the Package Manager Console window, type the following commands:</span></span>

[!code-console[Main](part-3/samples/sample4.cmd)]

<span data-ttu-id="5b7c8-113">O primeiro comando gera o código que cria o banco de dados, e o segundo comando executa esse código.</span><span class="sxs-lookup"><span data-stu-id="5b7c8-113">The first command generates code that creates the database, and the second command executes that code.</span></span> <span data-ttu-id="5b7c8-114">O banco de dados é criado localmente, usando [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx).</span><span class="sxs-lookup"><span data-stu-id="5b7c8-114">The database is created locally, using [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx).</span></span>

![](part-3/_static/image2.png)

## <a name="explore-the-api-optional"></a><span data-ttu-id="5b7c8-115">Explorar a API (opcional)</span><span class="sxs-lookup"><span data-stu-id="5b7c8-115">Explore the API (Optional)</span></span>

<span data-ttu-id="5b7c8-116">Pressione F5 para executar o aplicativo no modo de depuração.</span><span class="sxs-lookup"><span data-stu-id="5b7c8-116">Press F5 to run the application in debug mode.</span></span> <span data-ttu-id="5b7c8-117">O Visual Studio inicia o IIS Express e executa seu aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="5b7c8-117">Visual Studio starts IIS Express and runs your web app.</span></span> <span data-ttu-id="5b7c8-118">Visual Studio, em seguida, inicia um navegador e abre a home page do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5b7c8-118">Visual Studio then launches a browser and opens the app's home page.</span></span>

<span data-ttu-id="5b7c8-119">Quando o Visual Studio executará um projeto da web, ele atribui um número de porta.</span><span class="sxs-lookup"><span data-stu-id="5b7c8-119">When Visual Studio runs a web project, it assigns a port number.</span></span> <span data-ttu-id="5b7c8-120">Na imagem abaixo, o número da porta é 50524.</span><span class="sxs-lookup"><span data-stu-id="5b7c8-120">In the image below, the port number is 50524.</span></span> <span data-ttu-id="5b7c8-121">Quando você executa o aplicativo, você verá um número de porta diferente.</span><span class="sxs-lookup"><span data-stu-id="5b7c8-121">When you run the application, you'll see a different port number.</span></span>

![](part-3/_static/image3.png)

<span data-ttu-id="5b7c8-122">A home page é implementada usando o ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="5b7c8-122">The home page is implemented using ASP.NET MVC.</span></span> <span data-ttu-id="5b7c8-123">Na parte superior da página, há um link que diz "API".</span><span class="sxs-lookup"><span data-stu-id="5b7c8-123">At the top of the page, there is a link that says "API".</span></span> <span data-ttu-id="5b7c8-124">Este link traz uma página de ajuda gerada automaticamente para a API da web.</span><span class="sxs-lookup"><span data-stu-id="5b7c8-124">This link brings you to an auto-generated help page for the web API.</span></span> <span data-ttu-id="5b7c8-125">(Para saber como esta página de Ajuda é gerada e como você pode adicionar sua própria documentação para a página, consulte [criando páginas de ajuda para a ASP.NET Web API](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) Você pode clicar no menu Ajuda links da página para ver os detalhes sobre a API, incluindo o formato de solicitação e resposta.</span><span class="sxs-lookup"><span data-stu-id="5b7c8-125">(To learn how this help page is generated, and how you can add your own documentation to the page, see [Creating Help Pages for ASP.NET Web API](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) You can click on the help page links to see details about the API, including the request and response format.</span></span>

![](part-3/_static/image4.png)

<span data-ttu-id="5b7c8-126">A API permite operações CRUD no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="5b7c8-126">The API enables CRUD operations on the database.</span></span> <span data-ttu-id="5b7c8-127">A seguir resume a API.</span><span class="sxs-lookup"><span data-stu-id="5b7c8-127">The following summarizes the API.</span></span>

| <span data-ttu-id="5b7c8-128">Autores</span><span class="sxs-lookup"><span data-stu-id="5b7c8-128">Authors</span></span> |  |
| --- | -- |
| <span data-ttu-id="5b7c8-129">OBTER api/autores</span><span class="sxs-lookup"><span data-stu-id="5b7c8-129">GET api/authors</span></span> | <span data-ttu-id="5b7c8-130">Obter todos os autores.</span><span class="sxs-lookup"><span data-stu-id="5b7c8-130">Get all authors.</span></span> |
| <span data-ttu-id="5b7c8-131">Api GET/autores / {id}</span><span class="sxs-lookup"><span data-stu-id="5b7c8-131">GET api/authors/{id}</span></span> | <span data-ttu-id="5b7c8-132">Obter um autor por ID.</span><span class="sxs-lookup"><span data-stu-id="5b7c8-132">Get an author by ID.</span></span> |
| <span data-ttu-id="5b7c8-133">Autores/api/POST</span><span class="sxs-lookup"><span data-stu-id="5b7c8-133">POST /api/authors</span></span> | <span data-ttu-id="5b7c8-134">Crie um novo autor.</span><span class="sxs-lookup"><span data-stu-id="5b7c8-134">Create a new author.</span></span> |
| <span data-ttu-id="5b7c8-135">COLOCAR /api autores / {id}</span><span class="sxs-lookup"><span data-stu-id="5b7c8-135">PUT /api/authors/{id}</span></span> | <span data-ttu-id="5b7c8-136">Atualize um autor existente.</span><span class="sxs-lookup"><span data-stu-id="5b7c8-136">Update an existing author.</span></span> |
| <span data-ttu-id="5b7c8-137">Excluir /api autores / {id}</span><span class="sxs-lookup"><span data-stu-id="5b7c8-137">DELETE /api/authors/{id}</span></span> | <span data-ttu-id="5b7c8-138">Exclua um autor.</span><span class="sxs-lookup"><span data-stu-id="5b7c8-138">Delete an author.</span></span> |

| <span data-ttu-id="5b7c8-139">Livros</span><span class="sxs-lookup"><span data-stu-id="5b7c8-139">Books</span></span> |  |
| --- | -- |
| <span data-ttu-id="5b7c8-140">OBTER /api/books</span><span class="sxs-lookup"><span data-stu-id="5b7c8-140">GET /api/books</span></span> | <span data-ttu-id="5b7c8-141">Obter todos os livros.</span><span class="sxs-lookup"><span data-stu-id="5b7c8-141">Get all books.</span></span> |
| <span data-ttu-id="5b7c8-142">OBTER /api manuais / {id}</span><span class="sxs-lookup"><span data-stu-id="5b7c8-142">GET /api/books/{id}</span></span> | <span data-ttu-id="5b7c8-143">Obter um livro por ID.</span><span class="sxs-lookup"><span data-stu-id="5b7c8-143">Get a book by ID.</span></span> |
| <span data-ttu-id="5b7c8-144">Enviar/api/catálogos</span><span class="sxs-lookup"><span data-stu-id="5b7c8-144">POST /api/books</span></span> | <span data-ttu-id="5b7c8-145">Crie um novo catálogo.</span><span class="sxs-lookup"><span data-stu-id="5b7c8-145">Create a new book.</span></span> |
| <span data-ttu-id="5b7c8-146">COLOCAR /api manuais / {id}</span><span class="sxs-lookup"><span data-stu-id="5b7c8-146">PUT /api/books/{id}</span></span> | <span data-ttu-id="5b7c8-147">Atualize um catálogo existente.</span><span class="sxs-lookup"><span data-stu-id="5b7c8-147">Update an existing book.</span></span> |
| <span data-ttu-id="5b7c8-148">Excluir /api manuais / {id}</span><span class="sxs-lookup"><span data-stu-id="5b7c8-148">DELETE /api/books/{id}</span></span> | <span data-ttu-id="5b7c8-149">Exclua um catálogo.</span><span class="sxs-lookup"><span data-stu-id="5b7c8-149">Delete a book.</span></span> |

## <a name="view-the-database-optional"></a><span data-ttu-id="5b7c8-150">Exibir o banco de dados (opcional)</span><span class="sxs-lookup"><span data-stu-id="5b7c8-150">View the Database (Optional)</span></span>

<span data-ttu-id="5b7c8-151">Quando você executou o comando Update-Database, EF criou o banco de dados e a chamada a `Seed` método.</span><span class="sxs-lookup"><span data-stu-id="5b7c8-151">When you ran the Update-Database command, EF created the database and called the `Seed` method.</span></span> <span data-ttu-id="5b7c8-152">Quando você executar o aplicativo localmente, usa EF [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx).</span><span class="sxs-lookup"><span data-stu-id="5b7c8-152">When you run the application locally, EF uses [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx).</span></span> <span data-ttu-id="5b7c8-153">Você pode exibir o banco de dados no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5b7c8-153">You can view the database in Visual Studio.</span></span> <span data-ttu-id="5b7c8-154">Do **exibição** menu, selecione **Pesquisador de objetos do SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="5b7c8-154">From the **View** menu, select **SQL Server Object Explorer**.</span></span>

![](part-3/_static/image5.png)

<span data-ttu-id="5b7c8-155">No **conectar ao servidor** caixa de diálogo, no **nome do servidor** caixa de edição, digite "\v11.0 (localdb)".</span><span class="sxs-lookup"><span data-stu-id="5b7c8-155">In the **Connect to Server** dialog, in the **Server Name** edit box, type "(localdb)\v11.0".</span></span> <span data-ttu-id="5b7c8-156">Deixe o **autenticação** opção como "Autenticação do Windows".</span><span class="sxs-lookup"><span data-stu-id="5b7c8-156">Leave the **Authentication** option as "Windows Authentication".</span></span> <span data-ttu-id="5b7c8-157">Clique em **Conectar**.</span><span class="sxs-lookup"><span data-stu-id="5b7c8-157">Click **Connect**.</span></span>

![](part-3/_static/image6.png)

<span data-ttu-id="5b7c8-158">Visual Studio se conecta ao LocalDB e mostra os bancos de dados existentes na janela do Pesquisador de objetos do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="5b7c8-158">Visual Studio connects to LocalDB and shows your existing databases in the SQL Server Object Explorer window.</span></span> <span data-ttu-id="5b7c8-159">Você pode expandir os nós para ver as tabelas que EF criada.</span><span class="sxs-lookup"><span data-stu-id="5b7c8-159">You can expand the nodes to see the tables that EF created.</span></span>

![](part-3/_static/image7.png)

<span data-ttu-id="5b7c8-160">Para exibir os dados, clique em uma tabela e selecione **exibir dados**.</span><span class="sxs-lookup"><span data-stu-id="5b7c8-160">To view the data, right-click a table and select **View Data**.</span></span>

![](part-3/_static/image8.png)

<span data-ttu-id="5b7c8-161">Captura de tela a seguir mostra os resultados da tabela livros.</span><span class="sxs-lookup"><span data-stu-id="5b7c8-161">The following screenshot shows the results for the Books table.</span></span> <span data-ttu-id="5b7c8-162">Observe que o EF populado o banco de dados com os dados de semente, e a tabela contém a chave estrangeira para a tabela de autores.</span><span class="sxs-lookup"><span data-stu-id="5b7c8-162">Notice that EF populated the database with the seed data, and the table contains the foreign key to the Authors table.</span></span>

![](part-3/_static/image9.png)

>[!div class="step-by-step"]
<span data-ttu-id="5b7c8-163">[Anterior](part-2.md)
[Próximo](part-4.md)</span><span class="sxs-lookup"><span data-stu-id="5b7c8-163">[Previous](part-2.md)
[Next](part-4.md)</span></span>
