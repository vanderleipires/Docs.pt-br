---
uid: signalr/get-started-signalr-core
title: "Introdução ao SignalR no ASP.NET Core"
author: rachelappel
ms.author: rachelap
description: "Neste tutorial, você deve criar um aplicativo usando o SignalR para ASP.NET Core."
manager: wpickett
ms.date: 03/06/2018
ms.topic: tutorial
ms.technology: dotnet-signalr
ms.prod: aspnet-core
ms.custom: mvc
ms.openlocfilehash: 5e16569aa492e3639aa97abd241610b361fb0c56
ms.sourcegitcommit: 53ee14b9c8200f44705d8997c3619fa874192d45
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/08/2018
---
# <a name="tutorial-get-started-with-signalr-for-aspnet-core"></a><span data-ttu-id="15b16-103">Tutorial: Introdução ao SignalR para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="15b16-103">Tutorial: Get started with SignalR for ASP.NET Core</span></span>

<span data-ttu-id="15b16-104">Por [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="15b16-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="15b16-105">Este tutorial ensina as Noções básicas de criação de um aplicativo em tempo real com SignalR para ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="15b16-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Solução](get-started-signalr-core/_static/signalr-get-started-finished.png)

<span data-ttu-id="15b16-107">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started-signalr-core/sample/) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="15b16-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started-signalr-core/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="15b16-108">Este tutorial demonstra as seguintes tarefas de desenvolvimento SignalR:</span><span class="sxs-lookup"><span data-stu-id="15b16-108">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="15b16-109">Crie um aplicativo web do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="15b16-109">Create an ASP.NET Core web app.</span></span>
> * <span data-ttu-id="15b16-110">Crie um hub SignalR para enviar por push o conteúdo aos clientes.</span><span class="sxs-lookup"><span data-stu-id="15b16-110">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="15b16-111">Use a biblioteca de SignalR JavaScript para enviar mensagens e exibir atualizações do hub.</span><span class="sxs-lookup"><span data-stu-id="15b16-111">Use the SignalR JavaScript library to send messages and display updates from the hub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="15b16-112">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="15b16-112">Prerequisites</span></span>

<span data-ttu-id="15b16-113">Instale o software a seguir:</span><span class="sxs-lookup"><span data-stu-id="15b16-113">Install the following software:</span></span>

* <span data-ttu-id="15b16-114">[.NET core 2.1.0 visualizar 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) ou posterior</span><span class="sxs-lookup"><span data-stu-id="15b16-114">[.NET Core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) or later</span></span>
* <span data-ttu-id="15b16-115">[Visual Studio de 2017](https://www.visualstudio.com/downloads/) versão 15.6 ou posterior com a carga de trabalho de desenvolvimento do ASP.NET e web</span><span class="sxs-lookup"><span data-stu-id="15b16-115">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.6 or later with the ASP.NET and web development workload</span></span>
* [<span data-ttu-id="15b16-116">npm</span><span class="sxs-lookup"><span data-stu-id="15b16-116">npm</span></span>](https://www.npmjs.com/get-npm)

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="15b16-117">Criar um projeto do ASP.NET Core que hospeda o servidor e cliente SignalR</span><span class="sxs-lookup"><span data-stu-id="15b16-117">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

1. <span data-ttu-id="15b16-118">Use o **arquivo** > **novo projeto** menu opção e escolha **aplicativo Web do ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="15b16-118">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="15b16-119">Nomeie o projeto `SignalRChat`.</span><span class="sxs-lookup"><span data-stu-id="15b16-119">Name the project `SignalRChat`.</span></span>

  ![Caixa de diálogo Nova projeto no Visual Studio](get-started-signalr-core/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="15b16-121">Selecione **aplicativo Web** para criar um projeto usando as páginas Razor.</span><span class="sxs-lookup"><span data-stu-id="15b16-121">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="15b16-122">Em seguida, selecione **Okey**.</span><span class="sxs-lookup"><span data-stu-id="15b16-122">Then select **Ok**.</span></span> <span data-ttu-id="15b16-123">Certifique-se de que **ASP.NET Core 2.1** é selecionado do seletor do framework, embora o SignalR é executado em versões anteriores do .NET.</span><span class="sxs-lookup"><span data-stu-id="15b16-123">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

  ![Caixa de diálogo Nova projeto no Visual Studio](get-started-signalr-core/_static/signalr-new-project-choose-type.png)

  <span data-ttu-id="15b16-125">As bibliotecas que hospedam o código do lado do servidor SignalR são incluídas no modelo de projeto.</span><span class="sxs-lookup"><span data-stu-id="15b16-125">The libraries that host SignalR server-side code are included in the project template.</span></span> <span data-ttu-id="15b16-126">Instalar o JavaScript do lado do cliente separadamente com [npm](https://www.npmjs.com/).</span><span class="sxs-lookup"><span data-stu-id="15b16-126">Install the client-side JavaScript separately with [npm](https://www.npmjs.com/).</span></span>

  ```console
   npm install @aspnet/signalr
  ```

3. <span data-ttu-id="15b16-127">Copie o *signalr.js* de *node_modules\\ @aspnet\signalr\dist\browser*  para o *wwwroot\lib* pasta em seu projeto.</span><span class="sxs-lookup"><span data-stu-id="15b16-127">Copy the *signalr.js* from *node_modules\\@aspnet\signalr\dist\browser* to the *wwwroot\lib* folder in your project.</span></span>

## <a name="create-the-signalr-hub"></a><span data-ttu-id="15b16-128">Criar o Hub SignalR</span><span class="sxs-lookup"><span data-stu-id="15b16-128">Create the SignalR Hub</span></span>

<span data-ttu-id="15b16-129">Um hub é uma classe que serve como um pipeline de alto nível que permite que o cliente e servidor chamar métodos em si.</span><span class="sxs-lookup"><span data-stu-id="15b16-129">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

1. <span data-ttu-id="15b16-130">Adicionar uma classe ao projeto, escolhendo **arquivo** > **novo** > **arquivo** e selecionando **Visual C# classe**.</span><span class="sxs-lookup"><span data-stu-id="15b16-130">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span> 

1. <span data-ttu-id="15b16-131">Herdar de `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="15b16-131">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="15b16-132">O `Hub` classe contém propriedades e eventos para gerenciar conexões e grupos, bem como enviar e receber dados.</span><span class="sxs-lookup"><span data-stu-id="15b16-132">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

1. <span data-ttu-id="15b16-133">Criar o `Send` método que envia uma mensagem a todos os clientes conectados bate-papo.</span><span class="sxs-lookup"><span data-stu-id="15b16-133">Create the `Send` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="15b16-134">Observe que ele retorna um `Task`, pois o SignalR é assíncrono.</span><span class="sxs-lookup"><span data-stu-id="15b16-134">Notice it returns a `Task`, because SignalR is asynchronous.</span></span> <span data-ttu-id="15b16-135">Será melhor dimensionado código assíncrono.</span><span class="sxs-lookup"><span data-stu-id="15b16-135">Asynchronous code scales better.</span></span>

  [!code-csharp[Startup](get-started-signalr-core/sample/Hubs/ChatHub.cs?range=7-14)]

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="15b16-136">Configurar o projeto para usar o SignalR</span><span class="sxs-lookup"><span data-stu-id="15b16-136">Configure the project to use SignalR</span></span>

<span data-ttu-id="15b16-137">O servidor do SignalR deve ser configurado para que ele saiba que pode passar solicitações para o SignalR.</span><span class="sxs-lookup"><span data-stu-id="15b16-137">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="15b16-138">Para configurar um projeto SignalR, modifique o `ConfigureServices` método do aplicativo `Startup` classe inserindo uma chamada para `services.AddSignalR`.</span><span class="sxs-lookup"><span data-stu-id="15b16-138">To configure a SignalR project, modify the `ConfigureServices` method of the application's `Startup` class by inserting a call to `services.AddSignalR`.</span></span>

  <span data-ttu-id="15b16-139">`services.AddSignalR` Adiciona o SignalR como parte do [middleware ASP.NET Core](xref:fundamentals/middleware/index) pipeline.</span><span class="sxs-lookup"><span data-stu-id="15b16-139">`services.AddSignalR` adds SignalR as part of the [ASP.NET Core middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

1. <span data-ttu-id="15b16-140">Configurar rotas para seus hubs usando `UseSignalR`.</span><span class="sxs-lookup"><span data-stu-id="15b16-140">Configure routes to your hubs using `UseSignalR`.</span></span>

  [!code-csharp[Startup](get-started-signalr-core/sample/Startup.cs?highlight=22,40-43)]

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="15b16-141">Criar o código de cliente SignalR</span><span class="sxs-lookup"><span data-stu-id="15b16-141">Create the SignalR client code</span></span>

1. <span data-ttu-id="15b16-142">Substitua o conteúdo *Pages\Index.cshtml* com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="15b16-142">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](get-started-signalr-core/sample/Pages/Index.cshtml)]

  <span data-ttu-id="15b16-143">O HTML anterior exibe o nome e os campos de mensagem e um botão de envio.</span><span class="sxs-lookup"><span data-stu-id="15b16-143">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="15b16-144">Observe as referências de script na parte inferior: uma referência para o SignalR e *chat.js*.</span><span class="sxs-lookup"><span data-stu-id="15b16-144">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

1. <span data-ttu-id="15b16-145">Adicionar um arquivo JavaScript para o *wwwroot\js* pasta denominada *chat.js* e adicione o seguinte código para ele:</span><span class="sxs-lookup"><span data-stu-id="15b16-145">Add a JavaScript file to the *wwwroot\js* folder named *chat.js* and add the following code to it:</span></span>

  [!code-javascript[Index](get-started-signalr-core/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a><span data-ttu-id="15b16-146">Executar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="15b16-146">Run the app</span></span>

1. <span data-ttu-id="15b16-147">Selecione **depurar** > **iniciar sem depuração** para iniciar um navegador e carregar o site localmente.</span><span class="sxs-lookup"><span data-stu-id="15b16-147">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="15b16-148">Copie a URL da barra de endereços.</span><span class="sxs-lookup"><span data-stu-id="15b16-148">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="15b16-149">Abrir outra instância de navegador (qualquer navegador) e cole a URL na barra de endereços.</span><span class="sxs-lookup"><span data-stu-id="15b16-149">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="15b16-150">Escolha um navegador, digite um nome e uma mensagem e clique no **enviar** botão.</span><span class="sxs-lookup"><span data-stu-id="15b16-150">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="15b16-151">O nome e a mensagem são exibidos em ambas as páginas instantaneamente.</span><span class="sxs-lookup"><span data-stu-id="15b16-151">The name and message are displayed on both pages instantly.</span></span>

  ![Solução](get-started-signalr-core/_static/signalr-get-started-finished.png)
