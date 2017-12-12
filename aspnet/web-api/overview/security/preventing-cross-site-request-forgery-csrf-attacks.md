---
uid: web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
title: "Impedindo ataques CSRF (falsificação) de solicitação entre sites na API da Web ASP.NET | Microsoft Docs"
author: MikeWasson
description: "Descreve o ataque CSRF (falsificação) de solicitação entre sites e como implementar medidas de anti-CSRF na API da Web do ASP.NET."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/12/2012
ms.topic: article
ms.assetid: 81d46f14-8f48-4d8c-830d-cc8d594dc11b
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
msc.type: authoredcontent
ms.openlocfilehash: 1cd03f3b396cc2ece1d8dbe6820f6277c02d8e62
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="preventing-cross-site-request-forgery-csrf-attacks-in-aspnet-web-api"></a>Impedindo ataques CSRF (falsificação) de solicitação entre sites na API da Web ASP.NET
====================
por [Mike Wasson](https://github.com/MikeWasson)

Falsificação de solicitação entre sites (CSRF) é um ataque em que um site mal-intencionado envia uma solicitação para um site vulnerável onde o usuário está atualmente conectado

Aqui está um exemplo de um ataque CSRF:

1. Um usuário faz logon em www.example.com, usando a autenticação de formulários.
2. O servidor autentica o usuário. A resposta do servidor inclui um cookie de autenticação.
3. Sem o logout, o usuário acessa um site mal-intencionado. Este site mal-intencionado contém o formulário HTML a seguir: 

    [!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample1.html)]

    Observe que a ação de formulário envia para o site vulnerável, não para o site mal-intencionado. Esta é a parte de "sites" de CSRF.
4. O usuário clica no botão Enviar. O navegador inclui o cookie de autenticação com a solicitação.
5. A solicitação é executado no servidor com o contexto de autenticação do usuário e pode fazer tudo o que um usuário autenticado tem permissão para fazer.

Embora este exemplo requer que o usuário clicar no botão do formulário, a página mal-intencionado pode apenas executar facilmente um script que envia o formulário automaticamente. Além disso, usando o SSL não impede que um ataque CSRF, porque o site mal-intencionado pode enviar uma solicitação de "https://".

Normalmente, ataques CSRF são possíveis nos sites da web que usam cookies para autenticação, como navegadores enviam todos os cookies relevantes para o site de destino. No entanto, ataques CSRF não estão limitados a exploração de cookies. Por exemplo, a autenticação básica e resumida também são vulneráveis. Depois de um usuário faz logon com a autenticação básica ou Digest. o navegador envia automaticamente as credenciais, até que a sessão termina.

## <a name="anti-forgery-tokens"></a>Tokens Antifalsificação

Para ajudar a evitar ataques CSRF, ASP.NET MVC usa os tokens antifalsificação, também chamados de *solicitar tokens de verificação*.

1. O cliente solicita uma página HTML que contém um formulário.
2. O servidor inclui dois tokens na resposta. Um token é enviado como um cookie. O outro é colocado em um campo de formulário oculto. Os tokens são gerados aleatoriamente para que um adversário não é possível deduzir os valores.
3. Quando o cliente envia o formulário, ele deve enviar ambos os tokens de volta para o servidor. O cliente envia o token de cookie como um cookie e envia o token de formulário dentro dos dados do formulário. (Um cliente navegador faz isso automaticamente quando o usuário envia o formulário.)
4. Se uma solicitação não inclui ambos os tokens, o servidor não permite a solicitação.

Aqui está um exemplo de um formulário HTML com um token de formulário oculto:

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample2.html)]

Tokens antifalsificação funciona porque a página mal-intencionado não é possível ler os tokens do usuário, devido às diretivas de mesma origem. ([Políticas de mesma origem](http://www.w3.org/Security/wiki/Same_Origin_Policy) impedir que os documentos hospedados em dois locais diferentes de acessar o conteúdo da outra. Portanto, no exemplo anterior, a página mal-intencionados pode enviar solicitações a example.com, mas ele não é possível ler a resposta.)

Para evitar ataques CSRF, use tokens antifalsificação com qualquer protocolo de autenticação em que o navegador envia silenciosamente credenciais depois que o usuário fizer logon. Isso inclui os protocolos de autenticação baseada em cookie, como autenticação de formulários, bem como protocolos, como autenticação básica e resumida.

Você deve exigir tokens antifalsificação para todos os métodos nonsafe (POST, PUT, DELETE). Além disso, certifique-se de que os métodos de seguros (GET, HEAD) não tem efeitos colaterais. Além disso, se você habilitar o suporte de domínio cruzado, como CORS ou JSONP, métodos mesmo seguros, como GET são potencialmente vulneráveis a ataques CSRF, permitindo que o invasor leia dados potencialmente confidenciais.

## <a name="anti-forgery-tokens-in-aspnet-mvc"></a>Tokens Antifalsificação no ASP.NET MVC

Para adicionar os tokens antifalsificação para uma página Razor, use o **HtmlHelper.AntiForgeryToken** método auxiliar:

[!code-cshtml[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample3.cshtml)]

Este método adiciona o campo de formulário oculto e também define o token de cookie.

## <a name="anti-csrf-and-ajax"></a>Anti-CSRF e AJAX

O token de formulário pode ser um problema para solicitações do AJAX, porque uma solicitação AJAX pode enviar dados JSON, não os dados de formulário HTML. É uma solução enviar os tokens em um cabeçalho HTTP personalizado. O código a seguir usa a sintaxe do Razor para gerar tokens e, em seguida, adiciona os tokens para uma solicitação AJAX. Os tokens são gerados no servidor chamando **AntiForgery.GetTokens**.

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample4.html)]

Quando você processar a solicitação, extrai os tokens de cabeçalho de solicitação. Em seguida, chame o **AntiForgery.Validate** método para validar os tokens. O **validar** método lançará uma exceção se os tokens não são válidos.

[!code-csharp[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample5.cs)]
