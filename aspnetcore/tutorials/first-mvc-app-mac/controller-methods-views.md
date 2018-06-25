---
title: Exibições e métodos do controlador em um aplicativo ASP.NET Core MVC
author: rick-anderson
description: Trabalhando com métodos do controlador, exibições e DataAnnotations
ms.author: riande
ms.date: 04/07/2017
uid: tutorials/first-mvc-app-mac/controller-methods-views
ms.openlocfilehash: 0bd57597c74918829de38764326e35df08ab1e05
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279071"
---
# <a name="controller-methods-and-views-in-an-aspnet-core-mvc-app"></a><span data-ttu-id="d19eb-103">Exibições e métodos do controlador em um aplicativo ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="d19eb-103">Controller methods and views in an ASP.NET Core MVC app</span></span>

<span data-ttu-id="d19eb-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d19eb-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d19eb-105">Temos um bom começo para o aplicativo de filme, mas a apresentação não é ideal.</span><span class="sxs-lookup"><span data-stu-id="d19eb-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="d19eb-106">Você não deseja ver a hora (12:00:00 AM na imagem a seguir) e **ReleaseDate** deve ser escrito em duas palavras.</span><span class="sxs-lookup"><span data-stu-id="d19eb-106">We don't want to see the time (12:00:00 AM in the following image) and **ReleaseDate** should be two words.</span></span>

![Exibição de índice: a Data de Lançamento é uma palavra (sem espaço) e cada data de lançamento do filme mostra o horário 12h](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

<span data-ttu-id="d19eb-108">Abra o arquivo *Models/Movie.cs* e adicione as linhas realçadas mostradas abaixo:</span><span class="sxs-lookup"><span data-stu-id="d19eb-108">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1&highlight=2,11-12)]

<span data-ttu-id="d19eb-109">Compile e execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d19eb-109">Build and run the app.</span></span>

<!-- include start
![MVC Movie application open browser showing movie data](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

 -->

[!INCLUDE [adding-model](../../includes/mvc-intro/controller-methods-views.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="d19eb-110">[Anterior – Trabalhando com o SQLite](working-with-sql.md)
> [Próximo – Adicionar uma pesquisa](search.md)</span><span class="sxs-lookup"><span data-stu-id="d19eb-110">[Previous - Working with SQLite](working-with-sql.md)
[Next - Add search](search.md)</span></span>
