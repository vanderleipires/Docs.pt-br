---
title: Introdução ao ASP.NET Core MVC e ao Visual Studio
author: rick-anderson
description: Saiba como começar a usar o ASP.NET Core MVC e o Visual Studio.
ms.author: riande
ms.date: 10/07/2017
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 738c49272c2ae2b075866001f06ad09fe73969f9
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52862194"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio"></a>Introdução ao ASP.NET Core MVC e ao Visual Studio

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [consider RP](~/includes/razor.md)]

Há três versões deste tutorial:

* macOS: [Criar um aplicativo ASP.NET Core MVC com o Visual Studio para Mac](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows: [Como criar um aplicativo ASP.NET Core MVC com o Visual Studio](xref:tutorials/first-mvc-app/start-mvc)
* macOS, Linux e Windows: [Como criar um aplicativo ASP.NET Core MVC com o Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)

> [!NOTE]
> Estamos testando a usabilidade de uma nova estrutura proposta para o sumário do ASP.NET Core.  Se você tiver alguns minutos para experimentar um exercício de localização de sete tópicos diferentes no sumário atual ou proposto, [clique aqui para participar do estudo](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).

## <a name="install-visual-studio-and-net-core"></a>Como instalar o Visual Studio e o .NET Core

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-web-app"></a>Como criar um aplicativo Web

No Visual Studio, selecione **Arquivo > Novo > Projeto**.

![Arquivo > Novo > Projeto](start-mvc/_static/alt_new_project.png)

Complete a caixa de diálogo **Novo Projeto**:

* No painel esquerdo, toque em **.NET Core**
* No painel central, toque em **Aplicativo Web ASP.NET Core (.NET Core)**
* Nomeie o projeto "MvcMovie" (é importante nomear o projeto "MvcMovie" para que, quando você copiar o código, o namespace corresponda).
* Toque em **OK**

![Caixa de diálogo Novo projeto, .NET Core no painel esquerdo, Web do ASP.NET Core ](start-mvc/_static/new_project2-21.png)

Faça as configurações necessárias na caixa de diálogo **Novo aplicativo Web ASP.NET Core (.NET Core) – MvcMovie**:

* Na caixa suspensa do seletor de versão, selecione **ASP.NET Core 2.1**
* Selecione **Aplicativo Web (Model-View-Controller)**
* Toque em **OK**.

![Caixa de diálogo Novo projeto, .NET Core no painel esquerdo, Web do ASP.NET Core ](start-mvc/_static/new_project22-21.png)

O Visual Studio usou um modelo padrão para o projeto MVC que você acabou de criar. Para que o aplicativo comece a funcionar agora mesmo, digite um nome de projeto e selecione algumas opções. Este é um projeto inicial básico e é um bom ponto de partida.

Toque em **F5** para executar o aplicativo no modo de depuração ou **Ctrl-F5** para executá-lo no modo de não depuração.
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![aplicativo em execução](start-mvc/_static/1.png)

* O Visual Studio inicia o [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) e executa o aplicativo. Observe que a barra de endereços mostra `localhost:port#` e não algo como `example.com`. Isso ocorre porque `localhost` é o nome do host padrão do computador local. Quando o Visual Studio cria um projeto Web, uma porta aleatória é usada para o servidor Web. Na imagem acima, o número da porta é 5000. A URL no navegador mostra `localhost:5000`. Quando você executar o aplicativo, verá um número de porta diferente.
* Iniciar o aplicativo com **Ctrl+F5** (modo de não depuração) permite que você faça alterações de código, salve o arquivo, atualize o navegador e veja as alterações de código. Muitos desenvolvedores preferem usar modo de não depuração para iniciar o aplicativo e exibir alterações rapidamente.
* Você pode iniciar o aplicativo no modo de não depuração ou de depuração por meio do item de menu **Depurar**:

![Menu Depurar](start-mvc/_static/debug_menu.png)

* Você pode depurar o aplicativo tocando no botão **IIS Express**

![IIS Express](start-mvc/_static/iis_express.png)

O modelo padrão fornece os links funcionais **Página Inicial, Sobre** e **Contato**. A imagem do navegador acima não mostra esses links. Dependendo do tamanho do navegador, talvez você precise clicar no ícone de navegação para mostrá-los.

![ícone de navegação na parte superior direita](start-mvc/_static/2.png)

Se você estava executando no modo de depuração, toque em **Shift-F5** para interromper a depuração.

Na próxima parte deste tutorial, saberemos mais sobre o MVC e começaremos a escrever um pouco de código.

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-web-app"></a>Como criar um aplicativo Web

No Visual Studio, selecione **Arquivo > Novo > Projeto**.

![Arquivo > Novo > Projeto](start-mvc/_static/alt_new_project.png)

Complete a caixa de diálogo **Novo Projeto**:

* No painel esquerdo, toque em **.NET Core**
* No painel central, toque em **Aplicativo Web ASP.NET Core (.NET Core)**
* Nomeie o projeto "MvcMovie" (é importante nomear o projeto "MvcMovie" para que, quando você copiar o código, o namespace corresponda).
* Toque em **OK**

![Caixa de diálogo Novo projeto, .NET Core no painel esquerdo, Web do ASP.NET Core ](start-mvc/_static/new_project2.png)

Faça as configurações necessárias na caixa de diálogo **Novo aplicativo Web ASP.NET Core (.NET Core) – MvcMovie**:

* Na caixa de lista suspensa do seletor de versão, selecione **ASP.NET Core 2.-**
* Selecione **Aplicativo Web (Modelo-Exibir-Controlador)**
* Toque em **OK**.

![Caixa de diálogo Novo projeto, .NET Core no painel esquerdo, Web do ASP.NET Core ](start-mvc/_static/new_project22.png)

O Visual Studio usou um modelo padrão para o projeto MVC que você acabou de criar. Para que o aplicativo comece a funcionar agora mesmo, digite um nome de projeto e selecione algumas opções. Este é um projeto inicial básico e é um bom ponto de partida,

Toque em **F5** para executar o aplicativo no modo de depuração ou **Ctrl-F5** para executá-lo no modo de não depuração.
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![aplicativo em execução](start-mvc/_static/1.png)

* O Visual Studio inicia o [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) e executa o aplicativo. Observe que a barra de endereços mostra `localhost:port#` e não algo como `example.com`. Isso ocorre porque `localhost` é o nome do host padrão do computador local. Quando o Visual Studio cria um projeto Web, uma porta aleatória é usada para o servidor Web. Na imagem acima, o número da porta é 5000. A URL no navegador mostra `localhost:5000`. Quando você executar o aplicativo, verá um número de porta diferente.
* Iniciar o aplicativo com **Ctrl+F5** (modo de não depuração) permite que você faça alterações de código, salve o arquivo, atualize o navegador e veja as alterações de código. Muitos desenvolvedores preferem usar modo de não depuração para iniciar o aplicativo e exibir alterações rapidamente.
* Você pode iniciar o aplicativo no modo de não depuração ou de depuração por meio do item de menu **Depurar**:

![Menu Depurar](start-mvc/_static/debug_menu.png)

* Você pode depurar o aplicativo tocando no botão **IIS Express**

![IIS Express](start-mvc/_static/iis_express.png)

O modelo padrão fornece os links funcionais **Página Inicial, Sobre** e **Contato**. A imagem do navegador acima não mostra esses links. Dependendo do tamanho do navegador, talvez você precise clicar no ícone de navegação para mostrá-los.

![ícone de navegação na parte superior direita](start-mvc/_static/2.png)

Se você estava executando no modo de depuração, toque em **Shift-F5** para interromper a depuração.

Na próxima parte deste tutorial, saberemos mais sobre o MVC e começaremos a escrever um pouco de código.

::: moniker-end

> [!div class="step-by-step"]
> [Avançar](adding-controller.md)  
