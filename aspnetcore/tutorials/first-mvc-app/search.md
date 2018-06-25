---
title: Adicionando uma pesquisa
author: rick-anderson
description: Mostra como adicionar uma pesquisa a um aplicativo ASP.NET Core MVC simples
ms.author: riande
ms.date: 03/07/2017
uid: tutorials/first-mvc-app/search
ms.openlocfilehash: fb93d9688c9abf76ad0057c646c4b7662d003108
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274834"
---
[!INCLUDE [adding-model](~/includes/mvc-intro/search1.md)]

Renomeie rapidamente o parâmetro `searchString` para `id` com o comando **rename**. Clique com o botão direito do mouse em `searchString` **> Renomear**.

![Menu contextual](search/_static/rename.png)

Os destinos de renomeação são realçados.

![Editor de código mostrando a variável realçada em todo o método ActionResult do Índice](search/_static/rename2.png)

Altere o parâmetro para `id` e todas as ocorrências de `searchString` altere para `id`.

![Editor de código mostrando que a variável foi alterada para ID](search/_static/rename3.png)

[!INCLUDE [adding-model](~/includes/mvc-intro/search2.md)]

Observe como o IntelliSense nos ajuda a atualizar a marcação.

![Menu contextual do IntelliSense com o método selecionado na lista de atributos do elemento de formulário](search/_static/int_m.png)

![Menu contextual do IntelliSense com get selecionado na lista de valores de atributo de método](search/_static/int_get.png)

Observe a fonte diferenciada na marcação `<form>`. Essa fonte diferenciada indica que a marcação tem o suporte de [Auxiliares de Marcação](~/mvc/views/tag-helpers/intro.md).

![marcação de formulário com texto roxo](search/_static/th_font.png)

[!INCLUDE [adding-model](~/includes/mvc-intro/search3.md)]

> [!div class="step-by-step"]
> [Anterior](controller-methods-views.md)
> [Próximo](new-field.md)  
