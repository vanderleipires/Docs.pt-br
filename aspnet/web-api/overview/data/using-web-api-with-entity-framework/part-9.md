---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-9
title: Adicionar um novo Item no banco de dados | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 0967c29e-e124-4db0-a788-c45d0ff5aff2
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: b1f7935c70efcc3ee486e76fc356ff43716632dd
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368871"
---
<a name="add-a-new-item-to-the-database"></a>Adicionar um novo Item no banco de dados
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Baixe o projeto concluído](https://github.com/MikeWasson/BookService)

Nesta seção, você adicionará a capacidade dos usuários criar um novo catálogo. No App. js, adicione o seguinte código para o modelo de exibição:

[!code-javascript[Main](part-9/samples/sample1.js)]

No index. cshtml, substitua a marcação a seguir:

[!code-html[Main](part-9/samples/sample2.html)]

Por:

[!code-html[Main](part-9/samples/sample3.html)]

Essa marcação cria um formulário para enviar um novo autor. Os valores para a lista suspensa de autor são associadas a dados para o `authors` observável no modelo de exibição. Para as outras entradas do formulário, os valores são associados a dados para o `newBook` propriedade do modelo de exibição.

O manipulador de envio do formulário está associado a `addBook` função:

[!code-html[Main](part-9/samples/sample4.html)]

O `addBook` função lê os valores atuais das entradas de formulário de associação de dados para criar um objeto JSON. Em seguida, ele envia o objeto JSON para `/api/books`.

> [!div class="step-by-step"]
> [Anterior](part-8.md)
> [Próximo](part-10.md)
