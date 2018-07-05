---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
title: Tratamento de Postbacks de um controle pop-up com um UpdatePanel (VB) | Microsoft Docs
author: wenz
description: O extensor PopupControl no AJAX Control Toolkit oferece uma maneira fácil de disparar um pop-up quando qualquer outro controle é ativado. Tenha cuidado especial a ser executada...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ec9db57c-9f68-402a-bf4c-0d63d5f6908e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: cf17025219fbf24e749c172f2ddb47ec20980cdc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395896"
---
<a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-vb"></a>Tratamento de Postbacks de um controle pop-up com um UpdatePanel (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2VB.pdf)

> O extensor PopupControl no AJAX Control Toolkit oferece uma maneira fácil de disparar um pop-up quando qualquer outro controle é ativado. Tenha cuidado especial a ser executada quando ocorre um postback dentro de tal um pop-up.


## <a name="overview"></a>Visão geral

O extensor PopupControl no AJAX Control Toolkit oferece uma maneira fácil de disparar um pop-up quando qualquer outro controle é ativado. Tenha cuidado especial a ser executada quando ocorre um postback dentro de tal um pop-up.

## <a name="steps"></a>Etapas

Ao usar um `PopupControl` com um postback, um `UpdatePanel` pode evitar a atualização de página causada por postback. A marcação a seguir define dois elementos importantes:

- Um `ScriptManager` controlar de forma que o ASP.NET AJAX Control Toolkit funciona
- Dois `TextBox` controla quais ambas disparará um pop-up
- Um `Panel` controle que servirá como o pop-up
- Dentro do painel, um `Calendar` controle é inserido em um `UpdatePanel` controle
- Dois `PopupControlExtender` controles que atribui o painel para as caixas de texto

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample1.aspx)]

Observe que o `OnSelectionChanged` atributo do `Calendar` controle está definido. Portanto, quando o usuário seleciona uma data no calendário, ocorre um postback e o método do lado do servidor `c1_SelectionChanged()` é executado. Dentro desse método, a data atual deve ser recuperada e Write-back para a caixa de texto.

A sintaxe para o que é da seguinte maneira: em primeiro lugar, um proxy do objeto para o `PopupControlExtender` na página deve ser gerado. O ASP.NET AJAX Control Toolkit oferece o `GetProxyForCurrentPopup()` método. O objeto retorna este método dá suporte a `Commit()` método que envia um valor de volta para o controle que disparou o pop-up (não o controle que disparou a chamada de método!). O código a seguir fornece a data selecionada como o argumento para o `Commit()` método, fazendo com que o código gravar a data selecionada de volta à caixa de texto:

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample2.aspx)]

Agora sempre que você clica em uma data de calendário, a data selecionada aparece na caixa de texto associada, criando um controle de seletor de data que é utilizada em vários sites.


[![O calendário é exibido quando o usuário clica na caixa de texto](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image1.png)

O calendário é exibido quando o usuário clica na caixa de texto ([clique para exibir a imagem em tamanho normal](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image3.png))


[![Clicar em uma data coloca na caixa de texto](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image4.png)

Clicar em uma data coloca na caixa de texto ([clique para exibir a imagem em tamanho normal](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Anterior](using-multiple-popup-controls-vb.md)
> [Próximo](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb.md)
