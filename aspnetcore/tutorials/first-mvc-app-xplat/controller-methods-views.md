---
title: "Exibições e métodos do controlador"
author: rick-anderson
description: "Trabalhando com métodos do controlador, exibições e DataAnnotations"
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 04/07/2017
ms.topic: get-started-article
ms.assetid: c7313211-bb71-4adf-babe-8e72603cc0ce
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-xplat/controller-methods-views
ms.openlocfilehash: b72fb5077d3bd577cd6282e7a6231a0806ee1fd2
ms.sourcegitcommit: 74a8ad9c1ba5c155d7c4303e67632a0922c38e86
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/20/2017
---
# <a name="controller-methods-and-views"></a><span data-ttu-id="1d391-104">Exibições e métodos do controlador</span><span class="sxs-lookup"><span data-stu-id="1d391-104">Controller methods and views</span></span>

<span data-ttu-id="1d391-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1d391-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1d391-106">Temos um bom começo para o aplicativo de filme, mas a apresentação não é ideal.</span><span class="sxs-lookup"><span data-stu-id="1d391-106">We have a good start to the movie app, but the presentation is not ideal.</span></span> <span data-ttu-id="1d391-107">Você não deseja ver a hora (12:00:00 AM na imagem abaixo) e **ReleaseDate** deve ser escrito em duas palavras.</span><span class="sxs-lookup"><span data-stu-id="1d391-107">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![Exibição de índice: a Data de Lançamento é uma palavra (sem espaço) e cada data de lançamento do filme mostra o horário 12h](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

<span data-ttu-id="1d391-109">Abra o arquivo *Models/Movie.cs* e adicione as linhas realçadas mostradas abaixo:</span><span class="sxs-lookup"><span data-stu-id="1d391-109">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1&highlight=2,11-12)]

<span data-ttu-id="1d391-110">Compile e execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1d391-110">Build and run the app.</span></span>

<!-- include start
![MVC Movie application open browser showing movie data](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

 -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
<span data-ttu-id="1d391-111">[Anterior – Trabalhando com o SQLite](working-with-sql.md)
[Próximo – Adicionar uma pesquisa](search.md)</span><span class="sxs-lookup"><span data-stu-id="1d391-111">[Previous - Working with SQLite](working-with-sql.md)
[Next - Add search](search.md)</span></span>  
