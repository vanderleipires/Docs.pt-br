---
title: Introdução ao ASP.NET Core MVC no Mac, no Linux ou no Windows
author: rick-anderson
description: Saiba como começar a usar o ASP.NET Core MVC e o Visual Studio Code no Windows, no Linux e no macOS
manager: wpickett
ms.author: riande
ms.date: 07/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: 50fbd54c6b0cc1146271afda7e45a0dab590dd7d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30893554"
---
# <a name="introduction-to-aspnet-core-mvc-on-macos-linux-or-windows"></a>Introdução ao ASP.NET Core MVC no Mac, no Linux ou no Windows

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Este tutorial ensinará os conceitos básicos da criação de um aplicativo Web ASP.NET Core MVC usando o [VS Code](https://code.visualstudio.com) (Visual Studio Code). O tutorial pressupõe que você já tenha familiaridade com o VS Code. Consulte [Introdução ao VS Code](https://code.visualstudio.com/docs) e [Ajuda do Visual Studio Code](#visual-studio-code-help) para obter mais informações. 

[!INCLUDE [consider RP](../../includes/razor.md)]

Há três versões deste tutorial:

* macOS: [Criar um aplicativo ASP.NET Core MVC com o Visual Studio para Mac](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows: [Como criar um aplicativo ASP.NET Core MVC com o Visual Studio](xref:tutorials/first-mvc-app/start-mvc)
* macOS, Linux e Windows: [Como criar um aplicativo ASP.NET Core MVC com o Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc) 

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

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

  - [Atalhos de teclado do macOS](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [Atalhos de teclado do Linux](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [Atalhos de teclado do Windows](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

> [!div class="step-by-step"]
> [Próximo – Adicionar um controlador](adding-controller.md)
