---
title: Introdução ao SignalR no ASP.NET Core
author: rachelappel
description: Neste tutorial, você deve criar um aplicativo usando o SignalR para ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/22/2018
ms.prod: aspnet-core
ms.topic: tutorial
ms.technology: aspnet
uid: signalr/get-started
ms.openlocfilehash: eb14fbf42f5c18ccdc3ca42af8fd8bcfaa15c623
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2018
ms.locfileid: "34688582"
---
# <a name="get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="816b6-103">Introdução ao SignalR no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="816b6-103">Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="816b6-104">Por [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="816b6-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="816b6-105">Este tutorial ensina as Noções básicas de criação de um aplicativo em tempo real com SignalR para ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="816b6-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Solução](get-started/_static/signalr-get-started-finished.png)

<span data-ttu-id="816b6-107">Este tutorial demonstra as seguintes tarefas de desenvolvimento SignalR:</span><span class="sxs-lookup"><span data-stu-id="816b6-107">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="816b6-108">Crie um SignalR no aplicativo web do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="816b6-108">Create a SignalR on ASP.NET Core web app.</span></span>
> * <span data-ttu-id="816b6-109">Crie um hub SignalR para enviar por push o conteúdo aos clientes.</span><span class="sxs-lookup"><span data-stu-id="816b6-109">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="816b6-110">Modificar o `Startup` classe e configurar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="816b6-110">Modify the `Startup` class and configure the app.</span></span>

<span data-ttu-id="816b6-111">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="816b6-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="prerequisites"></a><span data-ttu-id="816b6-112">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="816b6-112">Prerequisites</span></span>

<span data-ttu-id="816b6-113">Instale o software a seguir:</span><span class="sxs-lookup"><span data-stu-id="816b6-113">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="816b6-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="816b6-114">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="816b6-115">.NET core SDK 2.1 ou posterior</span><span class="sxs-lookup"><span data-stu-id="816b6-115">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="816b6-116">[Visual Studio de 2017](https://www.visualstudio.com/downloads/) 15.7 ou posterior com o **desenvolvimento ASP.NET e web** carga de trabalho</span><span class="sxs-lookup"><span data-stu-id="816b6-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7 or later with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="816b6-117">npm</span><span class="sxs-lookup"><span data-stu-id="816b6-117">npm</span></span>](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="816b6-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="816b6-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="816b6-119">.NET core SDK 2.1 ou posterior</span><span class="sxs-lookup"><span data-stu-id="816b6-119">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="816b6-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="816b6-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="816b6-121">C# para o código do Visual Studio</span><span class="sxs-lookup"><span data-stu-id="816b6-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="816b6-122">npm</span><span class="sxs-lookup"><span data-stu-id="816b6-122">npm</span></span>](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="816b6-123">Criar um projeto do ASP.NET Core que hospeda o servidor e cliente SignalR</span><span class="sxs-lookup"><span data-stu-id="816b6-123">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="816b6-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="816b6-124">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="816b6-125">Use o **arquivo** > **novo projeto** menu opção e escolha **aplicativo Web do ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="816b6-125">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="816b6-126">Nomeie o projeto *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="816b6-126">Name the project *SignalRChat*.</span></span>

   ![Caixa de diálogo Nova projeto no Visual Studio](get-started/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="816b6-128">Selecione **aplicativo Web** para criar um projeto usando as páginas Razor.</span><span class="sxs-lookup"><span data-stu-id="816b6-128">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="816b6-129">Em seguida, selecione **Okey**.</span><span class="sxs-lookup"><span data-stu-id="816b6-129">Then select **OK**.</span></span> <span data-ttu-id="816b6-130">Certifique-se de que **ASP.NET Core 2.1** é selecionado do seletor do framework, embora o SignalR é executado em versões anteriores do .NET.</span><span class="sxs-lookup"><span data-stu-id="816b6-130">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

   ![Caixa de diálogo Nova projeto no Visual Studio](get-started/_static/signalr-new-project-choose-type.png)

<span data-ttu-id="816b6-132">O Visual Studio inclui a `Microsoft.AspNetCore.SignalR` pacote que contém suas bibliotecas do servidor como parte de seu **aplicativo Web do ASP.NET Core** modelo.</span><span class="sxs-lookup"><span data-stu-id="816b6-132">Visual Studio includes the `Microsoft.AspNetCore.SignalR` package containing its server libraries as part of its **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="816b6-133">No entanto, a biblioteca de cliente JavaScript para o SignalR deve ser instalada usando *npm*.</span><span class="sxs-lookup"><span data-stu-id="816b6-133">However, the JavaScript client library for SignalR must be installed using *npm*.</span></span>

3. <span data-ttu-id="816b6-134">Execute os seguintes comandos **Package Manager Console** janela, na raiz do projeto:</span><span class="sxs-lookup"><span data-stu-id="816b6-134">Run the following commands in the **Package Manager Console** window, from the project root:</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```     

4. <span data-ttu-id="816b6-135">Copiar o *signalr.js* arquivo *node_modules\\ @aspnet\signalr\dist\browser*  para o *lib* pasta em seu projeto.</span><span class="sxs-lookup"><span data-stu-id="816b6-135">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to the *lib* folder in your project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="816b6-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="816b6-136">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="816b6-137">Do **Terminal integrada**, execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="816b6-137">From the **Integrated Terminal**, run the following command:</span></span>

    ```console
    dotnet new webapp -o SignalRChat
    ```

2. <span data-ttu-id="816b6-138">Instalar a biblioteca de cliente JavaScript usando *npm*.</span><span class="sxs-lookup"><span data-stu-id="816b6-138">Install the JavaScript client library using *npm*.</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

3. <span data-ttu-id="816b6-139">Copiar o *signalr.js* arquivo *node_modules\\ @aspnet\signalr\dist\browser*  para o *lib* pasta em seu projeto.</span><span class="sxs-lookup"><span data-stu-id="816b6-139">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to the *lib* folder in your project.</span></span>

-----

## <a name="create-the-signalr-hub"></a><span data-ttu-id="816b6-140">Criar o Hub SignalR</span><span class="sxs-lookup"><span data-stu-id="816b6-140">Create the SignalR Hub</span></span>

<span data-ttu-id="816b6-141">Um hub é uma classe que serve como um pipeline de alto nível que permite que o cliente e servidor chamar métodos em si.</span><span class="sxs-lookup"><span data-stu-id="816b6-141">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="816b6-142">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="816b6-142">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="816b6-143">Adicionar uma classe ao projeto, escolhendo **arquivo** > **novo** > **arquivo** e selecionando **Visual C# classe**.</span><span class="sxs-lookup"><span data-stu-id="816b6-143">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span>

2. <span data-ttu-id="816b6-144">Herdar de `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="816b6-144">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="816b6-145">O `Hub` classe contém propriedades e eventos para gerenciar conexões e grupos, bem como enviar e receber dados.</span><span class="sxs-lookup"><span data-stu-id="816b6-145">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

3. <span data-ttu-id="816b6-146">Criar o `SendMessage` método que envia uma mensagem a todos os clientes conectados bate-papo.</span><span class="sxs-lookup"><span data-stu-id="816b6-146">Create the `SendMessage` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="816b6-147">Observe que ele retorna um [tarefa](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), pois o SignalR é assíncrono.</span><span class="sxs-lookup"><span data-stu-id="816b6-147">Notice it returns a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), because SignalR is asynchronous.</span></span> <span data-ttu-id="816b6-148">Será melhor dimensionado código assíncrono.</span><span class="sxs-lookup"><span data-stu-id="816b6-148">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="816b6-149">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="816b6-149">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="816b6-150">Abra o *SignalRChat* pasta no código do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="816b6-150">Open the *SignalRChat* folder in Visual Studio Code.</span></span>

2. <span data-ttu-id="816b6-151">Adicionar uma classe ao projeto selecionando **arquivo** > **novo arquivo** no menu.</span><span class="sxs-lookup"><span data-stu-id="816b6-151">Add a class to the project by selecting **File** > **New File** from the menu.</span></span>

3. <span data-ttu-id="816b6-152">Herdar de `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="816b6-152">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="816b6-153">O `Hub` classe contém propriedades e eventos para gerenciar conexões e grupos, bem como enviar e receber dados aos clientes.</span><span class="sxs-lookup"><span data-stu-id="816b6-153">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data to clients.</span></span>

4. <span data-ttu-id="816b6-154">Adicione o método `SendMessage` à classe.</span><span class="sxs-lookup"><span data-stu-id="816b6-154">Add a `SendMessage` method to the class.</span></span> <span data-ttu-id="816b6-155">O `SendMessage` método envia uma mensagem a todos os clientes conectados bate-papo.</span><span class="sxs-lookup"><span data-stu-id="816b6-155">The `SendMessage` method sends a message to all connected chat clients.</span></span> <span data-ttu-id="816b6-156">Observe que ele retorna um [tarefa](/dotnet/api/system.threading.tasks.task), pois o SignalR é assíncrono.</span><span class="sxs-lookup"><span data-stu-id="816b6-156">Notice it returns a [Task](/dotnet/api/system.threading.tasks.task), because SignalR is asynchronous.</span></span> <span data-ttu-id="816b6-157">Será melhor dimensionado código assíncrono.</span><span class="sxs-lookup"><span data-stu-id="816b6-157">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs?range=6-12)]

-----

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="816b6-158">Configurar o projeto para usar o SignalR</span><span class="sxs-lookup"><span data-stu-id="816b6-158">Configure the project to use SignalR</span></span>

<span data-ttu-id="816b6-159">O servidor do SignalR deve ser configurado para que ele saiba que pode passar solicitações para o SignalR.</span><span class="sxs-lookup"><span data-stu-id="816b6-159">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="816b6-160">Para configurar um projeto SignalR, modifique o projeto `Startup.ConfigureServices` método.</span><span class="sxs-lookup"><span data-stu-id="816b6-160">To configure a SignalR project, modify the project's `Startup.ConfigureServices` method.</span></span>

   <span data-ttu-id="816b6-161">`services.AddSignalR` Adiciona o SignalR como parte do [middleware](xref:fundamentals/middleware/index) pipeline.</span><span class="sxs-lookup"><span data-stu-id="816b6-161">`services.AddSignalR` adds SignalR as part of the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

2. <span data-ttu-id="816b6-162">Configurar rotas para seus hubs usando `UseSignalR`.</span><span class="sxs-lookup"><span data-stu-id="816b6-162">Configure routes to your hubs using `UseSignalR`.</span></span>


   [!code-csharp[Startup](get-started/sample/Startup.cs?highlight=37,57-60)]


## <a name="create-the-signalr-client-code"></a><span data-ttu-id="816b6-163">Criar o código de cliente SignalR</span><span class="sxs-lookup"><span data-stu-id="816b6-163">Create the SignalR client code</span></span>

1. <span data-ttu-id="816b6-164">Substitua o conteúdo *Pages\Index.cshtml* com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="816b6-164">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

   [!code-cshtml[Index](get-started/sample/Pages/Index.cshtml)]

   <span data-ttu-id="816b6-165">O HTML anterior exibe o nome e os campos de mensagem e um botão de envio.</span><span class="sxs-lookup"><span data-stu-id="816b6-165">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="816b6-166">Observe as referências de script na parte inferior: uma referência para o SignalR e *chat.js*.</span><span class="sxs-lookup"><span data-stu-id="816b6-166">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

2. <span data-ttu-id="816b6-167">Adicione um arquivo JavaScript, denominado *chat.js*, para o *wwwroot\js* pasta.</span><span class="sxs-lookup"><span data-stu-id="816b6-167">Add a JavaScript file, named *chat.js*, to the *wwwroot\js* folder.</span></span> <span data-ttu-id="816b6-168">Adicione o seguinte código a ele:</span><span class="sxs-lookup"><span data-stu-id="816b6-168">Add the following code to it:</span></span>

   [!code-javascript[Index](get-started/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a><span data-ttu-id="816b6-169">Executar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="816b6-169">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="816b6-170">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="816b6-170">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="816b6-171">Selecione **depurar** > **iniciar sem depuração** para iniciar um navegador e carregar o site localmente.</span><span class="sxs-lookup"><span data-stu-id="816b6-171">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="816b6-172">Copie a URL da barra de endereços.</span><span class="sxs-lookup"><span data-stu-id="816b6-172">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="816b6-173">Abrir outra instância de navegador (qualquer navegador) e cole a URL na barra de endereços.</span><span class="sxs-lookup"><span data-stu-id="816b6-173">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="816b6-174">Escolha um navegador, digite um nome e uma mensagem e clique no **enviar** botão.</span><span class="sxs-lookup"><span data-stu-id="816b6-174">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="816b6-175">O nome e a mensagem são exibidos em ambas as páginas instantaneamente.</span><span class="sxs-lookup"><span data-stu-id="816b6-175">The name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="816b6-176">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="816b6-176">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="816b6-177">Pressione **Depurar** (F5) para compilar e executar o programa.</span><span class="sxs-lookup"><span data-stu-id="816b6-177">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="816b6-178">Executar o programa abre uma janela do navegador.</span><span class="sxs-lookup"><span data-stu-id="816b6-178">Running the program opens a browser window.</span></span>

1. <span data-ttu-id="816b6-179">Abrir outra janela do navegador e carregar o site localmente nele.</span><span class="sxs-lookup"><span data-stu-id="816b6-179">Open another browser window and load the website locally in it.</span></span>

1. <span data-ttu-id="816b6-180">Escolha um navegador, digite um nome e uma mensagem e clique no **enviar** botão.</span><span class="sxs-lookup"><span data-stu-id="816b6-180">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="816b6-181">O nome e a mensagem são exibidos em ambas as páginas instantaneamente.</span><span class="sxs-lookup"><span data-stu-id="816b6-181">The name and message are displayed on both pages instantly.</span></span>

---

  ![Solução](get-started/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a><span data-ttu-id="816b6-183">Recursos relacionados</span><span class="sxs-lookup"><span data-stu-id="816b6-183">Related resources</span></span>

[<span data-ttu-id="816b6-184">Introdução ao ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="816b6-184">Introduction to ASP.NET Core SignalR</span></span>](introduction.md)
