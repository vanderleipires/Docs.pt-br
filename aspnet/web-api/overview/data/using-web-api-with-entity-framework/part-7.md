---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: Criar a exibição de (UI) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: eeaa36ede45b82afd112a270acf68105d1ae6dbc
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820980"
---
<a name="create-the-view-ui"></a><span data-ttu-id="c208a-102">Criar modo de exibição (interface do usuário)</span><span class="sxs-lookup"><span data-stu-id="c208a-102">Create the View (UI)</span></span>
====================
<span data-ttu-id="c208a-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c208a-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="c208a-104">Baixe o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="c208a-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="c208a-105">Nesta seção, você começará a definir o HTML para o aplicativo e adicionar a associação de dados entre o HTML e o modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="c208a-105">In this section, you will start to define the HTML for the app, and add data binding between the HTML and the view model.</span></span>

<span data-ttu-id="c208a-106">Abra o arquivo Views/Home/Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="c208a-106">Open the file Views/Home/Index.cshtml.</span></span> <span data-ttu-id="c208a-107">Substitua todo o conteúdo desse arquivo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="c208a-107">Replace the entire contents of that file with the following.</span></span>

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

<span data-ttu-id="c208a-108">A maioria dos `div` elementos existem para o [Bootstrap](http://getbootstrap.com/) definindo o estilo.</span><span class="sxs-lookup"><span data-stu-id="c208a-108">Most of the `div` elements are there for [Bootstrap](http://getbootstrap.com/) styling.</span></span> <span data-ttu-id="c208a-109">Os elementos importantes são aqueles com `data-bind` atributos.</span><span class="sxs-lookup"><span data-stu-id="c208a-109">The important elements are the ones with `data-bind` attributes.</span></span> <span data-ttu-id="c208a-110">Esse atributo vincula o HTML para o modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="c208a-110">This attribute links the HTML to the view model.</span></span>

<span data-ttu-id="c208a-111">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="c208a-111">For example:</span></span>

[!code-html[Main](part-7/samples/sample2.html)]

<span data-ttu-id="c208a-112">Neste exemplo, o &quot; `text` &quot; associação faz com que o `<p>` elemento para mostrar o valor da `error` propriedade do modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="c208a-112">In this example, the &quot;`text`&quot; binding causes the `<p>` element to show the value of the `error` property from the view model.</span></span> <span data-ttu-id="c208a-113">Lembre-se de que `error` foi declarado como um `ko.observable`:</span><span class="sxs-lookup"><span data-stu-id="c208a-113">Recall that `error` was declared as a `ko.observable`:</span></span>

[!code-javascript[Main](part-7/samples/sample3.js)]

<span data-ttu-id="c208a-114">Sempre que um novo valor é atribuído a `error`, o Knockout atualiza o texto no `<p>` elemento.</span><span class="sxs-lookup"><span data-stu-id="c208a-114">Whenever a new value is assigned to `error`, Knockout updates the text in the `<p>` element.</span></span>

<span data-ttu-id="c208a-115">O `foreach` associação informa ao Knockout para executar um loop pelo conteúdo do `books` matriz.</span><span class="sxs-lookup"><span data-stu-id="c208a-115">The `foreach` binding tells Knockout to loop through the contents of the `books` array.</span></span> <span data-ttu-id="c208a-116">Para cada item na matriz, o Knockout cria um novo &lt;li&gt; elemento.</span><span class="sxs-lookup"><span data-stu-id="c208a-116">For each item in the array, Knockout creates a new &lt;li&gt; element.</span></span> <span data-ttu-id="c208a-117">Associações de dentro do contexto da `foreach` fazer referência às propriedades do item da matriz.</span><span class="sxs-lookup"><span data-stu-id="c208a-117">Bindings inside the context of the `foreach` refer to properties on the array item.</span></span> <span data-ttu-id="c208a-118">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="c208a-118">For example:</span></span>

[!code-html[Main](part-7/samples/sample4.html)]

<span data-ttu-id="c208a-119">Aqui o `text` associação lê a propriedade de autor de cada livro.</span><span class="sxs-lookup"><span data-stu-id="c208a-119">Here the `text` binding reads the Author property of each book.</span></span>

<span data-ttu-id="c208a-120">Se você executar o aplicativo agora, ele deve ser assim:</span><span class="sxs-lookup"><span data-stu-id="c208a-120">If you run the application now, it should look like this:</span></span>

![](part-7/_static/image1.png)

<span data-ttu-id="c208a-121">A lista de livros carrega de forma assíncrona, depois que a página for carregada.</span><span class="sxs-lookup"><span data-stu-id="c208a-121">The list of books loads asynchronously, after the page loads.</span></span> <span data-ttu-id="c208a-122">Agora, o &quot;detalhes&quot; links não são funcionais.</span><span class="sxs-lookup"><span data-stu-id="c208a-122">Right now, the &quot;Details&quot; links are not functional.</span></span> <span data-ttu-id="c208a-123">Vamos adicionar essa funcionalidade na próxima seção.</span><span class="sxs-lookup"><span data-stu-id="c208a-123">We'll add this functionality in the next section.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c208a-124">[Anterior](part-6.md)
> [Próximo](part-8.md)</span><span class="sxs-lookup"><span data-stu-id="c208a-124">[Previous](part-6.md)
[Next](part-8.md)</span></span>
