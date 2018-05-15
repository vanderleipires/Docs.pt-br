---
title: Introdução ao NSwag e ao ASP.NET Core
author: zuckerthoben
description: Saiba como usar o NSwag para gerar a documentação e as páginas de ajuda para um aplicativo de API Web ASP.NET Core.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 03/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: 80e6a9e1702d8f68d139d2ff9c3a01a27c40cecb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-nswag-and-aspnet-core"></a><span data-ttu-id="e05c9-103">Introdução ao NSwag e ao ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e05c9-103">Get started with NSwag and ASP.NET Core</span></span>

<span data-ttu-id="e05c9-104">Por [Christoph Nienaber](https://twitter.com/zuckerthoben) e [Rico Suter](https://rsuter.com)</span><span class="sxs-lookup"><span data-stu-id="e05c9-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben) and [Rico Suter](https://rsuter.com)</span></span>

<span data-ttu-id="e05c9-105">O uso do [NSwag](https://github.com/RSuter/NSwag) com o middleware ASP.NET Core exige o pacote do NuGet [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/).</span><span class="sxs-lookup"><span data-stu-id="e05c9-105">Using [NSwag](https://github.com/RSuter/NSwag) with ASP.NET Core middleware requires the [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet package.</span></span> <span data-ttu-id="e05c9-106">O pacote consiste em um gerador do Swagger, uma interface do usuário do Swagger (v2 e v3) e uma [interface do usuário do ReDoc](https://github.com/Rebilly/ReDoc).</span><span class="sxs-lookup"><span data-stu-id="e05c9-106">The package consists of a Swagger generator, Swagger UI (v2 and v3), and [ReDoc UI](https://github.com/Rebilly/ReDoc).</span></span>

<span data-ttu-id="e05c9-107">É altamente recomendável usar os recursos de geração de código do NSwag.</span><span class="sxs-lookup"><span data-stu-id="e05c9-107">It's highly recommended to make use of NSwag's code generation capabilities.</span></span> <span data-ttu-id="e05c9-108">Escolha uma das seguintes opções para a geração de código:</span><span class="sxs-lookup"><span data-stu-id="e05c9-108">Choose one of the following options for code generation:</span></span>

* <span data-ttu-id="e05c9-109">Use o [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), um aplicativo da área de trabalho do Windows para a geração de código do cliente em C# e em TypeScript para sua API</span><span class="sxs-lookup"><span data-stu-id="e05c9-109">Use [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), a Windows desktop app for generating client code in C# and TypeScript for your API</span></span>
* <span data-ttu-id="e05c9-110">Use os pacotes do NuGet [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) ou [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) para a geração de código dentro do projeto</span><span class="sxs-lookup"><span data-stu-id="e05c9-110">Use the [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) or [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet packages to do code generation inside your project</span></span>
* <span data-ttu-id="e05c9-111">Use o NSwag da [linha de comando](https://github.com/NSwag/NSwag/wiki/CommandLine)</span><span class="sxs-lookup"><span data-stu-id="e05c9-111">Use NSwag from the [command line](https://github.com/NSwag/NSwag/wiki/CommandLine)</span></span>
* <span data-ttu-id="e05c9-112">Use o pacote do NuGet [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild)</span><span class="sxs-lookup"><span data-stu-id="e05c9-112">Use the [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) NuGet package</span></span>

## <a name="features"></a><span data-ttu-id="e05c9-113">Recursos</span><span class="sxs-lookup"><span data-stu-id="e05c9-113">Features</span></span>

<span data-ttu-id="e05c9-114">O principal motivo para usar o NSwag é que além da capacidade de apresentar a interface do usuário do Swagger e o gerador do Swagger, também é possível usar recursos flexíveis para geração de código.</span><span class="sxs-lookup"><span data-stu-id="e05c9-114">The main reason to use NSwag is the ability to not only introduce the Swagger UI and Swagger generator, but to make use of the flexible code generation capabilities.</span></span> <span data-ttu-id="e05c9-115">Você não precisa de uma API existente. É possível usar APIs de terceiros que incorporam o Swagger e permitem que o NSwag gere uma implementação de cliente.</span><span class="sxs-lookup"><span data-stu-id="e05c9-115">You don't need an existing API&mdash;you can use third-party APIs that incorporate Swagger and let NSwag generate a client implementation.</span></span> <span data-ttu-id="e05c9-116">De qualquer forma, o ciclo de desenvolvimento é acelerado e você pode se adaptar às alterações da API com mais facilidade.</span><span class="sxs-lookup"><span data-stu-id="e05c9-116">Either way, the development cycle is expedited and you can more easily adapt to API changes.</span></span>

## <a name="package-installation"></a><span data-ttu-id="e05c9-117">Instalação do pacote</span><span class="sxs-lookup"><span data-stu-id="e05c9-117">Package installation</span></span>

<span data-ttu-id="e05c9-118">O pacote do NuGet do NSwag pode ser adicionado com as seguintes abordagens:</span><span class="sxs-lookup"><span data-stu-id="e05c9-118">The NSwag NuGet package can be added with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e05c9-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e05c9-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e05c9-120">Da janela **Console do Gerenciador de Pacotes**:</span><span class="sxs-lookup"><span data-stu-id="e05c9-120">From the **Package Manager Console** window:</span></span>

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* <span data-ttu-id="e05c9-121">Da caixa de diálogo **Gerenciar Pacotes NuGet**:</span><span class="sxs-lookup"><span data-stu-id="e05c9-121">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="e05c9-122">Clique com o botão direito do mouse no projeto em **Gerenciador de Soluções** > **Gerenciar Pacotes NuGet**</span><span class="sxs-lookup"><span data-stu-id="e05c9-122">Right-click your project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="e05c9-123">Defina a **Origem do pacote** para "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="e05c9-123">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="e05c9-124">Insira "NSwag.AspNetCore" na caixa de pesquisa</span><span class="sxs-lookup"><span data-stu-id="e05c9-124">Enter "NSwag.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="e05c9-125">Selecione o pacote "NSwag.AspNetCore" na guia **Procurar** e clique em **Instalar**</span><span class="sxs-lookup"><span data-stu-id="e05c9-125">Select the "NSwag.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e05c9-126">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="e05c9-126">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e05c9-127">Clique com o botão direito do mouse na pasta *Pacotes* em **Painel de Soluções** > **Adicionar Pacotes...**</span><span class="sxs-lookup"><span data-stu-id="e05c9-127">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="e05c9-128">Defina a lista suspensa **Origem** da janela **Adicionar Pacotes** para "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="e05c9-128">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="e05c9-129">Insira NSwag.AspNetCore na caixa de pesquisa</span><span class="sxs-lookup"><span data-stu-id="e05c9-129">Enter NSwag.AspNetCore in the search box</span></span>
* <span data-ttu-id="e05c9-130">Selecione o pacote NSwag.AspNetCore no painel de resultados e clique em **Adicionar Pacote**</span><span class="sxs-lookup"><span data-stu-id="e05c9-130">Select the NSwag.AspNetCore package from the results pane and click **Add Package**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e05c9-131">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e05c9-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="e05c9-132">Execute o comando a seguir do **Terminal Integrado**:</span><span class="sxs-lookup"><span data-stu-id="e05c9-132">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.NSwag.csproj package NSwag.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e05c9-133">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="e05c9-133">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="e05c9-134">Execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="e05c9-134">Run the following command:</span></span>

```console
dotnet add TodoApi.NSwag.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="e05c9-135">Adicionar e configurar o middleware do Swagger</span><span class="sxs-lookup"><span data-stu-id="e05c9-135">Add and configure Swagger middleware</span></span>

<span data-ttu-id="e05c9-136">Importe os seguintes namespaces na classe `Info`:</span><span class="sxs-lookup"><span data-stu-id="e05c9-136">Import the following namespaces in the `Info` class:</span></span>

```csharp
using NSwag.AspNetCore;
using System.Reflection;
using NJsonSchema;
```

<span data-ttu-id="e05c9-137">No método `Startup.Configure`, habilite o middleware para atender à especificação do Swagger gerado e à interface do usuário do Swagger:</span><span class="sxs-lookup"><span data-stu-id="e05c9-137">In the `Startup.Configure` method, enable the middleware for serving the generated Swagger specification and the Swagger UI:</span></span>

[!code-cs[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=4,7-10)]

<span data-ttu-id="e05c9-138">Inicie o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e05c9-138">Launch the app.</span></span> <span data-ttu-id="e05c9-139">Navegue até `/swagger` para exibir a interface do usuário do Swagger.</span><span class="sxs-lookup"><span data-stu-id="e05c9-139">Navigate to `/swagger` to view the Swagger UI.</span></span> <span data-ttu-id="e05c9-140">Navegue até `/swagger/v1/swagger.json` para exibir a especificação do Swagger.</span><span class="sxs-lookup"><span data-stu-id="e05c9-140">Navigate to `/swagger/v1/swagger.json` to view the Swagger specification.</span></span>

## <a name="code-generation"></a><span data-ttu-id="e05c9-141">Geração de código</span><span class="sxs-lookup"><span data-stu-id="e05c9-141">Code generation</span></span>

### <a name="via-nswagstudio"></a><span data-ttu-id="e05c9-142">Por meio do NSwagStudio</span><span class="sxs-lookup"><span data-stu-id="e05c9-142">Via NSwagStudio</span></span>

* <span data-ttu-id="e05c9-143">Instale o `NSwagStudio` por meio do [Repositório do GitHub](https://github.com/RSuter/NSwag/wiki/NSwagStudio) oficial.</span><span class="sxs-lookup"><span data-stu-id="e05c9-143">Install `NSwagStudio` from the official [GitHub repository](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span></span>

* <span data-ttu-id="e05c9-144">Inicie o NSwagStudio.</span><span class="sxs-lookup"><span data-stu-id="e05c9-144">Launch NSwagStudio.</span></span> <span data-ttu-id="e05c9-145">Insira o local do *swagger.json* ou copie-o diretamente:</span><span class="sxs-lookup"><span data-stu-id="e05c9-145">Enter the location of your *swagger.json* or copy it directly:</span></span>

![NSwagStudio](web-api-help-pages-using-swagger/_static/NSwagStudio.png)

* <span data-ttu-id="e05c9-147">Indique o tipo de saída do cliente desejado.</span><span class="sxs-lookup"><span data-stu-id="e05c9-147">Indicate the desired client output type.</span></span> <span data-ttu-id="e05c9-148">As opções incluem **Cliente TypeScript**, **Cliente CSharp** ou **Controlador da API Web do CSharp**.</span><span class="sxs-lookup"><span data-stu-id="e05c9-148">Options include **TypeScript Client**, **CSharp Client**, or **CSharp Web API Controller**.</span></span> <span data-ttu-id="e05c9-149">O uso de um controlador da API Web é basicamente uma geração inversa.</span><span class="sxs-lookup"><span data-stu-id="e05c9-149">Using a Web API Controller is basically a reverse generation.</span></span> <span data-ttu-id="e05c9-150">Ele usa uma especificação de um serviço para recriar o serviço.</span><span class="sxs-lookup"><span data-stu-id="e05c9-150">It uses a specification of a service to rebuild the service.</span></span>

* <span data-ttu-id="e05c9-151">Clique em **Gerar Saídas**.</span><span class="sxs-lookup"><span data-stu-id="e05c9-151">Click **Generate Outputs**.</span></span>

* <span data-ttu-id="e05c9-152">Veja aqui uma implementação de cliente completa do exemplo *TodoApi.NSwag* em C#:</span><span class="sxs-lookup"><span data-stu-id="e05c9-152">Here you see a complete client implementation of the *TodoApi.NSwag* sample in C#:</span></span>

![NSwagStudio-Output](web-api-help-pages-using-swagger/_static/NSwagStudio-Output.png)

* <span data-ttu-id="e05c9-154">Coloque o arquivo em um projeto de cliente (por exemplo, um aplicativo [Xamarin.Forms](/xamarin/xamarin-forms/)).</span><span class="sxs-lookup"><span data-stu-id="e05c9-154">Put the file into a client project (for example, a [Xamarin.Forms](/xamarin/xamarin-forms/) app).</span></span> <span data-ttu-id="e05c9-155">Inicie o consumo da API:</span><span class="sxs-lookup"><span data-stu-id="e05c9-155">Start consuming the API:</span></span>

```csharp
var todoClient = new TodoClient();

// Gets all Todos from the Api
var allTodos = await todoClient.GetAllAsync();

// Create a new TodoItem and save it in the Api
var createdTodo = await todoClient.CreateAsync(new TodoItem);

// Get a single Todo by Id
var foundTodo = await todoClient.GetByIdAsync(1);
```

> [!NOTE]
> <span data-ttu-id="e05c9-156">Você pode injetar uma URL base e/ou um cliente HTTP no cliente da API.</span><span class="sxs-lookup"><span data-stu-id="e05c9-156">You can inject a base URL and/or a HTTP client into the API client.</span></span> <span data-ttu-id="e05c9-157">A melhor prática é sempre [reutilizar o HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span><span class="sxs-lookup"><span data-stu-id="e05c9-157">The best practice is to always [reuse the HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span></span>

<span data-ttu-id="e05c9-158">Agora você pode começar a implementar facilmente a API em projetos de cliente.</span><span class="sxs-lookup"><span data-stu-id="e05c9-158">You can now start implementing your API into client projects easily.</span></span>

### <a name="other-ways-to-generate-client-code"></a><span data-ttu-id="e05c9-159">Outras maneiras de gerar o código do cliente</span><span class="sxs-lookup"><span data-stu-id="e05c9-159">Other ways to generate client code</span></span>

<span data-ttu-id="e05c9-160">Você pode gerar o código de outras maneiras que sejam mais adequadas ao seu fluxo de trabalho:</span><span class="sxs-lookup"><span data-stu-id="e05c9-160">You can generate the code in other ways, more suited to your workflow:</span></span>

* [<span data-ttu-id="e05c9-161">MSBuild</span><span class="sxs-lookup"><span data-stu-id="e05c9-161">MSBuild</span></span>](https://www.nuget.org/packages/NSwag.MSBuild/)

* [<span data-ttu-id="e05c9-162">No código</span><span class="sxs-lookup"><span data-stu-id="e05c9-162">In code</span></span>](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [<span data-ttu-id="e05c9-163">Modelos T4</span><span class="sxs-lookup"><span data-stu-id="e05c9-163">T4 templates</span></span>](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customization"></a><span data-ttu-id="e05c9-164">Personalização</span><span class="sxs-lookup"><span data-stu-id="e05c9-164">Customization</span></span>

### <a name="xml-comments"></a><span data-ttu-id="e05c9-165">comentários XML</span><span class="sxs-lookup"><span data-stu-id="e05c9-165">XML comments</span></span>

<span data-ttu-id="e05c9-166">Os comentários XML podem ser habilitados com as seguintes abordagens:</span><span class="sxs-lookup"><span data-stu-id="e05c9-166">XML comments are enabled with the following approaches:</span></span>

#### <a name="visual-studiotabvisual-studio-xml"></a>[<span data-ttu-id="e05c9-167">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e05c9-167">Visual Studio</span></span>](#tab/visual-studio-xml/)
* <span data-ttu-id="e05c9-168">Clique com o botão direito do mouse no projeto no **Gerenciador de Soluções** e selecione **Propriedades**</span><span class="sxs-lookup"><span data-stu-id="e05c9-168">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="e05c9-169">Verifique a caixa **Arquivo de documentação XML** sob a seção **Saída** da guia **Build**:</span><span class="sxs-lookup"><span data-stu-id="e05c9-169">Check the **XML documentation file** box under the **Output** section of the **Build** tab:</span></span>

![Compile a guia das propriedades do projeto](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

#### <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[<span data-ttu-id="e05c9-171">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="e05c9-171">Visual Studio for Mac</span></span>](#tab/visual-studio-mac-xml/)
* <span data-ttu-id="e05c9-172">Abra a caixa de diálogo **Opções do Projeto** > **Compilar** > **Compilador**</span><span class="sxs-lookup"><span data-stu-id="e05c9-172">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="e05c9-173">Verifique a caixa **Gerar documentação XML** sob a seção **Opções Gerais**:</span><span class="sxs-lookup"><span data-stu-id="e05c9-173">Check the **Generate xml documentation** box under the **General Options** section:</span></span>

![Seção Opções Gerais das opções do projeto](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

#### <a name="visual-studio-codetabvisual-studio-code-xml"></a>[<span data-ttu-id="e05c9-175">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e05c9-175">Visual Studio Code</span></span>](#tab/visual-studio-code-xml/)
<span data-ttu-id="e05c9-176">Adicione manualmente o trecho a seguir ao arquivo *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="e05c9-176">Manually add the following snippet to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.NSwag/TodoApiNSwag.csproj?range=7-9)]

* * *
## <a name="data-annotations"></a><span data-ttu-id="e05c9-177">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="e05c9-177">Data annotations</span></span>

<span data-ttu-id="e05c9-178">O NSwag usa [Reflexão](/dotnet/csharp/programming-guide/concepts/reflection) e a melhor prática para as ações da API Web é retornar [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span><span class="sxs-lookup"><span data-stu-id="e05c9-178">NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the best practice for Web API actions is to return [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span></span> <span data-ttu-id="e05c9-179">Consequentemente, o NSwag não consegue inferir o que a sua ação está executando e o que ela retorna.</span><span class="sxs-lookup"><span data-stu-id="e05c9-179">Consequently, NSwag can't infer what your action is doing and what it returns.</span></span> <span data-ttu-id="e05c9-180">Considere o exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="e05c9-180">Consider the following example:</span></span>

```csharp
public IActionResult Create([FromBody] TodoItem item)
{
    if (item == null)
    {
        return BadRequest();
    }

    _context.TodoItems.Add(item);
    _context.SaveChanges();

     return CreatedAtRoute("GetTodo", new { id = item.Id }, item);
}
```

<span data-ttu-id="e05c9-181">A ação anterior retorna `IActionResult`, mas dentro da ação, ela está retornando [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) ou [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span><span class="sxs-lookup"><span data-stu-id="e05c9-181">The preceding action returns `IActionResult`, but inside the action it's returning either [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) or [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span></span> <span data-ttu-id="e05c9-182">As anotações de dados são usadas para informar os clientes sobre qual resposta HTTP essa ação está retornando.</span><span class="sxs-lookup"><span data-stu-id="e05c9-182">Data annotations are used to tell clients which HTTP response this action is returning.</span></span> <span data-ttu-id="e05c9-183">Decore a ação com os seguintes atributos:</span><span class="sxs-lookup"><span data-stu-id="e05c9-183">Decorate the action with the following attributes:</span></span>

```csharp
[HttpPost]
[ProducesResponseType(typeof(TodoItem), 201)] // Created
[ProducesResponseType(typeof(TodoItem), 400)] // BadRequest
public IActionResult Create([FromBody] TodoItem item)
```

<span data-ttu-id="e05c9-184">O gerador do Swagger agora pode descrever essa ação precisamente e os clientes gerados conseguem saber o que recebem ao chamar o ponto de extremidade.</span><span class="sxs-lookup"><span data-stu-id="e05c9-184">The Swagger generator can now accurately describe this action, and generated clients know what they receive when calling the endpoint.</span></span> <span data-ttu-id="e05c9-185">É altamente recomendável decorar todas as ações com esses atributos.</span><span class="sxs-lookup"><span data-stu-id="e05c9-185">Decorating all actions with these attributes is highly recommended.</span></span> <span data-ttu-id="e05c9-186">Para obter diretrizes sobre quais respostas HTTP as ações da API devem retornar, confira a [Especificação RFC 7231](https://tools.ietf.org/html/rfc7231#section-4.3).</span><span class="sxs-lookup"><span data-stu-id="e05c9-186">For guidelines on what HTTP responses your API actions should return, see the [RFC 7231 specification](https://tools.ietf.org/html/rfc7231#section-4.3).</span></span>
