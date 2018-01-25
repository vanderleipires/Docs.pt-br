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
ms.openlocfilehash: 815355c00c9c15cfefa5f98dc89da676743b0390
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="katana-samples"></a>Exemplos de Katana
====================
por [Microsoft](https://github.com/microsoft)

## <a name="katana-samples"></a>Exemplos de Katana

**Exemplo de rotas do ASP.NET** | [código-fonte](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)  
Em alguns aplicativos você deseja conectar componentes OWIN na tabela de rotas Asp.Net lado a lado com componentes não-OWIN. Este exemplo mostra como usar os métodos de extensão RouteCollection MapOwinPath e MapOwinRoute fornecida pelo systemweb.

**A ramificação Pipelines exemplo** | [código-fonte](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)  
Pipelines de processamento de solicitação OWIN não precisam ser lineares, eles podem ser ramificados para processar solicitações de maneiras diferentes. Este exemplo mostra como construir um pipeline de ramificação com base em caminhos de solicitação ou outros dados de solicitação, como cabeçalhos. Esses componentes estão disponíveis no pacote de nuget Microsoft.Owin.Mapping.

**Exemplo de servidor personalizado** | [código-fonte](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs)   
Mostra como usar um servidor OWIN personalizado quando auto-hospedagem OWIN.

**Inserido exemplo** | [código-fonte](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)  
Alguns servidores OWIN podem ser executados em seu próprio processo (&quot;auto-hospedado&quot;). Este exemplo mostra como iniciar um aplicativo OWIN usando as ferramentas fornecidas pelo pacote nuget Hosting.

**Exemplo de HelloWorld** | [código-fonte](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)  
OWIN é um abstração de API que permite a portabilidade de aplicativo em vários servidores do servidor HTTP. Este exemplo demonstra como escrever um aplicativo Hello World usando alguns **wrappers simples** em torno de bruto abstração OWIN e execute-o em um servidor web como o ASP.NET.

**Exemplo OWIN bruto Hello World** | [código-fonte](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)  
Este exemplo demonstra como escrever um aplicativo Hello World usando o **bruto** abstração OWIN e executá-lo em um servidor web como o Asp.Net.

**Exemplo de SignalR** | [código-fonte](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)  
Mostra como hospedar o SignalR usando OWIN / Katana. Para obter mais informações sobre hospedagem interna SignalR, consulte [Tutorial: SignalR auto-host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

**Exemplo de arquivos estáticos** | [código-fonte](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs)   
Mostra como dar suporte a solicitações HTTP para arquivos estáticos usando OWIN / Katana.

**Web API** | [código-fonte](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt)   
Este exemplo mostra como hospedar OWIN no IIS e adicionar a API da Web para o pipeline OWIN.

**Exemplo de soquete de Web** | [código-fonte](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs)   
Mostra como suportar soquetes da Web em OWIN usando o [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) classe.
