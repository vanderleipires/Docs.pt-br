---
title: Introdução ao SignalR no ASP.NET Core
author: rachelappel
description: Neste tutorial, você deverá criar um aplicativo usando o SignalR para ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/22/2018
uid: tutorials/signalr
ms.openlocfilehash: 62cef2d6f032caa2f048cfdd49a225d975dad10d
ms.sourcegitcommit: 7003d27b607e529642ded0400aa48ae692a0e666
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37033336"
---
# <a name="get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="f12db-103">Introdução ao SignalR no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f12db-103">Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="f12db-104">Por [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="f12db-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="f12db-105">Este tutorial ensina as noções básicas para compilar um aplicativo em tempo real com SignalR para ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f12db-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Solução](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="f12db-107">Este tutorial demonstra as seguintes tarefas de desenvolvimento SignalR:</span><span class="sxs-lookup"><span data-stu-id="f12db-107">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f12db-108">Crie um SignalR no aplicativo Web do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f12db-108">Create a SignalR on ASP.NET Core web app.</span></span>
> * <span data-ttu-id="f12db-109">Crie um hub SignalR para efetuar push do conteúdo aos clientes.</span><span class="sxs-lookup"><span data-stu-id="f12db-109">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="f12db-110">Modifique a classe `Startup` e configure o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="f12db-110">Modify the `Startup` class and configure the app.</span></span>

<span data-ttu-id="f12db-111">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f12db-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f12db-112">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="f12db-112">Prerequisites</span></span>

<span data-ttu-id="f12db-113">Instale o software a seguir:</span><span class="sxs-lookup"><span data-stu-id="f12db-113">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f12db-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f12db-114">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="f12db-115">SDK do .NET Core 2.1 Core ou posterior</span><span class="sxs-lookup"><span data-stu-id="f12db-115">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="f12db-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) versão 15.7 ou posterior com a carga de trabalho do **ASP.NET e desenvolvimento para a Web**</span><span class="sxs-lookup"><span data-stu-id="f12db-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7 or later with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="f12db-117">npm</span><span class="sxs-lookup"><span data-stu-id="f12db-117">npm</span></span>](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f12db-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f12db-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="f12db-119">SDK do .NET Core 2.1 Core ou posterior</span><span class="sxs-lookup"><span data-stu-id="f12db-119">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="f12db-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f12db-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="f12db-121">C# para Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f12db-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="f12db-122">npm</span><span class="sxs-lookup"><span data-stu-id="f12db-122">npm</span></span>](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="f12db-123">Criar um projeto do ASP.NET Core que hospede o cliente e o servidor SignalR</span><span class="sxs-lookup"><span data-stu-id="f12db-123">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f12db-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f12db-124">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="f12db-125">Use a opção de menu **Arquivo** > **Novo Projeto** e escolha **Aplicativo Web do ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="f12db-125">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="f12db-126">Dê ao projeto o nome de *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="f12db-126">Name the project *SignalRChat*.</span></span>

   ![Caixa de diálogo Novo Projeto no Visual Studio](signalr/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="f12db-128">Selecione **Aplicativo Web** para criar um projeto usando o Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="f12db-128">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="f12db-129">Em seguida, selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="f12db-129">Then select **OK**.</span></span> <span data-ttu-id="f12db-130">Saiba que o **ASP.NET Core 2.1** é selecionado do seletor de estrutura, embora o SignalR seja executado em versões anteriores do .NET.</span><span class="sxs-lookup"><span data-stu-id="f12db-130">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

   ![Caixa de diálogo Novo Projeto no Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

<span data-ttu-id="f12db-132">O Visual Studio inclui o pacote `Microsoft.AspNetCore.SignalR` que contém suas bibliotecas do servidor como parte de seu modelo de **Aplicativo Web do ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="f12db-132">Visual Studio includes the `Microsoft.AspNetCore.SignalR` package containing its server libraries as part of its **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="f12db-133">No entanto, a biblioteca de cliente JavaScript para SignalR deve ser instalada usando *npm*.</span><span class="sxs-lookup"><span data-stu-id="f12db-133">However, the JavaScript client library for SignalR must be installed using *npm*.</span></span>

3. <span data-ttu-id="f12db-134">Execute os seguintes comandos na janela **Console do Gerenciador de Pacotes**, na raiz do projeto:</span><span class="sxs-lookup"><span data-stu-id="f12db-134">Run the following commands in the **Package Manager Console** window, from the project root:</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

4. <span data-ttu-id="f12db-135">Criar uma nova pasta chamada "signalr" dentro da pasta *lib* em seu projeto.</span><span class="sxs-lookup"><span data-stu-id="f12db-135">Create a new folder named "signalr" inside the  *lib* folder in your project.</span></span> <span data-ttu-id="f12db-136">Copiar o arquivo *signalr.js* de *node_modules\\@aspnet\signalr\dist\browser* para essa pasta.</span><span class="sxs-lookup"><span data-stu-id="f12db-136">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f12db-137">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f12db-137">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="f12db-138">No **Terminal integrada**, execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="f12db-138">From the **Integrated Terminal**, run the following command:</span></span>

    ```console
    dotnet new webapp -o SignalRChat
    ```

    [!INCLUDE[](~/includes/webapp-alias-notice.md)]

2. <span data-ttu-id="f12db-139">Instale a biblioteca de cliente JavaScript usando *npm*.</span><span class="sxs-lookup"><span data-stu-id="f12db-139">Install the JavaScript client library using *npm*.</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

3. <span data-ttu-id="f12db-140">Criar uma nova pasta chamada "signalr" dentro da pasta *lib* em seu projeto.</span><span class="sxs-lookup"><span data-stu-id="f12db-140">Create a new folder named "signalr" inside the  *lib* folder in your project.</span></span> <span data-ttu-id="f12db-141">Copiar o arquivo *signalr.js* de *node_modules\\@aspnet\signalr\dist\browser* para essa pasta.</span><span class="sxs-lookup"><span data-stu-id="f12db-141">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

---

## <a name="create-the-signalr-hub"></a><span data-ttu-id="f12db-142">Criar o Hub SignalR</span><span class="sxs-lookup"><span data-stu-id="f12db-142">Create the SignalR Hub</span></span>

<span data-ttu-id="f12db-143">Um hub é uma classe que serve como um pipeline de alto nível que permite ao cliente e ao servidor chamar métodos um no outro.</span><span class="sxs-lookup"><span data-stu-id="f12db-143">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f12db-144">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f12db-144">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="f12db-145">Adicione uma classe ao projeto escolhendo **Arquivo** > **Novo** > **Arquivo** e selecionando a **Classe Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="f12db-145">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span> <span data-ttu-id="f12db-146">Nomeie a classe `ChatHub` e o arquivo *ChatHub.cs*.</span><span class="sxs-lookup"><span data-stu-id="f12db-146">Name the class `ChatHub` and the file *ChatHub.cs*.</span></span>

2. <span data-ttu-id="f12db-147">Herdar de `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="f12db-147">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="f12db-148">A classe `Hub` contém propriedades e eventos para gerenciar conexões e grupos, bem como enviar e receber dados.</span><span class="sxs-lookup"><span data-stu-id="f12db-148">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

3. <span data-ttu-id="f12db-149">Crie o método `SendMessage` que envia uma mensagem a todos os clientes de chat conectados.</span><span class="sxs-lookup"><span data-stu-id="f12db-149">Create the `SendMessage` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="f12db-150">Observe que ele retorna uma [Tarefa](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), pois o SignalR é assíncrono.</span><span class="sxs-lookup"><span data-stu-id="f12db-150">Notice it returns a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), because SignalR is asynchronous.</span></span> <span data-ttu-id="f12db-151">O código assíncrono tem um dimensionamento melhor.</span><span class="sxs-lookup"><span data-stu-id="f12db-151">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f12db-152">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f12db-152">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="f12db-153">Abra a pasta *SignalRChat* no Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="f12db-153">Open the *SignalRChat* folder in Visual Studio Code.</span></span>

2. <span data-ttu-id="f12db-154">Adicione uma classe ao projeto selecionando **Arquivo** > **Novo Arquivo** no menu.</span><span class="sxs-lookup"><span data-stu-id="f12db-154">Add a class to the project by selecting **File** > **New File** from the menu.</span></span> <span data-ttu-id="f12db-155">Nomeie a classe `ChatHub` e o arquivo *ChatHub.cs*.</span><span class="sxs-lookup"><span data-stu-id="f12db-155">Name the class `ChatHub` and the file *ChatHub.cs*.</span></span>

3. <span data-ttu-id="f12db-156">Herdar de `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="f12db-156">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="f12db-157">A classe `Hub` contém propriedades e eventos para gerenciar conexões e grupos, bem como enviar e receber dados de clientes.</span><span class="sxs-lookup"><span data-stu-id="f12db-157">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data to clients.</span></span>

4. <span data-ttu-id="f12db-158">Adicione o método `SendMessage` à classe.</span><span class="sxs-lookup"><span data-stu-id="f12db-158">Add a `SendMessage` method to the class.</span></span> <span data-ttu-id="f12db-159">O método `SendMessage` envia uma mensagem a todos os clientes de chat conectados.</span><span class="sxs-lookup"><span data-stu-id="f12db-159">The `SendMessage` method sends a message to all connected chat clients.</span></span> <span data-ttu-id="f12db-160">Observe que ele retorna uma [Tarefa](/dotnet/api/system.threading.tasks.task), pois o SignalR é assíncrono.</span><span class="sxs-lookup"><span data-stu-id="f12db-160">Notice it returns a [Task](/dotnet/api/system.threading.tasks.task), because SignalR is asynchronous.</span></span> <span data-ttu-id="f12db-161">O código assíncrono tem um dimensionamento melhor.</span><span class="sxs-lookup"><span data-stu-id="f12db-161">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

-----

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="f12db-162">Configurar o projeto para usar o SignalR</span><span class="sxs-lookup"><span data-stu-id="f12db-162">Configure the project to use SignalR</span></span>

<span data-ttu-id="f12db-163">O servidor do SignalR deve ser configurado de moldo que saiba que pode passar solicitações para o SignalR.</span><span class="sxs-lookup"><span data-stu-id="f12db-163">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="f12db-164">Para configurar um projeto SignalR, modifique o método `Startup.ConfigureServices` do projeto.</span><span class="sxs-lookup"><span data-stu-id="f12db-164">To configure a SignalR project, modify the project's `Startup.ConfigureServices` method.</span></span>

   <span data-ttu-id="f12db-165">`services.AddSignalR` adiciona o SignalR como parte do pipeline [middleware](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="f12db-165">`services.AddSignalR` adds SignalR as part of the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

2. <span data-ttu-id="f12db-166">Configure rotas para seus hubs usando `UseSignalR`.</span><span class="sxs-lookup"><span data-stu-id="f12db-166">Configure routes to your hubs using `UseSignalR`.</span></span>

   [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=37,57-60)]

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="f12db-167">Criar o código de cliente SignalR</span><span class="sxs-lookup"><span data-stu-id="f12db-167">Create the SignalR client code</span></span>

1. <span data-ttu-id="f12db-168">Adicione um arquivo JavaScript chamado *chat.js* à pasta *wwwroot\js*.</span><span class="sxs-lookup"><span data-stu-id="f12db-168">Add a JavaScript file, named *chat.js*, to the *wwwroot\js* folder.</span></span> <span data-ttu-id="f12db-169">Adicione o seguinte código a ele:</span><span class="sxs-lookup"><span data-stu-id="f12db-169">Add the following code to it:</span></span>

   [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

2. <span data-ttu-id="f12db-170">Substitua o conteúdo *Pages\Index.cshtml* pelo código a seguir:</span><span class="sxs-lookup"><span data-stu-id="f12db-170">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

   [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

   <span data-ttu-id="f12db-171">O HTML anterior exibe o nome e os campos de mensagem, além de um botão de envio.</span><span class="sxs-lookup"><span data-stu-id="f12db-171">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="f12db-172">Observe as referências de script na parte inferior: uma referência para o SignalR e *chat.js*.</span><span class="sxs-lookup"><span data-stu-id="f12db-172">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="f12db-173">Executar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="f12db-173">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f12db-174">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f12db-174">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="f12db-175">Selecione **Depurar** > **Iniciar sem depuração** para iniciar um navegador e carregar o site localmente.</span><span class="sxs-lookup"><span data-stu-id="f12db-175">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="f12db-176">Copie a URL da barra de endereços.</span><span class="sxs-lookup"><span data-stu-id="f12db-176">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="f12db-177">Abra outra instância de navegador (qualquer navegador) e cole a URL na barra de endereços.</span><span class="sxs-lookup"><span data-stu-id="f12db-177">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="f12db-178">Escolha um navegador, digite um nome e uma mensagem e clique no botão **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="f12db-178">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="f12db-179">O nome e a mensagem são exibidos em ambas as páginas instantaneamente.</span><span class="sxs-lookup"><span data-stu-id="f12db-179">The name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f12db-180">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f12db-180">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="f12db-181">Pressione **Depurar** (F5) para compilar e executar o programa.</span><span class="sxs-lookup"><span data-stu-id="f12db-181">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="f12db-182">Executar o programa abre uma janela do navegador.</span><span class="sxs-lookup"><span data-stu-id="f12db-182">Running the program opens a browser window.</span></span>

1. <span data-ttu-id="f12db-183">Abra outra janela do navegador e carregue o site localmente.</span><span class="sxs-lookup"><span data-stu-id="f12db-183">Open another browser window and load the website locally in it.</span></span>

1. <span data-ttu-id="f12db-184">Escolha um navegador, digite um nome e uma mensagem e clique no botão **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="f12db-184">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="f12db-185">O nome e a mensagem são exibidos em ambas as páginas instantaneamente.</span><span class="sxs-lookup"><span data-stu-id="f12db-185">The name and message are displayed on both pages instantly.</span></span>

---

  ![Solução](signalr/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a><span data-ttu-id="f12db-187">Recursos relacionados</span><span class="sxs-lookup"><span data-stu-id="f12db-187">Related resources</span></span>

[<span data-ttu-id="f12db-188">Introdução ao ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="f12db-188">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
