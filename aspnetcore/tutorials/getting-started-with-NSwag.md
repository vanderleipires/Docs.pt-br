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
# <a name="get-started-with-nswag-and-aspnet-core"></a>Introdução ao NSwag e ao ASP.NET Core

Por [Christoph Nienaber](https://twitter.com/zuckerthoben) e [Rico Suter](https://rsuter.com)

O uso do [NSwag](https://github.com/RSuter/NSwag) com o middleware ASP.NET Core exige o pacote do NuGet [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/). O pacote consiste em um gerador do Swagger, uma interface do usuário do Swagger (v2 e v3) e uma [interface do usuário do ReDoc](https://github.com/Rebilly/ReDoc).

É altamente recomendável usar os recursos de geração de código do NSwag. Escolha uma das seguintes opções para a geração de código:

* Use o [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), um aplicativo da área de trabalho do Windows para a geração de código do cliente em C# e em TypeScript para sua API
* Use os pacotes do NuGet [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) ou [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) para a geração de código dentro do projeto
* Use o NSwag da [linha de comando](https://github.com/NSwag/NSwag/wiki/CommandLine)
* Use o pacote do NuGet [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild)

## <a name="features"></a>Recursos

O principal motivo para usar o NSwag é que além da capacidade de apresentar a interface do usuário do Swagger e o gerador do Swagger, também é possível usar recursos flexíveis para geração de código. Você não precisa de uma API existente. É possível usar APIs de terceiros que incorporam o Swagger e permitem que o NSwag gere uma implementação de cliente. De qualquer forma, o ciclo de desenvolvimento é acelerado e você pode se adaptar às alterações da API com mais facilidade.

## <a name="package-installation"></a>Instalação do pacote

O pacote do NuGet do NSwag pode ser adicionado com as seguintes abordagens:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Da janela **Console do Gerenciador de Pacotes**:

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* Da caixa de diálogo **Gerenciar Pacotes NuGet**:
  * Clique com o botão direito do mouse no projeto em **Gerenciador de Soluções** > **Gerenciar Pacotes NuGet**
  * Defina a **Origem do pacote** para "nuget.org"
  * Insira "NSwag.AspNetCore" na caixa de pesquisa
  * Selecione o pacote "NSwag.AspNetCore" na guia **Procurar** e clique em **Instalar**

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

* Clique com o botão direito do mouse na pasta *Pacotes* em **Painel de Soluções** > **Adicionar Pacotes...**
* Defina a lista suspensa **Origem** da janela **Adicionar Pacotes** para "nuget.org"
* Insira NSwag.AspNetCore na caixa de pesquisa
* Selecione o pacote NSwag.AspNetCore no painel de resultados e clique em **Adicionar Pacote**

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Execute o comando a seguir do **Terminal Integrado**:

```console
dotnet add TodoApi.NSwag.csproj package NSwag.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[CLI do .NET Core](#tab/netcore-cli)

Execute o seguinte comando:

```console
dotnet add TodoApi.NSwag.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a>Adicionar e configurar o middleware do Swagger

Importe os seguintes namespaces na classe `Info`:

```csharp
using NSwag.AspNetCore;
using System.Reflection;
using NJsonSchema;
```

No método `Startup.Configure`, habilite o middleware para atender à especificação do Swagger gerado e à interface do usuário do Swagger:

[!code-cs[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=4,7-10)]

Inicie o aplicativo. Navegue até `/swagger` para exibir a interface do usuário do Swagger. Navegue até `/swagger/v1/swagger.json` para exibir a especificação do Swagger.

## <a name="code-generation"></a>Geração de código

### <a name="via-nswagstudio"></a>Por meio do NSwagStudio

* Instale o `NSwagStudio` por meio do [Repositório do GitHub](https://github.com/RSuter/NSwag/wiki/NSwagStudio) oficial.

* Inicie o NSwagStudio. Insira o local do *swagger.json* ou copie-o diretamente:

![NSwagStudio](web-api-help-pages-using-swagger/_static/NSwagStudio.png)

* Indique o tipo de saída do cliente desejado. As opções incluem **Cliente TypeScript**, **Cliente CSharp** ou **Controlador da API Web do CSharp**. O uso de um controlador da API Web é basicamente uma geração inversa. Ele usa uma especificação de um serviço para recriar o serviço.

* Clique em **Gerar Saídas**.

* Veja aqui uma implementação de cliente completa do exemplo *TodoApi.NSwag* em C#:

![NSwagStudio-Output](web-api-help-pages-using-swagger/_static/NSwagStudio-Output.png)

* Coloque o arquivo em um projeto de cliente (por exemplo, um aplicativo [Xamarin.Forms](/xamarin/xamarin-forms/)). Inicie o consumo da API:

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
> Você pode injetar uma URL base e/ou um cliente HTTP no cliente da API. A melhor prática é sempre [reutilizar o HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).

Agora você pode começar a implementar facilmente a API em projetos de cliente.

### <a name="other-ways-to-generate-client-code"></a>Outras maneiras de gerar o código do cliente

Você pode gerar o código de outras maneiras que sejam mais adequadas ao seu fluxo de trabalho:

* [MSBuild](https://www.nuget.org/packages/NSwag.MSBuild/)

* [No código](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [Modelos T4](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customization"></a>Personalização

### <a name="xml-comments"></a>comentários XML

Os comentários XML podem ser habilitados com as seguintes abordagens:

#### <a name="visual-studiotabvisual-studio-xml"></a>[Visual Studio](#tab/visual-studio-xml/)
* Clique com o botão direito do mouse no projeto no **Gerenciador de Soluções** e selecione **Propriedades**
* Verifique a caixa **Arquivo de documentação XML** sob a seção **Saída** da guia **Build**:

![Compile a guia das propriedades do projeto](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

#### <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[Visual Studio para Mac](#tab/visual-studio-mac-xml/)
* Abra a caixa de diálogo **Opções do Projeto** > **Compilar** > **Compilador**
* Verifique a caixa **Gerar documentação XML** sob a seção **Opções Gerais**:

![Seção Opções Gerais das opções do projeto](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

#### <a name="visual-studio-codetabvisual-studio-code-xml"></a>[Visual Studio Code](#tab/visual-studio-code-xml/)
Adicione manualmente o trecho a seguir ao arquivo *.csproj*:

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.NSwag/TodoApiNSwag.csproj?range=7-9)]

* * *
## <a name="data-annotations"></a>Anotações de dados

O NSwag usa [Reflexão](/dotnet/csharp/programming-guide/concepts/reflection) e a melhor prática para as ações da API Web é retornar [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult). Consequentemente, o NSwag não consegue inferir o que a sua ação está executando e o que ela retorna. Considere o exemplo a seguir:

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

A ação anterior retorna `IActionResult`, mas dentro da ação, ela está retornando [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) ou [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest). As anotações de dados são usadas para informar os clientes sobre qual resposta HTTP essa ação está retornando. Decore a ação com os seguintes atributos:

```csharp
[HttpPost]
[ProducesResponseType(typeof(TodoItem), 201)] // Created
[ProducesResponseType(typeof(TodoItem), 400)] // BadRequest
public IActionResult Create([FromBody] TodoItem item)
```

O gerador do Swagger agora pode descrever essa ação precisamente e os clientes gerados conseguem saber o que recebem ao chamar o ponto de extremidade. É altamente recomendável decorar todas as ações com esses atributos. Para obter diretrizes sobre quais respostas HTTP as ações da API devem retornar, confira a [Especificação RFC 7231](https://tools.ietf.org/html/rfc7231#section-4.3).
