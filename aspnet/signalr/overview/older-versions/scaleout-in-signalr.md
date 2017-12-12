---
uid: signalr/overview/older-versions/scaleout-in-signalr
title: "Introdução à expansão no SignalR 1. x | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/29/2013
ms.topic: article
ms.assetid: 3fd9f11c-799b-4001-bd60-1e70cfc61c19
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/scaleout-in-signalr
msc.type: authoredcontent
ms.openlocfilehash: e6230d4d65adb8c9a064545ad761898ca53562bf
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="introduction-to-scaleout-in-signalr-1x"></a>Introdução à expansão no SignalR 1. x
====================
por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

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

Se você implantar seu aplicativo no Azure, considere o uso do backplane do barramento de serviço do Azure. Se você estiver implantando ao farm de servidor, considere o SQL Server ou painéis posteriores do Redis.

Os tópicos a seguir contêm os tutoriais passo a passo para cada backplane:

- [Expansão do SignalR com o barramento de serviço do Azure](scaleout-with-windows-azure-service-bus.md)
- [Expansão do SignalR com Redis](scaleout-with-redis.md)
- [Expansão do SignalR com o SQL Server](scaleout-with-sql-server.md)

## <a name="implementation"></a>Implementação

No SignalR, cada mensagem é enviada por meio de um barramento de mensagem. Implementa um barramento de mensagem a [IMessageBus](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) interface, que fornece uma abstração de publicação/assinatura. Os painéis posteriores trabalham substituindo o padrão **IMessageBus** com barramento projetado para que backplane. Por exemplo, o barramento de mensagem Redis é [RedisMessageBus](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.redis.redismessagebus(v=vs.100).aspx), e usa o Redis [pub/sub](http://redis.io/topics/pubsub) mecanismo para enviar e receber mensagens.

Cada instância de servidor se conecta ao backplane por meio do barramento. Quando uma mensagem é enviada, ele passa para o backplane e backplane envia para todos os servidores. Quando um servidor recebe uma mensagem do backplane, ele coloca a mensagem em seu cache local. O servidor, em seguida, entrega mensagens para os clientes de seu cache local.

Para cada conexão de cliente, progresso do cliente ao ler o fluxo de mensagem é controlado usando um cursor. (Um cursor representa uma posição no fluxo de mensagens.) Se um cliente se desconecta e, em seguida, reconecta-se, ele solicita o barramento de quaisquer mensagens que chegaram após o valor de cursor do cliente. A mesma coisa acontece quando uma conexão usa [pesquisa longa](../getting-started/introduction-to-signalr.md#transports). Após a conclusão de uma solicitação de sondagem longa, o cliente abre uma nova conexão e pede para mensagens que chegaram após o cursor.

A cursor mecanismo funciona mesmo se um cliente é roteado para um servidor diferente no se reconectar. O backplane está ciente de todos os servidores e não importa qual servidor um cliente se conecta.

## <a name="limitations"></a>Limitações

Usando um backplane, a taxa de transferência máxima é menor do que quando os clientes se comunicar diretamente para um único nó do servidor. Isso ocorre porque o backplane encaminha todas as mensagens em cada nó para que o backplane pode se tornar um afunilamento. Se essa limitação é um problema depende do aplicativo. Por exemplo, aqui estão alguns cenários típicos de SignalR:

- [Difusão de servidor](tutorial-server-broadcast-with-aspnet-signalr.md) (por exemplo, cotações da bolsa): painéis posteriores funcionam bem para este cenário, porque o servidor controla a taxa na qual as mensagens são enviadas.
- [Cliente-para-cliente](tutorial-getting-started-with-signalr.md) (por exemplo, bate-papo): nesse cenário, backplane pode ser um gargalo se o número de mensagens pode ser dimensionado com o número de clientes, ou seja, se aumentar a taxa de mensagens de clientes mais proporcionalmente como ingressar.
- [Em tempo real de alta frequência](tutorial-high-frequency-realtime-with-signalr.md) (por exemplo, jogos em tempo real): um backplane não é recomendado para este cenário.

## <a name="enabling-tracing-for-signalr-scaleout"></a>Habilitação do rastreamento de expansão do SignalR

Para habilitar o rastreamento para os painéis posteriores, adicione as seções a seguir ao arquivo Web. config, sob a raiz **configuração** elemento:

[!code-html[Main](scaleout-in-signalr/samples/sample1.html)]
