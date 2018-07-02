---
title: Introdução ao SignalR no ASP.NET Core
author: rachelappel
description: Neste tutorial, você deverá criar um aplicativo usando o SignalR para ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/22/2018
uid: tutorials/signalr
ms.openlocfilehash: 8762a4be1032d58014dd32dfdd3707197e14c6f9
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36297195"
---
# <a name="get-started-with-signalr-on-aspnet-core"></a>Introdução ao SignalR no ASP.NET Core

Por [Rachel Appel](https://twitter.com/rachelappel)

Este tutorial ensina as noções básicas para compilar um aplicativo em tempo real com SignalR para ASP.NET Core.

   ![Solução](signalr/_static/signalr-get-started-finished.png)

Este tutorial demonstra as seguintes tarefas de desenvolvimento SignalR:

> [!div class="checklist"]
> * Crie um SignalR no aplicativo Web do ASP.NET Core.
> * Crie um hub SignalR para efetuar push do conteúdo aos clientes.
> * Modifique a classe `Startup` e configure o aplicativo.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

# <a name="prerequisites"></a>Pré-requisitos

Instale o software a seguir:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [SDK do .NET Core 2.1 Core ou posterior](https://www.microsoft.com/net/download/all)
* [Visual Studio 2017](https://www.visualstudio.com/downloads/) versão 15.7 ou posterior com a carga de trabalho do **ASP.NET e desenvolvimento para a Web**
* [npm](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [SDK do .NET Core 2.1 Core ou posterior](https://www.microsoft.com/net/download/all)
* [Visual Studio Code](https://code.visualstudio.com/download)
* [C# para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [npm](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a>Criar um projeto do ASP.NET Core que hospede o cliente e o servidor SignalR

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

1. Use a opção de menu **Arquivo** > **Novo Projeto** e escolha **Aplicativo Web do ASP.NET Core**. Dê ao projeto o nome de *SignalRChat*.

   ![Caixa de diálogo Novo Projeto no Visual Studio](signalr/_static/signalr-new-project-dialog.png)

2. Selecione **Aplicativo Web** para criar um projeto usando o Razor Pages. Em seguida, selecione **OK**. Saiba que o **ASP.NET Core 2.1** é selecionado do seletor de estrutura, embora o SignalR seja executado em versões anteriores do .NET.

   ![Caixa de diálogo Novo Projeto no Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

O Visual Studio inclui o pacote `Microsoft.AspNetCore.SignalR` que contém suas bibliotecas do servidor como parte de seu modelo de **Aplicativo Web do ASP.NET Core**. No entanto, a biblioteca de cliente JavaScript para SignalR deve ser instalada usando *npm*.

3. Execute os seguintes comandos na janela **Console do Gerenciador de Pacotes**, na raiz do projeto:

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

4. Criar uma nova pasta chamada "signalr" dentro da pasta *lib* em seu projeto. Copiar o arquivo *signalr.js* de *node_modules\\@aspnet\signalr\dist\browser* para essa pasta.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

1. No **Terminal integrada**, execute o seguinte comando:

    ```console
    dotnet new webapp -o SignalRChat
    ```

    [!INCLUDE[](~/includes/webapp-alias-notice.md)]

2. Instale a biblioteca de cliente JavaScript usando *npm*.

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

3. Criar uma nova pasta chamada "signalr" dentro da pasta *lib* em seu projeto. Copiar o arquivo *signalr.js* de *node_modules\\@aspnet\signalr\dist\browser* para essa pasta.

---

## <a name="create-the-signalr-hub"></a>Criar o Hub SignalR

Um hub é uma classe que serve como um pipeline de alto nível que permite ao cliente e ao servidor chamar métodos um no outro.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

1. Adicione uma classe ao projeto escolhendo **Arquivo** > **Novo** > **Arquivo** e selecionando a **Classe Visual C#**. Dê ao arquivo o nome de *ChatHub*.

2. Herdar de `Microsoft.AspNetCore.SignalR.Hub`. A classe `Hub` contém propriedades e eventos para gerenciar conexões e grupos, bem como enviar e receber dados.

3. Crie o método `SendMessage` que envia uma mensagem a todos os clientes de chat conectados. Observe que ele retorna uma [Tarefa](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), pois o SignalR é assíncrono. O código assíncrono tem um dimensionamento melhor.

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

1. Abra a pasta *SignalRChat* no Visual Studio Code.

2. Adicione uma classe ao projeto selecionando **Arquivo** > **Novo Arquivo** no menu.

3. Herdar de `Microsoft.AspNetCore.SignalR.Hub`. A classe `Hub` contém propriedades e eventos para gerenciar conexões e grupos, bem como enviar e receber dados de clientes.

4. Adicione o método `SendMessage` à classe. O método `SendMessage` envia uma mensagem a todos os clientes de chat conectados. Observe que ele retorna uma [Tarefa](/dotnet/api/system.threading.tasks.task), pois o SignalR é assíncrono. O código assíncrono tem um dimensionamento melhor.

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs?range=6-12)]

-----

## <a name="configure-the-project-to-use-signalr"></a>Configurar o projeto para usar o SignalR

O servidor do SignalR deve ser configurado de moldo que saiba que pode passar solicitações para o SignalR.

1. Para configurar um projeto SignalR, modifique o método `Startup.ConfigureServices` do projeto.

   `services.AddSignalR` adiciona o SignalR como parte do pipeline [middleware](xref:fundamentals/middleware/index).

2. Configure rotas para seus hubs usando `UseSignalR`.

   [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=37,57-60)]

## <a name="create-the-signalr-client-code"></a>Criar o código de cliente SignalR

1. Adicione um arquivo JavaScript chamado *chat.js* à pasta *wwwroot\js*. Adicione o seguinte código a ele:

   [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

2. Substitua o conteúdo *Pages\Index.cshtml* pelo código a seguir:

   [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

   O HTML anterior exibe o nome e os campos de mensagem, além de um botão de envio. Observe as referências de script na parte inferior: uma referência para o SignalR e *chat.js*.

## <a name="run-the-app"></a>Executar o aplicativo

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Selecione **Depurar** > **Iniciar sem depuração** para iniciar um navegador e carregar o site localmente. Copie a URL da barra de endereços.

1. Abra outra instância de navegador (qualquer navegador) e cole a URL na barra de endereços.

1. Escolha um navegador, digite um nome e uma mensagem e clique no botão **Enviar**. O nome e a mensagem são exibidos em ambas as páginas instantaneamente.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Pressione **Depurar** (F5) para compilar e executar o programa. Executar o programa abre uma janela do navegador.

1. Abra outra janela do navegador e carregue o site localmente.

1. Escolha um navegador, digite um nome e uma mensagem e clique no botão **Enviar**. O nome e a mensagem são exibidos em ambas as páginas instantaneamente.

---

  ![Solução](signalr/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a>Recursos relacionados

[Introdução ao ASP.NET Core SignalR](xref:signalr/introduction)
