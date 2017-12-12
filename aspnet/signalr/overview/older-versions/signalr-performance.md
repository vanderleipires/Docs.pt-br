---
uid: signalr/overview/older-versions/signalr-performance
title: Desempenho do SignalR (SignalR 1. x) | Microsoft Docs
author: pfletcher
description: Desempenho do SignalR
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2013
ms.topic: article
ms.assetid: 9594d644-66b6-4223-acdd-23e29a6e4c46
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: 52052ad202958eb5d648ceb64d9f06fb86ef3777
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="signalr-performance-signalr-1x"></a>Desempenho do SignalR (SignalR 1. x)
====================
por [Patrick Fletcher](https://github.com/pfletcher)

> Este tópico descreve como criar, medir e melhorar o desempenho em um aplicativo do SignalR.


Para uma apresentação recente de dimensionamento e desempenho de SignalR, consulte [dimensionamento Web em tempo real com ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).

Esse tópico contém as seguintes seções:

- [Considerações de design](#design)
- [Ajuste de seu servidor do SignalR para desempenho](#tuning)
- [Solucionar problemas de desempenho](#troubleshooting)
- [Usando contadores de desempenho do SignalR](#perfcounters)
- [Usando outros contadores de desempenho](#othercounters)
- [Outros recursos](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a>Considerações de design

Esta seção descreve os padrões que podem ser implementados durante o design de um aplicativo de SignalR, para garantir que o desempenho não é sendo prejudicado por gerar tráfego de rede desnecessário.

### <a name="throttling-message-frequency"></a>Frequência de mensagem de limitação

Mesmo em um aplicativo que envia mensagens em uma frequência alta (como um aplicativo de jogos em tempo real), a maioria dos aplicativos não precisa enviar mais do que alguns mensagens por segundo. Para reduzir a quantidade de tráfego que cada cliente gera, um loop de mensagens pode ser implementado filas e envia as mensagens não mais frequentemente do que uma taxa fixa (ou seja, até um certo número de mensagens será enviado por segundo, se houver mensagens em que o tempo em intervalo a ser enviado). Para um aplicativo de exemplo que limita as mensagens para uma determinada taxa (de cliente e servidor), consulte [alta frequência em tempo real com SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).

### <a name="reducing-message-size"></a>Reduzindo o tamanho da mensagem

Você pode reduzir o tamanho de uma mensagem do SignalR, reduzindo o tamanho dos objetos serializados. No código do servidor, se você estiver enviando um objeto que contém propriedades que não precisam ser transmitido, impedir que as propriedades que está sendo serializado usando o `JsonIgnore` atributo. Os nomes das propriedades também são armazenados na mensagem; os nomes das propriedades podem ser reduzidos usando o `JsonProperty` atributo. O exemplo de código a seguir demonstra como excluir uma propriedade que está sendo enviado ao cliente e como reduzir os nomes de propriedade:

**Código do servidor .NET que demonstra o atributo JsonIgnore para excluir os dados sejam enviados para o cliente e o atributo JsonProperty para reduzir o tamanho de mensagem**

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

Para manter a legibilidade / facilidade de manutenção no código do cliente, os nomes de propriedade abreviada podem ser remapeados para humanos amigável nomes depois que a mensagem for recebida. O exemplo de código a seguir demonstra uma maneira possível de remapeamento reduzidos nomes para os mais longo, definindo um contrato de mensagem (mapeamento) e usando o `reMap` função para aplicar o contrato para a classe de mensagem otimizado:

**Código de JavaScript do lado do cliente que remapeia reduzido nomes de propriedade nomes legíveis**

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

Nomes podem ser reduzidos em mensagens do cliente para o servidor, usando o mesmo método.

Redução da superfície de memória (ou seja, a quantidade de memória usada para a mensagem) da mensagem de objeto também pode melhorar o desempenho. Por exemplo, se a gama completa de um `int` não é necessária uma `short` ou `byte` pode ser usado em vez disso.

Como as mensagens são armazenadas no barramento de mensagem na memória do servidor, reduzindo o tamanho de mensagens também pode resolver problemas de memória do servidor.

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a>Ajuste de seu servidor do SignalR para desempenho

As definições de configuração a seguir podem ser usadas para ajustar o servidor para melhorar o desempenho em um aplicativo do SignalR. Para obter informações gerais sobre como melhorar o desempenho em um aplicativo ASP.NET, consulte [melhorando o desempenho do ASP.NET](https://msdn.microsoft.com/en-us/library/ff647787.aspx).

**Definições de configuração de SignalR**

- **DefaultMessageBufferSize**: por padrão, o SignalR retém 1000 mensagens na memória por hub por conexão. Se estiverem sendo usada mensagens grandes, isso pode criar problemas de memória que podem ser atenuados por redução desse valor. Essa configuração pode ser definida `Application_Start` manipulador de eventos em um aplicativo ASP.NET ou no `Configuration` método de uma classe de inicialização OWIN em um aplicativo auto-hospedado. O exemplo a seguir demonstra como reduzir esse valor para reduzir o tamanho da memória do seu aplicativo para reduzir a quantidade de memória de servidor usada:

    **Código de servidor .NET em global. asax para diminuir o tamanho do buffer de mensagem padrão**

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

**Configurações do IIS**

- **Máximo de solicitações simultâneas por aplicativo**: aumentar o número de IIS simultâneo solicitações aumentará de recursos disponíveis para atender às solicitações do servidor. O valor padrão é 5000; para aumentar essa configuração, execute os seguintes comandos em um prompt de comando elevado:

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]

**Definições de configuração do ASP.NET**

Esta seção inclui configurações que podem ser definidas no `aspnet.config` arquivo. Esse arquivo está localizado em um dos dois locais, dependendo da plataforma:

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

Configurações do ASP.NET que podem melhorar o desempenho do SignalR incluem o seguinte:

- **Máximo de solicitações simultâneo por CPU**: aumentar essa configuração pode atenuar os gargalos de desempenho. Para aumentar essa configuração, adicione a seguinte configuração para o `aspnet.config` arquivo:

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- **Limite de fila de solicitações**: quando o número total de conexões excede o `maxConcurrentRequestsPerCPU` configuração, ASP.NET será iniciado usando uma fila de solicitações de limitação. Para aumentar o tamanho da fila, você pode aumentar a `requestQueueLimit` configuração. Para fazer isso, adicione a seguinte configuração para o `processModel` nó `config/machine.config` (em vez de `aspnet.config`):

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a>Solucionar problemas de desempenho

Esta seção descreve maneiras de localizar os afunilamentos de desempenho em seu aplicativo.

### <a name="verifying-that-websocket-is-being-used"></a>Verificando se o WebSocket está sendo usado

Enquanto o SignalR pode usar uma variedade de transportes para comunicação entre cliente e servidor, WebSocket oferece uma vantagem de desempenho significativa e deve ser usado se o cliente e o servidor oferecer suporte a ele. Para determinar se o cliente e o servidor atendem aos requisitos de WebSocket, consulte [transportes e Fallbacks](../getting-started/introduction-to-signalr.md#transports). Para determinar qual transporte está sendo usado em seu aplicativo, você pode usar as ferramentas de desenvolvedor do navegador e examine os logs para ver qual transporte está sendo usado para a conexão. Para obter informações sobre como usar as ferramentas de desenvolvimento de navegador no Internet Explorer e no Chrome, consulte [transportes e Fallbacks](../getting-started/introduction-to-signalr.md#transports).

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a>Usando contadores de desempenho do SignalR

Esta seção descreve como habilitar e usar contadores de desempenho do SignalR, encontrado no `Microsoft.AspNet.SignalR.Utils` pacote.

### <a name="installing-signalrexe"></a>Instalando signalr.exe

Contadores de desempenho podem ser adicionados ao servidor usando um utilitário chamado SignalR.exe. Para instalar este utilitário, siga estas etapas:

1. Em seu aplicativo do Visual Studio, selecione **ferramentas**, **Gerenciador de biblioteca de pacote**, **gerenciar pacotes NuGet para solução...**
2. Procurar **signalr.utils**e selecione instalar.

    ![](signalr-performance/_static/image1.png)
3. Aceite o contrato de licença para instalar o pacote.
4. Será instalado SignalR.exe `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.

### <a name="installing-performance-counters-with-signalrexe"></a>Instalando Contadores de desempenho com SignalR.exe

Para instalar contadores de desempenho do SignalR, execute SignalR.exe em um prompt de comando com o seguinte parâmetro:

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

Para remover contadores de desempenho do SignalR, execute SignalR.exe em um prompt de comando com o seguinte parâmetro:

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a>Contadores de desempenho do SignalR

O pacote de utilitários instala os seguintes contadores de desempenho. Os contadores de "Total" medem o número de eventos desde o último pool de aplicativos ou o servidor reiniciar.

**Métricas de Conexão**

As seguintes métricas de medem os eventos de tempo de vida de conexão que ocorrem. Para obter mais informações, consulte [compreensão e tratamento de eventos de tempo de vida de Conexão](../guide-to-the-api/handling-connection-lifetime-events.md).

- **Conexões conectadas**
- **Conexões reconectados**
- **Conexões Disonnected**
- **Conexões atuais**

**Métricas de mensagem**

As seguintes métricas de medem o tráfego de mensagens gerado pelo SignalR.

- **Total de mensagens recebidas de Conexão**
- **Total de mensagens de Conexão enviadas**
- **Conexão de mensagens recebidos/s**
- **Conexão de mensagens enviadas/s**

**Métricas do barramento de mensagem**

As seguintes métricas de medem o tráfego por meio do barramento de mensagem SignalR interno, a fila na qual todas as mensagens recebidas e enviadas para o SignalR são colocadas. Uma mensagem é **publicado** quando ele é enviado ou difusão. Um **assinante** neste contexto é uma assinatura do barramento de mensagem; isso deve ser igual ao número de clientes mais o próprio servidor. Um **trabalho alocado** é um componente que envia dados para as conexões ativas; um **trabalho ocupado** é aquele que está enviando uma mensagem ativamente.

- **Recebida Total de mensagens do barramento de mensagem**
- **Mensagens de barramento de mensagem recebidos/s**
- **Mensagens do barramento de mensagem publicado Total**
- **Barramento de mensagem mensagens publicadas/s**
- **Atual de assinantes do barramento de mensagem**
- **Total de assinantes do barramento de mensagem**
- **Os assinantes do barramento de mensagem/s**
- **Barramento de mensagem alocada trabalhadores**
- **Operadores ocupados de barramento de mensagem**
- **Atual de tópicos do barramento de mensagem**

**Métricas de erro**

As seguintes métricas de medem os erros gerados pelo tráfego de mensagens do SignalR. **Resolução de hub** erros ocorrem quando um hub ou um método de hub não pode ser resolvido. **Invocação de hub** erros são exceções geradas durante a invocação de um método de hub. **Transporte** erros são erros de conexão gerados durante uma solicitação ou resposta HTTP.

- **Erros: Total de todos os**
- **Erros: All/s**
- **Erros: Total de resolução de Hub**
- **Erros: Resolução de Hub/s**
- **Erros: Total de invocação de Hub**
- **Erros: Invocação de Hub / s**
- **Erros: Total de transporte**
- **Erros: Transporte/s**

**Métricas de expansão**

As seguintes métricas medem o tráfego e os erros gerados pelo provedor de expansão. Um **fluxo** neste contexto é uma unidade de escala usada pelo provedor de expansão; essa é uma tabela se o SQL Server é usado, um tópico que se o barramento de serviço é usado e uma assinatura se Redis for usado. Por padrão, apenas um fluxo é usado, mas pode ser aumentado por meio de configuração no SQL Server e o barramento de serviço. Um **buffer** fluxo é aquele que entrou em um estado de falha; quando o fluxo está no estado de falha, todas as mensagens enviadas ao backplane falhará imediatamente até que o fluxo não está falhando. O **comprimento da fila de envio** é o número de mensagens que foram lançadas, mas ainda não enviadas.

- **Barramento de mensagem escalável mensagens recebidos/s**
- **Total de fluxos de expansão**
- **Abrir de fluxos de expansão**
- **Fluxos de expansão de buffer**
- **Total de erros de expansão**
- **Erros de expansão/s**
- **Comprimento da fila de envio de expansão**

Para obter mais informações sobre o que está sendo medida esses contadores, consulte [SignalR Scaleout com o Azure Service Bus](scaleout-with-windows-azure-service-bus.md).

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a>Usando outros contadores de desempenho

Os seguintes contadores de desempenho também podem ser útil no monitoramento do desempenho do seu aplicativo.

**Memória**

- .NET CLR memória n º de bytes em todos os Heaps (para w3wp)

**ASP.NET**

- ASP.NET \ solicitações atuais
- ASP.NET\Queued
- ASP.NET\Rejected

**CPU**

- Tempo do processador Information\Processor

**TCP/IP**

- TCPv6/conexões estabelecidas
- TCPv4/conexões estabelecidas

**Serviço Web**

- Web \ conexões atuais
- Conexões de Service\Maximum da Web

**Threading**

- .NET CLR LocksAndThreads\# de segmentos lógicos atuais
- Threads do .NET CLR LocksAnd\# de segmentos físicos atuais

<a id="otherresources"></a>

## <a name="other-resources"></a>Outros recursos

Para obter mais informações sobre monitoramento e ajuste de desempenho do ASP.NET, consulte os tópicos a seguir:

- [Visão geral do desempenho do ASP.NET](https://msdn.microsoft.com/en-us/library/cc668225(v=vs.100).aspx)
- [Uso de Thread do ASP.NET no IIS 6.0, IIS 7.5 e IIS 7.0](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [&lt;applicationPool&gt; elemento (configurações da Web)](https://msdn.microsoft.com/en-us/library/dd560842.aspx)
