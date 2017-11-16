---
title: "Introdução ao ASP.NET Core MVC e ao Visual Studio"
author: rick-anderson
description: "Introdução ao ASP.NET Core MVC e ao Visual Studio"
keywords: ASP.NET Core, MVC
ms.author: riande
manager: wpickett
ms.date: 10/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 4b86eb22e367d2305b7995421aec6f37008c5637
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio"></a>Introdução ao ASP.NET Core MVC e ao Visual Studio

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[consider RP](../../includes/razor.md)]

Há três versões deste tutorial:

* macOS: [Criar um aplicativo ASP.NET Core MVC com o Visual Studio para Mac](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows: [Criar um aplicativo ASP.NET Core MVC com o Visual Studio](xref:tutorials/first-mvc-app/start-mvc)
* macOS, Linux e Windows: [Criar um aplicativo ASP.NET Core MVC com o Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)

## <a name="install-visual-studio-and-net-core"></a>Instalar Visual Studio e .NET Core

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Instale o Visual Studio Community 2017. Selecione o download de comunidade. Ignore esta etapa se você tiver o Visual Studio 2017 instalado.

* [Instalador de home page do Visual Studio 2017](https://www.visualstudio.com/)

Execute o instalador e selecione as cargas de trabalho a seguir:

* **Desenvolvimento na Web e no ASP.NET** (em **Web e Nuvem**)
* **Desenvolvimento de plataforma cruzada do .NET Core** (em **Outros conjuntos de ferramentas**)

![**Desenvolvimento na Web e no ASP.NET** (em **Web e Nuvem**)](start-mvc/_static/web_workload.png)

![**Desenvolvimento de plataforma cruzada do .NET Core** (em **Outros conjuntos de ferramentas**)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a>Criar um aplicativo Web

No Visual Studio, selecione **Arquivo > Novo > Projeto**.

![Arquivo > Novo > Projeto](start-mvc/_static/alt_new_project.png)

Complete a caixa de diálogo **Novo Projeto**:

* No painel esquerdo, toque em **.NET Core**
* No painel central, toque em **Aplicativo Web ASP.NET Core (.NET Core)**
* Nomeie o projeto "MvcMovie" (é importante nomear o projeto "MvcMovie" para que, quando você copiar o código, o namespace corresponda).
* Toque em **OK**

![Caixa de diálogo Novo projeto, .NET Core no painel esquerdo, Web do ASP.NET Core ](start-mvc/_static/new_project2.png)


# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Complete a caixa de diálogo **Novo aplicativo Web ASP.NET Core (.NET Core) – MvcMovie**:

* Na caixa de lista suspensa do seletor de versão, selecione **ASP.NET Core 2.-**
* Selecione **Aplicativo Web (Modelo-Exibir-Controlador)**
* Toque em **OK**.

![Caixa de diálogo Novo projeto, .NET Core no painel esquerdo, Web do ASP.NET Core ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Complete a caixa de diálogo **Novo aplicativo Web ASP.NET Core (.NET Core) – MvcMovie**:

* Na caixa de lista suspensa do seletor de versão, toque em **ASP.NET Core 1.1**
* Toque em **Aplicativo Web**
* Mantenha o padrão **Sem Autenticação**
* Toque em **OK**.

![Novo aplicativo Web ASP.NET Core](start-mvc/_static/p3.png)

---

O Visual Studio usou um modelo padrão para o projeto MVC que você acabou de criar. Você pode ter um aplicativo funcionando agora mesmo digitando um nome de projeto e selecionando algumas opções. Este é um projeto inicial simples e é um bom lugar para começar,

Toque em **F5** para executar o aplicativo no modo de depuração ou **Ctrl-F5** para executá-lo no modo de não depuração.
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![aplicativo em execução](start-mvc/_static/1.png)

* O Visual Studio inicia o [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) e executa o aplicativo. Observe que a barra de endereços mostra `localhost:port#` e não algo como `example.com`. Isso ocorre porque `localhost` é o nome do host padrão do computador local. Quando o Visual Studio cria um projeto Web, uma porta aleatória é usada para o servidor Web. Na imagem acima, o número da porta é 5000. Quando você executar o aplicativo, você verá um número da porta diferente.
* Iniciar o aplicativo com **Ctrl+F5** (modo de não depuração) permite que você faça alterações de código, salve o arquivo, atualize o navegador e veja as alterações de código. Muitos desenvolvedores preferem usar modo de não depuração para iniciar o aplicativo e exibir alterações rapidamente.
* Você pode iniciar o aplicativo no modo de não depuração ou de depuração por meio do item de menu **Depurar**:

![Menu Depurar](start-mvc/_static/debug_menu.png)

* Você pode depurar o aplicativo tocando no botão **IIS Express**

![IIS Express](start-mvc/_static/iis_express.png)

O modelo padrão fornece os links funcionais **Página Inicial, Sobre** e **Contato**. A imagem do navegador acima não mostra esses links. Dependendo do tamanho do navegador, talvez você precise clicar no ícone de navegação para mostrá-los.

![ícone de navegação na parte superior direita](start-mvc/_static/2.png)

Se você estava executando no modo de depuração, toque em **Shift-F5** para interromper a depuração.

Na próxima parte deste tutorial, saberemos mais sobre o MVC e começaremos a escrever um pouco de código.

>[!div class="step-by-step"]
[Avançar](adding-controller.md)  
