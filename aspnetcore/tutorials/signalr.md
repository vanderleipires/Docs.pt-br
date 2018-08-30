---
title: 'Tutorial: Introdução ao SignalR no ASP.NET Core'
author: tdykstra
description: Neste tutorial, você criará um aplicativo de chat que usa o SignalR para ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/20/2018
uid: tutorials/signalr
ms.openlocfilehash: db7f31963f6a4280069f1f4f82a547e2879e64bb
ms.sourcegitcommit: d27317c16f113e7c111583042ec7e4c5a26adf6f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/20/2018
ms.locfileid: "41751705"
---
# <a name="tutorial-get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="0a1ec-103">Tutorial: Introdução ao SignalR no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0a1ec-103">Tutorial: Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="0a1ec-104">Este tutorial ensina as noções básicas da criação de um aplicativo em tempo real usando o SignalR.</span><span class="sxs-lookup"><span data-stu-id="0a1ec-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="0a1ec-105">Você aprenderá como:</span><span class="sxs-lookup"><span data-stu-id="0a1ec-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0a1ec-106">Criar um aplicativo Web que usa o SignalR no ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0a1ec-106">Create a web app that uses SignalR on ASP.NET Core.</span></span>
> * <span data-ttu-id="0a1ec-107">Criar um hub do SignalR no servidor.</span><span class="sxs-lookup"><span data-stu-id="0a1ec-107">Create a SignalR hub on the server.</span></span>
> * <span data-ttu-id="0a1ec-108">Conectar-se ao hub do SignalR de clientes JavaScript.</span><span class="sxs-lookup"><span data-stu-id="0a1ec-108">Connect to the SignalR hub from JavaScript clients.</span></span>
> * <span data-ttu-id="0a1ec-109">Usar o hub para enviar mensagens de qualquer cliente a todos os clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="0a1ec-109">Use the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="0a1ec-110">No final, você terá um aplicativo de chat funcionando:</span><span class="sxs-lookup"><span data-stu-id="0a1ec-110">At the end, you'll have a working chat app:</span></span>

![Aplicativo de exemplo do SignalR](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="0a1ec-112">[Exibir ou baixar um código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="0a1ec-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0a1ec-113">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="0a1ec-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0a1ec-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0a1ec-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0a1ec-115">[Visual Studio 2017 versão 15.7.3 ou posteriores](https://www.visualstudio.com/downloads/) com a carga de trabalho **ASP.NET e desenvolvimento para a Web**</span><span class="sxs-lookup"><span data-stu-id="0a1ec-115">[Visual Studio 2017 version 15.7.3 or later](https://www.visualstudio.com/downloads/) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="0a1ec-116">SDK do .NET Core 2.1 Core ou posterior</span><span class="sxs-lookup"><span data-stu-id="0a1ec-116">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="0a1ec-117">[npm](https://www.npmjs.com/get-npm) (gerenciador de pacotes do Node.js, usado para a biblioteca de clientes JavaScript do SignalR.)</span><span class="sxs-lookup"><span data-stu-id="0a1ec-117">[npm](https://www.npmjs.com/get-npm) (Package manager for Node.js, used for the SignalR JavaScript client library.)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0a1ec-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0a1ec-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="0a1ec-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0a1ec-119">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="0a1ec-120">SDK do .NET Core 2.1 Core ou posterior</span><span class="sxs-lookup"><span data-stu-id="0a1ec-120">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="0a1ec-121">C# para Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0a1ec-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* <span data-ttu-id="0a1ec-122">[npm](https://www.npmjs.com/get-npm) (gerenciador de pacotes do Node.js, usado para a biblioteca de clientes JavaScript do SignalR.)</span><span class="sxs-lookup"><span data-stu-id="0a1ec-122">[npm](https://www.npmjs.com/get-npm) (Package manager for Node.js, used for the SignalR JavaScript client library.)</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0a1ec-123">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="0a1ec-123">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="0a1ec-124">Visual Studio para Mac versão 7.5.4 ou posteriores</span><span class="sxs-lookup"><span data-stu-id="0a1ec-124">Visual Studio for Mac version 7.5.4 or later</span></span>](https://www.visualstudio.com/downloads/)
* <span data-ttu-id="0a1ec-125">[SDK do .NET Core 2.1 ou posteriores](https://www.microsoft.com/net/download/all) (incluído na instalação do Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="0a1ec-125">[.NET Core SDK 2.1 or later](https://www.microsoft.com/net/download/all) (included in the Visual Studio install)</span></span>
* <span data-ttu-id="0a1ec-126">[npm](https://www.npmjs.com/get-npm) (gerenciador de pacotes do Node.js, usado para a biblioteca de clientes JavaScript do SignalR.)</span><span class="sxs-lookup"><span data-stu-id="0a1ec-126">[npm](https://www.npmjs.com/get-npm) (Package manager for Node.js, used for the SignalR JavaScript client library.)</span></span>

---

## <a name="create-the-project"></a><span data-ttu-id="0a1ec-127">Criar o projeto</span><span class="sxs-lookup"><span data-stu-id="0a1ec-127">Create the project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0a1ec-128">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0a1ec-128">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="0a1ec-129">No menu, selecione **Arquivo > Novo Projeto**.</span><span class="sxs-lookup"><span data-stu-id="0a1ec-129">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="0a1ec-130">Na caixa de diálogo **Novo Projeto**, selecione **Instalado > Visual C# > Web > Aplicativo Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="0a1ec-130">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="0a1ec-131">Dê ao projeto o nome de *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="0a1ec-131">Name the project *SignalRChat*.</span></span>

  ![Caixa de diálogo Novo Projeto no Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="0a1ec-133">Selecione **Aplicativo Web** para criar um projeto que usa Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="0a1ec-133">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="0a1ec-134">Selecione uma estrutura de destino do **.NET Core**, selecione **ASP.NET Core 2.1** e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="0a1ec-134">Select a target framework of **.NET Core**, select **ASP.NET Core 2.1**, and click **OK**.</span></span>

  ![Caixa de diálogo Novo Projeto no Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0a1ec-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0a1ec-136">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="0a1ec-137">Abra uma pasta que você possa usar para um novo projeto.</span><span class="sxs-lookup"><span data-stu-id="0a1ec-137">Open a folder that you can use for a new project.</span></span>

* <span data-ttu-id="0a1ec-138">No **Terminal Integrado**, execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="0a1ec-138">In the **Integrated Terminal**, run the following command:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   ```

   [!INCLUDE[](~/includes/webapp-alias-notice.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0a1ec-139">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="0a1ec-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="0a1ec-140">No menu, selecione **Arquivo > Nova Solução**.</span><span class="sxs-lookup"><span data-stu-id="0a1ec-140">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="0a1ec-141">Selecione **.NET Core > Aplicativo > Aplicativo Web ASP.NET Core** (não selecione **Aplicativo Web ASP.NET Core (MVC)**).</span><span class="sxs-lookup"><span data-stu-id="0a1ec-141">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="0a1ec-142">Selecione **Avançar**.</span><span class="sxs-lookup"><span data-stu-id="0a1ec-142">Select **Next**.</span></span>

* <span data-ttu-id="0a1ec-143">Nomeie o projeto como *SignalRChat* e, em seguida, selecione **Criar**.</span><span class="sxs-lookup"><span data-stu-id="0a1ec-143">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="0a1ec-144">Adicionar a biblioteca de clientes do SignalR</span><span class="sxs-lookup"><span data-stu-id="0a1ec-144">Add the SignalR client library</span></span>

<span data-ttu-id="0a1ec-145">A biblioteca do servidor SignalR está incluída no [Metapacote Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="0a1ec-145">The SignalR server library is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="0a1ec-146">Mas você precisa obter a biblioteca de clientes JavaScript do npm, ou seja, o gerenciador de pacotes do Node.js.</span><span class="sxs-lookup"><span data-stu-id="0a1ec-146">But you have to get the JavaScript client library from npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0a1ec-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0a1ec-147">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="0a1ec-148">No **PMC** (console do gerenciador de pacotes), mude para a pasta de projeto (aquela que contém o arquivo *SignalRChat.csproj*).</span><span class="sxs-lookup"><span data-stu-id="0a1ec-148">In **Package Manager Console** (PMC), change to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

  ```console
  cd SignalRChat
  ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0a1ec-149">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0a1ec-149">Visual Studio Code</span></span>](#tab/visual-studio-code/)

2. <span data-ttu-id="0a1ec-150">Mude para a nova pasta de projeto.</span><span class="sxs-lookup"><span data-stu-id="0a1ec-150">Change to the new project folder.</span></span>

  ```console
  cd SignalRChat
  ``` 

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0a1ec-151">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="0a1ec-151">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="0a1ec-152">No **Terminal**, navegue até a pasta de projeto (aquela que contém o arquivo *SignalRChat.csproj*).</span><span class="sxs-lookup"><span data-stu-id="0a1ec-152">In the **Terminal**, navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

---

* <span data-ttu-id="0a1ec-153">Execute o inicializador do npm para criar um arquivo *package.json*:</span><span class="sxs-lookup"><span data-stu-id="0a1ec-153">Run the npm initializer to create a *package.json* file:</span></span>

  ```console
  npm init -y
  ```

  <span data-ttu-id="0a1ec-154">O comando cria uma saída semelhante ao exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="0a1ec-154">The command creates output similar to the following example:</span></span>

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

* <span data-ttu-id="0a1ec-155">Instale o pacote da biblioteca de clientes:</span><span class="sxs-lookup"><span data-stu-id="0a1ec-155">Install the client library package:</span></span>

  ```console
  npm install @aspnet/signalr
  ```

  <span data-ttu-id="0a1ec-156">O comando cria uma saída semelhante ao exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="0a1ec-156">The command creates output similar to the following example:</span></span>

  ```
  npm notice created a lockfile as package-lock.json. You should commit this file.
  npm WARN signalrchat@1.0.0 No description
  npm WARN signalrchat@1.0.0 No repository field.

  + @aspnet/signalr@1.0.2
  added 1 package in 0.98s
  ```

<span data-ttu-id="0a1ec-157">O comando `npm install` baixou a biblioteca de clientes JavaScript em uma subpasta em *node_modules*.</span><span class="sxs-lookup"><span data-stu-id="0a1ec-157">The `npm install` command downloaded the JavaScript client library to a subfolder under *node_modules*.</span></span> <span data-ttu-id="0a1ec-158">Copie-o de lá para uma pasta em *wwwroot* que possa ser referenciada da página da Web do aplicativo de chat.</span><span class="sxs-lookup"><span data-stu-id="0a1ec-158">Copy it from there to a folder under *wwwroot* that you can reference from the chat app web page.</span></span>

* <span data-ttu-id="0a1ec-159">Crie uma pasta *signalr* em *wwwroot/lib*.</span><span class="sxs-lookup"><span data-stu-id="0a1ec-159">Create a *signalr* folder in *wwwroot/lib*.</span></span>

* <span data-ttu-id="0a1ec-160">Copie o arquivo *signalr.js* de *node_modules/@aspnet/signalr/dist/browser* para a nova pasta *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="0a1ec-160">Copy the *signalr.js* file from *node_modules/@aspnet/signalr/dist/browser* to the new *wwwroot/lib/signalr* folder.</span></span>

## <a name="create-the-signalr-hub"></a><span data-ttu-id="0a1ec-161">Criar o hub do SignalR</span><span class="sxs-lookup"><span data-stu-id="0a1ec-161">Create the SignalR hub</span></span>

<span data-ttu-id="0a1ec-162">Um [hub](xref:signalr/hubs) é uma classe que funciona como um pipeline de alto nível que lida com a comunicação entre cliente e servidor.</span><span class="sxs-lookup"><span data-stu-id="0a1ec-162">A [hub](xref:signalr/hubs) is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="0a1ec-163">Na pasta do projeto SignalRChat, crie uma pasta *Hubs*.</span><span class="sxs-lookup"><span data-stu-id="0a1ec-163">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="0a1ec-164">Na pasta *Hubs*, crie um arquivo *ChatHub.cs* com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="0a1ec-164">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="0a1ec-165">A classe `ChatHub` é herdada da classe [Hub](/dotnet/api/microsoft.aspnetcore.signalr.hub) do SignalR.</span><span class="sxs-lookup"><span data-stu-id="0a1ec-165">The `ChatHub` class inherits from the SignalR [Hub](/dotnet/api/microsoft.aspnetcore.signalr.hub) class.</span></span> <span data-ttu-id="0a1ec-166">A classe `Hub` gerencia conexões, grupos e sistemas de mensagens.</span><span class="sxs-lookup"><span data-stu-id="0a1ec-166">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="0a1ec-167">O método `SendMessage` pode ser chamado por qualquer cliente conectado.</span><span class="sxs-lookup"><span data-stu-id="0a1ec-167">The `SendMessage` method can be called by any connected client.</span></span> <span data-ttu-id="0a1ec-168">Ele envia a mensagem recebida para todos os clientes.</span><span class="sxs-lookup"><span data-stu-id="0a1ec-168">It sends the received message to all clients.</span></span> <span data-ttu-id="0a1ec-169">O código do SignalR é assíncrono para fornecer o máximo de escalabilidade.</span><span class="sxs-lookup"><span data-stu-id="0a1ec-169">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="0a1ec-170">Configurar o projeto para usar o SignalR</span><span class="sxs-lookup"><span data-stu-id="0a1ec-170">Configure the project to use SignalR</span></span>

<span data-ttu-id="0a1ec-171">O servidor do SignalR precisa ser configurado para passar solicitações do SignalR ao SignalR.</span><span class="sxs-lookup"><span data-stu-id="0a1ec-171">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="0a1ec-172">Adicione o seguinte código realçado ao arquivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="0a1ec-172">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="0a1ec-173">Essas alterações adicionam o SignalR ao sistema de [injeção de dependência](xref:fundamentals/dependency-injection) e ao pipeline do [middleware](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="0a1ec-173">These changes add SignalR to the [dependency injection](xref:fundamentals/dependency-injection) system and the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="0a1ec-174">Criar o código de cliente SignalR</span><span class="sxs-lookup"><span data-stu-id="0a1ec-174">Create the SignalR client code</span></span>

* <span data-ttu-id="0a1ec-175">Substitua o conteúdo em *Pages\Index.cshtml* pelo seguinte:</span><span class="sxs-lookup"><span data-stu-id="0a1ec-175">Replace the content in *Pages\Index.cshtml* with the following:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="0a1ec-176">O código anterior:</span><span class="sxs-lookup"><span data-stu-id="0a1ec-176">The preceding code:</span></span>

  * <span data-ttu-id="0a1ec-177">Cria as caixas de texto para o nome e a mensagem de texto e um botão Enviar.</span><span class="sxs-lookup"><span data-stu-id="0a1ec-177">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="0a1ec-178">Cria uma lista com `id="messagesList"` para exibir as mensagens recebidas do hub do SignalR.</span><span class="sxs-lookup"><span data-stu-id="0a1ec-178">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="0a1ec-179">Inclui referências de script ao SignalR e ao código do aplicativo *chat.js* que você criará na próxima etapa.</span><span class="sxs-lookup"><span data-stu-id="0a1ec-179">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="0a1ec-180">Na pasta *wwwroot/js*, crie um arquivo *chat.js* com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="0a1ec-180">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="0a1ec-181">O código anterior:</span><span class="sxs-lookup"><span data-stu-id="0a1ec-181">The preceding code:</span></span>

  * <span data-ttu-id="0a1ec-182">Cria e inicia uma conexão.</span><span class="sxs-lookup"><span data-stu-id="0a1ec-182">Creates and starts a connection.</span></span>
  * <span data-ttu-id="0a1ec-183">Adiciona no botão Enviar um manipulador que envia mensagens ao hub.</span><span class="sxs-lookup"><span data-stu-id="0a1ec-183">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="0a1ec-184">Adiciona no objeto de conexão um manipulador que recebe mensagens do hub e as adiciona à lista.</span><span class="sxs-lookup"><span data-stu-id="0a1ec-184">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="0a1ec-185">Executar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="0a1ec-185">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0a1ec-186">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0a1ec-186">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0a1ec-187">Pressione **CTRL + F5** para executar o aplicativo sem depuração.</span><span class="sxs-lookup"><span data-stu-id="0a1ec-187">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0a1ec-188">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0a1ec-188">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="0a1ec-189">Pressione **CTRL + F5** para executar o aplicativo sem depuração.</span><span class="sxs-lookup"><span data-stu-id="0a1ec-189">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0a1ec-190">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="0a1ec-190">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="0a1ec-191">No menu, selecione **Executar > Iniciar sem Depuração**.</span><span class="sxs-lookup"><span data-stu-id="0a1ec-191">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="0a1ec-192">Copie a URL da barra de endereços, abra outra instância ou guia do navegador e cole a URL na barra de endereços.</span><span class="sxs-lookup"><span data-stu-id="0a1ec-192">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="0a1ec-193">Escolha qualquer navegador, insira um nome e uma mensagem e selecione o botão **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="0a1ec-193">Choose either browser, enter a name and message, and select the **Send** button.</span></span>

  <span data-ttu-id="0a1ec-194">O nome e a mensagem são exibidos em ambas as páginas instantaneamente.</span><span class="sxs-lookup"><span data-stu-id="0a1ec-194">The name and message are displayed on both pages instantly.</span></span>

  ![Aplicativo de exemplo do SignalR](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="0a1ec-196">Se o aplicativo não funcionar, abra as ferramentas para desenvolvedores do navegador (F12) e acesse o console.</span><span class="sxs-lookup"><span data-stu-id="0a1ec-196">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="0a1ec-197">Você pode encontrar erros relacionados ao código HTML e JavaScript.</span><span class="sxs-lookup"><span data-stu-id="0a1ec-197">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="0a1ec-198">Por exemplo, suponha que você coloque *signalr.js* em uma pasta diferente daquela direcionada.</span><span class="sxs-lookup"><span data-stu-id="0a1ec-198">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="0a1ec-199">Nesse caso, a referência a esse arquivo não funcionará e ocorrerá um erro 404 no console.</span><span class="sxs-lookup"><span data-stu-id="0a1ec-199">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="0a1ec-200">![Erro de signalr.js não encontrado](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="0a1ec-200">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="0a1ec-201">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="0a1ec-201">Next steps</span></span>

<span data-ttu-id="0a1ec-202">Se você quiser que os clientes se conectem a um aplicativo do SignalR de domínios diferentes, habilite o CORS (Compartilhamento de recursos entre origens).</span><span class="sxs-lookup"><span data-stu-id="0a1ec-202">If you want clients to connect to a SignalR app from different domains, you have to enable Cross-Origin Resource Sharing (CORS).</span></span> <span data-ttu-id="0a1ec-203">Para obter mais informações, confira [Compartilhamento de recursos entre origens](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing).</span><span class="sxs-lookup"><span data-stu-id="0a1ec-203">For more information, see [Cross-origin resource sharing](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing).</span></span>

<span data-ttu-id="0a1ec-204">Para saber mais sobre o SignalR, os hubs e os clientes JavaScript, confira estes recursos:</span><span class="sxs-lookup"><span data-stu-id="0a1ec-204">To learn more about SignalR, hubs, and JavaScript clients, see these resources:</span></span>

* [<span data-ttu-id="0a1ec-205">Introdução ao SignalR para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0a1ec-205">Introduction to SignalR for ASP.NET Core</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="0a1ec-206">Usar hubs no SignalR do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0a1ec-206">Use hubs in SignalR for ASP.NET Core</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="0a1ec-207">Cliente JavaScript do SignalR do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0a1ec-207">ASP.NET Core SignalR JavaScript client</span></span>](xref:signalr/javascript-client)
