---
title: Adicionar a pesquisa a um aplicativo ASP.NET Core MVC
author: rick-anderson
description: Mostra como adicionar uma pesquisa a um aplicativo ASP.NET Core MVC simples
ms.author: riande
ms.date: 04/07/2017
uid: tutorials/first-mvc-app-xplat/search
ms.openlocfilehash: cf4fe3806b45008f48bf5f0598057552bdcfae7c
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961432"
---
[!INCLUDE [adding-model](../../includes/mvc-intro/search1.md)]

Observação: o SQLlite diferencia maiúsculas de minúsculas e, portanto, você precisará pesquisar “Ghost” e não “ghost”.

[!INCLUDE [adding-model](../../includes/mvc-intro/search2.md)]

Altere a marcação `<form>` na exibição *Views\movie\Index.cshtml* do Razor para especificar `method="get"`:

```html
<form asp-controller="Movies" asp-action="Index" method="get">
```

[!INCLUDE [adding-model](../../includes/mvc-intro/search3.md)]

> [!div class="step-by-step"]
> [Anterior – Exibições e métodos do controlador](controller-methods-views.md)
> [Próximo – Adicionar um campo](new-field.md)  
