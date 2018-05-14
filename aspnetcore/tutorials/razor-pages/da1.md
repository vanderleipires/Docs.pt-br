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
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a>Atualizar as páginas geradas em um aplicativo ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Temos um bom começo para o aplicativo de filme, mas a apresentação não é ideal. Você não deseja ver a hora (12:00:00 AM na imagem abaixo) e **ReleaseDate** deve ser **Release Date** (duas palavras).

![Aplicativo de filme aberto no Chrome mostrando os dados do filme](sql/_static/m55.png)

## <a name="update-the-generated-code"></a>Atualize o código gerado

Abra o arquivo *Models/Movie.cs* e adicione as linhas realçadas mostradas no seguinte código:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

Clique com o botão direito do mouse em uma linha curvada vermelha > ** Ações Rápidas e Refatorações**.

  ![Menu contextual mostra **> Ações Rápidas e Refatorações**.](da1/qa.png)

Selecione `using System.ComponentModel.DataAnnotations;`

  ![usando System.ComponentModel.DataAnnotations na parte superior da lista](da1/da.png)

  O Visual Studio adiciona `using System.ComponentModel.DataAnnotations;`.

[!INCLUDE [model1](../../includes/RP/da2.md)]

> [!div class="step-by-step"]
> [Anterior: Trabalhando com o LocalDB do SQL Server](xref:tutorials/razor-pages/sql)
> [Adicionar pesquisa](xref:tutorials/razor-pages/search)
