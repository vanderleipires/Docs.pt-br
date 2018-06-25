---
title: Introdução ao NSwag e ao ASP.NET Core
author: zuckerthoben
description: Saiba como usar o NSwag para gerar a documentação e as páginas de ajuda para uma API Web ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 05/08/2018
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: f4cc9ec1f32ef2bd0056ba8d0cbbbe9228834d85
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279187"
---
# <a name="get-started-with-nswag-and-aspnet-core"></a><span data-ttu-id="69c3d-103">Introdução ao NSwag e ao ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="69c3d-103">Get started with NSwag and ASP.NET Core</span></span>

<span data-ttu-id="69c3d-104">Por [Christoph Nienaber](https://twitter.com/zuckerthoben) e [Rico Suter](https://rsuter.com)</span><span class="sxs-lookup"><span data-stu-id="69c3d-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben) and [Rico Suter](https://rsuter.com)</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="69c3d-105">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="69c3d-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="69c3d-106">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="69c3d-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
::: moniker-end

<span data-ttu-id="69c3d-107">O uso do [NSwag](https://github.com/RSuter/NSwag) com o middleware ASP.NET Core exige o pacote do NuGet [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/).</span><span class="sxs-lookup"><span data-stu-id="69c3d-107">Using [NSwag](https://github.com/RSuter/NSwag) with ASP.NET Core middleware requires the [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet package.</span></span> <span data-ttu-id="69c3d-108">O pacote consiste em um gerador do Swagger, uma interface do usuário do Swagger (v2 e v3) e uma [interface do usuário do ReDoc](https://github.com/Rebilly/ReDoc).</span><span class="sxs-lookup"><span data-stu-id="69c3d-108">The package consists of a Swagger generator, Swagger UI (v2 and v3), and [ReDoc UI](https://github.com/Rebilly/ReDoc).</span></span>

<span data-ttu-id="69c3d-109">É altamente recomendável usar os recursos de geração de código do NSwag.</span><span class="sxs-lookup"><span data-stu-id="69c3d-109">It's highly recommended to make use of NSwag's code generation capabilities.</span></span> <span data-ttu-id="69c3d-110">Escolha uma das seguintes opções para a geração de código:</span><span class="sxs-lookup"><span data-stu-id="69c3d-110">Choose one of the following options for code generation:</span></span>

* <span data-ttu-id="69c3d-111">Use o [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), um aplicativo da área de trabalho do Windows para a geração de código do cliente em C# e em TypeScript para sua API.</span><span class="sxs-lookup"><span data-stu-id="69c3d-111">Use [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), a Windows desktop app for generating client code in C# and TypeScript for your API.</span></span>
* <span data-ttu-id="69c3d-112">Use os pacotes do NuGet [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) ou [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) para a geração de código dentro do projeto.</span><span class="sxs-lookup"><span data-stu-id="69c3d-112">Use the [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) or [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet packages to do code generation inside your project.</span></span>
* <span data-ttu-id="69c3d-113">Use o NSwag na [linha de comando](https://github.com/NSwag/NSwag/wiki/CommandLine).</span><span class="sxs-lookup"><span data-stu-id="69c3d-113">Use NSwag from the [command line](https://github.com/NSwag/NSwag/wiki/CommandLine).</span></span>
* <span data-ttu-id="69c3d-114">Use o pacote NuGet [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild).</span><span class="sxs-lookup"><span data-stu-id="69c3d-114">Use the [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) NuGet package.</span></span>

## <a name="features"></a><span data-ttu-id="69c3d-115">Recursos</span><span class="sxs-lookup"><span data-stu-id="69c3d-115">Features</span></span>

<span data-ttu-id="69c3d-116">O principal motivo para usar o NSwag é que além da capacidade de apresentar a interface do usuário do Swagger e o gerador do Swagger, também é possível usar recursos flexíveis para geração de código.</span><span class="sxs-lookup"><span data-stu-id="69c3d-116">The main reason to use NSwag is the ability to not only introduce the Swagger UI and Swagger generator, but to make use of the flexible code generation capabilities.</span></span> <span data-ttu-id="69c3d-117">Você não precisa de uma API existente. É possível usar APIs de terceiros que incorporam o Swagger e permitem que o NSwag gere uma implementação de cliente.</span><span class="sxs-lookup"><span data-stu-id="69c3d-117">You don't need an existing API&mdash;you can use third-party APIs that incorporate Swagger and let NSwag generate a client implementation.</span></span> <span data-ttu-id="69c3d-118">De qualquer forma, o ciclo de desenvolvimento é acelerado e você pode se adaptar às alterações da API com mais facilidade.</span><span class="sxs-lookup"><span data-stu-id="69c3d-118">Either way, the development cycle is expedited and you can more easily adapt to API changes.</span></span>

## <a name="package-installation"></a><span data-ttu-id="69c3d-119">Instalação do pacote</span><span class="sxs-lookup"><span data-stu-id="69c3d-119">Package installation</span></span>

<span data-ttu-id="69c3d-120">O pacote do NuGet do NSwag pode ser adicionado com as seguintes abordagens:</span><span class="sxs-lookup"><span data-stu-id="69c3d-120">The NSwag NuGet package can be added with the following approaches:</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="69c3d-121">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="69c3d-121">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="69c3d-122">Da janela **Console do Gerenciador de Pacotes**:</span><span class="sxs-lookup"><span data-stu-id="69c3d-122">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="69c3d-123">Acesse **Exibição** > **Outras Janelas** > **Console do Gerenciador de Pacotes**</span><span class="sxs-lookup"><span data-stu-id="69c3d-123">Go to **View** > **Other Windows** > **Package Manager Console**</span></span>
  * <span data-ttu-id="69c3d-124">Navegue para o diretório no qual o arquivo *TodoApi.csproj* está localizado</span><span class="sxs-lookup"><span data-stu-id="69c3d-124">Navigate to the directory in which the *TodoApi.csproj* file exists</span></span>
  * <span data-ttu-id="69c3d-125">Execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="69c3d-125">Execute the following command:</span></span>

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* <span data-ttu-id="69c3d-126">Da caixa de diálogo **Gerenciar Pacotes NuGet**:</span><span class="sxs-lookup"><span data-stu-id="69c3d-126">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="69c3d-127">Clique com o botão direito do mouse no projeto em **Gerenciador de Soluções** > **Gerenciar Pacotes NuGet**</span><span class="sxs-lookup"><span data-stu-id="69c3d-127">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="69c3d-128">Defina a **Origem do pacote** para "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="69c3d-128">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="69c3d-129">Insira "NSwag.AspNetCore" na caixa de pesquisa</span><span class="sxs-lookup"><span data-stu-id="69c3d-129">Enter "NSwag.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="69c3d-130">Selecione o pacote "NSwag.AspNetCore" na guia **Procurar** e clique em **Instalar**</span><span class="sxs-lookup"><span data-stu-id="69c3d-130">Select the "NSwag.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="69c3d-131">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="69c3d-131">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="69c3d-132">Clique com o botão direito do mouse na pasta *Pacotes* em **Painel de Soluções** > **Adicionar Pacotes...**</span><span class="sxs-lookup"><span data-stu-id="69c3d-132">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="69c3d-133">Defina a lista suspensa **Origem** da janela **Adicionar Pacotes** para "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="69c3d-133">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="69c3d-134">Insira "NSwag.AspNetCore" na caixa de pesquisa</span><span class="sxs-lookup"><span data-stu-id="69c3d-134">Enter "NSwag.AspNetCore" in the search box</span></span>
* <span data-ttu-id="69c3d-135">Selecione o pacote "NSwag.AspNetCore" no painel de resultados e clique em **Adicionar Pacote**</span><span class="sxs-lookup"><span data-stu-id="69c3d-135">Select the "NSwag.AspNetCore" package from the results pane and click **Add Package**</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="69c3d-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="69c3d-136">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="69c3d-137">Execute o comando a seguir do **Terminal Integrado**:</span><span class="sxs-lookup"><span data-stu-id="69c3d-137">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="69c3d-138">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="69c3d-138">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="69c3d-139">Execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="69c3d-139">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="69c3d-140">Adicionar e configurar o middleware do Swagger</span><span class="sxs-lookup"><span data-stu-id="69c3d-140">Add and configure Swagger middleware</span></span>

<span data-ttu-id="69c3d-141">Importe os seguintes namespaces na classe `Startup`:</span><span class="sxs-lookup"><span data-stu-id="69c3d-141">Import the following namespaces in the `Startup` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_StartupConfigureImports)]

<span data-ttu-id="69c3d-142">No método `Startup.Configure`, habilite o middleware para atender à especificação do Swagger gerado e à interface do usuário do Swagger:</span><span class="sxs-lookup"><span data-stu-id="69c3d-142">In the `Startup.Configure` method, enable the middleware for serving the generated Swagger specification and the Swagger UI:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=6-10)]

<span data-ttu-id="69c3d-143">Inicie o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="69c3d-143">Launch the app.</span></span> <span data-ttu-id="69c3d-144">Navegue até `http://localhost:<port>/swagger` para exibir a interface do usuário do Swagger.</span><span class="sxs-lookup"><span data-stu-id="69c3d-144">Navigate to `http://localhost:<port>/swagger` to view the Swagger UI.</span></span> <span data-ttu-id="69c3d-145">Navegue até `http://localhost:<port>/swagger/v1/swagger.json` para exibir a especificação do Swagger.</span><span class="sxs-lookup"><span data-stu-id="69c3d-145">Navigate to `http://localhost:<port>/swagger/v1/swagger.json` to view the Swagger specification.</span></span>

## <a name="code-generation"></a><span data-ttu-id="69c3d-146">Geração de código</span><span class="sxs-lookup"><span data-stu-id="69c3d-146">Code generation</span></span>

### <a name="via-nswagstudio"></a><span data-ttu-id="69c3d-147">Por meio do NSwagStudio</span><span class="sxs-lookup"><span data-stu-id="69c3d-147">Via NSwagStudio</span></span>

* <span data-ttu-id="69c3d-148">Instale o NSwagStudio por meio do [repositório GitHub](https://github.com/RSuter/NSwag/wiki/NSwagStudio) oficial.</span><span class="sxs-lookup"><span data-stu-id="69c3d-148">Install NSwagStudio from the official [GitHub repository](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span></span>
* <span data-ttu-id="69c3d-149">Inicie o NSwagStudio.</span><span class="sxs-lookup"><span data-stu-id="69c3d-149">Launch NSwagStudio.</span></span> <span data-ttu-id="69c3d-150">Insira a URL do arquivo *swagger.json* na caixa de texto **URL de Especificação do Swagger** e clique no botão **Criar Cópia local**.</span><span class="sxs-lookup"><span data-stu-id="69c3d-150">Enter the *swagger.json* file URL in the **Swagger Specification URL** textbox, and click the **Create local Copy** button.</span></span>
* <span data-ttu-id="69c3d-151">Selecione o tipo de saída do cliente **Cliente do CSharp**.</span><span class="sxs-lookup"><span data-stu-id="69c3d-151">Select the **CSharp Client** client output type.</span></span> <span data-ttu-id="69c3d-152">Outras opções incluem **Cliente do TypeScript** e **Controlador da API Web do CSharp**.</span><span class="sxs-lookup"><span data-stu-id="69c3d-152">Other options include **TypeScript Client** and **CSharp Web API Controller**.</span></span> <span data-ttu-id="69c3d-153">O uso de um controlador da API Web é basicamente uma geração inversa.</span><span class="sxs-lookup"><span data-stu-id="69c3d-153">Using a Web API Controller is basically a reverse generation.</span></span> <span data-ttu-id="69c3d-154">Ele usa uma especificação de um serviço para recriar o serviço.</span><span class="sxs-lookup"><span data-stu-id="69c3d-154">It uses a specification of a service to rebuild the service.</span></span>
* <span data-ttu-id="69c3d-155">Clique no botão **Gerar Saídas**.</span><span class="sxs-lookup"><span data-stu-id="69c3d-155">Click the **Generate Outputs** button.</span></span> <span data-ttu-id="69c3d-156">Uma implementação completa de cliente em C# do projeto *TodoApi.NSwag* é produzida.</span><span class="sxs-lookup"><span data-stu-id="69c3d-156">A complete C# client implementation of the *TodoApi.NSwag* project is produced.</span></span> <span data-ttu-id="69c3d-157">Clique na guia **Cliente do CSharp** da seção **Saídas** para ver o código de cliente gerado:</span><span class="sxs-lookup"><span data-stu-id="69c3d-157">Click the **CSharp Client** tab of the **Outputs** section to see the generated client code:</span></span>

```csharp
//----------------------
// <auto-generated>
//     Generated using the NSwag toolchain v11.17.3.0 (NJsonSchema v9.10.46.0 (Newtonsoft.Json v9.0.0.0)) (http://NSwag.org)
// </auto-generated>
//----------------------

namespace MyNamespace
{
    #pragma warning disable // Disable all warnings

    [System.CodeDom.Compiler.GeneratedCode("NSwag",
        "11.17.3.0 (NJsonSchema v9.10.46.0 (Newtonsoft.Json v9.0.0.0))")]
    public partial class TodoClient
    {
        private string _baseUrl = "http://localhost:50499";
        private System.Lazy<Newtonsoft.Json.JsonSerializerSettings> _settings;

        public TodoClient()
        {
            _settings = new System.Lazy
                <Newtonsoft.Json.JsonSerializerSettings>(() =>
            {
                var settings = new Newtonsoft.Json.JsonSerializerSettings();
                UpdateJsonSerializerSettings(settings);
                return settings;
            });
        }

        public string BaseUrl
        {
            get { return _baseUrl; }
            set { _baseUrl = value; }
        }

        // code omitted for brevity
```

> [!TIP]
> <span data-ttu-id="69c3d-158">O código de cliente em C# é gerado com base nas configurações definidas na guia **Configurações** da guia **Cliente do CSharp**. Modifique as configurações para executar tarefas como geração de método síncrono e renomeação de namespace padrão.</span><span class="sxs-lookup"><span data-stu-id="69c3d-158">The C# client code is generated based on settings defined in the **Settings** tab of the **CSharp Client** tab. Modify the settings to perform tasks such as default namespace renaming and synchronous method generation.</span></span>

* <span data-ttu-id="69c3d-159">Copie o código C# gerado para um arquivo em um projeto de cliente (por exemplo, um aplicativo [Xamarin.Forms](/xamarin/xamarin-forms/)).</span><span class="sxs-lookup"><span data-stu-id="69c3d-159">Copy the generated C# code into a file in a client project (for example, a [Xamarin.Forms](/xamarin/xamarin-forms/) app).</span></span>
* <span data-ttu-id="69c3d-160">Inicie o consumo da API Web:</span><span class="sxs-lookup"><span data-stu-id="69c3d-160">Start consuming the web API:</span></span>

```csharp
var todoClient = new TodoClient();

// Gets all to-dos from the API
var allTodos = await todoClient.GetAllAsync();

// Create a new TodoItem, and save it in the API
var createdTodo = await todoClient.CreateAsync(new TodoItem());

// Get a single to-do by ID
var foundTodo = await todoClient.GetByIdAsync(1);
```

> [!NOTE]
> <span data-ttu-id="69c3d-161">Você pode injetar uma URL base e/ou um cliente HTTP no cliente da API.</span><span class="sxs-lookup"><span data-stu-id="69c3d-161">You can inject a base URL and/or a HTTP client into the API client.</span></span> <span data-ttu-id="69c3d-162">A melhor prática é sempre [reutilizar o HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span><span class="sxs-lookup"><span data-stu-id="69c3d-162">The best practice is to always [reuse the HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span></span>

### <a name="other-ways-to-generate-client-code"></a><span data-ttu-id="69c3d-163">Outras maneiras de gerar o código do cliente</span><span class="sxs-lookup"><span data-stu-id="69c3d-163">Other ways to generate client code</span></span>

<span data-ttu-id="69c3d-164">Você pode gerar o código de cliente de outras maneiras mais adequadas ao seu fluxo de trabalho:</span><span class="sxs-lookup"><span data-stu-id="69c3d-164">You can generate the client code in other ways, more suited to your workflow:</span></span>

* [<span data-ttu-id="69c3d-165">MSBuild</span><span class="sxs-lookup"><span data-stu-id="69c3d-165">MSBuild</span></span>](https://www.nuget.org/packages/NSwag.MSBuild/)

* [<span data-ttu-id="69c3d-166">No código</span><span class="sxs-lookup"><span data-stu-id="69c3d-166">In code</span></span>](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [<span data-ttu-id="69c3d-167">Modelos T4</span><span class="sxs-lookup"><span data-stu-id="69c3d-167">T4 templates</span></span>](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customize"></a><span data-ttu-id="69c3d-168">Personalizar</span><span class="sxs-lookup"><span data-stu-id="69c3d-168">Customize</span></span>

<span data-ttu-id="69c3d-169">O Swagger fornece opções para documentar o modelo de objeto para facilitar o consumo da API Web.</span><span class="sxs-lookup"><span data-stu-id="69c3d-169">Swagger provides options for documenting the object model to ease consumption of the web API.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="69c3d-170">Descrição e informações da API</span><span class="sxs-lookup"><span data-stu-id="69c3d-170">API info and description</span></span>

<span data-ttu-id="69c3d-171">No método `Startup.Configure`, uma ação de configuração passada para o método `UseSwagger` adiciona informações como o autor, a licença e a descrição:</span><span class="sxs-lookup"><span data-stu-id="69c3d-171">In the `Startup.Configure` method, a configuration action passed to the `UseSwagger` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup2.cs?name=snippet_UseSwagger)]

<span data-ttu-id="69c3d-172">A interface do usuário do Swagger exibe as informações da versão:</span><span class="sxs-lookup"><span data-stu-id="69c3d-172">The Swagger UI displays the version's information:</span></span>

![Interface do usuário do Swagger com informações de versão](web-api-help-pages-using-swagger/_static/custom-info-nswag.png)

### <a name="xml-comments"></a><span data-ttu-id="69c3d-174">comentários XML</span><span class="sxs-lookup"><span data-stu-id="69c3d-174">XML comments</span></span>

<span data-ttu-id="69c3d-175">Os comentários XML podem ser habilitados com as seguintes abordagens:</span><span class="sxs-lookup"><span data-stu-id="69c3d-175">XML comments are enabled with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio-xml"></a>[<span data-ttu-id="69c3d-176">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="69c3d-176">Visual Studio</span></span>](#tab/visual-studio-xml/)

* <span data-ttu-id="69c3d-177">Clique com o botão direito do mouse no projeto no **Gerenciador de Soluções** e selecione **Propriedades**</span><span class="sxs-lookup"><span data-stu-id="69c3d-177">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="69c3d-178">Marque a caixa **Arquivo de documentação XML** na seção **Saída** da guia **Build**</span><span class="sxs-lookup"><span data-stu-id="69c3d-178">Check the **XML documentation file** box under the **Output** section of the **Build** tab</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[<span data-ttu-id="69c3d-179">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="69c3d-179">Visual Studio for Mac</span></span>](#tab/visual-studio-mac-xml/)

* <span data-ttu-id="69c3d-180">Abra a caixa de diálogo **Opções do Projeto** > **Compilar** > **Compilador**</span><span class="sxs-lookup"><span data-stu-id="69c3d-180">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="69c3d-181">Marque a caixa **Gerar documentação XML** na seção **Opções Gerais**</span><span class="sxs-lookup"><span data-stu-id="69c3d-181">Check the **Generate xml documentation** box under the **General Options** section</span></span>

# <a name="visual-studio-codetabvisual-studio-code-xml"></a>[<span data-ttu-id="69c3d-182">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="69c3d-182">Visual Studio Code</span></span>](#tab/visual-studio-code-xml/)

<span data-ttu-id="69c3d-183">Adicione manualmente o trecho a seguir ao arquivo *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="69c3d-183">Manually add the following snippet to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement)]

---

### <a name="data-annotations"></a><span data-ttu-id="69c3d-184">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="69c3d-184">Data annotations</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="69c3d-185">O NSwag usa [Reflection](/dotnet/csharp/programming-guide/concepts/reflection) e o tipo de retorno recomendado para as ações da API Web é [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span><span class="sxs-lookup"><span data-stu-id="69c3d-185">NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span></span> <span data-ttu-id="69c3d-186">Consequentemente, o NSwag não consegue inferir o que a sua ação está executando e o que ela retorna.</span><span class="sxs-lookup"><span data-stu-id="69c3d-186">Consequently, NSwag can't infer what your action is doing and what it returns.</span></span> <span data-ttu-id="69c3d-187">Considere o exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="69c3d-187">Consider the following example:</span></span>

<span data-ttu-id="69c3d-188">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]</span><span class="sxs-lookup"><span data-stu-id="69c3d-188">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]</span></span>

<span data-ttu-id="69c3d-189">A ação anterior retorna `IActionResult`, mas dentro da ação, ela está retornando [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) ou [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span><span class="sxs-lookup"><span data-stu-id="69c3d-189">The preceding action returns `IActionResult`, but inside the action it's returning either [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) or [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span></span> <span data-ttu-id="69c3d-190">As anotações de dados são usadas para informar os clientes de quais códigos de status HTTP essa ação deverá retornar.</span><span class="sxs-lookup"><span data-stu-id="69c3d-190">Data annotations are used to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="69c3d-191">Decore a ação com os seguintes atributos:</span><span class="sxs-lookup"><span data-stu-id="69c3d-191">Decorate the action with the following attributes:</span></span>

<span data-ttu-id="69c3d-192">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]</span><span class="sxs-lookup"><span data-stu-id="69c3d-192">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="69c3d-193">O NSwag usa [Reflection](/dotnet/csharp/programming-guide/concepts/reflection) e o tipo de retorno recomendado para as ações da API Web é [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1).</span><span class="sxs-lookup"><span data-stu-id="69c3d-193">NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1).</span></span> <span data-ttu-id="69c3d-194">Consequentemente, o NSwag pode inferir apenas o tipo de retorno definido por `T`.</span><span class="sxs-lookup"><span data-stu-id="69c3d-194">Consequently, NSwag can only infer the return type defined by `T`.</span></span> <span data-ttu-id="69c3d-195">Outros tipos de retorno possíveis na ação não podem ser inferidos.</span><span class="sxs-lookup"><span data-stu-id="69c3d-195">Other possible return types in the action cannot be inferred.</span></span> <span data-ttu-id="69c3d-196">Considere o exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="69c3d-196">Consider the following example:</span></span>

<span data-ttu-id="69c3d-197">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]</span><span class="sxs-lookup"><span data-stu-id="69c3d-197">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]</span></span>

<span data-ttu-id="69c3d-198">A ação anterior retorna `ActionResult<T>`, mas dentro da ação, ela está retornando [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute).</span><span class="sxs-lookup"><span data-stu-id="69c3d-198">The preceding action returns `ActionResult<T>`, but inside the action it's returning either [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute).</span></span> <span data-ttu-id="69c3d-199">Como o controlador está decorado com o atributo [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute), uma resposta [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest) também é possível.</span><span class="sxs-lookup"><span data-stu-id="69c3d-199">Since the controller is decorated with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute, a [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest) response is possible too.</span></span> <span data-ttu-id="69c3d-200">Confira [Respostas automáticas HTTP 400](xref:web-api/index#automatic-http-400-responses) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="69c3d-200">See [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses) for more info.</span></span> <span data-ttu-id="69c3d-201">As anotações de dados são usadas para informar os clientes de quais códigos de status HTTP essa ação deverá retornar.</span><span class="sxs-lookup"><span data-stu-id="69c3d-201">Data annotations are used to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="69c3d-202">Decore a ação com os seguintes atributos:</span><span class="sxs-lookup"><span data-stu-id="69c3d-202">Decorate the action with the following attributes:</span></span>

<span data-ttu-id="69c3d-203">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]</span><span class="sxs-lookup"><span data-stu-id="69c3d-203">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]</span></span>
::: moniker-end

<span data-ttu-id="69c3d-204">O gerador do Swagger agora pode descrever essa ação precisamente e os clientes gerados conseguem saber o que recebem ao chamar o ponto de extremidade.</span><span class="sxs-lookup"><span data-stu-id="69c3d-204">The Swagger generator can now accurately describe this action, and generated clients know what they receive when calling the endpoint.</span></span> <span data-ttu-id="69c3d-205">É altamente recomendável decorar todas as ações com esses atributos.</span><span class="sxs-lookup"><span data-stu-id="69c3d-205">Decorating all actions with these attributes is highly recommended.</span></span> <span data-ttu-id="69c3d-206">Para obter diretrizes sobre quais respostas HTTP as ações da API devem retornar, confira a [Especificação RFC 7231](https://tools.ietf.org/html/rfc7231#section-4.3).</span><span class="sxs-lookup"><span data-stu-id="69c3d-206">For guidelines on what HTTP responses your API actions should return, see the [RFC 7231 specification](https://tools.ietf.org/html/rfc7231#section-4.3).</span></span>
