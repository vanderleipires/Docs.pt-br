---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
title: Tratamento de postagens de um controle Popup sem um UpdatePanel (VB) | Microsoft Docs
author: wenz
description: O extensor PopupControl no Kit de ferramentas de controle AJAX oferece uma maneira fácil para disparar um pop-up quando qualquer outro controle está ativado. Quando ocorre um postback no su...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a0b9186c-0912-4fff-916a-6d17e696a50b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: c92083d4a57067c7b646721f21af059fc1e1e7d9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871357"
---
<a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-vb"></a>Tratamento de postagens de um controle Popup sem um UpdatePanel (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3VB.pdf)

> O extensor PopupControl no Kit de ferramentas de controle AJAX oferece uma maneira fácil para disparar um pop-up quando qualquer outro controle está ativado. Quando ocorre um postback em tal um painel e há vários painéis na página é difícil determinar qual painel foi clicado.


## <a name="overview"></a>Visão geral

O extensor PopupControl no Kit de ferramentas de controle AJAX oferece uma maneira fácil para disparar um pop-up quando qualquer outro controle está ativado. Quando ocorre um postback em tal um painel e há vários painéis na página é difícil determinar qual painel foi clicado.

## <a name="steps"></a>Etapas

Ao usar um `PopupControl` com um postback, mas sem ter um `UpdatePanel` na página, o Kit de ferramentas de controle não oferece uma maneira de determinar qual elemento cliente acionou o pop-up que por sua vez causou o postback. No entanto, um pequeno truque fornece uma solução alternativa para esse cenário.

Em primeiro lugar, aqui está a configuração básica: duas caixas de texto que disparam o mesmo pop-up, um calendário. Dois `PopupControlExtenders` reunir caixas de texto e pop-up.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample1.aspx)]

A ideia básica é adicionar um campo de formulário oculto o &lt; `form` &gt; elemento que contém a caixa de texto que iniciou o pop-up:

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample2.aspx)]

Quando a página for carregada, o código JavaScript adiciona um manipulador de eventos para ambas as caixas de texto: sempre que o usuário clica em uma caixa de texto, seu nome será gravado no campo oculto do formulário:

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample3.html)]

No código do lado do servidor, o valor do campo oculto deve ser lidos. Como os campos ocultos do formulário são triviais para manipular, uma abordagem de lista branca para validar o valor de hidden é necessária. Depois que a caixa de texto correto foi identificada, a data do calendário é gravada nele.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample4.aspx)]


[![O calendário será exibido quando o usuário clica na caixa de texto](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image1.png)

O calendário será exibido quando o usuário clica na caixa de texto ([clique para exibir a imagem em tamanho normal](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image3.png))


[![Clicar em uma data coloca na caixa de texto](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image4.png)

Clicar em uma data coloca na caixa de texto ([clique para exibir a imagem em tamanho normal](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Anterior](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)
