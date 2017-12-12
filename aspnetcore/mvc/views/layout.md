---
title: Layout
author: ardalis
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 29f12d1f-9734-48bd-bf1a-cee53a8ab700
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/layout
ms.openlocfilehash: 064621d8756b007c5b8859111bf3a03a0d7dda81
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="layout"></a>Layout

Por [Steve Smith](https://ardalis.com/)

Modos de exibição frequentemente compartilham elementos visuais e através de programação. Neste artigo, você aprenderá a usar layouts comuns, compartilhar diretivas e executar o código comum antes de modos de exibição de renderização em seu aplicativo ASP.NET.

## <a name="what-is-a-layout"></a>O que é um Layout

A maioria dos aplicativos web têm um layout comum que fornece aos usuários uma experiência consistente conforme eles navegam de uma página. O layout normalmente inclui elementos de interface do usuário comum, como o cabeçalho de aplicativo, navegação ou elementos de menu e rodapé.

![Exemplo de Layout de página](layout/_static/page-layout.png)

Estruturas HTML comuns, como scripts e folhas de estilo são também usadas pelo número de páginas dentro de um aplicativo. Todos esses elementos compartilhados podem ser definidos em uma *layout* arquivo, que pode ser referenciado por qualquer exibição usada dentro do aplicativo. Layouts de reduzem código duplicado em exibições, ajudando a seguir o [princípio não repita por conta própria (teste)](http://deviq.com/don-t-repeat-yourself/).

Por convenção, o layout padrão para um aplicativo ASP.NET é denominado `_Layout.cshtml`. O modelo de projeto do Visual Studio ASP.NET Core MVC inclui o arquivo de layout no `Views/Shared` pasta:

![pasta de modos de exibição no Gerenciador de soluções](layout/_static/web-project-views.png)

Este layout define um modelo de nível superior para modos de exibição no aplicativo. Aplicativos não requerem um layout e aplicativos podem definir mais de um layout, com diferentes modos de exibição especificando layouts diferentes.

Um exemplo `_Layout.cshtml`:

[!code-html[Main](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]

## <a name="specifying-a-layout"></a>Especificando um Layout

Modos de exibição Razor têm um `Layout` propriedade. Modos de exibição individuais Especifica um layout definindo essa propriedade:

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

O layout especificado pode usar um caminho completo (exemplo: `/Views/Shared/_Layout.cshtml`) ou um nome parcial (exemplo: `_Layout`). Quando é fornecido um nome parcial, o mecanismo de exibição Razor procurará o arquivo de layout usando seu processo de descoberta padrão. A pasta associada de controlador é pesquisada primeiro, seguido de `Shared` pasta. Esse processo de descoberta é idêntico àquele usado para descobrir [exibições parciais](partial.md).

Por padrão, todo layout deve chamar `RenderBody`. Sempre que a chamada para `RenderBody` é colocada, o conteúdo do modo de exibição será renderizado.

<a name="layout-sections-label"></a>

### <a name="sections"></a>Seções

Um layout, opcionalmente, pode fazer referência a um ou mais *seções*, chamando `RenderSection`. Seções fornecem uma maneira de organizar onde determinados elementos da página devem ser colocados. Cada chamada para `RenderSection` pode especificar se essa seção é obrigatória ou opcional. Se uma seção obrigatória não for encontrada, uma exceção será lançada. Modos de exibição individuais especificam o conteúdo a ser renderizados dentro de uma seção usando o `@section` a sintaxe do Razor. Se uma exibição define uma seção, deve ser processado ou ocorrerá um erro.

Um exemplo `@section` definição em um modo de exibição:

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

No código acima, os scripts de validação são adicionados para o `scripts` seção em uma exibição que inclui um formulário. Outros modos de exibição no mesmo aplicativo podem não exigir qualquer script adicional e portanto não precisam definir uma seção de scripts.

Seções definidas em um modo de exibição estão disponíveis apenas em sua página de layout imediata. Eles não podem ser referenciados de existe meio-termo, componentes do modo de exibição ou outras partes do sistema de exibição.

### <a name="ignoring-sections"></a>Ignorando seções

Por padrão, o corpo e todas as seções de uma página de conteúdo devem todas ser renderizadas por página de layout. O mecanismo de exibição Razor impõe esse controle se o corpo e cada seção foi renderizados.

Para instruir o mecanismo de exibição para ignorar o corpo ou seções, chame o `IgnoreBody` e `IgnoreSection` métodos.

O corpo e cada seção em uma página de Razor devem ser processados ou ignorados.

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a>Importar diretivas compartilhadas

Modos de exibição podem usar diretivas do Razor para muitas coisas, como importar namespaces ou executar [injeção de dependência](dependency-injection.md). Diretivas compartilhadas por vários modos de exibição podem ser especificadas em um comum `_ViewImports.cshtml` arquivo. O `_ViewImports` arquivo suporta as seguintes diretivas:

* `@addTagHelper`

* `@removeTagHelper`

* `@tagHelperPrefix`

* `@using`

* `@model`

* `@inherits`

* `@inject`

O arquivo não oferece suporte a outros recursos do Razor, como funções e definições de seção.

Um exemplo de `_ViewImports.cshtml` arquivo:

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

O `_ViewImports.cshtml` de arquivos para um aplicativo MVC do ASP.NET Core normalmente é colocado no `Views` pasta. Um `_ViewImports.cshtml` arquivo pode ser colocado em qualquer pasta na qual o caso só serão aplicada a exibições dentro dessa pasta e suas subpastas. `_ViewImports`os arquivos são processados, iniciando no nível raiz, e, em seguida, para cada pasta à esquerda até o local da exibição em si, para que as configurações especificadas no nível raiz pode ser substituído no nível da pasta.

Por exemplo, se um nível de raiz `_ViewImports.cshtml` arquivo especifica `@model` e `@addTagHelper`e outro `_ViewImports.cshtml` arquivo na pasta do modo de exibição associado a controlador Especifica outro `@model` e adiciona outro `@addTagHelper`, o modo de exibição terão acesso a ambos os auxiliares de marcação e usará o último `@model`.

Se vários `_ViewImports.cshtml` arquivos sejam executados por um modo de exibição, combinados comportamento das diretivas incluídas no `ViewImports.cshtml` arquivos será da seguinte maneira:

* `@addTagHelper`, `@removeTagHelper`: execução, em ordem

* `@tagHelperPrefix`: o mais próximo um para o modo de exibição substitui quaisquer outras

* `@model`: o mais próximo um para o modo de exibição substitui quaisquer outras

* `@inherits`: o mais próximo um para o modo de exibição substitui quaisquer outras

* `@using`: todos são incluídos. duplicatas serão ignoradas

* `@inject`: para cada propriedade, aquele mais próximo à exibição substitui quaisquer outros usuários com o mesmo nome de propriedade

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a>Executar código antes de cada modo de exibição

Se você tiver o código necessário executar antes de cada exibição, isso deve ser colocado no `_ViewStart.cshtml` arquivo. Por convenção, o `_ViewStart.cshtml` arquivo está localizado no `Views` pasta. As instruções listadas no `_ViewStart.cshtml` são executados antes de cada exibição completa (não layouts e exibições parciais não). Como [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` são hierárquicos. Se um `_ViewStart.cshtml` arquivo é definido na pasta de exibição associada por controlador, ele será executado depois de definido na raiz do `Views` pasta (se houver).

Um exemplo de `_ViewStart.cshtml` arquivo:

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

O arquivo acima Especifica que todas as exibições usarão o `_Layout.cshtml` layout.

> [!NOTE]
> Nem `_ViewStart.cshtml` nem `_ViewImports.cshtml` geralmente são colocadas as `/Views/Shared` pasta. As versões de nível de aplicativo desses arquivos devem ser colocadas diretamente no `/Views` pasta.
