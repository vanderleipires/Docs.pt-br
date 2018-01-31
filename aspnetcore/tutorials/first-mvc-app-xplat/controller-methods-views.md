---
title: "Exibições e métodos do controlador"
author: rick-anderson
description: "Trabalhando com métodos do controlador, exibições e DataAnnotations"
manager: wpickett
ms.author: riande
ms.date: 04/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-xplat/controller-methods-views
ms.openlocfilehash: 34bd73af9bd0e4a7c1e59b491105f959bcbc06c6
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="controller-methods-and-views"></a><span data-ttu-id="d6aab-103">Exibições e métodos do controlador</span><span class="sxs-lookup"><span data-stu-id="d6aab-103">Controller methods and views</span></span>

<span data-ttu-id="d6aab-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d6aab-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d6aab-105">Temos um bom começo para o aplicativo de filme, mas a apresentação não é ideal.</span><span class="sxs-lookup"><span data-stu-id="d6aab-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="d6aab-106">Você não deseja ver a hora (12:00:00 AM na imagem abaixo) e **ReleaseDate** deve ser escrito em duas palavras.</span><span class="sxs-lookup"><span data-stu-id="d6aab-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![Exibição de índice: a Data de Lançamento é uma palavra (sem espaço) e cada data de lançamento do filme mostra o horário 12h](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

<span data-ttu-id="d6aab-108">Abra o arquivo *Models/Movie.cs* e adicione as linhas realçadas mostradas abaixo:</span><span class="sxs-lookup"><span data-stu-id="d6aab-108">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1&highlight=2,11-12)]

<span data-ttu-id="d6aab-109">Compile e execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d6aab-109">Build and run the app.</span></span>

<!-- include start
![MVC Movie application open browser showing movie data](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

 -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
<span data-ttu-id="d6aab-110">[Anterior – Trabalhando com o SQLite](working-with-sql.md)
[Próximo – Adicionar uma pesquisa](search.md)</span><span class="sxs-lookup"><span data-stu-id="d6aab-110">[Previous - Working with SQLite](working-with-sql.md)
[Next - Add search](search.md)</span></span>  
