---
title: Atualizar as páginas geradas em um aplicativo ASP.NET Core
author: rick-anderson
description: Saiba como atualizar as páginas geradas em um aplicativo ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/da1
ms.openlocfilehash: f47a68840a6307b69bc92a7b157037d91dce5422
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477209"
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a>Atualizar as páginas geradas em um aplicativo ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Temos um bom começo para o aplicativo de filme, mas a apresentação não é ideal. Você não deseja ver a hora (12:00:00 AM na imagem abaixo) e **ReleaseDate** deve ser **Release Date** (duas palavras).

![Aplicativo de filme aberto no Chrome mostrando os dados do filme](sql/_static/m55.png)

## <a name="update-the-generated-code"></a>Atualize o código gerado

Abra o arquivo *Models/Movie.cs* e adicione as linhas realçadas mostradas no seguinte código:

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/MovieDate.cs?name=snippet_1&highlight=10-11,15)]

::: moniker-end

Clique com o botão direito do mouse em uma linha vermelha ondulada > **Ações Rápidas e Refatorações**.

  ![Menu contextual mostra **> Ações Rápidas e Refatorações**.](da1/qa.png)

Selecione `using System.ComponentModel.DataAnnotations;`

  ![usando System.ComponentModel.DataAnnotations na parte superior da lista](da1/da.png)

  O Visual Studio adiciona `using System.ComponentModel.DataAnnotations;`.

[!INCLUDE [model1](~/includes/RP/da2.md)]

> [!div class="step-by-step"]
> [Anterior: trabalhando com o LocalDB do SQL Server](xref:tutorials/razor-pages/sql)
> [Próximo: adicionar pesquisa](xref:tutorials/razor-pages/search)
