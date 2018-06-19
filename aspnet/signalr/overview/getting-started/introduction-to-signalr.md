---
uid: signalr/overview/getting-started/introduction-to-signalr
title: Introdução ao SignalR | Microsoft Docs
author: pfletcher
description: Este artigo descreve o SignalR e algumas das soluções que ele foi projetado para criar.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 0fab5e35-8c1f-43d4-8635-b8aba8766a71
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/introduction-to-signalr
msc.type: authoredcontent
ms.openlocfilehash: 0ceca3edc26d35b1155946e60863a84da0bbe592
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873687"
---
<a name="introduction-to-signalr"></a>Introdução ao SignalR
====================
por [Patrick Fletcher](https://github.com/pfletcher)

> Este artigo descreve o SignalR e algumas das soluções que ele foi projetado para criar. 
> 
> ## <a name="questions-and-comments"></a>Perguntas e comentários
> 
> Deixe comentários em como você gostou neste tutorial e o que podemos melhorar nos comentários na parte inferior da página. Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, você poderá postá-los para o [ASP.NET SignalR fórum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](https://stackoverflow.com/questions/tagged/signalr).


## <a name="what-is-signalr"></a>O que é o SignalR?

O SignalR do ASP.NET é uma biblioteca para desenvolvedores do ASP.NET que simplifica o processo de adição de funcionalidade da web em tempo real para aplicativos. A funcionalidade da web em tempo real é a capacidade de ter push de código do servidor de conteúdo para clientes conectados imediatamente assim que possível, em vez de ter o servidor espera para um cliente solicitar novos dados.

SignalR pode ser usado para adicionar qualquer tipo de funcionalidade da web "em tempo real" em seu aplicativo ASP.NET. Enquanto bate-papo geralmente é usada como um exemplo, você pode fazer muito mais. Sempre que um usuário atualiza uma página da web para ver os novos dados ou a página implementa [pesquisa longa](http://en.wikipedia.org/wiki/Push_technology#Long_polling) para recuperar novos dados, é um candidato ao uso do SignalR. Os exemplos incluem painéis e monitoramento de aplicativos, aplicativos de colaboração (como simultâneas de edição de documentos), atualizações de andamento e trabalho formulários em tempo real.

O SignalR também permite completamente novos tipos de aplicativos web que exigem atualizações de alta frequência do servidor, por exemplo, jogos em tempo real.

O SignalR fornece uma API simples para a criação de chamadas de procedimento remoto do servidor-para-cliente (RPC) que chamam funções JavaScript no cliente de navegadores (e outras plataformas de cliente) do código do .NET do servidor. O SignalR também inclui API para gerenciamento de conexão (por exemplo, se conectar e desconectar eventos) e o agrupamento de conexões.

![Chamar métodos com SignalR](introduction-to-signalr/_static/image1.png)

SignalR trata do gerenciamento de conexão automaticamente e permite que você as mensagens de difusão para todos os clientes conectados ao mesmo tempo, como uma sala de bate-papo. Você também pode enviar mensagens para clientes específicos. A conexão entre o cliente e servidor é persistente, ao contrário de uma conexão HTTP clássico, que é restabelecida para cada comunicação.

SignalR dá suporte à funcionalidade de "push" server", no qual código de servidor pode chamar o código do cliente no navegador usando chamadas de procedimento remoto (RPC), em vez do modelo de solicitação-resposta comum atualmente na web.

Os aplicativos SignalR podem dimensionar para milhares de clientes usando o barramento de serviço do SQL Server ou [Redis](http://redis.io).

SignalR é o código-fonte aberto, acessível por meio de [GitHub](https://github.com/signalr).

## <a name="signalr-and-websocket"></a>SignalR e WebSocket

SignalR usa o transporte de WebSocket novo quando disponíveis e voltará a transportes mais antigos quando necessário. Embora você possa escrever certamente seu aplicativo usando o WebSocket diretamente, usando o SignalR significa que muitos a funcionalidade extra que você precisa implementar já ter sido feito para você. Mais importante, isso significa que você pode codificar seu aplicativo para tirar proveito do WebSocket sem precisar se preocupar sobre a criação de um caminho de código separado para os clientes mais antigos. O SignalR também protege contra preocupar atualizações WebSocket, pois o SignalR continuará a ser atualizado para dar suporte a alterações no transporte subjacente, fornecendo o seu aplicativo uma interface consistente entre as versões do WebSocket.

Enquanto você certamente pode criar uma solução usando o WebSocket sozinho, o SignalR fornece todas as funcionalidades que você precisa escrever sozinho, como fallback para outros transportes e revisar seu aplicativo para atualizações para implementações de WebSocket.

<a id="transports"></a>

## <a name="transports-and-fallbacks"></a>Transportes e restaurações

SignalR é uma abstração sobre alguns dos transportes que são necessários para fazer o trabalho em tempo real entre cliente e servidor. Uma conexão SignalR é iniciado como HTTP e, em seguida, é promovida a uma conexão WebSocket se ele está disponível. WebSocket é o transporte ideal para o SignalR, pois ele torna o uso mais eficiente da memória do servidor, tem a menor latência e tem os recursos mais subjacentes (como full-duplex comunicação entre cliente e servidor), mas também tem mais rígidos requisitos: WebSocket exige que o servidor esteja usando o .NET Framework 4.5 ou Windows 8 e Windows Server 2012. Se esses requisitos não forem atendidos, o SignalR tentará usar outros transportes para fazer suas conexões.

### <a name="html-5-transports"></a>Os transportes de HTML 5

Esses transportes dependem de suporte para [HTML 5](http://en.wikipedia.org/wiki/HTML5). Se o navegador do cliente não oferece suporte ao padrão de HTML 5, transportes mais antigos serão usados.

- **WebSocket** (se o servidor e navegador indicam podem suportar Websocket). WebSocket é o único transporte que estabelece uma conexão persistente, bidirecional true entre cliente e servidor. No entanto, o WebSocket também tem os requisitos mais rígidos; ele tem suporte total somente nas versões mais recentes do Mozilla Firefox, Google Chrome e o Microsoft Internet Explorer e só tem uma implementação parcial em outros navegadores, como Opera e Safari.
- **Eventos enviados do servidor**, também conhecido como EventSource (se o navegador oferece suporte a enviados eventos do servidor, que é basicamente todos os navegadores, exceto o Internet Explorer.)

### <a name="comet-transports"></a>Transportes Comet

Os transportes a seguir se baseiam o [Comet](http://en.wikipedia.org/wiki/Comet_(programming)) modelo de aplicativo da web, na qual um navegador ou outro cliente mantém uma solicitação HTTP mantidos longa, que o servidor pode usar para enviar dados por push para o cliente sem o cliente especificamente solicitá-la.

- **Para sempre quadro** (para o Internet Explorer apenas). Para sempre quadro cria um IFrame oculto que faz uma solicitação para um ponto de extremidade no servidor que não for concluída. O servidor, em seguida, envia continuamente script para o cliente que é executado imediatamente, fornecendo uma conexão unidirecional em tempo real do servidor ao cliente. A conexão de cliente para servidor usa uma conexão separada do servidor para conexão de cliente e como uma solicitação HTTP padrão, uma conexão nova é criada para cada parte dos dados que precisam ser enviada.
- **AJAX sondagens longa**. Sondagem longa não cria uma conexão persistente, mas em vez disso, sonda o servidor com uma solicitação que permanece aberta até que o servidor responde, no ponto em que a conexão é fechada, e uma nova conexão é solicitado imediatamente. Isso pode introduzir alguma latência enquanto redefine a conexão.

Para obter mais informações sobre os transportes que têm suporte em quais configurações, consulte [plataformas com suporte](supported-platforms.md).

### <a name="transport-selection-process"></a>Processo de seleção de transporte

A lista a seguir mostra as etapas que usa o SignalR para decidir qual transporte a ser usado.

1. Se o navegador Internet Explorer 8 ou anterior, a sondagem longa é usado.
2. Se JSONP está configurado (ou seja, o `jsonp` parâmetro está definido como `true` quando a conexão é iniciado), a sondagem longa é usado.
3. Se estiver sendo feita uma conexão entre domínios (ou seja, se o ponto de extremidade do SignalR não está no mesmo domínio que a página de hospedagem), WebSocket será usado se os seguintes critérios são atendidos:

   - O cliente oferece suporte a CORS (compartilhamento de recursos entre origens). Para obter detalhes sobre os clientes que suportam CORS, consulte [CORS em caniuse.com](http://www.caniuse.com/CORS).
   - O cliente oferece suporte para WebSocket
   - O suporte do servidor WebSocket

     Se qualquer um desses critérios não forem atendidos, a sondagem longa será usado. Para obter mais informações sobre conexões entre domínios, consulte [como estabelecer uma conexão entre domínios](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).
4. Se JSONP não está configurado e a conexão não está entre domínios, WebSocket será usado se o cliente e o servidor oferecer suporte a ele.
5. Se o cliente ou o servidor não dão suporte WebSocket, eventos enviados do servidor será usado se estiver disponível.
6. Se os eventos enviados do servidor não estiver disponível, quadro para sempre é tentado.
7. Se o quadro para sempre falhar, a sondagem longa é usado.

<a id="MonitoringTransports"></a>
### <a name="monitoring-transports"></a>Transportes de monitoramentos

Você pode determinar qual transporte seu aplicativo está usando, permitindo que o log em seu hub e abrir a janela de console em seu navegador.

Para habilitar o log de eventos do hub em um navegador, adicione o seguinte comando para seu aplicativo cliente:

`$.connection.hub.logging = true;`

- No Internet Explorer, abra as ferramentas de desenvolvedor pressionando F12 e clique na guia do Console.

    ![Console do Microsoft Internet Explorer](introduction-to-signalr/_static/image2.png)
- No Chrome, abra o console pressionando Ctrl + Shift + J.

    ![Console no Google Chrome](introduction-to-signalr/_static/image3.png)

Com o console aberto e o log habilitado, você poderá ver qual transporte está sendo usado pelo SignalR.

![No Internet Explorer mostrando o transporte de WebSocket do console](introduction-to-signalr/_static/image4.png)

### <a name="specifying-a-transport"></a>Especificar um transporte

Negociar um transporte entra em um determinado período de tempo e de cliente/servidor recursos. Se os recursos de cliente são conhecidos, em seguida, um transporte pode ser especificado quando a conexão do cliente é iniciado. O trecho de código a seguir demonstra como iniciar uma conexão usando o transporte de sondagem longa Ajax, que seriam usadas se ela era conhecida que o cliente não oferecia suporte a qualquer outro protocolo:

`connection.start({ transport: 'longPolling' });`

Se você quiser que um cliente tentar transportes específicos em ordem, você pode especificar uma ordem de fallback. O trecho de código a seguir demonstra o WebSocket tentativa e caso de falha, indo diretamente para a sondagem longa.

`connection.start({ transport: ['webSockets','longPolling'] });`

As constantes de cadeia de caracteres para especificar os transportes são definidas da seguinte maneira:

- `webSockets`
- `foreverFrame`
- `serverSentEvents`
- `longPolling`

## <a name="connections-and-hubs"></a>Conexões e Hubs

A API SignalR contém dois modelos para comunicação entre clientes e servidores: conexões persistentes e Hubs.

Uma Conexão representa um ponto de extremidade simple para enviadas mensagens de difusão, agrupados ou de único destinatário. O oferece API de Conexão persistente (representado pela classe PersistentConnection em código .NET) o desenvolvedor acesso direto ao protocolo de comunicação de baixo nível que expõe o SignalR. Usando o modelo de comunicação de conexões será familiar aos desenvolvedores que usaram APIs com base em conexão, como o Windows Communication Foundation.

Um Hub é um pipeline de mais alto nível construído com a API de Conexão que permite que o cliente e o servidor chamar os métodos diretamente entre si. SignalR lida com a distribuição entre limites de máquina, como se fosse pela magic, permitindo que os clientes chamar os métodos no servidor como facilmente como métodos locais e vice-versa. Usando o modelo de comunicação de Hubs será familiar aos desenvolvedores que usaram invocação remota APIs, como a comunicação remota do .NET. Usando um Hub também permite que você passar parâmetros com rigidez de tipos para métodos, permitindo que a associação de modelo.

### <a name="architecture-diagram"></a>diagrama de arquitetura

O diagrama a seguir mostra a relação entre Hubs, conexões persistentes e as tecnologias subjacentes usadas para os transportes.

![Diagrama de arquitetura de SignalR mostrando APIs, transportes e clientes](introduction-to-signalr/_static/image5.png)

### <a name="how-hubs-work"></a>Como funcionam os Hubs

Quando o código do lado do servidor chama um método no cliente, um pacote é enviado pelo transporte ativo que contém o nome e os parâmetros do método a ser chamado (quando um objeto é enviado como um parâmetro de método, ele é serializado usando JSON). O cliente, em seguida, corresponde ao nome de método para métodos definidos no código do lado do cliente. Se houver uma correspondência, o método do cliente será executado usando os dados de parâmetro desserializado.

A chamada do método pode ser monitorada usando ferramentas como [Fiddler.](http://fiddler2.com/) A imagem a seguir mostra uma chamada de método enviada de um servidor do SignalR para um cliente de navegador da web no painel de Logs do Fiddler. A chamada do método está sendo enviada de um hub chamado `MoveShapeHub`, e o método que está sendo invocado é chamado `updateShape`.

![Exibição do log do Fiddler mostrando tráfego SignalR](introduction-to-signalr/_static/image6.png)

Neste exemplo, o nome do hub é identificado com o `H` parâmetro; o método de nome é identificado com o `M` parâmetro e os dados sendo enviados para o método é identificada com o `A` parâmetro. O aplicativo que gerou essa mensagem é criado na [em tempo real de alta frequência](tutorial-high-frequency-realtime-with-signalr.md) tutorial.

### <a name="choosing-a-communication-model"></a>Escolhendo um modelo de comunicação

A maioria dos aplicativos deve usar a API de Hubs. A API de conexões pode ser usada nas seguintes circunstâncias:

- O formato da mensagem real enviada precisa ser especificado.
- O desenvolvedor prefere trabalhar com um modelo de mensagens e de distribuição em vez de um modelo de invocação remota.
- Um aplicativo existente que usa um modelo de mensagens está sendo movido para usar o SignalR.
