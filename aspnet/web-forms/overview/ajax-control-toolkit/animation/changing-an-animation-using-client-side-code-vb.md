---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
title: "Alterando uma animação usando o código do lado do cliente (VB) | Microsoft Docs"
author: wenz
description: "O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. A animação também pode..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a7fe5de5-a964-4780-ae5e-70821dfb50a0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 83d1a21fba37d8807be467d02b5550dc7d096e6c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="changing-an-animation-using-client-side-code-vb"></a>Alterando uma animação usando o código do lado do cliente (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11VB.pdf)

> O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. A animação também pode ser alterada usando código personalizado de JavaScript do lado do cliente.


## <a name="overview"></a>Visão Geral

O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. A animação também pode ser alterada usando código personalizado de JavaScript do lado do cliente.

## <a name="steps"></a>Etapas

Em primeiro lugar, incluem o `ScriptManager` na página; em seguida, a biblioteca ASP.NET AJAX é carregada, tornando possível usar o Kit de ferramentas:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample1.aspx)]

A animação será aplicada a um painel de texto que tem esta aparência:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample2.aspx)]

Na classe CSS associada para o painel, definir uma cor de plano de fundo adequado e também uma largura fixa para o painel:

[!code-css[Main](changing-an-animation-using-client-side-code-vb/samples/sample3.css)]

A animação real é iniciada por um botão HTML:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample4.aspx)]

Em seguida, adicione o `AnimationExtender` para a página, fornecendo um `ID`, o `TargetControlID` atributo e o obrigatórias `runat="server"`:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample5.aspx)]

Observe que não há nenhum `<Animations>` nó dentro de `AnimationExtender` controle. Código JavaScript personalizado é usado para fornecer as animações devem ser usados com o controle.

Assim como acontece com a API do servidor de `AnimationExtender`, não é possível atribuir uma animação para o extensor ainda fácil. No entanto o extensor expor vários métodos para ler e gravar animações registrado com os vários eventos (`OnClick`, `OnLoad`e assim por diante). Estes são alguns exemplos:

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

O formato do valor de retorno a `get_*()` funções e o formato do argumento para o `set_*()` funções é uma cadeia de caracteres JSON, fornecendo uma representação de objeto do que seria a marcação XML. Atualmente, não é possível passar um objeto, mas é possível ler um objeto de uma determinada animação (`get_OnXXXBehavior()` métodos).

Aqui está uma cadeia de caracteres JSON (sem as aspas delimitação e formatada satisfatoriamente) que representa uma animação disparada pelo botão, mas o painel de animação, redimensioná-la e desaparecimento ao mesmo tempo:

[!code-json[Main](changing-an-animation-using-client-side-code-vb/samples/sample6.json)]

O código JavaScript a seguir atribui esse descripting JSON para o `OnClick` animação do extensor atual e a executa:

[!code-html[Main](changing-an-animation-using-client-side-code-vb/samples/sample7.html)]


[![A animação é executada imediatamente, sem um clique do mouse (e com muito pouco marcação)](changing-an-animation-using-client-side-code-vb/_static/image2.png)](changing-an-animation-using-client-side-code-vb/_static/image1.png)

A animação é executada imediatamente, sem um clique do mouse (e com muito pouco marcação) ([clique para exibir a imagem em tamanho normal](changing-an-animation-using-client-side-code-vb/_static/image3.png))

>[!div class="step-by-step"]
[Anterior](executing-animations-using-client-side-code-vb.md)
[Próximo](animating-an-updatepanel-control-vb.md)
