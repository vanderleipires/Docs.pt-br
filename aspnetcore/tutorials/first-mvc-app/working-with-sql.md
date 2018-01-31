---
title: Trabalhando com o SQL Server LocalDB
author: rick-anderson
description: Usando o SQL Server LocalDB com um aplicativo MVC simples
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/working-with-sql
ms.openlocfilehash: 0394782d1318f810c2ffe6d2e08bcf082f490a7d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
# <a name="working-with-sql-server-localdb"></a><span data-ttu-id="b32e2-103">Trabalhando com o SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="b32e2-103">Working with SQL Server LocalDB</span></span>

<span data-ttu-id="b32e2-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b32e2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b32e2-105">O objeto `MvcMovieContext` cuida da tarefa de se conectar ao banco de dados e mapear objetos `Movie` para registros do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="b32e2-105">The `MvcMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="b32e2-106">O contexto de banco de dados é registrado com o contêiner [Injeção de Dependência](xref:fundamentals/dependency-injection) no método `ConfigureServices` no arquivo *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="b32e2-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

<span data-ttu-id="b32e2-107">O sistema de [Configuração](xref:fundamentals/configuration/index) do ASP.NET Core lê a `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="b32e2-107">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="b32e2-108">Para o desenvolvimento local, ele obtém a cadeia de conexão do arquivo *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="b32e2-108">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-json[Main](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]

<span data-ttu-id="b32e2-109">Quando você implanta o aplicativo em um servidor de teste ou de produção, você pode usar uma variável de ambiente ou outra abordagem para definir a cadeia de conexão como um SQL Server real.</span><span class="sxs-lookup"><span data-stu-id="b32e2-109">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="b32e2-110">Consulte [Configuração](xref:fundamentals/configuration/index) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="b32e2-110">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="b32e2-111">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="b32e2-111">SQL Server Express LocalDB</span></span>

<span data-ttu-id="b32e2-112">O LocalDB é uma versão leve do Mecanismo de Banco de Dados do SQL Server Express, que é direcionado para o desenvolvimento de programas.</span><span class="sxs-lookup"><span data-stu-id="b32e2-112">LocalDB is a lightweight version of the SQL Server Express Database Engine that's targeted for program development.</span></span> <span data-ttu-id="b32e2-113">O LocalDB é iniciado sob demanda e executado no modo de usuário e, portanto, não há nenhuma configuração complexa.</span><span class="sxs-lookup"><span data-stu-id="b32e2-113">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="b32e2-114">Por padrão, o banco de dados LocalDB cria arquivos “\*.mdf” no diretório *C:/Users/\<user\>*.</span><span class="sxs-lookup"><span data-stu-id="b32e2-114">By default, LocalDB database creates "\*.mdf" files in the *C:/Users/\<user\>* directory.</span></span>

* <span data-ttu-id="b32e2-115">No menu **Exibir**, abra **SSOX** (Pesquisador de Objetos do SQL Server).</span><span class="sxs-lookup"><span data-stu-id="b32e2-115">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Menu de exibição](working-with-sql/_static/ssox.png)

* <span data-ttu-id="b32e2-117">Clique com o botão direito do mouse na tabela `Movie` **> Designer de Exibição**</span><span class="sxs-lookup"><span data-stu-id="b32e2-117">Right click on the `Movie` table **> View Designer**</span></span>

  ![Menu contextual aberto na tabela Movie](working-with-sql/_static/design.png)

  ![Tabela Movie aberta no Designer](working-with-sql/_static/dv.png)

<span data-ttu-id="b32e2-120">Observe o ícone de chave ao lado de `ID`.</span><span class="sxs-lookup"><span data-stu-id="b32e2-120">Note the key icon next to `ID`.</span></span> <span data-ttu-id="b32e2-121">Por padrão, o EF tornará uma propriedade chamada `ID` a chave primária.</span><span class="sxs-lookup"><span data-stu-id="b32e2-121">By default, EF will make a property named `ID` the primary key.</span></span>

* <span data-ttu-id="b32e2-122">Clique com o botão direito do mouse na tabela `Movie` **> Dados de Exibição**</span><span class="sxs-lookup"><span data-stu-id="b32e2-122">Right click on the `Movie` table **> View Data**</span></span>

  ![Menu contextual aberto na tabela Movie](working-with-sql/_static/ssox2.png)

  ![Tabela Movie aberta mostrando os dados da tabela](working-with-sql/_static/vd22.png)

## <a name="seed-the-database"></a><span data-ttu-id="b32e2-125">Propagar o banco de dados</span><span class="sxs-lookup"><span data-stu-id="b32e2-125">Seed the database</span></span>

<span data-ttu-id="b32e2-126">Crie uma nova classe chamada `SeedData` na pasta *Models*.</span><span class="sxs-lookup"><span data-stu-id="b32e2-126">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="b32e2-127">Substitua o código gerado pelo seguinte:</span><span class="sxs-lookup"><span data-stu-id="b32e2-127">Replace the generated code with the following:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="b32e2-128">Se houver um filme no BD, o inicializador de semeadura será retornado e nenhum filme será adicionado.</span><span class="sxs-lookup"><span data-stu-id="b32e2-128">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="b32e2-129">Adicionar o inicializador de semeadura</span><span class="sxs-lookup"><span data-stu-id="b32e2-129">Add the seed initializer</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b32e2-130">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b32e2-130">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b32e2-131">Adicione o inicializador de semeadura ao método `Main` no arquivo *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="b32e2-131">Add the seed initializer to the `Main` method in the *Program.cs* file:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Program.cs?highlight=6,14-32)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b32e2-132">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b32e2-132">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b32e2-133">Adicione o inicializador de semeadura ao final do método `Configure` no arquivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="b32e2-133">Add the seed initializer to the end of the `Configure` method in the *Startup.cs* file.</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?highlight=9&name=snippet_seed)]

---

<span data-ttu-id="b32e2-134">Testar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="b32e2-134">Test the app</span></span>

* <span data-ttu-id="b32e2-135">Exclua todos os registros no BD.</span><span class="sxs-lookup"><span data-stu-id="b32e2-135">Delete all the records in the DB.</span></span> <span data-ttu-id="b32e2-136">Faça isso com os links Excluir no navegador ou no SSOX.</span><span class="sxs-lookup"><span data-stu-id="b32e2-136">You can do this with the delete links in the browser or from SSOX.</span></span>
* <span data-ttu-id="b32e2-137">Force o aplicativo a ser inicializado (chame os métodos na classe `Startup`) para que o método de semeadura seja executado.</span><span class="sxs-lookup"><span data-stu-id="b32e2-137">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="b32e2-138">Para forçar a inicialização, o IIS Express deve ser interrompido e reiniciado.</span><span class="sxs-lookup"><span data-stu-id="b32e2-138">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="b32e2-139">Faça isso com uma das seguintes abordagens:</span><span class="sxs-lookup"><span data-stu-id="b32e2-139">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="b32e2-140">Clique com botão direito do mouse no ícone de bandeja do sistema do IIS Express na área de notificação e toque em **Sair** ou **Parar Site**</span><span class="sxs-lookup"><span data-stu-id="b32e2-140">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**</span></span>

    ![Ícone de bandeja do sistema do IIS Express](working-with-sql/_static/iisExIcon.png)

    ![Menu contextual](working-with-sql/_static/stopIIS.png)

   * <span data-ttu-id="b32e2-143">Se você estiver executando o VS no modo sem depuração, pressione F5 para executar no modo de depuração</span><span class="sxs-lookup"><span data-stu-id="b32e2-143">If you were running VS in non-debug mode, press F5 to run in debug mode</span></span>
   * <span data-ttu-id="b32e2-144">Se você estiver executando o VS no modo de depuração, pare o depurador e pressione F5</span><span class="sxs-lookup"><span data-stu-id="b32e2-144">If you were running VS in debug mode, stop the debugger and press F5</span></span>
   
<span data-ttu-id="b32e2-145">O aplicativo mostra os dados propagados.</span><span class="sxs-lookup"><span data-stu-id="b32e2-145">The app shows the seeded data.</span></span>

![Aplicativo de filme MVC aberto no Microsoft Edge, mostrando os dados do filme](working-with-sql/_static/m55.png)

>[!div class="step-by-step"]
<span data-ttu-id="b32e2-147">[Anterior](adding-model.md)
[Próximo](controller-methods-views.md)</span><span class="sxs-lookup"><span data-stu-id="b32e2-147">[Previous](adding-model.md)
[Next](controller-methods-views.md)</span></span>  
