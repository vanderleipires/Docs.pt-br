---
uid: signalr/overview/performance/signalr-connection-density-testing-with-crank
title: Densidade de Conexão do SignalR testes com pedais | Microsoft Docs
author: tfitzmac
description: Densidade de Conexão do SignalR testes com pedais
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/22/2015
ms.topic: article
ms.assetid: 148d9ca7-1af1-44b6-a9fb-91e261b9b463
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/signalr-connection-density-testing-with-crank
msc.type: authoredcontent
ms.openlocfilehash: a3e8fdb47bd80d819358f9c48cd82fd51d6c0400
ms.sourcegitcommit: fe880bf4ed1c8116071c0e47c0babf3623b7f44a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/21/2017
ms.locfileid: "26535325"
---
<a name="signalr-connection-density-testing-with-crank"></a>Densidade de Conexão do SignalR testes com pedais
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo descreve como usar a ferramenta pedais para testar um aplicativo com vários clientes simulados.


Quando seu aplicativo estiver em execução no seu ambiente de hospedagem (ou um Azure web IIS, função ou auto-hospedado usando Owin), você pode testar a resposta do aplicativo para um alto nível de densidade de conexão usando a ferramenta pedais. O ambiente de hospedagem pode ser um servidor de serviços de informações da Internet (IIS), um host Owin ou uma função web do Azure. (Observação: os contadores de desempenho não estão disponíveis em aplicativos de Web do serviço do aplicativo do Azure, e você não poderá obter dados de desempenho de um teste de densidade de conexão.)

Densidade de Conexão se refere ao número de conexões TCP simultâneas que podem ser estabelecidas em um servidor. Cada conexão TCP gera seu próprio sobrecarga e abrir um grande número de conexões ociosas eventualmente criará um afunilamento de memória.

[O SignalR codebase](https://github.com/signalr/signalr) inclui uma ferramenta de teste de carga chamada **Crank**. A versão mais recente do pedais pode ser encontrada em [a ramificação de desenvolvimento](https://github.com/SignalR/signalr/tree/dev) no GitHub. Você pode baixar um Zip codebase do arquivo morto do branch de desenvolvimento do SignalR [aqui](https://github.com/SignalR/SignalR/archive/dev.zip).

Pedais podem ser usado para totalmente esgotar a memória do servidor para calcular o número total de conexões ociosas possíveis no hardware do servidor. Como alternativa, você também pode usar pedais para teste de carga do servidor em uma determinada quantidade de pressão de memória por aumentando conexões até que uma contagem específica ou um limite de memória específica é alcançado.

Durante o teste, é importante usar clientes remotos para evitar qualquer competição por recursos (ou seja, as conexões TCP e memória). Monitore clientes para garantir que eles não estão atingindo gargalos que podem impedir que o servidor atingir sua capacidade total (CPU ou memória). Talvez seja necessário aumentar o número de clientes para carregar completamente o servidor.

### <a name="running-a-connection-density-test"></a>Executar um teste de densidade de Conexão

Esta seção descreve as etapas necessárias para executar um teste de densidade de conexão em um aplicativo do SignalR.

1. Baixe e compilar o [ramificação de desenvolvimento do SignalR codebase](https://github.com/SignalR/SignalR/archive/dev.zip). Em um prompt de comando, navegue até &lt;diretório do projeto&gt;\src\Microsoft.AspNet.SignalR.Crank\bin\debug.
2. Implante seu aplicativo no seu ambiente de hospedagem pretendido. Anote o ponto de extremidade que usa seu aplicativo; Por exemplo, no aplicativo criado no [tutorial de Introdução](../getting-started/tutorial-getting-started-with-signalr.md), o ponto de extremidade `http://<yourhost>:8080/signalr`.
3. Instalar [contadores de desempenho do SignalR](signalr-performance.md#perfcounters) no servidor. Se seu aplicativo estiver em execução no Azure, consulte [usando os contadores de desempenho de SignalR em uma função Web](using-signalr-performance-counters-in-an-azure-web-role.md).

Depois que você baixou e compilado a base de código e contadores de desempenho instalados em seu host, a ferramenta de linha de comando pedais pode ser encontrada no `src\Microsoft.AspNet.SignalR.Crank\bin\Debug` pasta.

As opções disponíveis para a ferramenta pedais incluem:

- **/?** : Mostra a tela de Ajuda. As opções disponíveis são exibidas também se o **Url** parâmetro é omitido.
- **/ Url**: A URL para conexões do SignalR. Este parâmetro é necessário. Para um aplicativo de SignalR usando o mapeamento padrão, o caminho termina em "/ signalr".
- **/ Transporte**: O nome do transporte usado. O padrão é `auto`, que seleciona o melhor protocolo disponível. As opções incluem `WebSockets`, `ServerSentEvents`, e `LongPolling` (`ForeverFrame` não é uma opção para pedais, desde que o cliente .NET, em vez de Internet Explorer é usado). Para obter mais informações sobre como o SignalR seleciona transportes, consulte [transportes e Fallbacks](../getting-started/introduction-to-signalr.md#transports).
- **/ BatchSize**: O número de clientes adicionado em cada lote. O padrão é 50.
- **/ ConnectInterval**: O intervalo em milissegundos, entre a adição de conexões. O padrão é 500.
- **/ Conexões**: O número de conexões usado para o aplicativo de teste de carga. O padrão é 100.000.
- **/ ConnectTimeout**: O tempo limite em segundos antes de cancelar o teste. O padrão é 300.
- **MinServerMBytes**: os megabytes mínima do servidor para acessar. O padrão é 500.
- **SendBytes**: O tamanho da carga enviada ao servidor em bytes. O padrão é 0.
- **SendInterval**: O atraso em milissegundos entre as mensagens para o servidor. O padrão é 500.
- **SendTimeout**: O tempo limite em milissegundos para mensagens para o servidor. O padrão é 300.
- **ControllerUrl**: A Url em que um cliente hospedará um hub de controlador. O padrão é null (nenhuma hub controlador). O hub de controlador é iniciado quando a sessão de pedais iniciada; Nenhum outro contato entre o hub de controlador e pedais é feito.
- **NumClients**: O número de clientes simulados para se conectar ao aplicativo. O padrão é um.
- **Arquivo de log**: O nome do arquivo para o arquivo de log para a execução de teste. O padrão é `crank.csv`.
- **SampleInterval**: O tempo em milissegundos, entre as amostras de contador de desempenho. O padrão é 1000.
- **SignalRInstance**: O nome da instância dos contadores de desempenho no servidor. O padrão é usar o estado de conexão do cliente.

### <a name="example"></a>Exemplo

O comando a seguir testará um site chamado `pfsignalr` no Azure que hospeda um aplicativo na porta 8080 com um hub denominado "ControllerHub", usando 100 conexões.

`crank /Connections:100 /Url:http://pfsignalr.cloudapp.net:8080/signalr`
