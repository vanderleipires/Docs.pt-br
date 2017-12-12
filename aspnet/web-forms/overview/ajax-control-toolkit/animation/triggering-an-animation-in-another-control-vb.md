---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
title: "Disparar uma animação em outro controle (VB) | Microsoft Docs"
author: wenz
description: "O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Em geral, iniciar um..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 25ebaf1f-5a9f-423d-98c7-1d694e93664f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
msc.type: authoredcontent
ms.openlocfilehash: ce1d29cbd06ef8a470780ff4c7bda8039575d59f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="triggering-an-animation-in-another-control-vb"></a>Disparar uma animação em outro controle (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8VB.pdf)

> O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Em geral, iniciar uma animação é disparada pela interação do usuário com o mesmo controle. No entanto, também é possível interagir com um controle e, em seguida, animação outro controle.


## <a name="overview"></a>Visão Geral

O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Em geral, iniciar uma animação é disparada pela interação do usuário com o mesmo controle. No entanto, também é possível interagir com um controle e, em seguida, animação outro controle.

## <a name="steps"></a>Etapas

Em primeiro lugar, incluem o `ScriptManager` na página; em seguida, a biblioteca ASP.NET AJAX é carregada, tornando possível usar o Kit de ferramentas:

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample1.aspx)]

A animação será aplicada a um painel de texto que tem esta aparência:

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample2.aspx)]

Na classe CSS associada para o painel, definir uma cor de plano de fundo adequado e também uma largura fixa para o painel:

[!code-css[Main](triggering-an-animation-in-another-control-vb/samples/sample3.css)]

Para iniciar o painel de animação, um botão HTML é usado. Observe que `<input type="button" />` é favoured pela `<asp:Button />` porque não queremos um postback quando o usuário clica no botão.

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample4.aspx)]

Em seguida, adicione o `AnimationExtender` para a página, fornecendo um `ID`, o `TargetControlID` atributo e o obrigatórias `runat="server"`. É importante definir `TargetControlID` para a ID do botão (o elemento acionar a animação), não para a ID do painel (o elemento que está sendo animado)

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample5.aspx)]

Dentro de `<Animations>` nó, animações local como de costume. Para torná-los a alterar o painel, não o botão Definir o `AnimationTarget` atributo para cada elemento de animação em `AnimationExtender`. O valor de `AnimationTarget` é a ID do painel, é claro. Dessa forma, as animações acontecem com o painel, não com o botão de disparo. Aqui está o `AnimationExtender` marcação para este cenário:

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample6.aspx)]

Observe a ordem especial em que as animações individuais aparecem. Em primeiro lugar, o botão é desativado quando a animação é executada. Porque não há nenhum `AnimationTarget` atributo o `<EnableAction>` elemento, a animação é aplicado ao controle de origem: o botão. As etapas dois animação devem ser executadas parallelly (`<Parallel>` elemento). Ambos têm seus `AnimationTarget` atributos definidos como `"Panel1"`, animação, portanto, o painel, não o botão.


[![Um clique do mouse no botão inicia a animação de painel](triggering-an-animation-in-another-control-vb/_static/image2.png)](triggering-an-animation-in-another-control-vb/_static/image1.png)

Um clique do mouse no botão inicia a animação de painel ([clique para exibir a imagem em tamanho normal](triggering-an-animation-in-another-control-vb/_static/image3.png))

>[!div class="step-by-step"]
[Anterior](disabling-actions-during-animation-vb.md)
[Próximo](modifying-animations-from-the-server-side-vb.md)
