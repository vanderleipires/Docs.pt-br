---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: Criar a exibição de (UI) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: aa0ea68dd83a294e6d6c343887738c60eef595bf
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834992"
---
<a name="create-the-view-ui"></a>Criar modo de exibição (interface do usuário)
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Baixe o projeto concluído](https://github.com/MikeWasson/BookService)

Nesta seção, você começará a definir o HTML para o aplicativo e adicionar a associação de dados entre o HTML e o modelo de exibição.

Abra o arquivo Views/Home/Index.cshtml. Substitua todo o conteúdo desse arquivo pelo seguinte.

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

A maioria dos `div` elementos existem para o [Bootstrap](http://getbootstrap.com/) definindo o estilo. Os elementos importantes são aqueles com `data-bind` atributos. Esse atributo vincula o HTML para o modelo de exibição.

Por exemplo:

[!code-html[Main](part-7/samples/sample2.html)]

Neste exemplo, o &quot; `text` &quot; associação faz com que o `<p>` elemento para mostrar o valor da `error` propriedade do modelo de exibição. Lembre-se de que `error` foi declarado como um `ko.observable`:

[!code-javascript[Main](part-7/samples/sample3.js)]

Sempre que um novo valor é atribuído a `error`, o Knockout atualiza o texto no `<p>` elemento.

O `foreach` associação informa ao Knockout para executar um loop pelo conteúdo do `books` matriz. Para cada item na matriz, o Knockout cria um novo &lt;li&gt; elemento. Associações de dentro do contexto da `foreach` fazer referência às propriedades do item da matriz. Por exemplo:

[!code-html[Main](part-7/samples/sample4.html)]

Aqui o `text` associação lê a propriedade de autor de cada livro.

Se você executar o aplicativo agora, ele deve ser assim:

![](part-7/_static/image1.png)

A lista de livros carrega de forma assíncrona, depois que a página for carregada. Agora, o &quot;detalhes&quot; links não são funcionais. Vamos adicionar essa funcionalidade na próxima seção.

> [!div class="step-by-step"]
> [Anterior](part-6.md)
> [Próximo](part-8.md)
