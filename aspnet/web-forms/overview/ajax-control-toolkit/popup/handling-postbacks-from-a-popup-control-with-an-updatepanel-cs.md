---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
title: Tratamento de postagens de um controle pop-up com um UpdatePanel (c#) | Microsoft Docs
author: wenz
description: O extensor PopupControl no Kit de ferramentas de controle AJAX oferece uma maneira fácil para disparar um pop-up quando qualquer outro controle está ativado. Tenha cuidado especial a ser executada...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 1f68f59d-9c1e-4cf3-b304-c13ae6b7203e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: abedb5247f710b02752651a7bfb011ab63d32844
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879628"
---
<a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-c"></a>Tratamento de postagens de um controle pop-up com um UpdatePanel (c#)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2CS.pdf)

> O extensor PopupControl no Kit de ferramentas de controle AJAX oferece uma maneira fácil para disparar um pop-up quando qualquer outro controle está ativado. Cuidado especial precisa ser executada quando ocorre um postback dentro de tal um pop-up.


## <a name="overview"></a>Visão Geral

O extensor PopupControl no Kit de ferramentas de controle AJAX oferece uma maneira fácil para disparar um pop-up quando qualquer outro controle está ativado. Cuidado especial precisa ser executada quando ocorre um postback dentro de tal um pop-up.

## <a name="steps"></a>Etapas

Ao usar um `PopupControl` com um postback, um `UpdatePanel` pode impedir que a atualização de página causada por postback. A marcação a seguir define alguns dos elementos importantes:

- Um `ScriptManager` controlar de forma que o ASP.NET AJAX Control Toolkit funciona
- Dois `TextBox` controla quais ambos dispararão um pop-up
- Um `Panel` controle que servirá como o pop-up
- No painel de um `Calendar` controle é inserido em um `UpdatePanel` controle
- Dois `PopupControlExtender` controles que atribui o painel para as caixas de texto

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample1.aspx)]

Observe que o `OnSelectionChanged` atributo o `Calendar` controle está definido. Portanto, quando o usuário seleciona uma data no calendário, ocorre um postback e o método do lado do servidor `c1_SelectionChanged()` é executado. Dentro desse método, a data atual deve ser recuperada e gravada de volta para a caixa de texto.

A sintaxe para o que é o seguinte: primeiro, um proxy do objeto para o `PopupControlExtender` na página deve ser gerado. O Kit de ferramentas de controle do ASP.NET AJAX oferece o `GetProxyForCurrentPopup()` método. O objeto retorna este método oferece suporte a `Commit()` método que envia um valor para o controle que disparou o pop-up (não o controle que disparou a chamada do método!). O código a seguir fornece a data selecionada como o argumento para o `Commit()` método, fazendo com que o código para gravar as datas selecionadas de volta à caixa de texto:

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample2.aspx)]

Agora sempre que você clicar em uma data do calendário, a data selecionada aparece na caixa de texto associada, criando um controle de seletor de data que é utilizada em vários sites.


[![O calendário será exibido quando o usuário clica na caixa de texto](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image1.png)

O calendário será exibido quando o usuário clica na caixa de texto ([clique para exibir a imagem em tamanho normal](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image3.png))


[![Clicar em uma data coloca na caixa de texto](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image4.png)

Clicar em uma data coloca na caixa de texto ([clique para exibir a imagem em tamanho normal](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Anterior](using-multiple-popup-controls-cs.md)
> [Próximo](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
