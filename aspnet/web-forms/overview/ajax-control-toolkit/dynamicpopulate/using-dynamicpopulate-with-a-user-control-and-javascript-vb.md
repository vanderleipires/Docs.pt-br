---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
title: "Usando DynamicPopulate com um controle de usuário e o JavaScript (VB) | Microsoft Docs"
author: wenz
description: "O controle DynamicPopulate no Kit de ferramentas de controle AJAX ASP.NET chama um serviço da web (ou método de página) e preenche o valor resultante em um controle de destino em t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 778b9009-76f2-4665-940e-afc0e35bc917
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
msc.type: authoredcontent
ms.openlocfilehash: 69066edc18b58cc3148a738fe8dd48cb92a84f11
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="using-dynamicpopulate-with-a-user-control-and-javascript-vb"></a>Usando DynamicPopulate com um controle de usuário e o JavaScript (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2VB.pdf)

> O controle DynamicPopulate no Kit de ferramentas de controle AJAX ASP.NET chama um serviço da web (ou método de página) e preenche o valor resultante em um controle de destino na página, sem uma atualização de página. Também é possível disparar a população usando código personalizado de JavaScript do lado do cliente. No entanto, um cuidado especial tem a ser tomada quando o extensor reside em um controle de usuário.


## <a name="overview"></a>Visão Geral

O `DynamicPopulate` controle no Kit de ferramentas de controle AJAX ASP.NET chama um serviço da web (ou método de página) e preenche o valor resultante em um controle de destino na página, sem uma atualização de página. Também é possível disparar a população usando código personalizado de JavaScript do lado do cliente. No entanto, um cuidado especial tem a ser tomada quando o extensor reside em um controle de usuário.

## <a name="steps"></a>Etapas

Em primeiro lugar, você precisa de um serviço Web do ASP.NET que implementa o método a ser chamado `DynamicPopulateExtender` controle. O serviço web implementa o método `getDate()` que espera um argumento do tipo cadeia de caracteres chamado `contextKey`, pois o `DynamicPopulate` controle envia um pedaço de informações de contexto com cada chamada de serviço web. Aqui está o código (arquivos `DynamicPopulate.vb.asmx`) que recupera a data atual em um dos três formatos:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample1.aspx)]

Na próxima etapa, crie um novo controle de usuário (`.ascx` arquivo), indicado pela seguinte declaração em sua primeira linha:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample2.aspx)]

Um &lt; `label` &gt; elemento será usado para exibir os dados provenientes do servidor.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample3.aspx)]

Também no arquivo de controle de usuário, usaremos três botões de opção, cada um representando um dos três formatos de data possível suportados pelo serviço da web. Quando o usuário clica em um dos botões de opção, o navegador executará o código JavaScript que tem esta aparência:

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample4.ps1)]

Este código acessa o `DynamicPopulateExtender` (não se preocupe a ID estranha ainda, isso será abordado mais adiante) e dispara a população dinâmica com dados. No contexto do botão de opção atual, `this.value` refere-se ao seu valor que é `format1`, `format2` ou `format3` exatamente o que o método web espera.

A única coisa ausente no controle de usuário ainda é o `DynamicPopulateExtender` controle que vincula os botões de opção para o serviço web.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample5.aspx)]

Novamente, você pode observar o estranho ID usada no controle: `mcd1$myDate` em vez de `myDate`. Anteriormente, o código JavaScript usado `mcd1_dpe1` para acessar o `DynamicPopulateExtender` em vez de `dpe1`. Essa estratégia de nomenclatura é um requisito especial ao usar `DynamicPopulateExtender` dentro de um controle de usuário. Além disso, você precisa inserir o controle de usuário de uma maneira específica para que tudo funcione. Crie uma nova página ASP.NET e registre um prefixo de marca para o controle de usuário que você implementou:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample6.aspx)]

Em seguida, inclua o ASP.NET AJAX `ScriptManager` controle na nova página:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample7.aspx)]

Finalmente, adicione o controle de usuário para a página. Você só precisa definir seu `ID` atributo (e `runat="server"`, é claro), mas você também tem defini-lo como um nome específico: `mcd1` já que este é o prefixo usado dentro do controle de usuário para acessá-lo usando JavaScript.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample8.aspx)]

E pronto. A página se comporta como esperado: um usuário clica em dos botões de opção, o controle no Kit de ferramentas chama o serviço web e exibe a data atual no formato desejado.


[![Os botões de opção residem em um controle de usuário](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image1.png)

Os botões de opção residem em um controle de usuário ([clique para exibir a imagem em tamanho normal](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image3.png))

>[!div class="step-by-step"]
[Anterior](dynamically-populating-a-control-using-javascript-code-vb.md)
