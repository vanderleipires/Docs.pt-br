---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
title: Criando um numérico para cima/para baixo do controle com um serviço back-end Web (VB) | Microsoft Docs
author: wenz
description: Em vez de deixar um usuário digitar um valor em uma caixa de seleção, um controle (o que existe no Windows e outros sistemas operacionais) para cima/baixo numérico pode revelar mais assim como c...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: afa59dfa-fef1-43d3-8fdd-aea3be36ed3c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
msc.type: authoredcontent
ms.openlocfilehash: bf69b3e6932df528e8ccd2348ffa6f13132f99f8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830828"
---
<a name="creating-a-numeric-updown-control-with-a-web-service-backend-vb"></a>Criando um controle cima/baixo numérico com um serviço back-end Web (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1VB.pdf)

> Em vez de deixar um usuário digitar um valor em uma caixa de seleção, um numérico para cima/para baixo do controle (o que existe no Windows e outros sistemas operacionais) pode provar conforme mais confortável. Por padrão, o controle NumericUpDown sempre aumenta ou diminui um valor 1, mas um serviço web prova mais flexibilidade.


## <a name="overview"></a>Visão geral

Em vez de deixar um usuário digitar um valor em uma caixa de seleção, um numérico para cima/para baixo do controle (o que existe no Windows e outros sistemas operacionais) pode provar conforme mais confortável. Por padrão, o `NumericUpDown` controle sempre aumenta ou diminui um valor 1, mas um serviço web prova mais flexibilidade.

## <a name="steps"></a>Etapas

O ASP.NET AJAX Control Toolkit contém o `NumericUpDown` extensor que adiciona dois botões automaticamente uma caixa de texto: uma para aumentar seu valor, uma para reduzi-lo. No entanto o controle também dá suporte a uma chamada de serviço web (ou chamada de método de página). Sempre que o para cima ou para baixo do botão é clicado, o JavaScript de código conecta-se ao servidor web e executa um método lá. A assinatura do método é mostrada a seguir:

[!code-vb[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample1.vb)]

O `current` argumento é o valor atual na caixa de texto; a `tag` atributo é dados de contexto adicional que podem ser definidos como uma propriedade do `NumericUpDown` extensor (mas não é necessário).

Para este exemplo, o valor numérico para cima/para baixo do controle deve permitir apenas valores que são potências de dois: 1, 2, 4, 8, 16, 32, 64 e assim por diante. Portanto, o método executado quando o usuário deseja aumentar o valor deve duas vezes o valor antigo; o outro método deve dividir o valor por dois. Aqui está o serviço web completo:

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample2.aspx)]

Por fim, crie uma nova página ASP.NET. Como de costume, você precisa de uma `ScriptManager` controle, um `TextBox` controle e um `NumericUpDownExtender` controle. Para o último, você precisará fornecer as informações do serviço web:

- `ServiceDownMethod` nome da busca método web ou método de página
- `ServiceDownPath` caminho para o serviço web com o método de serviço para baixo; omitir se você estiver usando um método de página
- `ServiceUpMethod` nome de cima método web ou método de página
- `ServiceUpPath` caminho para o serviço web com o método de serviço de backup; omitir se você estiver usando um método de página

Aqui está a marcação completa para a página:

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample3.aspx)]

Se você executar a página, observe como o valor na caixa de texto dobra sempre quando você clica no botão superior e é reduzido à metade quando você clica no botão inferior.


[![São exibidos apenas os números sejam uma potência de 2](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image1.png)

São exibidos apenas os números sejam uma potência de 2 ([clique para exibir a imagem em tamanho normal](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](creating-a-numeric-up-down-control-with-a-web-service-backend-cs.md)
