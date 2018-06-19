---
uid: web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
title: '[Como fazer:] Use a propriedade Reponse.Filter para substituir o HTML em uma página ASP.NET | Microsoft Docs'
author: rick-anderson
description: Este Chris Pels vídeo mostra como usar a propriedade Reponse.Filter para interceptar e alterar o HTML que está sendo enviado para uma página. Primeiro, uma página de exemplo é criada w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/29/2009
ms.topic: article
ms.assetid: 3e5ae74a-9798-47d8-a2b3-0d8ad42dd4bc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
msc.type: video
ms.openlocfilehash: 3e7c1f2a947d185d7c2a01deb75e42c92ba914c3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26525525"
---
<a name="how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page"></a>[Como fazer:] Use a propriedade Reponse.Filter para substituir o HTML em uma página ASP.NET
====================
por [Carlos Pels](https://twitter.com/chrispels)

Este Chris Pels vídeo mostra como usar a propriedade Reponse.Filter para interceptar e alterar o HTML que está sendo enviado para uma página. Primeiro, uma página de exemplo é criada com algum texto simple. Em seguida, uma classe personalizada de fluxo é criada que serve como o fluxo de substituição para o fluxo atual que está sendo enviado para o navegador do usuário. Essa classe de fluxo personalizados o conteúdo da página é recuperado do fluxo, alterada e, em seguida, é gravado no fluxo de resposta. Nessa classe de fluxo personalizados o método Write é personalizado para substituir o HTML no fluxo de resposta base, alterando assim que for enviado para o navegador do usuário. Por fim, a nova classe de fluxo é atribuída à propriedade Response na página\_evento de carregamento, assim, fornecendo o mecanismo para alterar o conteúdo da página.

[&#9654; Assista ao vídeo (13 minutos)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page)
