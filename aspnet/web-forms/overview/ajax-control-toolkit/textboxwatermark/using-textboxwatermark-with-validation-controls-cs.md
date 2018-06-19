---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
title: Usando TextBoxWatermark com controles de validação (c#) | Microsoft Docs
author: wenz
description: O controle TextBoxWatermark AJAX Control Toolkit estende uma caixa de texto para que um texto seja exibido dentro da caixa. Quando um usuário clica na caixa, ele,...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: d49940cb-d38c-456a-b800-5f0eb705d09f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: b5cc7974f3444b54770cba54b991aab7b103f753
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868874"
---
<a name="using-textboxwatermark-with-validation-controls-c"></a>Usando TextBoxWatermark com controles de validação (c#)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)

> O controle TextBoxWatermark AJAX Control Toolkit estende uma caixa de texto para que um texto seja exibido dentro da caixa. Quando um usuário clica na caixa, é esvaziado. Se o usuário deixar a caixa sem inserir texto, o texto pré-preenchidos reaparecerá. Isso pode entrar em conflito com os controles de validação ASP.NET na mesma página, mas podem ser solucionar esses problemas.


## <a name="overview"></a>Visão geral

O `TextBoxWatermark` controle AJAX Control Toolkit estende uma caixa de texto para que um texto seja exibido dentro da caixa. Quando um usuário clica na caixa, é esvaziado. Se o usuário deixar a caixa sem inserir texto, o texto pré-preenchidos reaparecerá. Isso pode entrar em conflito com os controles de validação ASP.NET na mesma página, mas podem ser solucionar esses problemas.

## <a name="steps"></a>Etapas

A configuração básica do exemplo é o seguinte: uma `TextBox` controle é marca d'água usando um `TextBoxWatermarkExtender` controle. Um botão aciona um postback e será posteriormente usado para disparar os controles de validação na página. Além disso, um `ScriptManager` controle é necessário para inicializar o ASP.NET AJAX:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample1.aspx)]

Agora, adicione um `RequiredFieldValidator` controle que verifica se há texto no campo quando o formulário é enviado. O `InitialValue` propriedade do validador deve ser definida com o mesmo valor que é usado no `TextBoxWatermarkExtender` controle: quando o formulário é enviado, o valor de uma caixa de texto inalterado é o valor de marca d'água nele:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample2.aspx)]

No entanto, há um problema com essa abordagem: se o cliente desativa o JavaScript, o campo de texto não é previamente preenchido com o texto de marca d'água, portanto o `RequiredFieldValidator` não aciona uma mensagem de erro. Portanto, um segundo `RequiredFieldValidator` controle é necessário que verifica se há uma caixa de texto (omitindo o `InitialValue` atributo).

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample3.aspx)]

Como usam ambos os validadores `Display` = `"Dynamic"`, o usuário final não pode distinguir a aparência visual que os dois validadores foi acionado; em vez disso, parece que houve apenas um deles.

Finalmente, adicione o código do lado do servidor para o texto no campo de saída, se nenhum validador emitida uma mensagem de erro:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample4.aspx)]


[![O validador reclama que não há nenhum texto no campo](using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)

O validador reclama que não há nenhum texto no campo ([clique para exibir a imagem em tamanho normal](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](using-textboxwatermark-in-a-formview-cs.md)
> [Próximo](using-textboxwatermark-in-a-formview-vb.md)
