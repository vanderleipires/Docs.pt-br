---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
title: Posicionando um ModalPopup (VB) | Microsoft Docs
author: wenz
description: O controle ModalPopup no Kit de ferramentas de controle AJAX oferece uma maneira simples para criar um pop-up modal usando meios do lado do cliente. No entanto o controle não tem um...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 8a07210c-eb0e-485e-9ee8-82a101520e65
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: 2d20888674dfedee7a7af85efd8df118c8394c6c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="positioning-a-modalpopup-vb"></a>Posicionando um ModalPopup (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4VB.pdf)

> O controle ModalPopup no Kit de ferramentas de controle AJAX oferece uma maneira simples para criar um pop-up modal usando meios do lado do cliente. No entanto o controle não oferece uma funcionalidade interna para posicionar o pop-up.


## <a name="overview"></a>Visão geral

O controle ModalPopup no Kit de ferramentas de controle AJAX oferece uma maneira simples para criar um pop-up modal usando meios do lado do cliente. No entanto o controle não oferece uma funcionalidade interna para posicionar o pop-up.

## <a name="steps"></a>Etapas

Para ativar a funcionalidade do ASP.NET AJAX e o Kit de ferramentas de controle, o `ScriptManager`. controle deve ser colocado em qualquer lugar na página (mas dentro do `<form>` elemento):

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample1.aspx)]

Em seguida, adicione um painel que serve como o pop-up modal. Um botão é usado para fechar o janela pop-up:

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample2.aspx)]

Sempre que o pop-up for exibido, ele deve ser posicionado em um determinado local na página. Para esta tarefa, uma função de JavaScript do lado do cliente é criada. Ele primeiro tenta acessar o painel. Se tiver êxito, a posição do painel é definida usando CSS e JavaScript (alterar a posição do popup no será). No entanto, a `ModalPopupExtender` controle também tenta posicionar o pop-up. Portanto, o código JavaScript repetidamente posiciona o pop-up, cada décimo de segundo.

[!code-html[Main](positioning-a-modalpopup-vb/samples/sample3.html)]

Como você pode ver, o valor de retorno de `setTimeout()` método JavaScript é salvo em uma variável global. Isso permite que ao interromper o posicionamento repetidas de pop-up sob demanda, usando o `clearTimeout()` método:

[!code-javascript[Main](positioning-a-modalpopup-vb/samples/sample4.js)]

Agora tudo o que resta para fazer é fazer com que o navegador chamar essas funções sempre que apropriado. O `movePanel()` função JavaScript deve ser chamada quando o botão é clicado que dispara o painel:

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample5.aspx)]

E o `stopMoving()` função entra em jogo quando o pop-up é fechado isso pode ser disparadas no `ModalPopupExtender` controle:

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample6.aspx)]


[![O pop-up modal aparece na posição designada](positioning-a-modalpopup-vb/_static/image2.png)](positioning-a-modalpopup-vb/_static/image1.png)

O pop-up modal aparece na posição designada ([clique para exibir a imagem em tamanho normal](positioning-a-modalpopup-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](handling-postbacks-from-a-modalpopup-vb.md)
