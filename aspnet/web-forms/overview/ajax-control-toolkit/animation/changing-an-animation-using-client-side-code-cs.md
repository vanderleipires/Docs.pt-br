---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
title: Alterar uma animação usando o código do lado do cliente (c#) | Microsoft Docs
author: wenz
description: O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. A animação também pode...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 2bfbc5cc-f942-44b7-a62d-a29520f1da9a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 9711e93ee6f119ec1825e32bdad64535435970c9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808950"
---
<a name="changing-an-animation-using-client-side-code-c"></a>Alterar uma animação usando o código do lado do cliente (c#)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)

> O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. A animação pode ser alterada usando código personalizado de JavaScript do lado do cliente.


## <a name="overview"></a>Visão geral

O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. A animação pode ser alterada usando código personalizado de JavaScript do lado do cliente.

## <a name="steps"></a>Etapas

Em primeiro lugar, inclua o `ScriptManager` na página; em seguida, a biblioteca do AJAX ASP.NET é carregada, tornando possível usar o Kit de ferramentas de controle:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample1.aspx)]

A animação será aplicada a um painel de texto que tem esta aparência:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample2.aspx)]

Na classe CSS associado para o painel, definir uma cor de fundo interessante e também definir uma largura fixa para o painel:

[!code-css[Main](changing-an-animation-using-client-side-code-cs/samples/sample3.css)]

A animação real é iniciada por um botão HTML:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample4.aspx)]

Em seguida, adicione a `AnimationExtender` para a página, fornecendo uma `ID`, o `TargetControlID` atributo e o obrigatoriamente `runat="server"`:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample5.aspx)]

Observe que não há nenhuma `<Animations>` nó dentro de `AnimationExtender` controle. Código JavaScript personalizado é usado para fornecer as animações a serem usados com o controle.

Assim como acontece com a API do servidor de `AnimationExtender`, não há nenhuma maneira fácil para atribuir uma animação para o extensor ainda. No entanto o extensor expõe vários métodos para ler e gravar animações registrado com os vários eventos (`OnClick`, `OnLoad`e assim por diante). Estes são alguns exemplos:

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

O formato do valor de retorno do `get_*()` funções e o formato do argumento para o `set_*()` functions é uma cadeia de caracteres JSON, fornecendo uma representação de objeto do qual seria a marcação XML. Atualmente, não há nenhuma maneira de passar um objeto no, mas é possível ler um objeto de uma determinada animação (`get_OnXXXBehavior()` métodos).

Aqui está uma cadeia de caracteres JSON (sem as aspas de delimitação e formatado legalmente) que representa uma animação disparada pelo botão, mas Animando o painel, redimensioná-la e desaparecimento ao mesmo tempo:

[!code-json[Main](changing-an-animation-using-client-side-code-cs/samples/sample6.json)]

O código JavaScript a seguir atribui esse descripting JSON para o `OnClick` animação do extensor atual e a executa:

[!code-html[Main](changing-an-animation-using-client-side-code-cs/samples/sample7.html)]


[![A animação é executada imediatamente, sem um clique do mouse (e com muito pouco de marcação)](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)

A animação é executada imediatamente, sem um clique do mouse (e com muito pouco de marcação) ([clique para exibir a imagem em tamanho normal](changing-an-animation-using-client-side-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](executing-animations-using-client-side-code-cs.md)
> [Próximo](animating-an-updatepanel-control-cs.md)
