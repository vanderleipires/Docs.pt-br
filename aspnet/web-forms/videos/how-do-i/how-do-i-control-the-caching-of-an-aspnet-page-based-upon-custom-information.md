---
uid: web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
title: '[Como fazer:] Controle de cache de uma página ASP.NET com base nas informações personalizadas | Microsoft Docs'
author: rick-anderson
description: Este Chris Pels vídeo mostra como controlar os critérios para armazenar em cache uma página ASP.NET com base em informações personalizadas. Um exemplo de página é criado e, em seguida, ão...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/19/2009
ms.topic: article
ms.assetid: f230c316-1313-4b8f-967c-62f9684fe378
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
msc.type: video
ms.openlocfilehash: b785de4e1161ae82ee458148aee4b30820801147
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26528125"
---
<a name="how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information"></a><span data-ttu-id="f1e34-104">[Como fazer:] Controle de cache de uma página ASP.NET com base nas informações personalizadas</span><span class="sxs-lookup"><span data-stu-id="f1e34-104">[How Do I:] Control the Caching of an ASP.NET Page Based Upon Custom Information</span></span>
====================
<span data-ttu-id="f1e34-105">por [Carlos Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="f1e34-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="f1e34-106">Este Chris Pels vídeo mostra como controlar os critérios para armazenar em cache uma página ASP.NET com base em informações personalizadas.</span><span class="sxs-lookup"><span data-stu-id="f1e34-106">In this video Chris Pels shows how to control the criteria for caching an ASP.NET page based upon custom information.</span></span> <span data-ttu-id="f1e34-107">Um exemplo de página é criado e, em seguida, a diretiva OutputCache é usada com o atributo VaryByCustom que contém um valor personalizado.</span><span class="sxs-lookup"><span data-stu-id="f1e34-107">A sample page is created and then the OutputCache directive is used with the VaryByCustom attribute which contains a custom value.</span></span> <span data-ttu-id="f1e34-108">Em seguida, o método GetVaryCustomByString() é substituído no módulo global. asax que fornece a manipulação do atributo personalizado.</span><span class="sxs-lookup"><span data-stu-id="f1e34-108">Next, the GetVaryCustomByString() method is overridden in the global.asax module which provides the handling of the custom attribute.</span></span> <span data-ttu-id="f1e34-109">Nesse método, uma cadeia de caracteres é retornada que identifica exclusivamente a versão armazenada em cache da página.</span><span class="sxs-lookup"><span data-stu-id="f1e34-109">In that method a string is returned that uniquely identifies the cached version of the page.</span></span> <span data-ttu-id="f1e34-110">Por fim, há uma discussão sobre como cache usando um valor personalizado pode ser usado de várias maneiras para um site da web.</span><span class="sxs-lookup"><span data-stu-id="f1e34-110">Finally, there is a discussion about how caching using a custom value can be used in several ways for a web site.</span></span>

[<span data-ttu-id="f1e34-111">&#9654; Assista ao vídeo (12 minutos)</span><span class="sxs-lookup"><span data-stu-id="f1e34-111">&#9654; Watch video (12 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information)
