---
title: Atualizar as páginas geradas em um aplicativo ASP.NET Core
author: rick-anderson
description: Saiba como atualizar as páginas geradas em um aplicativo ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/da1
ms.openlocfilehash: 78490be1cfa3018c465cb1e8125918404a7e4525
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011588"
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a><span data-ttu-id="fe9f5-103">Atualizar as páginas geradas em um aplicativo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fe9f5-103">Update the generated pages in an ASP.NET Core app</span></span>

<span data-ttu-id="fe9f5-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fe9f5-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="fe9f5-105">Temos um bom começo para o aplicativo de filme, mas a apresentação não é ideal.</span><span class="sxs-lookup"><span data-stu-id="fe9f5-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="fe9f5-106">Você não deseja ver a hora (12:00:00 AM na imagem abaixo) e **ReleaseDate** deve ser **Release Date** (duas palavras).</span><span class="sxs-lookup"><span data-stu-id="fe9f5-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![Aplicativo de filme aberto no Chrome mostrando os dados do filme](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="fe9f5-108">Atualize o código gerado</span><span class="sxs-lookup"><span data-stu-id="fe9f5-108">Update the generated code</span></span>

<span data-ttu-id="fe9f5-109">Abra o arquivo *Models/Movie.cs* e adicione as linhas realçadas mostradas no seguinte código:</span><span class="sxs-lookup"><span data-stu-id="fe9f5-109">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/MovieDate.cs?name=snippet_1&highlight=10-11,15)]

::: moniker-end

<span data-ttu-id="fe9f5-110">Clique com o botão direito do mouse em uma linha vermelha ondulada > **Ações Rápidas e Refatorações**.</span><span class="sxs-lookup"><span data-stu-id="fe9f5-110">Right click on a red squiggly line > **Quick Actions and Refactorings**.</span></span>

  ![Menu contextual mostra **> Ações Rápidas e Refatorações**.](da1/qa.png)

<span data-ttu-id="fe9f5-112">Selecione `using System.ComponentModel.DataAnnotations;`</span><span class="sxs-lookup"><span data-stu-id="fe9f5-112">Select `using System.ComponentModel.DataAnnotations;`</span></span>

  ![usando System.ComponentModel.DataAnnotations na parte superior da lista](da1/da.png)

  <span data-ttu-id="fe9f5-114">O Visual Studio adiciona `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="fe9f5-114">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

[!INCLUDE [model1](~/includes/RP/da2.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="fe9f5-115">[Anterior: Trabalhando com o LocalDB do SQL Server](xref:tutorials/razor-pages/sql)
> [Adicionar pesquisa](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="fe9f5-115">[Previous: Working with SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[Add search](xref:tutorials/razor-pages/search)</span></span>
