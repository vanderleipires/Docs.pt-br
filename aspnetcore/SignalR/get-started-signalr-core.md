---
title: "Introdução ao SignalR no ASP.NET Core"
author: rachelappel
description: "Neste tutorial, você deve criar um aplicativo usando o SignalR para ASP.NET Core."
manager: wpickett
ms.author: rachelap
ms.custom: mvc
ms.date: 03/16/2018
ms.prod: aspnet-core
ms.topic: tutorial
ms.technology: aspnet
uid: signalr/get-started-signalr-core
ms.openlocfilehash: 139da5a2d0dadf51fece94b7c54ccd531e0ae8c2
ms.sourcegitcommit: 6548a3dd0cd1e3e92ac2310dee757ddad9fd6456
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/16/2018
---
# <a name="tutorial-get-started-with-signalr-for-aspnet-core"></a>Tutorial: Introdução ao SignalR para ASP.NET Core

Por [Rachel Appel](https://twitter.com/rachelappel)

Este tutorial ensina as Noções básicas de criação de um aplicativo em tempo real com SignalR para ASP.NET Core.

   ![Solução](get-started-signalr-core/_static/signalr-get-started-finished.png)

Este tutorial demonstra as seguintes tarefas de desenvolvimento SignalR:

> [!div class="checklist"]
> * Crie um SignalR no aplicativo web do ASP.NET Core.
> * Crie um hub SignalR para enviar por push o conteúdo aos clientes.
> * Modificar o `Startup` classe e configurar o aplicativo.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started-signalr-core/sample/) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

# <a name="prerequisites"></a>Pré-requisitos

Instale o software a seguir:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [.NET core 2.1.0 visualizar 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) ou posterior
* [Visual Studio de 2017](https://www.visualstudio.com/downloads/) versão 15.6 ou posterior com o **desenvolvimento ASP.NET e web** carga de trabalho
* [npm](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [.NET core 2.1.0 visualizar 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) ou posterior
* [Visual Studio Code](https://code.visualstudio.com/download) 
* [C# para o código do Visual Studio](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [npm](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a>Criar um projeto do ASP.NET Core que hospeda o servidor e cliente SignalR

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Use o **arquivo** > **novo projeto** menu opção e escolha **aplicativo Web do ASP.NET Core**. Nomeie o projeto *SignalRChat*.

  ![Caixa de diálogo Nova projeto no Visual Studio](get-started-signalr-core/_static/signalr-new-project-dialog.png)

2. Selecione **aplicativo Web** para criar um projeto usando as páginas Razor. Em seguida, selecione **Okey**. Certifique-se de que **ASP.NET Core 2.1** é selecionado do seletor do framework, embora o SignalR é executado em versões anteriores do .NET.

  ![Caixa de diálogo Nova projeto no Visual Studio](get-started-signalr-core/_static/signalr-new-project-choose-type.png)

3. Clique com botão direito no projeto no **Solution Explorer** > **adicionar** > **Novo Item** > **npm arquivo de configuração** . Nomeie o arquivo *Package. JSON*.

4. Execute o seguinte comando **Package Manager Console** janela, na raiz do projeto:

    ```console
      npm install @aspnet/signalr
    ```
5. Copiar o *signalr.js* arquivo *node_modules\\ @aspnet\signalr\dist\browser*  para o *wwwroot\lib* pasta em seu projeto.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Do **Terminal integrada**, execute o seguinte comando:
 
    ```console
      dotnet new razor -o SignalRChat
    ```

2. Instalar a biblioteca de cliente JavaScript usando *npm*.

    ```
      npm init -y
      npm install @aspnet/signalr
    ```

-----

## <a name="create-the-signalr-hub"></a>Criar o Hub SignalR

Um hub é uma classe que serve como um pipeline de alto nível que permite que o cliente e servidor chamar métodos em si.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Adicionar uma classe ao projeto, escolhendo **arquivo** > **novo** > **arquivo** e selecionando **Visual C# classe**.

1. Herdar de `Microsoft.AspNetCore.SignalR.Hub`. O `Hub` classe contém propriedades e eventos para gerenciar conexões e grupos, bem como enviar e receber dados.

1. Criar o `SendMessage` método que envia uma mensagem a todos os clientes conectados bate-papo. Observe que ele retorna um [tarefa](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task(v=vs.110).aspx), pois o SignalR é assíncrono. Será melhor dimensionado código assíncrono.

  [!code-csharp[Startup](get-started-signalr-core/sample/Hubs/ChatHub.cs?range=7-14)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Abra o *SignalRChat* pasta no código do Visual Studio.

1. Adicionar uma classe ao projeto selecionando **arquivo** > **novo arquivo** no menu.

1. Herdar de `Microsoft.AspNetCore.SignalR.Hub`. O `Hub` classe contém propriedades e eventos para gerenciar conexões e grupos, bem como enviar e receber dados aos clientes.

1. Adicione o método `SendMessage` à classe. O `SendMessage` método envia uma mensagem a todos os clientes conectados bate-papo. Observe que ele retorna um [tarefa](/dotnet/api/system.threading.tasks.task), pois o SignalR é assíncrono. Será melhor dimensionado código assíncrono.

  [!code-csharp[Startup](get-started-signalr-core/sample/Hubs/ChatHub.cs?range=7-14)]

-----

## <a name="configure-the-project-to-use-signalr"></a>Configurar o projeto para usar o SignalR

O servidor do SignalR deve ser configurado para que ele saiba que pode passar solicitações para o SignalR.

1. Para configurar um projeto SignalR, modifique o projeto `Startup.ConfigureServices` método.

  `services.AddSignalR` Adiciona o SignalR como parte do [middleware](xref:fundamentals/middleware/index) pipeline.

1. Configurar rotas para seus hubs usando `UseSignalR`.

  [!code-csharp[Startup](get-started-signalr-core/sample/Startup.cs?highlight=22,40-43)]

## <a name="create-the-signalr-client-code"></a>Criar o código de cliente SignalR

1. Substitua o conteúdo *Pages\Index.cshtml* com o código a seguir:

  [!code-cshtml[Index](get-started-signalr-core/sample/Pages/Index.cshtml)]

  O HTML anterior exibe o nome e os campos de mensagem e um botão de envio. Observe as referências de script na parte inferior: uma referência para o SignalR e *chat.js*.

1. Adicione um arquivo JavaScript, denominado *chat.js*, para o *wwwroot\js* pasta. Adicione o seguinte código a ele:

  [!code-javascript[Index](get-started-signalr-core/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a>Executar o aplicativo

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Selecione **depurar** > **iniciar sem depuração** para iniciar um navegador e carregar o site localmente. Copie a URL da barra de endereços.

1. Abrir outra instância de navegador (qualquer navegador) e cole a URL na barra de endereços.

1. Escolha um navegador, digite um nome e uma mensagem e clique no **enviar** botão. O nome e a mensagem são exibidos em ambas as páginas instantaneamente.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Pressione **Depurar** (F5) para compilar e executar o programa. Executar o programa abre uma janela do navegador.

1. Abrir outra janela do navegador e carregar o site localmente nele.

1. Escolha um navegador, digite um nome e uma mensagem e clique no **enviar** botão. O nome e a mensagem são exibidos em ambas as páginas instantaneamente.

-----

  ![Solução](get-started-signalr-core/_static/signalr-get-started-finished.png)