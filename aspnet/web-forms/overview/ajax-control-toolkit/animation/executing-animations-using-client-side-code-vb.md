---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
title: Executar animações usando o código do lado do cliente (VB) | Microsoft Docs
author: wenz
description: O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. A execução de animação...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: f7073f50-d765-456d-9957-926ce60f35f6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 9b180ad17e0e3d2dffa6262d10a83a8353e0a1d0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811880"
---
<a name="executing-animations-using-client-side-code-vb"></a>Executar animações usando o código do lado do cliente (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10VB.pdf)

> O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. A execução de animação também pode ser disparada usando o código de JavaScript do lado do cliente personalizado.


## <a name="overview"></a>Visão geral

O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. A execução de animação também pode ser disparada usando o código de JavaScript do lado do cliente personalizado.

## <a name="steps"></a>Etapas

Em primeiro lugar, inclua o `ScriptManager` na página; em seguida, a biblioteca do AJAX ASP.NET é carregada, tornando possível usar o Kit de ferramentas de controle:

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample1.aspx)]

A animação será aplicada a um painel de texto que tem esta aparência:

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample2.aspx)]

Na classe CSS associado para o painel, definir uma cor de fundo interessante e também definir uma largura fixa para o painel:

[!code-css[Main](executing-animations-using-client-side-code-vb/samples/sample3.css)]

Em seguida, adicione a `AnimationExtender` para a página, fornecendo uma `ID`, o `TargetControlID` atributo e o obrigatoriamente `runat="server"`:

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample4.aspx)]

Dentro de `<Animations>` nó, use `<OnClick>` executar as animações quando o usuário clica no painel. Adicione duas animações a serem executadas parallelly:

[!code-xml[Main](executing-animations-using-client-side-code-vb/samples/sample5.xml)]

Para fins de demonstração, essa animação (e todas as outras animações criadas usando o Kit de ferramentas de controle) são executados usando o código JavaScript, depois que a página é executada. Em primeiro lugar é necessário acessar o `AnimationExtender` controle. A biblioteca do AJAX ASP.NET fornece o `$find()` função para esta tarefa:

[!code-csharp[Main](executing-animations-using-client-side-code-vb/samples/sample6.cs)]

O `AnimationExtender` controle expõe uma API avançada, incluindo os métodos com nomes idênticos aos manipuladores de eventos usados na marcação XML: `OnClick()`, `OnLoad()`e assim por diante. Por exemplo, uma chamada do `OnClick()` a animação dentro do método executa o `<OnClick>` elemento do `AnimationExtender` controle:

[!code-javascript[Main](executing-animations-using-client-side-code-vb/samples/sample7.js)]

Aqui está o código de JavaScript do lado do cliente completo que emula o clique no painel depois que a página tiver sido totalmente carregada Observe que o `pageLoad()` nome da função é usado que é chamado pelo ASP.NET AJAX uma vez a página e todos os incluídos foram bibliotecas de JavaScript carregado.

[!code-html[Main](executing-animations-using-client-side-code-vb/samples/sample8.html)]


[![A animação é executada imediatamente, sem um clique do mouse](executing-animations-using-client-side-code-vb/_static/image2.png)](executing-animations-using-client-side-code-vb/_static/image1.png)

A animação é executada imediatamente, sem um clique do mouse ([clique para exibir a imagem em tamanho normal](executing-animations-using-client-side-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](modifying-animations-from-the-server-side-vb.md)
> [Próximo](changing-an-animation-using-client-side-code-vb.md)
