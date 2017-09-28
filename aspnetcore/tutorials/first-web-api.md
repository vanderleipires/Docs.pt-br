---
title: Criar uma API Web com o ASP.NET Core e o Visual Studio para Windows
author: rick-anderson
description: Compilar uma API Web com o ASP.NET Core MVC e o Visual Studio para Windows
keywords: "ASP.NET Core, WebAPI, API Web, REST, HTTP, Serviço, Serviço HTTP"
ms.author: riande
manager: wpickett
ms.date: 08/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-web-api
ms.openlocfilehash: 617b11cd7652e393c06446c62138802e4a4e90df
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/19/2017
---
#<a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a>Criar uma API Web com o ASP.NET Core e o Visual Studio para Windows

Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Mike Wasson](https://github.com/mikewasson)

Neste tutorial, você compilará uma API Web para gerenciar uma lista de itens de “tarefas pendentes”. Você não compilará uma interface do usuário.

Há três versões deste tutorial:

* Windows: API Web com o Visual Studio para Windows (este tutorial)
* macOS: [API Web com o Visual Studio para Mac](xref:tutorials/first-web-api-mac)
* macOS, Linux, Windows: [API Web com o Visual Studio Code](xref:tutorials/web-api-vsc)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE[install 2.0](../includes/install2.0.md)]

Consulte [este PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-web-api/_static/_webAPI.pdf) para o ASP.NET Core versão 1.1.

## <a name="create-the-project"></a>Criar o projeto

No Visual Studio, selecione o menu **Arquivo**, > **Novo** > **Projeto**.

Selecione o modelo de projeto **Aplicativo Web ASP.NET Core (.NET Core)**. Nomeie o projeto `TodoApi` e selecione **OK**.

![Caixa de diálogo Novo projeto](first-web-api/_static/new-project.png)

Na caixa de diálogo **Novo aplicativo Web ASP.NET Core – TodoApi**, selecione o modelo **API Web**. Selecione **OK**. **Não** selecione **Habilitar Suporte ao Docker**.

![Caixa de diálogo Novo Aplicativo Web ASP.NET com modelo de projeto de API Web selecionado de Modelos do ASP.NET Core](first-web-api/_static/web-api-project.png)

### <a name="launch-the-app"></a>Iniciar o aplicativo

No Visual Studio, pressione CTRL + F5 para iniciar o aplicativo. O Visual Studio inicia um navegador e navega para `http://localhost:port/api/values`, em que *porta* é um número da porta escolhido aleatoriamente. Chrome, Edge e Firefox exibem o seguinte:

```
["value1","value2"]
``` 

### <a name="add-a-model-class"></a>Adicionar uma classe de modelo

Um modelo é um objeto que representa os dados em seu aplicativo. Nesse caso, o único modelo é um item de tarefa pendente.

Adicione uma pasta denominada "Modelos". No Gerenciador de Soluções, clique com o botão direito do mouse no projeto. Selecione **Adicionar** > **Nova Pasta**. Nomeie a pasta como *Modelos*.

Observação: as classes de modelo podem ser colocadas em qualquer lugar no seu projeto, mas a pasta *Modelos* é usada por convenção.

Adicione uma classe `TodoItem`. Clique com o botão direito do mouse na pasta *Modelos* e selecione **Adicionar** > **Classe**. Nomeie a classe `TodoItem` e, em seguida, selecione **Adicionar**.

Substitua o código gerado pelo mostrado a seguir:

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

O banco de dados gera o `Id` quando um `TodoItem` é criado.

### <a name="create-the-database-context"></a>Criar o contexto de banco de dados

O *contexto de banco de dados* é a classe principal que coordena a funcionalidade do Entity Framework para um determinado modelo de dados. Essa classe é criada derivando-a da classe `Microsoft.EntityFrameworkCore.DbContext`.

Adicione uma classe `TodoContext`. Clique com o botão direito do mouse na pasta *Modelos* e selecione **Adicionar** > **Classe**. Nomeie a classe `TodoContext` e, em seguida, selecione **Adicionar**.

Substitua o código gerado pelo mostrado a seguir:

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a>Adicionar um controlador

No Gerenciador de Soluções, clique com o botão direito do mouse na pasta *Controladores*. Selecione **Adicionar** > **Novo Item**. Na caixa de diálogo **Adicionar Novo Item**, selecione o modelo **Classe do Controlador de API Web**. Nomeie a classe `TodoController`.

![Caixa de diálogo Adicionar Novo Item com o controlador na caixa de pesquisa e o controlador da API Web selecionados](first-web-api/_static/new_controller.png)

Substitua o código gerado pelo mostrado a seguir:

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]
  
### <a name="launch-the-app"></a>Iniciar o aplicativo

No Visual Studio, pressione CTRL + F5 para iniciar o aplicativo. O Visual Studio inicia um navegador e navega para `http://localhost:port/api/values`, em que *porta* é um número da porta escolhido aleatoriamente. Se você estiver usando o Chrome, o Edge ou o Firefox, os dados serão exibidos. Se você estiver usando o IE, ele solicitará que você abra ou salve o arquivo *values.json*. Navegue até o controlador `Todo` que acabamos de criar em `http://localhost:port/api/todo`.

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]

