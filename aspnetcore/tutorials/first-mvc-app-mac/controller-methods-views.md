---
title: Exibições e métodos do controlador em um aplicativo ASP.NET Core MVC
author: rick-anderson
description: Trabalhando com métodos do controlador, exibições e DataAnnotations
manager: wpickett
ms.author: riande
ms.date: 04/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-mac/controller-methods-views
ms.openlocfilehash: 7a6a965d99742e7e06e6da82999dc60264cac6c8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30897872"
---
# <a name="controller-methods-and-views-in-an-aspnet-core-mvc-app"></a>Exibições e métodos do controlador em um aplicativo ASP.NET Core MVC

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Temos um bom começo para o aplicativo de filme, mas a apresentação não é ideal. Você não deseja ver a hora (12:00:00 AM na imagem a seguir) e **ReleaseDate** deve ser escrito em duas palavras.

![Exibição de índice: a Data de Lançamento é uma palavra (sem espaço) e cada data de lançamento do filme mostra o horário 12h](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

Abra o arquivo *Models/Movie.cs* e adicione as linhas realçadas mostradas abaixo:

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1&highlight=2,11-12)]

Compile e execute o aplicativo.

<!-- include start
![MVC Movie application open browser showing movie data](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

 -->

[!INCLUDE [adding-model](../../includes/mvc-intro/controller-methods-views.md)]

> [!div class="step-by-step"]
> [Anterior – Trabalhando com o SQLite](working-with-sql.md)
> [Próximo – Adicionar uma pesquisa](search.md)
