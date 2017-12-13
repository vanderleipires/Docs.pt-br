---
uid: web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
title: "Empacotando e minimizando ativos em uma Web ASP.NET páginas Site (Razor) | Microsoft Docs"
author: microsoft
description: "Empacotamento e minimização são maneiras de acelerar seu site. Agrupamento permite que você combine vários arquivos JavaScript (. js) ou vários folha estilos em cascata (..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/21/2012
ms.topic: article
ms.assetid: 8906f1e9-4b66-4a03-8e8a-9e9debf8ed91
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
msc.type: authoredcontent
ms.openlocfilehash: df00b5c9e741e429011143d04121df97c1930602
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="2d412-104">Empacotando e minimizando os ativos em um Site de páginas da Web (Razor) do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="2d412-104">Bundling and Minifying Assets in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="2d412-105">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="2d412-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="2d412-106">Empacotamento e minimização são maneiras de acelerar seu site.</span><span class="sxs-lookup"><span data-stu-id="2d412-106">Bundling and minification are ways to make your site faster.</span></span> <span data-ttu-id="2d412-107">Agrupamento permite que você combine várias JavaScript (*. js*) arquivos ou em várias folhas de estilo em cascata (*. CSS*) arquivos para que eles podem ser baixados como uma unidade, em vez de um de cada vez.</span><span class="sxs-lookup"><span data-stu-id="2d412-107">Bundling lets you combine multiple JavaScript (*.js*) files or multiple cascading style sheet (*.css*) files so that they can be downloaded as a unit, rather than one at a time.</span></span> <span data-ttu-id="2d412-108">Minimização comprime limite de espaço em branco e executa outros tipos de compactação para tornar os arquivos baixados como pequeno uma possível.</span><span class="sxs-lookup"><span data-stu-id="2d412-108">Minification squeezes out whitespace and performs other types of compression to make the downloaded files as small a possible.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="2d412-109">A versão RC 2 de páginas da Web do ASP.NET não dá suporte a empacotamento e minimização porque o pacote que contém os elementos necessários ainda não está disponível no Microsoft WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="2d412-109">The RC release of ASP.NET Web Pages 2 does not support bundling and minification because the package that contains the required elements is not yet available in Microsoft WebMatrix.</span></span> <span data-ttu-id="2d412-110">Pedimos desculpas pelo inconveniente.</span><span class="sxs-lookup"><span data-stu-id="2d412-110">We apologize for this inconvenience.</span></span> <span data-ttu-id="2d412-111">O pacote deve estar disponível na versão final de 2 de páginas da Web do ASP.NET e o WebMatrix 2.</span><span class="sxs-lookup"><span data-stu-id="2d412-111">The package is expected to be available in the final release of ASP.NET Web Pages 2 and WebMatrix 2.</span></span>
