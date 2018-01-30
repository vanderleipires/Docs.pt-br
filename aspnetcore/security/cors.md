---
title: "Habilitar solicitações entre origens (CORS)"
author: rick-anderson
description: "Este documento apresenta CORS como um padrão para permitir ou rejeitar solicitações entre origens em um aplicativo do ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 05/17/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/cors
ms.openlocfilehash: 1c0d87b61882f69dbf2aeb0a896d9294bd029374
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="enabling-cross-origin-requests-cors"></a>Habilitar solicitações entre origens (CORS)

Por [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), e [Tom Dykstra](https://github.com/tdykstra)

Segurança do navegador impede que uma página da web fazer solicitações do AJAX para outro domínio. Essa restrição é chamada de *política de mesma origem*e impede que um site mal-intencionado lendo dados confidenciais de outro site. No entanto, às vezes, convém permitir que outros sites fazer solicitações entre origens sua API da web.

[Entre o compartilhamento de recursos de origem](http://www.w3.org/TR/cors/) (CORS) é um padrão de W3C que permite que um servidor atenuar a política de mesma origem. Usando CORS, um servidor pode permitir explicitamente algumas solicitações entre origens durante a rejeição de outras pessoas. CORS é mais segura e mais flexível que técnicas anteriores como [JSONP](https://wikipedia.org/wiki/JSONP). Este tópico mostra como habilitar o CORS em um aplicativo do ASP.NET Core.

## <a name="what-is-same-origin"></a>O que é "origem mesmo"?

Duas URLs têm a mesma origem se eles tiverem portas, hosts e esquemas idênticas. ([RFC 6454](http://tools.ietf.org/html/rfc6454))

Essas duas URLs têm a mesma origem:

* `http://example.com/foo.html`

* `http://example.com/bar.html`

Essas URLs têm diferentes origens que anterior dois:

* `http://example.net`-Domínio diferente

* `http://www.example.com/foo.html`-Subdomínio diferente

* `https://example.com/foo.html`-Esquema diferente

* `http://example.com:9000/foo.html`-Porta diferente

> [!NOTE]
> Internet Explorer não considera a porta ao comparar as origens.

## <a name="setting-up-cors"></a>Configuração de CORS

Para configurar CORS para o seu aplicativo, adicione o `Microsoft.AspNetCore.Cors` pacote ao seu projeto.

Adicione os serviços CORS em Startup.cs:

[!code-csharp[Main](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors)]

## <a name="enabling-cors-with-middleware"></a>Habilitar o CORS com middleware

Para habilitar o CORS para todo o seu aplicativo adicionar o middleware CORS ao seu pipeline de solicitação usando o `UseCors` método de extensão. Observe que o middleware CORS deve preceder quaisquer pontos de extremidade definidos no aplicativo que você deseja dar suporte a solicitações entre origens (ex. antes de qualquer chamada para `UseMvc`).

Você pode especificar uma política entre origens ao adicionar o uso de middleware CORS a `CorsPolicyBuilder` classe. Há duas formas de fazer isso. A primeira é chamar UseCors com uma expressão lambda:

[!code-csharp[Main](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

**Observação:** a URL deve ser especificada sem uma barra à direita (`/`). Se a URL termina com `/`, a comparação retornará `false` e nenhum cabeçalho será retornado.

O lambda leva um `CorsPolicyBuilder` objeto. Você encontrará uma lista da [opções de configuração](#cors-policy-options) mais adiante neste tópico. Neste exemplo, a política permite que as solicitações entre origens de `http://example.com` e não há outras origens.

Observe que CorsPolicyBuilder tem uma API fluente, portanto, é possível encadear chamadas de método:

[!code-csharp[Main](../security/cors/sample/CorsExample3/Startup.cs?highlight=3&range=29-32)]

A segunda abordagem é definir uma ou mais políticas CORS nomeadas e, em seguida, selecione a política por nome em tempo de execução.

[!code-csharp[Main](cors/sample/CorsExample2/Startup.cs?name=snippet_begin)]

Este exemplo adiciona uma política CORS denominada "AllowSpecificOrigin". Para selecionar a política, passe o nome para `UseCors`.

## <a name="enabling-cors-in-mvc"></a>Habilitando CORS no MVC

Como alternativa, você pode usar MVC para aplicar CORS específicos por ação, por controlador ou globalmente para todos os controladores. Ao usar o MVC para habilitar o CORS os mesmos serviços CORS são usados, mas o middleware CORS não estiver.

### <a name="per-action"></a>Cada ação

Para especificar uma política CORS para uma ação específica, adicione o `[EnableCors]` de atributo para a ação. Especifique o nome da política.

[!code-csharp[Main](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction)]

### <a name="per-controller"></a>Por controlador

Para especificar a política CORS para um controlador específico, adicione o `[EnableCors]` de atributo para a classe do controlador. Especifique o nome da política.

[!code-csharp[Main](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController)]

### <a name="globally"></a>Globalmente

Você pode habilitar o CORS globalmente para todos os controladores, adicionando o `CorsAuthorizationFilterFactory` filtro para a coleção de filtros globais:

[!code-csharp[Main](cors/sample/CorsMVC/Startup2.cs?name=snippet_configureservices)]

A ordem de precedência é: ação, controlador, global. Políticas de nível de ação têm precedência sobre políticas de nível de controlador e políticas no nível do controlador têm precedência sobre as políticas globais.

### <a name="disable-cors"></a>Desabilitar CORS

Para desabilitar CORS para um controlador ou ação, use o `[DisableCors]` atributo.

[!code-csharp[Main](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction)]

## <a name="cors-policy-options"></a>Opções de política CORS

Esta seção descreve as várias opções que podem ser definidas em uma política CORS.

* [Definir as origens permitidas](#set-the-allowed-origins)

* [Definir os métodos HTTP permitidos](#set-the-allowed-http-methods)

* [Definir os cabeçalhos de solicitação permitido](#set-the-allowed-request-headers)

* [Definir os cabeçalhos de resposta exposto](#set-the-exposed-response-headers)

* [Credenciais nas solicitações entre origens](#credentials-in-cross-origin-requests)

* [Definir o tempo de expiração de simulação](#set-the-preflight-expiration-time)

Para algumas opções pode ser útil ler [funciona como CORS](#how-cors-works) primeiro.

### <a name="set-the-allowed-origins"></a>Definir as origens permitidas

Para permitir que um ou mais origens específicas:

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=19-23)]

Para permitir que todas as origens:

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs??range=27-31)]

Considere cuidadosamente antes de permitir solicitações de qualquer origem. Isso significa que literalmente qualquer site pode fazer chamadas AJAX para sua API.

### <a name="set-the-allowed-http-methods"></a>Definir os métodos HTTP permitidos

Para permitir que todos os métodos HTTP:

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=44-49)]

Isso afeta a solicitações preliminares e cabeçalho Access-controle-Allow-Methods.

### <a name="set-the-allowed-request-headers"></a>Definir os cabeçalhos de solicitação permitido

Uma solicitação de simulação de CORS pode incluir um cabeçalho Access-Control-Request-Headers, listando os cabeçalhos HTTP definidos pelo aplicativo (os chamados "author cabeçalhos de solicitação").

Para cabeçalhos de específico de lista de permissões:

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=53-58)]

Para permitir que todos os cabeçalhos de solicitação do autor:

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=62-67)]

Navegadores não são totalmente consistentes em como eles definidos Access-Control-Request-Headers. Se você definir cabeçalhos para algo diferente de "*", você deve incluir pelo menos "aceitar", "content-type" e "origem", além de quaisquer cabeçalhos personalizados que você deseja dar suporte.

### <a name="set-the-exposed-response-headers"></a>Definir os cabeçalhos de resposta exposto

Por padrão, o navegador não expõe todos os cabeçalhos de resposta para o aplicativo. (Consulte [http://www.w3.org/TR/cors/#simple-response-header](http://www.w3.org/TR/cors/#simple-response-header).) Os cabeçalhos de resposta que estão disponíveis por padrão são:

* Cache-Control

* Content-Language

* Tipo de conteúdo

* Expirar

* Last-Modified

* Pragma

A especificação CORS chama esses *cabeçalhos de resposta simples*. Para disponibilizar outros cabeçalhos para o aplicativo:

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=71-76)]

### <a name="credentials-in-cross-origin-requests"></a>Credenciais nas solicitações entre origens

Credenciais requerem tratamento especial em uma solicitação CORS. Por padrão, o navegador não envia credenciais com uma solicitação entre origens. As credenciais incluem cookies, bem como os esquemas de autenticação HTTP. Para enviar as credenciais com uma solicitação entre origens, o cliente deve definir XMLHttpRequest.withCredentials como true.

Usando XMLHttpRequest diretamente:

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'http://www.example.com/api/test');
xhr.withCredentials = true;
```

Em jQuery:

```jQuery
$.ajax({
  type: 'get',
  url: 'http://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

Além disso, o servidor deve permitir que as credenciais. Para permitir que as credenciais de cross-origin:

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=80-85)]

Agora, a resposta HTTP incluirá um cabeçalho Access-controle-Allow-Credentials, que informa ao navegador que o servidor permite que as credenciais para uma solicitação entre origens.

Se o navegador envia as credenciais, mas a resposta não incluir um cabeçalho Access-controle-Allow-Credentials válido, o navegador não expõe a resposta para o aplicativo e haverá falha na solicitação AJAX.

Tenha cuidado ao permitir que as credenciais de entre origens. Um site da Web em outro domínio pode enviar credenciais do usuário conectado para o aplicativo em nome do usuário sem o conhecimento do usuário. A especificação de CORS também define essa configuração origens para "*" (todas as origens) não é válido se o `Access-Control-Allow-Credentials` cabeçalho estiver presente.

### <a name="set-the-preflight-expiration-time"></a>Definir o tempo de expiração de simulação

O cabeçalho de acesso-controle-Max-Age Especifica quanto tempo a resposta à solicitação de simulação pode ser armazenados em cache. Para definir esse cabeçalho:

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=89-94)]

<a name="cors-how-cors-works"></a>

## <a name="how-cors-works"></a>Como funciona o CORS

Esta seção descreve o que acontece em uma solicitação CORS no nível das mensagens HTTP. É importante entender o funcionamento de CORS para que a política CORS possa ser configurada corretamente e troubleshooted quando ocorrerem comportamentos inesperados.

A especificação CORS apresenta vários novos cabeçalhos HTTP que habilitam solicitações entre origens. Se um navegador dá suporte a CORS, ele define esses cabeçalhos automaticamente para solicitações entre origens. Código JavaScript personalizado não é necessário habilitar o CORS.

Aqui está um exemplo de uma solicitação entre origens. O `Origin` cabeçalho fornece o domínio do site que está fazendo a solicitação:

```
GET http://myservice.azurewebsites.net/api/test HTTP/1.1
Referer: http://myclient.azurewebsites.net/
Accept: */*
Accept-Language: en-US
Origin: http://myclient.azurewebsites.net
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
```

Se o servidor permite que a solicitação, ele define o cabeçalho Access-Control-Allow-Origin na resposta. O valor desse cabeçalho corresponde o cabeçalho de origem da solicitação tanto é o valor de curinga "*", o que significa que qualquer origem é permitida:

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Type: text/plain; charset=utf-8
Access-Control-Allow-Origin: http://myclient.azurewebsites.net
Date: Wed, 20 May 2015 06:27:30 GMT
Content-Length: 12

Test message
```

Se a resposta não incluir o cabeçalho Access-Control-Allow-Origin, a solicitação AJAX falhará. Especificamente, o navegador não permite a solicitação. Mesmo se o servidor retornará uma resposta bem-sucedida, o navegador não disponibiliza a resposta para o aplicativo cliente.

### <a name="preflight-requests"></a>Solicitações de simulação

Para algumas solicitações CORS, o navegador envia uma solicitação de adicional, chamada uma "solicitação de simulação," antes de enviar a solicitação real do recurso. O navegador pode ignorar a solicitação de simulação se as seguintes condições forem verdadeiras:

* O método de solicitação é GET, HEAD ou POST, e

* O aplicativo não definir os cabeçalhos de solicitação diferente de idioma do conteúdo Accept, Accept-Language, Content-Type ou última--ID do evento, e

* O cabeçalho Content-Type (se definido) é um dos seguintes:

  * application/x-www-form-urlencoded

  * multipart/form-data

  * texto/sem formatação

A regra sobre cabeçalhos de solicitação se aplica aos cabeçalhos que o aplicativo define chamando setRequestHeader no objeto XMLHttpRequest. (A especificação CORS chama esses cabeçalhos de solicitação"autor".) A regra não se aplica aos cabeçalhos que pode definir o navegador, como o agente do usuário, o Host ou o comprimento do conteúdo.

Aqui está um exemplo de uma solicitação de simulação:

```
OPTIONS http://myservice.azurewebsites.net/api/test HTTP/1.1
Accept: */*
Origin: http://myclient.azurewebsites.net
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: accept, x-my-custom-header
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
Content-Length: 0
```

A solicitação de simulação usa o método HTTP OPTIONS. Ele inclui dois cabeçalhos especiais:

* Access-Control-Request-Method: O método HTTP que será usado para a solicitação real.

* Access-Control-Request-Headers: Uma lista de cabeçalhos de solicitação que o aplicativo definido na solicitação atual. (Novamente, isso não inclui os cabeçalhos que define o navegador).

Aqui está um exemplo de resposta, supondo que o servidor permite que a solicitação:

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Access-Control-Allow-Origin: http://myclient.azurewebsites.net
Access-Control-Allow-Headers: x-my-custom-header
Access-Control-Allow-Methods: PUT
Date: Wed, 20 May 2015 06:33:22 GMT
```

A resposta inclui um cabeçalho de acesso--permitir-métodos de controle que lista os métodos permitidos e, opcionalmente, um cabeçalho Access-Control-permitir-Headers, que lista os cabeçalhos permitidos. Se a solicitação de simulação for bem-sucedida, o navegador envia a solicitação real, conforme descrito anteriormente.
