---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
title: Abrindo uma janela pop-up Modal do código do servidor (VB) | Microsoft Docs
author: wenz
description: O controle ModalPopup no AJAX Control Toolkit oferece uma maneira simples de criar um pop-up modal usando meios do lado do cliente. No entanto, alguns cenários exigem que o...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 36ca81d7-906d-4db2-952b-add18a4ff421
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 52eec262a9241aa7b11cbb892bdf2142c7a2b83a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822762"
---
<a name="launching-a-modal-popup-window-from-server-code-vb"></a>Abrindo uma janela pop-up Modal do código do servidor (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1VB.pdf)

> O controle ModalPopup no AJAX Control Toolkit oferece uma maneira simples de criar um pop-up modal usando meios do lado do cliente. No entanto, alguns cenários exigem que a abertura do popup modal é acionada no lado do servidor.


## <a name="overview"></a>Visão geral

O controle ModalPopup no AJAX Control Toolkit oferece uma maneira simples de criar um pop-up modal usando meios do lado do cliente. No entanto, alguns cenários exigem que a abertura do popup modal é acionada no lado do servidor.

## <a name="steps"></a>Etapas

Em primeiro lugar, um controle de botão do ASP.NET web é necessário para demonstrar como funciona o controle ModalPopup. Adicionar este botão dentro de &lt;formulário&gt; elemento em uma nova página:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample1.aspx)]

Em seguida, você precisa a marcação para o pop-up que você deseja criar. Defina-o como um `<asp:Panel>` controlar e certifique-se de que ele inclui um controle de botão. O controle ModalPopup oferece a funcionalidade para que tal um botão fechar o pop-up; Caso contrário, não há nenhuma maneira fácil para permitir que ele desaparecer.

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample2.aspx)]

Em seguida adicione o controle ModalPopup do ASP.NET AJAX Toolkit para a página. Definir propriedades para o botão que carrega o controle, o botão que o torna desaparecem e a ID do popup real.

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample3.aspx)]

Assim como acontece com todas as páginas da web com base no ASP.NET AJAX; o Gerenciador de scripts é necessária para carregar as bibliotecas JavaScript necessárias para os navegadores de destino diferente:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample4.aspx)]

Execute o exemplo no navegador. Quando você clica no botão, o popup modal é exibida. Para obter o mesmo efeito usando código do lado do servidor, é necessário um novo botão:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample5.aspx)]

Como você pode ver, um clique no botão gera um postback e executa o `ServerButton_Click()` método no servidor. Esse método, uma função JavaScript chamada `launchModal()` é executada para ser mais exato, a função de JavaScript será executada depois que a página foi carregada:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample6.aspx)]

O trabalho de `launchModal()` é exibir o ModalPopup. O `launchModal()` função é executada depois que a página HTML completa foi carregada. Nesse momento, no entanto, a estrutura ASP.NET AJAX não foi totalmente carregada ainda. Portanto, o `launchModal()` função simplesmente define uma variável que o controle ModalPopup deve ser mostrado posteriormente:

[!code-html[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample7.html)]

O `pageLoad()` função JavaScript é uma função especial que é executada depois que o ASP.NET AJAX foi totalmente carregado. Portanto, podemos adicionar código para essa função para mostrar o controle ModalPopup, mas somente se `launchModal()` foi chamado antes de:

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample8.js)]

O `$find()` está procurando por um elemento nomeado na página de função e espera que a ID do lado do servidor como um parâmetro. Portanto, `$find("mpe")` retorna a representação de cliente do controle ModalPopup; sua `show()` método permite que o pop-up aparecer.


[![O popup modal é exibida quando qualquer um dos botões é clicado](launching-a-modal-popup-window-from-server-code-vb/_static/image2.png)](launching-a-modal-popup-window-from-server-code-vb/_static/image1.png)

O popup modal é exibida quando qualquer um dos botões é clicado ([clique para exibir a imagem em tamanho normal](launching-a-modal-popup-window-from-server-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](positioning-a-modalpopup-cs.md)
> [Próximo](using-modalpopup-with-a-repeater-control-vb.md)
