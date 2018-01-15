---
title: Criar uma API Web com o ASP.NET Core e o VS Code
description: Compilar uma API Web no macOS, Linux ou Windows com o ASP.NET Core MVC e o Visual Studio Code
author: rick-anderson
ms.author: riande
ms.date: 09/22/2017
ms.topic: get-started-article
ms.prod: asp.net-core
ms.technology: aspnet
keywords: "ASP.NET Core, WebAPI, API Web, REST, Mac, Linux, HTTP, Serviço, Serviço HTTP, VS Code"
manager: wpickett
ms.assetid: 830b4bf5-dd14-423e-9f59-764a6f13a8f6
uid: tutorials/web-api-vsc
ms.openlocfilehash: 40f9259101e5d006378562a27e97948641e29450
ms.sourcegitcommit: 281f0c614543a6c3db565ea4655b70fe49b61d84
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/03/2018
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-code-on-linux-macos-and-windows"></a>Criar uma API Web com o ASP.NET Core MVC e o Visual Studio Code no Linux, macOS e Windows

Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Mike Wasson](https://github.com/mikewasson)

Neste tutorial, você compilará uma API Web para gerenciar uma lista de itens de “tarefas pendentes”. Você não compilará uma interface do usuário.

Há três versões deste tutorial:

* macOS, Linux, Windows: API Web com o Visual Studio Code (Este tutorial)
* macOS: [API Web com o Visual Studio para Mac](xref:tutorials/first-web-api-mac)
* Windows: [API Web com o Visual Studio para Windows](xref:tutorials/first-web-api)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="set-up-your-development-environment"></a>Configurar o ambiente de desenvolvimento

Baixe e instale:
- [SDK do .NET Core 2.0.0](https://www.microsoft.com/net/core) ou posterior.
- [Visual Studio Code](https://code.visualstudio.com)
- [Extensão do C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) do Visual Studio Code

## <a name="create-the-project"></a>Criar o projeto

Em um console, execute os seguintes comandos:

```console
mkdir TodoApi
cd TodoApi
dotnet new webapi
```

Abra a pasta *TodoApi* no Visual Studio Code (VS Code) e selecione o arquivo *Startup.cs*.

- Selecione **Sim** para a mensagem de **Aviso** “Os ativos necessários para compilar e depurar estão ausentes em 'TodoApi'. Deseja adicioná-los?”
- Selecione **Restaurar** para a mensagem **Informações** “Há dependências não resolvidas”.

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![VS Code com o Aviso – Os ativos necessários para compilar e depurar estão ausentes em “TodoApi”. Deseja adicioná-los? Não Perguntar Novamente, Agora Não, Sim](web-api-vsc/_static/vsc_restore.png)

Pressione **Depurar** (F5) para compilar e executar o programa. Em um navegador, navegue para http://localhost:5000/api/values. O seguinte é exibido:

`["value1","value2"]`

Consulte [Ajuda do Visual Studio Code](#visual-studio-code-help) para obter dicas sobre como usar o VS Code.

## <a name="add-support-for-entity-framework-core"></a>Adicionar suporte ao Entity Framework Core

A criação de um novo projeto no .NET Core 2.0 adiciona o provedor “Microsoft.AspNetCore.All” ao arquivo *TodoApi.csproj*. Não é necessário instalar o provedor de banco de dados [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) separadamente. Este provedor de banco de dados permite que o Entity Framework Core seja usado com um banco de dados em memória.

[!code-xml[Main](web-api-vsc/sample/TodoApi/TodoApi.csproj?highlight=12)]

## <a name="add-a-model-class"></a>Adicionar uma classe de modelo

Um modelo é um objeto que representa os dados em seu aplicativo. Nesse caso, o único modelo é um item de tarefas pendentes.

Adicione uma pasta chamada *Models*. Você pode colocar as classes de modelo em qualquer lugar no projeto, mas a pasta *Models* é usada por convenção.

Adicione uma classe `TodoItem` com o seguinte código:

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

O banco de dados gera o `Id` quando um `TodoItem` é criado.

## <a name="create-the-database-context"></a>Criar o contexto de banco de dados

O *contexto de banco de dados* é a classe principal que coordena a funcionalidade do Entity Framework para determinado modelo de dados. Você cria essa classe derivando-a da classe `Microsoft.EntityFrameworkCore.DbContext`.

Adicione uma classe `TodoContext` à pasta *Models*:

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a>Adicionar um controlador

Na pasta *Controllers*, crie uma classe chamada `TodoController`. Adicione o seguinte código:

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Iniciar o aplicativo

No VS Code, pressione F5 para iniciar o aplicativo. Navegue para http://localhost:5000/api/todo (o controlador `Todo` que acabamos de criar).

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a>Ajuda do Visual Studio Code

- [Introdução](https://code.visualstudio.com/docs)
- [Depuração](https://code.visualstudio.com/docs/editor/debugging)
- [Terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [Atalhos de teclado](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [Atalhos de teclado do Mac](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [Atalhos de teclado do Linux](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [Atalhos de teclado do Windows](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]


