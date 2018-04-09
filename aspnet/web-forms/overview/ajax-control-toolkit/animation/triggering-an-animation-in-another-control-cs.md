---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
title: Disparar uma animação em outro controle (c#) | Microsoft Docs
author: wenz
description: O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Em geral, iniciar um...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e5d99c2b-d8ee-413c-80d5-c120cffb0a4c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
msc.type: authoredcontent
ms.openlocfilehash: e94046ca70607e37c1b5ef57d5cedef67a236b94
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="triggering-an-animation-in-another-control-c"></a>Disparar uma animação em outro controle (c#)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)

> O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Em geral, iniciar uma animação é disparada pela interação do usuário com o mesmo controle. No entanto, também é possível interagir com um controle e, em seguida, animação outro controle.


## <a name="overview"></a>Visão geral

O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Em geral, iniciar uma animação é disparada pela interação do usuário com o mesmo controle. No entanto, também é possível interagir com um controle e, em seguida, animação outro controle.

## <a name="steps"></a>Etapas

Em primeiro lugar, incluem o `ScriptManager` na página; em seguida, a biblioteca ASP.NET AJAX é carregada, tornando possível usar o Kit de ferramentas:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample1.aspx)]

A animação será aplicada a um painel de texto que tem esta aparência:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample2.aspx)]

Na classe CSS associada para o painel, definir uma cor de plano de fundo adequado e também uma largura fixa para o painel:

[!code-css[Main](triggering-an-animation-in-another-control-cs/samples/sample3.css)]

Para iniciar o painel de animação, um botão HTML é usado. Observe que `<input type="button" />` é favoured pela `<asp:Button />` porque não queremos um postback quando o usuário clica no botão.

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample4.aspx)]

Em seguida, adicione o `AnimationExtender` para a página, fornecendo um `ID`, o `TargetControlID` atributo e o obrigatórias `runat="server"`. É importante definir `TargetControlID` para a ID do botão (o elemento acionar a animação), não para a ID do painel (o elemento que está sendo animado)

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample5.aspx)]

Dentro de `<Animations>` nó, animações local como de costume. Para torná-los a alterar o painel, não o botão Definir o `AnimationTarget` atributo para cada elemento de animação em `AnimationExtender`. O valor de `AnimationTarget` é a ID do painel, é claro. Dessa forma, as animações acontecem com o painel, não com o botão de disparo. Aqui está o `AnimationExtender` marcação para este cenário:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample6.aspx)]

Observe a ordem especial em que as animações individuais aparecem. Em primeiro lugar, o botão é desativado quando a animação é executada. Porque não há nenhum `AnimationTarget` atributo o `<EnableAction>` elemento, a animação é aplicado ao controle de origem: o botão. As etapas dois animação devem ser executadas parallelly (`<Parallel>` elemento). Ambos têm seus `AnimationTarget` atributos definidos como `"Panel1"`, animação, portanto, o painel, não o botão.


[![Um clique do mouse no botão inicia a animação de painel](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)

Um clique do mouse no botão inicia a animação de painel ([clique para exibir a imagem em tamanho normal](triggering-an-animation-in-another-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](disabling-actions-during-animation-cs.md)
> [Próximo](modifying-animations-from-the-server-side-cs.md)
