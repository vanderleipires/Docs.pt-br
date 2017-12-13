---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Exemplos de Katana | Microsoft Docs
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2014
ms.topic: article
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: 5540164bda8db31c4e78b49ecb7f7c573acca013
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="katana-samples"></a><span data-ttu-id="0e9fe-102">Exemplos de Katana</span><span class="sxs-lookup"><span data-stu-id="0e9fe-102">Katana Samples</span></span>
====================
<span data-ttu-id="0e9fe-103">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="0e9fe-103">by [Microsoft](https://github.com/microsoft)</span></span>

## <a name="katana-samples"></a><span data-ttu-id="0e9fe-104">Exemplos de Katana</span><span class="sxs-lookup"><span data-stu-id="0e9fe-104">Katana Samples</span></span>

<span data-ttu-id="0e9fe-105">**Exemplo de rotas do ASP.NET** | [código-fonte](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="0e9fe-105">**ASP.NET Routes Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)</span></span>  
<span data-ttu-id="0e9fe-106">Em alguns aplicativos você deseja conectar componentes OWIN na tabela de rotas Asp.Net lado a lado com componentes não-OWIN.</span><span class="sxs-lookup"><span data-stu-id="0e9fe-106">In some applications you will want to hook up OWIN components in the Asp.Net route table side by side with non-OWIN components.</span></span> <span data-ttu-id="0e9fe-107">Este exemplo mostra como usar os métodos de extensão RouteCollection MapOwinPath e MapOwinRoute fornecida pelo systemweb.</span><span class="sxs-lookup"><span data-stu-id="0e9fe-107">This sample shows how to use the RouteCollection extension methods MapOwinPath and MapOwinRoute provided by Microsoft.Owin.Host.SystemWeb.</span></span>

<span data-ttu-id="0e9fe-108">**A ramificação Pipelines exemplo** | [código-fonte](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="0e9fe-108">**Branching Pipelines Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)</span></span>  
<span data-ttu-id="0e9fe-109">Pipelines de processamento de solicitação OWIN não precisam ser lineares, eles podem ser ramificados para processar solicitações de maneiras diferentes.</span><span class="sxs-lookup"><span data-stu-id="0e9fe-109">OWIN request processing pipelines do not need to be linear, they can be branched to process requests in different ways.</span></span> <span data-ttu-id="0e9fe-110">Este exemplo mostra como construir um pipeline de ramificação com base em caminhos de solicitação ou outros dados de solicitação, como cabeçalhos.</span><span class="sxs-lookup"><span data-stu-id="0e9fe-110">This sample shows how to construct a branching pipeline based on request paths or other request data such as headers.</span></span> <span data-ttu-id="0e9fe-111">Esses componentes estão disponíveis no pacote de nuget Microsoft.Owin.Mapping.</span><span class="sxs-lookup"><span data-stu-id="0e9fe-111">These components are available in the Microsoft.Owin.Mapping nuget package.</span></span>

<span data-ttu-id="0e9fe-112">**Exemplo de servidor personalizado** | [código-fonte](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs) </span><span class="sxs-lookup"><span data-stu-id="0e9fe-112">**Custom Server Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs) </span></span>  
<span data-ttu-id="0e9fe-113">Mostra como usar um servidor OWIN personalizado quando auto-hospedagem OWIN.</span><span class="sxs-lookup"><span data-stu-id="0e9fe-113">Shows how to use a custom OWIN server when self-hosting OWIN.</span></span>

<span data-ttu-id="0e9fe-114">**Inserido exemplo** | [código-fonte](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="0e9fe-114">**Embedded Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)</span></span>  
<span data-ttu-id="0e9fe-115">Alguns servidores OWIN podem ser executados em seu próprio processo (&quot;auto-hospedado&quot;).</span><span class="sxs-lookup"><span data-stu-id="0e9fe-115">Some OWIN servers can be run inside of your own process (&quot;self-hosted&quot;).</span></span> <span data-ttu-id="0e9fe-116">Este exemplo mostra como iniciar um aplicativo OWIN usando as ferramentas fornecidas pelo pacote nuget Hosting.</span><span class="sxs-lookup"><span data-stu-id="0e9fe-116">This sample shows how to start an OWIN application using the tools provided by the Microsoft.Owin.Hosting nuget package.</span></span>

<span data-ttu-id="0e9fe-117">**Exemplo de HelloWorld** | [código-fonte](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="0e9fe-117">**HelloWorld Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)</span></span>  
<span data-ttu-id="0e9fe-118">OWIN é um abstração de API que permite a portabilidade de aplicativo em vários servidores do servidor HTTP.</span><span class="sxs-lookup"><span data-stu-id="0e9fe-118">OWIN is a HTTP server API abstraction that enables application portability across various servers.</span></span> <span data-ttu-id="0e9fe-119">Este exemplo demonstra como escrever um aplicativo Hello World usando alguns **wrappers simples** em torno de bruto abstração OWIN e execute-o em um servidor web como o ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0e9fe-119">This sample demonstrates how to write a Hello World application using some **simple wrappers** around the raw OWIN abstraction and run it on a web server like ASP.NET.</span></span>

<span data-ttu-id="0e9fe-120">**Exemplo OWIN bruto Hello World** | [código-fonte](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="0e9fe-120">**Hello World Raw OWIN Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)</span></span>  
<span data-ttu-id="0e9fe-121">Este exemplo demonstra como escrever um aplicativo Hello World usando o **bruto** abstração OWIN e executá-lo em um servidor web como o Asp.Net.</span><span class="sxs-lookup"><span data-stu-id="0e9fe-121">This sample demonstrates how to write a Hello World application using the **raw** OWIN abstraction and run it on a web server like Asp.Net.</span></span>

<span data-ttu-id="0e9fe-122">**Exemplo de SignalR** | [código-fonte](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)</span><span class="sxs-lookup"><span data-stu-id="0e9fe-122">**SignalR Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)</span></span>  
<span data-ttu-id="0e9fe-123">Mostra como hospedar o SignalR usando OWIN / Katana.</span><span class="sxs-lookup"><span data-stu-id="0e9fe-123">Shows how to self-host SignalR using OWIN / Katana.</span></span> <span data-ttu-id="0e9fe-124">Para obter mais informações sobre hospedagem interna SignalR, consulte [Tutorial: SignalR auto-host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span><span class="sxs-lookup"><span data-stu-id="0e9fe-124">For more info about self-hosting SignalR, see [Tutorial: SignalR Self-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span></span>

<span data-ttu-id="0e9fe-125">**Exemplo de arquivos estáticos** | [código-fonte](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs) </span><span class="sxs-lookup"><span data-stu-id="0e9fe-125">**Static Files Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs) </span></span>  
<span data-ttu-id="0e9fe-126">Mostra como dar suporte a solicitações HTTP para arquivos estáticos usando OWIN / Katana.</span><span class="sxs-lookup"><span data-stu-id="0e9fe-126">Shows how to support HTTP requests for static files using OWIN / Katana.</span></span>

<span data-ttu-id="0e9fe-127">**Web API** | [código-fonte](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt) </span><span class="sxs-lookup"><span data-stu-id="0e9fe-127">**Web API** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt) </span></span>  
<span data-ttu-id="0e9fe-128">Este exemplo mostra como hospedar OWIN no IIS e adicionar a API da Web para o pipeline OWIN.</span><span class="sxs-lookup"><span data-stu-id="0e9fe-128">This sample shows how to host OWIN in IIS and add Web API to the OWIN pipeline.</span></span>

<span data-ttu-id="0e9fe-129">**Exemplo de soquete de Web** | [código-fonte](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs) </span><span class="sxs-lookup"><span data-stu-id="0e9fe-129">**Web Socket Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs) </span></span>  
<span data-ttu-id="0e9fe-130">Mostra como suportar soquetes da Web em OWIN usando o [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/en-us/library/system.net.websockets.websocket(v=vs.110).aspx) classe.</span><span class="sxs-lookup"><span data-stu-id="0e9fe-130">Shows how to support Web Sockets in OWIN by using the [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/en-us/library/system.net.websockets.websocket(v=vs.110).aspx) class.</span></span>
