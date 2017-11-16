---
title: "Páginas de ajuda da API Web ASP.NET Core usando o Swagger"
author: spboyer
description: "Este tutorial fornece um passo a passo da adição de Swagger para gerar a documentação e páginas Ajuda para um aplicativo de API Web."
keywords: "ASP.NET Core, Swagger, Swashbuckle, páginas de ajuda, API Web"
ms.author: spboyer
manager: wpickett
ms.date: 09/01/2017
ms.topic: article
ms.assetid: 54bb961d-29d9-4dee-8e2c-a93fc33c16f2
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/web-api-help-pages-using-swagger
ms.openlocfilehash: 8a87972a7394246ece2af3485d93739975ba5383
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-core-web-api-help-pages-using-swagger"></a><span data-ttu-id="8e23c-104">Páginas de ajuda da API Web ASP.NET Core usando o Swagger</span><span class="sxs-lookup"><span data-stu-id="8e23c-104">ASP.NET Core Web API Help Pages using Swagger</span></span>

<a name="web-api-help-pages-using-swagger"></a>

<span data-ttu-id="8e23c-105">Por [Shayne Boyer](https://twitter.com/spboyer) e [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="8e23c-105">By [Shayne Boyer](https://twitter.com/spboyer) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="8e23c-106">Compreender os vários métodos de uma API pode ser um desafio para um desenvolvedor durante a criação de um aplicativo de consumo.</span><span class="sxs-lookup"><span data-stu-id="8e23c-106">Understanding the various methods of an API can be a challenge for a developer when building a consuming application.</span></span>

<span data-ttu-id="8e23c-107">A geração de boa documentação e páginas de ajuda para a API Web, usando [Swagger](https://swagger.io/) com a implementação do .NET Core [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore), é tão fácil quanto adicionar alguns pacotes NuGet e modificar o *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="8e23c-107">Generating good documentation and help pages for your Web API, using [Swagger](https://swagger.io/) with the .NET Core implementation [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore), is as easy as adding a couple of NuGet packages and modifying the *Startup.cs*.</span></span>

* <span data-ttu-id="8e23c-108">O [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) é um projeto de software livre para geração de documentos do Swagger para APIs Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8e23c-108">[Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) is an open source project for generating Swagger documents for ASP.NET Core Web APIs.</span></span>

* <span data-ttu-id="8e23c-109">O [Swagger](https://swagger.io/) é uma representação legível por computador de uma API RESTful que habilita o suporte para documentação interativa, geração de SDK do cliente e detectabilidade.</span><span class="sxs-lookup"><span data-stu-id="8e23c-109">[Swagger](https://swagger.io/) is a machine-readable representation of a RESTful API that enables support for interactive documentation, client SDK generation, and discoverability.</span></span>

<span data-ttu-id="8e23c-110">Este tutorial se baseia no exemplo [Criando sua primeira API Web com ASP.NET Core MVC e Visual Studio](xref:tutorials/first-web-api).</span><span class="sxs-lookup"><span data-stu-id="8e23c-110">This tutorial builds on the sample on [Building Your First Web API with ASP.NET Core MVC and Visual Studio](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="8e23c-111">Se você quiser acompanhar, baixe a amostra em [https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample).</span><span class="sxs-lookup"><span data-stu-id="8e23c-111">If you'd like to follow along, download the sample at [https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample).</span></span>

## <a name="getting-started"></a><span data-ttu-id="8e23c-112">Guia de Introdução</span><span class="sxs-lookup"><span data-stu-id="8e23c-112">Getting Started</span></span>

<span data-ttu-id="8e23c-113">Há três componentes principais bo Swashbuckle:</span><span class="sxs-lookup"><span data-stu-id="8e23c-113">There are three main components to Swashbuckle:</span></span>

* <span data-ttu-id="8e23c-114">`Swashbuckle.AspNetCore.Swagger`: um modelo de objeto de Swagger e middleware para expor objetos `SwaggerDocument` como pontos de extremidade JSON.</span><span class="sxs-lookup"><span data-stu-id="8e23c-114">`Swashbuckle.AspNetCore.Swagger`: a Swagger object model and middleware to expose `SwaggerDocument` objects as JSON endpoints.</span></span>

* <span data-ttu-id="8e23c-115">`Swashbuckle.AspNetCore.SwaggerGen`: um gerador de Swagger cria objetos `SwaggerDocument` diretamente dos modelos, controladores e rotas.</span><span class="sxs-lookup"><span data-stu-id="8e23c-115">`Swashbuckle.AspNetCore.SwaggerGen`: a Swagger generator that builds `SwaggerDocument` objects directly from your routes, controllers, and models.</span></span> <span data-ttu-id="8e23c-116">Normalmente, ele é combinado com o middleware de ponto de extremidade do Swagger para expor automaticamente o JSON do Swagger.</span><span class="sxs-lookup"><span data-stu-id="8e23c-116">It's typically combined with the Swagger endpoint middleware to automatically expose Swagger JSON.</span></span>

* <span data-ttu-id="8e23c-117">`Swashbuckle.AspNetCore.SwaggerUI`: uma versão inserida da ferramenta de interface do usuário Swagger, que interpreta JSON do Swagger a fim de criar uma experiência rica e personalizável para descrever a funcionalidade da API Web.</span><span class="sxs-lookup"><span data-stu-id="8e23c-117">`Swashbuckle.AspNetCore.SwaggerUI`: an embedded version of the Swagger UI tool which interprets Swagger JSON to build a rich, customizable experience for describing the Web API functionality.</span></span> <span data-ttu-id="8e23c-118">Ela inclui o agente de teste interno para os métodos públicos.</span><span class="sxs-lookup"><span data-stu-id="8e23c-118">It includes built-in test harnesses for the public methods.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="8e23c-119">Pacotes NuGet</span><span class="sxs-lookup"><span data-stu-id="8e23c-119">NuGet Packages</span></span>

<span data-ttu-id="8e23c-120">O Swashbuckle pode ser adicionado com as seguintes abordagens:</span><span class="sxs-lookup"><span data-stu-id="8e23c-120">Swashbuckle can be added with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8e23c-121">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8e23c-121">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="8e23c-122">Da janela **Console do Gerenciador de Pacotes**:</span><span class="sxs-lookup"><span data-stu-id="8e23c-122">From the **Package Manager Console** window:</span></span>

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* <span data-ttu-id="8e23c-123">Da caixa de diálogo **Gerenciar Pacotes NuGet**:</span><span class="sxs-lookup"><span data-stu-id="8e23c-123">From the **Manage NuGet Packages** dialog:</span></span>

     * <span data-ttu-id="8e23c-124">Clique com o botão direito do mouse no projeto em **Gerenciador de Soluções** > **Gerenciar Pacotes NuGet**</span><span class="sxs-lookup"><span data-stu-id="8e23c-124">Right-click your project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
     * <span data-ttu-id="8e23c-125">Defina a **Origem do pacote** para "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="8e23c-125">Set the **Package source** to "nuget.org"</span></span>
     * <span data-ttu-id="8e23c-126">Insira "Swashbuckle.AspNetCore" na caixa de pesquisa</span><span class="sxs-lookup"><span data-stu-id="8e23c-126">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
     * <span data-ttu-id="8e23c-127">Selecione o pacote "Swashbuckle.AspNetCore" na guia **Procurar** e clique em **Instalar**</span><span class="sxs-lookup"><span data-stu-id="8e23c-127">Select the "Swashbuckle.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8e23c-128">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="8e23c-128">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="8e23c-129">Clique com o botão direito do mouse na pasta *Pacotes* em **Painel de Soluções** > **Adicionar Pacotes...**</span><span class="sxs-lookup"><span data-stu-id="8e23c-129">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="8e23c-130">Defina a lista suspensa **Origem** da janela **Adicionar Pacotes** para "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="8e23c-130">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="8e23c-131">Insira Swashbuckle.AspNetCore na caixa de pesquisa</span><span class="sxs-lookup"><span data-stu-id="8e23c-131">Enter Swashbuckle.AspNetCore in the search box</span></span>
* <span data-ttu-id="8e23c-132">Selecione o pacote "Swashbuckle.AspNetCore" no painel de resultados e clique em **Adicionar Pacote**</span><span class="sxs-lookup"><span data-stu-id="8e23c-132">Select the Swashbuckle.AspNetCore package from the results pane and click **Add Package**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8e23c-133">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8e23c-133">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="8e23c-134">Execute o comando a seguir do **Terminal Integrado**:</span><span class="sxs-lookup"><span data-stu-id="8e23c-134">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8e23c-135">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="8e23c-135">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="8e23c-136">Execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="8e23c-136">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-to-the-middleware"></a><span data-ttu-id="8e23c-137">Adicionar e configurar o Swagger para o middleware</span><span class="sxs-lookup"><span data-stu-id="8e23c-137">Add and configure Swagger to the middleware</span></span>

<span data-ttu-id="8e23c-138">Adicione o gerador de Swagger à coleção de serviços no método `ConfigureServices` de *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="8e23c-138">Add the Swagger generator to the services collection in the `ConfigureServices` method of *Startup.cs*:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_ConfigureServices&highlight=7-10)]

<span data-ttu-id="8e23c-139">Adicione a seguinte instrução using da classe `Info`:</span><span class="sxs-lookup"><span data-stu-id="8e23c-139">Add the following using statement for the `Info` class:</span></span>

```csharp
using Swashbuckle.AspNetCore.Swagger;
```

<span data-ttu-id="8e23c-140">No método `Configure` de *Startup.cs*, habilite o middleware para servir o documento JSON gerado e o SwaggerUI:</span><span class="sxs-lookup"><span data-stu-id="8e23c-140">In the `Configure` method of *Startup.cs*, enable the middleware for serving the generated JSON document and the SwaggerUI:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_Configure&highlight=4,7-10)]

<span data-ttu-id="8e23c-141">Inicie o aplicativo e navegue até `http://localhost:<random_port>/swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="8e23c-141">Launch the app, and navigate to `http://localhost:<random_port>/swagger/v1/swagger.json`.</span></span> <span data-ttu-id="8e23c-142">O documento gerado que descreve os pontos de extremidade é exibido.</span><span class="sxs-lookup"><span data-stu-id="8e23c-142">The generated document describing the endpoints appears.</span></span>

<span data-ttu-id="8e23c-143">**Observação:** Microsoft Edge, Google Chrome e Firefox exibem documentos JSON nativamente.</span><span class="sxs-lookup"><span data-stu-id="8e23c-143">**Note:** Microsoft Edge, Google Chrome, and Firefox display JSON documents natively.</span></span> <span data-ttu-id="8e23c-144">Há extensões para o Chrome que formatam o documento para facilitar a leitura.</span><span class="sxs-lookup"><span data-stu-id="8e23c-144">There are extensions for Chrome that format the document for easier reading.</span></span> <span data-ttu-id="8e23c-145">*O exemplo a seguir é reduzido para fins de brevidade.*</span><span class="sxs-lookup"><span data-stu-id="8e23c-145">*The following example is reduced for brevity.*</span></span>

```json
{
   "swagger": "2.0",
   "info": {
       "version": "v1",
       "title": "API V1"
   },
   "basePath": "/",
   "paths": {
       "/api/Todo": {
           "get": {
               "tags": [
                   "Todo"
               ],
               "operationId": "ApiTodoGet",
               "consumes": [],
               "produces": [
                   "text/plain",
                   "application/json",
                   "text/json"
               ],
               "responses": {
                   "200": {
                       "description": "Success",
                       "schema": {
                           "type": "array",
                           "items": {
                               "$ref": "#/definitions/TodoItem"
                           }
                       }
                   }
                }
           },
           "post": {
               ...
           }
       },
       "/api/Todo/{id}": {
           "get": {
               ...
           },
           "put": {
               ...
           },
           "delete": {
               ...
   },
   "definitions": {
       "TodoItem": {
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
   "securityDefinitions": {}
}
```

<span data-ttu-id="8e23c-146">Este documento orienta sobre a interface do usuário do Swagger, que pode ser exibida navegando para `http://localhost:<random_port>/swagger`:</span><span class="sxs-lookup"><span data-stu-id="8e23c-146">This document drives the Swagger UI, which can be viewed by navigating to `http://localhost:<random_port>/swagger`:</span></span>

![Interface do usuário do Swagger](web-api-help-pages-using-swagger/_static/swagger-ui.png)

<span data-ttu-id="8e23c-148">Cada método de ação pública em `TodoController` pode ser testado da interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="8e23c-148">Each public action method in `TodoController` can be tested from the UI.</span></span> <span data-ttu-id="8e23c-149">Clique em um nome de método para expandir a seção.</span><span class="sxs-lookup"><span data-stu-id="8e23c-149">Click a method name to expand the section.</span></span> <span data-ttu-id="8e23c-150">Adicione quaisquer parâmetros necessários e clique em "Experimente!".</span><span class="sxs-lookup"><span data-stu-id="8e23c-150">Add any necessary parameters, and click "Try it out!".</span></span>

![Teste GET de Swagger de exemplo](web-api-help-pages-using-swagger/_static/get-try-it-out.png)

## <a name="customization--extensibility"></a><span data-ttu-id="8e23c-152">Personalização e extensibilidade</span><span class="sxs-lookup"><span data-stu-id="8e23c-152">Customization & Extensibility</span></span>

<span data-ttu-id="8e23c-153">O Swagger fornece opções para documentar o modelo de objeto e personalizar a interface do usuário para corresponder ao seu tema.</span><span class="sxs-lookup"><span data-stu-id="8e23c-153">Swagger provides options for documenting the object model and customizing the UI to match your theme.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="8e23c-154">Informações e descrição da API</span><span class="sxs-lookup"><span data-stu-id="8e23c-154">API Info and Description</span></span>

<span data-ttu-id="8e23c-155">A ação de configuração passada para o método `AddSwaggerGen` pode ser usada para adicionar informações como o autor, a licença e a descrição:</span><span class="sxs-lookup"><span data-stu-id="8e23c-155">The configuration action passed to the `AddSwaggerGen` method can be used to add information such as the author, license, and description:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?range=20-30,36)]

<span data-ttu-id="8e23c-156">A imagem a seguir representa a interface do usuário do Swagger exibindo as informações de versão:</span><span class="sxs-lookup"><span data-stu-id="8e23c-156">The following image depicts the Swagger UI displaying the version information:</span></span>

![A interface do usuário do Swagger com informações de versão: descrição, autor e link veja mais](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a><span data-ttu-id="8e23c-158">Comentários XML</span><span class="sxs-lookup"><span data-stu-id="8e23c-158">XML Comments</span></span>

<span data-ttu-id="8e23c-159">Comentários XML podem ser habilitados com as seguintes abordagens:</span><span class="sxs-lookup"><span data-stu-id="8e23c-159">XML comments can be enabled with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8e23c-160">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8e23c-160">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="8e23c-161">Clique com o botão direito do mouse no projeto no **Gerenciador de Soluções** e selecione **Propriedades**</span><span class="sxs-lookup"><span data-stu-id="8e23c-161">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="8e23c-162">Verifique a caixa **Arquivo de documentação XML** sob a seção **Saída** da guia **Build**:</span><span class="sxs-lookup"><span data-stu-id="8e23c-162">Check the **XML documentation file** box under the **Output** section of the **Build** tab:</span></span>

![Compile a guia das propriedades do projeto](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8e23c-164">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="8e23c-164">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="8e23c-165">Abra a caixa de diálogo **Opções do Projeto** > **Compilar** > **Compilador**</span><span class="sxs-lookup"><span data-stu-id="8e23c-165">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="8e23c-166">Verifique a caixa **Gerar documentação XML** sob a seção **Opções Gerais**:</span><span class="sxs-lookup"><span data-stu-id="8e23c-166">Check the **Generate xml documentation** box under the **General Options** section:</span></span>

![Seção Opções Gerais das opções do projeto](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8e23c-168">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8e23c-168">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="8e23c-169">Adicione manualmente o trecho a seguir ao arquivo *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="8e23c-169">Manually add the following snippet to the *.csproj* file:</span></span>

[!code-xml[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/TodoApi.csproj?range=7-9)]

---

<span data-ttu-id="8e23c-170">Configure o Swagger para usar o arquivo XML gerado.</span><span class="sxs-lookup"><span data-stu-id="8e23c-170">Configure Swagger to use the generated XML file.</span></span> <span data-ttu-id="8e23c-171">Para sistemas operacionais Linux ou não Windows, caminhos e nomes de arquivo podem diferenciar maiúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="8e23c-171">For Linux or non-Windows operating systems, file names and paths can be case sensitive.</span></span> <span data-ttu-id="8e23c-172">Por exemplo, um arquivo *ToDoApi.XML* pode ser encontrado no Windows, mas não em CentOS.</span><span class="sxs-lookup"><span data-stu-id="8e23c-172">For example, a *ToDoApi.XML* file would be found on Windows but not CentOS.</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_ConfigureServices&highlight=20-22)]

<span data-ttu-id="8e23c-173">No código anterior, `ApplicationBasePath` obtém o caminho base do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8e23c-173">In the preceding code, `ApplicationBasePath` gets the base path of the app.</span></span> <span data-ttu-id="8e23c-174">O caminho base é usado para localizar o arquivo de comentários XML.</span><span class="sxs-lookup"><span data-stu-id="8e23c-174">The base path is used to locate the XML comments file.</span></span> <span data-ttu-id="8e23c-175">*TodoApi.xml* funciona apenas para este exemplo, desde que o nome do arquivo de comentários XML gerado seja baseado no nome do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8e23c-175">*TodoApi.xml* only works for this example, since the name of the generated XML comments file is based on the application name.</span></span>

<span data-ttu-id="8e23c-176">Adicionar comentários de barra tripla ao método aprimora a interface do usuário do Swagger adicionando a descrição ao cabeçalho da seção:</span><span class="sxs-lookup"><span data-stu-id="8e23c-176">Adding the triple-slash comments to the method enhances the Swagger UI by adding the description to the section header:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete&highlight=2)]

![A interface do usuário do Swagger, mostrando o comentário XML 'Exclui um TodoItem específico'.](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

<span data-ttu-id="8e23c-179">A interface do usuário é controlada pelo arquivo JSON gerado, que também contém estes comentários:</span><span class="sxs-lookup"><span data-stu-id="8e23c-179">The UI is driven by the generated JSON file, which also contains these comments:</span></span>

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

<span data-ttu-id="8e23c-180">Adicione uma marca [<remarks>](https://docs.microsoft.com/dotnet/csharp/programming-guide/xmldoc/remarks) à documentação do método de ação `Create`.</span><span class="sxs-lookup"><span data-stu-id="8e23c-180">Add a [<remarks>](https://docs.microsoft.com/dotnet/csharp/programming-guide/xmldoc/remarks) tag to the `Create` action method documentation.</span></span> <span data-ttu-id="8e23c-181">Ele suplementa as informações especificadas na marca `<summary>` e fornece uma interface do usuário do Swagger mais robusta.</span><span class="sxs-lookup"><span data-stu-id="8e23c-181">It supplements information specified in the `<summary>` tag and provides a more robust Swagger UI.</span></span> <span data-ttu-id="8e23c-182">O conteúdo de marca `<remarks>` pode consistir em texto, JSON ou XML.</span><span class="sxs-lookup"><span data-stu-id="8e23c-182">The `<remarks>` tag content can consist of text, JSON, or XML.</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

<span data-ttu-id="8e23c-183">Observe os aprimoramentos de interface do usuário com esses comentários adicionais.</span><span class="sxs-lookup"><span data-stu-id="8e23c-183">Notice the UI enhancements with these additional comments.</span></span>

![Interface do usuário do Swagger com comentários adicionais mostrados](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a><span data-ttu-id="8e23c-185">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="8e23c-185">Data Annotations</span></span>

<span data-ttu-id="8e23c-186">Decore o modelo com atributos encontrados em `System.ComponentModel.DataAnnotations` para ajudar a controlar os componentes de interface do usuário do Swagger.</span><span class="sxs-lookup"><span data-stu-id="8e23c-186">Decorate the model with attributes, found in `System.ComponentModel.DataAnnotations`, to help drive the Swagger UI components.</span></span>

<span data-ttu-id="8e23c-187">Adicione o atributo `[Required]` à propriedade `Name` da classe `TodoItem`:</span><span class="sxs-lookup"><span data-stu-id="8e23c-187">Add the `[Required]` attribute to the `Name` property of the `TodoItem` class:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Models/TodoItem.cs?highlight=10)]

<span data-ttu-id="8e23c-188">A presença desse atributo altera o comportamento da interface do usuário e altera o esquema JSON subjacente:</span><span class="sxs-lookup"><span data-stu-id="8e23c-188">The presence of this attribute changes the UI behavior and alters the underlying JSON schema:</span></span>

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

<span data-ttu-id="8e23c-189">Adicione o atributo `[Produces("application/json")]` ao controlador da API.</span><span class="sxs-lookup"><span data-stu-id="8e23c-189">Add the `[Produces("application/json")]` attribute to the API controller.</span></span> <span data-ttu-id="8e23c-190">A finalidade dele é declarar que as ações do controlador dão suporte a retornar um tipo de conteúdo de *application/json*:</span><span class="sxs-lookup"><span data-stu-id="8e23c-190">Its purpose is to declare that the controller's actions support a return a content type of *application/json*:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_TodoController&highlight=3)]

<span data-ttu-id="8e23c-191">A lista suspensa **Tipo de Conteúdo de Resposta** seleciona esse tipo de conteúdo como o padrão para ações GET do controlador:</span><span class="sxs-lookup"><span data-stu-id="8e23c-191">The **Response Content Type** drop-down selects this content type as the default for the controller's GET actions:</span></span>

![Interface do usuário do Swagger com o tipo de conteúdo de resposta padrão](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

<span data-ttu-id="8e23c-193">À medida que aumenta o uso de anotações de dados na API Web, a interface do usuário e as páginas de ajuda da API se tornam mais descritivas e úteis.</span><span class="sxs-lookup"><span data-stu-id="8e23c-193">As the usage of data annotations in the Web API increases, the UI and API help pages become more descriptive and useful.</span></span>

### <a name="describing-response-types"></a><span data-ttu-id="8e23c-194">Descrevendo os tipos de resposta</span><span class="sxs-lookup"><span data-stu-id="8e23c-194">Describing Response Types</span></span>

<span data-ttu-id="8e23c-195">Os desenvolvedores de consumo estão mais preocupados com o que é retornado &mdash; especificamente, tipos de resposta e códigos de erro (se não padrão).</span><span class="sxs-lookup"><span data-stu-id="8e23c-195">Consuming developers are most concerned with what is returned &mdash; specifically response types and error codes (if not standard).</span></span> <span data-ttu-id="8e23c-196">Eles são manipulados nos comentários XML e anotações de dados.</span><span class="sxs-lookup"><span data-stu-id="8e23c-196">These are handled in the XML comments and data annotations.</span></span>

<span data-ttu-id="8e23c-197">A ação `Create` retorna `201 Created` em caso de êxito ou `400 Bad Request` quando o corpo da solicitação postada é nulo.</span><span class="sxs-lookup"><span data-stu-id="8e23c-197">The `Create` action returns `201 Created` on success or `400 Bad Request` when the posted request body is null.</span></span> <span data-ttu-id="8e23c-198">Sem a documentação adequada na interface do usuário do Swagger, o consumidor não tem conhecimento desses resultados esperados.</span><span class="sxs-lookup"><span data-stu-id="8e23c-198">Without proper documentation in the Swagger UI, the consumer lacks knowledge of these expected outcomes.</span></span> <span data-ttu-id="8e23c-199">Esse problema é corrigido adicionando as linhas destacadas no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="8e23c-199">That problem is fixed by adding the highlighted lines in the following example:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

<span data-ttu-id="8e23c-200">A interface do usuário do Swagger agora documenta claramente os códigos de resposta HTTP esperados:</span><span class="sxs-lookup"><span data-stu-id="8e23c-200">The Swagger UI now clearly documents the expected HTTP response codes:</span></span>

![A interface do usuário do Swagger mostra a descrição da classe de resposta POST, 'Retorna o item de tarefa pendente recém-criado' e '400 – se o item for nulo' para o código de status e o motivo em Mensagens de Resposta](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

### <a name="customizing-the-ui"></a><span data-ttu-id="8e23c-202">Personalizando a interface do usuário</span><span class="sxs-lookup"><span data-stu-id="8e23c-202">Customizing the UI</span></span>

<span data-ttu-id="8e23c-203">A interface do usuário de estoque é funcional e também apresentável; no entanto, ao criar páginas de documentação para sua API, você deseja que ela represente sua marca ou tema.</span><span class="sxs-lookup"><span data-stu-id="8e23c-203">The stock UI is both functional and presentable; however, when building documentation pages for your API, you want it to represent your brand or theme.</span></span> <span data-ttu-id="8e23c-204">A realização dessa tarefa com os componentes de Swashbuckle requer a adição de recursos para servir arquivos estáticos e, em seguida, criar a estrutura de pastas para hospedar esses arquivos.</span><span class="sxs-lookup"><span data-stu-id="8e23c-204">Accomplishing that task with the Swashbuckle components requires adding the resources to serve static files and then building the folder structure to host those files.</span></span>

<span data-ttu-id="8e23c-205">Se você usar o .NET Framework como destino, adicione o pacote NuGet `Microsoft.AspNetCore.StaticFiles` ao projeto:</span><span class="sxs-lookup"><span data-stu-id="8e23c-205">If targeting .NET Framework, add the `Microsoft.AspNetCore.StaticFiles` NuGet package to the project:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

<span data-ttu-id="8e23c-206">Habilite o middleware de arquivos estáticos:</span><span class="sxs-lookup"><span data-stu-id="8e23c-206">Enable the static files middleware:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_Configure&highlight=3)]

<span data-ttu-id="8e23c-207">Adquira o conteúdo da pasta *dist* do [repositório GitHub da interface do usuário do Swagger](https://github.com/swagger-api/swagger-ui/tree/2.x/dist).</span><span class="sxs-lookup"><span data-stu-id="8e23c-207">Acquire the contents of the *dist* folder from the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui/tree/2.x/dist).</span></span> <span data-ttu-id="8e23c-208">Essa pasta contém os ativos necessários para a página da interface do usuário do Swagger.</span><span class="sxs-lookup"><span data-stu-id="8e23c-208">This folder contains the necessary assets for the Swagger UI page.</span></span>

<span data-ttu-id="8e23c-209">Crie uma pasta *swagger/wwwroot/ui* e copie para ela o conteúdo da pasta *dist*.</span><span class="sxs-lookup"><span data-stu-id="8e23c-209">Create a *wwwroot/swagger/ui* folder, and copy into it the contents of the *dist* folder.</span></span>

<span data-ttu-id="8e23c-210">Criar um arquivo *wwwroot/swagger/ui/css/custom.css* com o CSS a seguir para personalizar o cabeçalho da página:</span><span class="sxs-lookup"><span data-stu-id="8e23c-210">Create a *wwwroot/swagger/ui/css/custom.css* file with the following CSS to customize the page header:</span></span>

[!code-css[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/css/custom.css)]

<span data-ttu-id="8e23c-211">Referencie *custom.css* no arquivo *index.html*:</span><span class="sxs-lookup"><span data-stu-id="8e23c-211">Reference *custom.css* in the *index.html* file:</span></span>

[!code-html[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/index.html?range=14)]

<span data-ttu-id="8e23c-212">Navegue até a página *index.html* em `http://localhost:<random_port>/swagger/ui/index.html`.</span><span class="sxs-lookup"><span data-stu-id="8e23c-212">Browse to the *index.html* page at `http://localhost:<random_port>/swagger/ui/index.html`.</span></span> <span data-ttu-id="8e23c-213">Digite `http://localhost:<random_port>/swagger/v1/swagger.json` na caixa de texto do cabeçalho e clique no botão **Explorar**.</span><span class="sxs-lookup"><span data-stu-id="8e23c-213">Enter `http://localhost:<random_port>/swagger/v1/swagger.json` in the header's textbox, and click the **Explore** button.</span></span> <span data-ttu-id="8e23c-214">A página resultante será semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="8e23c-214">The resulting page looks as follows:</span></span>

![Interface do usuário do Swagger com o título do cabeçalho personalizado](web-api-help-pages-using-swagger/_static/custom-header.png)

<span data-ttu-id="8e23c-216">Há muito mais que você pode fazer com a página.</span><span class="sxs-lookup"><span data-stu-id="8e23c-216">There is much more you can do with the page.</span></span> <span data-ttu-id="8e23c-217">Consulte as funcionalidades completas para os recursos de interface do usuário no [repositório GitHub da interface do usuário do Swagger](https://github.com/swagger-api/swagger-ui).</span><span class="sxs-lookup"><span data-stu-id="8e23c-217">See the full capabilities for the UI resources at the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui).</span></span>
