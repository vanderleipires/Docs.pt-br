---
title: Criar uma API Web com o ASP.NET Core e o Visual Studio para Windows
author: rick-anderson
description: Compilar uma API Web com o ASP.NET Core MVC e o Visual Studio para Windows
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-web-api
ms.openlocfilehash: 1680d5e0be0f4844c904d923af30634c53bd1b81
ms.sourcegitcommit: 9a35906446af7ffd4ccfc18daec38874b5abbef7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/19/2018
ms.locfileid: "34306667"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a>Criar uma API Web com o ASP.NET Core e o Visual Studio para Windows

Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Mike Wasson](https://github.com/mikewasson)

Este tutorial compilará uma API Web para gerenciar uma lista de itens de “tarefas pendentes”. Uma interface do usuário (UI) não será criada.

Há três versões deste tutorial:

* Windows: API Web com o Visual Studio para Windows (este tutorial)
* macOS: [API Web com o Visual Studio para Mac](xref:tutorials/first-web-api-mac)
* macOS, Linux, Windows: [API Web com o Visual Studio Code](xref:tutorials/web-api-vsc)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="create-the-project"></a>Criar o projeto

Siga estas etapas no Visual Studio:

* No menu **Arquivo**, selecione **Novo** > **Projeto**.
* Selecione o modelo **Aplicativo Web ASP.NET Core**. Nomeie o projeto como *TodoApi* e clique em **OK**.
* Na caixa de diálogo **Novo aplicativo Web ASP.NET Core – TodoApi** e escolha a versão do ASP.NET Core. Selecione o modelo **API** e clique em **OK**. **Não** selecione **Habilitar Suporte ao Docker**.

### <a name="launch-the-app"></a>Iniciar o aplicativo

No Visual Studio, pressione CTRL + F5 para iniciar o aplicativo. O Visual Studio inicia um navegador e navega para `http://localhost:<port>/api/values`, em que `<port>` é um número de porta escolhido aleatoriamente. O Chrome, Microsoft Edge e Firefox exibem a seguinte saída:

```json
["value1","value2"]
```

Se usar o Internet Explorer, você deverá salvar um arquivo *values.json*.

### <a name="add-a-model-class"></a>Adicionar uma classe de modelo

Um modelo é um objeto que representa os dados no aplicativo. Nesse caso, o único modelo é um item de tarefas pendentes.

No Gerenciador de Soluções, clique com o botão direito do mouse no projeto. Selecione **Adicionar** > **Nova Pasta**. Nomeie a pasta *Models*.

> [!NOTE]
> As classes de modelo podem ficar em qualquer lugar no projeto. A pasta *Modelos* é usada por convenção para as classes de modelo.

No Gerenciador de Soluções, clique com o botão direito do mouse na pasta *Modelos* e selecione **Adicionar** > **Classe**. Nomeie a classe como *TodoItem* e clique em **Adicionar**.

Atualize a classe `TodoItem` com o código a seguir:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

O banco de dados gera o `Id` quando um `TodoItem` é criado.

### <a name="create-the-database-context"></a>Criar o contexto de banco de dados

O *contexto de banco de dados* é a classe principal que coordena a funcionalidade do Entity Framework para um determinado modelo de dados. Essa classe é criada derivando-a da classe `Microsoft.EntityFrameworkCore.DbContext`.

No Gerenciador de Soluções, clique com o botão direito do mouse na pasta *Modelos* e selecione **Adicionar** > **Classe**. Nomeie a classe como *TodoContext* e clique em **Adicionar**.

Substitua a classe pelo código a seguir:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a>Adicionar um controlador

No Gerenciador de Soluções, clique com o botão direito do mouse na pasta *Controladores*. Selecione **Adicionar** > **Novo Item**. Na caixa de diálogo **Adicionar Novo Item**, selecione o modelo **Classe do Controlador de API**. Nomeie a classe como *TodoController* e clique em **Adicionar**.

![Caixa de diálogo Adicionar Novo Item com o controlador na caixa de pesquisa e o controlador da API Web selecionados](first-web-api/_static/new_controller.png)

Substitua a classe pelo código a seguir:

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Iniciar o aplicativo

No Visual Studio, pressione CTRL + F5 para iniciar o aplicativo. O Visual Studio inicia um navegador e navega para `http://localhost:<port>/api/values`, em que `<port>` é um número de porta escolhido aleatoriamente. Navegue até o controlador `Todo` no `http://localhost:<port>/api/todo`.

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
