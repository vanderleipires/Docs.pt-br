---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Exemplos de Katana | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2014
ms.topic: article
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
ms.technology: ''
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: e81da1e650d8dfd24a3e0fda6aa42b7f360ce12d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393313"
---
<a name="katana-samples"></a>Exemplos de Katana
====================
por [Microsoft](https://github.com/microsoft)

## <a name="katana-samples"></a>Exemplos de Katana

**Exemplo de rotas do ASP.NET** | [código-fonte](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)  
Em alguns aplicativos você deseja conectar componentes do OWIN na tabela de rotas do Asp.Net lado a lado com componentes não OWIN. Este exemplo mostra como usar os métodos de extensão RouteCollection MapOwinPath e MapOwinRoute fornecidos pelo systemweb.

**A ramificação Pipelines de exemplo** | [código-fonte](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)  
Pipelines de processamento de solicitação OWIN não precisa ser lineares, eles podem ser ramificados para processar solicitações de maneiras diferentes. Este exemplo mostra como construir um pipeline de ramificação com base em caminhos de solicitação ou outros dados de solicitação, como cabeçalhos. Esses componentes estão disponíveis no pacote nuget Microsoft.Owin.Mapping.

**Exemplo de servidor personalizado** | [código-fonte](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs)   
Mostra como usar um servidor OWIN personalizado quando a auto-hospedagem de OWIN.

**Inserido amostra** | [código-fonte](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)  
Alguns servidores do OWIN podem ser executados em seu próprio processo (&quot;auto-hospedado&quot;). Este exemplo mostra como iniciar um aplicativo do OWIN usando as ferramentas fornecidas pelo pacote nuget Hosting.

**Exemplo HelloWorld** | [código-fonte](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)  
OWIN é um abstração de API que permite a portabilidade do aplicativo em vários servidores do servidor HTTP. Este exemplo demonstra como escrever um aplicativo Olá, mundo usando alguns **invólucros simples** em torno de brutos abstração do OWIN e execute-o em um servidor web, como ASP.NET.

**Exemplo do Hello World OWIN brutos** | [código-fonte](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)  
Este exemplo demonstra como escrever um aplicativo Hello World usando o **brutos** abstração do OWIN e executá-lo em um servidor web, como o Asp.Net.

**Exemplo de SignalR** | [código-fonte](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)  
Mostra como hospedar internamente o SignalR para usar o OWIN / Katana. Para obter mais informações sobre o SignalR hospedagem interna, consulte [Tutorial: auto-hospedar SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

**Exemplo de arquivos estáticos** | [código-fonte](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs)   
Mostra como dar suporte a solicitações HTTP para arquivos estáticos, usar o OWIN / Katana.

**API Web** | [código-fonte](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt)   
Este exemplo mostra como hospedar OWIN no IIS e adicionar a API da Web para o pipeline OWIN.

**Web de exemplo de soquete** | [código-fonte](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs)   
Mostra como dar suporte a soquetes da Web em OWIN usando o [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) classe.
