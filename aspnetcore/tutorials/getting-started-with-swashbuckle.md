---
title: Introdução ao Swashbuckle e ao ASP.NET Core
author: zuckerthoben
description: Saiba como adicionar o Swashbuckle ao seu projeto do ASP.NET Core para integrar a interface do usuário do Swagger.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 03/26/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/get-started-with-swashbuckle
ms.openlocfilehash: e90339f2884dd9b20cf135f879c9cab6110efecf
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-swashbuckle-and-aspnet-core"></a><span data-ttu-id="41284-103">Introdução ao Swashbuckle e ao ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="41284-103">Get started with Swashbuckle and ASP.NET Core</span></span>

<span data-ttu-id="41284-104">Por [Shayne Boyer](https://twitter.com/spboyer) e [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="41284-104">By [Shayne Boyer](https://twitter.com/spboyer) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="41284-105">Há três componentes principais bo Swashbuckle:</span><span class="sxs-lookup"><span data-stu-id="41284-105">There are three main components to Swashbuckle:</span></span>

* <span data-ttu-id="41284-106">[Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): um modelo de objeto e um middleware do Swagger para expor objetos `SwaggerDocument` como pontos de extremidade JSON.</span><span class="sxs-lookup"><span data-stu-id="41284-106">[Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): a Swagger object model and middleware to expose `SwaggerDocument` objects as JSON endpoints.</span></span>

* <span data-ttu-id="41284-107">[Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): um gerador do Swagger cria objetos `SwaggerDocument` diretamente de modelos, controladores e rotas.</span><span class="sxs-lookup"><span data-stu-id="41284-107">[Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): a Swagger generator that builds `SwaggerDocument` objects directly from your routes, controllers, and models.</span></span> <span data-ttu-id="41284-108">Normalmente, ele é combinado com o middleware de ponto de extremidade do Swagger para expor automaticamente o JSON do Swagger.</span><span class="sxs-lookup"><span data-stu-id="41284-108">It's typically combined with the Swagger endpoint middleware to automatically expose Swagger JSON.</span></span>

* <span data-ttu-id="41284-109">[Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): uma versão incorporada da ferramenta de interface do usuário do Swagger.</span><span class="sxs-lookup"><span data-stu-id="41284-109">[Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): an embedded version of the Swagger UI tool.</span></span> <span data-ttu-id="41284-110">Ele interpreta o JSON do Swagger para criar uma experiência rica e personalizável para descrever a funcionalidade da API da Web.</span><span class="sxs-lookup"><span data-stu-id="41284-110">It interprets Swagger JSON to build a rich, customizable experience for describing the Web API functionality.</span></span> <span data-ttu-id="41284-111">Ela inclui o agente de teste interno para os métodos públicos.</span><span class="sxs-lookup"><span data-stu-id="41284-111">It includes built-in test harnesses for the public methods.</span></span>

## <a name="package-installation"></a><span data-ttu-id="41284-112">Instalação do pacote</span><span class="sxs-lookup"><span data-stu-id="41284-112">Package installation</span></span>

<span data-ttu-id="41284-113">O Swashbuckle pode ser adicionado com as seguintes abordagens:</span><span class="sxs-lookup"><span data-stu-id="41284-113">Swashbuckle can be added with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="41284-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="41284-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="41284-115">Da janela **Console do Gerenciador de Pacotes**:</span><span class="sxs-lookup"><span data-stu-id="41284-115">From the **Package Manager Console** window:</span></span>

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* <span data-ttu-id="41284-116">Da caixa de diálogo **Gerenciar Pacotes NuGet**:</span><span class="sxs-lookup"><span data-stu-id="41284-116">From the **Manage NuGet Packages** dialog:</span></span>

  * <span data-ttu-id="41284-117">Clique com o botão direito do mouse no projeto em **Gerenciador de Soluções** > **Gerenciar Pacotes NuGet**</span><span class="sxs-lookup"><span data-stu-id="41284-117">Right-click your project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="41284-118">Defina a **Origem do pacote** para "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="41284-118">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="41284-119">Insira "Swashbuckle.AspNetCore" na caixa de pesquisa</span><span class="sxs-lookup"><span data-stu-id="41284-119">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="41284-120">Selecione o pacote "Swashbuckle.AspNetCore" na guia **Procurar** e clique em **Instalar**</span><span class="sxs-lookup"><span data-stu-id="41284-120">Select the "Swashbuckle.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="41284-121">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="41284-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="41284-122">Clique com o botão direito do mouse na pasta *Pacotes* em **Painel de Soluções** > **Adicionar Pacotes...**</span><span class="sxs-lookup"><span data-stu-id="41284-122">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="41284-123">Defina a lista suspensa **Origem** da janela **Adicionar Pacotes** para "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="41284-123">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="41284-124">Insira Swashbuckle.AspNetCore na caixa de pesquisa</span><span class="sxs-lookup"><span data-stu-id="41284-124">Enter Swashbuckle.AspNetCore in the search box</span></span>
* <span data-ttu-id="41284-125">Selecione o pacote "Swashbuckle.AspNetCore" no painel de resultados e clique em **Adicionar Pacote**</span><span class="sxs-lookup"><span data-stu-id="41284-125">Select the Swashbuckle.AspNetCore package from the results pane and click **Add Package**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="41284-126">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="41284-126">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="41284-127">Execute o comando a seguir do **Terminal Integrado**:</span><span class="sxs-lookup"><span data-stu-id="41284-127">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="41284-128">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="41284-128">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="41284-129">Execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="41284-129">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="41284-130">Adicionar e configurar o middleware do Swagger</span><span class="sxs-lookup"><span data-stu-id="41284-130">Add and configure Swagger middleware</span></span>

<span data-ttu-id="41284-131">Adicione o gerador do Swagger à coleção de serviços no método `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="41284-131">Add the Swagger generator to the services collection in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=7-10)]

<span data-ttu-id="41284-132">Importe o namespace a seguir para usar a classe `Info`:</span><span class="sxs-lookup"><span data-stu-id="41284-132">Import the following namespace to use the `Info` class:</span></span>

```csharp
using Swashbuckle.AspNetCore.Swagger;
```

<span data-ttu-id="41284-133">No método `Startup.Configure`, habilite o middleware para atender ao documento JSON gerado e à interface do usuário do Swagger:</span><span class="sxs-lookup"><span data-stu-id="41284-133">In the `Startup.Configure` method, enable the middleware for serving the generated JSON document and the Swagger UI:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Startup2.cs?name=snippet_Configure&highlight=4,7-10)]

<span data-ttu-id="41284-134">Inicie o aplicativo e navegue até `http://localhost:<random_port>/swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="41284-134">Launch the app, and navigate to `http://localhost:<random_port>/swagger/v1/swagger.json`.</span></span> <span data-ttu-id="41284-135">O documento gerado que descreve os pontos de extremidade é exibido conforme é mostrado na [Especificação do Swagger (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).</span><span class="sxs-lookup"><span data-stu-id="41284-135">The generated document describing the endpoints appears as shown in [Swagger specification (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).</span></span>

<span data-ttu-id="41284-136">A interface do usuário do Swagger pode ser encontrada em `http://localhost:<random_port>/swagger`.</span><span class="sxs-lookup"><span data-stu-id="41284-136">The Swagger UI can be found at `http://localhost:<random_port>/swagger`.</span></span> <span data-ttu-id="41284-137">Explore a API por meio da interface do usuário do Swagger e incorpore-a em outros programas.</span><span class="sxs-lookup"><span data-stu-id="41284-137">Explore the API via Swagger UI and incorporate it in other programs.</span></span>

> [!TIP]
> <span data-ttu-id="41284-138">Para atender à interface do usuário do Swagger na raiz do aplicativo (`http://localhost:<random_port>/`), defina a propriedade `RoutePrefix` como uma cadeia de caracteres vazia:</span><span class="sxs-lookup"><span data-stu-id="41284-138">To serve the Swagger UI at the app's root (`http://localhost:<random_port>/`), set the `RoutePrefix` property to an empty string:</span></span>
> 
> [!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Startup3.cs?name=snippet_UseSwaggerUI&highlight=4)]

## <a name="customize--extend"></a><span data-ttu-id="41284-139">Personalizar e estender</span><span class="sxs-lookup"><span data-stu-id="41284-139">Customize & extend</span></span>

<span data-ttu-id="41284-140">O Swagger fornece opções para documentar o modelo de objeto e personalizar a interface do usuário para corresponder ao seu tema.</span><span class="sxs-lookup"><span data-stu-id="41284-140">Swagger provides options for documenting the object model and customizing the UI to match your theme.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="41284-141">Descrição e informações da API</span><span class="sxs-lookup"><span data-stu-id="41284-141">API info and description</span></span>

<span data-ttu-id="41284-142">A ação de configuração passada para o método `AddSwaggerGen` adiciona informações como o autor, a licença e a descrição:</span><span class="sxs-lookup"><span data-stu-id="41284-142">The configuration action passed to the `AddSwaggerGen` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Startup.cs?range=21-40,46)]

<span data-ttu-id="41284-143">A interface do usuário do Swagger exibe as informações da versão:</span><span class="sxs-lookup"><span data-stu-id="41284-143">The Swagger UI displays the version's information:</span></span>

![A interface do usuário do Swagger com informações de versão: descrição, autor e link veja mais](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a><span data-ttu-id="41284-145">comentários XML</span><span class="sxs-lookup"><span data-stu-id="41284-145">XML comments</span></span>

<span data-ttu-id="41284-146">Comentários XML podem ser habilitados com as seguintes abordagens:</span><span class="sxs-lookup"><span data-stu-id="41284-146">XML comments can be enabled with the following approaches:</span></span>

#### <a name="visual-studiotabvisual-studio-xml"></a>[<span data-ttu-id="41284-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="41284-147">Visual Studio</span></span>](#tab/visual-studio-xml/)
* <span data-ttu-id="41284-148">Clique com o botão direito do mouse no projeto no **Gerenciador de Soluções** e selecione **Propriedades**</span><span class="sxs-lookup"><span data-stu-id="41284-148">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="41284-149">Verifique a caixa **Arquivo de documentação XML** sob a seção **Saída** da guia **Build**:</span><span class="sxs-lookup"><span data-stu-id="41284-149">Check the **XML documentation file** box under the **Output** section of the **Build** tab:</span></span>

![Compile a guia das propriedades do projeto](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

#### <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[<span data-ttu-id="41284-151">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="41284-151">Visual Studio for Mac</span></span>](#tab/visual-studio-mac-xml/)
* <span data-ttu-id="41284-152">Abra a caixa de diálogo **Opções do Projeto** > **Compilar** > **Compilador**</span><span class="sxs-lookup"><span data-stu-id="41284-152">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="41284-153">Verifique a caixa **Gerar documentação XML** sob a seção **Opções Gerais**:</span><span class="sxs-lookup"><span data-stu-id="41284-153">Check the **Generate xml documentation** box under the **General Options** section:</span></span>

![Seção Opções Gerais das opções do projeto](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

#### <a name="visual-studio-codetabvisual-studio-code-xml"></a>[<span data-ttu-id="41284-155">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="41284-155">Visual Studio Code</span></span>](#tab/visual-studio-code-xml/)
<span data-ttu-id="41284-156">Adicione manualmente o trecho a seguir ao arquivo *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="41284-156">Manually add the following snippet to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=2)]

* * *
<span data-ttu-id="41284-157">A habilitação de comentários XML fornece informações de depuração para os membros e os tipos públicos não documentados.</span><span class="sxs-lookup"><span data-stu-id="41284-157">Enabling XML comments provides debug information for undocumented public types and members.</span></span> <span data-ttu-id="41284-158">Os membros e tipos não documentados são indicados por mensagem de aviso.</span><span class="sxs-lookup"><span data-stu-id="41284-158">Undocumented types and members are indicated by the warning message.</span></span> <span data-ttu-id="41284-159">Por exemplo, a seguinte mensagem indica uma violação do código de aviso 1591:</span><span class="sxs-lookup"><span data-stu-id="41284-159">For example, the following message indicates a violation of warning code 1591:</span></span>

```text
warning CS1591: Missing XML comment for publicly visible type or member 'TodoController.GetAll()'
```

<span data-ttu-id="41284-160">Suprima os avisos definindo uma lista separada por ponto-e-vírgula dos códigos de aviso a serem ignorados no arquivo *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="41284-160">Suppress warnings by defining a semicolon-delimited list of warning codes to ignore in the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=3)]

<span data-ttu-id="41284-161">Configure o Swagger para usar o arquivo XML gerado.</span><span class="sxs-lookup"><span data-stu-id="41284-161">Configure Swagger to use the generated XML file.</span></span> <span data-ttu-id="41284-162">Para sistemas operacionais Linux ou que não sejam Windows, os caminhos e nomes de arquivo podem diferenciar maiúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="41284-162">For Linux or non-Windows operating systems, file names and paths can be case-sensitive.</span></span> <span data-ttu-id="41284-163">Por exemplo, um arquivo *TodoApi.XML* é válido no Windows, mas não no CentOS.</span><span class="sxs-lookup"><span data-stu-id="41284-163">For example, a *TodoApi.XML* file is valid on Windows but not CentOS.</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=29-31)]

<span data-ttu-id="41284-164">No código anterior, a [Reflexão](/dotnet/csharp/programming-guide/concepts/reflection) é usada para criar um nome de arquivo XML correspondente ao do projeto de API da Web.</span><span class="sxs-lookup"><span data-stu-id="41284-164">In the preceding code, [Reflection](/dotnet/csharp/programming-guide/concepts/reflection) is used to build an XML file name matching that of the Web API project.</span></span> <span data-ttu-id="41284-165">Essa abordagem garante que o nome do arquivo XML gerado corresponda ao nome do projeto.</span><span class="sxs-lookup"><span data-stu-id="41284-165">This approach ensures that the generated XML file name matches the project name.</span></span> <span data-ttu-id="41284-166">A propriedade [AppContext.BaseDirectory](/dotnet/api/system.appcontext.basedirectory#System_AppContext_BaseDirectory) é usada para construir um caminho para o arquivo XML.</span><span class="sxs-lookup"><span data-stu-id="41284-166">The [AppContext.BaseDirectory](/dotnet/api/system.appcontext.basedirectory#System_AppContext_BaseDirectory) property is used to construct a path to the XML file.</span></span>

<span data-ttu-id="41284-167">Adicionar comentários de barra tripla a uma ação aprimora a interface do usuário do Swagger adicionando a descrição ao cabeçalho da seção.</span><span class="sxs-lookup"><span data-stu-id="41284-167">Adding triple-slash comments to an action enhances the Swagger UI by adding the description to the section header.</span></span> <span data-ttu-id="41284-168">Adicione um elemento [\<summary>](/dotnet/csharp/programming-guide/xmldoc/summary) acima da ação `Delete`:</span><span class="sxs-lookup"><span data-stu-id="41284-168">Add a [\<summary>](/dotnet/csharp/programming-guide/xmldoc/summary) element above the `Delete` action:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Delete&highlight=1-3)]

<span data-ttu-id="41284-169">A interface do usuário do Swagger exibe o texto interno do elemento `<summary>` do código anterior:</span><span class="sxs-lookup"><span data-stu-id="41284-169">The Swagger UI displays the inner text of the preceding code's `<summary>` element:</span></span>

![A interface do usuário do Swagger, mostrando o comentário XML 'Exclui um TodoItem específico'.](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

<span data-ttu-id="41284-172">A interface do usuário é controlada pelo esquema JSON gerado:</span><span class="sxs-lookup"><span data-stu-id="41284-172">The UI is driven by the generated JSON schema:</span></span>

```json
"delete": {
    "tags": [
        "Todo"
    ],
    "summary": "Deletes a specific TodoItem.",
    "operationId": "ApiTodoByIdDelete",
    "consumes": [],
    "produces": [],
    "parameters": [
        {
            "name": "id",
            "in": "path",
            "description": "",
            "required": true,
            "type": "integer",
            "format": "int64"
        }
    ],
    "responses": {
        "200": {
            "description": "Success"
        }
    }
}
```

<span data-ttu-id="41284-173">Adicione um elemento [\<remarks>](/dotnet/csharp/programming-guide/xmldoc/remarks) na documentação do método da ação `Create`.</span><span class="sxs-lookup"><span data-stu-id="41284-173">Add a [\<remarks>](/dotnet/csharp/programming-guide/xmldoc/remarks) element to the `Create` action method documentation.</span></span> <span data-ttu-id="41284-174">Ele complementa as informações especificadas no elemento `<summary>` e fornece uma interface de usuário do Swagger mais robusta.</span><span class="sxs-lookup"><span data-stu-id="41284-174">It supplements information specified in the `<summary>` element and provides a more robust Swagger UI.</span></span> <span data-ttu-id="41284-175">O conteúdo do elemento `<remarks>` pode consistir em texto, JSON ou XML.</span><span class="sxs-lookup"><span data-stu-id="41284-175">The `<remarks>` element content can consist of text, JSON, or XML.</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

<span data-ttu-id="41284-176">Observe os aprimoramentos da interface do usuário com esses comentários adicionais:</span><span class="sxs-lookup"><span data-stu-id="41284-176">Notice the UI enhancements with these additional comments:</span></span>

![Interface do usuário do Swagger com comentários adicionais mostrados](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a><span data-ttu-id="41284-178">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="41284-178">Data annotations</span></span>

<span data-ttu-id="41284-179">Decore o modelo com atributos, encontrados no namespace [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations), para ajudar a gerar os componentes da interface do usuário do Swagger.</span><span class="sxs-lookup"><span data-stu-id="41284-179">Decorate the model with attributes, found in the [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace, to help drive the Swagger UI components.</span></span>

<span data-ttu-id="41284-180">Adicione o atributo `[Required]` à propriedade `Name` da classe `TodoItem`:</span><span class="sxs-lookup"><span data-stu-id="41284-180">Add the `[Required]` attribute to the `Name` property of the `TodoItem` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Models/TodoItem.cs?highlight=10)]

<span data-ttu-id="41284-181">A presença desse atributo altera o comportamento da interface do usuário e altera o esquema JSON subjacente:</span><span class="sxs-lookup"><span data-stu-id="41284-181">The presence of this attribute changes the UI behavior and alters the underlying JSON schema:</span></span>

```json
"definitions": {
    "TodoItem": {
        "required": [
            "name"
        ],
        "type": "object",
        "properties": {
            "id": {
                "format": "int64",
                "type": "integer"
            },
            "name": {
                "type": "string"
            },
            "isComplete": {
                "default": false,
                "type": "boolean"
            }
        }
    }
},
```

<span data-ttu-id="41284-182">Adicione o atributo `[Produces("application/json")]` ao controlador da API.</span><span class="sxs-lookup"><span data-stu-id="41284-182">Add the `[Produces("application/json")]` attribute to the API controller.</span></span> <span data-ttu-id="41284-183">Sua finalidade é declarar que as ações do controlador permitem o tipo de conteúdo de resposta *application/json*:</span><span class="sxs-lookup"><span data-stu-id="41284-183">Its purpose is to declare that the controller's actions support a response content type of *application/json*:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=3)]

<span data-ttu-id="41284-184">A lista suspensa **Tipo de Conteúdo de Resposta** seleciona esse tipo de conteúdo como o padrão para ações GET do controlador:</span><span class="sxs-lookup"><span data-stu-id="41284-184">The **Response Content Type** drop-down selects this content type as the default for the controller's GET actions:</span></span>

![Interface do usuário do Swagger com o tipo de conteúdo de resposta padrão](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

<span data-ttu-id="41284-186">À medida que aumenta o uso de anotações de dados na API Web, a interface do usuário e as páginas de ajuda da API se tornam mais descritivas e úteis.</span><span class="sxs-lookup"><span data-stu-id="41284-186">As the usage of data annotations in the Web API increases, the UI and API help pages become more descriptive and useful.</span></span>

### <a name="describe-response-types"></a><span data-ttu-id="41284-187">Descrever os tipos de resposta</span><span class="sxs-lookup"><span data-stu-id="41284-187">Describe response types</span></span>

<span data-ttu-id="41284-188">Os desenvolvedores de consumo estão mais preocupados com o que é retornado, especificamente, o tipos de resposta e o códigos de erro (se eles não forem padrão).</span><span class="sxs-lookup"><span data-stu-id="41284-188">Consuming developers are most concerned with what's returned&mdash;specifically response types and error codes (if not standard).</span></span> <span data-ttu-id="41284-189">Os tipos de resposta e os códigos de erro são indicados nos comentários XML e nas anotações de dados.</span><span class="sxs-lookup"><span data-stu-id="41284-189">The response types and error codes are denoted in the XML comments and data annotations.</span></span>

<span data-ttu-id="41284-190">A ação `Create` retorna um código de status HTTP 201 em caso de sucesso.</span><span class="sxs-lookup"><span data-stu-id="41284-190">The `Create` action returns an HTTP 201 status code on success.</span></span> <span data-ttu-id="41284-191">Um código de status HTTP 400 é retornado quando o corpo da solicitação postada é nulo.</span><span class="sxs-lookup"><span data-stu-id="41284-191">An HTTP 400 status code is returned when the posted request body is null.</span></span> <span data-ttu-id="41284-192">Sem a documentação adequada na interface do usuário do Swagger, o consumidor não tem conhecimento desses resultados esperados.</span><span class="sxs-lookup"><span data-stu-id="41284-192">Without proper documentation in the Swagger UI, the consumer lacks knowledge of these expected outcomes.</span></span> <span data-ttu-id="41284-193">Corrija esse problema adicionando as linhas realçadas no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="41284-193">Fix that problem by adding the highlighted lines in the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

<span data-ttu-id="41284-194">A interface do usuário do Swagger agora documenta claramente os códigos de resposta HTTP esperados:</span><span class="sxs-lookup"><span data-stu-id="41284-194">The Swagger UI now clearly documents the expected HTTP response codes:</span></span>

![A interface do usuário do Swagger mostra a descrição da classe de resposta POST, 'Retorna o item de tarefa pendente recém-criado' e '400 – se o item for nulo' para o código de status e o motivo em Mensagens de Resposta](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

### <a name="customize-the-ui"></a><span data-ttu-id="41284-196">Personalizar a interface do usuário</span><span class="sxs-lookup"><span data-stu-id="41284-196">Customize the UI</span></span>

<span data-ttu-id="41284-197">A interface do usuário de estoque é apresentável e funcional.</span><span class="sxs-lookup"><span data-stu-id="41284-197">The stock UI is both functional and presentable.</span></span> <span data-ttu-id="41284-198">No entanto, as páginas de documentação da API devem representar a sua marca ou o seu tema.</span><span class="sxs-lookup"><span data-stu-id="41284-198">However, API documentation pages should represent your brand or theme.</span></span> <span data-ttu-id="41284-199">Para inserir sua marca nos componentes do Swashbuckle é necessário adicionar recursos para atender aos arquivos estáticos e criar a estrutura de pasta para hospedar esses arquivos.</span><span class="sxs-lookup"><span data-stu-id="41284-199">Branding the Swashbuckle components requires adding the resources to serve static files and building the folder structure to host those files.</span></span>

<span data-ttu-id="41284-200">Se você estiver direcionando ao .NET Framework ou ao .NET Core 1.x, adicione o pacote do NuGet [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) ao projeto:</span><span class="sxs-lookup"><span data-stu-id="41284-200">If targeting .NET Framework or .NET Core 1.x, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) NuGet package to the project:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

<span data-ttu-id="41284-201">O pacote do NuGet anterior já estará instalado se você estiver direcionando ao .NET Core 2.x e usando o [metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="41284-201">The preceding NuGet package is already installed if targeting .NET Core 2.x and using the [metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="41284-202">Habilite o middleware de arquivos estáticos:</span><span class="sxs-lookup"><span data-stu-id="41284-202">Enable the static files middleware:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Startup.cs?name=snippet_Configure&highlight=3)]

<span data-ttu-id="41284-203">Adquira o conteúdo da pasta *dist* do [repositório GitHub da interface do usuário do Swagger](https://github.com/swagger-api/swagger-ui/tree/master/dist).</span><span class="sxs-lookup"><span data-stu-id="41284-203">Acquire the contents of the *dist* folder from the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui/tree/master/dist).</span></span> <span data-ttu-id="41284-204">Essa pasta contém os ativos necessários para a página da interface do usuário do Swagger.</span><span class="sxs-lookup"><span data-stu-id="41284-204">This folder contains the necessary assets for the Swagger UI page.</span></span>

<span data-ttu-id="41284-205">Crie uma pasta *swagger/wwwroot/ui* e copie para ela o conteúdo da pasta *dist*.</span><span class="sxs-lookup"><span data-stu-id="41284-205">Create a *wwwroot/swagger/ui* folder, and copy into it the contents of the *dist* folder.</span></span>

<span data-ttu-id="41284-206">Crie um arquivo *custom.css* em *swagger/wwwroot/ui*, com o seguinte CSS para personalizar o cabeçalho da página:</span><span class="sxs-lookup"><span data-stu-id="41284-206">Create a *custom.css* file, in *wwwroot/swagger/ui*, with the following CSS to customize the page header:</span></span>

[!code-css[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/wwwroot/swagger/ui/custom.css)]

<span data-ttu-id="41284-207">Referencie *custom.css* no arquivo *index.html* depois de todos os outros arquivos CSS:</span><span class="sxs-lookup"><span data-stu-id="41284-207">Reference *custom.css* in the *index.html* file, after any other CSS files:</span></span>

[!code-html[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/wwwroot/swagger/ui/index.html?name=snippet_SwaggerUiCss&highlight=3)]

<span data-ttu-id="41284-208">Navegue até a página *index.html* em `http://localhost:<random_port>/swagger/ui/index.html`.</span><span class="sxs-lookup"><span data-stu-id="41284-208">Browse to the *index.html* page at `http://localhost:<random_port>/swagger/ui/index.html`.</span></span> <span data-ttu-id="41284-209">Digite `http://localhost:<random_port>/swagger/v1/swagger.json` na caixa de texto do cabeçalho e clique no botão **Explorar**.</span><span class="sxs-lookup"><span data-stu-id="41284-209">Enter `http://localhost:<random_port>/swagger/v1/swagger.json` in the header's textbox, and click the **Explore** button.</span></span> <span data-ttu-id="41284-210">A página resultante será semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="41284-210">The resulting page looks as follows:</span></span>

![Interface do usuário do Swagger com o título do cabeçalho personalizado](web-api-help-pages-using-swagger/_static/custom-header.png)

<span data-ttu-id="41284-212">Há muito mais que você pode fazer com a página.</span><span class="sxs-lookup"><span data-stu-id="41284-212">There's much more you can do with the page.</span></span> <span data-ttu-id="41284-213">Consulte as funcionalidades completas para os recursos de interface do usuário no [repositório GitHub da interface do usuário do Swagger](https://github.com/swagger-api/swagger-ui).</span><span class="sxs-lookup"><span data-stu-id="41284-213">See the full capabilities for the UI resources at the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui).</span></span>
