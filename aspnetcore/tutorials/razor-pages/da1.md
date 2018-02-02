---
title: "Atualizar as páginas geradas"
author: rick-anderson
description: "Atualize as páginas geradas com melhor exibição."
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/da1
ms.openlocfilehash: a1bb1ab1e4fac9c634f4048947ac3f934af3d625
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="update-the-generated-pages"></a>Atualizar as páginas geradas

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Temos um bom começo para o aplicativo de filme, mas a apresentação não é ideal. Você não deseja ver a hora (12:00:00 AM na imagem abaixo) e **ReleaseDate** deve ser **Release Date** (duas palavras).

![Aplicativo de filme aberto no Chrome mostrando os dados do filme](sql/_static/m55.png)

## <a name="update-the-generated-code"></a>Atualize o código gerado

Abra o arquivo *Models/Movie.cs* e adicione as linhas realçadas mostradas no seguinte código:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

Clique com o botão direito do mouse em uma linha curvada vermelha > ** Ações Rápidas e Refatorações**.

  ![Menu contextual mostra **> Ações Rápidas e Refatorações**.](da1/qa.png)

Selecione `using System.ComponentModel.DataAnnotations;`

  ![usando System.ComponentModel.DataAnnotations na parte superior da lista](da1/da.png)

  O Visual Studio adiciona `using System.ComponentModel.DataAnnotations;`.

[!INCLUDE[model1](../../includes/RP/da2.md)]

>[!div class="step-by-step"]
[Anterior: Trabalhando com o SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[Adicionando uma pesquisa](xref:tutorials/razor-pages/search)
