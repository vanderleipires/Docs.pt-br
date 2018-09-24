---
title: Introdução a Páginas Razor no ASP.NET Core no macOS com o Visual Studio para Mac
author: rick-anderson
description: Descubra como começar a usar Páginas Razor no ASP.NET Core usando o Visual Studio para Mac.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 07/27/2017
uid: tutorials/razor-pages-mac/razor-pages-start
ms.openlocfilehash: 0e1c2a9ab436968e2c24aa5f306ec69674fb025b
ms.sourcegitcommit: c12ebdab65853f27fbb418204646baf6ce69515e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523266"
---
# <a name="get-started-with-razor-pages-in-aspnet-core-on-macos-with-visual-studio-for-mac"></a>Introdução a Páginas Razor no ASP.NET Core no macOS com o Visual Studio para Mac

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Este tutorial ensina as noções básicas de criação de um aplicativo Web de Páginas do Razor do ASP.NET Core. Recomendamos que você examine a [Introdução a Páginas do Razor](xref:razor-pages/index) antes de iniciar este tutorial. Páginas do Razor é a maneira recomendada para criar a interface do usuário para aplicativos Web no ASP.NET Core.

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [](~/includes/net-core-prereqs-macos.md)]

## <a name="create-a-razor-web-app"></a>Criar um aplicativo Web do Razor

Em um terminal, execute os seguintes comandos:

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

Os comandos anteriores usam a [CLI do .NET Core](https://docs.microsoft.com/dotnet/core/tools/dotnet) para criar e executar um projeto de Páginas do Razor. Abra um navegador em http://localhost:5000 para exibir o aplicativo.

![Página Inicial ou de Índice](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a>Abrir o projeto

Pressione Ctrl + C para encerrar o aplicativo.

No Visual Studio, selecione **Arquivo > Abrir** e, em seguida, selecione o arquivo *RazorPagesMovie.csproj*.

### <a name="launch-the-app"></a>Iniciar o aplicativo

No Visual Studio, selecione **Executar > Iniciar Sem Depuração** para iniciar o aplicativo. O Visual Studio inicia o [Kestrel](xref:fundamentals/servers/kestrel), inicia um navegador e navega para `http://localhost:5000`.

No próximo tutorial, adicionaremos um modelo para o projeto.

> [!div class="step-by-step"]
> [Próximo: adicionando um modelo](xref:tutorials/razor-pages-mac/model)
