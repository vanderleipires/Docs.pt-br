---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
title: Uso de vários controles pop-up (VB) | Microsoft Docs
author: wenz
description: O extensor PopupControl no AJAX Control Toolkit oferece uma maneira fácil de disparar um pop-up quando qualquer outro controle é ativado. Também é possível usar o m...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4da43d77-f6c4-43a8-9124-f1e8e1c8f0a2
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 81b0dbd794d31bc3c55bff4ba8110c590b88b509
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834080"
---
<a name="using-multiple-popup-controls-vb"></a>Uso de vários controles pop-up (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)

> O extensor PopupControl no AJAX Control Toolkit oferece uma maneira fácil de disparar um pop-up quando qualquer outro controle é ativado. Também é possível usar mais de um controle pop-up em uma única página.


## <a name="overview"></a>Visão geral

O extensor PopupControl no AJAX Control Toolkit oferece uma maneira fácil de disparar um pop-up quando qualquer outro controle é ativado. Também é possível usar mais de um controle pop-up em uma única página.

## <a name="steps"></a>Etapas

Para ativar a funcionalidade do AJAX ASP.NET e o Kit de ferramentas de controle, o `ScriptManager` controle deve ser colocada em qualquer lugar na página (mas dentro de `<form>` elemento):

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample1.aspx)]

Em seguida, adicione um painel que serve como o pop-up. No cenário atual, o painel contém um `Calendar` controle. Para evitar as atualizações de página causadas por postagens do calendário, o painel é colocado dentro um `UpdatePanel` controle:

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample2.aspx)]

A página também contém duas caixas de texto. Para cada caixa de texto, o calendário pop-up deverão aparecer depois que a caixa de texto for ativada.

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample3.aspx)]

Agora estender cada uma das duas caixas de texto com um `PopupControlExtender`. O `TargetControlID` atributo fornece a ID do controle ligado ao extensor. O `PopupControlID` atributo contém a ID do painel pop-up. Nesse caso, ambos os extensores mostram o mesmo painel, mas diferentes painéis são possíveis, também.

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample4.aspx)]

Agora sempre que você clica em um campo de texto, um calendário aparece abaixo do campo, permitindo que você selecione uma data. (Voltando a data selecionada nas caixas de texto será abordado em um tutorial diferente.)


[![O calendário é exibido quando o usuário clica na caixa de texto](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)

O calendário é exibido quando o usuário clica na caixa de texto ([clique para exibir a imagem em tamanho normal](using-multiple-popup-controls-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
> [Próximo](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)
