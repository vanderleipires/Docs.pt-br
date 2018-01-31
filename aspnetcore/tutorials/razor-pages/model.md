---
title: "Adicionando um modelo a um aplicativo de Páginas do Razor no ASP.NET Core"
author: rick-anderson
description: "Adicionando um modelo a um aplicativo de Páginas do Razor no ASP.NET Core"
manager: wpickett
ms.author: riande
ms.date: 07/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/model
ms.openlocfilehash: 0ce7693bfdc37d930488304b329dbcd533a5ec1d
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="adding-a-model-to-a-razor-pages-app"></a><span data-ttu-id="14dcf-103">Adicionando um modelo a um aplicativo de Páginas do Razor</span><span class="sxs-lookup"><span data-stu-id="14dcf-103">Adding a model to a Razor Pages app</span></span>

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="14dcf-104">Adicionar um modelo de dados</span><span class="sxs-lookup"><span data-stu-id="14dcf-104">Add a data model</span></span>

<span data-ttu-id="14dcf-105">No Gerenciador de Soluções, clique com o botão direito do mouse no projeto **RazorPagesMovie** > **Adicionar** > **Nova Pasta**.</span><span class="sxs-lookup"><span data-stu-id="14dcf-105">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="14dcf-106">Nomeie a pasta *Models*.</span><span class="sxs-lookup"><span data-stu-id="14dcf-106">Name the folder *Models*.</span></span>

<span data-ttu-id="14dcf-107">Clique com o botão direito do mouse na pasta *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="14dcf-107">Right click the *Models* folder.</span></span> <span data-ttu-id="14dcf-108">Selecione **Adicionar** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="14dcf-108">Select **Add** > **Class**.</span></span> <span data-ttu-id="14dcf-109">Nomeie a classe **Movie** e adicione as seguintes propriedades:</span><span class="sxs-lookup"><span data-stu-id="14dcf-109">Name the class **Movie** and add the following properties:</span></span>

[!INCLUDE[model 2](../../includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="14dcf-110">Adicionar uma cadeia de conexão de banco de dados</span><span class="sxs-lookup"><span data-stu-id="14dcf-110">Add a database connection string</span></span>

<span data-ttu-id="14dcf-111">Adicione uma cadeia de conexão ao arquivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="14dcf-111">Add a connection string to the *appsettings.json* file.</span></span>

[!code-json[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="14dcf-112">Registrar o contexto do banco de dados</span><span class="sxs-lookup"><span data-stu-id="14dcf-112">Register the database context</span></span>

<span data-ttu-id="14dcf-113">Registre o contexto do banco de dados com o contêiner de [injeção de dependência](xref:fundamentals/dependency-injection) no arquivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="14dcf-113">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the *Startup.cs* file.</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]

<span data-ttu-id="14dcf-114">Compile o projeto para verificar se não há erros.</span><span class="sxs-lookup"><span data-stu-id="14dcf-114">Build the project to verify you don't have any errors.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="14dcf-115">Adicionar ferramentas de scaffold e executar a migração inicial</span><span class="sxs-lookup"><span data-stu-id="14dcf-115">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="14dcf-116">Nesta seção, você deve usar o PMC (Console de Gerenciador de Pacotes) para:</span><span class="sxs-lookup"><span data-stu-id="14dcf-116">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="14dcf-117">Adicione o pacote de geração de código da Web do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="14dcf-117">Add the Visual Studio web code generation package.</span></span> <span data-ttu-id="14dcf-118">Esse pacote é necessário para executar o mecanismo de scaffolding.</span><span class="sxs-lookup"><span data-stu-id="14dcf-118">This package is required to run the scaffolding engine.</span></span>
* <span data-ttu-id="14dcf-119">Adicionar uma migração inicial.</span><span class="sxs-lookup"><span data-stu-id="14dcf-119">Add an initial migration.</span></span>
* <span data-ttu-id="14dcf-120">Atualize o banco de dados com a migração inicial.</span><span class="sxs-lookup"><span data-stu-id="14dcf-120">Update the database with the initial migration.</span></span>

<span data-ttu-id="14dcf-121">No menu **Ferramentas**, selecione **Gerenciador de pacotes NuGet** > **Console do Gerenciador de pacotes**.</span><span class="sxs-lookup"><span data-stu-id="14dcf-121">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu do PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="14dcf-123">No PMC, digite os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="14dcf-123">In the PMC, enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Add-Migration Initial
Update-Database
```

<span data-ttu-id="14dcf-124">O comando `Install-Package` instala as ferramentas necessárias para executar o mecanismo de scaffolding.</span><span class="sxs-lookup"><span data-stu-id="14dcf-124">The `Install-Package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="14dcf-125">O comando `Add-Migration` gera código para criar o esquema de banco de dados inicial.</span><span class="sxs-lookup"><span data-stu-id="14dcf-125">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="14dcf-126">O esquema é baseado no modelo especificado no `DbContext` (no arquivo *Models/MovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="14dcf-126">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="14dcf-127">O argumento `Initial` é usado para nomear as migrações.</span><span class="sxs-lookup"><span data-stu-id="14dcf-127">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="14dcf-128">Você pode usar qualquer nome, mas, por convenção, escolha um nome que descreve a migração.</span><span class="sxs-lookup"><span data-stu-id="14dcf-128">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="14dcf-129">Consulte [Introdução às migrações](xref:data/ef-mvc/migrations#introduction-to-migrations) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="14dcf-129">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="14dcf-130">O comando `Update-Database` executa o método `Up` no arquivo *Migrations/\<time-stamp>_InitialCreate.cs*, que cria o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="14dcf-130">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE[model 4windows](../../includes/RP/model4Win.md)]

[!INCLUDE[model 4](../../includes/RP/model4tbl.md)]

<a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="14dcf-131">Testar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="14dcf-131">Test the app</span></span>

* <span data-ttu-id="14dcf-132">Executar o aplicativo e acrescentar `/Movies` à URL no navegador (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="14dcf-132">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="14dcf-133">Teste o link **Criar**.</span><span class="sxs-lookup"><span data-stu-id="14dcf-133">Test the **Create** link.</span></span>

 ![Criar página](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="14dcf-135">Teste os links **Editar**, **Detalhes** e **Excluir**.</span><span class="sxs-lookup"><span data-stu-id="14dcf-135">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="14dcf-136">Se você receber uma exceção SQL, verifique se você executou migrações e atualizou o banco de dados:</span><span class="sxs-lookup"><span data-stu-id="14dcf-136">If you get a SQL exception, verify you have run migrations and updated the database:</span></span>

<span data-ttu-id="14dcf-137">O tutorial a seguir explica os arquivos criados por scaffolding.</span><span class="sxs-lookup"><span data-stu-id="14dcf-137">The next tutorial explains the files created by scaffolding.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="14dcf-138">[Anterior: Introdução](xref:tutorials/razor-pages/razor-pages-start)
[Próximo: Páginas do Razor geradas por scaffolding](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="14dcf-138">[Previous: Getting Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>    
