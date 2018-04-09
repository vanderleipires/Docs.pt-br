---
uid: signalr/overview/security/hub-authorization
title: Autenticação e autorização para os Hubs de SignalR | Microsoft Docs
author: pfletcher
description: Este tópico descreve como restringir quais usuários ou funções podem acessar os métodos de hub. Versões de software usados neste tópico ve do Visual Studio 2013 .NET 4.5 SignalR...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/05/2015
ms.topic: article
ms.assetid: a610c796-c131-473c-baef-2e6c568cb2a2
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/security/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: 8e3bc8889efb1be80c57084fb04dc8030b386601
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="authentication-and-authorization-for-signalr-hubs"></a>Autenticação e autorização para os Hubs de SignalR
====================
por [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

> Este tópico descreve como restringir quais usuários ou funções podem acessar os métodos de hub. 
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

Esse tópico contém as seguintes seções:

- [Autorizar atributo](#authorizeattribute)
- [Exigir autenticação para todos os hubs](#requireauth)
- [Autorização personalizada](#custom)
- [Informações de autenticação de passagem para clientes](#passauth)
- [Opções de autenticação para clientes do .NET](#authoptions)

    - [Cookie de autenticação de formulários](#cookie)
    - [Autenticação do Windows](#windows)
    - [Cabeçalho de Conexão](#header)
    - [Certificate](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a>Autorizar atributo

O SignalR fornece o [autorizar](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) atributo para especificar quais usuários ou funções têm acesso a um hub ou método. Esse atributo está localizado no `Microsoft.AspNet.SignalR` namespace. Aplicar o `Authorize` de atributo para um hub ou métodos específicos em um hub. Quando você aplica o `Authorize` atributo a uma classe de hub, o requisito de autorização especificada é aplicado a todos os métodos no hub. Este tópico fornece exemplos de diferentes tipos de requisitos de autorização que você pode aplicar. Sem o `Authorize` atributo, um conectado cliente pode acessar qualquer método público no hub.

Se você tiver definido uma função chamada "Admin" em seu aplicativo web, você pode especificar que apenas usuários nessa função podem acessar um hub com o código a seguir.

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

Ou, você pode especificar que um hub contém um método que está disponível para todos os usuários e um segundo método só está disponível para usuários autenticados, conforme mostrado abaixo.

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

Os exemplos a seguir abordam os cenários de autorização diferentes:

- `[Authorize]` – somente usuários autenticados
- `[Authorize(Roles = "Admin,Manager")]` – somente usuários nas funções especificadas autenticados
- `[Authorize(Users = "user1,user2")]` – somente usuários com os nomes de usuário especificado autenticados
- `[Authorize(RequireOutgoing=false)]` – somente os usuários autenticados podem invocar o hub, mas chamadas do servidor para os clientes não são limitadas por autorização, como quando apenas alguns usuários podem enviar uma mensagem, mas todos os outros podem receber a mensagem. A propriedade RequireOutgoing só pode ser aplicada ao hub inteiro, não em métodos de indivíduos dentro do hub. Quando RequireOutgoing não está definido como false, apenas os usuários que atendem ao requisito de autorização são chamados do servidor.

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a>Exigir autenticação para todos os hubs

Você pode exigir autenticação para todos os hubs e métodos de hub em seu aplicativo chamando o [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) método quando o aplicativo for iniciado. Você pode usar esse método quando você tiver vários hubs e impor um requisito de autenticação para todos eles. Com esse método, você não pode especificar os requisitos de autorização de saída, o usuário ou função. Você só pode especificar que o acesso aos métodos de hub é restrito aos usuários autenticados. No entanto, o atributo de autorizar ainda se aplicam a hubs ou métodos para especificar requisitos adicionais. Necessidade de que especificar um atributo é adicionada ao requisito de autenticação básico.

O exemplo a seguir mostra um arquivo de inicialização que restringe a todos os métodos de hub para usuários autenticados.

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

Se você chamar o `RequireAuthentication()` método após o processamento de uma solicitação de SignalR, SignalR lançará um `InvalidOperationException` exceção. SignalR lança esta exceção porque não é possível adicionar um módulo ao HubPipeline depois que o pipeline foi invocado. O exemplo anterior mostra a chamada a `RequireAuthentication` método o `Configuration` método que é executado uma vez antes de manipular a primeira solicitação.

<a id="custom"></a>

## <a name="customized-authorization"></a>Autorização personalizada

Se você precisar personalizar como a autorização é determinada, você pode criar uma classe que deriva de `AuthorizeAttribute` e substituir o [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) método. Para cada solicitação, SignalR chama esse método para determinar se o usuário está autorizado a concluir a solicitação. O método substituído, você fornece a lógica necessária para seu cenário de autorização. O exemplo a seguir mostra como implantar a autorização por meio de identidade baseada em declarações.

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a>Informações de autenticação de passagem para clientes

Talvez seja necessário usar as informações de autenticação no código que é executado no cliente. Você passar as informações necessárias ao chamar os métodos no cliente. Por exemplo, um método de aplicativo de bate-papo poderá transmitir como parâmetro o nome de usuário da pessoa postar uma mensagem, conforme mostrado abaixo.

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

Ou, você pode criar um objeto para representar as informações de autenticação e passa o objeto como um parâmetro, conforme mostrado abaixo.

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

Você nunca deve passar id de conexão de um cliente para outros clientes, como um usuário mal-intencionado pode usar para simular uma solicitação de cliente.

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a>Opções de autenticação para clientes do .NET

Quando você tem um cliente .NET, como um aplicativo de console, que interage com um hub que é limitado a usuários autenticados, você pode passar as credenciais de autenticação em um cookie, o cabeçalho de conexão ou um certificado. Os exemplos nesta seção mostram como usar os métodos diferentes para autenticar um usuário. Eles não são aplicativos de SignalR totalmente funcional. Para obter mais informações sobre clientes .NET com o SignalR, consulte [guia de API de Hubs - cliente .NET](../guide-to-the-api/hubs-api-guide-net-client.md).

<a id="cookie"></a>

### <a name="cookie"></a>Cookie

Quando o cliente .NET interage com um hub que usa autenticação de formulários do ASP.NET, você precisará definir manualmente o cookie de autenticação para a conexão. Você adiciona o cookie para o `CookieContainer` propriedade o [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) objeto. O exemplo a seguir mostra um aplicativo de console que recupera um cookie de autenticação de uma página da web e adiciona esse cookie para a conexão.

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

O aplicativo de console envia as credenciais a serem <strong>www.contoso.com/RemoteLogin</strong> que pode se referir a uma página vazia que contém o seguinte arquivo de code-behind.

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a>Autenticação do Windows

Ao usar a autenticação do Windows, você pode passar as credenciais do usuário atual usando o [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) propriedade. Você pode definir as credenciais para a conexão com o valor da DefaultCredentials.

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a>Cabeçalho de Conexão

Se seu aplicativo não estiver usando cookies, você pode passar informações de usuário no cabeçalho da conexão. Por exemplo, você pode passar um token no cabeçalho da conexão.

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

Em seguida, no hub, você verificará o token do usuário.

<a id="certificate"></a>

### <a name="certificate"></a>certificado

Você pode passar um certificado de cliente para verificar se o usuário. Você adicionar o certificado ao criar a conexão. O exemplo a seguir mostra apenas como adicionar um certificado de cliente para a conexão; ele não mostra o aplicativo de console completo. Ele usa o [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) classe que fornece várias maneiras diferentes para criar o certificado.

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
