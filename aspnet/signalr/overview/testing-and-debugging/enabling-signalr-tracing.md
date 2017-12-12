---
uid: signalr/overview/testing-and-debugging/enabling-signalr-tracing
title: Habilitar o rastreamento do SignalR | Microsoft Docs
author: tfitzmac
description: "Este documento descreve como habilitar e configurar o rastreamento de clientes e servidores do SignalR. O rastreamento permite que você exiba informações sobre eventos de diagnóstico..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/08/2014
ms.topic: article
ms.assetid: 30060acb-be3e-4347-996f-3870f0c37829
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/testing-and-debugging/enabling-signalr-tracing
msc.type: authoredcontent
ms.openlocfilehash: 2f01ab5d66e44cd82634f1b3df1ca6c78b7fd9d5
ms.sourcegitcommit: c07fb5cb5df0a12f9fe6735fcbc90964608fa687
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/14/2017
---
<a name="enabling-signalr-tracing"></a>Habilitar o rastreamento do SignalR
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este documento descreve como habilitar e configurar o rastreamento de clientes e servidores do SignalR. Rastreamento permite que você exiba informações de diagnóstico sobre eventos em seu aplicativo do SignalR.
> 
> Este tópico foi criado originalmente por Patrick Fletcher.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET Framework 4.5
> - SignalR versão 2
>   
> 
> 
> ## <a name="questions-and-comments"></a>Perguntas e comentários
> 
> Deixe comentários em como você gostou neste tutorial e o que podemos melhorar nos comentários na parte inferior da página. Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, você poderá postá-los para o [ASP.NET SignalR fórum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).


Quando o rastreamento estiver habilitado, um aplicativo SignalR cria entradas de log de eventos. Você pode registrar eventos do cliente e do servidor. O rastreamento na conexão de logs do servidor, o provedor de expansão e eventos de barramento de mensagem. Os eventos de conexão do cliente logs de rastreamento. No SignalR 2.1 e posterior, o rastreamento no cliente registra todo o conteúdo das mensagens de invocação de hub.

## <a name="contents"></a>Conteúdo

- [Ativar o rastreamento no servidor](#server)

    - [Log de eventos do servidor de arquivos de texto](#server_text)
    - [Log de eventos do servidor para o log de eventos](#server_eventlog)
- [Ativar o rastreamento no cliente .NET (aplicativos da área de trabalho do Windows)](#net_client)

    - [Log de eventos do cliente de área de trabalho para o console](#desktop_console)
    - [Log de eventos do cliente de área de trabalho para um arquivo de texto](#desktop_text)
- [Ativar o rastreamento em clientes do Windows Phone 8](#phone)

    - [Log de eventos do cliente do Windows Phone para a interface do usuário](#phone_ui)
    - [Log de eventos do cliente do Windows Phone para o console de depuração](#phone_debug)
- [Ativar o rastreamento no cliente de JavaScript](#javascript)

<a id="server"></a>
## <a name="enabling-tracing-on-the-server"></a>Ativar o rastreamento no servidor

Habilitar o rastreamento no servidor no arquivo de configuração do aplicativo (App. config ou Web. config, dependendo do tipo de projeto.) Você especificar quais categorias de eventos que você deseja registrar. No arquivo de configuração, você também especificar se deseja registrar os eventos para um arquivo de texto, o log de eventos do Windows ou um log personalizado usando uma implementação do [TraceListener](https://msdn.microsoft.com/en-us/library/system.diagnostics.tracelistener(v=vs.110).aspx).

As categorias de evento de servidor incluem os seguintes tipos de mensagens:

| Origem | Mensagens |
| --- | --- |
| SignalR.SqlMessageBus | Instalação do provedor de expansão de barramento de mensagem do SQL, a operação de banco de dados, erros e eventos de tempo limite |
| SignalR.ServiceBusMessageBus | Criação do Service bus expansão provedor tópico e assinatura, erro e mensagens de eventos |
| SignalR.RedisMessageBus | Eventos de erro de conexão e desconexão do provedor de expansão de redis |
| SignalR.ScaleoutMessageBus | Eventos de mensagens de expansão |
| SignalR.Transports.WebSocketTransport | Eventos de conexão, desconexão, mensagens e erros de transporte de WebSocket |
| SignalR.Transports.ServerSentEventsTransport | Eventos de conexão, desconexão, mensagens e erros de transporte ServerSentEvents |
| SignalR.Transports.ForeverFrameTransport | Eventos de conexão, desconexão, mensagens e erros de transporte ForeverFrame |
| SignalR.Transports.LongPollingTransport | Eventos de conexão, desconexão, mensagens e erros de transporte LongPolling |
| SignalR.Transports.TransportHeartBeat | Eventos de keepalive, conexão e desconexão de transporte |
| SignalR.ReflectedHubDescriptorProvider | Eventos do hub de descoberta |

<a id="server_text"></a>
### <a name="logging-server-events-to-text-files"></a>Log de eventos do servidor de arquivos de texto

O código a seguir mostra como habilitar o rastreamento para cada categoria de evento. Este exemplo configura o aplicativo para registrar eventos em arquivos de texto.

**Código do servidor XML para ativar o rastreamento**

[!code-html[Main](enabling-signalr-tracing/samples/sample1.html)]

No código acima, o `SignalRSwitch` entrada especifica o [TraceLevel](https://msdn.microsoft.com/en-us/library/system.diagnostics.tracelevel(v=vs.110).aspx) usado para eventos enviados para o log especificado. Nesse caso, ele é definido como `Verbose` que significa que todas as de depuração e rastreamento de mensagens são registradas.

A saída a seguir mostra as entradas do `transports.log.txt` arquivo para um aplicativo usando o arquivo de configuração acima. Mostra uma nova conexão, uma conexão removida e eventos de pulsação do transporte.

[!code-console[Main](enabling-signalr-tracing/samples/sample2.cmd)]

<a id="server_eventlog"></a>
### <a name="logging-server-events-to-the-event-log"></a>Log de eventos do servidor para o log de eventos

Para registrar eventos no log de eventos em vez de um arquivo de texto, altere os valores para as entradas na `sharedListeners` nó. O código a seguir mostra como registrar eventos de servidor para o log de eventos:

**Código de servidor XML para registrar eventos no log de eventos**

[!code-xml[Main](enabling-signalr-tracing/samples/sample3.xml)]

Os eventos são registrados no log do aplicativo e estão disponíveis por meio do Visualizador de eventos, como mostrado abaixo:

![Mostrando SignalR logs do Visualizador de eventos](enabling-signalr-tracing/_static/image1.png)

> [!NOTE]
> Ao usar o log de eventos, defina o **TraceLevel** para **erro** para manter o número de mensagens gerenciáveis.

<a id="net_client"></a>
## <a name="enabling-tracing-in-the-net-client-windows-desktop-apps"></a>Ativar o rastreamento no cliente .NET (aplicativos da área de trabalho do Windows)

O cliente .NET pode registrar eventos no console, um arquivo de texto, ou em um log personalizado usando uma implementação do [TextWriter](https://msdn.microsoft.com/en-us/library/system.io.textwriter.aspx).

Para habilitar o registro em log no cliente .NET, defina a conexão `TraceLevel` propriedade para um [TraceLevels](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) valor e o `TraceWriter` propriedade válido [TextWriter](https://msdn.microsoft.com/en-us/library/system.io.textwriter.aspx) instância.

<a id="desktop_console"></a>
### <a name="logging-desktop-client-events-to-the-console"></a>Log de eventos do cliente de área de trabalho para o console

O código c# a seguir mostra como registrar eventos no cliente do .NET no console:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample4.cs?highlight=2-3)]

<a id="desktop_text"></a>
### <a name="logging-desktop-client-events-to-a-text-file"></a>Log de eventos do cliente de área de trabalho para um arquivo de texto

O código c# a seguir mostra como registrar eventos no cliente do .NET em um arquivo de texto:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample5.cs?highlight=4-5)]

A saída a seguir mostra as entradas do `ClientLog.txt` arquivo para um aplicativo usando o arquivo de configuração acima. Ele mostra que o cliente se conectar ao servidor e o hub invocando um método de cliente chamado `addMessage`:

[!code-console[Main](enabling-signalr-tracing/samples/sample6.cmd)]

<a id="phone"></a>
## <a name="enabling-tracing-in-windows-phone-8-clients"></a>Ativar o rastreamento em clientes do Windows Phone 8

Os aplicativos SignalR para aplicativos do Windows Phone usam o mesmo cliente .NET como aplicativos de desktop, mas [Console.Out](https://msdn.microsoft.com/en-us/library/system.console.out(v=vs.110).aspx) e gravar em um arquivo com [StreamWriter](https://msdn.microsoft.com/en-us/library/system.io.streamwriter(v=vs.110).aspx) não estão disponíveis. Em vez disso, você precisa criar uma implementação personalizada de [TextWriter](https://msdn.microsoft.com/en-us/library/system.io.textwriter(v=vs.110).aspx) para rastreamento. 

<a id="phone_ui"></a>
### <a name="logging-windows-phone-client-events-to-the-ui"></a>Log de eventos do cliente do Windows Phone para a interface do usuário

O [SignalR codebase](https://github.com/SignalR/SignalR/archive/master.zip) inclui um exemplo do Windows Phone que grava a saída de rastreamento para uma [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) usando um personalizado [TextWriter](https://msdn.microsoft.com/en-us/library/system.io.textwriter(v=vs.110).aspx) implementação chamado `TextBlockWriter`. Essa classe pode ser encontrada no **samples/Microsoft.AspNet.SignalR.Client.WP8.Samples** projeto. Ao criar uma instância de `TextBlockWriter`, passar atual [SynchronizationContext](https://msdn.microsoft.com/en-us/library/system.threading.synchronizationcontext(v=vs.110).aspx)e um [StackPanel](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) onde ele criará uma [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) a ser usado para rastreamento saída:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample7.cs)]

A saída do rastreamento, em seguida, será gravada para uma nova [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) criado na [StackPanel](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) passado em:

![](enabling-signalr-tracing/_static/image2.png)

<a id="phone_debug"></a>
### <a name="logging-windows-phone-client-events-to-the-debug-console"></a>Log de eventos do cliente do Windows Phone para o console de depuração

Para enviar a saída para o console de depuração em vez de interface do usuário, crie uma implementação de [TextWriter](https://msdn.microsoft.com/en-us/library/system.io.textwriter(v=vs.110).aspx) que grava a janela de depuração e atribuí-lo a sua conexão [TraceWriter](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) propriedade:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample8.cs)]

Informações de rastreamento, em seguida, serão gravadas na janela de depuração no Visual Studio:

![](enabling-signalr-tracing/_static/image3.png)

<a id="javascript"></a>
## <a name="enabling-tracing-in-the-javascript-client"></a>Ativar o rastreamento no cliente de JavaScript

Para habilitar o registro de cliente em uma conexão, defina o `logging` propriedade no objeto de conexão antes de chamar o `start` método para estabelecer a conexão.

**Código de JavaScript do cliente para habilitar o rastreamento para o console do navegador (com o proxy gerado)**

[!code-javascript[Main](enabling-signalr-tracing/samples/sample9.js?highlight=1)]

**Código de JavaScript do cliente para habilitar o rastreamento para o console do navegador (sem o proxy gerado)**

[!code-javascript[Main](enabling-signalr-tracing/samples/sample10.js?highlight=2)]

Quando o rastreamento estiver habilitado, o cliente JavaScript registra eventos no console do navegador. Para acessar o console do navegador, consulte [monitoramento transportes](../getting-started/introduction-to-signalr.md#MonitoringTransports).

Captura de tela a seguir mostra um cliente SignalR JavaScript com o rastreamento ativado. Ele mostra a conexão e eventos de invocação de hub no console do navegador:

![Eventos de rastreamento do SignalR no console do navegador](enabling-signalr-tracing/_static/image4.png)
