---
title: "Adicionando um modelo a um aplicativo de Páginas do Razor no ASP.NET Core"
author: rick-anderson
description: "Adicionando um modelo a um aplicativo de Páginas do Razor no ASP.NET Core"
keywords: "ASP.NET Core, Páginas do Razor, Razor, MVC"
ms.author: riande
manager: wpickett
ms.date: 07/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/modelz
ms.openlocfilehash: bd2c3e09366e228fade69f843849e7452196c475
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/28/2017
---
# <a name="adding-a-model-to-a-razor-pages-app"></a><span data-ttu-id="342c8-104">Adicionando um modelo a um aplicativo de Páginas do Razor</span><span class="sxs-lookup"><span data-stu-id="342c8-104">Adding a model to a Razor Pages app</span></span>

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="342c8-105">Adicionar um modelo de dados</span><span class="sxs-lookup"><span data-stu-id="342c8-105">Add a data model</span></span>

<span data-ttu-id="342c8-106">No Gerenciador de Soluções, clique com o botão direito do mouse no projeto **RazorPagesMovie** > **Adicionar** > **Nova Pasta**.</span><span class="sxs-lookup"><span data-stu-id="342c8-106">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="342c8-107">Nomeie a pasta *Models*.</span><span class="sxs-lookup"><span data-stu-id="342c8-107">Name the folder *Models*.</span></span>

<span data-ttu-id="342c8-108">Clique com o botão direito do mouse na pasta *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="342c8-108">Right click the *Models* folder.</span></span> <span data-ttu-id="342c8-109">Selecione **Adicionar** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="342c8-109">Select **Add** > **Class**.</span></span> <span data-ttu-id="342c8-110">Nomeie a classe **Movie** e adicione as seguintes propriedades:</span><span class="sxs-lookup"><span data-stu-id="342c8-110">Name the class **Movie** and add the following properties:</span></span>

[!INCLUDE[model 2](../../includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="342c8-111">Adicionar uma cadeia de conexão de banco de dados</span><span class="sxs-lookup"><span data-stu-id="342c8-111">Add a database connection string</span></span>

<span data-ttu-id="342c8-112">Adicione uma cadeia de conexão ao arquivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="342c8-112">Add a connection string to the *appsettings.json* file.</span></span>

[!code-json[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="342c8-113">Registrar o contexto do banco de dados</span><span class="sxs-lookup"><span data-stu-id="342c8-113">Register the database context</span></span>

<span data-ttu-id="342c8-114">Registre o contexto do banco de dados com o contêiner de [injeção de dependência](xref:fundamentals/dependency-injection) no arquivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="342c8-114">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the *Startup.cs* file.</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-6)]

<span data-ttu-id="342c8-115">Crie o projeto para verificar se você não tem nenhum erro.</span><span class="sxs-lookup"><span data-stu-id="342c8-115">Build the project to verify you don't have any errors.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="342c8-116">Adicionar ferramentas de scaffold e executar a migração inicial</span><span class="sxs-lookup"><span data-stu-id="342c8-116">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="342c8-117">Nesta seção, você deve usar o PMC (Console de Gerenciador de Pacotes) para:</span><span class="sxs-lookup"><span data-stu-id="342c8-117">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="342c8-118">Adicione o pacote de geração de código da Web do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="342c8-118">Add the Visual Studio web code generation package.</span></span> <span data-ttu-id="342c8-119">Esse pacote é necessário para executar o mecanismo de scaffolding.</span><span class="sxs-lookup"><span data-stu-id="342c8-119">This package is required to run the scaffolding engine.</span></span>
* <span data-ttu-id="342c8-120">Adicionar uma migração inicial.</span><span class="sxs-lookup"><span data-stu-id="342c8-120">Add an initial migration.</span></span>
* <span data-ttu-id="342c8-121">Atualize o banco de dados com a migração inicial.</span><span class="sxs-lookup"><span data-stu-id="342c8-121">Update the database with the initial migration.</span></span>

<span data-ttu-id="342c8-122">No menu **Ferramentas**, selecione **Gerenciador de pacotes NuGet** > **Console do Gerenciador de pacotes**.</span><span class="sxs-lookup"><span data-stu-id="342c8-122">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu do PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="342c8-124">No PMC, digite os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="342c8-124">In the PMC, enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.0
Add-Migration Initial
Update-Database
```

<span data-ttu-id="342c8-125">O comando `Install-Package` instala as ferramentas necessárias para executar o mecanismo de scaffolding.</span><span class="sxs-lookup"><span data-stu-id="342c8-125">The `Install-Package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="342c8-126">O comando `Add-Migration` gera código para criar o esquema de banco de dados inicial.</span><span class="sxs-lookup"><span data-stu-id="342c8-126">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="342c8-127">O esquema é baseado no modelo especificado no `DbContext` (no arquivo *Models/MovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="342c8-127">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="342c8-128">O argumento `Initial` é usado para nomear as migrações.</span><span class="sxs-lookup"><span data-stu-id="342c8-128">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="342c8-129">Você pode usar qualquer nome, mas, por convenção, escolha um nome que descreve a migração.</span><span class="sxs-lookup"><span data-stu-id="342c8-129">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="342c8-130">Consulte [Introdução às migrações](xref:data/ef-mvc/migrations#introduction-to-migrations) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="342c8-130">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="342c8-131">O comando `Update-Database` executa o método `Up` no arquivo *Migrations/\<time-stamp>_InitialCreate.cs*, que cria o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="342c8-131">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE[model 4windows](../../includes/RP/model4Win.md)]

[!INCLUDE[model 4](../../includes/RP/model4.md)]

<span data-ttu-id="342c8-132">O tutorial a seguir explica os arquivos criados por scaffolding.</span><span class="sxs-lookup"><span data-stu-id="342c8-132">The next tutorial explains the files created by scaffolding.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="342c8-133">[Anterior: Introdução](xref:tutorials/razor-pages/razor-pages-start)
[Próximo: Páginas do Razor geradas por scaffolding](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="342c8-133">[Previous: Getting Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>    
