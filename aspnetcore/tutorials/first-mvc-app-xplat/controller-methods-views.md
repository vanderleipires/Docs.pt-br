---
title: "Exibições e métodos do controlador"
author: rick-anderson
description: "Trabalhando com métodos do controlador, exibições e DataAnnotations"
ms.author: riande
manager: wpickett
ms.date: 04/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-xplat/controller-methods-views
ms.openlocfilehash: 38a91fc6cac4a07f0d30fd9aaa45e473f1283e7d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
# <a name="controller-methods-and-views"></a>Exibições e métodos do controlador

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Temos um bom começo para o aplicativo de filme, mas a apresentação não é ideal. Você não deseja ver a hora (12:00:00 AM na imagem abaixo) e **ReleaseDate** deve ser escrito em duas palavras.

![Exibição de índice: a Data de Lançamento é uma palavra (sem espaço) e cada data de lançamento do filme mostra o horário 12h](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

Abra o arquivo *Models/Movie.cs* e adicione as linhas realçadas mostradas abaixo:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1&highlight=2,11-12)]

Compile e execute o aplicativo.

<!-- include start
![MVC Movie application open browser showing movie data](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

 -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
[Anterior – Trabalhando com o SQLite](working-with-sql.md)
[Próximo – Adicionar uma pesquisa](search.md)  
