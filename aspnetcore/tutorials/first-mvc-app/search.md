---
title: Adicionar a pesquisa a um aplicativo ASP.NET Core MVC
author: rick-anderson
description: Mostra como adicionar uma pesquisa a um aplicativo ASP.NET Core MVC simples
ms.author: riande
ms.date: 03/07/2017
uid: tutorials/first-mvc-app/search
ms.openlocfilehash: d0581142b505323385feba441b707d29b3f6db5d
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/26/2018
ms.locfileid: "36960834"
---
[!INCLUDE [adding-model](~/includes/mvc-intro/search1.md)]

<span data-ttu-id="c0364-103">Renomeie rapidamente o parâmetro `searchString` para `id` com o comando **rename**.</span><span class="sxs-lookup"><span data-stu-id="c0364-103">You can quickly rename the `searchString` parameter to `id` with the **rename** command.</span></span> <span data-ttu-id="c0364-104">Clique com o botão direito do mouse em `searchString` **> Renomear**.</span><span class="sxs-lookup"><span data-stu-id="c0364-104">Right click on `searchString` **> Rename**.</span></span>

![Menu contextual](search/_static/rename.png)

<span data-ttu-id="c0364-106">Os destinos de renomeação são realçados.</span><span class="sxs-lookup"><span data-stu-id="c0364-106">The rename targets are highlighted.</span></span>

![Editor de código mostrando a variável realçada em todo o método ActionResult do Índice](search/_static/rename2.png)

<span data-ttu-id="c0364-108">Altere o parâmetro para `id` e todas as ocorrências de `searchString` altere para `id`.</span><span class="sxs-lookup"><span data-stu-id="c0364-108">Change the parameter to `id` and all occurrences of `searchString` change to `id`.</span></span>

![Editor de código mostrando que a variável foi alterada para ID](search/_static/rename3.png)

[!INCLUDE [adding-model](~/includes/mvc-intro/search2.md)]

<span data-ttu-id="c0364-110">Observe como o IntelliSense nos ajuda a atualizar a marcação.</span><span class="sxs-lookup"><span data-stu-id="c0364-110">Notice how intelliSense helps us update the markup.</span></span>

![Menu contextual do IntelliSense com o método selecionado na lista de atributos do elemento de formulário](search/_static/int_m.png)

![Menu contextual do IntelliSense com get selecionado na lista de valores de atributo de método](search/_static/int_get.png)

<span data-ttu-id="c0364-113">Observe a fonte diferenciada na marcação `<form>`.</span><span class="sxs-lookup"><span data-stu-id="c0364-113">Notice the distinctive font in the `<form>` tag.</span></span> <span data-ttu-id="c0364-114">Essa fonte diferenciada indica que a marcação tem o suporte de [Auxiliares de Marcação](~/mvc/views/tag-helpers/intro.md).</span><span class="sxs-lookup"><span data-stu-id="c0364-114">That distinctive font indicates the tag is supported by [Tag Helpers](~/mvc/views/tag-helpers/intro.md).</span></span>

![marcação de formulário com texto roxo](search/_static/th_font.png)

[!INCLUDE [adding-model](~/includes/mvc-intro/search3.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="c0364-116">[Anterior](controller-methods-views.md)
> [Próximo](new-field.md)</span><span class="sxs-lookup"><span data-stu-id="c0364-116">[Previous](controller-methods-views.md)
[Next](new-field.md)</span></span>  
