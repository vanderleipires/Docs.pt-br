---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
title: Uso de TextBoxWatermark com controles de validação (c#) | Microsoft Docs
author: wenz
description: O controle TextBoxWatermark do AJAX Control Toolkit estende uma caixa de texto para que um texto seja exibido dentro da caixa. Quando um usuário clica na caixa de-eu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: d49940cb-d38c-456a-b800-5f0eb705d09f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: fe28f169e5e714ba07d8a71bb33d0f62608d688b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380685"
---
<a name="using-textboxwatermark-with-validation-controls-c"></a>Uso de TextBoxWatermark com controles de validação (c#)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)

> O controle TextBoxWatermark do AJAX Control Toolkit estende uma caixa de texto para que um texto seja exibido dentro da caixa. Quando um usuário clica na caixa, é esvaziado. Se o usuário deixa a caixa sem digitar texto, o texto previamente preenchido reaparece. Isso entrem em conflito com os controles de validação ASP.NET na mesma página, mas esses problemas podem ser superados.


## <a name="overview"></a>Visão geral

O `TextBoxWatermark` o controle no AJAX Control Toolkit, uma caixa de texto para que um texto seja exibido dentro da caixa. Quando um usuário clica na caixa, é esvaziado. Se o usuário deixa a caixa sem digitar texto, o texto previamente preenchido reaparece. Isso entrem em conflito com os controles de validação ASP.NET na mesma página, mas esses problemas podem ser superados.

## <a name="steps"></a>Etapas

A configuração básica do exemplo é o seguinte: uma `TextBox` controle é com marca d'água usando um `TextBoxWatermarkExtender` controle. Um botão dispara um postback e será posteriormente usado para disparar os controles de validação na página. Além disso, um `ScriptManager` controle é necessário para inicializar o ASP.NET AJAX:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample1.aspx)]

Agora, adicione um `RequiredFieldValidator` controle que verifica se há texto no campo quando o formulário é enviado. O `InitialValue` propriedade do validador deve ser definida com o mesmo valor que é usado no `TextBoxWatermarkExtender` controle: quando o formulário for enviado, o valor de uma caixa de texto inalterado é o valor de marca d'água dentro dele:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample2.aspx)]

No entanto, há um problema com essa abordagem: se o cliente desativa o JavaScript, o campo de texto não é preenchido com o texto de marca d'água, portanto o `RequiredFieldValidator` não dispara uma mensagem de erro. Portanto, um segundo `RequiredFieldValidator` é necessário o controle que verifica se há uma caixa de texto vazia (omitindo o `InitialValue` atributo).

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample3.aspx)]

Uma vez que utilizar ambos os validadores `Display` = `"Dynamic"`, o usuário final não é possível distinguir da aparência visual que foi acionada dos dois validadores; em vez disso, parece que houve apenas um deles.

Por fim, adicione algum código do lado do servidor para o texto no campo de saída se nenhum validador emitida uma mensagem de erro:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample4.aspx)]


[![O validador reclama que não há nenhum texto no campo](using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)

O validador reclama que não há nenhum texto no campo ([clique para exibir a imagem em tamanho normal](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](using-textboxwatermark-in-a-formview-cs.md)
> [Próximo](using-textboxwatermark-in-a-formview-vb.md)
