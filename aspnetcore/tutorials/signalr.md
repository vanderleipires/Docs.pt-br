---
title: 'Tutorial: Introdução ao SignalR no ASP.NET Core'
author: tdykstra
description: Neste tutorial, você criará um aplicativo de chat que usa o SignalR para ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/20/2018
uid: tutorials/signalr
ms.openlocfilehash: a2573e2817a2d8921954264ca17bc3a7e2a010a8
ms.sourcegitcommit: 847cc1de5526ff42a7303491e6336c2dbdb45de4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43055826"
---
# <a name="tutorial-get-started-with-signalr-on-aspnet-core"></a>Tutorial: Introdução ao SignalR no ASP.NET Core

Este tutorial ensina as noções básicas da criação de um aplicativo em tempo real usando o SignalR. Você aprenderá como:

> [!div class="checklist"]
> * Criar um aplicativo Web que usa o SignalR no ASP.NET Core.
> * Criar um hub do SignalR no servidor.
> * Conectar-se ao hub do SignalR de clientes JavaScript.
> * Usar o hub para enviar mensagens de qualquer cliente a todos os clientes conectados.

No final, você terá um aplicativo de chat funcionando:

![Aplicativo de exemplo do SignalR](signalr/_static/signalr-get-started-finished.png)

[Exibir ou baixar um código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample)).

## <a name="prerequisites"></a>Pré-requisitos

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [Visual Studio 2017 versão 15.7.3 ou posteriores](https://www.visualstudio.com/downloads/) com a carga de trabalho **ASP.NET e desenvolvimento para a Web**
* [SDK do .NET Core 2.1 Core ou posterior](https://www.microsoft.com/net/download/all)
* [npm](https://www.npmjs.com/get-npm) (gerenciador de pacotes do Node.js, usado para a biblioteca de clientes JavaScript do SignalR.)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [Visual Studio Code](https://code.visualstudio.com/download)
* [SDK do .NET Core 2.1 Core ou posterior](https://www.microsoft.com/net/download/all)
* [C# para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [npm](https://www.npmjs.com/get-npm) (gerenciador de pacotes do Node.js, usado para a biblioteca de clientes JavaScript do SignalR.)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

* [Visual Studio para Mac versão 7.5.4 ou posteriores](https://www.visualstudio.com/downloads/)
* [SDK do .NET Core 2.1 ou posteriores](https://www.microsoft.com/net/download/all) (incluído na instalação do Visual Studio)
* [npm](https://www.npmjs.com/get-npm) (gerenciador de pacotes do Node.js, usado para a biblioteca de clientes JavaScript do SignalR.)

---

## <a name="create-the-project"></a>Criar o projeto

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* No menu, selecione **Arquivo > Novo Projeto**.

* Na caixa de diálogo **Novo Projeto**, selecione **Instalado > Visual C# > Web > Aplicativo Web ASP.NET Core**. Dê ao projeto o nome de *SignalRChat*.

  ![Caixa de diálogo Novo Projeto no Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* Selecione **Aplicativo Web** para criar um projeto que usa Razor Pages.

* Selecione uma estrutura de destino do **.NET Core**, selecione **ASP.NET Core 2.1** e clique em **OK**.

  ![Caixa de diálogo Novo Projeto no Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

* Abra uma pasta que você possa usar para um novo projeto.

* No **Terminal Integrado**, execute o seguinte comando:

   ```console
   dotnet new webapp -o SignalRChat
   ```

   [!INCLUDE[](~/includes/webapp-alias-notice.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

* No menu, selecione **Arquivo > Nova Solução**.

* Selecione **.NET Core > Aplicativo > Aplicativo Web ASP.NET Core** (não selecione **Aplicativo Web ASP.NET Core (MVC)**).

* Selecione **Avançar**.

* Nomeie o projeto como *SignalRChat* e, em seguida, selecione **Criar**.

---

## <a name="add-the-signalr-client-library"></a>Adicionar a biblioteca de clientes do SignalR

A biblioteca do servidor SignalR está incluída no [Metapacote Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app). Mas você precisa obter a biblioteca de clientes JavaScript do [npm, ou seja, o gerenciador de pacotes do Node.js](https://www.npmjs.com/get-npm).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* No **PMC** (console do gerenciador de pacotes), mude para a pasta de projeto (aquela que contém o arquivo *SignalRChat.csproj*).

  ```console
  cd SignalRChat
  ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

2. Mude para a nova pasta de projeto.

  ```console
  cd SignalRChat
  ``` 

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

* No **Terminal**, navegue até a pasta de projeto (aquela que contém o arquivo *SignalRChat.csproj*).

---

* Execute o inicializador do npm para criar um arquivo *package.json*:

  ```console
  npm init -y
  ```

  O comando cria uma saída semelhante ao exemplo a seguir:

  ```console
  Wrote to C:\tmp\SignalRChat\package.json:
  {
    "name": "SignalRChat",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1"
    },
    "keywords": [],
    "author": "",
    "license": "ISC"0
  }
  ```

* Instale o pacote da biblioteca de clientes:

  ```console
  npm install @aspnet/signalr
  ```

  O comando cria uma saída semelhante ao exemplo a seguir:

  ```
  npm notice created a lockfile as package-lock.json. You should commit this file.
  npm WARN signalrchat@1.0.0 No description
  npm WARN signalrchat@1.0.0 No repository field.

  + @aspnet/signalr@1.0.2
  added 1 package in 0.98s
  ```

O comando `npm install` baixou a biblioteca de clientes JavaScript em uma subpasta em *node_modules*. Copie-o de lá para uma pasta em *wwwroot* que possa ser referenciada da página da Web do aplicativo de chat.

* Crie uma pasta *signalr* em *wwwroot/lib*.

* Copie o arquivo *signalr.js* de *node_modules/@aspnet/signalr/dist/browser* para a nova pasta *wwwroot/lib/signalr*.

## <a name="create-the-signalr-hub"></a>Criar o hub do SignalR

Um [hub](xref:signalr/hubs) é uma classe que funciona como um pipeline de alto nível que lida com a comunicação entre cliente e servidor.

* Na pasta do projeto SignalRChat, crie uma pasta *Hubs*.

* Na pasta *Hubs*, crie um arquivo *ChatHub.cs* com o código a seguir:

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  A classe `ChatHub` é herdada da classe [Hub](/dotnet/api/microsoft.aspnetcore.signalr.hub) do SignalR. A classe `Hub` gerencia conexões, grupos e sistemas de mensagens.

  O método `SendMessage` pode ser chamado por qualquer cliente conectado. Ele envia a mensagem recebida para todos os clientes. O código do SignalR é assíncrono para fornecer o máximo de escalabilidade.

## <a name="configure-the-project-to-use-signalr"></a>Configurar o projeto para usar o SignalR

O servidor do SignalR precisa ser configurado para passar solicitações do SignalR ao SignalR.

* Adicione o seguinte código realçado ao arquivo *Startup.cs*.

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  Essas alterações adicionam o SignalR ao sistema de [injeção de dependência](xref:fundamentals/dependency-injection) e ao pipeline do [middleware](xref:fundamentals/middleware/index).

## <a name="create-the-signalr-client-code"></a>Criar o código de cliente SignalR

* Substitua o conteúdo em *Pages\Index.cshtml* pelo seguinte:

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  O código anterior:

  * Cria as caixas de texto para o nome e a mensagem de texto e um botão Enviar.
  * Cria uma lista com `id="messagesList"` para exibir as mensagens recebidas do hub do SignalR.
  * Inclui referências de script ao SignalR e ao código do aplicativo *chat.js* que você criará na próxima etapa.

* Na pasta *wwwroot/js*, crie um arquivo *chat.js* com o código a seguir:

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  O código anterior:

  * Cria e inicia uma conexão.
  * Adiciona no botão Enviar um manipulador que envia mensagens ao hub.
  * Adiciona no objeto de conexão um manipulador que recebe mensagens do hub e as adiciona à lista.

## <a name="run-the-app"></a>Executar o aplicativo

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Pressione **CTRL + F5** para executar o aplicativo sem depuração.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Pressione **CTRL + F5** para executar o aplicativo sem depuração.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

* No menu, selecione **Executar > Iniciar sem Depuração**.

---

* Copie a URL da barra de endereços, abra outra instância ou guia do navegador e cole a URL na barra de endereços.

* Escolha qualquer navegador, insira um nome e uma mensagem e selecione o botão **Enviar**.

  O nome e a mensagem são exibidos em ambas as páginas instantaneamente.

  ![Aplicativo de exemplo do SignalR](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> Se o aplicativo não funcionar, abra as ferramentas para desenvolvedores do navegador (F12) e acesse o console. Você pode encontrar erros relacionados ao código HTML e JavaScript. Por exemplo, suponha que você coloque *signalr.js* em uma pasta diferente daquela direcionada. Nesse caso, a referência a esse arquivo não funcionará e ocorrerá um erro 404 no console.
> ![Erro de signalr.js não encontrado](signalr/_static/f12-console.png)

## <a name="next-steps"></a>Próximas etapas

Se você quiser que os clientes se conectem a um aplicativo do SignalR de domínios diferentes, habilite o CORS (Compartilhamento de recursos entre origens). Para obter mais informações, confira [Compartilhamento de recursos entre origens](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing).

Para saber mais sobre o SignalR, os hubs e os clientes JavaScript, confira estes recursos:

* [Introdução ao SignalR para ASP.NET Core](xref:signalr/introduction)
* [Usar hubs no SignalR do ASP.NET Core](xref:signalr/hubs)
* [Cliente JavaScript do SignalR do ASP.NET Core](xref:signalr/javascript-client)
