---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
title: Uso de DynamicPopulate com um controle de usuário e o JavaScript (VB) | Microsoft Docs
author: wenz
description: O controle de DynamicPopulate no ASP.NET AJAX Control Toolkit chama um serviço web (ou o método de página) e preenche o valor resultante em um controle de destino em t...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 778b9009-76f2-4665-940e-afc0e35bc917
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
msc.type: authoredcontent
ms.openlocfilehash: c2fe7fbe0d57c6cf64fe31607215c920e67ed736
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841309"
---
<a name="using-dynamicpopulate-with-a-user-control-and-javascript-vb"></a>Uso de DynamicPopulate com um controle de usuário e o JavaScript (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2VB.pdf)

> O controle de DynamicPopulate no ASP.NET AJAX Control Toolkit chama um serviço web (ou o método de página) e preenche o valor resultante em um controle de destino na página, sem uma atualização de página. Também é possível disparar a população usando código personalizado de JavaScript do lado do cliente. No entanto, um cuidado especial tem a ser tomada quando o extensor reside em um controle de usuário.


## <a name="overview"></a>Visão geral

O `DynamicPopulate` controle no ASP.NET AJAX Control Toolkit chama um serviço web (ou o método de página) e preenche o valor resultante em um controle de destino na página, sem uma atualização de página. Também é possível disparar a população usando código personalizado de JavaScript do lado do cliente. No entanto, um cuidado especial tem a ser tomada quando o extensor reside em um controle de usuário.

## <a name="steps"></a>Etapas

Em primeiro lugar, você precisa de um serviço Web do ASP.NET que implementa o método a ser chamado pelo `DynamicPopulateExtender` controle. O serviço da web implementa o método `getDate()` que espera um argumento de cadeia de caracteres de tipo, chamado `contextKey`, uma vez que o `DynamicPopulate` controle envia uma parte das informações de contexto com cada chamada de serviço web. Aqui está o código (arquivos `DynamicPopulate.vb.asmx`) que recupera a data atual em um dos três formatos:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample1.aspx)]

Na próxima etapa, crie um novo controle de usuário (`.ascx` arquivo), indicado pela seguinte declaração em sua primeira linha:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample2.aspx)]

Um &lt; `label` &gt; elemento será usado para exibir os dados provenientes do servidor.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample3.aspx)]

Também no arquivo de controle de usuário, usaremos três botões de opção, cada um representando um dos três formatos de data possível compatíveis com o serviço web. Quando o usuário clica em um dos botões de opção, o navegador executará o código JavaScript que tem esta aparência:

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample4.ps1)]

Esse código acessa a `DynamicPopulateExtender` (não se preocupe sobre a ID estranha ainda, isso será abordado mais adiante) e dispara o preenchimento dinâmico com os dados. No contexto do botão de opção atual `this.value` refere-se ao seu valor que é `format1`, `format2` ou `format3` exatamente o que o método web espera.

A única coisa faltando no controle de usuário ainda é o `DynamicPopulateExtender` controle que vincula os botões de opção para o serviço web.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample5.aspx)]

Novamente, você pode observar a ID estranha usada no controle: `mcd1$myDate` em vez de `myDate`. Anteriormente, o código JavaScript usado `mcd1_dpe1` para acessar o `DynamicPopulateExtender` em vez de `dpe1`. Essa estratégia de nomenclatura é uma necessidade especial ao usar `DynamicPopulateExtender` dentro de um controle de usuário. Além disso, você precisa inserir o controle de usuário de uma maneira específica para fazer tudo funcionar. Criar uma nova página ASP.NET e registrar um prefixo de marca para o controle de usuário que você acabou de implementar:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample6.aspx)]

Em seguida, inclua o AJAX ASP.NET `ScriptManager` controle na nova página:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample7.aspx)]

Por fim, adicione o controle de usuário para a página. Você deve definir seus `ID` atributo (e `runat="server"`, é claro), mas você também precisa defini-lo como um nome específico: `mcd1` como esse é o prefixo usado dentro do controle de usuário para acessá-los usando JavaScript.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample8.aspx)]

E pronto. A página se comporta conforme o esperado: um usuário clica em um dos botões de opção, o controle no Kit de ferramentas chama o serviço web e exibe a data atual no formato desejado.


[![Os botões de opção residem em um controle de usuário](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image1.png)

Os botões de opção residem em um controle de usuário ([clique para exibir a imagem em tamanho normal](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](dynamically-populating-a-control-using-javascript-code-vb.md)
