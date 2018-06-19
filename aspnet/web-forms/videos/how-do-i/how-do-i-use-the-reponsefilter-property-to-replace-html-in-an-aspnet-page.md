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
<a name="how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page"></a><span data-ttu-id="2ed53-104">[Como fazer:] Use a propriedade Reponse.Filter para substituir o HTML em uma página ASP.NET</span><span class="sxs-lookup"><span data-stu-id="2ed53-104">[How Do I:] Use the Reponse.Filter Property to Replace HTML in an ASP.NET Page</span></span>
====================
<span data-ttu-id="2ed53-105">por [Carlos Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="2ed53-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="2ed53-106">Este Chris Pels vídeo mostra como usar a propriedade Reponse.Filter para interceptar e alterar o HTML que está sendo enviado para uma página.</span><span class="sxs-lookup"><span data-stu-id="2ed53-106">In this video Chris Pels shows how to use the Reponse.Filter property to intercept and alter the HTML being sent to a page.</span></span> <span data-ttu-id="2ed53-107">Primeiro, uma página de exemplo é criada com algum texto simple.</span><span class="sxs-lookup"><span data-stu-id="2ed53-107">First, a sample page is created with some simple text.</span></span> <span data-ttu-id="2ed53-108">Em seguida, uma classe personalizada de fluxo é criada que serve como o fluxo de substituição para o fluxo atual que está sendo enviado para o navegador do usuário.</span><span class="sxs-lookup"><span data-stu-id="2ed53-108">Then, a custom Stream class is created which serves as the replacement stream for the current stream being sent to the user's browser.</span></span> <span data-ttu-id="2ed53-109">Essa classe de fluxo personalizados o conteúdo da página é recuperado do fluxo, alterada e, em seguida, é gravado no fluxo de resposta.</span><span class="sxs-lookup"><span data-stu-id="2ed53-109">In that custom stream class the contents of the page are retrieved from the stream, altered, and then written out to the response stream.</span></span> <span data-ttu-id="2ed53-110">Nessa classe de fluxo personalizados o método Write é personalizado para substituir o HTML no fluxo de resposta base, alterando assim que for enviado para o navegador do usuário.</span><span class="sxs-lookup"><span data-stu-id="2ed53-110">In this custom Stream class the Write method is customized to replace the HTML in the base Response stream, thereby altering what is sent to the user's browser.</span></span> <span data-ttu-id="2ed53-111">Por fim, a nova classe de fluxo é atribuída à propriedade Response na página\_evento de carregamento, assim, fornecendo o mecanismo para alterar o conteúdo da página.</span><span class="sxs-lookup"><span data-stu-id="2ed53-111">Finally, the new stream class is assigned to the Response.Filter property in the Page\_Load event, thereby, providing the mechanism for altering the page content.</span></span>

[<span data-ttu-id="2ed53-112">&#9654; Assista ao vídeo (13 minutos)</span><span class="sxs-lookup"><span data-stu-id="2ed53-112">&#9654; Watch video (13 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page)
