---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: "Criar modo de exibição (UI) | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: 8c5cc662e2e3c9cb07ca9e30ff57eb084d58e1bb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="create-the-view-ui"></a>Criar modo de exibição (IU)
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Baixe o projeto concluído](https://github.com/MikeWasson/BookService)

Nesta seção, você começará a definir o HTML para o aplicativo e adicionar a associação de dados entre o HTML e o modelo de exibição.

Abra o arquivo Views/Home/Index.cshtml. Substitua todo o conteúdo do arquivo com o seguinte.

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

A maioria do `div` elementos existem para [Bootstrap](http://getbootstrap.com/) estilo. Os elementos importantes são aqueles com `data-bind` atributos. Este atributo vincula o HTML para o modelo de exibição.

Por exemplo:

[!code-html[Main](part-7/samples/sample2.html)]

Neste exemplo, o &quot; `text` &quot; associação faz com que o `<p>` elemento para mostrar o valor da `error` propriedade do modelo de exibição. Lembre-se de que `error` foi declarado como um `ko.observable`:

[!code-javascript[Main](part-7/samples/sample3.js)]

Sempre que um novo valor é atribuído a `error`, Knockout atualiza o texto de `<p>` elemento.

O `foreach` associação informa Knockout para percorrer o conteúdo do `books` matriz. Para cada item na matriz, Knockout cria um novo &lt;li&gt; elemento. Associações de dentro do contexto da `foreach` consulte Propriedades do item de matriz. Por exemplo:

[!code-html[Main](part-7/samples/sample4.html)]

Aqui o `text` associação lê a propriedade de autor de cada livro.

Se você executar o aplicativo agora, ele deve ser assim:

![](part-7/_static/image1.png)

A lista de livros carrega assincronamente, depois que a página for carregada. Agora, o &quot;detalhes&quot; links não são funcionais. Vamos adicionar essa funcionalidade na próxima seção.

>[!div class="step-by-step"]
[Anterior](part-6.md)
[Próximo](part-8.md)
