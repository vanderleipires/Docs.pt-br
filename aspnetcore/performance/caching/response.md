---
title: O cache de resposta
author: rick-anderson
description: Explica como usar o cache para reduzir a largura de banda e melhorar o desempenho de resposta.
keywords: "ASP.NET Core, cache, cabeçalhos HTTP de resposta"
ms.author: riande
manager: wpickett
ms.date: 7/10/2017
ms.topic: article
ms.assetid: cb42035a-60b0-472e-a614-cb79f443f654
ms.prod: asp.net-core
uid: performance/caching/response
ms.openlocfilehash: 8b20ac70f257213ae3830749e729156ee5ab6447
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/12/2017
---
# <a name="response-caching"></a>O cache de resposta

Por [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), e [Steve Smith](https://ardalis.com/)

[Exibir ou baixar o código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/sample)

## <a name="what-is-response-caching"></a>O que é o cache de resposta

*O cache de resposta* adiciona cabeçalhos relacionados às respostas. Esses cabeçalhos especificam como você deseja o cliente, o proxy e o middleware para respostas de cache. O cache de resposta pode reduzir o número de solicitações que faz a um cliente ou um proxy para o servidor web. O cache de resposta também pode reduzir a quantidade de trabalho do servidor web executa para gerar a resposta. 

O cabeçalho HTTP principal usado para armazenar em cache é `Cache-Control`. Consulte o [HTTP 1.1 cache](https://tools.ietf.org/html/rfc7234#section-5.2) para obter mais informações. Diretivas de cache comuns:

* [public](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)
* [private](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)
* [sem cache](https://tools.ietf.org/html/rfc7234#section-5.2.1.4)
* [Pragma](https://tools.ietf.org/html/rfc7234#section-5.4)
* [Variar](https://tools.ietf.org/html/rfc7231#section-7.1.4)

O servidor web pode armazenar em cache respostas adicionando a resposta do cache de middleware. Consulte [resposta cache middleware](middleware.md) para obter mais informações.

## <a name="distributed-cache-tag-helper"></a>Auxiliar de marca de Cache distribuído

O [auxiliar de marca de Cache distribuído](xref:mvc/views/tag-helpers/builtin-th/DistributedCacheTagHelper) Habilita cache distribuído.


## <a name="responsecache-attribute"></a>Atributo ResponseCache

O `ResponseCacheAttribute` Especifica os parâmetros necessários para definir os cabeçalhos apropriados no cache de resposta. Consulte [ResponseCacheAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.responsecacheattribute) para obter uma descrição dos parâmetros.

>[!WARNING]
> Desabilite o cache de conteúdo que contém informações para clientes autenticados. Cache só deve ser habilitado para conteúdo que não muda com base na identidade do usuário, ou se um usuário está conectado.

`VaryByQueryKeys string[]`(requer o ASP.NET Core 1.1.0 e superior): quando definido, a resposta do cache de middleware irão variar a resposta armazenada pelos valores de determinada lista de chaves de consulta. A resposta do cache de middleware deve ser habilitada para definir o `VaryByQueryKeys` propriedade, caso contrário, uma exceção de tempo de execução será gerada. Não há nenhum cabeçalho HTTP correspondente para o `VaryByQueryKeys` propriedade. Essa propriedade é um recurso HTTP tratado pela resposta de cache de middleware. Para o middleware servir uma resposta em cache, a cadeia de caracteres de consulta e o valor de cadeia de caracteres de consulta devem corresponder uma solicitação anterior. Por exemplo, considere a seguinte sequência:

| Solicitação          | Resultado |
| ----------------- | ------------ | 
| `http://example.com?key1=value1` | retornado do servidor |
| `http://example.com?key1=value1` | retornado de middleware |
| `http://example.com?key1=value2` | retornado do servidor |

A primeira solicitação é retornada pelo servidor e armazenados em cache no middleware. A segunda solicitação é retornada pelo middleware porque a cadeia de caracteres de consulta corresponde a solicitação anterior. A terceira solicitação não está no cache de middleware porque o valor de cadeia de caracteres de consulta não corresponde a uma solicitação anterior. 

O `ResponseCacheAttribute` é usado para configurar e criar (por meio de `IFilterFactory`) um `ResponseCacheFilter`. O `ResponseCacheFilter` realiza o trabalho de atualização de cabeçalhos HTTP apropriados e os recursos da resposta. O filtro:

* Remove qualquer cabeçalho existente para `Vary`, `Cache-Control`, e `Pragma`. 
* Grava os cabeçalhos apropriados com base nas propriedades definidas no `ResponseCacheAttribute`. 
* Atualiza a resposta de armazenamento em cache o recurso HTTP se `VaryByQueryKeys` está definido.

### <a name="vary"></a>Variar

Esse cabeçalho é apenas gravado quando o `VaryByHeader` está definida. Ele é definido como o `Vary` valor da propriedade. O exemplo a seguir usa o `VaryByHeader` propriedade.

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

Você pode exibir os cabeçalhos de resposta com as ferramentas de rede de navegadores. A imagem a seguir mostra o F12 borda que saída o **rede** guia quando o `About2` método de ação é atualizado. 

![Borda F12 saída no * * * * de rede guia quando é chamado o método de ação 'About2'](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a>NoStore e Location.None

`NoStore`substitui a maioria das outras propriedades. Quando essa propriedade é definida como `true`, o `Cache-Control` cabeçalho será definido como "nenhum repositório". Se `Location` é definido como `None`:

* `Cache-Control` é definido como `"no-store, no-cache"`. 
* `Pragma` é definido como `no-cache`. 

Se `NoStore` é `false` e `Location` é `None`, `Cache-Control` e `Pragma` será definida como `no-cache`.

Você normalmente define `NoStore` para `true` nas páginas de erro. Por exemplo:

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

Isso fará com que os seguintes cabeçalhos:

```javascript
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a>Local e duração

Para habilitar o cache, `Duration` deve ser definido como um valor positivo e `Location` devem ser `Any` (o padrão) ou `Client`. Nesse caso, o `Cache-Control` cabeçalho será definido como o valor do local seguido de "max-age" da resposta.

> [!NOTE]
> `Location`do opções de `Any` e `Client` se traduz em `Cache-Control` valores de cabeçalho de `public` e `private`, respectivamente. Conforme observado anteriormente, definindo `Location` para `None` definirá ambos `Cache-Control` e `Pragma` cabeçalhos para `no-cache`.

Abaixo está um exemplo que mostra os cabeçalhos produzido definindo `Duration` e deixar o padrão `Location` valor.

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

Produz os seguintes cabeçalhos:

```javascript
Cache-Control: public,max-age=60
   ```

### <a name="cache-profiles"></a>Perfis de cache

Em vez de duplicar `ResponseCache` configurações em muitos atributos de ação do controlador, perfis de cache podem ser configuradas como opções ao configurar MVC no `ConfigureServices` método `Startup`. Valores encontrados em um perfil de cache referenciado serão usados como padrão pelo `ResponseCache` de atributos e será substituído por quaisquer propriedades especificadas no atributo.

Configurando um perfil de cache:

[!code-csharp[Main](response/sample/Startup.cs?name=snippet1)] 

Um perfil de cache de referência:

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

O `ResponseCache` atributo pode ser aplicado a ações (métodos), bem como controladores (classes). Atributos de nível de método substituirão as configurações especificadas em atributos de nível de classe.

No exemplo acima, um atributo de nível de classe especifica uma duração de 30 segundos, enquanto um atributos de nível de método faz referência a um perfil de cache com uma duração definida como 60 segundos.

O cabeçalho resultante:

```
Cache-Control: public,max-age=60
   ```

  ### <a name="additional-resources"></a>Recursos adicionais

* [Armazenamento em cache em HTTP da especificação](https://tools.ietf.org/html/rfc7234#section-3)
* [Controle de cache](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
