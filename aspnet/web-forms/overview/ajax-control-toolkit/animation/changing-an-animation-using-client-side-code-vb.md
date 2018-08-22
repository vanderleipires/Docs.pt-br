---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
title: Alterar uma animação usando o código do lado do cliente (VB) | Microsoft Docs
author: wenz
description: O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. A animação também pode...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: a7fe5de5-a964-4780-ae5e-70821dfb50a0
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: bfce413cd45daab62a247d596d9b51cb82cc7f97
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824587"
---
<a name="changing-an-animation-using-client-side-code-vb"></a>Alterar uma animação usando o código do lado do cliente (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11VB.pdf)

> O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. A animação pode ser alterada usando código personalizado de JavaScript do lado do cliente.


## <a name="overview"></a>Visão geral

O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. A animação pode ser alterada usando código personalizado de JavaScript do lado do cliente.

## <a name="steps"></a>Etapas

Em primeiro lugar, inclua o `ScriptManager` na página; em seguida, a biblioteca do AJAX ASP.NET é carregada, tornando possível usar o Kit de ferramentas de controle:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample1.aspx)]

A animação será aplicada a um painel de texto que tem esta aparência:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample2.aspx)]

Na classe CSS associado para o painel, definir uma cor de fundo interessante e também definir uma largura fixa para o painel:

[!code-css[Main](changing-an-animation-using-client-side-code-vb/samples/sample3.css)]

A animação real é iniciada por um botão HTML:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample4.aspx)]

Em seguida, adicione a `AnimationExtender` para a página, fornecendo uma `ID`, o `TargetControlID` atributo e o obrigatoriamente `runat="server"`:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample5.aspx)]

Observe que não há nenhuma `<Animations>` nó dentro de `AnimationExtender` controle. Código JavaScript personalizado é usado para fornecer as animações a serem usados com o controle.

Assim como acontece com a API do servidor de `AnimationExtender`, não há nenhuma maneira fácil para atribuir uma animação para o extensor ainda. No entanto o extensor expõe vários métodos para ler e gravar animações registrado com os vários eventos (`OnClick`, `OnLoad`e assim por diante). Estes são alguns exemplos:

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

O formato do valor de retorno do `get_*()` funções e o formato do argumento para o `set_*()` functions é uma cadeia de caracteres JSON, fornecendo uma representação de objeto do qual seria a marcação XML. Atualmente, não há nenhuma maneira de passar um objeto no, mas é possível ler um objeto de uma determinada animação (`get_OnXXXBehavior()` métodos).

Aqui está uma cadeia de caracteres JSON (sem as aspas de delimitação e formatado legalmente) que representa uma animação disparada pelo botão, mas Animando o painel, redimensioná-la e desaparecimento ao mesmo tempo:

[!code-json[Main](changing-an-animation-using-client-side-code-vb/samples/sample6.json)]

O código JavaScript a seguir atribui esse descripting JSON para o `OnClick` animação do extensor atual e a executa:

[!code-html[Main](changing-an-animation-using-client-side-code-vb/samples/sample7.html)]


[![A animação é executada imediatamente, sem um clique do mouse (e com muito pouco de marcação)](changing-an-animation-using-client-side-code-vb/_static/image2.png)](changing-an-animation-using-client-side-code-vb/_static/image1.png)

A animação é executada imediatamente, sem um clique do mouse (e com muito pouco de marcação) ([clique para exibir a imagem em tamanho normal](changing-an-animation-using-client-side-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](executing-animations-using-client-side-code-vb.md)
> [Próximo](animating-an-updatepanel-control-vb.md)
