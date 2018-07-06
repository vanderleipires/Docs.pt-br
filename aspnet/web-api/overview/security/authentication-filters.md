---
uid: web-api/overview/security/authentication-filters
title: Filtros de autenticação na API Web ASP.NET 2 | Microsoft Docs
author: MikeWasson
description: Um filtro de autenticação é um componente que autentica uma solicitação HTTP. API Web 2 e MVC 5 oferecem suporte a filtros de autenticação, mas diferem um pouco...
ms.author: aspnetcontent
ms.date: 09/25/2014
ms.assetid: b9882e53-b3ca-4def-89b0-322846973ccb
msc.legacyurl: /web-api/overview/security/authentication-filters
msc.type: authoredcontent
ms.openlocfilehash: 6cad52e0454d685c6e96746524fbbad21e1c274d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839811"
---
<a name="authentication-filters-in-aspnet-web-api-2"></a>Filtros de autenticação na API Web ASP.NET 2
====================
por [Mike Wasson](https://github.com/MikeWasson)

> Um filtro de autenticação é um componente que autentica uma solicitação HTTP. API Web 2 e MVC 5 oferecem suporte a filtros de autenticação, mas elas podem diferir ligeiramente, principalmente em convenções de nomenclatura para a interface de filtro. Este tópico descreve os filtros de autenticação de API da Web.


Filtros de autenticação permitem que você definir um esquema de autenticação para controladores ou ações individuais. Dessa forma, seu aplicativo pode dar suporte a diferentes mecanismos de autenticação para recursos diferentes do HTTP.

Neste artigo, mostrarei o código do [autenticação básica](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt) exemplo no [ http://aspnet.codeplex.com ](http://aspnet.codeplex.com). O exemplo mostra um filtro de autenticação que implementa o esquema de autenticação básica de acesso HTTP (RFC 2617). O filtro é implementado em uma classe chamada `IdentityBasicAuthenticationAttribute`. Eu não mostraremos todo o código de exemplo, apenas as partes que ilustram como gravar um filtro de autenticação.

## <a name="setting-an-authentication-filter"></a>Definindo um filtro de autenticação

Como outros filtros, os filtros de autenticação podem ser aplicadas por controlador, por ação ou globalmente a todos os controladores de API da Web.

Para aplicar um filtro de autenticação para um controlador, decore a classe de controlador com o atributo de filtro. O código a seguir define o `[IdentityBasicAuthentication]` filtro em uma classe de controlador, que permite a autenticação básica para todas as ações do controlador.

[!code-csharp[Main](authentication-filters/samples/sample1.cs)]

Para aplicar o filtro a uma ação, decore a ação com o filtro. O seguinte código define a `[IdentityBasicAuthentication]` filtro do controlador `Post` método.

[!code-csharp[Main](authentication-filters/samples/sample2.cs)]

Para aplicar o filtro para todos os controladores de API da Web, adicione-o para **GlobalConfiguration.Filters**.

[!code-csharp[Main](authentication-filters/samples/sample3.cs)]

## <a name="implementing-a-web-api-authentication-filter"></a>Implementar um filtro de autenticação de API da Web

Na API da Web, implementam filtros de autenticação do [System.Web.Http.Filters.IAuthenticationFilter](https://msdn.microsoft.com/library/system.web.http.filters.iauthenticationfilter.aspx) interface. Eles também devem herdar de **System. Attribute**, de modo a ser aplicado como atributos.

O **IAuthenticationFilter** interface tem dois métodos:

- **AuthenticateAsync** autentica a solicitação, validando as credenciais na solicitação, se estiver presente.
- **ChallengeAsync** adiciona um desafio de autenticação para a resposta HTTP, se necessário.

Esses métodos correspondem ao fluxo de autenticação definido em [RFC 2612](http://tools.ietf.org/html/rfc2616) e [RFC 2617](http://tools.ietf.org/html/rfc2617):

1. O cliente envia as credenciais no cabeçalho de autorização. Isso geralmente acontece depois que o cliente recebe uma resposta de 401 (não autorizado) do servidor. No entanto, um cliente pode enviar credenciais com qualquer solicitação, não apenas depois de obter um 401.
2. Se o servidor não aceitará as credenciais, ela retornará uma resposta de 401 (não autorizado). A resposta inclui um cabeçalho Www-Authenticate que contém um ou mais desafios. Cada desafio Especifica um esquema de autenticação reconhecido pelo servidor.

O servidor também pode retornar 401 de uma solicitação anônima. Na verdade, que normalmente é como o processo de autenticação é iniciado:

1. O cliente envia uma solicitação anônima.
2. O servidor retorna um 401.
3. Os clientes reenvia a solicitação de credenciais.

Esse fluxo inclui tanto *autenticação* e *autorização* etapas.

- Autenticação comprova a identidade do cliente.
- Autorização determina se o cliente pode acessar um recurso específico.

Na API da Web, filtros de autenticação, lidar com a autenticação, mas não a autorização. Autorização deve ser feita por um filtro de autorização ou dentro da ação de controlador.

Aqui está o fluxo no pipeline da API Web 2:

1. Antes de invocar uma ação, a API da Web cria uma lista dos filtros de autenticação para essa ação. Isso inclui filtros com escopo da ação, o escopo do controlador e o escopo global.
2. Chamadas à API de Web **AuthenticateAsync** em cada filtro na lista. Cada filtro pode validar as credenciais na solicitação. Se qualquer filtro validado com êxito as credenciais, o filtro cria uma **IPrincipal** e a anexa à solicitação. Um filtro também pode disparar um erro neste momento. Nesse caso, o restante do pipeline não é executado.
3. Supondo que nenhum erro, a solicitação flui pelo resto do pipeline.
4. Por fim, a API da Web chama cada filtro de autenticação **ChallengeAsync** método. Filtros de usam esse método para adicionar um desafio para a resposta, se necessário. Normalmente (mas nem sempre) que deve acontecer em resposta a um erro 401.

Os diagramas a seguir mostram dois casos possíveis. No primeiro, o filtro de autenticação com êxito autentica a solicitação, um filtro de autorização autoriza a solicitação e a ação do controlador retorna 200 (Okey).

![](authentication-filters/_static/image1.png)

No segundo exemplo, o filtro de autenticação autentica a solicitação, mas o filtro de autorização retorna 401 (não autorizado). Nesse caso, a ação do controlador não é invocada. O filtro de autenticação adiciona um cabeçalho Www-Authenticate à resposta.

![](authentication-filters/_static/image2.png)

Outras combinações são possíveis&mdash;, por exemplo, se a ação do controlador permitir solicitações anônimas, você pode ter um filtro de autenticação, mas sem autorização.

## <a name="implementing-the-authenticateasync-method"></a>Implementando o método AuthenticateAsync

O **AuthenticateAsync** método tenta autenticar a solicitação. Aqui está a assinatura do método:

[!code-csharp[Main](authentication-filters/samples/sample4.cs)]

O **AuthenticateAsync** método deve fazer o seguinte:

1. Nada (não operacional).
2. Criar uma **IPrincipal** e defini-lo na solicitação.
3. Defina um resultado de erro.

Opção (1) significa que a solicitação não tinha nenhuma credencial que compreende o filtro. Opção (2) significa que o filtro de autenticado com êxito a solicitação. Opção (3) significa que a solicitação tinha credenciais inválidas (como a senha incorreta), que dispara uma resposta de erro.

Aqui está uma visão geral para implementar **AuthenticateAsync**.

1. Procure as credenciais na solicitação.
2. Se não houver nenhuma credencial, não faça nada e retornar (não operacional).
3. Se houver credenciais, mas o filtro não reconhece o esquema de autenticação, não faça nada e retornar (não operacional). Outro filtro no pipeline pode compreender o esquema.
4. Se não houver credenciais que compreende o filtro, tente autenticá-los.
5. Se as credenciais sejam ruins, retornar 401, definindo `context.ErrorResult`.
6. Se as credenciais forem válidas, crie uma **IPrincipal** e defina `context.Principal`.

Mostra o código a seguir a **AuthenticateAsync** método a partir de [autenticação básica](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt) exemplo. Os comentários indicam cada etapa. O código mostra vários tipos de erro: o cabeçalho de autorização de um sem credenciais, credenciais malformadas, e o nome de usuário/senha inválidos.

[!code-csharp[Main](authentication-filters/samples/sample5.cs)]

## <a name="setting-an-error-result"></a>Definindo um resultado de erro

Se as credenciais forem inválidas, o filtro deve ser definido `context.ErrorResult` para um **IHttpActionResult** que cria uma resposta de erro. Para obter mais informações sobre **IHttpActionResult**, consulte [resultados de ação na API Web 2](../getting-started-with-aspnet-web-api/action-results.md).

O exemplo de autenticação básica inclui um `AuthenticationFailureResult` classe adequada para essa finalidade.

[!code-csharp[Main](authentication-filters/samples/sample6.cs)]

## <a name="implementing-challengeasync"></a>Implementando ChallengeAsync

A finalidade de **ChallengeAsync** método é adicionar os desafios de autenticação para a resposta, se necessário. Aqui está a assinatura do método:

[!code-csharp[Main](authentication-filters/samples/sample7.cs)]

O método é chamado em cada filtro de autenticação no pipeline de solicitação.

É importante entender que **ChallengeAsync** é chamado *antes de* a resposta HTTP é criado e, possivelmente, até mesmo antes da execução da ação de controlador. Quando **ChallengeAsync** é chamado, `context.Result` contém um **IHttpActionResult**, que é usado posteriormente para criar a resposta HTTP. Portanto, quando **ChallengeAsync** é chamado, você não sabe nada sobre a resposta HTTP ainda. O **ChallengeAsync** método deve substituir o valor original da `context.Result` com uma nova **IHttpActionResult**. Isso **IHttpActionResult** deve encapsular original `context.Result`.

![](authentication-filters/_static/image3.png)

Chamarei o original **IHttpActionResult** as *resultados interno*e o novo **IHttpActionResult** o *resultado externo*. O resultado externo deve fazer o seguinte:

1. Invoque o resultado interno para criar a resposta HTTP.
2. Examine a resposta.
3. Adicione um desafio de autenticação para a resposta, se necessário.

O exemplo a seguir é obtido do exemplo de autenticação básica. Ele define uma **IHttpActionResult** para o resultado da externo.

[!code-csharp[Main](authentication-filters/samples/sample8.cs)]

O `InnerResult` propriedade mantém interna **IHttpActionResult**. O `Challenge` propriedade representa um cabeçalho de autenticação de Www. Observe que **ExecuteAsync** primeiro chama `InnerResult.ExecuteAsync` para criar a resposta HTTP e, em seguida, adiciona o desafio, se necessário.

Verifique o código de resposta antes de adicionar o desafio. A maioria dos esquemas de autenticação apenas adicionar um desafio se a resposta 401, conforme mostrado aqui. No entanto, alguns esquemas de autenticação de adicionar um desafio para uma resposta de êxito. Por exemplo, consulte [Negotiate](http://tools.ietf.org/html/rfc4559#section-5) (RFC 4559).

Dada a `AddChallengeOnUnauthorizedResult` classe, o código real no **ChallengeAsync** é simples. Você acabou de criar o resultado e anexá-lo para `context.Result`.

[!code-csharp[Main](authentication-filters/samples/sample9.cs)]

Observação: O exemplo de autenticação básica abstrai essa lógica de um pouco, colocando-o em um método de extensão.

## <a name="combining-authentication-filters-with-host-level-authentication"></a>A combinação de filtros de autenticação com autenticação no nível do Host

"Autenticação de nível de host" é executada pelo host (como o IIS), a autenticação antes da estrutura de atingir a API da Web de solicitação.

Muitas vezes, você talvez queira habilitar a autenticação de nível de host para o restante do seu aplicativo, mas desabilitá-lo para seus controladores de API da Web. Por exemplo, um cenário típico é habilitar a autenticação de formulários no nível do host, mas usar a autenticação baseada em token para API da Web.

Para desabilitar a autenticação em nível de host dentro do pipeline da API da Web, chame `config.SuppressHostPrincipal()` em sua configuração. Isso faz com que a API da Web remover o **IPrincipal** de qualquer solicitação que entra o pipeline da API Web. Na verdade, ele &quot;un-autentica&quot; a solicitação.

[!code-csharp[Main](authentication-filters/samples/sample10.cs)]

## <a name="additional-resources"></a>Recursos adicionais

[Filtros de segurança de API da Web do ASP.NET](https://msdn.microsoft.com/magazine/dn781361.aspx) (MSDN Magazine)
