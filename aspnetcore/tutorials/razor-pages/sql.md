---
title: Trabalhando com o SQL Server LocalDB e o ASP.NET Core
author: rick-anderson
description: Explica como trabalhar com o SQL Server LocalDB e o ASP.NET Core.
keywords: "ASP.NET Core, Páginas Razor, Razor, MVC, SQL, LocalDB"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 42fa98886f3e87e79ea1ea4a2223a79319676006
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/28/2017
---
# <a name="working-with-sql-server-localdb-and-aspnet-core"></a><span data-ttu-id="ca507-104">Trabalhando com o SQL Server LocalDB e o ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ca507-104">Working with SQL Server LocalDB and ASP.NET Core</span></span>

<span data-ttu-id="ca507-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="ca507-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span> 

<span data-ttu-id="ca507-106">O objeto `MovieContext` cuida da tarefa de se conectar ao banco de dados e mapear objetos `Movie` para registros do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ca507-106">The `MovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="ca507-107">O contexto de banco de dados é registrado com o contêiner [Injeção de Dependência](xref:fundamentals/dependency-injection) no método `ConfigureServices` no arquivo *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="ca507-107">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=6-7)]

<span data-ttu-id="ca507-108">O sistema de [Configuração](xref:fundamentals/configuration) do ASP.NET Core lê a `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="ca507-108">The ASP.NET Core [Configuration](xref:fundamentals/configuration) system reads the `ConnectionString`.</span></span> <span data-ttu-id="ca507-109">Para o desenvolvimento local, ele obtém a cadeia de conexão do arquivo *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="ca507-109">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-json[Main](razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=2&range=8-10)]

<span data-ttu-id="ca507-110">Quando você implanta o aplicativo em um servidor de teste ou de produção, você pode usar uma variável de ambiente ou outra abordagem para definir a cadeia de conexão como um SQL Server real.</span><span class="sxs-lookup"><span data-stu-id="ca507-110">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="ca507-111">Consulte [Configuração](xref:fundamentals/configuration) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="ca507-111">See [Configuration](xref:fundamentals/configuration) for more information.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="ca507-112">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="ca507-112">SQL Server Express LocalDB</span></span>

<span data-ttu-id="ca507-113">O LocalDB é uma versão leve do Mecanismo de Banco de Dados do SQL Server Express que é direcionado para o desenvolvimento de programas.</span><span class="sxs-lookup"><span data-stu-id="ca507-113">LocalDB is a lightweight version of the SQL Server Express Database Engine that is targeted for program development.</span></span> <span data-ttu-id="ca507-114">O LocalDB é iniciado sob demanda e executado no modo de usuário e, portanto, não há nenhuma configuração complexa.</span><span class="sxs-lookup"><span data-stu-id="ca507-114">LocalDB starts on demand and runs in user mode, so there is no complex configuration.</span></span> <span data-ttu-id="ca507-115">Por padrão, o banco de dados LocalDB cria arquivos “\*.mdf” no diretório *C:/Users/\<user\>*.</span><span class="sxs-lookup"><span data-stu-id="ca507-115">By default, LocalDB database creates "\*.mdf" files in the *C:/Users/\<user\>* directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="ca507-116">No menu **Exibir**, abra **SSOX** (Pesquisador de Objetos do SQL Server).</span><span class="sxs-lookup"><span data-stu-id="ca507-116">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Menu de exibição](sql/_static/ssox.png)

* <span data-ttu-id="ca507-118">Clique com o botão direito do mouse na tabela `Movie` e selecione **Designer de exibição**:</span><span class="sxs-lookup"><span data-stu-id="ca507-118">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![Menu contextual aberto na tabela Movie](sql/_static/design.png)

  ![Tabela Movie aberta no Designer](sql/_static/dv.png)

<span data-ttu-id="ca507-121">Observe o ícone de chave ao lado de `ID`.</span><span class="sxs-lookup"><span data-stu-id="ca507-121">Note the key icon next to `ID`.</span></span> <span data-ttu-id="ca507-122">Por padrão, o EF cria uma propriedade chamada `ID` para a chave primária.</span><span class="sxs-lookup"><span data-stu-id="ca507-122">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="ca507-123">Clique com o botão direito do mouse na tabela `Movie` e selecione **Exibir dados**:</span><span class="sxs-lookup"><span data-stu-id="ca507-123">Right click on the `Movie` table and select **View Data**:</span></span>

  ![Tabela Movie aberta mostrando os dados da tabela](sql/_static/vd22.png)

## <a name="seed-the-database"></a><span data-ttu-id="ca507-125">Propagar o banco de dados</span><span class="sxs-lookup"><span data-stu-id="ca507-125">Seed the database</span></span>

<span data-ttu-id="ca507-126">Crie uma nova classe chamada `SeedData` na pasta *Models*.</span><span class="sxs-lookup"><span data-stu-id="ca507-126">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="ca507-127">Substitua o código gerado pelo seguinte:</span><span class="sxs-lookup"><span data-stu-id="ca507-127">Replace the generated code with the following:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="ca507-128">Se houver um filme no BD, o inicializador de semeadura será retornado e nenhum filme será adicionado.</span><span class="sxs-lookup"><span data-stu-id="ca507-128">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movies.Any())
{
    return;   // DB has been seeded.
}
```
<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="ca507-129">Adicionar o inicializador de semeadura</span><span class="sxs-lookup"><span data-stu-id="ca507-129">Add the seed initializer</span></span>

<span data-ttu-id="ca507-130">Adicione o inicializador de semeadura ao final do método `Main` no arquivo *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="ca507-130">Add the seed initializer to the end of the `Main` method in the *Program.cs* file:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Program.cs?highlight=6,17-32)]

<span data-ttu-id="ca507-131">Testar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="ca507-131">Test the app</span></span>

* <span data-ttu-id="ca507-132">Exclua todos os registros no BD.</span><span class="sxs-lookup"><span data-stu-id="ca507-132">Delete all the records in the DB.</span></span> <span data-ttu-id="ca507-133">Faça isso com os links Excluir no navegador ou no [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span><span class="sxs-lookup"><span data-stu-id="ca507-133">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="ca507-134">Force o aplicativo a ser inicializado (chame os métodos na classe `Startup`) para que o método de semeadura seja executado.</span><span class="sxs-lookup"><span data-stu-id="ca507-134">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="ca507-135">Para forçar a inicialização, o IIS Express deve ser interrompido e reiniciado.</span><span class="sxs-lookup"><span data-stu-id="ca507-135">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="ca507-136">Faça isso com uma das seguintes abordagens:</span><span class="sxs-lookup"><span data-stu-id="ca507-136">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="ca507-137">Clique com botão direito do mouse no ícone de bandeja do sistema do IIS Express na área de notificação e toque em **Sair** ou **Parar site**:</span><span class="sxs-lookup"><span data-stu-id="ca507-137">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![Ícone de bandeja do sistema do IIS Express](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Menu contextual](sql/_static/stopIIS.png)

   * <span data-ttu-id="ca507-140">Se você estiver executando o VS no modo sem depuração, pressione F5 para executar no modo de depuração.</span><span class="sxs-lookup"><span data-stu-id="ca507-140">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
   * <span data-ttu-id="ca507-141">Se você estiver executando o VS no modo de depuração, pare o depurador e pressione F5.</span><span class="sxs-lookup"><span data-stu-id="ca507-141">If you were running VS in debug mode, stop the debugger and press F5.</span></span>
   
<span data-ttu-id="ca507-142">O aplicativo mostra os dados propagados:</span><span class="sxs-lookup"><span data-stu-id="ca507-142">The app shows the seeded data:</span></span>

![Aplicativo de filme aberto no Chrome mostrando os dados do filme](sql/_static/m55.png)

<span data-ttu-id="ca507-144">O próximo tutorial limpará a apresentação dos dados.</span><span class="sxs-lookup"><span data-stu-id="ca507-144">The next tutorial will clean up the presentation of the data.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="ca507-145">[Anterior: Páginas do Razor geradas por scaffolding](xref:tutorials/razor-pages/page)
[Próximo: Atualização das páginas](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="ca507-145">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>
