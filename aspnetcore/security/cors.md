---
title: Habilitar solicitações entre origens (CORS) no ASP.NET Core
author: rick-anderson
description: Saiba como CORS como padrão para permitir ou rejeitar solicitações entre origens em um aplicativo ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 09/05/2018
uid: security/cors
ms.openlocfilehash: cfbf24edb1dae76f676d51738b0d57266688d53e
ms.sourcegitcommit: 317f9be24db600499e79d25872d743af74bd86c0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/02/2018
ms.locfileid: "48045582"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a>Habilitar solicitações entre origens (CORS) no ASP.NET Core

Por [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), e [Tom Dykstra](https://github.com/tdykstra)

Segurança do navegador impede que uma página da web fazendo solicitações para um domínio diferente daquele que atendia a página da web. Essa restrição é chamada de *política de mesma origem*. A política de mesma origem impede que um site mal-intencionado leia dados confidenciais de outro site. Às vezes, você talvez queira permitir que outros sites fazem solicitações entre origens em seu aplicativo.

[Entre o compartilhamento de recursos de origem](https://www.w3.org/TR/cors/) (CORS) é um padrão W3C que permite que um servidor relaxar a política de mesma origem. Usando o CORS, um servidor pode explicitamente permitir algumas solicitações entre origens enquanto rejeita outras. O CORS é mais seguro e flexível que técnicas anteriores, tais como [JSONP](https://wikipedia.org/wiki/JSONP). Este tópico mostra como habilitar o CORS em um aplicativo ASP.NET Core.

## <a name="same-origin"></a>Mesma origem

Duas URLs têm a mesma origem se eles têm esquemas idênticas, hosts e portas ([6454 RFC](https://tools.ietf.org/html/rfc6454)).

Essas duas URLs têm a mesma origem:

* `https://example.com/foo.html`
* `https://example.com/bar.html`

Essas URLs têm diferentes origens que as duas URLs anteriores:

* `https://example.net` &ndash; Domínio diferente
* `https://www.example.com/foo.html` &ndash; Subdomínio diferente
* `http://example.com/foo.html` &ndash; Esquema diferente
* `https://example.com:9000/foo.html` &ndash; Porta diferente

> [!NOTE]
> Internet Explorer não considera a porta ao comparar as origens.

## <a name="register-cors-services"></a>Registrar os serviços do CORS

::: moniker range=">= aspnetcore-2.1"

Referência a [metapacote do Microsoft](xref:fundamentals/metapackage-app) ou adicionar uma referência de pacote para o [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) pacote.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Referência a [metapacote Microsoft.AspNetCore.All](xref:fundamentals/metapackage) ou adicionar uma referência de pacote para o [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) pacote.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Adicionar uma referência de pacote para o [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) pacote.

::: moniker-end

Chame <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> em `Startup.ConfigureServices` para adicionar serviços do CORS ao contêiner de serviço do aplicativo:

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors&highlight=3)]

## <a name="enable-cors"></a>Habilitar o CORS

Depois de registrar os serviços do CORS, use qualquer uma das abordagens a seguir para habilitar o CORS em um aplicativo ASP.NET Core:

* [Middleware do CORS](#enable-cors-with-cors-middleware) &ndash; políticas CORS se aplicam globalmente para o aplicativo por meio do middleware.
* [O CORS no MVC](#enable-cors-in-mvc) &ndash; políticas aplicar CORS por controlador ou por ação. Middleware CORS não é usada.

### <a name="enable-cors-with-cors-middleware"></a>Habilitar o CORS com Middleware CORS

Middleware CORS manipula solicitações entre origens para o aplicativo. Para habilitar o CORS Middleware no pipeline de processamento de solicitação, chame o <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> método de extensão no `Startup.Configure`.

Middleware CORS devem preceder quaisquer pontos de extremidade definidos em seu aplicativo onde você deseja dar suporte a solicitações entre origens (por exemplo, antes de chamar `UseMvc` de Middleware de páginas do Razor/MVC).

Um *política entre origens* pode ser especificado ao adicionar o Middleware CORS usando o <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> classe. Há duas abordagens para definir uma política CORS:

* Chamar `UseCors` com um lambda:

  [!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

  O lambda utiliza um <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> objeto. [Opções de configuração](#cors-policy-options), tais como `WithOrigins`, são descritas posteriormente neste tópico. No exemplo anterior, a política permite que solicitações entre origens de `https://example.com` e sem outras origens.

  A URL deve ser especificada sem uma barra à direita (`/`). Se a URL termina com `/`, a comparação retorna `false` e nenhum cabeçalho é retornado.

  `CorsPolicyBuilder` tem uma API fluente, portanto, é possível encadear chamadas de método:

  [!code-csharp[](cors/sample/CorsExample3/Startup.cs?highlight=2-3&range=29-32)]

* Definir uma ou mais políticas CORS e selecione a política por nome em tempo de execução. O exemplo a seguir adiciona uma política CORS definida pelo usuário chamada *AllowSpecificOrigin*. Para selecionar a política, passe o nome para `UseCors`:

  [!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin&highlight=5-6,21)]

### <a name="enable-cors-in-mvc"></a>Habilitar o CORS no MVC

Como alternativa, você pode usar o MVC para aplicar políticas CORS específicas por ação ou por controlador. Ao usar o MVC para habilitar o CORS, os serviços registrados de CORS são usados. O Middleware do CORS não é usado.

### <a name="per-action"></a>Por ação

Para especificar uma política CORS para uma ação específica, adicione a [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) atributo à ação. Especifique o nome da política.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction&highlight=2)]

### <a name="per-controller"></a>Por controlador

Para especificar a política CORS para um controlador específico, adicione a [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) atributo à classe do controlador. Especifique o nome da política.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController&highlight=2)]

A ordem de precedência é:

1. ação
1. controlador

### <a name="disable-cors"></a>Desabilitar o CORS

Para desabilitar CORS para um controlador ou ação, use o [ &lbrack;DisableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) atributo:

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction&highlight=2)]

## <a name="cors-policy-options"></a>Opções de política de CORS

Esta seção descreve as várias opções que podem ser definidas em uma política CORS.

* [Defina as origens permitidas](#set-the-allowed-origins)
* [Defina os métodos HTTP permitidos](#set-the-allowed-http-methods)
* [Definir os cabeçalhos de solicitação permitido](#set-the-allowed-request-headers)
* [Definir os cabeçalhos de resposta exposto](#set-the-exposed-response-headers)
* [Credenciais nas solicitações entre origens](#credentials-in-cross-origin-requests)
* [Definir o tempo de expiração de simulação](#set-the-preflight-expiration-time)

Para algumas opções, pode ser útil ler o [funciona como o CORS](#how-cors-works) seção pela primeira vez.

### <a name="set-the-allowed-origins"></a>Defina as origens permitidas

Para permitir que um ou mais origens específicas, chame <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithOrigins*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=20-24&highlight=4)]

Para permitir todas as origens, chamar <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=28-32&highlight=4)]

Considere cuidadosamente antes de permitir que solicitações de qualquer origem. Permitir solicitações de qualquer origem significa que *de qualquer site* pode fazer solicitações entre origens ao seu aplicativo.

Essa configuração afeta [comprovação solicitações e o cabeçalho Access-Control-Allow-Origin](#preflight-requests) (descrito posteriormente neste tópico).

### <a name="set-the-allowed-http-methods"></a>Defina os métodos HTTP permitidos

Para permitir que todos os métodos HTTP, chamar <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=45-50&highlight=5)]

Essa configuração afeta [comprovação solicitações e o cabeçalho Access-Control-Allow-Methods](#preflight-requests) (descrito posteriormente neste tópico).

### <a name="set-the-allowed-request-headers"></a>Definir os cabeçalhos de solicitação permitido

Para permitir que os cabeçalhos específicos a serem enviados em uma solicitação CORS, chamado *criar cabeçalhos de solicitação*, chame <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> e especificar os cabeçalhos permitidos:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=54-59&highlight=5)]

Para permitir que todos os cabeçalhos de solicitação do autor chamar <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=63-68&highlight=5)]

Essa configuração afeta [comprovação solicitações e o cabeçalho Access-Control-Request-Headers](#preflight-requests) (descrito posteriormente neste tópico).

::: moniker range=">= aspnetcore-2.2"

Uma correspondência de política de Middleware do CORS para cabeçalhos específicos especificado por `WithHeaders` só é possível quando os cabeçalhos enviados `Access-Control-Request-Headers` coincidir exatamente com os cabeçalhos indicados na `WithHeaders`.

Por exemplo, considere um aplicativo configurado da seguinte maneira:

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

Middleware CORS recusar uma solicitação de simulação com o seguinte cabeçalho de solicitação, pois `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) não está listado no `WithHeaders`:

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

O aplicativo retorna um *200 Okey* resposta, mas não envia de volta os cabeçalhos de CORS. Portanto, o navegador não tente a solicitação entre origens.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Middleware CORS sempre permite que os cabeçalhos de quatro no `Access-Control-Request-Headers` a ser enviado, independentemente dos valores configurados no CorsPolicy.Headers. Esta lista de cabeçalhos inclui:

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

Por exemplo, considere um aplicativo configurado da seguinte maneira:

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

Middleware CORS responde com êxito a uma solicitação de simulação com o seguinte cabeçalho de solicitação porque `Content-Language` é sempre na lista de permissões:

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a>Definir os cabeçalhos de resposta exposto

Por padrão, o navegador não expõe todos os cabeçalhos de resposta para o aplicativo. Para obter mais informações, consulte [W3C Cross-Origin Resource Sharing (terminologia): o cabeçalho de resposta simples](https://www.w3.org/TR/cors/#simple-response-header).

Os cabeçalhos de resposta que estão disponíveis por padrão são:

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

A especificação CORS chama esses cabeçalhos *cabeçalhos de resposta simples*. Para disponibilizar outros cabeçalhos para o aplicativo, chame <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=72-77&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a>Credenciais nas solicitações entre origens

Credenciais exigem tratamento especial em uma solicitação CORS. Por padrão, o navegador não envia as credenciais com uma solicitação entre origens. As credenciais incluem cookies e esquemas de autenticação HTTP. Para enviar as credenciais com uma solicitação entre origens, o cliente deve definir `XMLHttpRequest.withCredentials` para `true`.

Usando `XMLHttpRequest` diretamente:

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

No jQuery:

```jQuery
$.ajax({
  type: 'get',
  url: 'https://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

Além disso, o servidor deve permitir que as credenciais. Para permitir credenciais entre origens, chamar <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=81-86&highlight=5)]

A resposta HTTP inclui um `Access-Control-Allow-Credentials` cabeçalho, que informa ao navegador que o servidor permite que as credenciais para uma solicitação entre origens.

Se o navegador envia as credenciais, mas a resposta não inclui um válido `Access-Control-Allow-Credentials` cabeçalho, o navegador não expõe a resposta para o aplicativo e a solicitação entre origens falhar.

Tenha cuidado ao permitindo credenciais entre origens. Um site em outro domínio pode enviar credenciais do usuário conectado para o aplicativo em nome do usuário sem o conhecimento do usuário.

A especificação CORS também declara que a configuração origens `"*"` (todas as origens) é inválida se o `Access-Control-Allow-Credentials` cabeçalho está presente.

### <a name="preflight-requests"></a>Solicitações de simulação

Para algumas solicitações CORS, o navegador envia uma solicitação adicional antes de fazer a solicitação real. Essa solicitação é chamada de um *solicitação de simulação*. O navegador pode ignorar a solicitação de simulação se as seguintes condições forem verdadeiras:

* O método de solicitação é GET, HEAD ou POST.
* O aplicativo não define cabeçalhos de solicitação diferente de `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, ou `Last-Event-ID`.
* O `Content-Type` cabeçalho, se definido, tem um dos seguintes valores:
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

A regra em cabeçalhos de solicitação definido para a solicitação do cliente se aplica aos cabeçalhos que o aplicativo define chamando `setRequestHeader` sobre o `XMLHttpRequest` objeto. A especificação CORS chama esses cabeçalhos *criar cabeçalhos de solicitação*. A regra não se aplica aos cabeçalhos, o navegador pode definir, tais como `User-Agent`, `Host`, ou `Content-Length`.

Este é um exemplo de uma solicitação de simulação:

```
OPTIONS https://myservice.azurewebsites.net/api/test HTTP/1.1
Accept: */*
Origin: https://myclient.azurewebsites.net
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: accept, x-my-custom-header
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
Content-Length: 0
```

A solicitação de simulação usa o método HTTP OPTIONS. Ele inclui dois cabeçalhos especiais:

* `Access-Control-Request-Method`: O método HTTP que será usado para a solicitação real.
* `Access-Control-Request-Headers`: Uma lista de cabeçalhos de solicitação que o aplicativo define a solicitação real. Conforme mencionado anteriormente, isso não inclui os cabeçalhos que define o navegador, tais como `User-Agent`.

Uma solicitação de simulação de CORS pode incluir um `Access-Control-Request-Headers` cabeçalho, que indica ao servidor, os cabeçalhos que são enviados com a solicitação real.

Para permitir que os cabeçalhos específicos, chame <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=54-59&highlight=5)]

Para permitir que todos os cabeçalhos de solicitação do autor chamar <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=63-68&highlight=5)]

Navegadores não estão totalmente consistentes em como eles definir `Access-Control-Request-Headers`. Se você definir os cabeçalhos para algo diferente de `"*"` (ou use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), você deve incluir pelo menos `Accept`, `Content-Type`, e `Origin`, além de quaisquer cabeçalhos personalizados que você deseja dar suporte.

Este é um exemplo de resposta à solicitação de simulação (supondo que o servidor permite que a solicitação):

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Access-Control-Allow-Origin: https://myclient.azurewebsites.net
Access-Control-Allow-Headers: x-my-custom-header
Access-Control-Allow-Methods: PUT
Date: Wed, 20 May 2015 06:33:22 GMT
```

A resposta inclui um `Access-Control-Allow-Methods` cabeçalho que lista os métodos permitidos e, opcionalmente, um `Access-Control-Allow-Headers` cabeçalho, que lista os cabeçalhos permitidos. Se a solicitação de simulação for bem-sucedida, o navegador envia a solicitação real.

Se a solicitação de simulação for negada, o aplicativo retornar um *200 Okey* resposta, mas não envia de volta os cabeçalhos de CORS. Portanto, o navegador não tente a solicitação entre origens.

### <a name="set-the-preflight-expiration-time"></a>Definir o tempo de expiração de simulação

O `Access-Control-Max-Age` cabeçalho Especifica quanto tempo a resposta à solicitação de simulação pode ser armazenados em cache. Para definir esse cabeçalho, chame <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=90-95&highlight=5)]

## <a name="how-cors-works"></a>Como funciona o CORS

Esta seção descreve o que acontece em uma solicitação CORS no nível das mensagens HTTP. É importante entender o funcionamento de CORS para que a política CORS pode ser configurada corretamente e depurada quando comportamentos inesperados ocorrerem.

A especificação CORS apresenta vários novos cabeçalhos HTTP que permitem que solicitações entre origens. Se um navegador oferece suporte a CORS, ele define esses cabeçalhos automaticamente para solicitações entre origens. Código JavaScript personalizado não é necessário para habilitar o CORS.

O exemplo a seguir é um exemplo de uma solicitação entre origens. O `Origin` cabeçalho fornece o domínio do site que está fazendo a solicitação:

```
GET https://myservice.azurewebsites.net/api/test HTTP/1.1
Referer: https://myclient.azurewebsites.net/
Accept: */*
Accept-Language: en-US
Origin: https://myclient.azurewebsites.net
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
```

Se o servidor permite que a solicitação, ele define o `Access-Control-Allow-Origin` cabeçalho na resposta. O valor desse cabeçalho também corresponde a `Origin` cabeçalho da solicitação ou é o valor de curinga `"*"`, o que significa que qualquer origem é permitida:

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Type: text/plain; charset=utf-8
Access-Control-Allow-Origin: https://myclient.azurewebsites.net
Date: Wed, 20 May 2015 06:27:30 GMT
Content-Length: 12

Test message
```

Se a resposta não inclui o `Access-Control-Allow-Origin` falha do cabeçalho, a solicitação entre origens. Especificamente, o navegador não permite a solicitação. Mesmo se o servidor retorna uma resposta bem-sucedida, o navegador não disponibiliza a resposta para o aplicativo cliente.

## <a name="additional-resources"></a>Recursos adicionais

* [Cross-Origin Resource Sharing (CORS)](https://developer.mozilla.org/docs/Web/HTTP/CORS)
