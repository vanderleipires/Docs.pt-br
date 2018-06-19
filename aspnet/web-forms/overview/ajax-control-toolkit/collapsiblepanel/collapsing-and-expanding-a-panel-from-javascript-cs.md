---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
title: Recolher e expandir um painel de JavaScript (c#) | Microsoft Docs
author: wenz
description: O controle de CollapsiblePanel no ASP.NET AJAX Control Toolkit estende um painel e fornece a capacidade de seu conteúdo de recolher e expandi-lo um...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: de5500be-75e5-461a-8064-b70ae52ea6a4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: 7baa3be7144946bde7d11afd9b1cb5f14ad9dede
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875533"
---
<a name="collapsing-and-expanding-a-panel-from-javascript-c"></a>Recolher e expandir um painel de JavaScript (c#)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1CS.pdf)

> O controle CollapsiblePanel no ASP.NET AJAX Control Toolkit estende um painel e fornece a capacidade de seu conteúdo de recolher e expandi-lo novamente. Essas duas ações também podem ser disparadas de código JavaScript personalizado.


## <a name="overview"></a>Visão geral

O controle CollapsiblePanel no ASP.NET AJAX Control Toolkit estende um painel e fornece a capacidade de seu conteúdo de recolher e expandi-lo novamente. Essas duas ações também podem ser disparadas de código JavaScript personalizado.

## <a name="steps"></a>Etapas

Primeiro, crie uma nova página ASP.NET e incluem o `ScriptManager` dentro de um `<form>` elemento. Isso carrega a biblioteca ASP.NET AJAX, que é necessária para o Kit de ferramentas:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample1.aspx)]

Em seguida, crie um painel com algum texto para que o efeito de recolher/expandir pode ser visto:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample2.aspx)]

Como você pode ver, o painel faz referência a uma classe CSS que é mostrada aqui (e, basicamente, define uma cor de plano de fundo e a largura do painel):

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample3.css)]

O `CollapsiblePanelExtender` controle requer o `TargetControlID` atributo para que o Kit de ferramentas saiba qual painel para recolher ou expandir mediante solicitação:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample4.aspx)]

Infelizmente, o extensor atualmente não expõe uma API específica para recolher ou expandir o painel, mas farão alguns métodos não documentados. Em primeiro lugar, adicione três botões HTML para a página que, em seguida, irá disparar o JavaScript do lado do cliente para recolher ou expandir o conteúdo do painel:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample5.aspx)]

No código JavaScript do lado do cliente (Introdução ao `<script type="text/javascript">`), o `$find()` método precisa ser usada para acessar o `CollapsiblePanelExtender`. `$find("cpe")` Retorna uma referência a ele. A partir daí, os métodos específicos resolverá a tarefa em questão.

O método de abertura (expandir) o painel é chamado `_doOpen()`; o código a seguir implementa a `doOpen()` função é chamada quando o primeiro botão é clicado:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample6.js)]

Para fechar ou recolher o painel, o `_doClose()` método precisa ser executado. Quando o usuário clica no segundo botão, o seguinte código JavaScript é chamado:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample7.js)]

O terceiro botão alterna o estado do painel: de recolhido expandida e vice-versa. O `CollapsiblePanelExtender` expõe o `toggle()` método que faz exatamente isso: inverte o estado do painel. No entanto também há outra abordagem (que é usado internamente pelo `toggle()` método): O `get_Collapsed()` método do `CollapsiblePanelExtender()` informa o painel é recolhido. Dependendo do valor de retorno dessa função, o painel é, em seguida, seja expandido (`_doOpen()` método) ou recolhido (`_doClose()`) método:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample8.js)]


[![O terceiro botão altera o estado do painel: de recolhido expandido e](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image1.png)

O terceiro botão altera o estado do painel: de recolhido expandido e ([clique para exibir a imagem em tamanho normal](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Avançar](collapsing-and-expanding-a-panel-from-javascript-vb.md)
