---
title: Criar uma API Web com o ASP.NET Core e o Visual Studio Code
author: rick-anderson
description: Compilar uma API Web no macOS, Linux ou Windows com o ASP.NET Core MVC e o Visual Studio Code
ms.author: riande
ms.custom: mvc
ms.date: 05/08/2018
uid: tutorials/web-api-vsc
ms.openlocfilehash: 4c41c949a9b5ca8db8928a0a53aff928fd7c8a4e
ms.sourcegitcommit: 79b756ea03eae77a716f500ef88253ee9b1464d2
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36297245"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-code"></a>Criar uma API Web com o ASP.NET Core e o Visual Studio Code

Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Mike Wasson](https://github.com/mikewasson)

Neste tutorial, crie uma API Web para gerenciar uma lista de itens de "tarefas pendentes". Uma interface do usuário não é construída.

Há três versões deste tutorial:

* macOS, Linux, Windows: API Web com o Visual Studio Code (Este tutorial)
* macOS: [API Web com o Visual Studio para Mac](xref:tutorials/first-web-api-mac)
* Windows: [API Web com o Visual Studio para Windows](xref:tutorials/first-web-api)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE[prerequisites](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-the-project"></a>Criar o projeto

Em um console, execute os seguintes comandos:

```console
dotnet new webapi -o TodoApi
code TodoApi
```

A pasta *TodoApi* é aberta no VS Code (Visual Studio Code). Selecione o arquivo *Startup.cs*.

* Selecione **Sim** para a mensagem de **Aviso** “Os ativos necessários para compilar e depurar estão ausentes em 'TodoApi'. Deseja adicioná-los?”
* Selecione **Restaurar** para a mensagem **Informações** “Há dependências não resolvidas”.

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![VS Code com o Aviso – Os ativos necessários para compilar e depurar estão ausentes em “TodoApi”. Deseja adicioná-los? Não Perguntar Novamente, Agora Não, Sim](web-api-vsc/_static/vsc_restore.png)

Pressione **Depurar** (F5) para compilar e executar o programa. Em um navegador, navegue até http://localhost:5000/api/values. É exibida a saída a seguir:

```json
["value1","value2"]
```

Consulte [Ajuda do Visual Studio Code](#visual-studio-code-help) para obter dicas sobre como usar o VS Code.

## <a name="add-support-for-entity-framework-core"></a>Adicionar suporte ao Entity Framework Core

:::moniker range="<= aspnetcore-2.0"
A criação de um novo projeto no ASP.NET Core 2.0 adiciona a referência de pacote [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) ao arquivo *TodoApi.csproj*:

[!code-xml[](first-web-api/samples/2.0/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]
:::moniker-end
:::moniker range=">= aspnetcore-2.1"
A criação de um novo projeto no ASP.NET Core 2.1 ou posteriores adiciona a referência de pacote [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.App) ao arquivo *TodoApi.csproj*:

[!code-xml[](first-web-api/samples/2.1/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]
:::moniker-end

Não é necessário instalar o provedor de banco de dados [Entity Framework Core InMemory](/ef/core/providers/in-memory/) separadamente. Este provedor de banco de dados permite que o Entity Framework Core seja usado com um banco de dados em memória.

## <a name="add-a-model-class"></a>Adicionar uma classe de modelo

Um modelo é um objeto que representa os dados em seu aplicativo. Nesse caso, o único modelo é um item de tarefas pendentes.

Adicione uma pasta chamada *Models*. Você pode colocar as classes de modelo em qualquer lugar no projeto, mas a pasta *Models* é usada por convenção.

Adicione uma classe `TodoItem` com o seguinte código:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

O banco de dados gera o `Id` quando um `TodoItem` é criado.

## <a name="create-the-database-context"></a>Criar o contexto de banco de dados

O *contexto de banco de dados* é a classe principal que coordena a funcionalidade do Entity Framework para determinado modelo de dados. Você cria essa classe derivando-a da classe `Microsoft.EntityFrameworkCore.DbContext`.

Adicione uma classe `TodoContext` à pasta *Models*:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a>Adicionar um controlador

Na pasta *Controllers*, crie uma classe chamada `TodoController`. Substitua seu conteúdo pelo código a seguir:

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Iniciar o aplicativo

No VS Code, pressione F5 para iniciar o aplicativo. Navegue até http://localhost:5000/api/todo (o controlador `Todo` que nós criamos).

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a>Ajuda do Visual Studio Code

* [Introdução](https://code.visualstudio.com/docs)
* [Depuração](https://code.visualstudio.com/docs/editor/debugging)
* [Terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal)
* [Atalhos de teclado](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  * [Atalhos de teclado do macOS](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  * [Atalhos de teclado do Linux](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  * [Atalhos de teclado do Windows](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]
