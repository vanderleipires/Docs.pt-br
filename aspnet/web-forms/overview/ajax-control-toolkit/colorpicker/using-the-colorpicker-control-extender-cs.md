---
uid: web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-cs
title: Usar o extensor de controle ColorPicker (c#) | Microsoft Docs
author: microsoft
description: ColorPicker é um extensor ASP.NET AJAX que fornece funcionalidade de separação de cor do lado do cliente com interface do usuário em um controle pop-up. Ele pode ser anexado a qualquer ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 0d86a1e7-a910-4ab2-b85c-7a9ea6906c39
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-cs
msc.type: authoredcontent
ms.openlocfilehash: f20928099e2b4db477705cd1634fd28745a328ac
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37383898"
---
<a name="using-the-colorpicker-control-extender-c"></a>Usar o extensor de controle ColorPicker (c#)
====================
por [Microsoft](https://github.com/microsoft)

> ColorPicker é um extensor ASP.NET AJAX que fornece funcionalidade de separação de cor do lado do cliente com interface do usuário em um controle pop-up. Ele pode ser anexado a qualquer controle TextBox do ASP.NET. -Lo.


O objetivo deste tutorial é explicar como você pode usar o extensor do controle ColorPicker de kit de ferramentas de controle do AJAX. O extensor de controle ColorPicker exibe uma caixa de diálogo pop-up que permite que você selecione uma cor. ColorPicker é útil sempre que você deseja fornecer uma interface de usuário intuitiva para um usuário selecionar uma cor.

## <a name="extending-a-textbox-control-with-the-colorpicker-control-extender"></a>Estendendo um controle de caixa de texto com o extensor de controle ColorPicker

Por exemplo, imagine que você deseja criar um site que permite aos visitantes criar cartões de visita personalizados. Os visitantes podem digitar o texto para um cartão de visita e escolher a cor. A página ASP.NET na listagem 1 contém dois controles de caixa de texto chamado txtCardText e txtCardColor. Quando você envia o formulário, os valores selecionados são exibidos (consulte a Figura 1).


[![Formulário simples para a criação de um cartão de visita](using-the-colorpicker-control-extender-cs/_static/image1.jpg)](using-the-colorpicker-control-extender-cs/_static/image1.png)

**Figura 01**: um formulário simples para a criação de um cartão de visita ([clique para exibir a imagem em tamanho normal](using-the-colorpicker-control-extender-cs/_static/image2.png))


**Listagem 1 - CreateCard.aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample1.aspx)]

O formulário na listagem 1 funciona, mas não fornece uma excelente experiência de usuário. O usuário deve digitar uma cor na caixa de texto. Se o usuário deseja uma cor especializada - por exemplo, apenas a direito tonalidade de verde pea – e em seguida, o usuário deve descobrir o código de cor HTML sem qualquer ajuda.

Você pode usar o extensor do controle ColorPicker para criar uma melhor experiência do usuário. ColorPicker exibe uma caixa de diálogo de cor quando você move o foco para um controle TextBox (veja a Figura 2).


[![O extensor de controle ColorPicker](using-the-colorpicker-control-extender-cs/_static/image2.jpg)](using-the-colorpicker-control-extender-cs/_static/image3.png)

**Figura 02**: O extensor de controle ColorPicker ([clique para exibir a imagem em tamanho normal](using-the-colorpicker-control-extender-cs/_static/image4.png))


Você precisa concluir duas etapas para usar o extensor do controle ColorPicker com o formulário na listagem 1:

1. Adicione um controle ScriptManager à página
2. Adicione o extensor do controle ColorPicker à página

Antes de usar o ColorPicker, você deve adicionar um ScriptManager à página. Um bom lugar para adicionar o ScriptManager é logo abaixo do lado do servidor abrindo &lt;formulário&gt; marca. Você pode arrastar o ScriptManager para a página da caixa de ferramentas (o ScriptManager está localizado na guia Extensões AJAX). Como alternativa, você pode digitar a seguinte marca na exibição da fonte abaixo da marca de formulário do lado do servidor de abertura:

&lt;asp:ScriptManager ID="ScriptManager1" runat="server" /&gt;

A maneira mais fácil para adicionar o extensor do controle ColorPicker para a página está no modo de exibição de Design. Se você passar o mouse sobre a caixa de texto txtCardColor, uma opção de tarefa inteligente aparece o permite que você adicione um extensor (veja a Figura 3). Se você escolher essa opção, o Assistente de extensor aparece (veja a Figura 4).


[![Adicionando um extensor](using-the-colorpicker-control-extender-cs/_static/image3.jpg)](using-the-colorpicker-control-extender-cs/_static/image5.png)

**Figura 03**: adicionando um extensor ([clique para exibir a imagem em tamanho normal](using-the-colorpicker-control-extender-cs/_static/image6.png))


[![Selecionando um extensor de controle com o Assistente de extensor](using-the-colorpicker-control-extender-cs/_static/image4.jpg)](using-the-colorpicker-control-extender-cs/_static/image7.png)

**Figura 04**: selecionando um extensor de controle com o Assistente de extensor ([clique para exibir a imagem em tamanho normal](using-the-colorpicker-control-extender-cs/_static/image8.png))


Você pode escolher o extensor ColorPicker para estender o txtCardColor caixa de texto com o extensor ColorPicker. Clique em Okey para fechar a caixa de diálogo.

Depois de fazer essas alterações, o código-fonte para a página semelhante à listagem 2.

Listagem 2 - CreateCard.aspx (com ColorPicker)

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample2.aspx)]

Observe que agora a página contém um controle ColorPickerExtender que aparece diretamente abaixo do controle TextBox txtCardColor. O controle ColorPickerExtender estende o controle txtCardColor para que ele exiba uma caixa de diálogo do seletor de cor.

## <a name="using-a-button-to-launch-the-color-picker-dialog"></a>Usando um botão para iniciar a caixa de diálogo do seletor de cor

O extensor ColorPicker suporta as seguintes propriedades:

- PopupButtonId - a ID de um botão na página que faz com que a caixa de diálogo do seletor de cor aparecer.
- PopupPosition - a posição, em relação ao controle de destino da caixa de diálogo de seletor de cor. Valores possíveis são absoluto, Center, BottomLeft, BottomRight, TopLeft, superior direito, à direita e esquerda (o padrão é BottomLeft).
- SampleControlId - a ID de um controle que exibe a cor selecionada.
- SelectedColor – a cor inicial selecionada pelo ColorPicker.

Você pode usar essas propriedades para personalizar como a caixa de diálogo do seletor de cor é exibida e como a cor selecionada é exibida. A página na listagem 3 ilustra como você pode usar várias dessas propriedades.

**Listagem 3 - CreateCardButton.aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample3.aspx)]

A página na listagem 3 inclui selecionar cor botão (consulte a Figura 5). Quando você clica nesse botão, a caixa de diálogo do seletor de cor é exibido acima da caixa de texto. Se você selecionar uma cor na caixa de diálogo cor selecionada é exibida como a cor do plano de fundo do lblSample controle de rótulo.

A propriedade ColorPicker PopupButtonID é usada para associar o botão Selecionar cor o extensor ColorPicker. Ao fornecer um valor da propriedade PopupButtonID, a caixa de diálogo do seletor de cor será mais exibida quando o controle de destino tem o foco. Você deve clicar no botão para exibir a caixa de diálogo.

A propriedade SampleControlID é usada para associar um controle que exibe a cor selecionada com o ColorPicker. ColorPicker altera a cor de plano de fundo desse controle para a cor atualmente selecionada.


[![Exibir a caixa de diálogo do seletor de cor com um botão](using-the-colorpicker-control-extender-cs/_static/image5.jpg)](using-the-colorpicker-control-extender-cs/_static/image9.png)

**Figura 05**: exibir a caixa de diálogo do seletor de cor com um botão ([clique para exibir a imagem em tamanho normal](using-the-colorpicker-control-extender-cs/_static/image10.png))


## <a name="summary"></a>Resumo

Neste tutorial, você aprendeu como usar o extensor do controle ColorPicker para exibir uma caixa de diálogo do seletor de cor do pop-up. Primeiro, examinamos como você pode exibir a caixa de diálogo quando o foco é movido para um controle de caixa de texto. Em seguida, você aprendeu a criar um botão que exibe a caixa de diálogo do seletor de cor quando o botão é clicado.

> [!div class="step-by-step"]
> [Avançar](using-the-colorpicker-control-extender-vb.md)
