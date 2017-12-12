---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
title: "Modificando animações do lado do servidor (c#) | Microsoft Docs"
author: wenz
description: "O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. As animações também podem..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b0abec39-a1c9-422d-ba9a-ef16f6185af8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
msc.type: authoredcontent
ms.openlocfilehash: e1f9bda03b3e3edf3bbdc591cde9858944d39321
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="modifying-animations-from-the-server-side-c"></a>Modificando animações do lado do servidor (c#)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9CS.pdf)

> O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. As animações também podem ser alteradas no lado do servidor


## <a name="overview"></a>Visão Geral

O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. As animações também podem ser alteradas no lado do servidor

## <a name="steps"></a>Etapas

Em primeiro lugar, incluem o `ScriptManager` na página; em seguida, a biblioteca ASP.NET AJAX é carregada, tornando possível usar o Kit de ferramentas:

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample1.aspx)]

A animação será aplicada a um painel de texto que tem esta aparência:

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample2.aspx)]

Na classe CSS associada para o painel, definir uma cor de plano de fundo adequado e também uma largura fixa para o painel:

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample3.css)]

O restante do código é executado no lado do servidor e não usar a marcação; em vez disso, ele usa o código para criar o `AnimationExtender` controle:

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample4.aspx)]

No entanto, o Kit de ferramentas atualmente não fornece um acesso de API para criar as animações individuais. No entanto, é possível definir o `AnimationExtender`propriedade animações para uma cadeia de caracteres que contém a marcação XML usada durante a atribuição de animações declarativamente. Para criar o XML que não deve conter o `<Animations>` elemento, você pode usar o XML do .NET Framework oferecem suporte ou, como no código a seguir, basta fornecer a cadeia de caracteres:

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample5.css)]

Finalmente, adicione o `AnimationExtender` controle dentro da página atual, o `<form runat="server">` elemento, certificando-se de que a animação está incluída e é executado:

[!code-html[Main](modifying-animations-from-the-server-side-cs/samples/sample6.html)]


[![A animação é criada usando o código do lado do servidor C c# /VB](modifying-animations-from-the-server-side-cs/_static/image2.png)](modifying-animations-from-the-server-side-cs/_static/image1.png)

A animação é criada usando o código do lado do servidor C /VB ([clique para exibir a imagem em tamanho normal](modifying-animations-from-the-server-side-cs/_static/image3.png))

>[!div class="step-by-step"]
[Anterior](triggering-an-animation-in-another-control-cs.md)
[Próximo](executing-animations-using-client-side-code-cs.md)
