---
title: Os métodos e as exibições do controlador no ASP.NET Core
author: rick-anderson
description: Aprenda a trabalhar com os métodos, as exibições e as DataAnnotations do controlador no ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: 6fe0a0e71079bebcbd3a76abee0f2917f562e766
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
# <a name="controller-methods-and-views-in-aspnet-core"></a>Os métodos e as exibições do controlador no ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Temos um bom começo para o aplicativo de filme, mas a apresentação não é ideal. Você não deseja ver a hora (12:00:00 AM na imagem abaixo) e **ReleaseDate** deve ser escrito em duas palavras.

![Exibição de índice: a Data de Lançamento é uma palavra (sem espaço) e cada data de lançamento do filme mostra o horário 12h](working-with-sql/_static/m55.png)

Abra o arquivo *Models/Movie.cs* e adicione as linhas realçadas mostradas abaixo:

[!code-csharp[](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]

Clique com o botão direito do mouse em uma linha curvada vermelha **> Ações Rápidas e Refatorações**.

  ![Menu contextual mostra **> Ações Rápidas e Refatorações**.](controller-methods-views/_static/qa.png)


Toque em `using System.ComponentModel.DataAnnotations;`

  ![usando System.ComponentModel.DataAnnotations na parte superior da lista](controller-methods-views/_static/da.png)

  O Visual Studio adiciona `using System.ComponentModel.DataAnnotations;`.

Vamos remover as instruções `using` que não são necessárias. Elas são exibidas por padrão em uma fonte cinza-claro. Clique com o botão direito do mouse em qualquer lugar do arquivo *Movie.cs* **> Remover e Classificar Usos**.

![Remover e classificar usos](controller-methods-views/_static/rm.png)

O código atualizado:

[!code-csharp[](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]

<!-- include start -->

[!INCLUDE [adding-model](../../includes/mvc-intro/controller-methods-views.md)]

> [!div class="step-by-step"]
> [Anterior](working-with-sql.md)
> [Próximo](search.md)  
