---
title: Atualizar as páginas geradas em um aplicativo ASP.NET Core
author: rick-anderson
description: Saiba como atualizar as páginas geradas em um aplicativo ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/da1
ms.openlocfilehash: 5c188799b7a42bcd5e9d5eab8dfe8cdad8002fe5
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/18/2018
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a><span data-ttu-id="6d150-103">Atualizar as páginas geradas em um aplicativo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6d150-103">Update the generated pages in an ASP.NET Core app</span></span>

<span data-ttu-id="6d150-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="6d150-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="6d150-105">Temos um bom começo para o aplicativo de filme, mas a apresentação não é ideal.</span><span class="sxs-lookup"><span data-stu-id="6d150-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="6d150-106">Você não deseja ver a hora (12:00:00 AM na imagem abaixo) e **ReleaseDate** deve ser **Release Date** (duas palavras).</span><span class="sxs-lookup"><span data-stu-id="6d150-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![Aplicativo de filme aberto no Chrome mostrando os dados do filme](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="6d150-108">Atualize o código gerado</span><span class="sxs-lookup"><span data-stu-id="6d150-108">Update the generated code</span></span>

<span data-ttu-id="6d150-109">Abra o arquivo *Models/Movie.cs* e adicione as linhas realçadas mostradas no seguinte código:</span><span class="sxs-lookup"><span data-stu-id="6d150-109">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

<span data-ttu-id="6d150-110">Clique com o botão direito do mouse em uma linha curvada vermelha > ** Ações Rápidas e Refatorações**.</span><span class="sxs-lookup"><span data-stu-id="6d150-110">Right click on a red squiggly line > ** Quick Actions and Refactorings**.</span></span>

  ![Menu contextual mostra **> Ações Rápidas e Refatorações**.](da1/qa.png)

<span data-ttu-id="6d150-112">Selecione `using System.ComponentModel.DataAnnotations;`</span><span class="sxs-lookup"><span data-stu-id="6d150-112">Select `using System.ComponentModel.DataAnnotations;`</span></span>

  ![usando System.ComponentModel.DataAnnotations na parte superior da lista](da1/da.png)

  <span data-ttu-id="6d150-114">O Visual Studio adiciona `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="6d150-114">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

[!INCLUDE [model1](../../includes/RP/da2.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="6d150-115">[Anterior: Trabalhando com o LocalDB do SQL Server](xref:tutorials/razor-pages/sql)
> [Adicionar pesquisa](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="6d150-115">[Previous: Working with SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[Add search](xref:tutorials/razor-pages/search)</span></span>
