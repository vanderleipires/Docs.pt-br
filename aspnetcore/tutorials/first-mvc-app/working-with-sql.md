---
title: Trabalhar com o LocalDB do SQL Server no ASP.NET Core
author: rick-anderson
description: Saiba mais sobre como usar o LocalDB do SQL Server em um aplicativo ASP.NET Core MVC simples.
ms.author: riande
ms.date: 03/07/2017
uid: tutorials/first-mvc-app/working-with-sql
ms.openlocfilehash: 5b8bbcd3c6590edbe199a0a52494e83fd2aa4dcf
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273865"
---
# <a name="work-with-sql-server-localdb-in-aspnet-core"></a><span data-ttu-id="61793-103">Trabalhar com o LocalDB do SQL Server no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="61793-103">Work with SQL Server LocalDB in ASP.NET Core</span></span>

<span data-ttu-id="61793-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="61793-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="61793-105">O objeto `MvcMovieContext` cuida da tarefa de se conectar ao banco de dados e mapear objetos `Movie` para registros do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="61793-105">The `MvcMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="61793-106">O contexto de banco de dados é registrado com o contêiner [Injeção de Dependência](xref:fundamentals/dependency-injection) no método `ConfigureServices` no arquivo *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="61793-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="61793-107">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Startup.cs?name=ConfigureServices&highlight=13-99)]</span><span class="sxs-lookup"><span data-stu-id="61793-107">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Startup.cs?name=ConfigureServices&highlight=13-99)]</span></span>
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="61793-108">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]</span><span class="sxs-lookup"><span data-stu-id="61793-108">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]</span></span>
::: moniker-end

<span data-ttu-id="61793-109">O sistema de [Configuração](xref:fundamentals/configuration/index) do ASP.NET Core lê a `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="61793-109">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="61793-110">Para o desenvolvimento local, ele obtém a cadeia de conexão do arquivo *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="61793-110">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-json[](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]

<span data-ttu-id="61793-111">Quando você implanta o aplicativo em um servidor de teste ou de produção, você pode usar uma variável de ambiente ou outra abordagem para definir a cadeia de conexão como um SQL Server real.</span><span class="sxs-lookup"><span data-stu-id="61793-111">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="61793-112">Consulte [Configuração](xref:fundamentals/configuration/index) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="61793-112">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="61793-113">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="61793-113">SQL Server Express LocalDB</span></span>

<span data-ttu-id="61793-114">O LocalDB é uma versão leve do Mecanismo de Banco de Dados do SQL Server Express, que é direcionado para o desenvolvimento de programas.</span><span class="sxs-lookup"><span data-stu-id="61793-114">LocalDB is a lightweight version of the SQL Server Express Database Engine that's targeted for program development.</span></span> <span data-ttu-id="61793-115">O LocalDB é iniciado sob demanda e executado no modo de usuário e, portanto, não há nenhuma configuração complexa.</span><span class="sxs-lookup"><span data-stu-id="61793-115">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="61793-116">Por padrão, o banco de dados LocalDB cria arquivos “\*.mdf” no diretório *C:/Users/\<user\>*.</span><span class="sxs-lookup"><span data-stu-id="61793-116">By default, LocalDB database creates "\*.mdf" files in the *C:/Users/\<user\>* directory.</span></span>

* <span data-ttu-id="61793-117">No menu **Exibir**, abra **SSOX** (Pesquisador de Objetos do SQL Server).</span><span class="sxs-lookup"><span data-stu-id="61793-117">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Menu de exibição](working-with-sql/_static/ssox.png)

* <span data-ttu-id="61793-119">Clique com o botão direito do mouse na tabela `Movie` **> Designer de Exibição**</span><span class="sxs-lookup"><span data-stu-id="61793-119">Right click on the `Movie` table **> View Designer**</span></span>

  ![Menu contextual aberto na tabela Movie](working-with-sql/_static/design.png)

  ![Tabela Movie aberta no Designer](working-with-sql/_static/dv.png)

<span data-ttu-id="61793-122">Observe o ícone de chave ao lado de `ID`.</span><span class="sxs-lookup"><span data-stu-id="61793-122">Note the key icon next to `ID`.</span></span> <span data-ttu-id="61793-123">Por padrão, o EF tornará uma propriedade chamada `ID` a chave primária.</span><span class="sxs-lookup"><span data-stu-id="61793-123">By default, EF will make a property named `ID` the primary key.</span></span>

* <span data-ttu-id="61793-124">Clique com o botão direito do mouse na tabela `Movie` **> Dados de Exibição**</span><span class="sxs-lookup"><span data-stu-id="61793-124">Right click on the `Movie` table **> View Data**</span></span>

  ![Menu contextual aberto na tabela Movie](working-with-sql/_static/ssox2.png)

  ![Tabela Movie aberta mostrando os dados da tabela](working-with-sql/_static/vd22.png)

## <a name="seed-the-database"></a><span data-ttu-id="61793-127">Propagar o banco de dados</span><span class="sxs-lookup"><span data-stu-id="61793-127">Seed the database</span></span>

<span data-ttu-id="61793-128">Crie uma nova classe chamada `SeedData` na pasta *Models*.</span><span class="sxs-lookup"><span data-stu-id="61793-128">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="61793-129">Substitua o código gerado pelo seguinte:</span><span class="sxs-lookup"><span data-stu-id="61793-129">Replace the generated code with the following:</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="61793-130">Se houver um filme no BD, o inicializador de semeadura será retornado e nenhum filme será adicionado.</span><span class="sxs-lookup"><span data-stu-id="61793-130">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="61793-131">Adicionar o inicializador de semeadura</span><span class="sxs-lookup"><span data-stu-id="61793-131">Add the seed initializer</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="61793-132">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Program.cs)]</span><span class="sxs-lookup"><span data-stu-id="61793-132">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Program.cs)]</span></span>
::: moniker-end
::: moniker range="<= aspnetcore-2.0"

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="61793-133">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="61793-133">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="61793-134">Adicione o inicializador de semeadura ao método `Main` no arquivo *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="61793-134">Add the seed initializer to the `Main` method in the *Program.cs* file:</span></span>

<span data-ttu-id="61793-135">[!code-csharp[](start-mvc/sample/MvcMovie/Program.cs?highlight=6,14-32)]</span><span class="sxs-lookup"><span data-stu-id="61793-135">[!code-csharp[](start-mvc/sample/MvcMovie/Program.cs?highlight=6,14-32)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="61793-136">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="61793-136">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="61793-137">Adicione o inicializador de semeadura ao final do método `Configure` no arquivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="61793-137">Add the seed initializer to the end of the `Configure` method in the *Startup.cs* file.</span></span>

<span data-ttu-id="61793-138">[!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?highlight=9&name=snippet_seed)]</span><span class="sxs-lookup"><span data-stu-id="61793-138">[!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?highlight=9&name=snippet_seed)]</span></span>

---
::: moniker-end

<span data-ttu-id="61793-139">Testar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="61793-139">Test the app</span></span>

* <span data-ttu-id="61793-140">Exclua todos os registros no BD.</span><span class="sxs-lookup"><span data-stu-id="61793-140">Delete all the records in the DB.</span></span> <span data-ttu-id="61793-141">Faça isso com os links Excluir no navegador ou no SSOX.</span><span class="sxs-lookup"><span data-stu-id="61793-141">You can do this with the delete links in the browser or from SSOX.</span></span>
* <span data-ttu-id="61793-142">Force o aplicativo a ser inicializado (chame os métodos na classe `Startup`) para que o método de semeadura seja executado.</span><span class="sxs-lookup"><span data-stu-id="61793-142">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="61793-143">Para forçar a inicialização, o IIS Express deve ser interrompido e reiniciado.</span><span class="sxs-lookup"><span data-stu-id="61793-143">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="61793-144">Faça isso com uma das seguintes abordagens:</span><span class="sxs-lookup"><span data-stu-id="61793-144">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="61793-145">Clique com botão direito do mouse no ícone de bandeja do sistema do IIS Express na área de notificação e toque em **Sair** ou **Parar Site**</span><span class="sxs-lookup"><span data-stu-id="61793-145">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**</span></span>

    ![Ícone de bandeja do sistema do IIS Express](working-with-sql/_static/iisExIcon.png)

    ![Menu contextual](working-with-sql/_static/stopIIS.png)

    * <span data-ttu-id="61793-148">Se você estiver executando o VS no modo sem depuração, pressione F5 para executar no modo de depuração</span><span class="sxs-lookup"><span data-stu-id="61793-148">If you were running VS in non-debug mode, press F5 to run in debug mode</span></span>
    * <span data-ttu-id="61793-149">Se você estiver executando o VS no modo de depuração, pare o depurador e pressione F5</span><span class="sxs-lookup"><span data-stu-id="61793-149">If you were running VS in debug mode, stop the debugger and press F5</span></span>

<span data-ttu-id="61793-150">O aplicativo mostra os dados propagados.</span><span class="sxs-lookup"><span data-stu-id="61793-150">The app shows the seeded data.</span></span>

![Aplicativo de filme MVC aberto no Microsoft Edge, mostrando os dados do filme](working-with-sql/_static/m55.png)

> [!div class="step-by-step"]
> <span data-ttu-id="61793-152">[Anterior](adding-model.md)
> [Próximo](controller-methods-views.md)</span><span class="sxs-lookup"><span data-stu-id="61793-152">[Previous](adding-model.md)
[Next](controller-methods-views.md)</span></span>  
