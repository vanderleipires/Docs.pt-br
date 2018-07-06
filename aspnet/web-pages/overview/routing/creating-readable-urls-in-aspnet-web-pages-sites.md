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
<a name="creating-readable-urls-in-aspnet-web-pages-razor-sites"></a>Criação de URLs legíveis em Sites do ASP.NET Web Pages (Razor)
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo descreve o roteamento em um site de páginas da Web do ASP.NET (Razor) e como isso permite que você use URLs que são mais legíveis e melhor para SEO.
> 
> O que você aprenderá:
> 
> - Como o ASP.NET usa o roteamento para permitem que você use as URLs mais legíveis e pesquisáveis.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Páginas da Web do ASP.NET (Razor) 3
>   
> 
> Este tutorial também funciona com ASP.NET Web Pages 2.


## <a name="about-routing"></a>Sobre roteamento

As URLs para as páginas em seu site podem ter um impacto sobre como o site funciona. Uma URL que &quot;amigável&quot; pode tornar mais fácil para as pessoas a usar o site. Ele também pode ajudar com a otimização do mecanismo de pesquisa (SEO) para o site. Sites do ASP.NET incluem a capacidade de usar URLs amigáveis automaticamente.

ASP.NET permite que você crie URLs significativas que descrevem as ações do usuário em vez de apenas apontando para um arquivo no servidor. Considere estas URLs para um blog fictício:

- `http://www.contoso.com/Blog/blog.cshtml?categories=hardware`
- `http://www.contoso.com//Blog/blog.cshtml?startdate=2009-11-01&enddate=2009-11-30`

Compare essas URLs para os seguintes:

- `http://www.contoso.com/Blog/categories/hardware/`
- `http://www.contoso.com/Blog/2009/November`

O primeiro par, um usuário teria saber que o blog é exibido usando o *blog.cshtml* página e, em seguida, teria que construir uma cadeia de caracteres de consulta que obtém o intervalo de categoria ou datas à direita. O segundo conjunto de exemplos é muito mais fácil de compreender e criar.

As URLs para o primeiro exemplo também apontam diretamente para um arquivo específico (*blog.cshtml*). Se por algum motivo o blog foram movido para outra pasta no servidor, ou se o blog foram reescrito para usar uma página diferente, os links seria errados. O segundo conjunto de URLs não aponta para uma página específica, assim, mesmo que a implementação de blog ou o local é alterado, as URLs ainda seria válidas.

Em páginas de Web do ASP.NET, você pode criar URLs mais amigáveis, como aqueles nos exemplos acima como o ASP.NET usa *roteamento*. Roteamento cria o mapeamento de lógico de uma URL para uma página (ou páginas) que pode atender à solicitação. Como o mapeamento é lógico (não física, em um arquivo específico), o roteamento fornece grande flexibilidade em como definir as URLs para o seu site.

## <a name="how-routing-works"></a>Como funciona o roteamento

Quando o ASP.NET processa uma solicitação, ele lê a URL para determinar como roteá-lo. O ASP.NET tenta corresponder segmentos individuais da URL para arquivos no disco, indo da esquerda para a direita. Se houver uma correspondência, qualquer coisa restante na URL é passada para a página como *informações de caminho*.

Imagine que alguém faz uma solicitação usando esta URL:

`http://www.contoso.com/a/b/c`

A pesquisa funciona assim:

1. Há um arquivo com o caminho e o nome da */a/b/c.cshtml*? Nesse caso, executar nessa página e não passá-lo para nenhuma informação. Caso contrário,...
2. Há um arquivo com o caminho e o nome da */a/b.cshtml*? Se assim, executar nessa página e passar o valor `c` a ele. Caso contrário,...
3. Há um arquivo com o caminho e o nome da */a.cshtml*? Se assim, executar nessa página e passar o valor `b/c` a ele.

Se a pesquisa encontrada exata não corresponde a para *. cshtml* arquivos em suas pastas especificados, o ASP.NET continua procurando esses arquivos por sua vez:

1. */a/b/c/default.cshtml* (nenhuma informação de caminho).
2. */a/b/c/index.cshtml* (nenhuma informação de caminho).

> [!NOTE]
> Para ser honesto, as solicitações para páginas específicas (ou seja, as solicitações que incluem o *. cshtml* extensão de nome de arquivo) funcionará da mesma forma que você esperaria. Uma solicitação como `http://www.contoso.com/a/b.cshtml` executará a página *cshtml* bem.


Dentro de uma página, você pode obter as informações de caminho por meio da página `UrlData` propriedade, que é um dicionário. Imagine que você tenha um arquivo chamado *ViewCustomers.cshtml* e seu site recebe essa solicitação:

`http://mysite.com/myWebSite/ViewCustomers/1000`

Conforme descrito nas regras acima, a solicitação irá para a página. Dentro da página, você pode usar código semelhante ao seguinte para obter e exibir as informações de caminho (neste caso, o valor &quot;1000&quot;):

[!code-html[Main](creating-readable-urls-in-aspnet-web-pages-sites/samples/sample1.html)]

> [!NOTE]
> Como o roteamento não envolve a nomes de arquivo completos, pode haver ambiguidade se você tiver páginas que têm o mesmo nome, mas diferentes extensões de nome de arquivo (por exemplo, *MyPage.cshtml* e *MyPage.html*) . Para evitar problemas com o roteamento, é melhor certificar-se de que você não tem páginas em seu site cujos nomes diferem apenas em sua extensão.


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionais

[O WebMatrix - URLs, UrlData e roteamento para SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO). Essa entrada de blog por Mike Brind fornece alguns detalhes adicionais sobre como funciona o roteamento em páginas da Web do ASP.NET.
