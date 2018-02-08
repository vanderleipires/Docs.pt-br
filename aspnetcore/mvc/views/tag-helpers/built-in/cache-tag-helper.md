---
title: Auxiliar de Marca de Cache no ASP.NET Core MVC
author: pkellner
description: Mostra como trabalhar com o Auxiliar de Marca de Cache
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: 51811ee1669a24a0fc4ce9bc67e782b61bff655c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a>Auxiliar de Marca de Cache no ASP.NET Core MVC

Por [Peter Kellner](http://peterkellner.net) 

O Auxiliar de Marca de Cache possibilita melhorar consideravelmente o desempenho de seu aplicativo ASP.NET Core armazenando seu conteúdo em cache no provedor de cache interno do ASP.NET Core.

O Mecanismo de Exibição do Razor define o `expires-after` padrão como vinte minutos.

A seguinte marcação Razor armazena em cache a data/hora:

```cshtml
<cache>@DateTime.Now</cache>
```

A primeira solicitação para a página que contém `CacheTagHelper` exibirá a data/hora atual. Solicitações adicionais mostrarão o valor armazenado em cache até que o cache expire (padrão de 20 minutos) ou seja removido por demanda de memória.

Você pode definir a duração do cache com os seguintes atributos:

## <a name="cache-tag-helper-attributes"></a>Atributos do Auxiliar de Marca de Cache

- - -

### <a name="enabled"></a>habilitado    


| Tipo de atributo    | Valores válidos      |
|----------------   |----------------   |
| boolean           | "true" (padrão)  |
|                   | "false"   |


Determina se o conteúdo circundado pelo Auxiliar de Marca de Cache é armazenado em cache. O padrão é `true`.  Se for definido como `false`, o Auxiliar de Marca de Cache não terá efeito de armazenamento em cache na saída renderizada.

Exemplo:

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-on"></a>expires-on 

| Tipo de atributo    | Valor de exemplo     |
|----------------   |----------------   |
| DateTimeOffset    | "@new DateTime(2025,1,29,17,02,0)"    |


Define uma data de expiração absoluta. O exemplo a seguir armazenará em cache o conteúdo do Auxiliar de Marca de Cache até 17:02 de 29 de janeiro de 2025.

Exemplo:

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-after"></a>expires-after

| Tipo de atributo    | Valor de exemplo     |
|----------------   |----------------   |
| TimeSpan    | "@TimeSpan.FromSeconds(120)"    |


Define o tempo decorrido desde a primeira solicitação para armazenar o conteúdo em cache. 

Exemplo:

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-sliding"></a>expires-sliding

| Tipo de atributo    | Valor de exemplo     |
|----------------   |----------------   |
| TimeSpan    | "@TimeSpan.FromSeconds(60)"     |


Define a hora em que uma entrada de cache deve ser removida se não tiver sido acessada.

Exemplo:

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-header"></a>vary-by-header

| Tipo de atributo    | Valores de exemplo                |
|----------------   |----------------               |
| Cadeia de Caracteres            | "User-Agent"                  |
|                   | "User-Agent,content-encoding" |

Aceita um único valor de cabeçalho ou uma lista separada por vírgulas de valores de cabeçalho que disparam uma atualização do cache quando são alterados. O exemplo a seguir monitora o valor do cabeçalho `User-Agent`. O exemplo armazenará em cache o conteúdo para todos os `User-Agent` diferentes apresentado ao servidor Web.

Exemplo:

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-query"></a>vary-by-query

| Tipo de atributo    | Valores de exemplo                |
|----------------   |----------------               |
| Cadeia de Caracteres            | "Make"                |
|                   | "Make,Model" |

Aceita um único valor de cabeçalho ou uma lista separada por vírgulas de valores de cabeçalho que disparam uma atualização do cache quando o valor do cabeçalho é alterado. O exemplo a seguir examina os valores de `Make` e `Model`.

Exemplo:

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-route"></a>vary-by-route

| Tipo de atributo    | Valores de exemplo                |
|----------------   |----------------               |
| Cadeia de Caracteres            | "Make"                |
|                   | "Make,Model" |

Aceita um único valor de cabeçalho ou uma lista separada por vírgulas de valores de cabeçalho que disparam uma atualização do cache quando os valores do parâmetro de dados de rota do cabeçalho são alterados. Exemplo:

*Startup.cs* 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```
  
*Index.cshtml*

```cshtml
<cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-cookie"></a>vary-by-cookie

| Tipo de atributo    | Valores de exemplo                |
|----------------   |----------------               |
| Cadeia de Caracteres            | ".AspNetCore.Identity.Application"                |
|                   | ".AspNetCore.Identity.Application,HairColor" |

Aceita um único valor de cabeçalho ou uma lista separada por vírgulas de valores de cabeçalho que disparam uma atualização do cache quando os valores do cabeçalho são alterados. O exemplo a seguir examina o cookie associado à identidade do ASP.NET. Quando um usuário é autenticado, o cookie de solicitação é definido, o que dispara uma atualização do cache.

Exemplo:

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-user"></a>vary-by-user

| Tipo de atributo    | Valores de exemplo                |
|----------------   |----------------               |
| Boolean             | "true"                  |
|                     | "false" (padrão) |

Especifica se o cache deve ou não ser redefinido quando o usuário conectado (ou a entidade de contexto) é alterado. O usuário atual também é conhecido como a Entidade do contexto de solicitação e pode ser exibido em um modo de exibição do Razor referenciando `@User.Identity.Name`.

O exemplo a seguir examina o usuário conectado no momento.  

Exemplo:

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

Usar esse atributo mantém o conteúdo no cache durante um ciclo de logon e logoff.  Ao usar `vary-by-user="true"`, uma ação de logon e logoff invalida o cache para o usuário autenticado.  O cache é invalidado porque um novo valor de cookie exclusivo é gerado no logon. O cache é mantido para o estado anônimo quando nenhum cookie está presente ou expirou. Isso significa que se nenhum usuário estiver conectado, o cache será mantido.

- - -

### <a name="vary-by"></a>vary-by

| Tipo de atributo    | Valores de exemplo                |
|----------------   |----------------               |
| Cadeia de Caracteres             | "@Model"                 |


Permite a personalização de quais dados são armazenados no cache. Quando o objeto referenciado pelo valor de cadeia de caracteres do atributo é alterado, o conteúdo do Auxiliar de Marca de Cache é atualizado. Frequentemente, uma concatenação de cadeia de caracteres de valores do modelo é atribuída a este atributo.  Na verdade, isso significa que uma atualização de qualquer um dos valores concatenados invalida o cache.

O exemplo a seguir supõe que o método do controlador que renderiza a exibição somas o valor inteiro dos dois parâmetros de rota, `myParam1` e `myParam2`, e os retorna como a propriedade de modelo única. Quando essa soma é alterada, o conteúdo do Auxiliar de Marca de Cache é renderizado e armazenado em cache novamente.  

Exemplo:

Ação:

```csharp
public IActionResult Index(string myParam1,string myParam2,string myParam3)
{
    int num1;
    int num2;
    int.TryParse(myParam1, out num1);
    int.TryParse(myParam2, out num2);
    return View(viewName, num1 + num2);
}
```

*Index.cshtml*

```cshtml
<cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="priority"></a>priority

| Tipo de atributo    | Valores de exemplo                |
|----------------   |----------------               |
| CacheItemPriority  | "High"                   |
|                    | "Low" |
|                    | "NeverRemove" |
|                    | "Normal" |

Fornece diretrizes de remoção do cache para o provedor de cache interno. O servidor Web removerá entradas de cache `Low` primeiro quando estiver sob demanda de memória.

Exemplo:

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

O atributo `priority` não assegura um nível específico de retenção de cache. `CacheItemPriority` é apenas uma sugestão. Configurar esse atributo como `NeverRemove` não assegura que o cache sempre será retido. Consulte [Recursos adicionais](#additional-resources) para obter mais informações.

O Auxiliar de Marca de Cache é dependente do [serviço de cache de memória](xref:performance/caching/memory). O Auxiliar de Marca de Cache adiciona o serviço se ele não tiver sido adicionado.

## <a name="additional-resources"></a>Recursos adicionais

* [Cache in-memory](xref:performance/caching/memory)
* [Introdução ao Identity](xref:security/authentication/identity)
