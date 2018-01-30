---
title: "Resposta de cache Middleware no núcleo do ASP.NET"
author: guardrex
description: "Saiba como configurar e usar o Middleware de cache de resposta no núcleo do ASP.NET."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 01/26/2017
ms.topic: article
ms.prod: asp.net-core
uid: performance/caching/middleware
ms.openlocfilehash: a355dda0fdffe1e936c119ffb477c2817f28bd9c
ms.sourcegitcommit: 18ff1fdaa3e1ae204ed6a2ba9351ce8cf1371c85
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/29/2018
---
# <a name="response-caching-middleware-in-aspnet-core"></a>Resposta de cache Middleware no núcleo do ASP.NET

Por [Luke Latham](https://github.com/guardrex) e [John Luo](https://github.com/JunTaoLuo)

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/middleware/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

Este artigo explica como configurar o Middleware de cache de resposta em um aplicativo do ASP.NET Core. O middleware determina quando as respostas são armazenável em cache, repositórios de respostas e respostas de serve de cache. Para obter uma introdução ao cache de HTTP e o `ResponseCache` de atributo, consulte [cache de resposta](xref:performance/caching/response).

## <a name="package"></a>Pacote

Para incluir o middleware em um projeto, adicione uma referência para o [ `Microsoft.AspNetCore.ResponseCaching` ](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) pacote ou use o [ `Microsoft.AspNetCore.All` ](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) pacote (ASP.NET Core 2.0 ou posterior durante o direcionamento do .NET Core).

## <a name="configuration"></a>Configuração

Em `ConfigureServices`, adicione o middleware para a coleção de serviço.

[!code-csharp[Main](middleware/sample/Startup.cs?name=snippet1&highlight=3)]

Configurar o aplicativo para usar o middleware com o `UseResponseCaching` método de extensão, o que adiciona o middleware para o pipeline de processamento de solicitação. O aplicativo de exemplo adiciona um [ `Cache-Control` ](https://tools.ietf.org/html/rfc7234#section-5.2) cabeçalho para a resposta que armazena em cache respostas armazenável em cache por até 10 segundos. O exemplo envia um [ `Vary` ](https://tools.ietf.org/html/rfc7231#section-7.1.4) cabeçalho para configurar o middleware para servir uma resposta em cache somente se o [ `Accept-Encoding` ](https://tools.ietf.org/html/rfc7231#section-5.3.4) cabeçalho de solicitações subsequentes corresponde a solicitação original.

[!code-csharp[Main](middleware/sample/Startup.cs?name=snippet2&highlight=3,7-12)]

Middleware de cache de resposta só armazena em cache as respostas do servidor que resultam em um código de status 200 (Okey). Outras respostas, incluindo [páginas de erro](xref:fundamentals/error-handling), são ignorados pelo middleware.

> [!WARNING]
> Respostas que contêm o conteúdo para clientes autenticados devem ser marcadas como não armazenável em cache para evitar que o middleware de armazenamento e que atende a essas respostas. Consulte [condições para armazenar em cache](#conditions-for-caching) para obter detalhes sobre como o middleware determina se uma resposta pode ser armazenada em cache.

## <a name="options"></a>Opções

O middleware oferece três opções para controlar o cache de resposta.

| Opção                | Valor padrão |
| --------------------- | ------------- |
| UseCaseSensitivePaths | Determina se as respostas são armazenados em cache nos caminhos diferencia maiusculas de minúsculas.</p><p>O valor padrão é `false`. |
| MaximumBodySize       | O maior tamanho armazenável em cache para o corpo da resposta em bytes.</p>O valor padrão é `64 * 1024 * 1024` (64 MB). |
| SizeLimit             | O limite de tamanho para o middleware de cache de resposta em bytes. O valor padrão é `100 * 1024 * 1024` (100 MB). |

O exemplo a seguir configura o middleware para:

* Respostas de cache menores ou iguais a 1.024 bytes.
* Armazenar as respostas de caminhos diferencia maiusculas de minúsculas (por exemplo, `/page1` e `/Page1` são armazenadas separadamente).

```csharp
services.AddResponseCaching(options =>
{
    options.UseCaseSensitivePaths = true;
    options.MaximumBodySize = 1024;
});
```

## <a name="varybyquerykeys"></a>VaryByQueryKeys

Ao usar controladores de API MVC/da Web ou modelos de página de páginas Razor, o `ResponseCache` atributo especifica os parâmetros necessários para definir os cabeçalhos apropriados para o cache de resposta. O único parâmetro do `ResponseCache` atributo estritamente requer o middleware é `VaryByQueryKeys`, que não corresponde a um cabeçalho HTTP real. Para obter mais informações, consulte [ResponseCache atributo](xref:performance/caching/response#responsecache-attribute).

Quando não estiver usando o `ResponseCache` atributo, o cache de resposta pode variar com o `VaryByQueryKeys` recurso. Use o `ResponseCachingFeature` diretamente a partir de `IFeatureCollection` do `HttpContext`:

```csharp
var responseCachingFeature = context.HttpContext.Features.Get<IResponseCachingFeature>();
if (responseCachingFeature != null)
{
    responseCachingFeature.VaryByQueryKeys = new[] { "MyKey" };
}
```

## <a name="http-headers-used-by-response-caching-middleware"></a>Cabeçalhos HTTP usados pelo Middleware de cache de resposta

O cache de resposta pelo middleware é configurado usando cabeçalhos HTTP.

| Cabeçalho | Detalhes |
| ------ | ------- |
| Autorização | A resposta não está armazenada em cache se o cabeçalho existe. |
| Cache-Control | O middleware só considera o armazenamento em cache respostas marcadas com o `public` diretiva de cache. Controlam o cache com os seguintes parâmetros:<ul><li>max-age</li><li>max-stale&#8224;</li><li>nova min</li><li>must-revalidate</li><li>no-cache</li><li>Nenhum repositório</li><li>only-if-cached</li><li>particulares</li><li>públicos</li><li>s-maxage</li><li>proxy-revalidate&#8225;</li></ul>&#8224; se nenhum limite é especificado para `max-stale`, o middleware não executa nenhuma ação.<br>&#8225; `proxy-revalidate` tem o mesmo efeito que `must-revalidate`.<br><br>Para obter mais informações, consulte [RFC 7231: diretivas de controle de Cache de solicitação](https://tools.ietf.org/html/rfc7234#section-5.2.1). |
| Pragma | Um `Pragma: no-cache` cabeçalho na solicitação produz o mesmo efeito que `Cache-Control: no-cache`. Esse cabeçalho é substituído pelas diretivas desse relevantes a `Cache-Control` cabeçalho, se presente. Considerado para compatibilidade com versões anteriores com HTTP/1.0. |
| Set-Cookie | A resposta não está armazenada em cache se o cabeçalho existe. |
| Variar | O `Vary` cabeçalho é usado para variar a resposta armazenada em cache por outro cabeçalho. Por exemplo, armazenar em cache respostas de codificação, incluindo o `Vary: Accept-Encoding` cabeçalho, que armazena em cache as respostas para solicitações com cabeçalhos `Accept-Encoding: gzip` e `Accept-Encoding: text/plain` separadamente. Uma resposta com um valor de cabeçalho de `*` nunca é armazenada. |
| Expirar | Uma resposta considerada obsoleta por esse cabeçalho não está armazenada ou recuperada a menos que substituído por outros `Cache-Control` cabeçalhos. |
| If-None-Match | A resposta completa é servida do cache se o valor não é `*` e `ETag` da resposta não corresponde a nenhum dos valores fornecidos. Caso contrário, será fornecida uma resposta 304 (não modificados). |
| If-Modified-Since | Se o `If-None-Match` cabeçalho não estiver presente, uma resposta completa é servida do cache se a data de resposta em cache é mais recente do que o valor fornecido. Caso contrário, será fornecida uma resposta 304 (não modificados). |
| Date | Quando atendendo do cache, o `Date` cabeçalho é definido pelo middleware se ele não foi fornecido na resposta original. |
| Tamanho do conteúdo | Quando atendendo do cache, o `Content-Length` cabeçalho é definido pelo middleware se ele não foi fornecido na resposta original. |
| Idade | O `Age` cabeçalho enviado na resposta original será ignorado. O middleware calcula um novo valor ao oferecer uma resposta em cache. |

## <a name="caching-respects-request-cache-control-directives"></a>Cache respeita as diretivas de solicitação de controle de Cache

O middleware respeita as regras de [especificação HTTP 1.1 cache](https://tools.ietf.org/html/rfc7234#section-5.2). As regras exigem um cache cumprir válido `Cache-Control` cabeçalho enviado pelo cliente. Sob a especificação de um cliente pode fazer solicitações com uma `no-cache` valor de cabeçalho e forçar o servidor para gerar uma nova resposta para cada solicitação. Atualmente, não há nenhum controle de desenvolvedor sobre o comportamento de cache ao usar o middleware porque o middleware segue a especificação oficial do cache.

Para obter mais controle sobre o comportamento do cache, explore outros recursos de cache do ASP.NET Core. Consulte os tópicos a seguir:

* [Cache in-memory](xref:performance/caching/memory)
* [Trabalhando com um cache distribuído](xref:performance/caching/distributed)
* [Cache auxiliar de marca no núcleo do ASP.NET MVC](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Auxiliar de marca de cache distribuído](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)

## <a name="troubleshooting"></a>Solução de problemas

Se o comportamento do cache não está conforme o esperado, confirme se as respostas são armazenável em cache e é capaz de servido do cache. Examine os cabeçalhos de entrada da solicitação e cabeçalhos de saída da resposta. Habilitar [log](xref:fundamentals/logging/index) para ajudar na depuração.

Quando testar e solucionar problemas de comportamento de cache, um navegador pode definir cabeçalhos de solicitação que afetam o cache de maneira indesejada. Por exemplo, um navegador pode definir o `Cache-Control` cabeçalho para `no-cache` ou `max-age=0` ao atualizar uma página. As ferramentas a seguir podem definir explicitamente os cabeçalhos de solicitação e são preferenciais para testes de armazenamento em cache:

* [Fiddler](https://www.telerik.com/fiddler)
* [Postman](https://www.getpostman.com/)

### <a name="conditions-for-caching"></a>Condições para armazenar em cache

* A solicitação deve resultar em uma resposta do servidor com um código de status 200 (Okey).
* O método de solicitação deve ser GET ou HEAD.
* Middleware de terminal, como [Middleware de arquivo estático](xref:fundamentals/static-files), não deve processar a resposta antes do Middleware de cache de resposta.
* O `Authorization` cabeçalho não deve estar presente.
* `Cache-Control`parâmetros de cabeçalho devem ser válidos, e a resposta deve ser marcada `public` e não marcado `private`.
* O `Pragma: no-cache` cabeçalho não deve estar presente se a `Cache-Control` cabeçalho não estiver presente, como o `Cache-Control` cabeçalho substitui o `Pragma` cabeçalho quando presentes.
* O `Set-Cookie` cabeçalho não deve estar presente.
* `Vary`parâmetros de cabeçalho devem ser válido e não é igual a `*`.
* O `Content-Length` valor de cabeçalho (se definido) deve corresponder ao tamanho do corpo da resposta.
* O [IHttpSendFileFeature](/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpsendfilefeature) não é usado.
* A resposta não deve ser atualizada conforme especificado pelo `Expires` cabeçalho e o `max-age` e `s-maxage` diretivas de cache.
* Buffer de resposta deve ser bem-sucedida e o tamanho da resposta deve ser menor do que o configurado ou padrão `SizeLimit`.
* A resposta deve ser armazenado em cache de acordo com o [7234 RFC](https://tools.ietf.org/html/rfc7234) especificações. Por exemplo, o `no-store` diretiva não deve existir nos campos de cabeçalho de solicitação ou resposta. Consulte *seção 3: armazenar respostas em Caches* de [7234 RFC](https://tools.ietf.org/html/rfc7234) para obter detalhes.

> [!NOTE]
> O sistema Antiforgery para gerar tokens de seguras para evitar a falsificação de solicitação entre sites (CSRF) ataques de conjuntos de `Cache-Control` e `Pragma` cabeçalhos para `no-cache` para que as respostas não são armazenadas em cache.

## <a name="additional-resources"></a>Recursos adicionais

* [Inicialização de aplicativos](xref:fundamentals/startup)
* [Middleware](xref:fundamentals/middleware)
* [Cache in-memory](xref:performance/caching/memory)
* [Trabalhando com um cache distribuído](xref:performance/caching/distributed)
* [Detectar alterações com tokens de alteração](xref:fundamentals/primitives/change-tokens)
* [Cache de resposta](xref:performance/caching/response)
* [Auxiliar de marca de cache](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Auxiliar de marca de cache distribuído](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
