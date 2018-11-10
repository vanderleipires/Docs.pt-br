<span data-ttu-id="d113b-101">A tabela a seguir detalha os parâmetros do gerador de código do ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="d113b-101">The following table details the ASP.NET Core code generator parameters:</span></span>

| <span data-ttu-id="d113b-102">Parâmetro</span><span class="sxs-lookup"><span data-stu-id="d113b-102">Parameter</span></span>               | <span data-ttu-id="d113b-103">Descrição</span><span class="sxs-lookup"><span data-stu-id="d113b-103">Description</span></span>|
| ----------------- | ------------ |
| <span data-ttu-id="d113b-104">-m</span><span class="sxs-lookup"><span data-stu-id="d113b-104">-m</span></span>  | <span data-ttu-id="d113b-105">O nome do modelo.</span><span class="sxs-lookup"><span data-stu-id="d113b-105">The name of the model.</span></span> |
| <span data-ttu-id="d113b-106">-dc</span><span class="sxs-lookup"><span data-stu-id="d113b-106">-dc</span></span>  | <span data-ttu-id="d113b-107">O contexto de dados.</span><span class="sxs-lookup"><span data-stu-id="d113b-107">The data context.</span></span> |
| <span data-ttu-id="d113b-108">-udl</span><span class="sxs-lookup"><span data-stu-id="d113b-108">-udl</span></span> | <span data-ttu-id="d113b-109">Use o layout padrão.</span><span class="sxs-lookup"><span data-stu-id="d113b-109">Use the default layout.</span></span> |
| <span data-ttu-id="d113b-110">-outDir</span><span class="sxs-lookup"><span data-stu-id="d113b-110">-outDir</span></span> | <span data-ttu-id="d113b-111">O caminho da pasta de saída relativa para criar as exibições.</span><span class="sxs-lookup"><span data-stu-id="d113b-111">The relative output folder path to create the views.</span></span> |
| <span data-ttu-id="d113b-112">--referenceScriptLibraries</span><span class="sxs-lookup"><span data-stu-id="d113b-112">--referenceScriptLibraries</span></span> | <span data-ttu-id="d113b-113">Adiciona `_ValidationScriptsPartial` para editar e criar páginas</span><span class="sxs-lookup"><span data-stu-id="d113b-113">Adds `_ValidationScriptsPartial` to Edit and Create pages</span></span> |

<span data-ttu-id="d113b-114">Use a opção `h` para obter ajuda sobre o comando `aspnet-codegenerator razorpage`:</span><span class="sxs-lookup"><span data-stu-id="d113b-114">Use the `h` switch to get help on the `aspnet-codegenerator razorpage` command:</span></span>

```console
dotnet aspnet-codegenerator razorpage -h
```

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="d113b-115">Testar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="d113b-115">Test the app</span></span>

* <span data-ttu-id="d113b-116">Executar o aplicativo e acrescentar `/Movies` à URL no navegador (`http://localhost:port/Movies`).</span><span class="sxs-lookup"><span data-stu-id="d113b-116">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/Movies`).</span></span>
* <span data-ttu-id="d113b-117">Teste o link **Criar**.</span><span class="sxs-lookup"><span data-stu-id="d113b-117">Test the **Create** link.</span></span>

  ![Criar página](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="d113b-119">Teste os links **Editar**, **Detalhes** e **Excluir**.</span><span class="sxs-lookup"><span data-stu-id="d113b-119">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="d113b-120">Se você receber um erro similar ao seguinte, verifique se você executou migrações e atualizou o banco de dados:</span><span class="sxs-lookup"><span data-stu-id="d113b-120">If you get the error similar to the following, verify you have run migrations and updated the database:</span></span>

`An unhandled exception occurred while processing the request. 'no such table: Movie'.`
