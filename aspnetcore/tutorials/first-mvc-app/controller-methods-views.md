---
title: "Exibições e métodos do controlador"
author: rick-anderson
description: "Trabalhando com métodos do controlador, exibições e DataAnnotations"
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: get-started-article
ms.assetid: c7313211-b271-4adf-bab8-8e72603cc0ce
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: 8960853438d3227de3ef7c50936626149d8d5997
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
# <a name="controller-methods-and-views"></a>Exibições e métodos do controlador

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Temos um bom começo para o aplicativo de filme, mas a apresentação não é ideal. Você não deseja ver a hora (12:00:00 AM na imagem abaixo) e **ReleaseDate** deve ser escrito em duas palavras.

![Exibição de índice: a Data de Lançamento é uma palavra (sem espaço) e cada data de lançamento do filme mostra o horário 12h](working-with-sql/_static/m55.png)

Abra o arquivo *Models/Movie.cs* e adicione as linhas realçadas mostradas abaixo:

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]

Clique com o botão direito do mouse em uma linha curvada vermelha **> Ações Rápidas e Refatorações**.

  ![Menu contextual mostra **> Ações Rápidas e Refatorações**.](controller-methods-views/_static/qa.png)


Toque em `using System.ComponentModel.DataAnnotations;`

  ![usando System.ComponentModel.DataAnnotations na parte superior da lista](controller-methods-views/_static/da.png)

  O Visual Studio adiciona `using System.ComponentModel.DataAnnotations;`.

Vamos remover as instruções `using` que não são necessárias. Elas são exibidas por padrão em uma fonte cinza-claro. Clique com o botão direito do mouse em qualquer lugar do arquivo *Movie.cs* **> Remover e Classificar Usos**.

![Remover e classificar usos](controller-methods-views/_static/rm.png)

O código atualizado:

[!code-csharp[Main](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]

<!-- include start -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
[Anterior](working-with-sql.md)
[Próximo](search.md)  
