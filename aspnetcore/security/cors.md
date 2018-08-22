---
title: Habilitar solicitações entre origens (CORS) no ASP.NET Core
author: rick-anderson
description: Saiba como CORS como padrão para permitir ou rejeitar solicitações entre origens em um aplicativo ASP.NET Core.
ms.author: riande
ms.date: 08/17/2018
uid: security/cors
ms.openlocfilehash: 0dbb7933c76bb0d1d0cab519ea08c6c8f0ebedfd
ms.sourcegitcommit: 64c2ca86fff445944b155635918126165ee0f8aa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2018
ms.locfileid: "41830098"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a>Habilitar solicitações entre origens (CORS) no ASP.NET Core

Por [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), e [Tom Dykstra](https://github.com/tdykstra)

Segurança do navegador impede que uma página da web faça solicitações AJAX para outro domínio. Essa restrição é chamada de *política de mesma origem*e impede que um site mal-intencionado lendo dados confidenciais de outro site. No entanto, às vezes, você talvez queira permitir que outros sites fazer solicitações entre origens em sua API da web.

[Entre o compartilhamento de recursos de origem](http://www.w3.org/TR/cors/) (CORS) é um padrão W3C que permite que um servidor relaxar a política de mesma origem. Usando o CORS, um servidor pode explicitamente permitir algumas solicitações entre origens enquanto rejeita outras. O CORS é mais seguro e flexível que técnicas anteriores, como [JSONP](https://wikipedia.org/wiki/JSONP). Este tópico mostra como habilitar o CORS em um aplicativo ASP.NET Core.

## <a name="what-is-same-origin"></a>O que é "mesma origem"?

Duas URLs têm a mesma origem se eles tiverem as portas, hosts e esquemas idênticas. ([RFC 6454](http://tools.ietf.org/html/rfc6454))

Essas duas URLs têm a mesma origem:

* `http://example.com/foo.html`

* `http://example.com/bar.html`

Essas URLs tem diferentes origens que o anterior dois:

* `http://example.net` -Domínio diferente

* `http://www.example.com/foo.html` -Subdomínio diferente

* `https://example.com/foo.html` -Esquema diferente

* `http://example.com:9000/foo.html` -Porta diferente

> [!NOTE]
> Internet Explorer não considera a porta ao comparar as origens.

## <a name="enable-cors"></a>Habilitar o CORS

::: moniker range="<= aspnetcore-1.1"

Para configurar o CORS para o seu aplicativo, adicione o `Microsoft.AspNetCore.Cors` pacote ao seu projeto.

::: moniker-end

Chame [AddCors](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) em `Startup.ConfigureServices`:

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors)]

## <a name="enabling-cors-with-middleware"></a>Habilitando CORS com middleware

Para habilitar o CORS, adicione o middleware do CORS ao pipeline de solicitação usando o `UseCors` método de extensão. O middleware do CORS deve preceder quaisquer pontos de extremidade definidos em seu aplicativo onde você deseja dar suporte a solicitações entre origens (por exemplo, antes de qualquer chamada para `UseMvc`).

Uma política entre origens pode ser especificada ao adicionar o middleware do CORS usando o [CorsPolicyBuilder](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) classe. Há duas formas de fazer isso. A primeira é chamar `UseCors` com um lambda:

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

**Observação:** a URL deve ser especificada sem uma barra à direita (`/`). Se a URL termina com `/`, a comparação retornará `false` e nenhum cabeçalho será retornado.

O lambda utiliza um `CorsPolicyBuilder` objeto. Você encontrará uma lista da [opções de configuração](#cors-policy-options) mais adiante neste tópico. Neste exemplo, a política permite que solicitações entre origens de `http://example.com` e sem outras origens.

CorsPolicyBuilder tem uma API fluente, portanto, é possível encadear chamadas de método:

[!code-csharp[](../security/cors/sample/CorsExample3/Startup.cs?highlight=3&range=29-32)]

A segunda abordagem é definir uma ou mais políticas CORS e, em seguida, selecione a política por nome em tempo de execução.

[!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin)]

Este exemplo adiciona uma política CORS denominada "AllowSpecificOrigin". Para selecionar a política, passe o nome para `UseCors`.

## <a name="enabling-cors-in-mvc"></a>Habilitando CORS no MVC

Como alternativa, você pode usar o MVC para aplicar um CORS específico por ação, por controlador ou globalmente para todos os controladores. Ao usar o MVC para habilitar o CORS os mesmos serviços CORS são usados, mas não é o middleware do CORS.

### <a name="per-action"></a>Por ação

Para especificar uma política CORS para uma ação específica, adicione o `[EnableCors]` atributo à ação. Especifique o nome da política.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction)]

### <a name="per-controller"></a>Por controlador

Para especificar a política CORS para um controlador específico, adicione o `[EnableCors]` atributo à classe do controlador. Especifique o nome da política.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController)]

### <a name="globally"></a>Globalmente

Você pode habilitar o CORS globalmente para todos os controladores, adicionando o `CorsAuthorizationFilterFactory` filtro à coleção de filtros globais:

[!code-csharp[](cors/sample/CorsMVC/Startup2.cs?name=snippet_configureservices)]

A ordem de precedência é: ação, controlador, global. As políticas no nível de ação têm precedência sobre as políticas no nível do controlador e políticas no nível do controlador têm precedência sobre as políticas globais.

### <a name="disable-cors"></a>Desabilitar o CORS

Para desabilitar CORS para um controlador ou ação, use o `[DisableCors]` atributo.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction)]

## <a name="cors-policy-options"></a>Opções de política de CORS

Esta seção descreve as várias opções que podem ser definidas em uma política CORS.

* [Defina as origens permitidas](#set-the-allowed-origins)

* [Defina os métodos HTTP permitidos](#set-the-allowed-http-methods)

* [Definir os cabeçalhos de solicitação permitido](#set-the-allowed-request-headers)

* [Definir os cabeçalhos de resposta exposto](#set-the-exposed-response-headers)

* [Credenciais nas solicitações entre origens](#credentials-in-cross-origin-requests)

* [Definir o tempo de expiração de simulação](#set-the-preflight-expiration-time)

Para algumas opções, pode ser útil ler [funciona como o CORS](#how-cors-works) primeiro.

### <a name="set-the-allowed-origins"></a>Defina as origens permitidas

Para permitir que um ou mais origens específicas:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=19-23)]

Para permitir que todas as origens:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs??range=27-31)]

Considere cuidadosamente antes de permitir que solicitações de qualquer origem. Isso significa que, literalmente, qualquer site possa fazer chamadas AJAX à API.

### <a name="set-the-allowed-http-methods"></a>Defina os métodos HTTP permitidos

Para permitir que todos os métodos HTTP:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=44-49)]

Isso afeta solicitações preliminares e cabeçalho Access-Control-Allow-Methods.

### <a name="set-the-allowed-request-headers"></a>Definir os cabeçalhos de solicitação permitido

Uma solicitação de simulação de CORS pode incluir um cabeçalho Access-Control-Request-Headers, listando os cabeçalhos HTTP definidos pelo aplicativo (os chamados "author cabeçalhos de solicitação").

Para cabeçalhos específicos de lista de permissões:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=53-58)]

Para permitir que todos os cabeçalhos de solicitação do autor:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=62-67)]

Navegadores não estão totalmente consistentes em como eles definir Access-Control-Request-Headers. Se você definir os cabeçalhos para algo diferente de "*", você deve incluir pelo menos "accept", "content-type" e "origem", além de quaisquer cabeçalhos personalizados que você deseja dar suporte.

### <a name="set-the-exposed-response-headers"></a>Definir os cabeçalhos de resposta exposto

Por padrão, o navegador não expõe todos os cabeçalhos de resposta para o aplicativo. (Consulte [ http://www.w3.org/TR/cors/#simple-response-header ](http://www.w3.org/TR/cors/#simple-response-header).) Os cabeçalhos de resposta que estão disponíveis por padrão são:

* Cache-Control

* Idioma do conteúdo

* Tipo de conteúdo

* Expira

* Última modificação

* Pragma

A especificação CORS chama esses *cabeçalhos de resposta simples*. Para disponibilizar outros cabeçalhos para o aplicativo:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=71-76)]

### <a name="credentials-in-cross-origin-requests"></a>Credenciais nas solicitações entre origens

Credenciais exigem tratamento especial em uma solicitação CORS. Por padrão, o navegador não envia todas as credenciais com uma solicitação entre origens. As credenciais incluem cookies, bem como os esquemas de autenticação HTTP. Para enviar as credenciais com uma solicitação entre origens, o cliente deve definir XMLHttpRequest.withCredentials como true.

Usando XMLHttpRequest diretamente:

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'http://www.example.com/api/test');
xhr.withCredentials = true;
```

No jQuery:

```jQuery
$.ajax({
  type: 'get',
  url: 'http://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

Além disso, o servidor deve permitir que as credenciais. Para permitir credenciais entre origens:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=80-85)]

Agora, a resposta HTTP incluirá um cabeçalho Access-Control-Allow-Credentials, que informa ao navegador que o servidor permite que as credenciais para uma solicitação entre origens.

Se o navegador envia as credenciais, mas a resposta não inclui um cabeçalho Access-Control-Allow-Credentials válido, o navegador não expõe a resposta para o aplicativo e a solicitação AJAX falhar.

Tenha cuidado ao permitindo credenciais entre origens. Um site em outro domínio pode enviar credenciais do usuário conectado para o aplicativo em nome do usuário sem o conhecimento do usuário. A especificação CORS também declara que a configuração origens `"*"` (todas as origens) é inválida se o `Access-Control-Allow-Credentials` cabeçalho está presente.

### <a name="set-the-preflight-expiration-time"></a>Definir o tempo de expiração de simulação

O cabeçalho Access-Control-Max-Age Especifica quanto tempo a resposta à solicitação de simulação pode ser armazenados em cache. Para definir esse cabeçalho:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=89-94)]

<a name="cors-how-cors-works"></a>

## <a name="how-cors-works"></a>Como funciona o CORS

Esta seção descreve o que acontece em uma solicitação CORS no nível das mensagens HTTP. É importante entender o funcionamento de CORS para que a política CORS pode ser configurada corretamente e depurada quando comportamentos inesperados ocorrerem.

A especificação CORS apresenta vários novos cabeçalhos HTTP que permitem que solicitações entre origens. Se um navegador oferece suporte a CORS, ele define esses cabeçalhos automaticamente para solicitações entre origens. Código JavaScript personalizado não é necessário para habilitar o CORS.

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

Se o servidor permite que a solicitação, ele define o cabeçalho Access-Control-Allow-Origin na resposta. O valor desse cabeçalho corresponde ao cabeçalho da origem da solicitação, ou é o valor de curinga "*", o que significa que qualquer origem é permitida:

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

Se a resposta não incluir o cabeçalho Access-Control-Allow-Origin, a solicitação AJAX falha. Especificamente, o navegador não permite a solicitação. Mesmo se o servidor retorna uma resposta bem-sucedida, o navegador não disponibiliza a resposta para o aplicativo cliente.

### <a name="preflight-requests"></a>Solicitações de simulação

Para algumas solicitações CORS, o navegador envia uma solicitação adicional, chamada de uma "solicitação de simulação," antes de enviar a solicitação real para o recurso. O navegador pode ignorar a solicitação de simulação se as seguintes condições forem verdadeiras:

* O método de solicitação é GET, HEAD ou POST, e

* O aplicativo não definir quaisquer cabeçalhos de solicitação que não seja Accept, Accept-Language, Content-Language, Content-Type ou última--ID do evento, e

* O cabeçalho Content-Type (se definido) é um dos seguintes:

  * application/x-www-form-urlencoded

  * multipart/form-data

  * texto/simples

A regra sobre cabeçalhos de solicitação se aplica aos cabeçalhos que o aplicativo define chamando setRequestHeader no objeto XMLHttpRequest. (A especificação CORS chama esses cabeçalhos de solicitação"autor".) A regra não se aplica aos cabeçalhos, que o navegador pode definir, como o agente do usuário, o Host ou o comprimento do conteúdo.

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

* Access-Control-Request-Headers: Uma lista de cabeçalhos de solicitação que o aplicativo definido na solicitação real. (Novamente, isso não inclui os cabeçalhos que define o navegador.)

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

A resposta inclui um cabeçalho Access-Control-Allow-Methods que lista os métodos permitidos e, opcionalmente, um cabeçalho Access-Control-Allow-Headers, que lista os cabeçalhos permitidos. Se a solicitação de simulação for bem-sucedida, o navegador envia a solicitação real, conforme descrito anteriormente.
