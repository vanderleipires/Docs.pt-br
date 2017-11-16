---
title: "Introdução ao ASP.NET Core MVC no Mac, Linux ou Windows"
author: rick-anderson
description: "Introdução ao ASP.NET Core MVC e ao Visual Studio Code no Mac, Linux e Windows"
keywords: ASP.NET Core,MVC,VS Code,Visual Studio Code,Mac,Linux,Windows
ms.author: riande
manager: wpickett
ms.date: 07/07/2017
ms.topic: get-started-article
ms.assetid: 1d18b589-1638-4dc6-1638-fb0f41998d78
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: 87ce5dca011a7bc37d34799db159d933c158cba1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="getting-started-with-aspnet-core-mvc--on-mac-linux-or-windows"></a>Introdução ao ASP.NET Core MVC no Mac, Linux ou Windows

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Este tutorial ensinará os conceitos básicos da criação de um aplicativo Web ASP.NET Core MVC usando o [VS Code](https://code.visualstudio.com) (Visual Studio Code). O tutorial pressupõe que você já tenha familiaridade com o VS Code. Consulte [Introdução ao VS Code](https://code.visualstudio.com/docs) e [Ajuda do Visual Studio Code](#visual-studio-code-help) para obter mais informações. [!INCLUDE[consider RP](../../includes/razor.md)]

Há três versões deste tutorial:

* macOS: [Criar um aplicativo ASP.NET Core MVC com o Visual Studio para Mac](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows: [Criar um aplicativo ASP.NET Core MVC com o Visual Studio](xref:tutorials/first-mvc-app/start-mvc)
* macOS, Linux e Windows: [Criar um aplicativo ASP.NET Core MVC com o Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc) 

## <a name="install-vs-code-and-net-core"></a>Instalar o VS Code e o .NET Core

Este tutorial exige o [SDK do .NET Core 2.0.0](https://www.microsoft.com/net/core) ou posterior. Consulte [este pdf](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) para a versão ASP.NET Core 1.1.

Instale o seguinte:

* [SDK do .NET Core 2.0.0](https://www.microsoft.com/net/core) ou posterior.
* [Visual Studio Code](https://code.visualstudio.com)
* [Extensão C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) do VS Code 

## <a name="create-a-web-app-with-dotnet"></a>Criar um aplicativo Web com o dotnet

Em um terminal, execute os seguintes comandos:

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

Abra a pasta *MvcMovie* no Visual Studio Code (VS Code) e selecione o arquivo *Startup.cs*.

- Selecione **Sim** para a mensagem de **Aviso** – Os ativos necessários para compilar e depurar estão ausentes em “MvcMovie”. Deseja adicioná-los?”
- Selecione **Restaurar** para a mensagem **Informações** “Há dependências não resolvidas”.

![VS Code com o Aviso – Os ativos necessários para compilar e depurar estão ausentes em “MvcMovie”. Deseja adicioná-los? Não Perguntar Novamente, Agora Não, Sim e também Informações – há dependências não resolvidas – Restaurar – Fechar](../web-api-vsc/_static/vsc_restore.png)

Pressione **Depurar** (F5) para compilar e executar o programa.

![aplicativo em execução](../first-mvc-app/start-mvc/_static/1.png)

O VS Code inicia o servidor Web [Kestrel](xref:fundamentals/servers/kestrel) e executa o aplicativo. Observe que a barra de endereços mostra `localhost:5000` e não algo como `example.com`. Isso ocorre porque `localhost` é o nome do host padrão do computador local.

O modelo padrão fornece os links funcionais **Página Inicial, Sobre** e **Contato**. A imagem do navegador acima não mostra esses links. Dependendo do tamanho do navegador, talvez você precise clicar no ícone de navegação para mostrá-los.

![ícone de navegação na parte superior direita](../first-mvc-app/start-mvc/_static/2.png)

Na próxima parte deste tutorial, saberemos mais sobre o MVC e começaremos a escrever um pouco de código.

## <a name="visual-studio-code-help"></a>Ajuda do Visual Studio Code

- [Introdução](https://code.visualstudio.com/docs)
- [Depuração](https://code.visualstudio.com/docs/editor/debugging)
- [Terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [Atalhos de teclado](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [Atalhos de teclado do Mac](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [Atalhos de teclado do Linux](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [Atalhos de teclado do Windows](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

>[!div class="step-by-step"]
[Próximo – Adicionar um controlador](adding-controller.md)
