---
title: 'Tutorial: Introdução ao SignalR no ASP.NET Core'
author: tdykstra
description: Neste tutorial, você criará um aplicativo de chat que usa o SignalR para ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: tutorials/signalr
ms.openlocfilehash: 8581628b3f0cf878b8cc1d0684046d22a8374af9
ms.sourcegitcommit: c12ebdab65853f27fbb418204646baf6ce69515e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523214"
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

* [Visual Studio 2017 versão 15.8 ou posterior](https://www.visualstudio.com/downloads/) com a carga de trabalho **ASP.NET e desenvolvimento para a Web**
* [SDK do .NET Core 2.1 Core ou posterior](https://www.microsoft.com/net/download/all)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [Visual Studio Code](https://code.visualstudio.com/download)
* [SDK do .NET Core 2.1 Core ou posterior](https://www.microsoft.com/net/download/all)
* [C# para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

* [Visual Studio para Mac versão 7.5.4 ou posteriores](https://www.visualstudio.com/downloads/)
* [SDK do .NET Core 2.1 ou posteriores](https://www.microsoft.com/net/download/all) (incluído na instalação do Visual Studio)

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

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

* No menu, selecione **Arquivo > Nova Solução**.

* Selecione **.NET Core > Aplicativo > Aplicativo Web ASP.NET Core** (não selecione **Aplicativo Web ASP.NET Core (MVC)**).

* Selecione **Avançar**.

* Nomeie o projeto como *SignalRChat* e, em seguida, selecione **Criar**.

---

## <a name="add-the-signalr-client-library"></a>Adicionar a biblioteca de clientes do SignalR

A biblioteca do servidor SignalR está incluída no [Metapacote Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app). A biblioteca de clientes do JavaScript não é incluída automaticamente no projeto. No cenário deste tutorial, você usará o [Gerenciador de Bibliotecas (LibMan)](xref:client-side/libman/index) para baixar a biblioteca de clientes do *unpkg*. [unpkg](https://unpkg.com/#/) é uma [rede de distribuição de conteúdo](https://wikipedia.org/wiki/Content_delivery_network) que pode distribuir qualquer conteúdo do [npm, o gerenciador de pacotes do Node.js](https://www.npmjs.com/get-npm).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* No **Gerenciador de Soluções**, clique com o botão direito do mouse no projeto e selecione **Adicionar** > **Biblioteca do Lado do Cliente**.

* Na caixa de diálogo **Adicionar Biblioteca do Lado do Cliente**, para **Provedor**, selecione **unpkg**. 

* Para **Biblioteca**, insira _@aspnet/signalr@1_ e selecione a versão mais recente que não seja uma versão prévia.

  ![Caixa de diálogo Adicionar Biblioteca do Lado do Cliente – selecionar biblioteca](signalr/_static/libman1.png)

* Selecione **Escolher arquivos específicos**, expanda a pasta *distribuidor/navegador* e selecione *signalr.js* e *signalr.min.js*.

* Defina **Localização de Destino** como *wwwroot/lib/signalr/* e selecione **Instalar**.

  ![Caixa de diálogo Adicionar Biblioteca do Lado do Cliente – selecionar arquivos e destino](signalr/_static/libman2.png)

  O [LibMan](xref:client-side/libman/index) cria uma pasta *wwwroot/lib/signalr* e copia os arquivos selecionados para ela.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

* No **Terminal Integrado**, execute o seguinte comando para instalar o LibMan.

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* Navegue até a pasta do projeto, que inclui o arquivo *SignalRChat.csproj*.

* Execute o comando a seguir para obter a biblioteca de clientes SignalR usando LibMan. Talvez seja necessário aguardar alguns segundos antes de ver a saída.

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot\lib\signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  Os parâmetros especificam as seguintes opções:
  * Use o provedor unpkg.
  * Copie os arquivos para o destino *wwwroot/lib/signalr*.
  * Copie apenas os arquivos especificados.

  A saída tem a aparência do seguinte exemplo:

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot\lib\signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

* No **Terminal**, execute o comando a seguir para instalar o LibMan.

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* Navegue até a pasta do projeto, que inclui o arquivo *SignalRChat.csproj*.

* Execute o comando a seguir para obter a biblioteca de clientes SignalR usando LibMan.

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot\lib\signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  Os parâmetros especificam as seguintes opções:
  * Use o provedor unpkg.
  * Copie os arquivos para o destino *wwwroot/lib/signalr*.
  * Copie apenas os arquivos especificados.

  A saída tem a aparência do seguinte exemplo:

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot\lib\signalr"
  ```

---

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

* Substitua o conteúdo *Pages\Index.cshtml* pelo código a seguir:

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
