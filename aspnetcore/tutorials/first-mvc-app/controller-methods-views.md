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
# <a name="controller-methods-and-views-in-aspnet-core"></a><span data-ttu-id="29732-103">Os métodos e as exibições do controlador no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="29732-103">Controller methods and views in ASP.NET Core</span></span>

<span data-ttu-id="29732-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="29732-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="29732-105">Temos um bom começo para o aplicativo de filme, mas a apresentação não é ideal.</span><span class="sxs-lookup"><span data-stu-id="29732-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="29732-106">Você não deseja ver a hora (12:00:00 AM na imagem abaixo) e **ReleaseDate** deve ser escrito em duas palavras.</span><span class="sxs-lookup"><span data-stu-id="29732-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![Exibição de índice: a Data de Lançamento é uma palavra (sem espaço) e cada data de lançamento do filme mostra o horário 12h](working-with-sql/_static/m55.png)

<span data-ttu-id="29732-108">Abra o arquivo *Models/Movie.cs* e adicione as linhas realçadas mostradas abaixo:</span><span class="sxs-lookup"><span data-stu-id="29732-108">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]

<span data-ttu-id="29732-109">Clique com o botão direito do mouse em uma linha curvada vermelha **> Ações Rápidas e Refatorações**.</span><span class="sxs-lookup"><span data-stu-id="29732-109">Right click on a red squiggly line **> Quick Actions and Refactorings**.</span></span>

  ![Menu contextual mostra **> Ações Rápidas e Refatorações**.](controller-methods-views/_static/qa.png)


<span data-ttu-id="29732-111">Toque em `using System.ComponentModel.DataAnnotations;`</span><span class="sxs-lookup"><span data-stu-id="29732-111">Tap `using System.ComponentModel.DataAnnotations;`</span></span>

  ![usando System.ComponentModel.DataAnnotations na parte superior da lista](controller-methods-views/_static/da.png)

  <span data-ttu-id="29732-113">O Visual Studio adiciona `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="29732-113">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

<span data-ttu-id="29732-114">Vamos remover as instruções `using` que não são necessárias.</span><span class="sxs-lookup"><span data-stu-id="29732-114">Let's remove the `using` statements that are not needed.</span></span> <span data-ttu-id="29732-115">Elas são exibidas por padrão em uma fonte cinza-claro.</span><span class="sxs-lookup"><span data-stu-id="29732-115">They show up by default in a light grey font.</span></span> <span data-ttu-id="29732-116">Clique com o botão direito do mouse em qualquer lugar do arquivo *Movie.cs* **> Remover e Classificar Usos**.</span><span class="sxs-lookup"><span data-stu-id="29732-116">Right click anywhere in the *Movie.cs* file **> Remove and Sort Usings**.</span></span>

![Remover e classificar usos](controller-methods-views/_static/rm.png)

<span data-ttu-id="29732-118">O código atualizado:</span><span class="sxs-lookup"><span data-stu-id="29732-118">The updated code:</span></span>

[!code-csharp[](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]

<!-- include start -->

[!INCLUDE [adding-model](../../includes/mvc-intro/controller-methods-views.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="29732-119">[Anterior](working-with-sql.md)
> [Próximo](search.md)</span><span class="sxs-lookup"><span data-stu-id="29732-119">[Previous](working-with-sql.md)
[Next](search.md)</span></span>  
