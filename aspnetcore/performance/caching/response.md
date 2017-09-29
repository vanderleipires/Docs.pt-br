---
title: "O cache de resposta no núcleo do ASP.NET"
author: rick-anderson
description: Saiba como usar o cache para reduzir a largura de banda e melhorar o desempenho de resposta.
keywords: "ASP.NET Core, cache, cabeçalhos HTTP de resposta"
ms.author: riande
manager: wpickett
ms.date: 09/20/2017
ms.topic: article
ms.assetid: cb42035a-60b0-472e-a614-cb79f443f654
ms.prod: asp.net-core
uid: performance/caching/response
ms.openlocfilehash: 957bdf5fe24216fa3459ac7ecee0464a45226828
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/28/2017
---
# <a name="response-caching-in-aspnet-core"></a>O cache de resposta no núcleo do ASP.NET

Por [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), e [Luke Latham](https://github.com/guardrex)

[Exibir ou baixar o código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/sample)

O cache de resposta reduz o número de solicitações de que um cliente ou um proxy faz a um servidor web. O cache de resposta também reduz a quantidade de trabalho do servidor web executa para gerar uma resposta. O cache de resposta é controlado por cabeçalhos que especifique como deseja middleware para respostas de cache de cliente e proxy.

O servidor web pode armazenar em cache respostas quando você adiciona [Middleware de cache de resposta](xref:performance/caching/middleware).

## <a name="http-based-response-caching"></a>O cache de resposta baseado em HTTP

O [especificação HTTP 1.1 cache](https://tools.ietf.org/html/rfc7234) descreve o comportamento do caches de Internet. O cabeçalho HTTP principal usado para armazenar em cache é [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), que é usado para especificar o cache *diretivas*. As diretivas de controlam o comportamento de cache conforme solicitações passarão de clientes para servidores e respostas passarão de servidores de volta aos clientes. Solicitações e respostas movem através de servidores proxy e servidores proxy também devem estar em conformidade com a especificação de cache do HTTP 1.1.

Comuns `Cache-Control` diretivas são mostradas na tabela a seguir.

| Diretiva                                                       | Ação |
| --------------------------------------------------------------- | ------ |
| [public](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | Um cache pode armazenar a resposta. |
| [private](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | A resposta não deve ser armazenada por um cache compartilhado. Um cache privado pode armazenar e reutilizar a resposta. |
| [idade máxima](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | O cliente não aceitará uma resposta cuja idade seja maior que o número especificado de segundos. Exemplos: `max-age=60` (60 segundos), `max-age=2592000` (mês) |
| [sem cache](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | **Em solicitações**: um cache não deve usar uma resposta armazenada para satisfazer a solicitação. Observação: O servidor de origem gera novamente a resposta para o cliente e o middleware atualiza a resposta armazenada em seu cache.<br><br>**As respostas**: A resposta não deve ser usada para uma solicitação subsequente sem validação no servidor de origem. |
| [Nenhum repositório](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | **Em solicitações**: um cache não deve armazenar a solicitação.<br><br>**As respostas**: um cache não deve armazenar qualquer parte da resposta. |

Outros cabeçalhos de cache que desempenham uma função no cache são mostrados na tabela a seguir.

| Cabeçalho                                                     | Função |
| ---------------------------------------------------------- | -------- |
| [Idade](https://tools.ietf.org/html/rfc7234#section-5.1)     | Uma estimativa da quantidade de tempo em segundos desde que a resposta foi gerada ou validada com êxito no servidor de origem. |
| [Expirar](https://tools.ietf.org/html/rfc7234#section-5.3) | A data/hora após o qual a resposta é considerada obsoleta. |
| [Pragma](https://tools.ietf.org/html/rfc7234#section-5.4)  | Existe para versões anteriores a compatibilidade com HTTP/1.0 armazena em cache para a configuração `no-cache` comportamento. Se o `Cache-Control` cabeçalho estiver presente, o `Pragma` cabeçalho será ignorado. |
| [Variar](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | Especifica que uma resposta em cache não deve ser enviada a menos que todos os do `Vary` correspondem a campos de cabeçalho na solicitação original da resposta em cache e a nova solicitação. |

## <a name="http-based-caching-respects-request-cache-control-directives"></a>Aspectos de cache com base em HTTP solicitar as diretivas de controle de Cache

O [especificação de cache do HTTP 1.1 para o cabeçalho Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2) requer um cache cumprir válido `Cache-Control` cabeçalho enviado pelo cliente. Um cliente pode fazer solicitações com uma `no-cache` valor de cabeçalho e forçar o servidor para gerar uma nova resposta para cada solicitação.

Sempre para respeitar cliente `Cache-Control` cabeçalhos de solicitação faz sentido se você considerar o objetivo do cache de HTTP. Sob a especificação oficial, cache destina-se para reduzir a sobrecarga de rede e latência de atender solicitações em uma rede de clientes, proxies e servidores. Não necessariamente é uma maneira de controlar a carga em um servidor de origem.

Não há nenhum atual desenvolvedor controle sobre o comportamento de cache ao usar o [Middleware de cache de resposta](xref:performance/caching/middleware) porque o middleware cumpre o oficial da especificação de cache. [Aperfeiçoamentos futuros para o middleware](https://github.com/aspnet/ResponseCaching/issues/96) permitirá a configurar o middleware para ignorar uma solicitação `Cache-Control` cabeçalho ao decidir servir uma resposta em cache. Isso oferecerá uma oportunidade para controlar melhor a carga no servidor quando você usar o middleware.

## <a name="other-caching-technology-in-aspnet-core"></a>Outras tecnologias de cache no núcleo do ASP.NET

### <a name="in-memory-caching"></a>O armazenamento em cache na memória

O armazenamento em cache na memória usa a memória do servidor para armazenar dados em cache. Esse tipo de cache é adequado para um único servidor ou vários servidores usando *sessões Autoadesivas*. Sessões Autoadesivas significa que as solicitações de um cliente sempre são roteadas para o mesmo servidor para processamento.

Para obter mais informações, consulte [Introdução ao cache na memória no ASP.NET Core](xref:performance/caching/memory).

### <a name="distributed-cache"></a>Cache distribuído

Use um cache distribuído para armazenar dados na memória quando o aplicativo é hospedado em um farm de servidor ou de nuvem. O cache é compartilhado entre os servidores que processam solicitações. Um cliente pode enviar uma solicitação que é tratada por qualquer servidor no grupo e os dados armazenados em cache para o cliente estão disponíveis. ASP.NET Core oferece o SQL Server e os caches Redis distribuído.

Para obter mais informações, consulte [trabalhando com um cache distribuído](xref:performance/caching/distributed).

### <a name="cache-tag-helper"></a>Auxiliar de marca de cache

Você pode armazenar em cache o conteúdo de um modo de exibição do MVC ou Razor de página com o auxiliar de marca de Cache. O auxiliar de marca de Cache usa o cache de memória para armazenar dados.

Para obter mais informações, consulte [auxiliar de marca de Cache no ASP.NET MVC de núcleo](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).

### <a name="distributed-cache-tag-helper"></a>Auxiliar de marca de Cache distribuído

Você pode armazenar em cache o conteúdo de um modo de exibição do MVC ou página de Razor em nuvem distribuído ou cenários de farm da web com o auxiliar de marca de Cache distribuído. O auxiliar de marca de Cache distribuído usa o SQL Server ou Redis para armazenar dados.

Para obter mais informações, consulte [auxiliar de marca de Cache distribuído](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).

## <a name="responsecache-attribute"></a>Atributo ResponseCache

O `ResponseCacheAttribute` Especifica os parâmetros necessários para definir os cabeçalhos apropriados no cache de resposta. Consulte [ResponseCacheAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.responsecacheattribute) para obter uma descrição dos parâmetros.

> [!WARNING]
> Desabilite o cache de conteúdo que contém informações para clientes autenticados. O cache deve ser habilitado apenas para o conteúdo que não são alterados com base na identidade do usuário ou se um usuário estiver conectado.

`VaryByQueryKeys string[]`(requer o ASP.NET Core 1.1 e posteriores): quando definido, o Middleware de cache de resposta a resposta armazenada varia de acordo com os valores de determinada lista de chaves de consulta. O Middleware de cache de resposta deve ser habilitado para definir o `VaryByQueryKeys` propriedade; caso contrário, uma exceção de tempo de execução é gerada. Não há nenhum cabeçalho HTTP correspondente para o `VaryByQueryKeys` propriedade. Essa propriedade é um recurso HTTP manipulado pelo Middleware de armazenamento em cache a resposta. Para o middleware servir uma resposta em cache, a cadeia de caracteres de consulta e o valor de cadeia de caracteres de consulta devem corresponder uma solicitação anterior. Por exemplo, considere a sequência de solicitações e resultados mostrados na tabela a seguir.

| Solicitação                          | Resultado                   |
| -------------------------------- | ------------------------ |
| `http://example.com?key1=value1` | retornado do servidor     |
| `http://example.com?key1=value1` | retornado de middleware |
| `http://example.com?key1=value2` | retornado do servidor     |

A primeira solicitação é retornada pelo servidor e armazenados em cache no middleware. A segunda solicitação é retornada pelo middleware porque a cadeia de caracteres de consulta corresponde a solicitação anterior. A terceira solicitação não está no cache de middleware porque o valor de cadeia de caracteres de consulta não corresponde a uma solicitação anterior. 

O `ResponseCacheAttribute` é usado para configurar e criar (por meio de `IFilterFactory`) um `ResponseCacheFilter`. O `ResponseCacheFilter` realiza o trabalho de atualização de cabeçalhos HTTP apropriados e os recursos da resposta. O filtro:

* Remove qualquer cabeçalho existente para `Vary`, `Cache-Control`, e `Pragma`. 
* Grava os cabeçalhos apropriados com base nas propriedades definidas no `ResponseCacheAttribute`. 
* Atualiza a resposta de armazenamento em cache o recurso HTTP se `VaryByQueryKeys` está definido.

### <a name="vary"></a>Variar

Esse cabeçalho é apenas gravado quando o `VaryByHeader` está definida. Ele é definido como o `Vary` valor da propriedade. O exemplo a seguir usa o `VaryByHeader` propriedade:

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

Você pode exibir os cabeçalhos de resposta com as ferramentas de rede do seu navegador. A imagem a seguir mostra o F12 borda que saída o **rede** guia quando o `About2` método de ação é atualizado:

![Na guia rede de borda F12 saída quando o método de ação About2 é chamado](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a>NoStore e Location.None

`NoStore`substitui a maioria das outras propriedades. Quando essa propriedade é definida como `true`, o `Cache-Control` cabeçalho é definido como `no-store`. Se `Location` é definido como `None`:

* `Cache-Control` é definido como `no-store,no-cache`.
* `Pragma` é definido como `no-cache`.

Se `NoStore` é `false` e `Location` é `None`, `Cache-Control` e `Pragma` são definidos como `no-cache`.

Você normalmente define `NoStore` para `true` nas páginas de erro. Por exemplo:

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

Isso resulta nos seguintes cabeçalhos:

```
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a>Local e duração

Para habilitar o cache, `Duration` deve ser definido como um valor positivo e `Location` devem ser `Any` (o padrão) ou `Client`. Nesse caso, o `Cache-Control` cabeçalho é definido como o valor do local seguido de `max-age` da resposta.

> [!NOTE]
> `Location`do opções de `Any` e `Client` se traduz em `Cache-Control` valores de cabeçalho de `public` e `private`, respectivamente. Conforme observado anteriormente, definindo `Location` para `None` define `Cache-Control` e `Pragma` cabeçalhos para `no-cache`.

Abaixo está um exemplo que mostra os cabeçalhos produzido definindo `Duration` e deixar o padrão `Location` valor:

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

Isso gera o cabeçalho a seguir:

```
Cache-Control: public,max-age=60
```

### <a name="cache-profiles"></a>Perfis de cache

Em vez de duplicar `ResponseCache` configurações em muitos atributos de ação do controlador, perfis de cache podem ser configuradas como opções ao configurar MVC no `ConfigureServices` método `Startup`. Valores encontrados em um perfil de cache de referência são usados como padrões pelo `ResponseCache` de atributos e são substituídos por qualquer propriedade especificada no atributo.

Configurando um perfil de cache:

[!code-csharp[Main](response/sample/Startup.cs?name=snippet1)] 

Um perfil de cache de referência:

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

O `ResponseCache` atributo pode ser aplicado a ações (métodos) e controladores (classes). Atributos de nível de método substituem as configurações especificadas em atributos de nível de classe.

No exemplo acima, um atributo de nível de classe especifica uma duração de 30 segundos, enquanto um atributo de nível de método faz referência a um perfil de cache com uma duração definida como 60 segundos.

O cabeçalho resultante:

```
Cache-Control: public,max-age=60
```

## <a name="additional-resources"></a>Recursos adicionais

* [Armazenamento em cache em HTTP da especificação](https://tools.ietf.org/html/rfc7234#section-3)
* [Controle de cache](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* [Middleware de Cache de Resposta](xref:performance/caching/middleware)
