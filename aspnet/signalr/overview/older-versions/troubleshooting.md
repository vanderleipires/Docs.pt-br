---
uid: signalr/overview/older-versions/troubleshooting
title: Solução de problemas do SignalR (SignalR 1. x) | Microsoft Docs
author: pfletcher
description: Este artigo descreve problemas comuns com o desenvolvimento de aplicativos do SignalR.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/05/2013
ms.topic: article
ms.assetid: 347210ba-c452-4feb-886f-b51d89f58971
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: eb739f806eb57d09008fa0bf04b5a40721dab73d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26505815"
---
<a name="signalr-troubleshooting-signalr-1x"></a>Solução de problemas do SignalR (SignalR 1. x)
====================
por [Patrick Fletcher](https://github.com/pfletcher)

> Este documento descreve problemas comuns com o SignalR.


Este documento contém as seções a seguir.

- [Chamando métodos entre o cliente e servidor silenciosamente falhar](#connection)
- [Outros problemas de conexão](#other)
- [Erros de compilação e do lado do servidor](#server)
- [Problemas do Visual Studio](#vs)
- [Problemas de serviços de informações da Internet](#iis)
- [Problemas do Azure](#azure)

<a id="connection"></a>

## <a name="calling-methods-between-the-client-and-server-silently-fails"></a>Chamando métodos entre o cliente e servidor silenciosamente falhar

Esta seção descreve possíveis causas de uma chamada de método entre cliente e servidor falhe sem uma mensagem de erro significativo. Em um aplicativo de SignalR, o servidor não possui informações sobre os métodos que o cliente implementa; Quando o servidor chama um método de cliente, os dados de nome e o parâmetro de método são enviados para o cliente e o método é executado somente se ele existe no formato que o servidor especificado. Se nenhum método correspondente foi encontrado no cliente, nada acontecerá, e nenhuma mensagem de erro é gerada no servidor.

Para investigar melhor métodos de cliente que está sendo chamados não, você pode ativar registro em log antes de chamar o método start no hub para ver quais chamadas são provenientes do servidor. Para habilitar o registro em log em um aplicativo JavaScript, consulte [como habilitar o registro de cliente (versão de cliente JavaScript)](../guide-to-the-api/hubs-api-guide-javascript-client.md#logging). Para habilitar o registro em log em um aplicativo cliente .NET, consulte [como habilitar o registro de cliente (versão de cliente .NET)](../guide-to-the-api/hubs-api-guide-net-client.md#logging).

### <a name="misspelled-method-incorrect-method-signature-or-incorrect-hub-name"></a>Método incorreta, assinatura de método incorreto ou nome de hub incorreto

Se o nome ou a assinatura de um método chamado corresponder exatamente um método apropriado no cliente, a chamada falhará. Verifique se o nome do método chamado pelo servidor corresponde o nome do método no cliente. Além disso, o SignalR cria o proxy do hub usando métodos concatenados, conforme for apropriado em JavaScript, então um método chamado `SendMessage` no servidor será chamado `sendMessage` no proxy do cliente. Se você usar o `HubName` atributo no código do lado do servidor, verifique se o nome usado corresponde o nome usado para criar o hub no cliente. Se você não usar o `HubName` de atributo, verifique se o nome do hub em um cliente JavaScript concatenados, como chatHub em vez de ChatHub.

### <a name="duplicate-method-name-on-client"></a>Nome do método duplicada no cliente

Verifique se que você não tem um método duplicado no cliente que difere somente em maiusculas. Se seu aplicativo cliente tem um método chamado `sendMessage`, verifique se que há também um método chamado `SendMessage` também.

### <a name="missing-json-parser-on-the-client"></a>Analisador JSON ausente no cliente

SignalR requer um analisador JSON devem estar presentes para serializar chamadas entre o servidor e o cliente. Se o cliente não tem um analisador JSON interno (como o Internet Explorer 7), você precisará incluir em seu aplicativo. Você pode baixar o analisador JSON [aqui](http://nuget.org/packages/json2).

### <a name="mixing-hub-and-persistentconnection-syntax"></a>Misturar sintaxe Hub e PersistentConnection

O SignalR usa os dois modelos de comunicação: PersistentConnections e Hubs. A sintaxe para chamar esses modelos de dois comunicação é diferente no código do cliente. Se você tiver adicionado um hub no seu código do servidor, verifique se que todo o código de cliente usa a sintaxe apropriada de hub.

**Código de cliente JavaScript que cria um PersistentConnection em um cliente JavaScript**

[!code-javascript[Main](troubleshooting/samples/sample1.js)]

**Código de cliente JavaScript que cria um Proxy do Hub em um cliente Javascript**

[!code-javascript[Main](troubleshooting/samples/sample2.js)]

**Servidor código c# que mapeia uma rota para um PersistentConnection**

[!code-csharp[Main](troubleshooting/samples/sample3.cs)]

**Servidor código c# que mapeia uma rota para um Hub ou diversos hubs, se você tiver vários aplicativos**

[!code-csharp[Main](troubleshooting/samples/sample4.cs)]

### <a name="connection-started-before-subscriptions-are-added"></a>Conexão iniciado antes que as assinaturas forem adicionadas

Se a conexão do Hub foi iniciado antes de métodos que podem ser chamados do servidor são adicionados ao proxy, as mensagens não serão recebidas. O seguinte código JavaScript não iniciará o hub corretamente:

**Código de cliente JavaScript incorreto que não permitirá que mensagens de Hubs ser recebida**

[!code-javascript[Main](troubleshooting/samples/sample5.js)]

Em vez disso, adicione as assinaturas de método antes de chamar Start:

**Código de cliente JavaScript que adiciona corretamente assinaturas a um hub**

[!code-javascript[Main](troubleshooting/samples/sample6.js)]

### <a name="missing-method-name-on-the-hub-proxy"></a>Nome do método ausente no proxy do hub

Verifique se que o método definido no servidor está inscrito no cliente. Mesmo que o servidor define o método, ele ainda deve ser adicionado para o proxy do cliente. Métodos podem ser adicionados para o proxy do cliente das seguintes maneiras (Observe que o método é adicionado para o `client` membro do hub, não o hub diretamente):

**Código de cliente JavaScript que adiciona métodos para um proxy do hub**

[!code-javascript[Main](troubleshooting/samples/sample7.js)]

### <a name="hub-or-hub-methods-not-declared-as-public"></a>Hub ou métodos de hub não declarados como público

Para ser visível no cliente, a implementação de hub e métodos devem ser declarados como `public`.

### <a name="accessing-hub-from-a-different-application"></a>Acessar o hub de um aplicativo diferente

Hubs de SignalR só pode ser acessados por meio de aplicativos que implementam os clientes do SignalR. SignalR não pode interoperar com outras bibliotecas de comunicação (como SOAP ou WCF serviços da web.) Se não houver nenhum cliente SignalR disponível para sua plataforma de destino, você não pode acessar diretamente o ponto de extremidade do servidor.

### <a name="manually-serializing-data"></a>Manualmente a serialização de dados

SignalR usará automaticamente JSON para o método serialize não precisa da parâmetros lá fazer isso.

### <a name="remote-hub-method-not-executed-on-client-in-ondisconnected-function"></a>Método de Hub remoto não é executado no cliente na função OnDisconnected

Esse comportamento é padrão. Quando `OnDisconnected` é chamado, o hub já tiver inserido o `Disconnected` estado, o que não permite mais métodos de hub a ser chamado.

**C# código do servidor que executa o código no evento OnDisconnected corretamente**

[!code-csharp[Main](troubleshooting/samples/sample8.cs)]

### <a name="connection-limit-reached"></a>Limite de Conexão atingido

Ao usar a versão completa do IIS em um sistema operacional de cliente com Windows 7, é imposto um limite de conexão de 10. Ao usar um sistema operacional cliente, use o IIS Express para evitar esse limite.

### <a name="cross-domain-connection-not-set-up-properly"></a>Conexão de domínio cruzado não configurado corretamente

Se uma conexão de domínio cruzado (uma conexão para o qual a URL de SignalR não é o mesmo domínio como a página de hospedagem) não está configurado corretamente, a conexão pode falhar sem uma mensagem de erro. Para obter informações sobre como habilitar a comunicação entre domínios, consulte [como estabelecer uma conexão entre domínios](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).

### <a name="connection-using-ntlm-active-directory-not-working-in-net-client"></a>Conexão usando NTLM (Active Directory) não está funcionando no cliente .NET

Uma conexão em um aplicativo cliente .NET que usa segurança de domínio poderão falhar se a conexão não está configurado corretamente. Para usar o SignalR em um ambiente de domínio, defina a propriedade de conexão necessárias da seguinte maneira:

**Cliente código c# que implementa as credenciais de conexão**

[!code-csharp[Main](troubleshooting/samples/sample9.cs)]

<a id="other"></a>

## <a name="other-connection-issues"></a>Outros problemas de conexão

Esta seção descreve as causas e soluções para os sintomas específicos ou mensagens de erro que ocorrem durante uma conexão.

### <a name="start-must-be-called-before-data-can-be-sent-error"></a>Erro de "Início deve ser chamado antes dos dados serem enviados"

Esse erro geralmente é visto se o código faz referência a objetos de SignalR antes que a conexão seja iniciada. Wireup para manipuladores e assim por diante que serão chamada de métodos definidos no servidor deve ser adicionados após a conexão. Observe que a chamada para `Start` é assíncrona, portanto, código após a chamada pode ser executada antes que ele for concluído. É a melhor maneira de adicionar manipuladores após o início de uma conexão completamente para colocá-los em uma função de retorno de chamada que é passada como um parâmetro para o método de início:

**Código de cliente JavaScript que adiciona corretamente os manipuladores de eventos que fazem referência a objetos de SignalR**

[!code-javascript[Main](troubleshooting/samples/sample10.js?highlight=1)]

Esse erro também será exibido se uma conexão for interrompido enquanto objetos SignalR ainda estão sendo referenciados.

### <a name="301-moved-permanently-or-302-moved-temporarily-error"></a>Erro de "302 movido temporariamente" ou "301 movido permanentemente"

Esse erro pode ser visto se o projeto contiver uma pasta chamada SignalR, que irá interferir com o proxy criado automaticamente. Para evitar esse erro, não use uma pasta chamada `SignalR` em seu aplicativo ou a geração de automática de proxy turn off. Consulte [o Proxy gerado e o que ele faz para você](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy) para obter mais detalhes.

### <a name="403-forbidden-error-in-net-or-silverlight-client"></a>Erro "403 Proibido" no cliente .NET ou o Silverlight

Esse erro pode ocorrer em ambientes de domínio cruzado onde a comunicação entre domínios corretamente não está habilitada. Para obter informações sobre como habilitar a comunicação entre domínios, consulte [como estabelecer uma conexão entre domínios](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain). Para estabelecer uma conexão entre domínios em um cliente do Silverlight, consulte [conexões entre domínios de clientes do Silverlight](../guide-to-the-api/hubs-api-guide-net-client.md#slcrossdomain).

### <a name="404-not-found-error"></a>Erro de "404 não encontrado"

Há várias causas para esse problema. Verifique se todas as seguintes opções:

- **Referência de endereço de proxy de Hub não está formatada corretamente:** esse erro geralmente é visto se a referência para o endereço de proxy do hub gerado não está formatada corretamente. Verifique se que a referência para o endereço do hub foi feita corretamente. Consulte [como referência ao proxy gerado dinamicamente](../guide-to-the-api/hubs-api-guide-javascript-client.md#dynamicproxy) para obter detalhes.
- **Adicionar rotas ao aplicativo antes de adicionar a rota do hub:** se seu aplicativo usa a outras rotas, verifique se a primeira rota adicionada é a chamada para `MapHubs`.

### <a name="500-internal-server-error"></a>"Erro de servidor interno 500"

Este é um erro muito genérico que pode ter uma ampla variedade de causas. Os detalhes do erro devem aparecer no log de eventos do servidor, ou podem ser encontrados usando o servidor de depuração. Informações de erro mais detalhadas podem ser obtidas ativando erros detalhados no servidor. Para obter mais informações, consulte [como tratar erros na classe Hub](../guide-to-the-api/hubs-api-guide-server.md#handleErrors).

### <a name="typeerror-lthubtypegt-is-undefined-error"></a>"TypeError: &lt;hubType&gt; é indefinido" Erro

Esse erro ocorrerá se a chamada para `MapHubs` não foi feita corretamente. Consulte [como registrar a rota SignalR e configurar opções de SignalR](../guide-to-the-api/hubs-api-guide-server.md#route) para obter mais informações.

### <a name="jsonserializationexception-was-unhandled-by-user-code"></a>JsonSerializationException não foi manipulada pelo código do usuário

Verifique se os parâmetros que você envia para seus métodos não têm tipos não serializáveis (como identificadores de arquivos ou conexões de banco de dados). Se você precisar usar membros em um objeto do lado do servidor que você não deseja ser enviada ao cliente (a segurança ou por motivos de serialização), use o `JSONIgnore` atributo.

### <a name="protocol-error-unknown-transport-error"></a>"Erro de protocolo: transporte desconhecido" Erro

Esse erro pode ocorrer se o cliente não oferece suporte os transportes SignalR usa. Consulte [transportes e Fallbacks](../getting-started/introduction-to-signalr.md#transports) para obter informações no qual os navegadores podem ser usados com o SignalR.

### <a name="javascript-hub-proxy-generation-has-been-disabled"></a>"Geração de proxy JavaScript Hub foi desabilitada."

Este erro ocorrerá se `DisableJavaScriptProxies` é definida durante a também inclui uma referência para o proxy gerado dinamicamente em `signalr/hubs`. Para obter mais informações sobre como criar o proxy manualmente, consulte [o proxy gerado e o que ele faz para você](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy).

### <a name="the-connection-id-is-in-the-incorrect-format-or-the-user-identity-cannot-change-during-an-active-signalr-connection-error"></a>"A ID de conexão está no formato incorreto" ou "a identidade do usuário não pode alterar durante uma conexão SignalR ativa" Erro

Esse erro pode ser visto se autenticação está sendo usada e o cliente é desconectado antes da conexão é interrompida. A solução é interromper a conexão SignalR antes de logoff do cliente.

### <a name="uncaught-error-signalr-jquery-not-found-please-ensure-jquery-is-referenced-before-the-signalrjs-file-error"></a>"Não percebida erro: SignalR: jQuery não encontrado. Certifique-se de que é referenciado jQuery antes do arquivo SignalR.js"Erro

O cliente do SignalR JavaScript requer jQuery para executar. Verifique se sua referência para jQuery está correta, se o caminho usado é válido e que a referência ao jQuery é antes da referência ao SignalR.

### <a name="uncaught-typeerror-cannot-read-property-ltpropertygt-of-undefined-error"></a>"Não percebida TypeError: não é possível ler a propriedade '&lt;propriedade&gt;' indefinido" Erro

Esse erro resulta de não ter jQuery ou o proxy de hubs referenciados corretamente. Verifique se sua referência para o proxy de hubs e jQuery está correta, se o caminho usado é válido e que a referência ao jQuery é antes da referência ao proxy de hubs. A referência padrão para o proxy de hubs deve parecer com o seguinte:

**Código de cliente HTML que referencia corretamente o proxy de Hubs**

[!code-html[Main](troubleshooting/samples/sample11.html)]

### <a name="runtimebinderexception-was-unhandled-by-user-code-error"></a>Erro "RuntimeBinderException não foi manipulada pelo código do usuário"

Esse erro pode ocorrer quando a sobrecarga incorretova do `Hub.On` é usado. Se o método tiver um valor de retorno, o tipo de retorno deve ser especificado como um parâmetro de tipo genérico:

**Método definido no cliente (sem o proxy gerado)**

[!code-html[Main](troubleshooting/samples/sample12.html?highlight=1)]

### <a name="connection-id-is-inconsistent-or-connection-breaks-between-page-loads"></a>ID de Conexão é inconsistente ou quebras de conexão entre a página for carregada

Esse comportamento é padrão. Desde que o objeto de hub está hospedado no objeto de página, o hub é destruído quando a página é atualizada. Precisa de um aplicativo de várias página manter a associação entre usuários e IDs de conexão para que fiquem consistentes entre os carregamentos de página. A IDs de conexão pode ser armazenado no servidor em um um `ConcurrentDictionary` objeto ou um banco de dados.

### <a name="value-cannot-be-null-error"></a>Erro "O valor não pode ser nulo"

Atualmente, não há suporte para métodos do lado do servidor com parâmetros opcionais; Se o parâmetro opcional for omitido, o método irá falhar. Para obter mais informações, consulte [parâmetros opcionais](https://github.com/SignalR/SignalR/issues/324).

### <a name="firefox-cant-establish-a-connection-to-the-server-at-ltaddressgt-error-in-firebug"></a>"Firefox não é possível estabelecer uma conexão ao servidor em &lt;endereço&gt;" Erro no Firebug

Essa mensagem de erro pode ser vista no Firebug se falha de negociação do transporte de WebSocket e outro transporte é usado em vez disso. Esse comportamento é padrão.

### <a name="the-remote-certificate-is-invalid-according-to-the-validation-procedure-error-in-net-client-application"></a>Erro "o certificado remoto é inválido de acordo com o procedimento de validação" no aplicativo cliente .NET

Se o servidor exigir certificados de cliente personalizado, em seguida, você pode adicionar um x509certificate para a conexão antes da solicitação é feita. Adicionar o certificado para a conexão usando `Connection.AddClientCertificate`.

### <a name="connection-drops-after-authentication-times-out"></a>Conexão descarta após autenticação

Esse comportamento é padrão. As credenciais de autenticação não podem ser modificadas enquanto uma conexão estiver ativa; Para atualizar as credenciais, a conexão deve ser interrompido e reiniciado.

### <a name="onconnected-gets-called-twice-when-using-jquery-mobile"></a>OnConnected é chamado duas vezes ao usar o jQuery Mobile

jQuery Mobile `initializePage` função força os scripts em cada página para ser executado novamente, criando assim uma segunda conexão. Soluções para este problema incluem:

- Inclua a referência ao jQuery Mobile antes de seu arquivo de JavaScript.
- Desabilitar o `initializePage` função definindo `$.mobile.autoInitializePage = false`.
- Aguarde até a página para concluir a inicialização antes de iniciar a conexão.

### <a name="messages-are-delayed-in-silverlight-applications-using-server-sent-events"></a>As mensagens estão atrasadas em aplicativos do Silverlight usando eventos enviados do servidor

As mensagens estão atrasadas quando usar o servidor enviadas eventos no Silverlight. Para forçar o tempo de sondagem para ser usado em vez disso, use o seguinte ao iniciar a conexão:

[!code-css[Main](troubleshooting/samples/sample13.css)]

### <a name="permission-denied-using-forever-frame-protocol"></a>Para sempre "Permissão negada" usando protocolo de quadro

Esse é um problema conhecido, descrito [aqui](https://github.com/SignalR/SignalR/issues/1963). Este sintoma pode ser visto usando a biblioteca JQuery mais recente; a solução é fazer downgrade do seu aplicativo para JQuery 1.8.2.

<a id="server"></a>

## <a name="compilation-and-server-side-errors"></a>Erros de compilação e do lado do servidor

 A seção a seguir contém as soluções possíveis para o compilador e erros de tempo de execução do lado do servidor. 

### <a name="reference-to-hub-instance-is-null"></a>Referência à instância de Hub é nula

Como uma instância de hub é criada para cada conexão, você não pode criar uma instância do hub no seu código por conta própria. Para chamar métodos em um hub de fora do hub em si, consulte [como chamar métodos de cliente e gerenciar grupos de fora da classe Hub](../guide-to-the-api/hubs-api-guide-server.md#callfromoutsidehub) para saber como obter uma referência para o contexto do hub.

### <a name="httpcontextcurrentsession-is-null"></a>HTTPContext.Current.Session é nulo

Esse comportamento é padrão. SignalR não oferece suporte para o estado da sessão ASP.NET, como habilitar o estado da sessão interrompe mensagens duplex.

### <a name="no-suitable-method-to-override"></a>Nenhum método adequado para substituição

Você verá esse erro se você estiver usando o código de documentação mais antiga ou blogs. Verifique se que você não fazem referência a nomes de métodos que foi alterados ou substituídos (como `OnConnectedAsync`).

### <a name="hostcontextextensionswebsocketserverurl-is-null"></a>HostContextExtensions.WebSocketServerUrl é nulo

Esse comportamento é padrão. Este membro está obsoleto e não deve ser usado.

### <a name="a-route-named-signalrhubs-is-already-in-the-route-collection-error"></a>Erro "uma rota denominada 'signalr.hubs' já está na coleção de rotas"

Esse erro será exibido se `MapHubs` é chamado duas vezes por seu aplicativo. Alguns aplicativos de exemplo chamam `MapHubs` diretamente no arquivo de aplicativo global; outros fazer a chamada em uma classe wrapper. Certifique-se de que seu aplicativo não ambos.

<a id="vs"></a>

## <a name="visual-studio-issues"></a>Problemas do Visual Studio

Esta seção descreve os problemas encontrados no Visual Studio.

### <a name="script-documents-node-does-not-appear-in-solution-explorer"></a>Nó de documentos de script não aparece no Gerenciador de soluções

Alguns dos nossos tutoriais direcioná-lo para o nó "Documentos de Script" no Gerenciador de soluções durante a depuração. Este nó é produzido pelo depurador JavaScript e só será exibida durante a depuração de clientes de navegador no Internet Explorer; o nó não será exibida se o Chrome ou Firefox é usado. O depurador do JavaScript também não será executada se outro depurador do cliente está em execução, como o depurador do Silverlight.

### <a name="signalr-does-not-work-on-visual-studio-2008-or-earlier"></a>SignalR não funciona no Visual Studio 2008 ou anterior

Esse comportamento é padrão. SignalR requer o .NET Framework 4 ou posterior; Isso exige que os aplicativos SignalR ser desenvolvidos no Visual Studio 2010 ou posterior.

<a id="iis"></a>

## <a name="iis-issues"></a>Problemas do IIS

Esta seção contém os problemas com os serviços de informações da Internet.

### <a name="web-site-crashes-after-maphubs-call"></a>Site da Web falhar após chamar MapHubs

Esse problema foi corrigido na versão mais recente do SignalR. Verifique se que você está usando a versão lançada mais recente do SignalR ao atualizar a instalação usando o NuGet.

<a id="azure"></a>

## <a name="azure-issues"></a>Problemas do Azure

Esta seção contém os problemas com o Microsoft Azure.

### <a name="messages-are-not-received-through-the-azure-backplane-after-altering-topic-names"></a>As mensagens não são recebidas por meio do backplane do Azure após a alteração de nomes de tópico

Os tópicos usados pelo plano posterior do Azure não pretendem ser configuráveis pelo usuário.
