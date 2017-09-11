<span data-ttu-id="29284-101">A tabela a seguir detalha os parâmetros dos geradores de código do ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="29284-101">The following table details the ASP.NET Core code generators\` parameters:</span></span>

| <span data-ttu-id="29284-102">Parâmetro</span><span class="sxs-lookup"><span data-stu-id="29284-102">Parameter</span></span>               | <span data-ttu-id="29284-103">Descrição</span><span class="sxs-lookup"><span data-stu-id="29284-103">Description</span></span>|
| ----------------- | ------------ |
| <span data-ttu-id="29284-104">-m</span><span class="sxs-lookup"><span data-stu-id="29284-104">-m</span></span>  | <span data-ttu-id="29284-105">O nome do modelo.</span><span class="sxs-lookup"><span data-stu-id="29284-105">The name of the model.</span></span> |
| <span data-ttu-id="29284-106">-dc</span><span class="sxs-lookup"><span data-stu-id="29284-106">-dc</span></span>  | <span data-ttu-id="29284-107">O contexto de dados.</span><span class="sxs-lookup"><span data-stu-id="29284-107">The data context.</span></span> |
| <span data-ttu-id="29284-108">-udl</span><span class="sxs-lookup"><span data-stu-id="29284-108">-udl</span></span> | <span data-ttu-id="29284-109">Use o layout padrão.</span><span class="sxs-lookup"><span data-stu-id="29284-109">Use the default layout.</span></span> |
| <span data-ttu-id="29284-110">-outDir</span><span class="sxs-lookup"><span data-stu-id="29284-110">-outDir</span></span> | <span data-ttu-id="29284-111">O caminho da pasta de saída relativa para criar as exibições.</span><span class="sxs-lookup"><span data-stu-id="29284-111">The relative output folder path to create the views.</span></span> |
| <span data-ttu-id="29284-112">--referenceScriptLibraries</span><span class="sxs-lookup"><span data-stu-id="29284-112">--referenceScriptLibraries</span></span> | <span data-ttu-id="29284-113">Adiciona `_ValidationScriptsPartial` para editar e criar páginas</span><span class="sxs-lookup"><span data-stu-id="29284-113">Adds `_ValidationScriptsPartial` to Edit and Create pages</span></span> |

<span data-ttu-id="29284-114">Use a opção `h` para obter ajuda sobre o comando `aspnet-codegenerator razorpage`:</span><span class="sxs-lookup"><span data-stu-id="29284-114">Use the `h` switch to get help on the `aspnet-codegenerator razorpage` command:</span></span>

```console
dotnet aspnet-codegenerator razorpage -h
```
<a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="29284-115">Testar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="29284-115">Test the app</span></span>

* <span data-ttu-id="29284-116">Executar o aplicativo e acrescentar `/Movies` à URL no navegador (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="29284-116">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="29284-117">Teste o link **Criar**.</span><span class="sxs-lookup"><span data-stu-id="29284-117">Test the **Create** link.</span></span>

 ![Criar página](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="29284-119">Teste os links **Editar**, **Detalhes** e **Excluir**.</span><span class="sxs-lookup"><span data-stu-id="29284-119">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="29284-120">Se você receber o seguinte erro, verifique se você executou migrações e atualizou o banco de dados:</span><span class="sxs-lookup"><span data-stu-id="29284-120">If you get the following error, verify you have run migrations and updated the database:</span></span>

```
An unhandled exception occurred while processing the request.
SqliteException: SQLite Error 1: 'no such table: Movie'.
Microsoft.Data.Sqlite.SqliteException.ThrowExceptionForRC(int rc, sqlite3 db)
```