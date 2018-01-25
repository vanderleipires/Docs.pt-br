---
title: "Cache auxiliar de marca no núcleo do ASP.NET MVC"
author: pkellner
description: Mostra como trabalhar com o auxiliar de marca de Cache
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: 10aa1b493dbd0672cac789f6e48ddf2f14ba35dc
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a>Cache auxiliar de marca no núcleo do ASP.NET MVC

Por [Peter Kellner](http://peterkellner.net) 

O auxiliar de marca de Cache fornece a capacidade de melhorar drasticamente o desempenho do seu aplicativo ASP.NET Core armazenando em cache seu conteúdo para o provedor de cache interno do ASP.NET Core.

O mecanismo de exibição Razor define o padrão `expires-after` vinte minutos.

A seguinte marcação Razor armazena em cache a data/hora:

```cshtml
<cache>@DateTime.Now</cache>
```

A primeira solicitação para a página que contém `CacheTagHelper` exibirá a data/hora atual. Solicitações adicionais mostrará o valor armazenado em cache até que o cache expira (padrão de 20 minutos) ou é removido por pressão de memória.

Você pode definir a duração do cache com os seguintes atributos:

## <a name="cache-tag-helper-attributes"></a>Atributos do auxiliar de marca de cache

- - -

### <a name="enabled"></a>habilitado    


| Tipo de atributo    | Valores válidos      |
|----------------   |----------------   |
| boolean           | "true" (padrão)  |
|                   | "false"   |


Determina se o conteúdo entre o auxiliar de marca de Cache é armazenado em cache. O padrão é `true`.  Se definido como `false` este auxiliar de marca de Cache não tem cache efeito na saída renderizada.

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


Define uma data de expiração absoluta. O exemplo a seguir armazenará em cache o conteúdo do auxiliar de marca de Cache até 17:02:00 em 29 de janeiro de 2025.

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


Define o período de tempo desde a primeira vez de solicitação para armazenar em cache o conteúdo. 

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

### <a name="vary-by-header"></a>variar por cabeçalho

| Tipo de atributo    | Valores de exemplo                |
|----------------   |----------------               |
| Cadeia de Caracteres            | "User-Agent"                  |
|                   | "User-Agent,content-encoding" |

Aceita um valor de cabeçalho único ou uma lista separada por vírgulas de valores de cabeçalho que disparam uma atualização do cache quando elas forem alteradas. O exemplo a seguir monitora o valor do cabeçalho `User-Agent`. O exemplo armazenará em cache o conteúdo para todos os diferentes `User-Agent` apresentado para o servidor web.

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
| Cadeia de Caracteres            | "Tornar"                |
|                   | Modelo de "criar" |

Aceita um valor de cabeçalho único ou uma lista separada por vírgulas de valores de cabeçalho que disparam uma atualização do cache quando o valor do cabeçalho é alterado. O exemplo a seguir examina os valores de `Make` e `Model`.

Exemplo:

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-route"></a>variar por rota

| Tipo de atributo    | Valores de exemplo                |
|----------------   |----------------               |
| Cadeia de Caracteres            | "Tornar"                |
|                   | Modelo de "criar" |

Aceita um valor de cabeçalho único ou uma lista separada por vírgulas de valores de cabeçalho que disparam uma atualização de cache quando a alteração de valores de parâmetro de dados de rota. Exemplo:

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

### <a name="vary-by-cookie"></a>variar por cookie

| Tipo de atributo    | Valores de exemplo                |
|----------------   |----------------               |
| Cadeia de Caracteres            | ".AspNetCore.Identity.Application"                |
|                   | ".AspNetCore.Identity.Application,HairColor" |

Aceita um valor de cabeçalho único ou uma lista separada por vírgulas de valores de cabeçalho que disparam uma atualização do cache quando os valores de alteração (s). O exemplo a seguir examina o cookie associado à identidade do ASP.NET. Quando um usuário é autenticado o cookie de solicitação a ser definido que dispara uma atualização do cache.

Exemplo:

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-user"></a>variar por usuário

| Tipo de atributo    | Valores de exemplo                |
|----------------   |----------------               |
| Boolean             | "true"                  |
|                     | "falso" (padrão) |

Especifica se ou não o cache deve ser redefinido quando o usuário conectado (ou entidade de contexto) é alterado. O usuário atual é também conhecido como a entidade de contexto de solicitação e podem ser exibido em um modo de exibição Razor referenciando `@User.Identity.Name`.

O exemplo a seguir examina o usuário conectado no momento.  

Exemplo:

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

Usar esse atributo mantém o conteúdo em cache por um ciclo de logon e logoff.  Ao usar `vary-by-user="true"`, uma ação de logon e logoff invalida o cache para o usuário autenticado.  O cache é invalidado porque um novo valor de cookie exclusivo é gerado no logon. Cache é mantido para o estado anônimo quando nenhum cookie está presente ou expirou. Isso significa que se nenhum usuário estiver conectado, o cache será mantido.

- - -

### <a name="vary-by"></a>vary-by

| Tipo de atributo    | Valores de exemplo                |
|----------------   |----------------               |
| Cadeia de Caracteres             | "@Model"                 |


Permite a personalização de dados que é armazenado em cache. Quando o objeto referenciado por alterações de valor de cadeia de caracteres do atributo, o conteúdo do auxiliar de marca de Cache é atualizado. Geralmente uma concatenação de cadeia de caracteres de valores de modelo são atribuídos a este atributo.  Na verdade, isso significa que uma atualização para qualquer um dos valores concatenados invalida o cache.

O exemplo a seguir supõe que o método do controlador renderização as somas de exibir o valor de inteiro de dois parâmetros de rota, `myParam1` e `myParam2`e retorna que como a propriedade de modelo único. Quando essa soma é alterado, o conteúdo do auxiliar de marca de Cache é renderizado e armazenado em cache novamente.  

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
| CacheItemPriority  | "Alta"                   |
|                    | "Baixo" |
|                    | "NeverRemove" |
|                    | "Normal" |

Fornece orientação de remoção de cache para o provedor de cache interno. O servidor web irá remover `Low` entradas de cache primeiro quando ele estiver sob pressão de memória.

Exemplo:

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

O `priority` atributo não garante um nível específico de retenção de cache. `CacheItemPriority`é apenas uma sugestão. A configuração desse atributo `NeverRemove` não garante que o cache sempre será retido. Consulte [recursos adicionais](#additional-resources) para obter mais informações.

O auxiliar de marca de Cache é dependente de [serviço de cache de memória](xref:performance/caching/memory). O auxiliar de marca de Cache adiciona o serviço se não tiver sido adicionado.

## <a name="additional-resources"></a>Recursos adicionais

* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
