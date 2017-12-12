---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
title: "Criando um numérico para cima/baixo controle com um serviço Web de Backend (VB) | Microsoft Docs"
author: wenz
description: "Em vez de permitir que um usuário digitar um valor em uma caixa de seleção, um numérico para cima/baixo controle (que existe no Windows e outros sistemas operacionais) poderá ser mais como c..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: afa59dfa-fef1-43d3-8fdd-aea3be36ed3c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
msc.type: authoredcontent
ms.openlocfilehash: 5ceefd6c18761c2abe3f3a4298d340642a0951d6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-numeric-updown-control-with-a-web-service-backend-vb"></a>Criando um controle para cima/baixo numérico com um serviço Web de Backend (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1VB.pdf)

> Em vez de permitir que um usuário digitar um valor em uma caixa de seleção, um numérico para cima/baixo controle (que existe no Windows e outros sistemas operacionais) pode revelar conforme mais confortável. Por padrão, o controle NumericUpDown sempre aumenta ou diminui um valor 1, mas um serviço web prova mais flexibilidade.


## <a name="overview"></a>Visão Geral

Em vez de permitir que um usuário digitar um valor em uma caixa de seleção, um numérico para cima/baixo controle (que existe no Windows e outros sistemas operacionais) pode revelar conforme mais confortável. Por padrão, o `NumericUpDown` controle sempre aumenta ou diminui um valor 1, mas um serviço web prova mais flexibilidade.

## <a name="steps"></a>Etapas

O Kit de ferramentas de controle do ASP.NET AJAX contém o `NumericUpDown` extensor que adiciona dois botões automaticamente uma caixa de texto: uma para aumentar seu valor, uma para reduzi-lo. No entanto o controle também oferece suporte a uma chamada de serviço web (ou chamada de método de página). Sempre que o para cima ou para baixo do botão é clicado, o JavaScript código conecta-se ao servidor web e executa um método existe. A assinatura do método é o seguinte:

[!code-vb[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample1.vb)]

O `current` argumento é o valor atual na caixa de texto; a `tag` atributo é dados de contexto adicional que podem ser definidos como uma propriedade do `NumericUpDown` extensor (mas não é necessário).

Para este exemplo, o valor numérico para cima/baixo controle só deve permitir valores que são potências de dois: 1, 2, 4, 8, 16, 32, 64 e assim por diante. Portanto, o método executado quando o usuário deseja aumentar o valor deve dobrar o valor antigo; o outro método deve dividir valor por dois. Aqui está o serviço web completa:

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample2.aspx)]

Finalmente, crie uma nova página ASP.NET. Normalmente, é necessário um `ScriptManager` controle, uma `TextBox` controle e um `NumericUpDownExtender` controle. Para o último, você precisa fornecer as informações do serviço web:

- `ServiceDownMethod`nome do suspenso método web ou método de página
- `ServiceDownPath`caminho para o serviço da web com o método de serviço para baixo; omitir se você estiver usando um método de página
- `ServiceUpMethod`nome de cima método web ou método de página
- `ServiceUpPath`caminho para o serviço da web com o método de serviço de backup; omitir se você estiver usando um método de página

Aqui está a marcação concluída para a página:

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample3.aspx)]

Se você executar a página, observe como o valor na caixa de texto dobra sempre quando você clicar no botão superior e é reduzido à metade quando você clica no botão inferior.


[![Somente os números são uma potência de 2 aparecem](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image1.png)

Somente os números são uma potência de 2 aparecem ([clique para exibir a imagem em tamanho normal](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image3.png))

>[!div class="step-by-step"]
[Anterior](creating-a-numeric-up-down-control-with-a-web-service-backend-cs.md)
