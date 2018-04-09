---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
title: Tratamento de postagens de um ModalPopup (VB) | Microsoft Docs
author: wenz
description: O controle ModalPopup no Kit de ferramentas de controle AJAX oferece uma maneira simples para criar um pop-up modal usando meios do lado do cliente. Deve ter especial cuidado quando um pos...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: f70ac2b3-900f-40fa-858f-ab057904506b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: 1aa2c42deb67015dd0b35edf4ba72d8d667ec88c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="handling-postbacks-from-a-modalpopup-vb"></a>Postagens de tratamento de um ModalPopup (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3VB.pdf)

> O controle ModalPopup no Kit de ferramentas de controle AJAX oferece uma maneira simples para criar um pop-up modal usando meios do lado do cliente. Cuidado cuidado especial quando um postback é criado de dentro do popup.


## <a name="overview"></a>Visão geral

O controle ModalPopup no Kit de ferramentas de controle AJAX oferece uma maneira simples para criar um pop-up modal usando meios do lado do cliente. Cuidado cuidado especial quando um postback é criado de dentro do popup.

## <a name="steps"></a>Etapas

Para ativar a funcionalidade do ASP.NET AJAX e o Kit de ferramentas de controle, o `ScriptManager` controle deve ser colocado em qualquer lugar na página (mas dentro do `<form>` elemento):

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample1.aspx)]

Em seguida, adicione um painel que serve como o pop-up modal. Lá, o usuário pode inserir um nome e um endereço de email. Um botão é usado para fechar o janela pop-up e salvar as informações. Observe que o `OnClick` atributo está definido para que um postback ocorre quando este botão é clicado:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample2.aspx)]

A página propriamente dita consiste em dois rótulos para exatamente as mesmas informações: nome e endereço de email. Um botão é usado para disparar o pop-up modal:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample3.aspx)]

Para fazer com que o pop-up aparecer, adicione o `ModalPopupExtender` controle. Definir o `PopupControlID` a ID do painel atributo e `TargetControlID` a ID do botão:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample4.aspx)]

Agora sempre que o `Save` botão dentro do popup modal é clicado, o lado do servidor `SaveData()` o método é executado. Lá, você pode salvar os dados inseridos em um repositório de dados. Para simplificar, os novos dados é apenas de saída no rótulo:

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample5.vb)]

Além disso, os controles de caixa de texto dentro do popup modal devem ser preenchidos com o nome atual e o email. No entanto isso só é necessário quando nenhum postback ocorre. Se houver um postback, o recurso de viewstate do ASP.NET preencherá automaticamente as caixas de texto com os valores apropriados.

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample6.vb)]


[![O pop-up modal causa um postback](handling-postbacks-from-a-modalpopup-vb/_static/image2.png)](handling-postbacks-from-a-modalpopup-vb/_static/image1.png)

O pop-up modal causa um postback ([clique para exibir a imagem em tamanho normal](handling-postbacks-from-a-modalpopup-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](using-modalpopup-with-a-repeater-control-vb.md)
> [Próximo](positioning-a-modalpopup-vb.md)
