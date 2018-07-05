---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
title: Predefinição de entradas de lista com CascadingDropDown (VB) | Microsoft Docs
author: wenz
description: O controle CascadingDropDown do AJAX Control Toolkit estende um controle DropDownList, de modo que as alterações em uma carga de DropDownList associado valores em anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ec61ced7-bbca-4bdd-aa3b-80878f295181
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: 2361d12aa66db55dacd7e034306dcbbda21570b8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37397607"
---
<a name="presetting-list-entries-with-cascadingdropdown-vb"></a>Predefinição de entradas de lista com CascadingDropDown (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/CascadingDropDown2VB.pdf)

> O controle CascadingDropDown do AJAX Control Toolkit estende um controle DropDownList, de modo que as alterações em uma carga de DropDownList associadas a valores em outra DropDownList. Com um pouco de código, é possível que um elemento de lista é pré-selecionado após os dados serem carregados dinamicamente.


## <a name="overview"></a>Visão geral

O controle CascadingDropDown do AJAX Control Toolkit estende um controle DropDownList, de modo que as alterações em uma carga de DropDownList associadas a valores em outra DropDownList. (Por exemplo, uma lista fornece uma lista de estados dos EUA e a lista seguinte, em seguida, é preenchida com principais cidades nesse estado.) Com um pouco de código, é possível que um elemento de lista é pré-selecionado após os dados serem carregados dinamicamente.

## <a name="steps"></a>Etapas

Para ativar a funcionalidade do AJAX ASP.NET e o Kit de ferramentas de controle, o `ScriptManager` controle deve ser colocada em qualquer lugar na página (mas dentro de `<form>` elemento):

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample1.aspx)]

Em seguida, um controle DropDownList é necessário:

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample2.aspx)]

Para obter essa lista, um extensor de CascadingDropDown é adicionado, fornecendo as informações de URL e o método de serviço web:

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample3.aspx)]

O extensor CascadingDropDown chama assincronamente um serviço web com a assinatura de método a seguir:

[!code-vb[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample4.vb)]

O método retorna uma matriz do tipo valor CascadingDropDown. O construtor do tipo primeiro espera legenda da entrada de lista e, em seguida, o valor (HTML `value` atributo). Se o terceiro argumento é definido como true, a lista de elemento é selecionado automaticamente no navegador.

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample5.aspx)]

Carregamento da página no navegador preencherá a lista suspensa com três fornecedores, o segundo é que está sendo pré-selecionado.


[![A lista é preenchida e pré-selecionado automaticamente](presetting-list-entries-with-cascadingdropdown-vb/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-vb/_static/image1.png)

A lista é preenchida e pré-selecionado automaticamente ([clique para exibir a imagem em tamanho normal](presetting-list-entries-with-cascadingdropdown-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](using-cascadingdropdown-with-a-database-vb.md)
> [Próximo](using-auto-postback-with-cascadingdropdown-vb.md)
