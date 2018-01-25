---
uid: signalr/overview/guide-to-the-api/handling-connection-lifetime-events
title: "Compreensão e tratamento de eventos de tempo de vida de Conexão no SignalR | Microsoft Docs"
author: pfletcher
description: Este artigo descreve como usar os eventos expostos pela API de Hubs.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 03960de2-8d95-4444-9169-4426dcc64913
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/handling-connection-lifetime-events
msc.type: authoredcontent
ms.openlocfilehash: 2fd9cafd8d7706807998793c3c39377fe9604266
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="understanding-and-handling-connection-lifetime-events-in-signalr"></a>Compreensão e tratamento de eventos de tempo de vida de Conexão no SignalR
====================
por [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

> Este artigo fornece uma visão geral de como os eventos de conexão, a reconexão e a desconexão de SignalR que você pode manipular e as configurações de tempo limite e keepalive que você pode configurar.
> 
> O artigo supõe que você já tiver algum conhecimento dos eventos de tempo de vida do SignalR e conexão. Para obter uma introdução ao SignalR, consulte [Introdução ao SignalR](../getting-started/introduction-to-signalr.md). Para listas de eventos de tempo de vida da conexão, consulte os seguintes recursos:
> 
> - [Como manipular eventos de tempo de vida da conexão na classe Hub](hubs-api-guide-server.md#connectionlifetime)
> - [Como manipular eventos de tempo de vida da conexão em clientes de JavaScript](hubs-api-guide-javascript-client.md#connectionlifetime)
> - [Como manipular eventos de tempo de vida de conexão de clientes .NET](hubs-api-guide-net-client.md#connectionlifetime)
> 
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


## <a name="overview"></a>Visão geral

Este artigo contém as seguintes seções:

- [Cenários e terminologia de tempo de vida da Conexão](#terminology)

    - [Conexões SignalR, conexões de transporte e conexões físicas](#signalrvstransport)
    - [Cenários de desconexão de transporte](#transportdisconnect)
    - [Cenários de desconexão do cliente](#clientdisconnect)
    - [Cenários de desconexão do servidor](#serverdisconnect)
- [Configurações de tempo limite e keepalive](#timeoutkeepalive)

    - [ConnectionTimeout](#connectiontimeout)
    - [DisconnectTimeout](#disconnecttimeout)
    - [KeepAlive](#keepalive)
    - [Como alterar as configurações de tempo limite e keepalive](#changetimeout)
- [Como notificar o usuário sobre desconexões](#notifydisconnect)
- [Como reconectar continuamente](#continuousreconnect)
- [Como desconectar um cliente no código do servidor](#disconnectclientfromserver)
- [Detectando o motivo de uma desconexão](#detectingreasonfordisconnection)

Links para tópicos de referência de API são para a versão do .NET 4.5 da API. Se você estiver usando o .NET 4, consulte [a versão do .NET 4 dos tópicos da API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="terminology"></a>

## <a name="connection-lifetime-terminology-and-scenarios"></a>Cenários e terminologia de tempo de vida da Conexão

O `OnReconnected` manipulador de eventos em um SignalR Hub pode executar diretamente após `OnConnected` , mas não após `OnDisconnected` para um determinado cliente. O motivo pelo qual que você pode ter uma reconexão sem uma desconexão é que há várias maneiras em que a palavra "conexão" é usada no SignalR.

<a id="signalrvstransport"></a>

### <a name="signalr-connections-transport-connections-and-physical-connections"></a>Conexões SignalR, conexões de transporte e conexões físicas

Este artigo diferenciará *conexões SignalR*, *conexões de transporte*, e *conexões físicas*:

- **Conexão do SignalR** refere-se a uma relação lógica entre um cliente e uma URL de servidor, mantido pela API do SignalR e identificada exclusivamente por uma ID de conexão. Os dados sobre essa relação são mantidos pelo SignalR e são usados para estabelecer uma conexão de transporte. Os fins de relação e SignalR descarta os dados quando o cliente chama o `Stop` método ou um tempo limite é atingido enquanto o SignalR tentando restabelecer uma conexão de transporte perdida.
- **Conexão de transporte** refere-se a uma relação lógica entre um cliente e um servidor, mantidas por um dos quatro transporte APIs: continuamente WebSockets, eventos enviados pelo servidor, quadro ou longa de sondagem. O SignalR usa o transporte de API para criar uma conexão de transporte e a API de transporte depende da existência de uma conexão de rede física para criar a conexão de transporte. A conexão de transporte termina quando SignalR termina ou quando o transporte API detecta que a conexão física é interrompida.
- **Conexão física** refere-se os links de rede física – fios, sinais sem fio, roteadores, etc. – que facilitam a comunicação entre um computador cliente e um computador do servidor. A conexão física deve estar presente para estabelecer uma conexão de transporte e é necessário estabelecer uma conexão de transporte para estabelecer uma conexão SignalR. No entanto, interrompendo a conexão física não sempre imediatamente termina a conexão de transporte ou conexão SignalR, conforme será explicado mais adiante neste tópico.

No diagrama a seguir, a conexão do SignalR é representado pela API de Hubs e camada PersistentConnection API SignalR, a conexão de transporte é representado pela camada de transportes e a conexão física é representada por linhas entre o servidor e os clientes.

![Diagrama de arquitetura de SignalR](handling-connection-lifetime-events/_static/image1.png)

Quando você chama o `Start` método em um cliente SignalR, você está fornecendo o código do cliente SignalR com todas as informações necessárias para estabelecer uma conexão física em um servidor. Código do cliente SignalR usa essas informações para fazer uma solicitação HTTP e estabelecer uma conexão física que usa um dos métodos de quatro transporte. Se a conexão de transporte falha ou o servidor falhar, a conexão do SignalR não desaparece imediatamente porque o cliente ainda tem as informações necessárias para restabelecer automaticamente uma nova conexão de transporte para a mesma URL SignalR. Nesse cenário, nenhuma intervenção por parte do aplicativo do usuário está envolvida e quando o código de cliente SignalR estabelece uma nova conexão de transporte, ele não inicia uma nova conexão do SignalR. A continuidade da conexão do SignalR é refletida no fato de que a ID de conexão, que é criada quando você chamar o `Start` método, não é alterado.

O `OnReconnected` manipulador de eventos no Hub é executado quando uma conexão de transporte é restabelecida automaticamente depois de ter sido perdidos. O `OnDisconnected` manipulador de eventos é executado no final de uma conexão SignalR. Uma conexão SignalR pode terminar em qualquer uma das seguintes maneiras:

- Se o cliente chama o `Stop` método, uma mensagem de parada é enviada ao servidor e cliente e servidor encerrar a conexão SignalR imediatamente.
- Depois que a conectividade entre cliente e servidor é perdida, o cliente tenta se reconectar e o servidor espera para o cliente para se reconectar. Se as tentativas de reconexão não forem bem-sucedidas e termina o período de tempo limite de desconexão, cliente e servidor encerrar a conexão do SignalR. O cliente para tentar reconectar-se e descarta o servidor de sua representação de conexão do SignalR.
- Se o cliente deixará de ser executado sem ter a chance de chamar o `Stop` método, o servidor aguarda para reconectar-se, o cliente e, em seguida, termina a conexão SignalR após o período de tempo limite de desconexão.
- Se o servidor parar a execução, o cliente tenta se reconectar (crie novamente a conexão de transporte) e, em seguida, termina a conexão SignalR após o período de tempo limite de desconexão.

Quando não há nenhum problema de conexão, e o aplicativo de usuário termina a conexão do SignalR chamando o `Stop` método, a conexão do SignalR e a conexão de transporte começam e terminam em aproximadamente ao mesmo tempo. As seções a seguir descrevem em mais detalhes os outros cenários.

<a id="transportdisconnect"></a>

### <a name="transport-disconnection-scenarios"></a>Cenários de desconexão de transporte

Conexões físicas podem ser lentas ou talvez haja interrupções na conectividade. Dependendo de fatores como o comprimento da interrupção, a conexão de transporte pode ser descartado. O SignalR, em seguida, tenta restabelecer a conexão de transporte. Às vezes, a conexão de transporte API detecta a interrupção e cancela a conexão de transporte e SignalR descobre imediatamente que a conexão for perdida. Em outros cenários, nem a conexão de transporte API nem SignalR torna-se com reconhecimento de imediatamente que a conectividade foi perdida. Para todos os transportes exceto sondagem longa, o cliente SignalR usa uma função chamada *keepalive* para verificar se há perda de conectividade com o API de transporte é capaz de detectar. Para obter informações sobre conexões de sondagem longa, consulte [as configurações de tempo limite e keepalive](#timeoutkeepalive) mais adiante neste tópico.

Quando uma conexão estiver inativo, periodicamente o servidor envia um pacote keepalive ao cliente. A partir da data em que este artigo está sendo gravado, a frequência padrão é a cada 10 segundos. Monitorando o esses pacotes, os clientes podem informar se há um problema de conexão. Se um pacote keepalive não for recebido quando se espera que, após um curto período de tempo o cliente presume que existem problemas de conexão como lentidão ou interrupções. Se o keepalive ainda não foram recebido após um tempo mais longo, o cliente presume que a conexão foi descartada, e ele começa a tentar se reconectar.

O diagrama a seguir ilustra os eventos de cliente e servidor que são gerados em um cenário típico, quando há problemas com a conexão física que não são reconhecidos imediatamente pelo transporte API. O diagrama se aplica aos seguintes circunstâncias:

- O transporte é WebSockets, sempre quadro ou eventos enviados pelo servidor.
- Há vários pontos de interrupção na conexão de rede física.
- O API de transporte não fica ciente de interrupções, então SignalR se baseia na funcionalidade keepalive para detectá-los.

![Desconexões de transporte](handling-connection-lifetime-events/_static/image2.png)

Se o cliente entra em modo reconectar-se, mas não é possível estabelecer uma conexão de transporte dentro do limite de tempo limite de desconexão, o servidor encerra a conexão do SignalR. Quando isso acontece, o servidor executa o Hub `OnDisconnected` método e filas de uma mensagem de desconexão para enviar ao cliente caso o cliente gerencia conectar mais tarde. Se o cliente, em seguida, reconectar, ele recebe o comando de desconexão e chama o `Stop` método. Nesse cenário, `OnReconnected` não é executado quando o cliente se reconecta, e `OnDisconnected` não é executado quando o cliente chama `Stop`. O diagrama a seguir ilustra esse cenário.

![Interrupções de transporte - tempo limite do servidor](handling-connection-lifetime-events/_static/image3.png)

Os eventos de tempo de vida de conexão SignalR que podem ser gerados no cliente são os seguintes:

- `ConnectionSlow`evento de cliente.

    Gerado quando uma predefinição proporção do período de tempo limite de keepalive passou desde a última mensagem ou keepalive ping foi recebida. O período de aviso de tempo limite de keepalive padrão é 2/3 do tempo limite keepalive. O tempo limite de keepalive é 20 segundos, para que o aviso é exibido em aproximadamente 13 segundos.

    Por padrão, o servidor envia pings keepalive a cada 10 segundos e o cliente verifica os pings de keepalive sobre cada 2 segundos (um terço da diferença entre o valor de tempo limite de manutenção de atividade e o valor de aviso de tempo limite de manutenção de atividade).

    Se o transporte API ficar atento a desconexão, SignalR pode ser informado sobre a desconexão antes do período de aviso de tempo limite de keepalive passa. Nesse caso, o `ConnectionSlow` evento não será gerado e SignalR deve ir diretamente para o `Reconnecting` evento.
- `Reconnecting`evento de cliente.

    Gerado quando o (a) o transporte API detecta que a conexão for perdida, ou (b) o período de tempo limite de keepalive passou desde a última mensagem ou keepalive ping foi recebida. O código de cliente SignalR começa a tentar se reconectar. Você pode manipular esse evento se você deseja que seu aplicativo para executar alguma ação quando uma conexão de transporte é perdida. Atualmente, o período de tempo limite de keepalive padrão é 20 segundos.

    Se seu código de cliente tentar chamar um método de Hub enquanto SignalR está em modo reconectar-se, SignalR tentará enviar o comando. Na maioria das vezes, essas tentativas falhará, mas em algumas circunstâncias que talvez tenham êxito. Para os eventos enviados pelo servidor, para sempre quadro e transportes de sondagem longa, o SignalR usa dois canais de comunicação que o cliente usa para enviar mensagens e que ele usa para receber mensagens. O canal usado para receber é aquele permanentemente aberta, e que é o que é fechada quando a conexão física é interrompido. O canal usado para enviar permanece disponível, para que se a conectividade física é restaurada, uma chamada de método de cliente para servidor pode ser bem-sucedida antes do canal de recebimento é restabelecido. O valor de retorno não seria recebido SignalR abrir novamente o canal usado para recebimento.
- `Reconnected`evento de cliente.

    Gerado quando a conexão de transporte é restabelecida. O `OnReconnected` executa do manipulador de eventos no Hub.
- `Closed`evento de cliente (`disconnected` eventos em JavaScript).

    Gerado quando o período de tempo limite de desconexão expira enquanto o código de cliente SignalR está tentando reconectar depois de perder a conexão de transporte. Desconecte o padrão é de 30 segundos do tempo limite. (Esse evento também é gerado quando a conexão é encerrada porque o `Stop` método é chamado.)

Interrupções de conexão de transporte que não são detectadas pelo transporte API e não atrasar a recepção de pings de manutenção de atividade do servidor por mais do que o período de aviso de tempo limite de keepalive não podem causar qualquer conexão eventos de tempo de vida a ser gerado.

Alguns ambientes de rede deliberadamente fechar conexões ociosas e outra função dos pacotes keepalive é evitar isso, permitindo que essas redes sabe que uma conexão SignalR está em uso. Em casos extremos frequência padrão de pings keepalive pode não ser suficiente para impedir conexões fechadas. Nesse caso, você pode configurar keepalive pings para serem enviadas com mais frequência. Para obter mais informações, consulte [as configurações de tempo limite e keepalive](#timeoutkeepalive) mais adiante neste tópico.

> [!NOTE] 
> 
> **Importante**: A sequência de eventos descritas aqui não é garantida. SignalR torna cada tentativa de gerar eventos de tempo de vida de conexão de maneira previsível de acordo com esse esquema, mas há muitas variações dos eventos de rede e muitas maneiras em que estruturas de comunicação subjacente como transporte APIs tratá-los. Por exemplo, o `Reconnected` evento pode não ser gerado quando o cliente se reconecta, ou o `OnConnected` manipulador no servidor pode ser executado quando a tentativa de estabelecer uma conexão for bem-sucedida. Este tópico descreve apenas os efeitos que normalmente seriam produzidos por certas circunstâncias normais.


<a id="clientdisconnect"></a>

### <a name="client-disconnection-scenarios"></a>Cenários de desconexão do cliente

Em um cliente de navegador, o código de cliente SignalR que mantém uma conexão SignalR é executado no contexto de uma página da web JavaScript. Que tem por que a conexão do SignalR tem que terminar quando você navegar de uma página para outra e esse por que você tiver várias conexões com conexão várias IDs se você se conectar de várias janelas do navegador ou guias. Quando o usuário fecha uma janela ou guia navegador, ou navega para uma nova página ou atualiza a página, a conexão do SignalR encerra imediatamente porque o código de cliente SignalR manipula esse evento de navegador para você e chama o `Stop` método. Nesses cenários, ou em qualquer plataforma de cliente quando o aplicativo chama o `Stop` método, o `OnDisconnected` manipulador de eventos imediatamente executa no servidor e o cliente gera o `Closed` evento (o evento é chamado `disconnected` em JavaScript).

Se um aplicativo cliente ou do computador que estiver executando no falha ou entra em suspensão (por exemplo, quando o usuário fecha o laptop), o servidor não é informado sobre o que aconteceu. Quanto o servidor sabe, a perda do cliente pode ser devido a interrupção da conectividade e o cliente pode estar tentando se reconectar. Portanto, nesses cenários em que o servidor espera para dar uma chance de reconectar-se, de cliente e `OnDisconnected` não é executado até que o período de tempo limite de desconexão expira (cerca de 30 segundos por padrão). O diagrama a seguir ilustra esse cenário.

![Falha no computador cliente](handling-connection-lifetime-events/_static/image4.png)

<a id="serverdisconnect"></a>

### <a name="server-disconnection-scenarios"></a>Cenários de desconexão do servidor

Quando um servidor ficar offline – for reinicializado, falhar, o domínio do aplicativo recicla, etc. – o resultado pode ser semelhante a uma conexão perdida ou a API de transporte e SignalR poderá saber imediatamente que o servidor será excluído e SignalR pode começa a tentar se reconectar sem Gerando o `ConnectionSlow` evento. Se o cliente entra em modo reconectar-se e se o servidor recupera ou reinicializações ou um novo servidor é colocado online antes de expira o período de tempo limite de desconexão, o cliente se reconectar ao servidor novo ou restaurado. Nesse caso, a conexão do SignalR continua no cliente e o `Reconnected` é gerado. No primeiro servidor, `OnDisconnected` nunca será executado e o novo servidor, `OnReconnected` é executado, embora `OnConnected` nunca foi executado para que o cliente no servidor antes. (O efeito é o mesmo se o cliente se reconecta ao mesmo servidor após uma reinicialização ou um aplicativo reciclagem de domínio, porque quando o servidor for reiniciado ele não tem memória de atividade de conexão anterior.) O diagrama a seguir pressupõe que a API de transporte fica ciente de que a conexão perdida imediatamente, portanto, o `ConnectionSlow` não é gerado.

![Falha do servidor e reconexão](handling-connection-lifetime-events/_static/image5.png)

Se um servidor não ficar disponível dentro do período de tempo limite de desconexão, termina a conexão do SignalR. Nesse cenário, o `Closed` evento (`disconnected` em clientes JavaScript) é gerado no cliente, mas `OnDisconnected` nunca é chamado no servidor. O diagrama a seguir pressupõe que a API de transporte não fique atento a conexão perdida, para que ele é detectado pelo SignalR keepalive funcionalidade e a `ConnectionSlow` é gerado.

![Falha do servidor e o tempo limite](handling-connection-lifetime-events/_static/image6.png)

<a id="timeoutkeepalive"></a>

## <a name="timeout-and-keepalive-settings"></a>Configurações de tempo limite e keepalive

O padrão `ConnectionTimeout`, `DisconnectTimeout`, e `KeepAlive` valores apropriados para a maioria dos cenários, mas pode ser alterados se seu ambiente tiver necessidades especiais. Por exemplo, se seu ambiente de rede fecha as conexões que estão inativas por 5 segundos, você talvez precise diminuir o valor de keepalive.

<a id="connectiontimeout"></a>

### <a name="connectiontimeout"></a>ConnectionTimeout

Esta configuração representa a quantidade de tempo para manter uma conexão de transporte abertas e está aguardando uma resposta antes de fechá-lo e abrir uma nova conexão. O valor padrão é 110 segundos.

Essa configuração se aplica somente quando keepalive funcionalidade está desabilitada, que normalmente se aplica somente ao long transporte de sondagem. O diagrama a seguir ilustra o efeito dessa configuração em um longo conexão de transporte de sondagem.

![Conexão de transporte de sondagem longa](handling-connection-lifetime-events/_static/image7.png)

<a id="disconnecttimeout"></a>

### <a name="disconnecttimeout"></a>DisconnectTimeout

Esta configuração representa a quantidade de tempo a aguardar após uma conexão de transporte é perdida antes de acionar o `Disconnected` evento. O valor padrão é 30 segundos. Quando você define `DisconnectTimeout`, `KeepAlive` é automaticamente definido como 1/3 do `DisconnectTimeout` valor.

<a id="keepalive"></a>

### <a name="keepalive"></a>KeepAlive

Esta configuração representa a quantidade de tempo de espera antes de enviar um pacote keepalive em uma conexão ociosa. O valor padrão é 10 segundos. Esse valor não deve ser mais de 1/3 do `DisconnectTimeout` valor.

Se você deseja definir ambos `DisconnectTimeout` e `KeepAlive`, defina `KeepAlive` depois `DisconnectTimeout`. Caso contrário, o `KeepAlive` configuração será substituído quando `DisconnectTimeout` define automaticamente `KeepAlive` para 1/3 do valor de tempo limite.

Se você quiser desabilitar a funcionalidade de manutenção de atividade, defina `KeepAlive` como null. Funcionalidade de keepalive é desabilitada automaticamente para o long transporte de sondagem.

<a id="changetimeout"></a>

### <a name="how-to-change-timeout-and-keepalive-settings"></a>Como alterar as configurações de tempo limite e keepalive

Para alterar os valores padrão para essas configurações, defini-los no `Application_Start` no seu *global. asax* de arquivo, como mostrado no exemplo a seguir. Os valores mostrados no código de exemplo são o mesmo que os valores padrão.

[!code-csharp[Main](handling-connection-lifetime-events/samples/sample1.cs)]

<a id="notifydisconnect"></a>

## <a name="how-to-notify-the-user-about-disconnections"></a>Como notificar o usuário sobre desconexões

Em alguns aplicativos, convém exibir uma mensagem para o usuário quando há problemas de conectividade. Você tem várias opções sobre como e quando fazer isso. Os exemplos de código a seguir são para um cliente JavaScript usando o proxy gerado.

- Manipular o `connectionSlow` evento para exibir uma mensagem assim que o SignalR é fica ciente dos problemas de conexão, antes de ele entra em modo reconectar-se.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample2.js)]
- Manipular o `reconnecting` evento para exibir uma mensagem quando SignalR está ciente de uma desconexão e vai para reconectar-se o modo.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample3.js)]
- Manipular o `disconnected` evento para exibir uma mensagem quando uma tentativa de reconexão foi atingido. Nesse cenário, a única maneira de restabelecer uma conexão com o servidor novamente é reiniciar o SignalR conexão chamando o `Start` método, que criará uma nova ID de conexão. O exemplo de código a seguir usa um sinalizador para certificar-se de que você emita a notificação somente após um tempo limite reconectar-se, não após um encerramento normal para a conexão do SignalR causado por chamar o `Stop` método.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample4.js)]

<a id="continuousreconnect"></a>

## <a name="how-to-continuously-reconnect"></a>Como reconectar continuamente

Em alguns aplicativos, talvez seja automaticamente restabelecer uma conexão depois que foi perdido e a tentativa de reconexão foi atingido. Para fazer isso, você pode chamar o `Start` método de sua `Closed` manipulador de eventos (`disconnected` manipulador de eventos JavaScript clientes). Talvez seja necessário aguardar um período de tempo antes de chamar `Start` para evitar isso muito frequentemente quando o servidor ou a conexão física não estão disponíveis. O exemplo de código a seguir é para um cliente JavaScript usando o proxy gerado.

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample5.js)]

Um problema potencial a serem consideradas em clientes móveis é que tentativas de reconexão contínua quando o servidor ou a conexão física não está disponível podem causar o desnecessária de bateria.

<a id="disconnectclientfromserver"></a>

## <a name="how-to-disconnect-a-client-in-server-code"></a>Como desconectar um cliente no código do servidor

SignalR versão 2 não tem um servidor interno API de desconexão de clientes. Há [planos para adicionar essa funcionalidade no futuro](https://github.com/SignalR/SignalR/issues/2101). Na versão atual do SignalR, a maneira mais simples para desconectar um cliente do servidor é implementar um método de desconexão no cliente e chame esse método do servidor. O exemplo de código a seguir mostra um método de desconexão de um cliente JavaScript usando o proxy gerado.

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample6.js)]

> [!WARNING]
> Segurança - nem esse método de desconexão de clientes nem a API interna proposta solucionará o cenário de atacados clientes que estão executando um código mal-intencionado, desde que os clientes podem se reconectar ou o código atacado pode remover o `stopClient` método ou alteração o que ele faz. O local apropriado para implementar a proteção de (DOS) de negação de serviço com monitoração de estado é a estrutura nem a camada de servidor, mas em vez disso, na infraestrutura de front-end.


<a id="detectingreasonfordisconnection"></a>
## <a name="detecting-the-reason-for-a-disconnection"></a>Detectando o motivo de uma desconexão

2.1 SignalR adiciona uma sobrecarga para o servidor `OnDisconnect` evento que indica se o cliente foi desconectado deliberadamente em vez de tempo limite. O `StopCalled` parâmetro é verdadeiro se o cliente explicitamente fechou a conexão. Em JavaScript, se um erro de servidor levou o cliente para se desconectar, as informações de erro serão passadas para o cliente como `$.connection.hub.lastError`.

**O código do servidor c#: `stopCalled` parâmetro**

[!code-csharp[Main](handling-connection-lifetime-events/samples/sample7.cs?highlight=1,3)]

**Código de cliente JavaScript: acessando `lastError` no `disconnect` evento.**

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample8.js?highlight=2-3)]
