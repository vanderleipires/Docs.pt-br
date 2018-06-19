---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-vb
title: Adicionando conteúdo dinâmico para uma página em cache (VB) | Microsoft Docs
author: microsoft
description: Saiba como combinar o conteúdo dinâmico e armazenados em cache na mesma página. Substituição post-cache permite que você exiba o conteúdo dinâmico, como o de anúncios de banner...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 68acd884-fb57-4486-a1be-aaa93e380780
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 89421b4bec2170e408ded87ccc918a7a16844a98
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879758"
---
<a name="adding-dynamic-content-to-a-cached-page-vb"></a>Adicionando conteúdo dinâmico para uma página em cache (VB)
====================
por [Microsoft](https://github.com/microsoft)

> Saiba como combinar o conteúdo dinâmico e armazenados em cache na mesma página. Substituição post-cache permite que você exiba o conteúdo dinâmico, como anúncios de banner ou itens de notícias, dentro de uma página que tenha sido saída armazenado em cache.


Aproveitando o cache de saída, você pode melhorar drasticamente o desempenho de um aplicativo ASP.NET MVC. Em vez de gerar novamente uma página de cada vez que a página é solicitada, a página pode ser gerada uma vez e armazenado em cache na memória para vários usuários.

Mas há um problema. Se você precisa exibir o conteúdo dinâmico na página? Por exemplo, imagine que você deseja exibir um anúncio de faixa na página. Você não deseja que o anúncio de cabeçalho a ser armazenado em cache para que cada usuário vê o anúncio do mesmo. Você não faz qualquer dinheiro dessa forma!

Felizmente, há uma solução fácil. Você pode tirar proveito de um recurso da estrutura do ASP.NET chamado *pós-cache*. Substituição post-cache permite que você substitua o conteúdo dinâmico em uma página que foi armazenado em cache na memória.


Normalmente, quando você saída cache uma página usando o &lt;OutputCache&gt; atributo, a página é armazenada em cache no servidor e o cliente (navegador da web). Quando você usar substituição post-cache, uma página é armazenada em cache somente no servidor.


#### <a name="using-post-cache-substitution"></a>Usar substituição POST-Cache

Usar substituição post-cache requer duas etapas. Primeiro, você precisa definir um método que retorna uma cadeia de caracteres que representa o conteúdo dinâmico que você deseja exibir na página em cache. Em seguida, você chamar o método HttpResponse.WriteSubstitution() para inserir o conteúdo dinâmico na página.

Por exemplo, imagine que você deseja exibir itens de notícias diferentes aleatoriamente em uma página em cache. A classe na listagem 1 expõe um método único, chamado RenderNews(), que retorna aleatoriamente um item de notícias em uma lista de três itens de notícias.

**Listando 1 – Models\News.vb**

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample1.vb)]

Para tirar proveito de substituição post-cache, você chamar o método HttpResponse.WriteSubstitution(). O método WriteSubstitution() define o código para substituir uma região de página em cache com conteúdo dinâmico. O método WriteSubstitution() é usado para exibir o item de notícias aleatório no modo de exibição na listagem 2.

**A listagem 2 – Views\Home\Index.aspx**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample2.aspx)]

O método RenderNews é passado para o método WriteSubstitution(). Observe que o método RenderNews não é chamado. Em vez disso, uma referência para o método é passada para WriteSubstitution() com a Ajuda do operador AddressOf.

O modo de exibição do índice é armazenado em cache. O modo de exibição é retornado pelo controlador na listagem 3. Observe que a ação Index () é decorada com um &lt;OutputCache&gt; atributo que faz com que a exibição do índice a ser armazenado em cache por 60 segundos.

**A listagem 3 – Controllers\HomeController.vb**

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample3.vb)]

Embora o modo de exibição do índice é armazenado em cache, itens de notícias aleatórios diferentes são exibidos quando você solicitar a página de índice. Quando você solicitar a página de índice, não altera a hora exibida pela página por 60 segundos (consulte a Figura 1). O fato de que não altera a hora comprova que a página é armazenada em cache. No entanto, o conteúdo é injetado pelas alterações de método – o item de notícias aleatório – WriteSubstitution() com cada solicitação.

**Figura 1 – injeção de itens de notícias dinâmico em uma página em cache**

![clip_image002](adding-dynamic-content-to-a-cached-page-vb/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a>Usar substituição POST-Cache em métodos auxiliares

É uma maneira mais fácil para tirar proveito de substituição post-cache encapsular a chamada ao método WriteSubstitution() dentro de um método auxiliar personalizado. Essa abordagem é ilustrada pelo método auxiliar na listagem 4.

**A listagem 4 – Helpers\AdHelper.vb**

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample4.vb)]

A listagem 4 contém um módulo do Visual Basic que expõe dois métodos: RenderBanner() e RenderBannerInternal(). O método RenderBanner() representa o método auxiliar real. Este método estende a classe HtmlHelper do ASP.NET MVC padrão, para que você possa chamar Html.RenderBanner() em uma exibição, assim como qualquer outro método auxiliar.

O método RenderBanner() chama o método de HttpResponse.WriteSubstitution() passando o método RenderBannerInternal() para o método WriteSubsitution().

O método RenderBannerInternal() é um método privado. Esse método não ser exposto como um método auxiliar. O método RenderBannerInternal() retorna aleatoriamente uma imagem de anúncio de faixa de uma lista de três imagens de anúncio de faixa.

O modo de exibição do índice modificado na listagem 5 ilustra como você pode usar o método auxiliar RenderBanner(). Observe que adicional &lt;% @ importação %&gt; diretiva está incluída na parte superior do modo de exibição para importar o namespace MvcApplication1.Helpers. Se você não importar esse namespace, o método RenderBanner() não aparecerá como um método na propriedade de Html.

**Listando 5 – Views\Home\Index.aspx (com o método RenderBanner())**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample5.aspx)]

Quando você solicitar a página renderizada pela exibição na listagem 5, uma faixa diferentes de anúncio é exibido com cada solicitação (consulte a Figura 2). A página é armazenada em cache, mas o anúncio de faixa é injetado dinamicamente pelo método auxiliar RenderBanner().

**Figura 2 – o modo de exibição do índice exibindo um anúncio de faixa aleatória**

![clip_image004](adding-dynamic-content-to-a-cached-page-vb/_static/image2.jpg)

#### <a name="summary"></a>Resumo

Este tutorial explica como você pode atualizar dinamicamente o conteúdo em uma página em cache. Você aprendeu a usar o método HttpResponse.WriteSubstitution() para habilitar o conteúdo dinâmico a ser inserida em uma página em cache. Você também aprendeu como encapsulam a chamada ao método WriteSubstitution() dentro de um método auxiliar HTML.

Tirar proveito do armazenamento em cache sempre que possível – ele pode ter um impacto significativo sobre o desempenho de seus aplicativos web. Conforme explicado neste tutorial, você pode tirar proveito do armazenamento em cache mesmo quando você precisa exibir conteúdo dinâmico em suas páginas.

> [!div class="step-by-step"]
> [Anterior](improving-performance-with-output-caching-vb.md)
> [Próximo](creating-a-controller-vb.md)
