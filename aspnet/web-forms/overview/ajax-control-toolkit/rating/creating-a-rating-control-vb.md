---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
title: Criando um controle de classificação (VB) | Microsoft Docs
author: wenz
description: Muitos sites, de comércio eletrônico para sites de comunidades, oferecem seus usuários para artigos de taxa ou itens. Isso geralmente exige algum esforço de codificação, mas é necessário o...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 6d0d70f4-725e-4258-8ae8-24a6ba1ddbf7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
msc.type: authoredcontent
ms.openlocfilehash: c19f36dfe1b72a3954db61ff1845e99c02e47c14
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868406"
---
<a name="creating-a-rating-control-vb"></a>Criando um controle de classificação (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0VB.pdf)

> Muitos sites, de comércio eletrônico para sites de comunidades, oferecem seus usuários para artigos de taxa ou itens. Isso geralmente exige algum esforço de codificação, mas temos o Kit de ferramentas de controle para nossa disposição.


## <a name="overview"></a>Visão geral

Muitos sites, de comércio eletrônico para sites de comunidades, oferecem seus usuários para artigos de taxa ou itens. Isso geralmente exige algum esforço de codificação, mas temos o Kit de ferramentas de controle para nossa disposição.

## <a name="steps"></a>Etapas

Em primeiro lugar, você precisa (pelo menos) dois tipos de imagens: uma para um preenchido item de classificação, e um para um item de classificação vazio. Um item de classificação geralmente é uma estrela ou um feliz. Para este cenário, você encontrar três arquivos, smiley.png e empty.png e done.png feliz como parte dos downloads de código de origem para este tutorial.

Em seguida, crie um novo arquivo do ASP.NET e comece a adicionar um `ScriptManager` controle a ela:

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample1.aspx)]

Em seguida, adicione o `Rating` controle do Kit de ferramentas de controle AJAX ASP.NET. Os atributos a seguir precisam ser definidas para este exemplo:

- `CurrentRating` a classificação inicial a ser usado
- `MaxRating` a classificação máxima
- `EmptyStarCssClass` a classe CSS a ser usado quando um item de classificação (estrela) está vazio
- `FilledStarCssClass` a classe CSS a ser usado quando um item de classificação (estrela) é preenchido
- `StarCssClass` a classe CSS a ser usado para um estado visível
- `WaitingStarCssClass` a classe CSS usar enquanto uma classificação por estrelas é enviada de volta ao servidor

E aqui está a marcação que cria um controle de classificação com cinco itens (smileys) do qual nenhum é preenchido inicialmente:

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample2.aspx)]

As três classes CSS referenciadas agora precisam mostrar os arquivos de imagem apropriada, o que é fácil de fazer uso de CSS:

[!code-css[Main](creating-a-rating-control-vb/samples/sample3.css)]

Certifique-se de que você fornecer a largura e altura das três imagens, caso contrário, a exibição pode parecer um pouco bagunçada backup.

Por fim, o resultado da classificação deve ser exibido para o usuário (ou, pelo menos salvo em um banco de dados). Para adicionar um rótulo para a saída de uma mensagem de texto e um botão de envio de enviar novamente o formulário de classificação para o servidor:

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample4.aspx)]

No código do lado do servidor, acessar o controle de classificação por meio de seu `ID` e acessar seu `CurrentRating` propriedade que é o número de itens de classificação selecionada, em nosso exemplo, um valor entre 0 e 5.

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample5.aspx)]

Salve a página e carregá-lo em seu navegador. Quando você focaliza os itens de classificação (inicialmente vazio), ocorre um efeito de JavaScript: as alterações de classificação. Quando você clica no conjunto de estrelas, a classificação atual é mantida. Finalmente, quando você enviar o formulário, o código do lado do servidor gera a classificação selecionada.


[![Criando um sistema de classificação com o mínimo de código](creating-a-rating-control-vb/_static/image2.png)](creating-a-rating-control-vb/_static/image1.png)

Criando um sistema de classificação com o mínimo de código ([clique para exibir a imagem em tamanho normal](creating-a-rating-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](creating-a-rating-control-cs.md)
