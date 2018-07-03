---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-8
title: Exibir detalhes de Item | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 75ef94b1-bbec-4681-9210-452dba816144
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-8
msc.type: authoredcontent
ms.openlocfilehash: 268c44f842cc2beb32a0a3e4c74b83b7ca9fd787
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375169"
---
<a name="display-item-details"></a>Exibir detalhes do Item
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Baixe o projeto concluído](https://github.com/MikeWasson/BookService)

Nesta seção, você adicionará a capacidade de exibir os detalhes de cada livro. No App. js, adicione o seguinte código para o modelo de exibição:

[!code-javascript[Main](part-8/samples/sample1.js)]

No Views/Home/Index.cshtml, adicione um elemento de ligação de dados para o link de detalhes:

[!code-html[Main](part-8/samples/sample2.html?highlight=5)]

Isso vincula o manipulador de cliques para o &lt;uma&gt; elemento para o `getBookDetail` função no modelo de exibição.

No mesmo arquivo, substitua a seguinte marcação:

[!code-html[Main](part-8/samples/sample3.html)]

com isso:

[!code-html[Main](part-8/samples/sample4.html)]

Essa marcação cria uma tabela que está associado a dados para as propriedades do `detail` observável no modelo de exibição.

O "&lt;! – ko –&gt; &quot; sintaxe permite que você inclua uma associação Knockout fora de um elemento DOM. Nesse caso, o `if` associação faz com que esta seção de marcação a ser exibido apenas quando `details` não for nulo.

[!code-html[Main](part-8/samples/sample5.html)]

Agora, se você executa o aplicativo e clique em um dos &quot;detalhe&quot; links, o aplicativo exibirá os detalhes do livro.

[![](part-8/_static/image2.png)](part-8/_static/image1.png)

> [!div class="step-by-step"]
> [Anterior](part-7.md)
> [Próximo](part-9.md)
