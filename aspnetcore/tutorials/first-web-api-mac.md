---
title: Criar uma API Web com o ASP.NET Core e o Visual Studio para Mac
author: rick-anderson
description: Criar uma API Web com o ASP.NET Core MVC e o Visual Studio para Mac
keywords: "ASP.NET Core, WebAPI, API Web, REST, mac, macOS, HTTP, Serviço, Serviço HTTP"
ms.author: riande
manager: wpickett
ms.date: 5/24/2017
ms.topic: get-started-article
ms.assetid: 830b4af5-ed14-1638-7734-764a6f13a8f6
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-web-api-mac
ms.openlocfilehash: 08619d3b4ab2d6fdb04794dcbafac0b696dd8504
ms.sourcegitcommit: 3273675dad5ac3e1dc1c589938b73db3f7d6660a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/28/2017
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-for-mac"></a>Criar uma API Web com o ASP.NET Core MVC e o Visual Studio para Mac

Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Mike Wasson](https://github.com/mikewasson)

Neste tutorial, você criará uma API Web para gerenciar uma lista de itens "pendentes". Você não criará uma interface do usuário.

Há três versões deste tutorial:

* macOS: API Web com o Visual Studio para Mac (este tutorial)
* Windows: [API Web com o Visual Studio para Windows](xref:tutorials/first-web-api)
* macOS, Linux, Windows: [API Web com o Visual Studio Code](xref:tutorials/web-api-vsc)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

* Consulte [Introdução ao ASP.NET Core MVC no Mac ou Linux](xref:tutorials/first-mvc-app-xplat/index) para obter um exemplo que usa um banco de dados persistente.

## <a name="prerequisites"></a>Pré-requisitos

Instale o seguinte:

- [SDK do .NET Core](https://www.microsoft.com/net/core#macos)  
- [Visual Studio para Mac](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-the-project"></a>Criar o projeto

No Visual Studio, selecione **Arquivo > Nova Solução**.

![Nova Solução do macOS](first-web-api-mac/_static/sln.png)

Selecione **Aplicativo .NET Core > API Web ASP.NET Core > Avançar**.

![Caixa de diálogo Novo Projeto do macOS](first-web-api-mac/_static/1.png)

Digite **TodoApi** para o **Nome do Projeto** e, em seguida, selecione Criar.

![caixa de diálogo de configuração](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a>Iniciar o aplicativo

No Visual Studio, selecione **Executar > Iniciar Com Depuração** para iniciar o aplicativo. O Visual Studio inicia um navegador e navega para `http://localhost:5000`. Você obtém um erro de HTTP 404 (Não Encontrado).  Altere a URL para `http://localhost:port/api/values`. Os dados de `ValuesController` serão exibidos:

```
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a>Adicionar suporte ao Entity Framework Core

Instalar o provedor de banco de dados [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/). Este provedor de banco de dados permite que o Entity Framework Core seja usado com um banco de dados em memória.

* No menu **Projeto**, selecione **Adicionar pacotes do NuGet**. 

  *  Como alternativa, clique com o botão direito do mouse em **Dependências** e, em seguida, selecione **Adicionar Pacotes**.

* Digite `EntityFrameworkCore.InMemory` na caixa de pesquisa.
* Selecione `Microsoft.EntityFrameworkCore.InMemory` e, em seguida, selecione **Adicionar Pacote**.

### <a name="add-a-model-class"></a>Adicionar uma classe de modelo

Um modelo é um objeto que representa os dados em seu aplicativo. Nesse caso, o único modelo é um item de tarefa pendente.

Adicione uma pasta denominada *Modelos*. No Gerenciador de Soluções, clique com o botão direito do mouse no projeto. Selecione **Adicionar** > **Nova Pasta**. Nomeie a pasta como *Modelos*.

![nova pasta](first-web-api-mac/_static/folder.png)

Observação: Você pode colocar as classes de modelo em qualquer lugar no seu projeto, mas a pasta *Modelos* é usada por convenção.

Adicione uma classe `TodoItem`. Clique com o botão direito do mouse na pasta *Modelos* e selecione **Adicionar > Novo Arquivo > Geral > Classe Vazia**. Nomeie a classe `TodoItem` e, em seguida, selecione **Novo**.

Substitua o código gerado por:

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

O banco de dados gera o `Id` quando um `TodoItem` é criado.

### <a name="create-the-database-context"></a>Criar o contexto do banco de dados

O *contexto de banco de dados* é a classe principal que coordena a funcionalidade do Entity Framework para um determinado modelo de dados. Você cria essa classe derivando-a da classe `Microsoft.EntityFrameworkCore.DbContext`.

Adicione uma classe denominada `TodoContext` à pasta *Modelos*.

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a>Adicionar um controlador

No Gerenciador de Soluções, na pasta *Controladores*, adicione a classe `TodoController`.

Substitua o código gerado pelo código a seguir (e adicione chaves de fechamento):

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Iniciar o aplicativo

No Visual Studio, selecione **Executar > Iniciar Com Depuração** para iniciar o aplicativo. O Visual Studio inicia um navegador e navega para `http://localhost:port`, em que *porta* é um número da porta escolhido aleatoriamente. Você obtém um erro de HTTP 404 (Não Encontrado).  Altere a URL para `http://localhost:port/api/values`. Os dados de `ValuesController` serão exibidos:

```
["value1","value2"]
```

Navegue até o controlador `Todo` no `http://localhost:port/api/todo`:

```
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a>Implementar as outras operações de CRUD

Adicionaremos os métodos `Create`, `Update` e `Delete` para o controlador. Essas são variações em um tema, então apenas mostrarei o código e realçarei as principais diferenças. Compile o projeto depois de adicionar ou alterar código.

### <a name="create"></a>Create

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Esse é um método HTTP POST, indicado pelo atributo [`[HttpPost]`](https://docs.asp.net/projects/api/en/latest/autoapi/Microsoft/AspNetCore/Mvc/HttpPostAttribute/index.html). O atributo [`[FromBody]`](https://docs.asp.net/projects/api/en/latest/autoapi/Microsoft/AspNetCore/Mvc/FromBodyAttribute/index.html) informa ao MVC para obter o valor do item de tarefa pendente do corpo da solicitação HTTP.

O método `CreatedAtRoute` retorna uma resposta 201, que é a resposta padrão para um método HTTP POST que cria um novo recurso no servidor. O `CreatedAtRoute` também adiciona um cabeçalho Local à resposta. O cabeçalho Local especifica o URI do item de tarefa pendente recém-criado. Veja [10.2.2 201 Criado](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).

### <a name="use-postman-to-send-a-create-request"></a>Use o Postman para enviar uma solicitação de criação

* Inicie o aplicativo (**Executar > Iniciar Com Depuração**).
* Inicie o Postman.

![Console do Postman](first-web-api/_static/pmc.png)

* Defina o método HTTP para `POST`
* Selecione o botão de opção **Corpo**
* Selecione o botão de opção **bruto**
* Defina o tipo como JSON
* No editor de chave-valor, insira um item de tarefa pendente, tal como

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* Selecione **Enviar**

* Selecione a guia Cabeçalhos no painel inferior e copie o cabeçalho **Local**:

![Guia Cabeçalhos do console do Postman](first-web-api/_static/pmget.png)

Você pode usar o URI do cabeçalho de local para acessar o recurso que você acabou de criar. Lembre-se de que o método `GetById` criou a rota nomeada `"GetTodo"`:

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(string id)
```

### <a name="update"></a>Atualização

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

`Update` é semelhante a `Create`, mas usa HTTP PUT. A resposta é [204 (sem conteúdo)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). Acordo com a especificação HTTP, uma solicitação PUT requer que o cliente envie a entidade atualizada inteira, não apenas os deltas. Para dar suporte a atualizações parciais, use HTTP PATCH.

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![Console do Postman mostrando a resposta 204 (sem conteúdo)](first-web-api/_static/pmcput.png)

### <a name="delete"></a>Excluir

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

A resposta é [204 (sem conteúdo)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

![Console do Postman mostrando a resposta 204 (sem conteúdo)](first-web-api/_static/pmd.png)

## <a name="next-steps"></a>Próximas etapas

* [Roteamento para Ações do Controlador](xref:mvc/controllers/routing)
* Para obter informações sobre como implantar sua API, consulte [Publicação e implantação](../publishing/index.md).
* [Exibir ou baixar o código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample)
* [Postman](https://www.getpostman.com/)
* [Fiddler](http://www.fiddler2.com/fiddler2/)
