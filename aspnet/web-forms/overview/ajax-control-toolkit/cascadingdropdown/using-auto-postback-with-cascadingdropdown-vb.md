---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
title: Usando Postback automático com CascadingDropDown (VB) | Microsoft Docs
author: wenz
description: O controle CascadingDropDown AJAX Control Toolkit estende um controle DropDownList para que as alterações em uma carga de DropDownList associados valores em anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0b34f7f6-a0cc-4b9f-9761-643fb0bb3ece
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: e8a48692dd96ee6a647bfb57ce2915b4e85544fe
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="using-auto-postback-with-cascadingdropdown-vb"></a>Usando Postback automático com CascadingDropDown (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3VB.pdf)

> O controle CascadingDropDown AJAX Control Toolkit estende um controle DropDownList para que as alterações em uma carga de DropDownList associados valores em outra DropDownList. No entanto ao usar o controle CascadingDropDown, ASP. Recurso de AutoPostBack do controle de DropDownList do NET não funcionará, pois assincronamente carregar dados para a lista gera um postback (desnecessário) em si. Com um código JavaScript, esse efeito pode ser evitado.


## <a name="overview"></a>Visão geral

O controle CascadingDropDown AJAX Control Toolkit estende um controle DropDownList para que as alterações em uma carga de DropDownList associados valores em outra DropDownList. (Por exemplo, uma lista fornece uma lista de nós estados e lista seguinte é então preenchida com cidades nesse estado.) No entanto ao usar o controle CascadingDropDown, ASP. Recurso de AutoPostBack do controle de DropDownList do NET não funcionará, pois assincronamente carregar dados para a lista gera um postback (desnecessário) em si. Com um código JavaScript, esse efeito pode ser evitado.

## <a name="steps"></a>Etapas

Para ativar a funcionalidade do ASP.NET AJAX e o Kit de ferramentas de controle, o `ScriptManager` controle deve ser colocado em qualquer lugar na página (mas dentro do &lt; `form` &gt; elemento):

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample1.aspx)]

Em seguida, um controle DropDownList é necessário:

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample2.aspx)]

Para esta lista é adicionado um extensor CascadingDropDown, fornecendo as informações de URL e o método de serviço web:

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample3.aspx)]

O extensor CascadingDropDown chama assincronamente um serviço web com a assinatura de método a seguir:

[!code-vb[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample4.vb)]

O método retorna uma matriz de tipo de valor de CascadingDropDown. O construtor do tipo primeiro espera legenda da entrada da lista e, em seguida, o valor (HTML `value` atributo).

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample5.aspx)]

Ao carregar a página no navegador preencher a lista suspensa com três fornecedores, o outro sendo pré-selecionados. Além disso, o ASP.NET define o `__doPostBack()` método JavaScript. Depois que a página é carregada, essa chamada JavaScript é adicionada à lista suspensa, mas somente se houver elementos nela. Se não houver nenhum elemento na lista, o Kit de ferramentas do controle é carregá-los, no momento para que o código JavaScript usa um tempo limite e tenta novamente dentro de meio segundo.

[!code-html[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample6.html)]

Dessa forma, um postback é executado apenas quando há realmente elementos na lista e o usuário seleciona uma entrada.


[![Selecionar um elemento da lista causa um postback](using-auto-postback-with-cascadingdropdown-vb/_static/image2.png)](using-auto-postback-with-cascadingdropdown-vb/_static/image1.png)

Selecionar um elemento da lista causa um postback ([clique para exibir a imagem em tamanho normal](using-auto-postback-with-cascadingdropdown-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](presetting-list-entries-with-cascadingdropdown-vb.md)
