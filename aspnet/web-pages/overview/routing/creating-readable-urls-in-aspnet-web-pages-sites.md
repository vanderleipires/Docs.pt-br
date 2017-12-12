---
uid: web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
title: "Criar legíveis URLs da Web ASP.NET de páginas de Sites (Razor) | Microsoft Docs"
author: tfitzmac
description: "Este artigo descreve o roteamento em um site de páginas da Web do ASP.NET (Razor) e como isso permite usar URLs mais legível e melhor para SEO. O que você vai..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: a8aac1ac-89de-4415-afe0-97a41c6423d2
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 7858b7cbd6dccafb2867ed9a1d102561e211e435
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="creating-readable-urls-in-aspnet-web-pages-razor-sites"></a>Criando URLs legíveis em Sites de páginas (Razor) da Web ASP.NET
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo descreve o roteamento em um site de páginas da Web do ASP.NET (Razor) e como isso permite usar URLs mais legível e melhor para SEO.
> 
> O que você aprenderá:
> 
> - Como o ASP.NET usa roteamento para permitir o uso de URLs mais legíveis e pesquisáveis.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Páginas da Web do ASP.NET (Razor) 3
>   
> 
> Este tutorial também funciona com 2 de páginas da Web do ASP.NET.


## <a name="about-routing"></a>Sobre roteamento

As URLs para as páginas em seu site podem ter um impacto em como o site funciona. Uma URL que &quot;amigável&quot; pode tornar mais fácil para as pessoas usarem o site. Ele também pode ajudar com o mecanismo de pesquisa SEO (otimização) para o site. Sites ASP.NET incluem a capacidade de usar URLs amigáveis automaticamente.

ASP.NET permite que você crie URLs significativos que descrevem as ações do usuário em vez de simplesmente apontando para um arquivo no servidor. Considere estas URLs para um blog fictício:

- `http://www.contoso.com/Blog/blog.cshtml?categories=hardware`
- `http://www.contoso.com//Blog/blog.cshtml?startdate=2009-11-01&enddate=2009-11-30`

Compare as URLs para os seguintes:

- `http://www.contoso.com/Blog/categories/hardware/`
- `http://www.contoso.com/Blog/2009/November`

No primeiro par, um usuário precisa saber que o blog é exibido usando o *blog.cshtml* página e, em seguida, teria que construir uma cadeia de caracteres de consulta que obtém o intervalo de data ou categoria à direita. O segundo conjunto de exemplos é muito mais fácil de entender e criar.

As URLs para o primeiro exemplo também apontem diretamente para um arquivo específico (*blog.cshtml*). Se por algum motivo o blog foram movido para outra pasta no servidor, ou se o blog foram reescrito para usar uma página diferente, os links seria errados. O segundo conjunto de URLs não aponta para uma página específica, assim, mesmo se a implementação de blog ou local for alterada, as URLs ainda será válidas.

Em páginas de Web do ASP.NET, você pode criar mais amigáveis URLs como aqueles nos exemplos acima como o ASP.NET usa *roteamento*. Roteamento cria um mapeamento de lógico de uma URL para uma página (ou páginas) que pode atender à solicitação. Como o mapeamento é lógico (não física, em um arquivo específico), roteamento fornece uma grande flexibilidade em como você definir as URLs para o seu site.

## <a name="how-routing-works"></a>Como funciona o roteamento

Quando o ASP.NET processa uma solicitação, ele lê a URL para determinar como encaminhá-lo. ASP.NET tenta corresponder segmentos individuais da URL para arquivos no disco, vai da esquerda para a direita. Se houver uma correspondência, nada restantes na URL é passado para a página como *informações de caminho*.

Imagine que alguém faz uma solicitação usando esta URL:

`http://www.contoso.com/a/b/c`

A pesquisa é a seguinte:

1. Há um arquivo com o caminho e nome de */a/b/c.cshtml*? Nesse caso, execute a página e não passá-lo para nenhuma informação. Caso contrário,...
2. Há um arquivo com o caminho e nome de */a/b.cshtml*? Se assim, executar a página e passar o valor `c` a ele. Caso contrário,...
3. Há um arquivo com o caminho e nome de */a.cshtml*? Se assim, executar a página e passar o valor `b/c` a ele.

Se a pesquisa encontrada exata não corresponde para *. cshtml* arquivos em suas pastas especificadas, o ASP.NET continua procurando esses arquivos por sua vez:

1. */a/b/c/default.cshtml* (nenhuma informação de caminho).
2. */a/b/c/index.cshtml* (nenhuma informação de caminho).

> [!NOTE]
> Para ser claro, solicitações de páginas específicas (ou seja, as solicitações que incluem o *. cshtml* extensão de nome de arquivo) funcionam como esperado. Como uma solicitação `http://www.contoso.com/a/b.cshtml` executará a página *b.cshtml* sem problemas.


Dentro de uma página, você pode obter as informações de caminho por meio da página `UrlData` propriedade, que é um dicionário. Imagine que você tenha um arquivo chamado *ViewCustomers.cshtml* e seu site recebe essa solicitação:

`http://mysite.com/myWebSite/ViewCustomers/1000`

Conforme descrito nas regras acima, a solicitação irá para a página. Dentro da página, você pode usar o código como o seguinte para obter e exibir as informações de caminho (neste caso, o valor &quot;1000&quot;):

[!code-html[Main](creating-readable-urls-in-aspnet-web-pages-sites/samples/sample1.html)]

> [!NOTE]
> Como o roteamento não envolve a nomes de arquivo completos, pode haver ambiguidade se você tiver páginas que têm o mesmo nome mas diferentes extensões de nome de arquivo (por exemplo, *MyPage.cshtml* e *MyPage.html*) . Para evitar problemas com o roteamento, é melhor certificar-se de que você não tem páginas em seu site cujos nomes diferem apenas em sua extensão.


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionais

[O WebMatrix - URLs, UrlData e roteamento para SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO). Essa entrada de blog, Mike Brind fornece alguns detalhes adicionais sobre como roteamento funciona em páginas da Web do ASP.NET.
