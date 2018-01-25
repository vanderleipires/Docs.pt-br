---
uid: signalr/overview/performance/scaleout-in-signalr
title: "Introdução à expansão no SignalR | Microsoft Docs"
author: MikeWasson
description: "Versões de software usados neste tópico Visual Studio 2013 .NET 4.5 SignalR versões anteriores de versão 2 deste tópico para obter informações sobre versões anteriores do..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 7e781fc1-1c1f-45a8-bc1d-338e96dbe9c9
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-in-signalr
msc.type: authoredcontent
ms.openlocfilehash: f1d15250682305f6d0512b72bd2e40cb4a8a18e5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="introduction-to-scaleout-in-signalr"></a>Introdução à expansão no SignalR
====================
por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

> ## <a name="software-versions-used-in-this-topic"></a>Versões de software usadas neste tópico
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR versão 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>Versões anteriores deste tópico
> 
> Para obter informações sobre versões anteriores do SignalR, consulte [versões mais antigas do SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Perguntas e comentários
> 
> Deixe comentários em como você gostou neste tutorial e o que podemos melhorar nos comentários na parte inferior da página. Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, você poderá postá-los para o [ASP.NET SignalR fórum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).


Em geral, há dois modos para dimensionar um aplicativo da web: *expandir* e *expansão*.

- Escala verticalmente significa que um servidor maior (ou com uma VM maior) mais RAM, CPUs, etc.
- Expandir significa adicionando mais servidores para tratar a carga.

O problema com o aumento de escala é que você rapidamente atingir um limite de tamanho da máquina. Além disso, é necessário expandir. No entanto, quando você expandir, os clientes podem obter roteados em servidores diferentes. Um cliente que está conectado a um servidor não receberá mensagens enviadas de outro servidor.

![](scaleout-in-signalr/_static/image1.png)

Uma solução é encaminhar mensagens entre servidores, usando um componente chamado um *backplane*. Com um backplane habilitado, cada instância de aplicativo envia mensagens ao backplane e backplane encaminha-os para as outras instâncias do aplicativo. (Em eletrônica, um backplane é um grupo de conectores paralelos. Por analogia, um backplane SignalR conecta vários servidores.)

![](scaleout-in-signalr/_static/image2.png)

O SignalR atualmente fornece três painéis de posteriores:

- **Barramento de serviço do Azure**. Barramento de serviço é uma infraestrutura de mensagens que permite que os componentes enviar mensagens de forma flexível.
- **Redis**. Redis é um repositório de chave-valor na memória. Redis oferece suporte a um padrão de publicação/assinatura ("pub/sub") para enviar mensagens.
- **SQL Server**. O backplane de SQL Server grava mensagens de tabelas SQL. Plano posterior usa o Service Broker para mensagens eficiente. No entanto, também funciona se o Service Broker não está habilitado.

Se você implantar seu aplicativo no Azure, considere o uso de backplane do Redis usando [Cache Redis do Azure](https://azure.microsoft.com/services/cache/). Se você estiver implantando ao farm de servidor, considere o SQL Server ou painéis posteriores do Redis.

Os tópicos a seguir contêm os tutoriais passo a passo para cada backplane:

- [Expansão do SignalR com o Barramento de Serviço do Azure](scaleout-with-windows-azure-service-bus.md)
- [Expansão do SignalR com Redis](scaleout-with-redis.md)
- [Expansão do SignalR com o SQL Server](scaleout-with-sql-server.md)

## <a name="implementation"></a>Implementação

No SignalR, cada mensagem é enviada por meio de um barramento de mensagem. Implementa um barramento de mensagem a [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) interface, que fornece uma abstração de publicação/assinatura. Os painéis posteriores trabalham substituindo o padrão **IMessageBus** com barramento projetado para que backplane. Por exemplo, o barramento de mensagem Redis é [RedisMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.redis.redismessagebus(v=vs.100).aspx), e usa o Redis [pub/sub](http://redis.io/topics/pubsub) mecanismo para enviar e receber mensagens.

Cada instância de servidor se conecta ao backplane por meio do barramento. Quando uma mensagem é enviada, ele passa para o backplane e backplane envia para todos os servidores. Quando um servidor recebe uma mensagem do backplane, ele coloca a mensagem em seu cache local. O servidor, em seguida, entrega mensagens para os clientes de seu cache local.

Para cada conexão de cliente, progresso do cliente ao ler o fluxo de mensagem é controlado usando um cursor. (Um cursor representa uma posição no fluxo de mensagens.) Se um cliente se desconecta e, em seguida, reconecta-se, ele solicita o barramento de quaisquer mensagens que chegaram após o valor de cursor do cliente. A mesma coisa acontece quando uma conexão usa [pesquisa longa](../getting-started/introduction-to-signalr.md#transports). Após a conclusão de uma solicitação de sondagem longa, o cliente abre uma nova conexão e pede para mensagens que chegaram após o cursor.

A cursor mecanismo funciona mesmo se um cliente é roteado para um servidor diferente no se reconectar. O backplane está ciente de todos os servidores e não importa qual servidor um cliente se conecta.

## <a name="limitations"></a>Limitações

Usando um backplane, a taxa de transferência máxima é menor do que quando os clientes se comunicar diretamente para um único nó do servidor. Isso ocorre porque o backplane encaminha todas as mensagens em cada nó para que o backplane pode se tornar um afunilamento. Se essa limitação é um problema depende do aplicativo. Por exemplo, aqui estão alguns cenários típicos de SignalR:

- [Difusão de servidor](../getting-started/tutorial-server-broadcast-with-signalr.md) (por exemplo, cotações da bolsa): painéis posteriores funcionam bem para este cenário, porque o servidor controla a taxa na qual as mensagens são enviadas.
- [Cliente-para-cliente](../getting-started/tutorial-getting-started-with-signalr.md) (por exemplo, bate-papo): nesse cenário, backplane pode ser um gargalo se o número de mensagens pode ser dimensionado com o número de clientes, ou seja, se aumentar a taxa de mensagens de clientes mais proporcionalmente como ingressar.
- [Em tempo real de alta frequência](../getting-started/tutorial-high-frequency-realtime-with-signalr.md) (por exemplo, jogos em tempo real): um backplane não é recomendado para este cenário.

## <a name="enabling-tracing-for-signalr-scaleout"></a>Habilitação do rastreamento de expansão do SignalR

Para habilitar o rastreamento para os painéis posteriores, adicione as seções a seguir ao arquivo Web. config, sob a raiz **configuração** elemento:

[!code-html[Main](scaleout-in-signalr/samples/sample1.html)]
