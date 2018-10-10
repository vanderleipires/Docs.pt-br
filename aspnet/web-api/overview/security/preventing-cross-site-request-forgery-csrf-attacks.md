---
uid: web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
title: Impedindo ataques do solicitação intersite forjada (CSRF) no ASP.NET MVC
author: MikeWasson
description: Descreve o ataque de solicitação intersite forjada (CSRF) e como implementar medidas de anti-CSRF no ASP.NET MVC da Web.
ms.author: riande
ms.date: 12/12/2012
ms.assetid: 81d46f14-8f48-4d8c-830d-cc8d594dc11b
msc.legacyurl: /web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
msc.type: authoredcontent
ms.openlocfilehash: c88975d1c205e9d0733bfb4c710b92bc8fdaaa7a
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48911491"
---
<a name="preventing-cross-site-request-forgery-csrf-attacks-in-aspnet-mvc-application"></a>Impedindo ataques do solicitação intersite forjada (CSRF) no aplicativo ASP.NET MVC
====================
por [Mike Wasson](https://github.com/MikeWasson)

Falsificação de solicitação entre sites (CSRF) é um ataque em que um site mal-intencionado envia uma solicitação para um site vulnerável no qual o usuário fez logon

Aqui está um exemplo de um ataque CSRF:

1. Um usuário faz logon em `www.example.com` usando a autenticação de formulários.
2. O servidor autentica o usuário. A resposta do servidor inclui um cookie de autenticação.
3. Sem fazer logoff, o usuário visitar um site mal-intencionado. Este site mal-intencionado contém o formulário HTML a seguir: 

    [!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample1.html)]

    Observe que a ação de formulário faz a postagem para o site vulnerável, não para o site mal-intencionado. Essa é a parte de "site cruzado" de CSRF.
4. O usuário clica no botão Enviar. O navegador inclui o cookie de autenticação com a solicitação.
5. A solicitação é executada no servidor com o contexto de autenticação do usuário e pode fazer qualquer coisa que um usuário autenticado tem permissão para fazer.

Embora este exemplo requer que o usuário clicar no botão do formulário, a página mal-intencionada pode apenas executar facilmente um script que envia o formulário automaticamente. Além disso, usando o SSL não impede que um ataque CSRF, porque o site mal-intencionado pode enviar uma solicitação de "https://".

Normalmente, ataques de CSRF são possíveis em relação a sites da web que usam cookies para autenticação, porque os navegadores enviam a todos os cookies relevantes para o site de destino. No entanto, ataques de CSRF não são limitados a exploração de cookies. Por exemplo, a autenticação básica e Digest também são vulneráveis. Depois de um usuário faz logon com a autenticação básica ou Digest. o navegador automaticamente envia as credenciais até que a sessão termina.

## <a name="anti-forgery-tokens"></a>Tokens Antifalsificação

Para ajudar a impedir ataques CSRF, o ASP.NET MVC usa tokens antifalsificação, também chamado de *solicitar tokens de verificação*.

1. O cliente solicita uma página HTML que contém um formulário.
2. O servidor inclui dois tokens na resposta. Um token é enviado como um cookie. O outro é colocado em um campo de formulário oculto. Os tokens são gerados aleatoriamente, de modo que um adversário não é possível prever os valores.
3. Quando o cliente envia o formulário, ele deve enviar os dois tokens de volta para o servidor. O cliente envia o token de cookie como um cookie e envia o token de formulário dentro dos dados de formulário. (Um cliente de navegador faz isso automaticamente quando o usuário envia o formulário.)
4. Se uma solicitação não inclui ambos os tokens, o servidor não permite a solicitação.

Aqui está um exemplo de um formulário HTML com um token de formulário oculto:

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample2.html)]

Tokens antifalsificação funciona porque a página mal-intencionados não é possível ler os tokens do usuário, devido às diretivas de mesma origem. ([Políticas de mesma origem](http://www.w3.org/Security/wiki/Same_Origin_Policy) evitar que os documentos hospedados em dois locais diferentes de acessar o conteúdo uns dos outros. Portanto, no exemplo anterior, a página mal-intencionado pode enviar solicitações para exemplo.com, mas ele não é possível ler a resposta.)

Para impedir ataques CSRF, use tokens antifalsificação com qualquer protocolo de autenticação em que o navegador envia silenciosamente credenciais depois que o usuário fizer logon. Isso inclui os protocolos de autenticação baseada em cookie, como autenticação de formulários, bem como protocolos, como autenticação básica e Digest.

Você deve exigir tokens antifalsificação para todos os métodos nonsafe (POST, PUT, DELETE). Além disso, certifique-se de que métodos seguros (GET, HEAD) não tem nenhum efeito colateral. Além disso, se você habilitar o suporte de domínio cruzado, como o CORS ou JSONP, até mesmo seguros métodos como GET são vulneráveis a ataques de CSRF, permitindo que o invasor leia dados potencialmente confidenciais.

## <a name="anti-forgery-tokens-in-aspnet-mvc"></a>Tokens Antifalsificação no ASP.NET MVC

Para adicionar os tokens antifalsificação para uma página Razor, use o **HtmlHelper.AntiForgeryToken** método auxiliar:

[!code-cshtml[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample3.cshtml)]

Esse método adiciona o campo de formulário oculto e também define o token de cookie.

## <a name="anti-csrf-and-ajax"></a>Anti-CSRF e AJAX

O token de formulário pode ser um problema para solicitações AJAX, pois uma solicitação AJAX pode enviar dados JSON, não os dados de formulário HTML. Uma solução é enviar os tokens em um cabeçalho HTTP personalizado. O código a seguir usa a sintaxe do Razor para gerar os tokens e, em seguida, adiciona os tokens a uma solicitação AJAX. Os tokens são gerados no servidor chamando **AntiForgery.GetTokens**.

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample4.html)]

Quando você processa a solicitação, extraia os tokens do cabeçalho de solicitação. Em seguida, chame o **Antiforgery** método para validar os tokens. O **validar** método gera uma exceção se os tokens não forem válidos.

[!code-csharp[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample5.cs)]
