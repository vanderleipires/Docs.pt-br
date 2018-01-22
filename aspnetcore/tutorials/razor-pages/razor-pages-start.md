---
title: "Introdução a Páginas do Razor no ASP.NET Core"
author: rick-anderson
description: "Introdução a Páginas do Razor no ASP.NET Core"
ms.author: riande
manager: wpickett
ms.date: 12/22/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 69a5bc439130ffacf2d267c79b1a6b0347171e49
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a>Introdução às Páginas do Razor no ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Este tutorial ensina as noções básicas de criação de um aplicativo Web de Páginas do Razor do ASP.NET Core. As Páginas do Razor são a maneira recomendada para criar a interface do usuário para aplicativos Web no ASP.NET Core.

Há três versões deste tutorial:

* Windows: este tutorial
* MacOS: [Introdução a Páginas do Razor com o Visual Studio para Mac](xref:tutorials/razor-pages-mac/razor-pages-start)
* macOS, Linux e Windows: [Introdução a Páginas do Razor no ASP.NET Core com o Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

## <a name="create-a-razor-web-app"></a>Criar um aplicativo Web do Razor

* No menu **Arquivo** do Visual Studio, selecione **Novo** > **Projeto**.
* Crie um novo Aplicativo Web ASP.NET Core. Nomeie o projeto **RazorPagesMovie**. É importante nomear o projeto *RazorPagesMovie* de modo que os namespaces façam a correspondência quando você copiar/colar código.
  ![novo aplicativo Web ASP.NET Core](../../mvc/razor-pages/index/_static/np.png)
* Selecione **ASP.NET Core 2.0** na lista suspensa e selecione **Aplicativo Web**.

  [!INCLUDE[install 2.0](../../includes/dotnetcore-on-dotnetfx-vs.md)]

O modelo do Visual Studio cria um projeto inicial:

![Gerenciador de Soluções](razor-pages-start/_static/se.png)

Pressione **F5** para executar o aplicativo no modo de depuração ou **Ctrl-F5** para executar sem anexar o depurador

![Página Inicial ou de Índice](razor-pages-start/_static/home.png)

* O Visual Studio inicia o [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) e executa o aplicativo. A barra de endereços mostra `localhost:port#` e não algo como `example.com`. Isso ocorre porque `localhost` é o nome do host padrão do computador local. Localhost serve somente solicitações da Web do computador local. Quando o Visual Studio cria um projeto Web, uma porta aleatória é usada para o servidor Web. Na imagem anterior, o número da porta é 5000. Quando você executar o aplicativo, verá um número da porta diferente.
* Iniciar o aplicativo com **Ctrl+F5** (modo de não depuração) permite que você faça alterações de código, salve o arquivo, atualize o navegador e veja as alterações de código. Muitos desenvolvedores preferem usar modo de não depuração para iniciar o aplicativo e exibir alterações rapidamente.

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

>[!div class="step-by-step"]
[Próximo: adicionando um modelo](xref:tutorials/razor-pages/model)

>[!div class="step-by-step"]
[Próximo: adicionando um modelo](xref:tutorials/razor-pages/model)
