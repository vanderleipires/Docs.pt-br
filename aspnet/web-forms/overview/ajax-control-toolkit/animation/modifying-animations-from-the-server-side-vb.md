---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
title: Modificando animações pelo lado do servidor (VB) | Microsoft Docs
author: wenz
description: O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. As animações podem também...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: addcf4aa-340a-460b-9c64-506424a1f725
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
msc.type: authoredcontent
ms.openlocfilehash: d39a40f2776d5b87bf82d1a6c6282ce920f4a907
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37401948"
---
<a name="modifying-animations-from-the-server-side-vb"></a>Modificando animações pelo lado do servidor (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9VB.pdf)

> O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. As animações também podem ser alteradas no lado do servidor


## <a name="overview"></a>Visão geral

O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. As animações também podem ser alteradas no lado do servidor

## <a name="steps"></a>Etapas

Em primeiro lugar, inclua o `ScriptManager` na página; em seguida, a biblioteca do AJAX ASP.NET é carregada, tornando possível usar o Kit de ferramentas de controle:

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample1.aspx)]

A animação será aplicada a um painel de texto que tem esta aparência:

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample2.aspx)]

Na classe CSS associado para o painel, definir uma cor de fundo interessante e também definir uma largura fixa para o painel:

[!code-css[Main](modifying-animations-from-the-server-side-vb/samples/sample3.css)]

O restante do código é executado no lado do servidor e não usa marcação; em vez disso, ele usa o código para criar o `AnimationExtender` controle:

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample4.aspx)]

No entanto, o Kit de ferramentas de controle atualmente não fornece um acesso de API para criar as animações individuais. No entanto, é possível definir o `AnimationExtender`propriedade Animations para uma cadeia de caracteres que contém a marcação XML usada ao atribuir as animações de forma declarativa. Para criar o XML que não deve conter o `<Animations>` elemento, você pode usar o XML do .NET Framework dão suporte ou, como no código a seguir, basta fornecer a cadeia de caracteres:

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample5.vb)]

Por fim, adicione a `AnimationExtender` dentro de controle para a página atual, o `<form runat="server">` elemento, certificando-se de que a animação está incluída e é executado:

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample6.vb)]


[![A animação é criada usando o código do lado do servidor C# /VB](modifying-animations-from-the-server-side-vb/_static/image2.png)](modifying-animations-from-the-server-side-vb/_static/image1.png)

A animação é criada usando o código do lado do servidor C# /VB ([clique para exibir a imagem em tamanho normal](modifying-animations-from-the-server-side-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](triggering-an-animation-in-another-control-vb.md)
> [Próximo](executing-animations-using-client-side-code-vb.md)
