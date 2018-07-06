---
uid: web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
title: Criação de URLs legíveis na Web do ASP.NET (Razor) Sites de páginas | Microsoft Docs
author: tfitzmac
description: Este artigo descreve o roteamento em um site de páginas da Web do ASP.NET (Razor) e como isso permite que você use URLs que são mais legíveis e melhor para SEO. O que você irá...
ms.author: aspnetcontent
ms.date: 02/17/2014
ms.assetid: a8aac1ac-89de-4415-afe0-97a41c6423d2
msc.legacyurl: /web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 3304a226e374618a567e69ac72448a9964a34c47
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809865"
---
<a name="creating-readable-urls-in-aspnet-web-pages-razor-sites"></a><span data-ttu-id="be463-104">Criação de URLs legíveis em Sites do ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="be463-104">Creating Readable URLs in ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="be463-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="be463-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="be463-106">Este artigo descreve o roteamento em um site de páginas da Web do ASP.NET (Razor) e como isso permite que você use URLs que são mais legíveis e melhor para SEO.</span><span class="sxs-lookup"><span data-stu-id="be463-106">This article describes routing in an ASP.NET Web Pages (Razor) website, and how this lets you use URLs that are more readable and better for SEO.</span></span>
> 
> <span data-ttu-id="be463-107">O que você aprenderá:</span><span class="sxs-lookup"><span data-stu-id="be463-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="be463-108">Como o ASP.NET usa o roteamento para permitem que você use as URLs mais legíveis e pesquisáveis.</span><span class="sxs-lookup"><span data-stu-id="be463-108">How ASP.NET uses routing to let you use more readable and searchable URLs.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="be463-109">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="be463-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="be463-110">Páginas da Web do ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="be463-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="be463-111">Este tutorial também funciona com ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="be463-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="about-routing"></a><span data-ttu-id="be463-112">Sobre roteamento</span><span class="sxs-lookup"><span data-stu-id="be463-112">About Routing</span></span>

<span data-ttu-id="be463-113">As URLs para as páginas em seu site podem ter um impacto sobre como o site funciona.</span><span class="sxs-lookup"><span data-stu-id="be463-113">The URLs for the pages in your site can have an impact on how well the site works.</span></span> <span data-ttu-id="be463-114">Uma URL que &quot;amigável&quot; pode tornar mais fácil para as pessoas a usar o site.</span><span class="sxs-lookup"><span data-stu-id="be463-114">A URL that's &quot;friendly&quot; can make it easier for people to use the site.</span></span> <span data-ttu-id="be463-115">Ele também pode ajudar com a otimização do mecanismo de pesquisa (SEO) para o site.</span><span class="sxs-lookup"><span data-stu-id="be463-115">It can also help with search-engine optimization (SEO) for the site.</span></span> <span data-ttu-id="be463-116">Sites do ASP.NET incluem a capacidade de usar URLs amigáveis automaticamente.</span><span class="sxs-lookup"><span data-stu-id="be463-116">ASP.NET websites include the ability to use friendly URLs automatically.</span></span>

<span data-ttu-id="be463-117">ASP.NET permite que você crie URLs significativas que descrevem as ações do usuário em vez de apenas apontando para um arquivo no servidor.</span><span class="sxs-lookup"><span data-stu-id="be463-117">ASP.NET lets you create meaningful URLs that describe user actions instead of just pointing to a file on the server.</span></span> <span data-ttu-id="be463-118">Considere estas URLs para um blog fictício:</span><span class="sxs-lookup"><span data-stu-id="be463-118">Consider these URLs for a fictional blog:</span></span>

- `http://www.contoso.com/Blog/blog.cshtml?categories=hardware`
- `http://www.contoso.com//Blog/blog.cshtml?startdate=2009-11-01&enddate=2009-11-30`

<span data-ttu-id="be463-119">Compare essas URLs para os seguintes:</span><span class="sxs-lookup"><span data-stu-id="be463-119">Compare those URLs to the following ones:</span></span>

- `http://www.contoso.com/Blog/categories/hardware/`
- `http://www.contoso.com/Blog/2009/November`

<span data-ttu-id="be463-120">O primeiro par, um usuário teria saber que o blog é exibido usando o *blog.cshtml* página e, em seguida, teria que construir uma cadeia de caracteres de consulta que obtém o intervalo de categoria ou datas à direita.</span><span class="sxs-lookup"><span data-stu-id="be463-120">In the first pair, a user would have to know that the blog is displayed using the *blog.cshtml* page, and would then have to construct a query string that gets the right category or date range.</span></span> <span data-ttu-id="be463-121">O segundo conjunto de exemplos é muito mais fácil de compreender e criar.</span><span class="sxs-lookup"><span data-stu-id="be463-121">The second set of examples is much easier to comprehend and create.</span></span>

<span data-ttu-id="be463-122">As URLs para o primeiro exemplo também apontam diretamente para um arquivo específico (*blog.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="be463-122">The URLs for the first example also point directly to a specific file (*blog.cshtml*).</span></span> <span data-ttu-id="be463-123">Se por algum motivo o blog foram movido para outra pasta no servidor, ou se o blog foram reescrito para usar uma página diferente, os links seria errados.</span><span class="sxs-lookup"><span data-stu-id="be463-123">If for some reason the blog were moved to another folder on the server, or if the blog were rewritten to use a different page, the links would be wrong.</span></span> <span data-ttu-id="be463-124">O segundo conjunto de URLs não aponta para uma página específica, assim, mesmo que a implementação de blog ou o local é alterado, as URLs ainda seria válidas.</span><span class="sxs-lookup"><span data-stu-id="be463-124">The second set of URLs doesn't point to a specific page, so even if the blog implementation or location changes, the URLs would still be valid.</span></span>

<span data-ttu-id="be463-125">Em páginas de Web do ASP.NET, você pode criar URLs mais amigáveis, como aqueles nos exemplos acima como o ASP.NET usa *roteamento*.</span><span class="sxs-lookup"><span data-stu-id="be463-125">In ASP.NET Web Pages, you can create friendlier URLs like those in the above examples because ASP.NET uses *routing*.</span></span> <span data-ttu-id="be463-126">Roteamento cria o mapeamento de lógico de uma URL para uma página (ou páginas) que pode atender à solicitação.</span><span class="sxs-lookup"><span data-stu-id="be463-126">Routing creates logical mapping from a URL to a page (or pages) that can fulfill the request.</span></span> <span data-ttu-id="be463-127">Como o mapeamento é lógico (não física, em um arquivo específico), o roteamento fornece grande flexibilidade em como definir as URLs para o seu site.</span><span class="sxs-lookup"><span data-stu-id="be463-127">Because the mapping is logical (not physical, to a specific file), routing provides great flexibility in how you define the URLs for your site.</span></span>

## <a name="how-routing-works"></a><span data-ttu-id="be463-128">Como funciona o roteamento</span><span class="sxs-lookup"><span data-stu-id="be463-128">How Routing Works</span></span>

<span data-ttu-id="be463-129">Quando o ASP.NET processa uma solicitação, ele lê a URL para determinar como roteá-lo.</span><span class="sxs-lookup"><span data-stu-id="be463-129">When ASP.NET processes a request, it reads the URL to determine how to route it.</span></span> <span data-ttu-id="be463-130">O ASP.NET tenta corresponder segmentos individuais da URL para arquivos no disco, indo da esquerda para a direita.</span><span class="sxs-lookup"><span data-stu-id="be463-130">ASP.NET tries to match individual segments of the URL to files on disk, going from left to right.</span></span> <span data-ttu-id="be463-131">Se houver uma correspondência, qualquer coisa restante na URL é passada para a página como *informações de caminho*.</span><span class="sxs-lookup"><span data-stu-id="be463-131">If there's a match, anything remaining in the URL is passed to the page as *path information*.</span></span>

<span data-ttu-id="be463-132">Imagine que alguém faz uma solicitação usando esta URL:</span><span class="sxs-lookup"><span data-stu-id="be463-132">Imagine that someone makes a request using this URL:</span></span>

`http://www.contoso.com/a/b/c`

<span data-ttu-id="be463-133">A pesquisa funciona assim:</span><span class="sxs-lookup"><span data-stu-id="be463-133">The search goes like this:</span></span>

1. <span data-ttu-id="be463-134">Há um arquivo com o caminho e o nome da */a/b/c.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="be463-134">Is there a file with the path and name of */a/b/c.cshtml*?</span></span> <span data-ttu-id="be463-135">Nesse caso, executar nessa página e não passá-lo para nenhuma informação.</span><span class="sxs-lookup"><span data-stu-id="be463-135">If so, run that page and pass no information to it.</span></span> <span data-ttu-id="be463-136">Caso contrário,...</span><span class="sxs-lookup"><span data-stu-id="be463-136">Otherwise ...</span></span>
2. <span data-ttu-id="be463-137">Há um arquivo com o caminho e o nome da */a/b.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="be463-137">Is there a file with the path and name of */a/b.cshtml*?</span></span> <span data-ttu-id="be463-138">Se assim, executar nessa página e passar o valor `c` a ele.</span><span class="sxs-lookup"><span data-stu-id="be463-138">If so, run that page and pass the value `c` to it.</span></span> <span data-ttu-id="be463-139">Caso contrário,...</span><span class="sxs-lookup"><span data-stu-id="be463-139">Otherwise …</span></span>
3. <span data-ttu-id="be463-140">Há um arquivo com o caminho e o nome da */a.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="be463-140">Is there a file with the path and name of */a.cshtml*?</span></span> <span data-ttu-id="be463-141">Se assim, executar nessa página e passar o valor `b/c` a ele.</span><span class="sxs-lookup"><span data-stu-id="be463-141">If so, run that page and pass the value `b/c` to it.</span></span>

<span data-ttu-id="be463-142">Se a pesquisa encontrada exata não corresponde a para *. cshtml* arquivos em suas pastas especificados, o ASP.NET continua procurando esses arquivos por sua vez:</span><span class="sxs-lookup"><span data-stu-id="be463-142">If the search found no exact matches for *.cshtml* files in their specified folders, ASP.NET continues looking for these files in turn:</span></span>

1. <span data-ttu-id="be463-143">*/a/b/c/default.cshtml* (nenhuma informação de caminho).</span><span class="sxs-lookup"><span data-stu-id="be463-143">*/a/b/c/default.cshtml* (no path information).</span></span>
2. <span data-ttu-id="be463-144">*/a/b/c/index.cshtml* (nenhuma informação de caminho).</span><span class="sxs-lookup"><span data-stu-id="be463-144">*/a/b/c/index.cshtml* (no path information).</span></span>

> [!NOTE]
> <span data-ttu-id="be463-145">Para ser honesto, as solicitações para páginas específicas (ou seja, as solicitações que incluem o *. cshtml* extensão de nome de arquivo) funcionará da mesma forma que você esperaria.</span><span class="sxs-lookup"><span data-stu-id="be463-145">To be clear, requests for specific pages (that is, requests that include the *.cshtml* filename extension) work just like you'd expect.</span></span> <span data-ttu-id="be463-146">Uma solicitação como `http://www.contoso.com/a/b.cshtml` executará a página *cshtml* bem.</span><span class="sxs-lookup"><span data-stu-id="be463-146">A request like `http://www.contoso.com/a/b.cshtml` will run the page *b.cshtml* just fine.</span></span>


<span data-ttu-id="be463-147">Dentro de uma página, você pode obter as informações de caminho por meio da página `UrlData` propriedade, que é um dicionário.</span><span class="sxs-lookup"><span data-stu-id="be463-147">Inside a page, you can get the path information via the page's `UrlData` property, which is a dictionary.</span></span> <span data-ttu-id="be463-148">Imagine que você tenha um arquivo chamado *ViewCustomers.cshtml* e seu site recebe essa solicitação:</span><span class="sxs-lookup"><span data-stu-id="be463-148">Imagine that you have a file named *ViewCustomers.cshtml* and your site gets this request:</span></span>

`http://mysite.com/myWebSite/ViewCustomers/1000`

<span data-ttu-id="be463-149">Conforme descrito nas regras acima, a solicitação irá para a página.</span><span class="sxs-lookup"><span data-stu-id="be463-149">As described in the rules above, the request will go to your page.</span></span> <span data-ttu-id="be463-150">Dentro da página, você pode usar código semelhante ao seguinte para obter e exibir as informações de caminho (neste caso, o valor &quot;1000&quot;):</span><span class="sxs-lookup"><span data-stu-id="be463-150">Inside the page, you can use code like the following to get and display the path information (in this case, the value &quot;1000&quot;):</span></span>

[!code-html[Main](creating-readable-urls-in-aspnet-web-pages-sites/samples/sample1.html)]

> [!NOTE]
> <span data-ttu-id="be463-151">Como o roteamento não envolve a nomes de arquivo completos, pode haver ambiguidade se você tiver páginas que têm o mesmo nome, mas diferentes extensões de nome de arquivo (por exemplo, *MyPage.cshtml* e *MyPage.html*) .</span><span class="sxs-lookup"><span data-stu-id="be463-151">Because routing doesn't involve complete file names, there can be ambiguity if you have pages that have the same name but different file-name extensions (for example, *MyPage.cshtml* and *MyPage.html*).</span></span> <span data-ttu-id="be463-152">Para evitar problemas com o roteamento, é melhor certificar-se de que você não tem páginas em seu site cujos nomes diferem apenas em sua extensão.</span><span class="sxs-lookup"><span data-stu-id="be463-152">In order to avoid problems with routing, it's best to make sure that you don't have pages in your site whose names differ only in their extension.</span></span>


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="be463-153">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="be463-153">Additional Resources</span></span>

<span data-ttu-id="be463-154">[O WebMatrix - URLs, UrlData e roteamento para SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO).</span><span class="sxs-lookup"><span data-stu-id="be463-154">[WebMatrix - URLs, UrlData and Routing for SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO).</span></span> <span data-ttu-id="be463-155">Essa entrada de blog por Mike Brind fornece alguns detalhes adicionais sobre como funciona o roteamento em páginas da Web do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="be463-155">This blog entry by Mike Brind provides some additional details on how routing works in ASP.NET Web Pages.</span></span>
