---
uid: web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-vb
title: Usando o extensor de controle ColorPicker (VB) | Microsoft Docs
author: microsoft
description: ColorPicker é um extensor AJAX ASP.NET que fornece funcionalidade de seleção de cor do lado do cliente com a interface do usuário em um controle de pop-up. Ele pode ser anexado a qualquer ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 577ae07b-a872-4818-a804-bca489b40ad0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-vb
msc.type: authoredcontent
ms.openlocfilehash: 3411119f85d7f5c26703b7df40cff24fdf30b81d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874508"
---
<a name="using-the-colorpicker-control-extender-vb"></a>Usando o extensor de controle ColorPicker (VB)
====================
por [Microsoft](https://github.com/microsoft)

> ColorPicker é um extensor AJAX ASP.NET que fornece funcionalidade de seleção de cor do lado do cliente com a interface do usuário em um controle de pop-up. Ele pode ser conectado a qualquer controle de caixa de texto do ASP.NET. It.


O objetivo deste tutorial é explicar como você pode usar o extensor do controle ColorPicker de kit de ferramentas de controle AJAX. O extensor do controle ColorPicker exibe uma caixa de diálogo pop-up que permite que você selecione uma cor. ColorPicker é útil sempre que você deseja fornecer uma interface de usuário intuitiva para um usuário selecionar uma cor.

## <a name="extending-a-textbox-control-with-the-colorpicker-control-extender"></a>Estendendo um controle de caixa de texto com o extensor do controle ColorPicker

Por exemplo, imagine que você deseja criar um site que permite que os visitantes criar cartões de negócios personalizados. Os visitantes podem inserir o texto para um cartão de visita e escolher a cor. A página ASP.NET na listagem 1 contém dois controles de caixa de texto denominada txtCardText e txtCardColor. Quando você enviar o formulário, os valores selecionados são exibidos (consulte a Figura 1).


[![Formulário Simple para criar um cartão de visita](using-the-colorpicker-control-extender-vb/_static/image1.jpg)](using-the-colorpicker-control-extender-vb/_static/image1.png)

**Figura 01**: formulário Simple para criar um cartão de visita ([clique para exibir a imagem em tamanho normal](using-the-colorpicker-control-extender-vb/_static/image2.png))


**Listando 1 - CreateCard.aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample1.aspx)]

O formulário na listagem 1 funciona, mas não fornecer uma excelente experiência de usuário. O usuário deve digitar uma cor na caixa de texto. Se o usuário deseja uma cor especializada - por exemplo, apenas a direito tonalidade de verde pea -, em seguida, o usuário deve descobrir o código HTML a cor sem nenhuma ajuda.

Você pode usar o extensor do controle ColorPicker para criar uma melhor experiência do usuário. ColorPicker exibe uma caixa de diálogo de cor quando você move o foco para um controle TextBox (consulte a Figura 2).


[![O extensor do controle ColorPicker](using-the-colorpicker-control-extender-vb/_static/image2.jpg)](using-the-colorpicker-control-extender-vb/_static/image3.png)

**Figura 02**: O extensor de controle ColorPicker ([clique para exibir a imagem em tamanho normal](using-the-colorpicker-control-extender-vb/_static/image4.png))


Você precisa concluir as duas etapas para usar o extensor do controle ColorPicker com o formulário na listagem 1:

1. Adicionar um controle ScriptManager à página
2. Adicione o extensor do controle ColorPicker à página

Antes de usar o ColorPicker, você deve adicionar um ScriptManager para sua página. É um bom lugar para adicionar o ScriptManager logo abaixo do lado do servidor abertura &lt;formulário&gt; marca. Você pode arrastar o ScriptManager para a página da caixa de ferramentas (o ScriptManager está localizado sob a guia Extensões AJAX). Como alternativa, você pode digitar a marca a seguir na exibição da fonte sob a marca de abertura formulário do lado do servidor:

&lt;asp:ScriptManager ID="ScriptManager1" runat="server" /&gt;

A maneira mais fácil para adicionar o extensor do controle ColorPicker para a página está no modo de Design. Se você posicionar o mouse sobre a caixa de texto txtCardColor, uma opção inteligente aparece o permite que você adicione um extensor (consulte a Figura 3). Se você escolher essa opção, o Assistente de extensor é exibido (consulte a Figura 4).


[![Adicionando um extensor](using-the-colorpicker-control-extender-vb/_static/image3.jpg)](using-the-colorpicker-control-extender-vb/_static/image5.png)

**Figura 03**: adicionando um extensor ([clique para exibir a imagem em tamanho normal](using-the-colorpicker-control-extender-vb/_static/image6.png))


[![Selecionando um extensor de controle com o Assistente de extensor](using-the-colorpicker-control-extender-vb/_static/image4.jpg)](using-the-colorpicker-control-extender-vb/_static/image7.png)

**Figura 04**: selecionando um extensor de controle com o Assistente de extensor ([clique para exibir a imagem em tamanho normal](using-the-colorpicker-control-extender-vb/_static/image8.png))


Você pode escolher o extensor ColorPicker para estender o txtCardColor caixa de texto com o extensor ColorPicker. Clique Okey para fechar a caixa de diálogo.

Depois de fazer essas alterações, a fonte para a página se parece com a listagem 2.

**A listagem 2 - CreateCard.aspx (com ColorPicker)**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample2.aspx)]

Observe que agora a página contém um controle ColorPickerExtender que aparece diretamente abaixo do controle TextBox txtCardColor. O controle de ColorPickerExtender estende o controle txtCardColor para que ele exibe uma caixa de diálogo de seletor de cor.

## <a name="using-a-button-to-launch-the-color-picker-dialog"></a>Usando um botão para iniciar a caixa de diálogo de seletor de cor

O extensor ColorPicker suporta as seguintes propriedades:

- PopupButtonId - a ID de um botão na página que faz com que a caixa de diálogo Seletor de cor aparecer.
- PopupPosition - a posição, em relação ao controle de destino da caixa de diálogo de seletor de cor. Os valores possíveis são absoluto, Center, BottomLeft, BottomRight, TopLeft, TopRight, à direita e esquerda (o padrão é BottomLeft).
- SampleControlId - a ID de um controle que exibe a cor selecionada.
- SelectedColor - a cor inicial, selecionada pelo ColorPicker.

Você pode usar essas propriedades para personalizar como a caixa de diálogo de seletor de cor é exibida e como a cor selecionada é exibida. A página na listagem 3 ilustra como você pode usar várias dessas propriedades.

**A listagem 3 - CreateCardButton.aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample3.aspx)]

A página na listagem 3 inclui selecionar cor botão (consulte a Figura 5). Quando você clica nesse botão, a caixa de diálogo de seletor de cor é exibido acima da caixa de texto. Se você selecionar uma cor na caixa de diálogo cor selecionada é exibida como a cor de plano de fundo do lblSample controle de rótulo.

A propriedade ColorPicker PopupButtonID é usada para associar o botão Selecionar cor o extensor ColorPicker. Quando você fornecer um valor para a propriedade PopupButtonID, a caixa de diálogo do seletor de cores não aparece mais quando o controle de destino tem foco. Você deve clicar no botão para exibir a caixa de diálogo.

A propriedade SampleControlID é usada para associar um controle que exibe a cor selecionada com o ColorPicker. ColorPicker altera a cor de plano de fundo deste controle para a cor atualmente selecionada.


[![Exibir a caixa de diálogo Seletor de cores com um botão](using-the-colorpicker-control-extender-vb/_static/image5.jpg)](using-the-colorpicker-control-extender-vb/_static/image9.png)

**Figura 05**: exibindo a caixa de diálogo Seletor de cores com um botão ([clique para exibir a imagem em tamanho normal](using-the-colorpicker-control-extender-vb/_static/image10.png))


## <a name="summary"></a>Resumo

Neste tutorial, você aprendeu a usar o extensor do controle ColorPicker para exibir uma caixa de diálogo de seletor de cor de pop-up. Primeiro, examinamos como você pode exibir a caixa de diálogo quando o foco é movido para um controle de caixa de texto. Em seguida, você aprendeu a criar um botão que exibe a caixa de diálogo de seletor de cor quando o botão é clicado.

> [!div class="step-by-step"]
> [Anterior](using-the-colorpicker-control-extender-cs.md)
