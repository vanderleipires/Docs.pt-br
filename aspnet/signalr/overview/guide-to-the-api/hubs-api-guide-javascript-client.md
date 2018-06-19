---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
title: Guia de API de Hubs de SignalR do ASP.NET - cliente JavaScript | Microsoft Docs
author: pfletcher
description: Este documento fornece uma introdução ao uso da API de Hubs de SignalR versão 2 em clientes de JavaScript, como navegadores e da Windows Store (WinJS) applicat...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/28/2015
ms.topic: article
ms.assetid: a9fd4dc0-1b96-4443-82ca-932a5b4a8ea4
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 794ab576d3c6600911f331bab7c335476e45a0c9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28035329"
---
<a name="aspnet-signalr-hubs-api-guide---javascript-client"></a>Guia de API de Hubs de SignalR do ASP.NET - cliente JavaScript
====================
por [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

> Este documento fornece uma introdução ao uso da API de Hubs de SignalR versão 2 em clientes de JavaScript, como navegadores e aplicativos da Windows Store (WinJS).
> 
> A API de Hubs de SignalR permite fazer chamadas de procedimento remoto (RPCs) de um servidor para clientes conectados e de clientes para o servidor. No código do servidor, você define métodos que podem ser chamados por clientes e chamar os métodos que são executados no cliente. No código do cliente, você define métodos que podem ser chamados do servidor e chamar os métodos que são executados no servidor. SignalR cuida de todos os detalhes do cliente para servidor para você.
> 
> O SignalR também oferece uma API de nível inferior chamada conexões persistentes. Para obter uma introdução para o SignalR, Hubs e conexões persistentes, consulte [Introdução ao SignalR](../getting-started/introduction-to-signalr.md).
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

Este documento contém as seguintes seções:

- [O proxy gerado e o que ele faz para você](#genproxy)

    - [Quando usar o proxy gerado](#cantusegenproxy)
- [Instalação do cliente](#clientsetup)

    - [Como referenciar o proxy gerado dinamicamente](#dynamicproxy)
    - [Como criar um arquivo físico para o SignalR gerado proxy](#manualproxy)
- [Como estabelecer uma conexão](#establishconnection)

    - [$. connection.hub é o mesmo objeto que $.hubConnection() cria](#connequivalence)
    - [Execução assíncrona do método start](#asyncstart)
- [Como estabelecer uma conexão entre domínios](#crossdomain)
- [Como configurar a conexão](#configureconnection)

    - [Como especificar parâmetros de cadeia de caracteres de consulta](#querystring)
    - [Como especificar o método de transporte](#transport)
- [Como obter um proxy para uma classe de Hub](#getproxy)
- [Como definir métodos no cliente que o servidor pode chamar](#callclient)
- [Como chamar métodos do servidor do cliente](#callserver)
- [Como manipular eventos de tempo de vida da conexão](#connectionlifetime)
- [Como tratar erros](#handleerrors)
- [Como habilitar o registro de cliente](#logging)

Para obter a documentação sobre como programar o servidor ou os clientes .NET, consulte os seguintes recursos:

- [Guia de API de Hubs de SignalR - servidor](hubs-api-guide-server.md)
- [Guia de API de Hubs de SignalR - cliente .NET](hubs-api-guide-net-client.md)

O componente de servidor do SignalR 2 só está disponível no .NET 4.5 (embora não haja um cliente .NET para SignalR 2 no .NET 4.0).

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a>O proxy gerado e o que ele faz para você

Você pode programar um cliente JavaScript para se comunicar com um serviço de SignalR com ou sem um proxy que SignalR gera para você. O que o proxy é simplificar a sintaxe do código que você usa para se conectar, métodos de gravação que o servidor chama, e chamar os métodos no servidor.

Quando você escrever código para chamar os métodos de servidor, o proxy gerado permite que você use a sintaxe que parece como se você estivesse executando uma função local: você pode escrever `serverMethod(arg1, arg2)` em vez de `invoke('serverMethod', arg1, arg2)`. A sintaxe do proxy gerado também permite que um erro de imediato e inteligível do lado do cliente se você digitar um nome de método server. E se você criar manualmente o arquivo que define os proxies, você também pode obter suporte ao IntelliSense para escrever código que chama os métodos de servidor.

Por exemplo, suponha que você tem a seguinte classe de Hub no servidor:

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

Os exemplos de código a seguir mostram que JavaScript código parece para invocar o `NewContosoChatMessage` método no servidor e receber invocações da `addContosoChatMessageToPage` método do servidor.

**Com o proxy gerado**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

**Sem o proxy gerado**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a>Quando usar o proxy gerado

Se você deseja registrar vários manipuladores de eventos para um método de cliente que chama o servidor, você não pode usar o proxy gerado. Caso contrário, você pode optar por usar o proxy gerado ou não com base em sua preferência de codificação. Se você optar por não utilizá-lo, você não precisa referenciar a URL "/ hubs de signalr" em um `script` elemento no código do cliente.

<a id="clientsetup"></a>

## <a name="client-setup"></a>Instalação do cliente

Um cliente JavaScript requer referências para jQuery e o arquivo de JavaScript do SignalR core. A versão do jQuery deve ser 1.6.4 ou versões posteriores principais, como 1.7.2, 1.8.2 ou 1.9.1. Se você decidir usar o proxy gerado, você também precisará uma referência ao proxy SignalR gerado arquivo JavaScript. O exemplo a seguir mostra as referências de aparência em uma página HTML que usa o proxy gerado.

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample4.html)]

Essas referências devem ser incluídas nesta ordem: jQuery, SignalR core depois disso e proxies SignalR sobrenome.

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a>Como referenciar o proxy gerado dinamicamente

No exemplo anterior, a referência ao proxy SignalR gerado é gerado dinamicamente código JavaScript, não em um arquivo físico. SignalR cria o código JavaScript para o proxy em tempo real e serve para o cliente em resposta à URL "hubs do signalr /". Se você especificou uma URL base diferente para conexões do SignalR no servidor em seu `MapSignalR` , a URL do arquivo proxy gerado dinamicamente é sua URL personalizada com "/ hubs" anexada a ele.

> [!NOTE]
> Para clientes de JavaScript do Windows 8 (Windows Store), use o arquivo físico proxy em vez de gerado dinamicamente. Para obter mais informações, consulte [como criar um arquivo físico para o SignalR gerado proxy](#manualproxy) mais adiante neste tópico.


Em um ASP.NET MVC 4 ou 5 Razor, use o til para referir-se a raiz do aplicativo em sua referência de arquivo de proxy:

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample5.html)]

Para obter mais informações sobre como usar o SignalR no MVC 5, consulte [Introdução ao SignalR e MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).

No modo de exibição Razor do ASP.NET MVC 3, use `Url.Content` para sua referência de arquivo de proxy:

[!code-cshtml[Main](hubs-api-guide-javascript-client/samples/sample6.cshtml)]

Em um aplicativo de Web Forms do ASP.NET, use `ResolveClientUrl` para os proxies referência do arquivo ou registrá-lo por meio do ScriptManager usando um caminho relativo da raiz de aplicativo (começando com um til):

[!code-aspx[Main](hubs-api-guide-javascript-client/samples/sample7.aspx)]

Como regra geral, use o mesmo método para especificar a URL "signalr/hubs" que você usa para arquivos CSS ou JavaScript. Se você especificar uma URL sem usar um til, em alguns cenários de seu aplicativo funcionará corretamente quando você testar no Visual Studio usando o IIS Express, mas falhará com um erro 404 quando você implanta para o IIS completo. Para obter mais informações, consulte **resolver referências a recursos de nível raiz** na [servidores Web no Visual Studio para projetos Web ASP.NET](https://msdn.microsoft.com/library/58wxa9w5.aspx) no site do MSDN.

Quando você executar um projeto da web no Visual Studio 2013 no modo de depuração e se você usar o Internet Explorer como navegador, você pode ver o arquivo de proxy no **Solution Explorer** em **documentos de Script**, conforme mostrado do ilustração a seguir.

![Arquivo de proxy gerado JavaScript no Gerenciador de soluções](hubs-api-guide-javascript-client/_static/image1.png)

Para ver o conteúdo do arquivo, clique duas vezes em **hubs**. Se você não estiver usando o Visual Studio 2012 ou 2013 e o Internet Explorer, ou se você não estiver no modo de depuração, você também pode obter o conteúdo do arquivo, navegando até a URL "hubs do signalR /". Por exemplo, se seu site estiver em execução em `http://localhost:56699`, vá para `http://localhost:56699/SignalR/hubs` em seu navegador.

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a>Como criar um arquivo físico para o SignalR gerado proxy

Como alternativa para o proxy gerado dinamicamente, você pode criar um arquivo físico que tem o código de proxy e fazer referência a esse arquivo. Convém fazer isso para controle de cache ou comportamento de agrupamento ou para obter o IntelliSense quando você estiver codificando chamadas para métodos de servidor.

Para criar um arquivo de proxy, execute as seguintes etapas:

1. Instalar o [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) pacote NuGet.
2. Abra um prompt de comando e navegue até o *ferramentas* pasta que contém o arquivo SignalR.exe. É a pasta Ferramentas no seguinte local:

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.2.1.0\tools`
3. Insira o seguinte comando:

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    O caminho para o *. dll* é normalmente o *bin* na sua pasta de projeto.

    Este comando cria um arquivo chamado *server.js* na mesma pasta que *signalr.exe*.
4. Coloque o *server.js* do arquivo em uma pasta apropriada em seu projeto, renomeá-la conforme for apropriado para seu aplicativo e adicione uma referência a ele em vez da referência "/ hubs de signalr".

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>Como estabelecer uma conexão

Antes de estabelecer uma conexão, você precisa criar um objeto de conexão, criar um proxy e registrar manipuladores de eventos para métodos que podem ser chamados do servidor. Ao configurar os proxy e manipuladores de eventos, estabelece a conexão chamando o `start` método.

Se você estiver usando o proxy gerado, você não precisa criar o objeto de conexão em seu próprio código, pois o código de proxy gerado faz isso para você.

<a id="nogenconnection"></a>

**Estabelecer uma conexão (com o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

**Estabelecer uma conexão (sem o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

O código de exemplo usa o padrão "/ signalr" URL para se conectar ao seu serviço SignalR. Para obter informações sobre como especificar uma URL base diferente, consulte [ASP.NET SignalR Hubs API guia - Server - a URL /signalr](hubs-api-guide-server.md#signalrurl).

Por padrão, o local de hub é o servidor atual; Se você estiver se conectando a um servidor diferente, especifique a URL antes de chamar o `start` método, conforme mostrado no exemplo a seguir:

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample10.js)]

> [!NOTE]
> Normalmente você registrar manipuladores de eventos antes de chamar o `start` método para estabelecer a conexão. Se você deseja registrar alguns manipuladores de eventos depois de estabelecer a conexão, você pode fazer isso, mas você deve registrar pelo menos um dos seus handler(s) evento antes de chamar o `start` método. Uma razão para isso é que pode haver muitos Hubs em um aplicativo, mas você não iria querer disparar o `OnConnected` eventos em cada Hub se apenas você pretende usar para um deles. Quando a conexão é estabelecida, a presença de um método de cliente no proxy do Hub é o que informa ao SignalR para disparar o `OnConnected` evento. Se você não registrar os manipuladores de evento antes de chamar o `start` método, você poderá invocar métodos de Hub, mas o Hub `OnConnected` método não será chamado e nenhum método de cliente será chamado de servidor.


<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a>$. connection.hub é o mesmo objeto que $.hubConnection() cria

Como você pode ver exemplos, quando você usar o proxy gerado, `$.connection.hub` refere-se ao objeto de conexão. Este é o mesmo objeto que você obtém chamando `$.hubConnection()` quando você não estiver usando o proxy gerado. O código de proxy gerado cria a conexão para você, executando a instrução a seguir:

![Criando uma conexão no arquivo proxy gerado](hubs-api-guide-javascript-client/_static/image3.png)

Quando você estiver usando o proxy gerado, você pode fazer tudo com `$.connection.hub` que você pode fazer com um objeto de conexão quando você não estiver usando o proxy gerado.

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a>Execução assíncrona do método start

O `start` método é executado de forma assíncrona. Ele retorna um [jQuery adiado objeto](http://api.jquery.com/category/deferred-object/), que significa que você pode adicionar funções de retorno de chamada por chamar métodos como `pipe`, `done`, e `fail`. Se você tiver o código que você deseja executar depois que a conexão é estabelecida, como uma chamada para um método de servidor, coloque esse código em uma função de retorno de chamada ou chamá-lo de uma função de retorno de chamada. O `.done` método de retorno de chamada é executado depois que a conexão foi estabelecida, e depois de qualquer código que você tem em seu `OnConnected` conclui a execução de método do manipulador de eventos no servidor.

Se você colocar a instrução "Agora conectado" do exemplo anterior, como a próxima linha de código após a `start` chamada de método (não em um `.done` retorno de chamada), o `console.log` linha será executada antes que a conexão seja estabelecida, conforme mostrado a seguir exemplo:

![Errada para escrever código que é executado após o estabelecimento de conexão](hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a>Como estabelecer uma conexão entre domínios

Normalmente se o navegador carrega uma página da `http://contoso.com`, a conexão do SignalR está no mesmo domínio, no `http://contoso.com/signalr`. Se a página de `http://contoso.com` faz uma conexão para `http://fabrikam.com/signalr`, que é uma conexão entre domínios. Por motivos de segurança, conexões de domínio cruzado são desabilitadas por padrão.

No SignalR 1. x, solicitações de domínio cruzado foram controlado por um sinalizador EnableCrossDomain único. Esse sinalizador controlado solicitações JSONP e CORS. Para obter maior flexibilidade, todos os CORS suporte foi removido do componente de servidor do SignalR (clientes JavaScript ainda usarem CORS normalmente se ele for detectado que o navegador oferece suporte a ele), e o middleware OWIN nova foi disponibilizado dar suporte a esses cenários.

Se JSONP for necessária no cliente (para dar suporte a solicitações de domínio cruzado em navegadores mais antigos), ele precisará ser habilitada explicitamente definindo `EnableJSONP` no `HubConfiguration` do objeto para `true`, conforme mostrado abaixo. JSONP está desabilitado por padrão, pois é menos segura do que CORS.

**Adicionando owin ao seu projeto:** para instalar essa biblioteca, execute o seguinte comando no Console do Gerenciador de pacotes:

`Install-Package Microsoft.Owin.Cors`

Esse comando adicionará o 2.1.0 versão do pacote ao seu projeto.

### <a name="calling-usecors"></a>Chamar UseCors

 O trecho de código a seguir demonstra como implementar conexões entre domínios em 2 de SignalR. 

**Implementando solicitações entre domínios em 2 de SignalR**

O código a seguir demonstra como habilitar o CORS ou JSONP em um projeto de SignalR 2. Este exemplo de código usa `Map` e `RunSignalR` em vez de `MapSignalR`, de modo que o middleware CORS é executado apenas para as solicitações de SignalR que exigem suporte de CORS (em vez de todo o tráfego no caminho especificado em `MapSignalR`.) Mapa também pode ser usado para qualquer outro tipo de middleware que precisa ser executado para um prefixo de URL específico, em vez de todo o aplicativo.

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample11.cs)]

> [!NOTE] 
> 
> - Não defina `jQuery.support.cors` como true no seu código.
> 
>     ![Não defina jQuery.support.cors como true](hubs-api-guide-javascript-client/_static/image7.png)
> 
>     SignalR lida com o uso de CORS. Definindo `jQuery.support.cors` para true desabilita JSONP porque ele faz com que o SignalR assuma o navegador dá suporte a CORS.
> - Quando você estiver se conectando a uma URL de localhost, Internet Explorer 10 não considerá-la uma conexão entre domínios, para que o aplicativo funcione localmente com o Internet Explorer 10 mesmo se você não tiver habilitado conexões entre domínios no servidor.
> - Para obter informações sobre como usar conexões entre domínios com o Internet Explorer 9, consulte [esse thread StackOverflow](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).
> - Para obter informações sobre como usar conexões entre domínios com o Chrome, consulte [esse thread StackOverflow](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).
> - O código de exemplo usa o padrão "/ signalr" URL para se conectar ao seu serviço SignalR. Para obter informações sobre como especificar uma URL base diferente, consulte [ASP.NET SignalR Hubs API guia - Server - a URL /signalr](hubs-api-guide-server.md#signalrurl).


<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>Como configurar a conexão

Antes de estabelecer uma conexão, você pode especificar parâmetros de cadeia de caracteres de consulta ou especificar o método de transporte.

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>Como especificar parâmetros de cadeia de caracteres de consulta

Se você deseja enviar dados para o servidor quando o cliente se conecta, você pode adicionar parâmetros de cadeia de caracteres de consulta para o objeto de conexão. Os exemplos a seguir mostram como definir um parâmetro de cadeia de caracteres de consulta no código do cliente.

**Defina um valor de cadeia de caracteres de consulta antes de chamar o método start (com o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

**Defina um valor de cadeia de caracteres de consulta antes de chamar o método start (sem o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample13.js?highlight=2)]

O exemplo a seguir mostra como ler um parâmetro de cadeia de caracteres de consulta no código do servidor.

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample14.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>Como especificar o método de transporte

Como parte do processo de conexão, um cliente SignalR normalmente negocia com o servidor para determinar o melhor transporte com suporte pelo servidor e cliente. Se você já souber qual transporte você deseja usar, você pode ignorar esse processo de negociação, especificando o método de transporte ao chamar o `start` método.

**Código do cliente que especifica o método de transporte (com o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample15.js?highlight=1)]

**Código do cliente que especifica o método de transporte (sem o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample16.js?highlight=2)]

Como alternativa, você pode especificar vários métodos de transporte na ordem em que você deseja que o SignalR para testá-los:

**Código do cliente que especifica um esquema de fallback de transporte personalizado (com o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

**Código do cliente que especifica um esquema de fallback de transporte personalizado (sem o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

Você pode usar os seguintes valores para especificar o método de transporte:

- "webSockets"
- "foreverFrame"
- "serverSentEvents"
- "longPolling"

Os exemplos a seguir mostram como descobrir qual método de transporte está sendo usado por uma conexão.

**Código do cliente que exibe o método de transporte usado por uma conexão (com o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample19.js?highlight=2)]

**Código do cliente que exibe o método de transporte usado por uma conexão (sem o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample20.js?highlight=3)]

Para obter informações sobre como verificar o método de transporte no código do servidor, consulte [guia de API de Hubs do ASP.NET SignalR - Server - como obter informações sobre o cliente da propriedade de contexto](hubs-api-guide-server.md#contextproperty). Para obter mais informações sobre os transportes e restaurações, consulte [Introdução ao SignalR - transportes e Fallbacks](../getting-started/introduction-to-signalr.md#transports).

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a>Como obter um proxy para uma classe de Hub

Cada objeto de conexão que você criar encapsula informações sobre uma conexão a um serviço de SignalR que contém uma ou mais classes de Hub. Para se comunicar com uma classe de Hub, você usa um objeto de proxy que você mesmo cria (se você não estiver usando o proxy gerado) ou que é gerado para você.

No cliente, o nome do proxy é uma versão concatenados de nome de classe do Hub. SignalR automaticamente faz essa alteração para que o código JavaScript pode seguir as convenções de JavaScript.

**Classe de Hub no servidor**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample21.cs?highlight=1)]

**Obter uma referência para o proxy de cliente gerada para o Hub**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample22.js?highlight=1)]

**Criar proxy de cliente para a classe de Hub (sem o proxy gerado)**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

Se você decora sua classe de Hub com um `HubName` de atributo, use o nome exato sem alterar o caso.

**Classe de Hub no servidor com o atributo HubName**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample24.cs?highlight=1)]

**Obter uma referência para o proxy de cliente gerada para o Hub**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample25.js?highlight=1)]

**Criar proxy de cliente para a classe de Hub (sem o proxy gerado)**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>Como definir métodos no cliente que o servidor pode chamar

Para definir um método que o servidor pode chamar de um Hub, adicione um manipulador de eventos para o proxy do Hub usando o `client` propriedade do proxy gerado ou chamada de `on` método se você não estiver usando o proxy gerado. Os parâmetros podem ser objetos complexos.

Adicione o manipulador de eventos antes de chamar o `start` método para estabelecer a conexão. (Se você quiser adicionar manipuladores de eventos após a chamada a `start` método, consulte a observação na [como estabelecer uma conexão](#establishconnection) anteriormente neste documento e usar a sintaxe mostrada para definir um método sem usar o proxy gerado.)

Correspondência de nome do método diferencia maiusculas de minúsculas. Por exemplo, `Clients.All.addContosoChatMessageToPage` no servidor executará `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, ou `addcontosochatmessagetopage` no cliente.

**Definir o método no cliente (com o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample27.js?highlight=2)]

**Maneira alternativa para definir o método no cliente (com o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample28.js?highlight=1-2)]

**Definir o método no cliente (sem o proxy gerado, ou ao adicionar depois de chamar o método start)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample29.js?highlight=3)]

**Código de servidor que chama o método de cliente**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample30.cs?highlight=5)]

Os exemplos a seguir incluem um objeto complexo como um parâmetro de método.

**Definir o método no cliente que usa um objeto complexo (com o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample31.js?highlight=2-3)]

**Definir o método no cliente que usa um objeto complexo (sem o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample32.js?highlight=3-4)]

**Código do servidor que define o objeto complexo**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample33.cs?highlight=1)]

**Código de servidor que chama o método de cliente usando um objeto complexo**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample34.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>Como chamar métodos do servidor do cliente

Para chamar um método de servidor do cliente, use o `server` propriedade do proxy gerado ou o `invoke` método no proxy do Hub, se você não estiver usando o proxy gerado. O valor de retorno ou os parâmetros podem ser objetos complexos.

Passar em uma versão de caso de ter o nome do método no Hub. SignalR automaticamente faz essa alteração para que o código JavaScript pode seguir as convenções de JavaScript.

Os exemplos a seguir mostram como chamar um método de servidor que não tem um valor de retorno e como chamar um método de servidor que tem um valor de retorno.

**Método de servidor sem o atributo HubMethodName**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample35.cs?highlight=3)]

**Código do servidor que define o objeto complexo passado em um parâmetro**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample36.cs)]

**Código do cliente que chama o método de servidor (com o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample37.js?highlight=1)]

**Código do cliente que chama o método de servidor (sem o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample38.js?highlight=1)]

Se você decorados com o método de Hub um `HubMethodName` de atributo, use esse nome sem alterar o caso.

**Método de servidor** com um atributo HubMethodName

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample39.cs?highlight=3)]

**Código do cliente que chama o método de servidor (com o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

**Código do cliente que chama o método de servidor (sem o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample41.js?highlight=1)]

Os exemplos acima mostram como chamar um método de servidor que não tem nenhum valor de retorno. Os exemplos a seguir mostram como chamar um método de servidor que tem um valor de retorno.

**Código do servidor para um método que tem um valor de retorno**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample42.cs?highlight=3)]

**A classe de estoque para o** valor de retorno

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample43.cs?highlight=1)]

**Código do cliente que chama o método de servidor (com o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample44.js?highlight=2,4-5)]

**Código do cliente que chama o método de servidor (sem o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample45.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>Como manipular eventos de tempo de vida da conexão

O SignalR fornece a seguinte conexão eventos de tempo de vida que você pode manipular:

- `starting`: Gerado antes de todos os dados são enviados pela conexão.
- `received`: Gerado quando nenhum dado é recebido na conexão. Fornece os dados recebidos.
- `connectionSlow`: Gerado quando o cliente detecta uma conexão lenta ou remoção com frequência.
- `reconnecting`: Gerado quando o transporte subjacente inicia a reconexão.
- `reconnected`: Gerado quando o transporte subjacente foi reconectada.
- `stateChanged`: Gerado quando o estado da conexão é alterado. Fornece o estado antigo e o novo estado (conectando, conectado, Reconnecting ou Disconnected).
- `disconnected`: Gerado quando a conexão foi desconectado.

Por exemplo, se você quiser exibir mensagens de aviso quando há problemas de conexão que podem causar atrasos notáveis, tratar o `connectionSlow` evento.

**Manipular o evento connectionSlow (com o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample46.js?highlight=1)]

**Manipular o evento connectionSlow (sem o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample47.js?highlight=2)]

Para obter mais informações, consulte [compreensão e tratamento de eventos de tempo de vida de Conexão no SignalR](handling-connection-lifetime-events.md).

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>Como tratar erros

O cliente SignalR JavaScript fornece um `error` que você pode adicionar um manipulador de eventos. Você também pode usar o método de falha para adicionar um manipulador de erros resultantes de uma invocação de método server.

Se você não habilitar explicitamente as mensagens de erro detalhadas no servidor, o objeto de exceção SignalR retorna após um erro contém algumas informações sobre o erro. Por exemplo, se uma chamada para `newContosoChatMessage` falhar, a mensagem de erro no objeto de erro contém "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" enviar mensagens de erro detalhadas para clientes de produção não é recomendado por motivos de segurança, mas se você deseja habilitar as mensagens de erro detalhadas para solução de problemas, use o seguinte código no servidor.

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample48.cs?highlight=2)]

O exemplo a seguir mostra como adicionar um manipulador para o evento de erro.

**Adicionar um manipulador de erros (com o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample49.js?highlight=1)]

**Adicionar um manipulador de erro (sem o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample50.js?highlight=2)]

O exemplo a seguir mostra como tratar o erro de uma invocação de método.

**Lidar com um erro de uma invocação de método (com o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample51.js?highlight=2)]

**Lidar com um erro de uma invocação de método (sem o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

Se uma invocação de método falhar, o `error` também é gerado, portanto o código do `error` manipulador de método e no `.fail` retorno de chamada do método seria executado.

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>Como habilitar o registro de cliente

Para habilitar o registro de cliente em uma conexão, defina o `logging` propriedade no objeto de conexão antes de chamar o `start` método para estabelecer a conexão.

**Habilitar registro em log (com o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample53.js?highlight=1)]

**Habilitar o log (sem o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

Para ver os logs, abra Ferramentas de desenvolvedor do seu navegador e vá para a guia Console. Para obter um tutorial que mostra a tela e instruções passo a passo capturas que mostram como fazer isso, consulte [de difusão de servidor com ASP.NET Signalr - Enable Logging](../getting-started/tutorial-server-broadcast-with-signalr.md#enablelogging).
