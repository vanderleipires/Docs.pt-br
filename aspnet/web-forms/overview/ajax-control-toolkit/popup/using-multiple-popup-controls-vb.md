---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
title: Usando vários controles de pop-up (VB) | Microsoft Docs
author: wenz
description: O extensor PopupControl no Kit de ferramentas de controle AJAX oferece uma maneira fácil para disparar um pop-up quando qualquer outro controle está ativado. Também é possível usar m...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4da43d77-f6c4-43a8-9124-f1e8e1c8f0a2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 7c57aab3ecf2c02a8488b5ea4e3e0ed33ac5e7fe
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869316"
---
<a name="using-multiple-popup-controls-vb"></a>Usando vários controles de pop-up (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)

> O extensor PopupControl no Kit de ferramentas de controle AJAX oferece uma maneira fácil para disparar um pop-up quando qualquer outro controle está ativado. Também é possível usar mais de um controle de pop-up em uma única página.


## <a name="overview"></a>Visão geral

O extensor PopupControl no Kit de ferramentas de controle AJAX oferece uma maneira fácil para disparar um pop-up quando qualquer outro controle está ativado. Também é possível usar mais de um controle de pop-up em uma única página.

## <a name="steps"></a>Etapas

Para ativar a funcionalidade do ASP.NET AJAX e o Kit de ferramentas de controle, o `ScriptManager` controle deve ser colocado em qualquer lugar na página (mas dentro do `<form>` elemento):

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample1.aspx)]

Em seguida, adicione um painel que serve como o pop-up. No cenário atual, o painel contém um `Calendar` controle. Para evitar as atualizações de página causadas por postagens do calendário, o painel é colocado dentro de um `UpdatePanel` controle:

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample2.aspx)]

A página também contém duas caixas de texto. Para cada caixa de texto, pop-up do calendário deve aparecer depois que a caixa de texto está ativada.

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample3.aspx)]

Agora estender cada uma das duas caixas de texto com um `PopupControlExtender`. O `TargetControlID` atributo fornece a ID do controle associado para o extensor. O `PopupControlID` atributo contém a ID do painel pop-up. Nesse caso, ambos os extensores mostram o mesmo painel, mas diferentes painéis são possíveis, também.

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample4.aspx)]

Agora sempre que você clicar em um campo de texto, um calendário aparece abaixo do campo, permitindo que você selecione uma data. (Voltar a data selecionada nas caixas de texto será abordado em um tutorial diferente.)


[![O calendário será exibido quando o usuário clica na caixa de texto](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)

O calendário será exibido quando o usuário clica na caixa de texto ([clique para exibir a imagem em tamanho normal](using-multiple-popup-controls-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
> [Próximo](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)
