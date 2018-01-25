---
title: "Provedores de arquivo no núcleo do ASP.NET"
author: ardalis
description: Saiba como o ASP.NET Core abstrai o acesso de sistema de arquivos com o uso de provedores de arquivo.
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/file-providers
ms.openlocfilehash: 10f3276d3e71e8a29b452d4c62865cbb82298513
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
# <a name="file-providers-in-aspnet-core"></a><span data-ttu-id="5e3ca-103">Provedores de arquivo no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="5e3ca-103">File Providers in ASP.NET Core</span></span>

<span data-ttu-id="5e3ca-104">Por [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="5e3ca-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="5e3ca-105">ASP.NET Core abstrai o acesso de sistema de arquivos com o uso de provedores de arquivo.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-105">ASP.NET Core abstracts file system access through the use of File Providers.</span></span>

<span data-ttu-id="5e3ca-106">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5e3ca-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="file-provider-abstractions"></a><span data-ttu-id="5e3ca-107">Abstrações de provedor de arquivo</span><span class="sxs-lookup"><span data-stu-id="5e3ca-107">File Provider abstractions</span></span>

<span data-ttu-id="5e3ca-108">Provedores de arquivo são uma abstração sobre os sistemas de arquivo.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-108">File Providers are an abstraction over file systems.</span></span> <span data-ttu-id="5e3ca-109">É a principal interface `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-109">The main interface is `IFileProvider`.</span></span> <span data-ttu-id="5e3ca-110">`IFileProvider`expõe métodos para obter informações sobre o arquivo (`IFileInfo`), informações de diretório (`IDirectoryContents`) e para configurar as notificações de alteração (usando um `IChangeToken`).</span><span class="sxs-lookup"><span data-stu-id="5e3ca-110">`IFileProvider` exposes methods to get file information (`IFileInfo`), directory information (`IDirectoryContents`), and to set up change notifications (using an `IChangeToken`).</span></span>

<span data-ttu-id="5e3ca-111">`IFileInfo`fornece métodos e propriedades sobre arquivos individuais ou diretórios.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-111">`IFileInfo` provides methods and properties about individual files or directories.</span></span> <span data-ttu-id="5e3ca-112">Ele tem duas propriedades booleanas, `Exists` e `IsDirectory`, bem como as propriedades que descrevem o arquivo `Name`, `Length` (em bytes), e `LastModified` Data.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-112">It has two boolean properties, `Exists` and `IsDirectory`, as well as properties describing the file's `Name`, `Length` (in bytes), and `LastModified` date.</span></span> <span data-ttu-id="5e3ca-113">É possível ler o arquivo usando seu `CreateReadStream` método.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-113">You can read from the file using its `CreateReadStream` method.</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="5e3ca-114">Implementações do provedor de arquivo</span><span class="sxs-lookup"><span data-stu-id="5e3ca-114">File Provider implementations</span></span>

<span data-ttu-id="5e3ca-115">Três implementações de `IFileProvider` estão disponíveis: física, inserida e composto.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-115">Three implementations of `IFileProvider` are available: Physical, Embedded, and Composite.</span></span> <span data-ttu-id="5e3ca-116">O provedor físico é usado para acessar os arquivos do sistema atual.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-116">The physical provider is used to access the actual system's files.</span></span> <span data-ttu-id="5e3ca-117">O provedor inserido é usado para acessar arquivos inseridos em assemblies.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-117">The embedded provider is used to access files embedded in assemblies.</span></span> <span data-ttu-id="5e3ca-118">O provedor composto é usado para fornecer acesso combinado a arquivos e diretórios de um ou mais provedores.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-118">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span>

### <a name="physicalfileprovider"></a><span data-ttu-id="5e3ca-119">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="5e3ca-119">PhysicalFileProvider</span></span>

<span data-ttu-id="5e3ca-120">O `PhysicalFileProvider` fornece acesso ao sistema de arquivos físico.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-120">The `PhysicalFileProvider` provides access to the physical file system.</span></span> <span data-ttu-id="5e3ca-121">Ele encapsula o `System.IO.File` tipo (para o provedor físico), todos os caminhos para um diretório e seus filhos de escopo.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-121">It wraps the `System.IO.File` type (for the physical provider), scoping all paths to a directory and its children.</span></span> <span data-ttu-id="5e3ca-122">Esse escopo limita o acesso a um determinado diretório e seus filhos, impedindo o acesso ao sistema de arquivos fora esse limite.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-122">This scoping limits access to a certain directory and its children, preventing access to the file system outside of this boundary.</span></span> <span data-ttu-id="5e3ca-123">Ao instanciar esse provedor, você deve fornecê-lo com um caminho de diretório, que serve como o caminho base para todas as solicitações feitas a esse provedor (e que restringe o acesso de fora deste caminho).</span><span class="sxs-lookup"><span data-stu-id="5e3ca-123">When instantiating this provider, you must provide it with a directory path, which serves as the base path for all requests made to this provider (and which restricts access outside of this path).</span></span> <span data-ttu-id="5e3ca-124">Em um aplicativo do ASP.NET Core, você pode instanciar uma `PhysicalFileProvider` provedor diretamente, ou você pode solicitar uma `IFileProvider` em um controlador ou o construtor do serviço por meio de [injeção de dependência](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="5e3ca-124">In an ASP.NET Core app, you can instantiate a `PhysicalFileProvider` provider directly, or you can request an `IFileProvider` in a Controller or service's constructor through [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="5e3ca-125">A segunda abordagem normalmente produzirá uma solução mais flexível e pode ser testada.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-125">The latter approach will typically yield a more flexible and testable solution.</span></span>

<span data-ttu-id="5e3ca-126">O exemplo a seguir mostra como criar um `PhysicalFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-126">The sample below shows how to create a `PhysicalFileProvider`.</span></span>


```csharp
IFileProvider provider = new PhysicalFileProvider(applicationRoot);
IDirectoryContents contents = provider.GetDirectoryContents(""); // the applicationRoot contents
IFileInfo fileInfo = provider.GetFileInfo("wwwroot/js/site.js"); // a file under applicationRoot
```

<span data-ttu-id="5e3ca-127">Você pode iterar por meio de seu conteúdo do diretório ou obter informações do arquivo específico, fornecendo um subcaminho.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-127">You can iterate through its directory contents or get a specific file's information by providing a subpath.</span></span>

<span data-ttu-id="5e3ca-128">Para solicitar um provedor de um controlador, especificá-lo no construtor do controlador e atribuí-la a um campo local.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-128">To request a provider from a controller, specify it in the controller's constructor and assign it to a local field.</span></span> <span data-ttu-id="5e3ca-129">Use a instância local de métodos de ação:</span><span class="sxs-lookup"><span data-stu-id="5e3ca-129">Use the local instance from your action methods:</span></span>

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Controllers/HomeController.cs?highlight=5,7,12&range=6-19)]

<span data-ttu-id="5e3ca-130">Em seguida, criar o provedor no aplicativo do `Startup` classe:</span><span class="sxs-lookup"><span data-stu-id="5e3ca-130">Then, create the provider in the app's `Startup` class:</span></span>

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=35,40&range=1-43)]

<span data-ttu-id="5e3ca-131">No *cshtml* exibir, percorrer o `IDirectoryContents` fornecido:</span><span class="sxs-lookup"><span data-stu-id="5e3ca-131">In the *Index.cshtml* view, iterate through the `IDirectoryContents` provided:</span></span>

[!code-html[Main](file-providers/sample/src/FileProviderSample/Views/Home/Index.cshtml?highlight=2,7,9,11,15)]

<span data-ttu-id="5e3ca-132">O resultado:</span><span class="sxs-lookup"><span data-stu-id="5e3ca-132">The result:</span></span>

![Pastas e arquivos físicos do provedor exemplo aplicativo listagem de arquivos](file-providers/_static/physical-directory-listing.png)

### <a name="embeddedfileprovider"></a><span data-ttu-id="5e3ca-134">EmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="5e3ca-134">EmbeddedFileProvider</span></span>

<span data-ttu-id="5e3ca-135">O `EmbeddedFileProvider` é usada para acessar arquivos incorporados em assemblies.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-135">The `EmbeddedFileProvider` is used to access files embedded in assemblies.</span></span> <span data-ttu-id="5e3ca-136">No núcleo do .NET, você deve inserir arquivos em um assembly com o `<EmbeddedResource>` elemento o *. csproj* arquivo:</span><span class="sxs-lookup"><span data-stu-id="5e3ca-136">In .NET Core, you embed files in an assembly with the `<EmbeddedResource>` element in the *.csproj* file:</span></span>

[!code-json[Main](file-providers/sample/src/FileProviderSample/FileProviderSample.csproj?range=13-18)]

<span data-ttu-id="5e3ca-137">Você pode usar [padrões de globalização](#globbing-patterns) ao especificar arquivos para inserir no assembly.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-137">You can use [globbing patterns](#globbing-patterns) when specifying files to embed in the assembly.</span></span> <span data-ttu-id="5e3ca-138">Esses padrões podem ser usados para corresponder a um ou mais arquivos.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-138">These patterns can be used to match one or more files.</span></span>

> [!NOTE]
> <span data-ttu-id="5e3ca-139">É improvável que você nunca deseja inserir, na verdade, todos os arquivos. js em seu projeto em seu assembly; o exemplo acima é apenas a fins de demonstração.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-139">It's unlikely you would ever want to actually embed every .js file in your project in its assembly; the above sample is for demo purposes only.</span></span>

<span data-ttu-id="5e3ca-140">Ao criar um `EmbeddedFileProvider`, passar o assembly será exibido para o construtor.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-140">When creating an `EmbeddedFileProvider`, pass the assembly it will read to its constructor.</span></span>

```csharp
var embeddedProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="5e3ca-141">O trecho de código acima demonstra como criar um `EmbeddedFileProvider` com acesso ao assembly em execução no momento.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-141">The snippet above demonstrates how to create an `EmbeddedFileProvider` with access to the currently executing assembly.</span></span>

<span data-ttu-id="5e3ca-142">Atualizando o aplicativo de exemplo para usar um `EmbeddedFileProvider` resulta na seguinte saída:</span><span class="sxs-lookup"><span data-stu-id="5e3ca-142">Updating the sample app to use an `EmbeddedFileProvider` results in the following output:</span></span>

![Aplicativo de exemplo de provedor de arquivo listando arquivos incorporados](file-providers/_static/embedded-directory-listing.png)

> [!NOTE]
> <span data-ttu-id="5e3ca-144">Recursos incorporados não expõem diretórios.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-144">Embedded resources don't expose directories.</span></span> <span data-ttu-id="5e3ca-145">Em vez disso, o caminho para o recurso (por meio de seu namespace) é inserido em seu nome de arquivo usando `.` separadores.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-145">Rather, the path to the resource (via its namespace) is embedded in its filename using `.` separators.</span></span>

> [!TIP]
> <span data-ttu-id="5e3ca-146">O `EmbeddedFileProvider` construtor aceita um opcional `baseNamespace` parâmetro.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-146">The `EmbeddedFileProvider` constructor accepts an optional `baseNamespace` parameter.</span></span> <span data-ttu-id="5e3ca-147">Especificar isso será escopo chamadas para `GetDirectoryContents` a esses recursos sob o namespace fornecido.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-147">Specifying this will scope calls to `GetDirectoryContents` to those resources under the provided namespace.</span></span>

### <a name="compositefileprovider"></a><span data-ttu-id="5e3ca-148">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="5e3ca-148">CompositeFileProvider</span></span>

<span data-ttu-id="5e3ca-149">O `CompositeFileProvider` combina `IFileProvider` instâncias, expor uma interface única para trabalhar com arquivos de vários provedores.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-149">The `CompositeFileProvider` combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="5e3ca-150">Ao criar o `CompositeFileProvider`, passar um ou mais `IFileProvider` instâncias para o construtor:</span><span class="sxs-lookup"><span data-stu-id="5e3ca-150">When creating the `CompositeFileProvider`, you pass one or more `IFileProvider` instances to its constructor:</span></span>

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=3&range=35-37)]

<span data-ttu-id="5e3ca-151">Atualizando o aplicativo de exemplo para usar um `CompositeFileProvider` que inclui ambos os físicos e incorporados provedores configurados anteriormente, o que resulta na seguinte saída:</span><span class="sxs-lookup"><span data-stu-id="5e3ca-151">Updating the sample app to use a `CompositeFileProvider` that includes both the physical and embedded providers configured previously, results in the following output:</span></span>

![Aplicativo de exemplo de provedor de arquivo listando arquivos físicos e pastas e arquivos incorporados](file-providers/_static/composite-directory-listing.png)

## <a name="watching-for-changes"></a><span data-ttu-id="5e3ca-153">Monitorar alterações</span><span class="sxs-lookup"><span data-stu-id="5e3ca-153">Watching for changes</span></span>

<span data-ttu-id="5e3ca-154">O `IFileProvider` `Watch` método fornece uma maneira de observar um ou mais arquivos ou pastas para que as alterações.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-154">The `IFileProvider` `Watch` method provides a way to watch one or more files or directories for changes.</span></span> <span data-ttu-id="5e3ca-155">Esse método aceita uma cadeia de caracteres de caminho, o que pode usar [padrões de globalização](#globbing-patterns) para especificar vários arquivos e retorna um `IChangeToken`.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-155">This method accepts a path string, which can use [globbing patterns](#globbing-patterns) to specify multiple files, and returns an `IChangeToken`.</span></span> <span data-ttu-id="5e3ca-156">Esse token expõe um `HasChanged` propriedade que pode ser inspecionada, e um `RegisterChangeCallback` método é chamado quando forem detectadas alterações para a cadeia de caracteres do caminho especificado.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-156">This token exposes a `HasChanged` property that can be inspected, and a `RegisterChangeCallback` method that's called when changes are detected to the specified path string.</span></span> <span data-ttu-id="5e3ca-157">Observe que cada token de alteração apenas chama o retorno de chamada associado em resposta a uma única alteração.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-157">Note that each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="5e3ca-158">Para habilitar o monitoramento constante, você pode usar um `TaskCompletionSource` conforme mostrado abaixo, ou recrie `IChangeToken` instâncias em resposta a alterações.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-158">To enable constant monitoring, you can use a `TaskCompletionSource` as shown below, or re-create `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="5e3ca-159">No exemplo deste artigo, um aplicativo de console é configurado para exibir uma mensagem sempre que um arquivo de texto é modificado:</span><span class="sxs-lookup"><span data-stu-id="5e3ca-159">In this article's sample, a console application is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[Main](file-providers/sample/src/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="5e3ca-160">O resultado, após salvar o arquivo várias vezes:</span><span class="sxs-lookup"><span data-stu-id="5e3ca-160">The result, after saving the file several times:</span></span>

![Janela de comando após a execução dotnet executar mostra o arquivo quotes.txt para alterações de monitoramento de aplicativos e que o arquivo foi alterado cinco vezes.](file-providers/_static/watch-console.png)

> [!NOTE]
> <span data-ttu-id="5e3ca-162">Alguns sistemas de arquivos, como contêineres do Docker e compartilhamentos de rede, podem não enviar notificações de alteração de forma confiável.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-162">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="5e3ca-163">Definir o `DOTNET_USE_POLLINGFILEWATCHER` variável de ambiente `1` ou `true` para sondar o sistema de arquivos para que as alterações a cada quatro segundos.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-163">Set the `DOTNET_USE_POLLINGFILEWATCHER` environment variable to `1` or `true` to poll the file system for changes every 4 seconds.</span></span>

## <a name="globbing-patterns"></a><span data-ttu-id="5e3ca-164">Padrões de globalização</span><span class="sxs-lookup"><span data-stu-id="5e3ca-164">Globbing patterns</span></span>

<span data-ttu-id="5e3ca-165">Caminhos de sistema de arquivos usam padrões de curinga chamados *padrões de globalização*.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-165">File system paths use wildcard patterns called *globbing patterns*.</span></span> <span data-ttu-id="5e3ca-166">Esses padrões simples podem ser usados para especificar grupos de arquivos.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-166">These simple patterns can be used to specify groups of files.</span></span> <span data-ttu-id="5e3ca-167">Os dois caracteres curinga são `*` e `**`.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-167">The two wildcard characters are `*` and `**`.</span></span>

**`*`**

   <span data-ttu-id="5e3ca-168">Corresponde qualquer coisa no nível da pasta atual, ou qualquer nome de arquivo ou qualquer extensão de arquivo.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-168">Matches anything at the current folder level, or any filename, or any file extension.</span></span> <span data-ttu-id="5e3ca-169">Correspondências são finalizadas pelo `/` e `.` caracteres no caminho do arquivo.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-169">Matches are terminated by `/` and `.` characters in the file path.</span></span>

<strong><code>**</code></strong>

   <span data-ttu-id="5e3ca-170">Coincide com qualquer coisa entre vários níveis de diretório.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-170">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="5e3ca-171">Pode ser usada para recursivamente corresponder muitos arquivos dentro de uma hierarquia de diretórios.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-171">Can be used to recursively match many files within a directory hierarchy.</span></span>

### <a name="globbing-pattern-examples"></a><span data-ttu-id="5e3ca-172">Exemplos de padrão de globalização</span><span class="sxs-lookup"><span data-stu-id="5e3ca-172">Globbing pattern examples</span></span>

**`directory/file.txt`**

   <span data-ttu-id="5e3ca-173">Corresponde a um arquivo específico em um diretório específico.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-173">Matches a specific file in a specific directory.</span></span>

**<code>directory/*.txt</code>**

   <span data-ttu-id="5e3ca-174">Corresponde a todos os arquivos com `.txt` extensão em um diretório específico.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-174">Matches all files with `.txt` extension in a specific directory.</span></span>

**`directory/*/bower.json`**

   <span data-ttu-id="5e3ca-175">Corresponde a todos os `bower.json` arquivos em diretórios exatamente um nível abaixo de `directory` directory.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-175">Matches all `bower.json` files in directories exactly one level below the `directory` directory.</span></span>

**<code>directory/&#42;&#42;/&#42;.txt</code>**

   <span data-ttu-id="5e3ca-176">Corresponde a todos os arquivos com `.txt` extensão encontrada em qualquer lugar no `directory` directory.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-176">Matches all files with `.txt` extension found anywhere under the `directory` directory.</span></span>

## <a name="file-provider-usage-in-aspnet-core"></a><span data-ttu-id="5e3ca-177">Uso do provedor de arquivo no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="5e3ca-177">File Provider usage in ASP.NET Core</span></span>

<span data-ttu-id="5e3ca-178">Várias partes do ASP.NET Core utilizam provedores de arquivo.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-178">Several parts of ASP.NET Core utilize file providers.</span></span> <span data-ttu-id="5e3ca-179">`IHostingEnvironment`expõe o aplicativo raiz do conteúdo e a raiz da web como `IFileProvider` tipos.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-179">`IHostingEnvironment` exposes the app's content root and web root as `IFileProvider` types.</span></span> <span data-ttu-id="5e3ca-180">O middleware de arquivos estáticos usa provedores de arquivo para localizar arquivos estáticos.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-180">The static files middleware uses file providers to locate static files.</span></span> <span data-ttu-id="5e3ca-181">Razor faz uso intenso de `IFileProvider` localizar os modos de exibição.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-181">Razor makes heavy use of `IFileProvider` in locating views.</span></span> <span data-ttu-id="5e3ca-182">Dotnet publica provedores de arquivo usa a funcionalidade e padrões de globalização para especificar quais arquivos devem ser publicados.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-182">Dotnet's publish functionality uses file providers and globbing patterns to specify which files should be published.</span></span>

## <a name="recommendations-for-use-in-apps"></a><span data-ttu-id="5e3ca-183">Recomendações para uso em aplicativos</span><span class="sxs-lookup"><span data-stu-id="5e3ca-183">Recommendations for use in apps</span></span>

<span data-ttu-id="5e3ca-184">Se seu aplicativo ASP.NET Core requer acesso de sistema de arquivos, você pode solicitar uma instância de `IFileProvider` injeção de dependência e, em seguida, usar seus métodos para executar o acesso, conforme mostrado neste exemplo.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-184">If your ASP.NET Core app requires file system access, you can request an instance of `IFileProvider` through dependency injection, and then use its methods to perform the access, as shown in this sample.</span></span> <span data-ttu-id="5e3ca-185">Isso permite que você configure o provedor de uma vez, quando o aplicativo é iniciado e reduz o número de tipos de implementação que cria uma instância do seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5e3ca-185">This allows you to configure the provider once, when the app starts up, and reduces the number of implementation types your app instantiates.</span></span>
