---
title: Adicionando uma pesquisa a um aplicativo ASP.NET Core MVC
author: rick-anderson
description: Mostra como adicionar uma pesquisa a um aplicativo ASP.NET Core MVC simples
manager: wpickett
ms.author: riande
ms.date: 04/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-mac/search
ms.openlocfilehash: 3f648bc6c6d095b9fe8b6ac65bf5f51741938ed1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30894782"
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
