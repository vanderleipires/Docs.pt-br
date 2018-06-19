---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
title: Alterando uma animação usando o código do lado do cliente (c#) | Microsoft Docs
author: wenz
description: O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. A animação também pode...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 2bfbc5cc-f942-44b7-a62d-a29520f1da9a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 2f572efeb012d88ab15740bab7b0e4383572f3f7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870642"
---
<a name="changing-an-animation-using-client-side-code-c"></a>Alterando uma animação usando o código do lado do cliente (c#)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)

> O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. A animação também pode ser alterada usando código personalizado de JavaScript do lado do cliente.


## <a name="overview"></a>Visão geral

O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. A animação também pode ser alterada usando código personalizado de JavaScript do lado do cliente.

## <a name="steps"></a>Etapas

Em primeiro lugar, incluem o `ScriptManager` na página; em seguida, a biblioteca ASP.NET AJAX é carregada, tornando possível usar o Kit de ferramentas:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample1.aspx)]

A animação será aplicada a um painel de texto que tem esta aparência:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample2.aspx)]

Na classe CSS associada para o painel, definir uma cor de plano de fundo adequado e também uma largura fixa para o painel:

[!code-css[Main](changing-an-animation-using-client-side-code-cs/samples/sample3.css)]

A animação real é iniciada por um botão HTML:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample4.aspx)]

Em seguida, adicione o `AnimationExtender` para a página, fornecendo um `ID`, o `TargetControlID` atributo e o obrigatórias `runat="server"`:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample5.aspx)]

Observe que não há nenhum `<Animations>` nó dentro de `AnimationExtender` controle. Código JavaScript personalizado é usado para fornecer as animações devem ser usados com o controle.

Assim como acontece com a API do servidor de `AnimationExtender`, não é possível atribuir uma animação para o extensor ainda fácil. No entanto o extensor expor vários métodos para ler e gravar animações registrado com os vários eventos (`OnClick`, `OnLoad`e assim por diante). Estes são alguns exemplos:

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

O formato do valor de retorno a `get_*()` funções e o formato do argumento para o `set_*()` funções é uma cadeia de caracteres JSON, fornecendo uma representação de objeto do que seria a marcação XML. Atualmente, não é possível passar um objeto, mas é possível ler um objeto de uma determinada animação (`get_OnXXXBehavior()` métodos).

Aqui está uma cadeia de caracteres JSON (sem as aspas delimitação e formatada satisfatoriamente) que representa uma animação disparada pelo botão, mas o painel de animação, redimensioná-la e desaparecimento ao mesmo tempo:

[!code-json[Main](changing-an-animation-using-client-side-code-cs/samples/sample6.json)]

O código JavaScript a seguir atribui esse descripting JSON para o `OnClick` animação do extensor atual e a executa:

[!code-html[Main](changing-an-animation-using-client-side-code-cs/samples/sample7.html)]


[![A animação é executada imediatamente, sem um clique do mouse (e com muito pouco marcação)](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)

A animação é executada imediatamente, sem um clique do mouse (e com muito pouco marcação) ([clique para exibir a imagem em tamanho normal](changing-an-animation-using-client-side-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](executing-animations-using-client-side-code-cs.md)
> [Próximo](animating-an-updatepanel-control-cs.md)
