<span data-ttu-id="77990-101">Execute o scaffolder de identidade:</span><span class="sxs-lookup"><span data-stu-id="77990-101">Run the Identity scaffolder:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="77990-102">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="77990-102">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="77990-103">Partir **Gerenciador de soluções**, clique com botão direito no projeto > **Add** > **New Scaffolded Item**.</span><span class="sxs-lookup"><span data-stu-id="77990-103">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="77990-104">No painel à esquerda do **adicionar Scaffold** caixa de diálogo, selecione **identidade** > **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="77990-104">From the left pane of the **Add Scaffold** dialog, select **Identity** > **ADD**.</span></span>
* <span data-ttu-id="77990-105">No **identidade de adição** caixa de diálogo, selecione as opções desejadas.</span><span class="sxs-lookup"><span data-stu-id="77990-105">In the **ADD Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="77990-106">Selecione a página de layout existente ou seu arquivo de layout será substituído pela marcação incorreta.</span><span class="sxs-lookup"><span data-stu-id="77990-106">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="77990-107">Quando um arquivo layout. cshtml existente é selecionado, vale **não** substituídos.</span><span class="sxs-lookup"><span data-stu-id="77990-107">When an existing _Layout.cshtml file is selected, it is **not** overwritten.</span></span>

 <span data-ttu-id="77990-108">Por exemplo `~/Pages/Shared/_Layout.cshtml` para as páginas Razor `~/Views/Shared/_Layout.cshtml` para projetos MVC</span><span class="sxs-lookup"><span data-stu-id="77990-108">For example `~/Pages/Shared/_Layout.cshtml` for Razor Pages `~/Views/Shared/_Layout.cshtml` for MVC projects</span></span>
* <span data-ttu-id="77990-109">Para usar o contexto de dados existente, selecione pelo menos um arquivo para substituir.</span><span class="sxs-lookup"><span data-stu-id="77990-109">To use your existing data context, select at least one file to override.</span></span> <span data-ttu-id="77990-110">Você deve selecionar pelo menos um arquivo para adicionar seu contexto de dados.</span><span class="sxs-lookup"><span data-stu-id="77990-110">You must select at least one file to add your data context.</span></span>
  * <span data-ttu-id="77990-111">Selecione sua classe de contexto de dados.</span><span class="sxs-lookup"><span data-stu-id="77990-111">Select your data context class.</span></span>
  * <span data-ttu-id="77990-112">Selecione **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="77990-112">Select **ADD**.</span></span>
* <span data-ttu-id="77990-113">Para criar um novo contexto de usuário e, possivelmente, crie uma classe de usuário personalizada para a identidade:</span><span class="sxs-lookup"><span data-stu-id="77990-113">To create a new user context and possibly create a custom user class for Identity:</span></span>
  * <span data-ttu-id="77990-114">Selecione o **+** botão para criar um novo **classe de contexto de dados**.</span><span class="sxs-lookup"><span data-stu-id="77990-114">Select the **+** button to create a new **Data context class**.</span></span>
  * <span data-ttu-id="77990-115">Selecione **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="77990-115">Select **ADD**.</span></span>

<span data-ttu-id="77990-116">Observação: Se você estiver criando um novo contexto de usuário, você não precisa selecionar um arquivo para substituir.</span><span class="sxs-lookup"><span data-stu-id="77990-116">Note: If you're creating a new user context, you don't have to select a file to override.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="77990-117">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="77990-117">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="77990-118">Se você já não tiver instalado o scaffolder ASP.NET, instale-o agora:</span><span class="sxs-lookup"><span data-stu-id="77990-118">If you have not previously installed the ASP.NET scaffolder, install it now:</span></span>

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="77990-119">Adicione uma referência de pacote ao [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) ao projeto (\*. csproj) arquivos.</span><span class="sxs-lookup"><span data-stu-id="77990-119">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (\*.csproj) file.</span></span> <span data-ttu-id="77990-120">Execute o seguinte comando no diretório do projeto:</span><span class="sxs-lookup"><span data-stu-id="77990-120">Run the following command in the project directory:</span></span>

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="77990-121">Execute o seguinte comando para listar as opções de scaffolder de identidade:</span><span class="sxs-lookup"><span data-stu-id="77990-121">Run the following command to list the Identity scaffolder options:</span></span>

```cli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="77990-122">Na pasta do projeto, execute o scaffolder de identidade com as opções desejadas.</span><span class="sxs-lookup"><span data-stu-id="77990-122">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="77990-123">Por exemplo, para configurar a identidade com a interface do usuário padrão e o número mínimo de arquivos, execute o comando a seguir.</span><span class="sxs-lookup"><span data-stu-id="77990-123">For example, to setup identity with the default UI and the minimum number of files, run the following command.</span></span> <span data-ttu-id="77990-124">Use o nome totalmente qualificado correto para o contexto de banco de dados:</span><span class="sxs-lookup"><span data-stu-id="77990-124">Use the correct fully qualified name for your DB context:</span></span>

```cli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files Account.Register
```

-------------
