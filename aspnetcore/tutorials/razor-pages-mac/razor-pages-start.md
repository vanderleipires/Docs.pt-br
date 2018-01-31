---
title: "Introdução a Páginas do Razor no ASP.NET Core em Mac"
author: rick-anderson
description: "Introdução a Páginas do Razor no ASP.NET Core usando o Visual Studio para Mac"
ms.author: riande
manager: wpickett
ms.date: 07/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-mac/razor-pages-start
ms.openlocfilehash: d4d8c7c543273aa38d1599b51d00a348df7182de
ms.sourcegitcommit: 09b342b45e7372ba9ebf17f35eee331e5a08fb26
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/26/2018
---
# <a name="getting-started-with-razor-pages-in-aspnet-core-with-visual-studio-for-mac"></a>Introdução a Páginas do Razor no ASP.NET Core com o Visual Studio para Mac

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Este tutorial ensina as noções básicas de criação de um aplicativo Web de Páginas do Razor do ASP.NET Core. Recomendamos que você examine a [Introdução a Páginas do Razor](xref:mvc/razor-pages/index) antes de iniciar este tutorial. Páginas do Razor é a maneira recomendada para criar a interface do usuário para aplicativos Web no ASP.NET Core.

## <a name="prerequisites"></a>Pré-requisitos

Instale o seguinte:

* [SDK do .NET Core 2.0.0](https://www.microsoft.com/net/core) ou posterior
* [Visual Studio para Mac](https://www.visualstudio.com/vs/visual-studio-mac/)

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

No Visual Studio, selecione **Arquivo > Abrir** e, em seguida, selecione o arquivo *RazorPagesMovie.csproj*.

### <a name="launch-the-app"></a>Iniciar o aplicativo

No Visual Studio, selecione **Executar > Iniciar Sem Depuração** para iniciar o aplicativo. O Visual Studio inicia o [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview), inicia um navegador e navega para `http://localhost:5000`.

No próximo tutorial, adicionaremos um modelo para o projeto.

>[!div class="step-by-step"]
[Próximo: adicionando um modelo](xref:tutorials/razor-pages-mac/model)
