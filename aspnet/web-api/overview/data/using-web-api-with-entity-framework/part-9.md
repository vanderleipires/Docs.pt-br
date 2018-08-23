---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-9
title: Adicionar um novo Item no banco de dados | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 0967c29e-e124-4db0-a788-c45d0ff5aff2
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: 01586ef6eecdbe1acfadfcfcc0c9ca8b442de2d3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832885"
---
<a name="add-a-new-item-to-the-database"></a><span data-ttu-id="828ee-102">Adicionar um novo Item no banco de dados</span><span class="sxs-lookup"><span data-stu-id="828ee-102">Add a New Item to the Database</span></span>
====================
<span data-ttu-id="828ee-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="828ee-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="828ee-104">Baixe o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="828ee-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="828ee-105">Nesta seção, você adicionará a capacidade dos usuários criar um novo catálogo.</span><span class="sxs-lookup"><span data-stu-id="828ee-105">In this section, you will add the ability for users to create a new book.</span></span> <span data-ttu-id="828ee-106">No App. js, adicione o seguinte código para o modelo de exibição:</span><span class="sxs-lookup"><span data-stu-id="828ee-106">In app.js, add the following code to the view model:</span></span>

[!code-javascript[Main](part-9/samples/sample1.js)]

<span data-ttu-id="828ee-107">No index. cshtml, substitua a marcação a seguir:</span><span class="sxs-lookup"><span data-stu-id="828ee-107">In Index.cshtml, replace the following markup:</span></span>

[!code-html[Main](part-9/samples/sample2.html)]

<span data-ttu-id="828ee-108">Por:</span><span class="sxs-lookup"><span data-stu-id="828ee-108">With:</span></span>

[!code-html[Main](part-9/samples/sample3.html)]

<span data-ttu-id="828ee-109">Essa marcação cria um formulário para enviar um novo autor.</span><span class="sxs-lookup"><span data-stu-id="828ee-109">This markup creates a form for submitting a new author.</span></span> <span data-ttu-id="828ee-110">Os valores para a lista suspensa de autor são associadas a dados para o `authors` observável no modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="828ee-110">The values for the author drop-down list are data-bound to the `authors` observable in the view model.</span></span> <span data-ttu-id="828ee-111">Para as outras entradas do formulário, os valores são associados a dados para o `newBook` propriedade do modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="828ee-111">For the other form inputs, the values are data-bound to the `newBook` property of the view model.</span></span>

<span data-ttu-id="828ee-112">O manipulador de envio do formulário está associado a `addBook` função:</span><span class="sxs-lookup"><span data-stu-id="828ee-112">The submit handler on the form is bound to the `addBook` function:</span></span>

[!code-html[Main](part-9/samples/sample4.html)]

<span data-ttu-id="828ee-113">O `addBook` função lê os valores atuais das entradas de formulário de associação de dados para criar um objeto JSON.</span><span class="sxs-lookup"><span data-stu-id="828ee-113">The `addBook` function reads the current values of the data-bound form inputs to create a JSON object.</span></span> <span data-ttu-id="828ee-114">Em seguida, ele envia o objeto JSON para `/api/books`.</span><span class="sxs-lookup"><span data-stu-id="828ee-114">Then it POSTs the JSON object to `/api/books`.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="828ee-115">[Anterior](part-8.md)
> [Próximo](part-10.md)</span><span class="sxs-lookup"><span data-stu-id="828ee-115">[Previous](part-8.md)
[Next](part-10.md)</span></span>
