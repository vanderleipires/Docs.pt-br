---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
title: Uso de Postback automático com CascadingDropDown (VB) | Microsoft Docs
author: wenz
description: O controle CascadingDropDown do AJAX Control Toolkit estende um controle DropDownList, de modo que as alterações em uma carga de DropDownList associado valores em anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0b34f7f6-a0cc-4b9f-9761-643fb0bb3ece
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: 274189184b82734e89a30c9450079d7e07971f3c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378155"
---
<a name="using-auto-postback-with-cascadingdropdown-vb"></a>Uso de Postback automático com CascadingDropDown (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3VB.pdf)

> O controle CascadingDropDown do AJAX Control Toolkit estende um controle DropDownList, de modo que as alterações em uma carga de DropDownList associadas a valores em outra DropDownList. No entanto ao usar o controle CascadingDropDown, ASP. Recurso de AutoPostBack do controle de DropDownList da rede não funciona, pois assincronamente Carregando dados na lista gera um postback (desnecessário) em si. Com um código JavaScript, esse efeito pode ser evitado.


## <a name="overview"></a>Visão geral

O controle CascadingDropDown do AJAX Control Toolkit estende um controle DropDownList, de modo que as alterações em uma carga de DropDownList associadas a valores em outra DropDownList. (Por exemplo, uma lista fornece uma lista de estados dos EUA e a lista seguinte, em seguida, é preenchida com principais cidades nesse estado.) No entanto ao usar o controle CascadingDropDown, ASP. Recurso de AutoPostBack do controle de DropDownList da rede não funciona, pois assincronamente Carregando dados na lista gera um postback (desnecessário) em si. Com um código JavaScript, esse efeito pode ser evitado.

## <a name="steps"></a>Etapas

Para ativar a funcionalidade do AJAX ASP.NET e o Kit de ferramentas de controle, o `ScriptManager` controle deve ser colocada em qualquer lugar na página (mas dentro de &lt; `form` &gt; elemento):

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample1.aspx)]

Em seguida, um controle DropDownList é necessário:

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample2.aspx)]

Para obter essa lista, um extensor de CascadingDropDown é adicionado, fornecendo as informações de URL e o método de serviço web:

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample3.aspx)]

O extensor CascadingDropDown chama assincronamente um serviço web com a assinatura de método a seguir:

[!code-vb[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample4.vb)]

O método retorna uma matriz do tipo valor CascadingDropDown. O construtor do tipo primeiro espera legenda da entrada de lista e, em seguida, o valor (HTML `value` atributo).

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample5.aspx)]

Carregamento da página no navegador preencherá a lista suspensa com três fornecedores, o segundo é que está sendo pré-selecionado. Além disso, o ASP.NET define o `__doPostBack()` método JavaScript. Depois que a página foi carregada, essa chamada de JavaScript é adicionada à lista suspensa, mas somente se houver elementos nele. Se não houver nenhum elemento na lista, o Kit de ferramentas de controle é carregá-los, no momento, para que o código JavaScript usa um tempo limite e tenta novamente em meio segundo.

[!code-html[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample6.html)]

Dessa forma, um postback é executado apenas quando há, na verdade, os elementos na lista e o usuário seleciona uma entrada.


[![Selecionar um elemento de lista faz com que um postback](using-auto-postback-with-cascadingdropdown-vb/_static/image2.png)](using-auto-postback-with-cascadingdropdown-vb/_static/image1.png)

Selecionar um elemento de lista faz com que um postback ([clique para exibir a imagem em tamanho normal](using-auto-postback-with-cascadingdropdown-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](presetting-list-entries-with-cascadingdropdown-vb.md)
