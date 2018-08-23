---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
title: Criação de um controle classificação (c#) | Microsoft Docs
author: wenz
description: Muitos sites, de comércio eletrônico para sites de comunidades, oferecem seus usuários para artigos de taxa ou itens. Isso geralmente exige algum esforço de codificação, mas temos o...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 969fb28f-2bff-4fc4-b24a-27f5e2534a37
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 5776101d9e40f999806aefa5a9529dbef21af161
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831013"
---
<a name="creating-a-rating-control-c"></a>Criação de um controle classificação (c#)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0CS.pdf)

> Muitos sites, de comércio eletrônico para sites de comunidades, oferecem seus usuários para artigos de taxa ou itens. Isso geralmente exige algum esforço de codificação, mas temos o Kit de ferramentas de controle à nossa disposição.


## <a name="overview"></a>Visão geral

Muitos sites, de comércio eletrônico para sites de comunidades, oferecem seus usuários para artigos de taxa ou itens. Isso geralmente exige algum esforço de codificação, mas temos o Kit de ferramentas de controle à nossa disposição.

## <a name="steps"></a>Etapas

Em primeiro lugar, você precisa (pelo menos) dois tipos de imagens: uma para um preenchido-out do item de classificação e um para um item de classificação vazia. Um item de classificação é normalmente uma estrela ou um sorriso. Para este cenário, você encontra três arquivos, smiley.png e empty.png e sorriso done.png como parte dos downloads de código de origem para este tutorial.

Em seguida, crie um novo arquivo do ASP.NET e comece a adicionar um `ScriptManager` controle a ele:

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample1.aspx)]

Em seguida, adicione o `Rating` controle do ASP.NET AJAX Control Toolkit. Os atributos a seguir precisam ser definidas para este exemplo:

- `CurrentRating` a classificação inicial a ser usado
- `MaxRating` a classificação máxima
- `EmptyStarCssClass` a classe CSS a ser usado quando um item de classificação (star) está vazio
- `FilledStarCssClass` a classe CSS a ser usado quando um item de classificação (star) é preenchido
- `StarCssClass` a classe CSS a ser usado para um estado visível
- `WaitingStarCssClass` a classe CSS a usar enquanto uma classificação por estrelas é enviada de volta ao servidor

E aqui está a marcação que cria um controle de classificação com cinco itens (smileys) de que nenhum é preenchido inicialmente:

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample2.aspx)]

As três classes CSS referenciadas agora precisam mostrar os arquivos de imagem apropriada, que é uma tarefa fácil usando CSS:

[!code-css[Main](creating-a-rating-control-cs/samples/sample3.css)]

Certifique-se de que você forneça a largura e altura de três imagens, caso contrário, a exibição pode parecer um pouco bagunçada backup.

Por fim, o resultado da avaliação deve ser exibido para o usuário (ou, pelo menos é salvo em um banco de dados). Portanto, adicione um rótulo para a saída de uma mensagem de texto e um botão de envio de enviar novamente o formulário de classificação para o servidor:

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample4.aspx)]

No código do lado do servidor, acessar o controle de classificação por meio de seu `ID` e, em seguida, acessar seu `CurrentRating` propriedade que é o número de itens de classificação selecionada, em nosso exemplo, um valor entre 0 e 5.

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample5.aspx)]

Salve a página e carregá-lo em seu navegador. Quando você focaliza os itens de classificação (inicialmente vazio), um efeito de JavaScript ocorre: as alterações de classificação. Quando você clica no conjunto de estrelas, a classificação atual é mantida. Por fim, quando você envia o formulário, o código do lado do servidor gera a classificação selecionada.


[![Criando um sistema de classificação com o mínimo de código](creating-a-rating-control-cs/_static/image2.png)](creating-a-rating-control-cs/_static/image1.png)

Criando um sistema de classificação com o mínimo de código ([clique para exibir a imagem em tamanho normal](creating-a-rating-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Avançar](creating-a-rating-control-vb.md)
