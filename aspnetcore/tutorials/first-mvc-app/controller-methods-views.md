---
title: "Exibições e métodos do controlador"
author: rick-anderson
description: "Trabalhando com métodos do controlador, exibições e DataAnnotations"
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: cfe1838371226334d368dca13bba37c5b1f6fc39
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="controller-methods-and-views"></a><span data-ttu-id="b7fc4-103">Exibições e métodos do controlador</span><span class="sxs-lookup"><span data-stu-id="b7fc4-103">Controller methods and views</span></span>

<span data-ttu-id="b7fc4-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b7fc4-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b7fc4-105">Temos um bom começo para o aplicativo de filme, mas a apresentação não é ideal.</span><span class="sxs-lookup"><span data-stu-id="b7fc4-105">We have a good start to the movie app, but the presentation is not ideal.</span></span> <span data-ttu-id="b7fc4-106">Você não deseja ver a hora (12:00:00 AM na imagem abaixo) e **ReleaseDate** deve ser escrito em duas palavras.</span><span class="sxs-lookup"><span data-stu-id="b7fc4-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![Exibição de índice: a Data de Lançamento é uma palavra (sem espaço) e cada data de lançamento do filme mostra o horário 12h](working-with-sql/_static/m55.png)

<span data-ttu-id="b7fc4-108">Abra o arquivo *Models/Movie.cs* e adicione as linhas realçadas mostradas abaixo:</span><span class="sxs-lookup"><span data-stu-id="b7fc4-108">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]

<span data-ttu-id="b7fc4-109">Clique com o botão direito do mouse em uma linha curvada vermelha **> Ações Rápidas e Refatorações**.</span><span class="sxs-lookup"><span data-stu-id="b7fc4-109">Right click on a red squiggly line **> Quick Actions and Refactorings**.</span></span>

  ![Menu contextual mostra **> Ações Rápidas e Refatorações**.](controller-methods-views/_static/qa.png)


<span data-ttu-id="b7fc4-111">Toque em `using System.ComponentModel.DataAnnotations;`</span><span class="sxs-lookup"><span data-stu-id="b7fc4-111">Tap `using System.ComponentModel.DataAnnotations;`</span></span>

  ![usando System.ComponentModel.DataAnnotations na parte superior da lista](controller-methods-views/_static/da.png)

  <span data-ttu-id="b7fc4-113">O Visual Studio adiciona `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="b7fc4-113">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

<span data-ttu-id="b7fc4-114">Vamos remover as instruções `using` que não são necessárias.</span><span class="sxs-lookup"><span data-stu-id="b7fc4-114">Let's remove the `using` statements that are not needed.</span></span> <span data-ttu-id="b7fc4-115">Elas são exibidas por padrão em uma fonte cinza-claro.</span><span class="sxs-lookup"><span data-stu-id="b7fc4-115">They show up by default in a light grey font.</span></span> <span data-ttu-id="b7fc4-116">Clique com o botão direito do mouse em qualquer lugar do arquivo *Movie.cs* **> Remover e Classificar Usos**.</span><span class="sxs-lookup"><span data-stu-id="b7fc4-116">Right click anywhere in the *Movie.cs* file **> Remove and Sort Usings**.</span></span>

![Remover e classificar usos](controller-methods-views/_static/rm.png)

<span data-ttu-id="b7fc4-118">O código atualizado:</span><span class="sxs-lookup"><span data-stu-id="b7fc4-118">The updated code:</span></span>

[!code-csharp[Main](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]

<!-- include start -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
<span data-ttu-id="b7fc4-119">[Anterior](working-with-sql.md)
[Próximo](search.md)</span><span class="sxs-lookup"><span data-stu-id="b7fc4-119">[Previous](working-with-sql.md)
[Next](search.md)</span></span>  
