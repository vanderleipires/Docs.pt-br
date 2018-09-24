---
title: Criar uma API Web com o ASP.NET Core e o Visual Studio para Mac
author: rick-anderson
description: Criar uma API Web com o ASP.NET Core MVC e o Visual Studio para Mac
ms.author: riande
ms.custom: mvc
ms.date: 05/08/2018
uid: tutorials/first-web-api-mac
ms.openlocfilehash: 40f9bd9c57b97826edfddeb00cb4fb38a026d46e
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011619"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-mac"></a>Criar uma API Web com o ASP.NET Core e o Visual Studio para Mac

Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Mike Wasson](https://github.com/mikewasson)

Neste tutorial, crie uma API Web para gerenciar uma lista de itens de "tarefas pendentes". A interface do usuário não é construída.

Há três versões deste tutorial:

* macOS: API Web com o Visual Studio para Mac (este tutorial)
* Windows: [API Web com o Visual Studio para Windows](xref:tutorials/first-web-api)
* macOS, Linux, Windows: [API Web com o Visual Studio Code](xref:tutorials/web-api-vsc)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

Confira [Introdução ao ASP.NET Core MVC no Mac ou Linux](xref:tutorials/first-mvc-app-xplat/index) para obter um exemplo que usa um banco de dados persistente.

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE[](~/includes/net-core-prereqs-macos.md)]

## <a name="create-the-project"></a>Criar o projeto

No Visual Studio, selecione **Arquivo** > **Nova Solução**.

![Nova solução do macOS](first-web-api-mac/_static/sln.png)

Selecione **Aplicativo .NET Core** > **API Web ASP.NET Core** > **Avançar**.

![Caixa de diálogo Novo projeto do macOS](first-web-api-mac/_static/1.png)

Digite *TodoApi* para o **Nome do Projeto** e, em seguida, clique em **Criar**.

![caixa de diálogo de configuração](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a>Iniciar o aplicativo

No Visual Studio, selecione **Executar** > **Iniciar com Depuração** para iniciar o aplicativo. O Visual Studio inicia um navegador e navega para `http://localhost:5000`. Você obtém um erro de HTTP 404 (Não Encontrado). Altere a URL para `http://localhost:<port>/api/values`. Os dados do `ValuesController` são exibidos:

```json
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a>Adicionar suporte ao Entity Framework Core

Instalar o provedor de banco de dados [Entity Framework Core InMemory](/ef/core/providers/in-memory/). Este provedor de banco de dados permite que o Entity Framework Core seja usado com um banco de dados em memória.

* No menu **Projeto**, selecione **Adicionar pacotes do NuGet**.

  * Como alternativa, clique com o botão direito do mouse em **Dependências** e, em seguida, selecione **Adicionar Pacotes**.

* Digite `EntityFrameworkCore.InMemory` na caixa de pesquisa.
* Selecione `Microsoft.EntityFrameworkCore.InMemory` e, em seguida, selecione **Adicionar Pacote**.

### <a name="add-a-model-class"></a>Adicionar uma classe de modelo

Um modelo é um objeto que representa os dados em seu aplicativo. Nesse caso, o único modelo é um item de tarefas pendentes.

No Gerenciador de Soluções, clique com o botão direito do mouse no projeto. Selecione **Adicionar** > **Nova Pasta**. Nomeie a pasta como *Modelos*.

![nova pasta](first-web-api-mac/_static/folder.png)

> [!NOTE]
> Você pode colocar as classes de modelo em qualquer lugar no projeto, mas a pasta *Models* é usada por convenção.

Clique com o botão direito do mouse na pasta *Modelos* e selecione **Adicionar** > **Novo Arquivo** > **Geral** > **Classe Vazia**. Nomeie a classe como *TodoItem* e, em seguida, clique em **Novo**.

Substitua o código gerado por:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

O banco de dados gera o `Id` quando um `TodoItem` é criado.

### <a name="create-the-database-context"></a>Criar o contexto de banco de dados

O *contexto de banco de dados* é a classe principal que coordena a funcionalidade do Entity Framework para determinado modelo de dados. Você cria essa classe derivando-a da classe `Microsoft.EntityFrameworkCore.DbContext`.

Adicione uma classe denominada `TodoContext` à pasta *Modelos*.

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a>Adicionar um controlador

No Gerenciador de Soluções, na pasta *Controladores*, adicione a classe `TodoController`.

Substitua o código gerado pelo seguinte:

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Iniciar o aplicativo

No Visual Studio, selecione **Executar** > **Iniciar com Depuração** para iniciar o aplicativo. O Visual Studio inicia um navegador e navega para `http://localhost:<port>`, em que `<port>` é um número de porta escolhido aleatoriamente. Você obtém um erro de HTTP 404 (Não Encontrado). Altere a URL para `http://localhost:<port>/api/values`. Os dados do `ValuesController` são exibidos:

```json
["value1","value2"]
```

Navegue até o controlador `Todo` no `http://localhost:<port>/api/todo`. O seguinte JSON é retornado:

```json
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a>Implementar as outras operações CRUD

Adicionaremos os métodos `Create`, `Update` e `Delete` ao controlador. Esses métodos são variações de um mesmo tema e, portanto, mostrarei apenas o código e realçarei as principais diferenças. Compile o projeto depois de adicionar ou alterar o código.

### <a name="create"></a>Create

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

O método anterior responde a um HTTP POST, conforme indicado pelo atributo [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute). O atributo [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) informa ao MVC que ele deve obter o valor do item pendente no corpo da solicitação HTTP.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

O método anterior responde a um HTTP POST, conforme indicado pelo atributo [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute). O MVC obtém o valor do item pendente no corpo da solicitação HTTP.

::: moniker-end

O método `CreatedAtRoute` retorna uma resposta 201. Essa é a resposta padrão para um método HTTP POST que cria um novo recurso no servidor. `CreatedAtRoute` também adiciona um cabeçalho Local à resposta. O cabeçalho Location especifica o URI do item de tarefas pendentes recém-criado. Consulte [10.2.2 201 criado](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).

### <a name="use-postman-to-send-a-create-request"></a>Usar o Postman para enviar uma solicitação Create

* Inicie o aplicativo (**Executar** > **Iniciar com Depuração**).
* Abra o Postman.

![Console do Postman](first-web-api/_static/pmc.png)

* Atualize o número da porta na URL do localhost.
* Defina o método HTTP para *POST*.
* Clique na guia **Corpo**.
* Selecione o botão de opção **bruto**.
* Defina o tipo como *JSON (aplicativo/json)*.
* Insira um corpo de solicitação com um item pendente que se assemelhe ao JSON a seguir:

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* Clique no botão **Enviar**.

::: moniker range=">= aspnetcore-2.1"

> [!TIP]
> Se nenhuma resposta for exibida depois de clicar em **Enviar**, desabilite a opção **Verificação de certificação SSL**. Isso é encontrado em **Arquivo** > **Configurações**. Clique no botão **Enviar** novamente depois de desabilitar a configuração.

::: moniker-end

Clique na guia **Cabeçalhos** no painel **Resposta** e copie o valor do cabeçalho **Local**:

![Guia Cabeçalhos do console do Postman](first-web-api/_static/pmc2.png)

É possível usar o URI do cabeçalho Local para acessar o recurso que você criou. O método `Create` retorna [CreatedAtRoute](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdatroute#Microsoft_AspNetCore_Mvc_ControllerBase_CreatedAtRoute_System_String_System_Object_System_Object_). O primeiro parâmetro passado para `CreatedAtRoute` representa a rota nomeada a ser usada para gerar a URL. Lembre-se de que o método `GetById` criou a rota nomeada `"GetTodo"`:

```csharp
[HttpGet("{id}", Name = "GetTodo")]
```

### <a name="update"></a>Atualização

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

::: moniker-end

`Update` é semelhante a `Create`, mas usa HTTP PUT. A resposta é [204 (Sem conteúdo)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). De acordo com a especificação HTTP, uma solicitação PUT exige que o cliente envie a entidade atualizada inteira, não apenas os deltas. Para dar suporte a atualizações parciais, use HTTP PATCH.

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![Console do Postman mostrando a resposta 204 (Sem conteúdo)](first-web-api/_static/pmcput.png)

### <a name="delete"></a>Excluir

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

A resposta é [204 (Sem conteúdo)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

![Console do Postman mostrando a resposta 204 (Sem conteúdo)](first-web-api/_static/pmd.png)

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
