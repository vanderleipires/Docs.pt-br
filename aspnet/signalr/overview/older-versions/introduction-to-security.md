---
uid: signalr/overview/older-versions/introduction-to-security
title: "Introdução à segurança do SignalR (SignalR 1. x) | Microsoft Docs"
author: pfletcher
description: "Descreve os problemas de segurança que você deve considerar ao desenvolver um aplicativo do SignalR."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 715a4059-d307-4631-abbb-c789c95d6eb4
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/introduction-to-security
msc.type: authoredcontent
ms.openlocfilehash: ebc83098b73902fa3f7a90a38dafc43b413e75fe
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="introduction-to-signalr-security-signalr-1x"></a>Introdução à segurança do SignalR (SignalR 1. x)
====================
por [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo descreve os problemas de segurança que você deve considerar ao desenvolver um aplicativo do SignalR.


## <a name="overview"></a>Visão geral

Este documento contém as seguintes seções:

- [Conceitos de segurança de SignalR](#concepts)

    - [Autenticação e autorização](#authentication)
    - [Token de Conexão](#connectiontoken)
    - [Incluir novamente grupos ao reconectar](#rejoingroup)
- [Como o SignalR impede que a falsificação de solicitação entre sites](#csrf)
- [Recomendações de segurança de SignalR](#recommendations)

    - [Protocolo de camadas de soquete (SSL)](#ssl)
    - [Não use grupos como um mecanismo de segurança](#groupsecurity)
    - [Tratamento com segurança de entrada de clientes](#input)
    - [Reconciliando uma alteração no status de usuário com uma conexão ativa](#reconcile)
    - [Arquivos de proxy JavaScript gerados automaticamente](#autogen)
    - [Exceções](#exceptions)

<a id="concepts"></a>

## <a name="signalr-security-concepts"></a>Conceitos de segurança de SignalR

<a id="authentication"></a>

### <a name="authentication-and-authorization"></a>Autenticação e autorização

SignalR é projetado para ser integrados a estrutura de autenticação existente para um aplicativo. Ele não fornece recursos para autenticar usuários. Em vez disso, você autenticar os usuários, como você faria normalmente em seu aplicativo e, em seguida, trabalhar com os resultados da autenticação em seu código SignalR. Por exemplo, você pode autenticar os usuários com a autenticação de formulários do ASP.NET e, em seguida, em seu hub, impor a quais usuários ou funções estão autorizadas a chamar um método. Em seu hub, você também pode passar informações de autenticação, como nome de usuário ou se um usuário pertence a uma função, para o cliente.

O SignalR fornece o [autorizar](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) atributo para especificar quais usuários têm acesso a um hub ou método. Você aplicar o atributo de autorizar um hub ou métodos específicos em um hub. Sem o atributo de autorização, todos os métodos públicos no hub estão disponíveis para um cliente que está conectado ao hub. Para obter mais informações sobre hubs, consulte [autenticação e autorização para os Hubs de SignalR](../security/hub-authorization.md).

O `Authorize` atributo é usado apenas com hubs. Para impor regras de autorização ao usar um `PersistentConnection` você deve substituir o `AuthorizeRequest` método. Para obter mais informações sobre conexões persistentes, consulte [autenticação e autorização para conexões persistentes SignalR](../security/persistent-connection-authorization.md).

<a id="connectiontoken"></a>

### <a name="connection-token"></a>Token de Conexão

SignalR reduz o risco de execução de comandos mal-intencionados ao validar a identidade do remetente. Um token de conexão, que contém a id de conexão e o nome de usuário para usuários autenticados, é passado entre o cliente e o servidor para cada solicitação. A id de conexão é um identificador exclusivo que é gerado aleatoriamente pelo servidor quando uma nova conexão é criada e é mantido durante a conexão. O nome de usuário é fornecida pelo mecanismo de autenticação para o aplicativo web. O token de conexão é protegido com uma assinatura digital e criptografia.

![](introduction-to-security/_static/image2.png)

Para cada solicitação, o servidor valida o conteúdo do token para garantir que a solicitação é proveniente do usuário especificado. O nome de usuário deve corresponder à id de conexão. Validando o nome de usuário e a id de conexão, SignalR impede que um usuário mal-intencionado facilmente representar outro usuário. Se o servidor não é possível validar o token de conexão, a solicitação falhará.

![](introduction-to-security/_static/image4.png)

Como a id de conexão faz parte do processo de verificação, não revelar a id de conexão de um usuário a outros usuários nem armazena o valor no cliente, como em um cookie.

<a id="rejoingroup"></a>

### <a name="rejoining-groups-when-reconnecting"></a>Incluir novamente grupos ao reconectar

Por padrão, o aplicativo SignalR novamente atribuirá automaticamente um usuário aos grupos apropriados ao reconectar-se de uma interrupção temporária, como quando uma conexão será descartada e restabelecida antes que a conexão expire. Ao reconectar, o cliente passa um token de grupo que inclui a id de conexão e os grupos atribuídos. O token de grupo está digitalmente assinado e criptografado. O cliente retém a mesma id de conexão após uma reconexão. Portanto, a id de conexão passada do cliente reconectado deve corresponder a id de conexão anterior usada pelo cliente. Essa verificação impede que um usuário mal-intencionado passar solicitações para ingressarem em grupos não autorizados ao reconectar.

No entanto, é importante observar que o token de grupo não expira. Se um usuário pertencia a um grupo no passado, mas foi excluído do grupo, esse usuário poderá simular um token de grupo que inclui o grupo proibido. Se você precisar gerenciar com segurança os usuários que pertencem a quais grupos, você precisa armazenar esses dados no servidor, como em um banco de dados. Em seguida, adicione lógica ao seu aplicativo que verifica no servidor se um usuário pertence a um grupo. Para obter um exemplo de verificar a associação de grupo, consulte [trabalhando com grupos de](../guide-to-the-api/working-with-groups.md).

Automaticamente incluir novamente grupos se aplica somente quando uma conexão é reconectada após uma interrupção temporária. Se um usuário se desconecta sair do aplicativo ou o aplicativo ser reiniciado, seu aplicativo deve tratar como adicionar esse usuário a grupos corretos. Para obter mais informações, consulte [trabalhando com grupos de](../guide-to-the-api/working-with-groups.md).

<a id="csrf"></a>

## <a name="how-signalr-prevents-cross-site-request-forgery"></a>Como o SignalR impede que a falsificação de solicitação entre sites

Falsificação de solicitação entre sites (CSRF) é um ataque em que um site mal-intencionado envia uma solicitação para um site vulnerável onde o usuário está atualmente conectado. SignalR impede CSRF, tornando muito improvável de um site mal-intencionado para criar uma solicitação válida para o seu aplicativo SignalR.

### <a name="description-of-csrf-attack"></a>Descrição de ataque CSRF

Aqui está um exemplo de um ataque CSRF:

1. Um usuário faz logon em www.example.com, usando a autenticação de formulários.
2. O servidor autentica o usuário. A resposta do servidor inclui um cookie de autenticação.
3. Sem o logout, o usuário acessa um site mal-intencionado. Este site mal-intencionado contém o formulário HTML a seguir: 

    [!code-html[Main](introduction-to-security/samples/sample1.html)]

 Observe que a ação de formulário envia para o site vulnerável, não para o site mal-intencionado. Esta é a parte de "sites" de CSRF.
4. O usuário clica no botão Enviar. O navegador inclui o cookie de autenticação com a solicitação.
5. A solicitação é executado no servidor e com o contexto de autenticação do usuário e pode fazer tudo o que um usuário autenticado tem permissão para fazer.

Embora este exemplo requer que o usuário clicar no botão do formulário, a página mal-intencionado pode apenas executar facilmente um script que envia uma solicitação AJAX para seu aplicativo do SignalR. Além disso, usando o SSL não impede que um ataque CSRF, porque o site mal-intencionado pode enviar uma solicitação de "https://".

Normalmente, ataques CSRF são possíveis nos sites da web que usam cookies para autenticação, como navegadores enviam todos os cookies relevantes para o site de destino. No entanto, ataques CSRF não estão limitados a exploração de cookies. Por exemplo, a autenticação básica e resumida também são vulneráveis. Depois que um usuário faz logon com a autenticação básica ou Digest, o navegador envia automaticamente as credenciais, até que a sessão termina.

### <a name="csrf-mitigations-taken-by-signalr"></a>Atenuantes CSRF realizadas pelo SignalR

SignalR realiza as seguintes etapas para impedir que um site mal-intencionado criar solicitações válidas para seu aplicativo SignalR. Essas etapas são executadas por padrão e não requer nenhuma ação no seu código.

- **Desabilite as solicitações entre domínios**  
 Por padrão, entre domínios solicitações estão desabilitadas em um aplicativo de SignalR para impedir que os usuários de um domínio externo ao chamar um ponto de extremidade do SignalR. Qualquer solicitação que vêm de um domínio externo automaticamente é considerada inválida e será bloqueada. É recomendável que você mantenha esse comportamento padrão; Caso contrário, um site mal-intencionado pode enganar os usuários enviar comandos ao seu site. Se você precisar usar solicitações de domínio cruzado, consulte [como estabelecer uma conexão entre domínios](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain) .
- **Passe o token de conexão na cadeia de caracteres de consulta, não o cookie**  
 SignalR passa o token de conexão como um valor de cadeia de caracteres de consulta, em vez de como um cookie. Ao não armazenar o token de conexão como um cookie, o token de conexão não é inadvertidamente encaminhado pelo navegador quando um código mal-intencionado é encontrado. Além disso, o token de conexão não persiste além de conexão atual. Portanto, um usuário mal-intencionado não pode fazer uma solicitação de credenciais de autenticação de outro usuário.
- **Verifique se o token de conexão**  
 Conforme descrito no [token de Conexão](#connectiontoken) seção, o servidor sabe qual id de conexão é associado a cada usuário autenticado. O servidor não processar qualquer solicitação de uma id de conexão que não coincide com o nome de usuário. É improvável que um usuário mal-intencionado pode deduzir uma solicitação válida porque o usuário mal-intencionado precisa saber o nome de usuário e a id gerada aleatoriamente conexão atual. Essa id de conexão se torna inválido, assim como a conexão será encerrada. Usuários anônimos não devem ter acesso a informações confidenciais.

<a id="recommendations"></a>

## <a name="signalr-security-recommendations"></a>Recomendações de segurança de SignalR

<a id="ssl"></a>

### <a name="secure-socket-layers-ssl-protocol"></a>Protocolo de camadas de soquete (SSL)

O protocolo SSL usa criptografia para proteger o transporte de dados entre um cliente e servidor. Se seu aplicativo SignalR transmite informações confidenciais entre o cliente e servidor, use o SSL para o transporte. Para obter mais informações sobre como configurar o SSL, consulte [como configurar o SSL no IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).

<a id="groupsecurity"></a>

### <a name="do-not-use-groups-as-a-security-mechanism"></a>Não use grupos como um mecanismo de segurança

Grupos são uma maneira conveniente de coleta de usuários relacionados, mas eles não são um mecanismo seguro para limitar o acesso a informações confidenciais. Isso é especialmente verdadeiro quando os usuários podem automaticamente incluir grupos novamente durante uma reconexão. Em vez disso, considere adicionar usuários com privilégios para uma função e limitando o acesso a um método de hub para somente os membros da função. Para obter um exemplo de restringir o acesso com base em uma função, consulte [autenticação e autorização para os Hubs de SignalR](../security/hub-authorization.md). Para obter um exemplo de verificação de acesso do usuário aos grupos ao reconectar, consulte [trabalhando com grupos de](../guide-to-the-api/working-with-groups.md).

<a id="input"></a>

### <a name="safely-handling-input-from-clients"></a>Tratamento com segurança de entrada de clientes

Todas as entradas de clientes que se destina para difusão para outros clientes devem ser codificadas para garantir que um usuário mal-intencionado não envia script para outros usuários. É melhor codificar mensagens os clientes de recebimento em vez de servidor, como seu aplicativo SignalR pode ter muitos tipos diferentes de clientes. Portanto, a codificação HTML funciona para um cliente web, mas não para outros tipos de clientes. Por exemplo, um método de cliente da web para exibir uma mensagem de chat com segurança usaria o nome de usuário e a mensagem chamando o `html()` função.

[!code-html[Main](introduction-to-security/samples/sample2.html?highlight=3-4)]

<a id="reconcile"></a>

### <a name="reconciling-a-change-in-user-status-with-an-active-connection"></a>Reconciliando uma alteração no status de usuário com uma conexão ativa

Se o status de autenticação do usuário for alterado enquanto existe uma conexão ativa, o usuário receberá um erro que afirma, "não é possível alterar a identidade do usuário durante uma conexão SignalR ativa." Nesse caso, o aplicativo deve se conectar novamente ao servidor para certificar-se de que o nome de usuário e a id de conexão sejam coordenadas. Por exemplo, se seu aplicativo permite que o usuário faça logoff quando existe uma conexão ativa, o nome de usuário para a conexão não irão corresponder ao nome que é passado para a próxima solicitação. Você deseja interromper a conexão antes do usuário faz logoff e, em seguida, reiniciá-lo.

No entanto, é importante observar que a maioria dos aplicativos não será necessário parar e iniciar a conexão manualmente. Se seu aplicativo redireciona os usuários para uma página separada depois de fazer logon, como o comportamento padrão em um aplicativo de formulários da Web ou aplicativo MVC ou atualiza a página atual após o logoff, a conexão ativa é desconectada automaticamente e não exigir nenhuma ação adicional.

O exemplo a seguir mostra como parar e iniciar uma conexão quando o status do usuário foi alterado.

[!code-html[Main](introduction-to-security/samples/sample3.html)]

Ou, o status de autenticação do usuário pode alterar se seu site usa a expiração deslizante com autenticação de formulários, e não há nenhuma atividade para manter o cookie de autenticação válido. Nesse caso, o usuário será desconectado e o nome de usuário não irão corresponder ao nome de usuário no token de conexão. Você pode corrigir esse problema adicionando o script que periodicamente solicita um recurso no servidor web para manter o cookie de autenticação válido. O exemplo a seguir mostra como solicitar um recurso a cada 30 minutos.

[!code-javascript[Main](introduction-to-security/samples/sample4.js)]

<a id="autogen"></a>

### <a name="automatically-generated-javascript-proxy-files"></a>Arquivos de proxy JavaScript gerados automaticamente

Se você não deseja incluir todos os hubs e métodos no arquivo de proxy JavaScript para cada usuário, você pode desabilitar a geração automática do arquivo. Você pode escolher essa opção se você tiver vários hubs e métodos, mas não quiser que cada usuário estar atento a todos os métodos. Desativar a geração automática definindo **EnableJavaScriptProxies** para **false**.

[!code-csharp[Main](introduction-to-security/samples/sample5.cs)]

Para obter mais informações sobre os arquivos de proxy JavaScript, consulte [o proxy gerado e o que ele faz para você](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy). <a id="exceptions"></a>

### <a name="exceptions"></a>Exceções

Você deve evitar passar objetos de exceção para os clientes porque os objetos podem expor informações confidenciais para os clientes. Em vez disso, chama um método no cliente que exibe a mensagem de erro relevantes.

[!code-csharp[Main](introduction-to-security/samples/sample6.cs)]
