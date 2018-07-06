---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-vb
title: Recolher e expandir um painel de JavaScript (VB) | Microsoft Docs
author: wenz
description: O controle de CollapsiblePanel no ASP.NET AJAX Control Toolkit estende um painel e fornece-a com a capacidade de seu conteúdo de recolher e expandi-lo um...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 298789b4-2964-49f5-a0a8-d4dbeb9ff2c2
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-vb
msc.type: authoredcontent
ms.openlocfilehash: da1bc6958dba99ffc5ef54fbfbc003bb26050495
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812499"
---
<a name="collapsing-and-expanding-a-panel-from-javascript-vb"></a>Recolher e expandir um painel de JavaScript (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1VB.pdf)

> O controle CollapsiblePanel no ASP.NET AJAX Control Toolkit estende um painel e fornece-o com a capacidade de seu conteúdo de recolher e expandi-la novamente. Essas duas ações também podem ser disparadas do código JavaScript personalizado.


## <a name="overview"></a>Visão geral

O controle CollapsiblePanel no ASP.NET AJAX Control Toolkit estende um painel e fornece-o com a capacidade de seu conteúdo de recolher e expandi-la novamente. Essas duas ações também podem ser disparadas do código JavaScript personalizado.

## <a name="steps"></a>Etapas

Em primeiro lugar, crie uma nova página ASP.NET e inclua o `ScriptManager` dentro da `<form>` elemento. Isso carrega a biblioteca AJAX do ASP.NET que é exigida pelo Kit de ferramentas de controle:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample1.aspx)]

Em seguida, crie um painel com algum texto para que o efeito de expandir/recolher pode ser visto:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample2.aspx)]

Como você pode ver, o painel faz referência a uma classe CSS que é mostrada aqui (e, basicamente, define uma cor de fundo e a largura do painel):

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample3.css)]

O `CollapsiblePanelExtender` controle requer o `TargetControlID` para que o Kit de ferramentas saiba qual painel para recolher ou expandir mediante solicitação do atributo:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample4.aspx)]

Infelizmente, o extensor no momento, não expõe uma API específica para recolher ou expandir o painel, mas farão alguns métodos não documentados. Em primeiro lugar, adicione três botões HTML para a página que, em seguida, irá disparar o JavaScript do lado do cliente para recolher ou expandir o conteúdo do painel:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample5.aspx)]

No código de JavaScript do lado do cliente (Introdução ao `<script type="text/javascript">`), o `$find()` método precisa ser usado para acessar o `CollapsiblePanelExtender`. `$find("cpe")` retornará uma referência a ele. A partir daí, os métodos específicos resolverá a tarefa em questão.

O método para abrir (expandir) o painel é chamado `_doOpen()`; o código a seguir implementa o `doOpen()` função é chamada quando o primeiro botão é clicado:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample6.js)]

Para fechar ou recolher o painel, o `_doClose()` método precisa ser executado. Portanto, quando o usuário clica no segundo botão, o seguinte código JavaScript é chamado:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample7.js)]

O terceiro botão alterna o estado do painel: de recolhidos para expandido e vice-versa. O `CollapsiblePanelExtender` expõe o `toggle()` método que faz exatamente isso: reverte o estado do painel. No entanto há também outra abordagem (que é usado internamente pelo `toggle()` método): O `get_Collapsed()` método do `CollapsiblePanelExtender()` nos informa se o painel é recolhido ou não. Dependendo do valor retornado dessa função, o painel é, em seguida, seja expandido (`_doOpen()` método) ou recolhido (`_doClose()`) método:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample8.js)]


[![O terceiro botão altera o estado do painel: de recolhidos para voltar e expandido](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image1.png)

O terceiro botão altera o estado do painel: de recolhidos para voltar e expandido ([clique para exibir a imagem em tamanho normal](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](collapsing-and-expanding-a-panel-from-javascript-cs.md)
