---
title: "Introdução a Páginas do Razor no ASP.NET Core com o Visual Studio Code"
author: rick-anderson
description: "Introdução a Páginas do Razor no ASP.NET Core usando o Visual Studio Code"
keywords: "ASP.NET Core, Páginas do Razor, scaffolding, Entity Framework Core, EF, EF Core, banco de dados, mac, macOS, Visual Studio Code, Code"
ms.author: riande
manager: wpickett
ms.date: 8/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: aa39de71addb2499af6d322db6da0ec635c54970
ms.sourcegitcommit: d9ec19e5452af83648074db5d96c0a0f4f9e7f9a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/29/2017
---
# <a name="getting-started-with-razor-pages-in-aspnet-core-with-visual-studio-code"></a>Introdução a Páginas do Razor no ASP.NET Core com o Visual Studio Code

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Este tutorial ensina as noções básicas de criação de um aplicativo Web de Páginas do Razor do ASP.NET Core. Recomendamos que você conclua a [Introdução a Páginas do Razor](xref:mvc/razor-pages/index) antes de iniciar este tutorial. Páginas do Razor é a maneira recomendada para criar a interface do usuário para aplicativos Web no ASP.NET Core.

## <a name="prerequisites"></a>Pré-requisitos

Instale o seguinte:

* [SDK do .NET Core 2.0.0](https://dot.net/core) ou posterior
* [Visual Studio Code](https://code.visualstudio.com)
* [Extensão C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) do VSCode 

## <a name="create-a-razor-web-app"></a>Criar um aplicativo Web do Razor

Em um terminal, execute os seguintes comandos:

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

Os comandos anteriores usam a [CLI do .NET Core](https://docs.microsoft.com/dotnet/core/tools/dotnet) para criar e executar um projeto de Páginas do Razor. Abra um navegador em http://localhost:5000 para exibir o aplicativo.

![Página Inicial ou de Índice](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a>Abrir o projeto

Pressione Ctrl + C para encerrar o aplicativo.

No Visual Studio Code (VSCode), selecione **Arquivo > Abrir Pasta** e, em seguida, selecione a pasta *RazorPagesMovie*.

- Selecione **Sim** para a mensagem de **Aviso** "Os ativos necessários para criar e depurar estão ausentes no 'RazorPagesMovie'. Deseja adicioná-los?"
- Selecione **Restaurar** para a mensagem **Informativa** "Há dependências não resolvidas".

### <a name="launch-the-app"></a>Iniciar o aplicativo

Pressione Ctrl+F5 para iniciar o aplicativo sem depuração. Alternativamente, do menu **Depurar**, selecione **Iniciar Sem Depurar**.

No próximo tutorial, adicionaremos um modelo para o projeto. 

>[!div class="step-by-step"]
[Próximo: adicionando um modelo](xref:tutorials/razor-pages-vsc/model)  
