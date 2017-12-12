---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-8
title: Exibir detalhes do Item | Microsoft Docs
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 75ef94b1-bbec-4681-9210-452dba816144
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-8
msc.type: authoredcontent
ms.openlocfilehash: 0b6ae9384843712cae824ea662b984a40f021e57
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="display-item-details"></a>Detalhes do Item de exibição
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Baixe o projeto concluído](https://github.com/MikeWasson/BookService)

Nesta seção, você adicionará a capacidade de exibir os detalhes de cada livro. Em app.js, adicione o seguinte código para o modelo de exibição:

[!code-javascript[Main](part-8/samples/sample1.js)]

No Views/Home/Index.cshtml, adicione um elemento de associação de dados para o link de detalhes:

[!code-html[Main](part-8/samples/sample2.html?highlight=5)]

Isso vincula o manipulador de cliques para o &lt;um&gt; elemento para o `getBookDetail` função no modelo de exibição.

No mesmo arquivo, substitua a seguinte marcação:

[!code-html[Main](part-8/samples/sample3.html)]

com isso:

[!code-html[Main](part-8/samples/sample4.html)]

Essa marcação cria uma tabela que é associado a dados para as propriedades do `detail` observável no modelo de exibição.

O "&lt;! – ko -&gt; &quot; sintaxe permite que você incluir uma associação Knockout fora de um elemento DOM. Nesse caso, o `if` associação faz com que esta seção de marcação a ser exibida somente quando `details` não for nulo.

[!code-html[Main](part-8/samples/sample5.html)]

Agora, se você executa o aplicativo e clique em um do &quot;detalhes&quot; links, o aplicativo exibirá os detalhes de catálogo.

[![](part-8/_static/image2.png)](part-8/_static/image1.png)

>[!div class="step-by-step"]
[Anterior](part-7.md)
[Próximo](part-9.md)
