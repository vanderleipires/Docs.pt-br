---
title: "Introdução ao SignalR no ASP.NET Core"
author: rachelappel
description: "Conheça os fundamentos da criação de um aplicativo em tempo real com SignalR para ASP.NET Core."
manager: wpickett
ms.author: rachelap
ms.custom: mvc
ms.date: 03/06/2018
ms.prod: aspnet-core
ms.technology: dotnet-signalr
ms.topic: tutorial
uid: signalr/get-started-signalr-core
ms.openlocfilehash: 79af59fc8c2ada71d764ada95a431e10f4f00f27
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/15/2018
---
# <a name="tutorial-get-started-with-signalr-for-aspnet-core"></a>Tutorial: Introdução ao SignalR para ASP.NET Core

Por [Rachel Appel](https://twitter.com/rachelappel)

Este tutorial ensina as Noções básicas de criação de um aplicativo em tempo real com SignalR para ASP.NET Core.

   ![Solução](get-started-signalr-core/_static/signalr-get-started-finished.png)

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started-signalr-core/sample/) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

Este tutorial demonstra as seguintes tarefas de desenvolvimento SignalR:

> [!div class="checklist"]
> * Crie um aplicativo web do ASP.NET Core.
> * Crie um hub SignalR para enviar por push o conteúdo aos clientes.
> * Use a biblioteca de SignalR JavaScript para enviar mensagens e exibir atualizações do hub.

## <a name="prerequisites"></a>Pré-requisitos

Instale o software a seguir:

* [.NET core 2.1.0 visualizar 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) ou posterior
* [Visual Studio de 2017](https://www.visualstudio.com/downloads/) versão 15.6 ou posterior com a carga de trabalho de desenvolvimento do ASP.NET e web
* [npm](https://www.npmjs.com/get-npm)

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a>Criar um projeto do ASP.NET Core que hospeda o servidor e cliente SignalR

1. Use o **arquivo** > **novo projeto** menu opção e escolha **aplicativo Web do ASP.NET Core**. Nomeie o projeto `SignalRChat`.

  ![Caixa de diálogo Nova projeto no Visual Studio](get-started-signalr-core/_static/signalr-new-project-dialog.png)

2. Selecione **aplicativo Web** para criar um projeto usando as páginas Razor. Em seguida, selecione **Okey**. Certifique-se de que **ASP.NET Core 2.1** é selecionado do seletor do framework, embora o SignalR é executado em versões anteriores do .NET.

  ![Caixa de diálogo Nova projeto no Visual Studio](get-started-signalr-core/_static/signalr-new-project-choose-type.png)

  As bibliotecas que hospedam o código do lado do servidor SignalR são incluídas no modelo de projeto. Instalar o JavaScript do lado do cliente separadamente com [npm](https://www.npmjs.com/).

  ```console
   npm install @aspnet/signalr
  ```

3. Copie o *signalr.js* de *node_modules\\ @aspnet\signalr\dist\browser*  para o *wwwroot\lib* pasta em seu projeto.

## <a name="create-the-signalr-hub"></a>Criar o Hub SignalR

Um hub é uma classe que serve como um pipeline de alto nível que permite que o cliente e servidor chamar métodos em si.

1. Adicionar uma classe ao projeto, escolhendo **arquivo** > **novo** > **arquivo** e selecionando **Visual C# classe**. 

1. Herdar de `Microsoft.AspNetCore.SignalR.Hub`. O `Hub` classe contém propriedades e eventos para gerenciar conexões e grupos, bem como enviar e receber dados.

1. Criar o `Send` método que envia uma mensagem a todos os clientes conectados bate-papo. Observe que ele retorna um `Task`, pois o SignalR é assíncrono. Será melhor dimensionado código assíncrono.

  [!code-csharp[Startup](get-started-signalr-core/sample/Hubs/ChatHub.cs?range=7-14)]

## <a name="configure-the-project-to-use-signalr"></a>Configurar o projeto para usar o SignalR

O servidor do SignalR deve ser configurado para que ele saiba que pode passar solicitações para o SignalR.

1. Para configurar um projeto SignalR, modifique o `ConfigureServices` método do aplicativo `Startup` classe inserindo uma chamada para `services.AddSignalR`.

  `services.AddSignalR` Adiciona o SignalR como parte do [middleware ASP.NET Core](xref:fundamentals/middleware/index) pipeline.

1. Configurar rotas para seus hubs usando `UseSignalR`.

  [!code-csharp[Startup](get-started-signalr-core/sample/Startup.cs?highlight=22,40-43)]

## <a name="create-the-signalr-client-code"></a>Criar o código de cliente SignalR

1. Substitua o conteúdo *Pages\Index.cshtml* com o código a seguir:

  [!code-cshtml[Index](get-started-signalr-core/sample/Pages/Index.cshtml)]

  O HTML anterior exibe o nome e os campos de mensagem e um botão de envio. Observe as referências de script na parte inferior: uma referência para o SignalR e *chat.js*.

1. Adicionar um arquivo JavaScript para o *wwwroot\js* pasta denominada *chat.js* e adicione o seguinte código para ele:

  [!code-javascript[Index](get-started-signalr-core/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a>Executar o aplicativo

1. Selecione **depurar** > **iniciar sem depuração** para iniciar um navegador e carregar o site localmente. Copie a URL da barra de endereços.

1. Abrir outra instância de navegador (qualquer navegador) e cole a URL na barra de endereços.

1. Escolha um navegador, digite um nome e uma mensagem e clique no **enviar** botão. O nome e a mensagem são exibidos em ambas as páginas instantaneamente.

  ![Solução](get-started-signalr-core/_static/signalr-get-started-finished.png)
