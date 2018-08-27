---
title: Trabalhar com o LocalDB do SQL Server e o ASP.NET Core
author: rick-anderson
description: Explica como trabalhar com o SQL Server LocalDB e o ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/07/2017
uid: tutorials/razor-pages/sql
ms.openlocfilehash: ef4e1fb3bf1ac1b3695ff89d6692ac6fa1641e31
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2018
ms.locfileid: "41820153"
---
# <a name="work-with-sql-server-localdb-and-aspnet-core"></a><span data-ttu-id="728ca-103">Trabalhar com o LocalDB do SQL Server e o ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="728ca-103">Work with SQL Server LocalDB and ASP.NET Core</span></span>

<span data-ttu-id="728ca-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="728ca-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span> 

<span data-ttu-id="728ca-105">O objeto `MovieContext` cuida da tarefa de se conectar ao banco de dados e mapear objetos `Movie` para registros do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="728ca-105">The `MovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="728ca-106">O contexto de banco de dados é registrado com o contêiner [Injeção de Dependência](xref:fundamentals/dependency-injection) no método `ConfigureServices` no arquivo *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="728ca-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

::: moniker range="= aspnetcore-2.0"
[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=7-8)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]

<span data-ttu-id="728ca-107">Para obter mais informações sobre os métodos usados em `ConfigureServices`, veja:</span><span class="sxs-lookup"><span data-stu-id="728ca-107">For more information on the methods used in `ConfigureServices`, see:</span></span>

* <span data-ttu-id="728ca-108">[Suporte ao RGPD (Regulamento Geral sobre a Proteção de Dados) da UE no ASP.NET Core](xref:security/gdpr) para `CookiePolicyOptions`.</span><span class="sxs-lookup"><span data-stu-id="728ca-108">[EU General Data Protection Regulation (GDPR) support in ASP.NET Core](xref:security/gdpr) for `CookiePolicyOptions`.</span></span>
* [<span data-ttu-id="728ca-109">SetCompatibilityVersion</span><span class="sxs-lookup"><span data-stu-id="728ca-109">SetCompatibilityVersion</span></span>](xref:mvc/compatibility-version)

::: moniker-end

<span data-ttu-id="728ca-110">O sistema de [Configuração](xref:fundamentals/configuration/index) do ASP.NET Core lê a `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="728ca-110">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="728ca-111">Para o desenvolvimento local, ele obtém a cadeia de conexão do arquivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="728ca-111">For local development, it gets the connection string from the *appsettings.json* file.</span></span> <span data-ttu-id="728ca-112">O valor do nome do banco de dados (`Database={Database name}`) será diferente para o seu código gerado.</span><span class="sxs-lookup"><span data-stu-id="728ca-112">The name value for the database (`Database={Database name}`) will be different for your generated code.</span></span> <span data-ttu-id="728ca-113">O valor do nome é arbitrário.</span><span class="sxs-lookup"><span data-stu-id="728ca-113">The name value is arbitrary.</span></span>

[!code-json[](razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=2&range=8-10)]

<span data-ttu-id="728ca-114">Quando você implanta o aplicativo em um servidor de teste ou de produção, você pode usar uma variável de ambiente ou outra abordagem para definir a cadeia de conexão como um SQL Server real.</span><span class="sxs-lookup"><span data-stu-id="728ca-114">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="728ca-115">Consulte [Configuração](xref:fundamentals/configuration/index) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="728ca-115">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="728ca-116">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="728ca-116">SQL Server Express LocalDB</span></span>

<span data-ttu-id="728ca-117">O LocalDB é uma versão leve do Mecanismo de Banco de Dados do SQL Server Express, que é direcionado para o desenvolvimento de programas.</span><span class="sxs-lookup"><span data-stu-id="728ca-117">LocalDB is a lightweight version of the SQL Server Express Database Engine that's targeted for program development.</span></span> <span data-ttu-id="728ca-118">O LocalDB é iniciado sob demanda e executado no modo de usuário e, portanto, não há nenhuma configuração complexa.</span><span class="sxs-lookup"><span data-stu-id="728ca-118">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="728ca-119">Por padrão, o banco de dados LocalDB cria arquivos “\*.mdf” no diretório *C:/Users/\<user\>*.</span><span class="sxs-lookup"><span data-stu-id="728ca-119">By default, LocalDB database creates "\*.mdf" files in the *C:/Users/\<user\>* directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="728ca-120">No menu **Exibir**, abra **SSOX** (Pesquisador de Objetos do SQL Server).</span><span class="sxs-lookup"><span data-stu-id="728ca-120">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Menu de exibição](sql/_static/ssox.png)

* <span data-ttu-id="728ca-122">Clique com o botão direito do mouse na tabela `Movie` e selecione **Designer de exibição**:</span><span class="sxs-lookup"><span data-stu-id="728ca-122">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![Menu contextual aberto na tabela Movie](sql/_static/design.png)

  ![Tabela Movie aberta no Designer](sql/_static/dv.png)

<span data-ttu-id="728ca-125">Observe o ícone de chave ao lado de `ID`.</span><span class="sxs-lookup"><span data-stu-id="728ca-125">Note the key icon next to `ID`.</span></span> <span data-ttu-id="728ca-126">Por padrão, o EF cria uma propriedade chamada `ID` para a chave primária.</span><span class="sxs-lookup"><span data-stu-id="728ca-126">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="728ca-127">Clique com o botão direito do mouse na tabela `Movie` e selecione **Exibir dados**:</span><span class="sxs-lookup"><span data-stu-id="728ca-127">Right click on the `Movie` table and select **View Data**:</span></span>

  ![Tabela Movie aberta mostrando os dados da tabela](sql/_static/vd22.png)

## <a name="seed-the-database"></a><span data-ttu-id="728ca-129">Propagar o banco de dados</span><span class="sxs-lookup"><span data-stu-id="728ca-129">Seed the database</span></span>

<span data-ttu-id="728ca-130">Crie uma nova classe chamada `SeedData` na pasta *Models*.</span><span class="sxs-lookup"><span data-stu-id="728ca-130">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="728ca-131">Substitua o código gerado pelo seguinte:</span><span class="sxs-lookup"><span data-stu-id="728ca-131">Replace the generated code with the following:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedData.cs?name=snippet_1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/SeedData.cs?name=snippet_1)]

::: moniker-end

<span data-ttu-id="728ca-132">Se houver um filme no BD, o inicializador de semeadura será retornado e nenhum filme será adicionado.</span><span class="sxs-lookup"><span data-stu-id="728ca-132">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```
<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="728ca-133">Adicionar o inicializador de semeadura</span><span class="sxs-lookup"><span data-stu-id="728ca-133">Add the seed initializer</span></span>

<span data-ttu-id="728ca-134">Em *Program.cs*, modifique o método `Main` para fazer o seguinte:</span><span class="sxs-lookup"><span data-stu-id="728ca-134">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="728ca-135">Obtenha uma instância de contexto de BD do contêiner de injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="728ca-135">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="728ca-136">Chame o método de semente passando a ele o contexto.</span><span class="sxs-lookup"><span data-stu-id="728ca-136">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="728ca-137">Descarte o contexto quando o método de semente for concluído.</span><span class="sxs-lookup"><span data-stu-id="728ca-137">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="728ca-138">O código a seguir mostra o arquivo *Program.cs* atualizado.</span><span class="sxs-lookup"><span data-stu-id="728ca-138">The following code shows the updated *Program.cs* file.</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Program.cs)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Program.cs)]

::: moniker-end

<span data-ttu-id="728ca-139">Um aplicativo de produção não chamaria `Database.Migrate`.</span><span class="sxs-lookup"><span data-stu-id="728ca-139">A production app would not call `Database.Migrate`.</span></span> <span data-ttu-id="728ca-140">Ele é adicionado ao código anterior para evitar a exceção a seguir quando `Update-Database` não foi executado:</span><span class="sxs-lookup"><span data-stu-id="728ca-140">It's added to the preceding code to prevent the following exception when `Update-Database` has not been run:</span></span>

<span data-ttu-id="728ca-141">SqlException: não pode abrir o banco de dados "RazorPagesMovieContext-21" solicitado pelo logon.</span><span class="sxs-lookup"><span data-stu-id="728ca-141">SqlException: Cannot open database "RazorPagesMovieContext-21" requested by the login.</span></span> <span data-ttu-id="728ca-142">O logon falhou.</span><span class="sxs-lookup"><span data-stu-id="728ca-142">The login failed.</span></span>
<span data-ttu-id="728ca-143">O logon falhou para o usuário 'user name'.</span><span class="sxs-lookup"><span data-stu-id="728ca-143">Login failed for user 'user name'.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="728ca-144">Testar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="728ca-144">Test the app</span></span>

* <span data-ttu-id="728ca-145">Exclua todos os registros no BD.</span><span class="sxs-lookup"><span data-stu-id="728ca-145">Delete all the records in the DB.</span></span> <span data-ttu-id="728ca-146">Faça isso com os links Excluir no navegador ou no [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span><span class="sxs-lookup"><span data-stu-id="728ca-146">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="728ca-147">Force o aplicativo a ser inicializado (chame os métodos na classe `Startup`) para que o método de semeadura seja executado.</span><span class="sxs-lookup"><span data-stu-id="728ca-147">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="728ca-148">Para forçar a inicialização, o IIS Express deve ser interrompido e reiniciado.</span><span class="sxs-lookup"><span data-stu-id="728ca-148">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="728ca-149">Faça isso com uma das seguintes abordagens:</span><span class="sxs-lookup"><span data-stu-id="728ca-149">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="728ca-150">Clique com botão direito do mouse no ícone de bandeja do sistema do IIS Express na área de notificação e toque em **Sair** ou **Parar site**:</span><span class="sxs-lookup"><span data-stu-id="728ca-150">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![Ícone de bandeja do sistema do IIS Express](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Menu contextual](sql/_static/stopIIS.png)

    * <span data-ttu-id="728ca-153">Se você estiver executando o VS no modo sem depuração, pressione F5 para executar no modo de depuração.</span><span class="sxs-lookup"><span data-stu-id="728ca-153">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
    * <span data-ttu-id="728ca-154">Se você estiver executando o VS no modo de depuração, pare o depurador e pressione F5.</span><span class="sxs-lookup"><span data-stu-id="728ca-154">If you were running VS in debug mode, stop the debugger and press F5.</span></span>
   
<span data-ttu-id="728ca-155">O aplicativo mostra os dados propagados:</span><span class="sxs-lookup"><span data-stu-id="728ca-155">The app shows the seeded data:</span></span>

![Aplicativo de filme aberto no Chrome mostrando os dados do filme](sql/_static/m55.png)

<span data-ttu-id="728ca-157">O próximo tutorial limpará a apresentação dos dados.</span><span class="sxs-lookup"><span data-stu-id="728ca-157">The next tutorial will clean up the presentation of the data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="728ca-158">[Anterior: Páginas do Razor geradas por scaffolding](xref:tutorials/razor-pages/page)
> [Próximo: Atualização das páginas](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="728ca-158">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>
