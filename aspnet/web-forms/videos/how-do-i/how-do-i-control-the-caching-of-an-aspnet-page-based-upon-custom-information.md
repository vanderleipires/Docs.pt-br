---
uid: web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
title: '[Como fazer:] O armazenamento em cache de uma página ASP.NET com base em informações personalizadas de controle | Microsoft Docs'
author: rick-anderson
description: Neste vídeo, Chris Pels mostra como controlar os critérios para uma página ASP.NET com base em informações personalizadas de cache. Uma página de exemplo é criada e, em seguida, ão....
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/19/2009
ms.topic: article
ms.assetid: f230c316-1313-4b8f-967c-62f9684fe378
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
msc.type: video
ms.openlocfilehash: d2c8e2384d39255f66c11f1cc303398750229779
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376014"
---
<a name="how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information"></a><span data-ttu-id="04876-104">[Como fazer:] O armazenamento em cache de uma página ASP.NET com base em informações personalizadas de controle</span><span class="sxs-lookup"><span data-stu-id="04876-104">[How Do I:] Control the Caching of an ASP.NET Page Based Upon Custom Information</span></span>
====================
<span data-ttu-id="04876-105">por [Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="04876-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="04876-106">Neste vídeo, Chris Pels mostra como controlar os critérios para uma página ASP.NET com base em informações personalizadas de cache.</span><span class="sxs-lookup"><span data-stu-id="04876-106">In this video Chris Pels shows how to control the criteria for caching an ASP.NET page based upon custom information.</span></span> <span data-ttu-id="04876-107">Uma página de exemplo é criada e, em seguida, a diretiva OutputCache é usada com o atributo VaryByCustom que contém um valor personalizado.</span><span class="sxs-lookup"><span data-stu-id="04876-107">A sample page is created and then the OutputCache directive is used with the VaryByCustom attribute which contains a custom value.</span></span> <span data-ttu-id="04876-108">Em seguida, o método GetVaryCustomByString() é substituído no módulo global. asax que fornece a manipulação do atributo personalizado.</span><span class="sxs-lookup"><span data-stu-id="04876-108">Next, the GetVaryCustomByString() method is overridden in the global.asax module which provides the handling of the custom attribute.</span></span> <span data-ttu-id="04876-109">Nesse método, uma cadeia de caracteres é retornada que identifica exclusivamente a versão em cache da página.</span><span class="sxs-lookup"><span data-stu-id="04876-109">In that method a string is returned that uniquely identifies the cached version of the page.</span></span> <span data-ttu-id="04876-110">Por fim, há uma discussão sobre como cache usando um valor personalizado pode ser usado de várias maneiras para um site da web.</span><span class="sxs-lookup"><span data-stu-id="04876-110">Finally, there is a discussion about how caching using a custom value can be used in several ways for a web site.</span></span>

[<span data-ttu-id="04876-111">&#9654;Assista ao vídeo (12 minutos)</span><span class="sxs-lookup"><span data-stu-id="04876-111">&#9654; Watch video (12 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information)
