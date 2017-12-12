---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-cs
title: "Predefinição de entradas da lista com CascadingDropDown (c#) | Microsoft Docs"
author: wenz
description: "O controle CascadingDropDown AJAX Control Toolkit estende um controle DropDownList para que as alterações em uma carga de DropDownList associados valores em anoth..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 04c79748-0f21-4a3b-aba5-e1ce3161c32e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: d86c5887a8b175a60c32a0970482266b7d690162
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="presetting-list-entries-with-cascadingdropdown-c"></a>Predefinição de entradas da lista com CascadingDropDown (c#)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingDropDown2CS.pdf)

> O controle CascadingDropDown AJAX Control Toolkit estende um controle DropDownList para que as alterações em uma carga de DropDownList associados valores em outra DropDownList. Com um pouco de código, é possível que um elemento de lista é pré-selecionado depois que os dados forem carregados dinamicamente.


## <a name="overview"></a>Visão Geral

O controle CascadingDropDown AJAX Control Toolkit estende um controle DropDownList para que as alterações em uma carga de DropDownList associados valores em outra DropDownList. (Por exemplo, uma lista fornece uma lista de nós estados e lista seguinte é então preenchida com cidades nesse estado.) Com um pouco de código, é possível que um elemento de lista é pré-selecionado depois que os dados forem carregados dinamicamente.

## <a name="steps"></a>Etapas

Para ativar a funcionalidade do ASP.NET AJAX e o Kit de ferramentas de controle, o `ScriptManager` controle deve ser colocado em qualquer lugar na página (mas dentro do `<form>` elemento):

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample1.aspx)]

Em seguida, um controle DropDownList é necessário:

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample2.aspx)]

Para esta lista é adicionado um extensor CascadingDropDown, fornecendo as informações de URL e o método de serviço web:

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample3.aspx)]

O extensor CascadingDropDown chama assincronamente um serviço web com a assinatura de método a seguir:

[!code-csharp[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample4.cs)]

O método retorna uma matriz de tipo de valor de CascadingDropDown. O construtor do tipo primeiro espera legenda da entrada da lista e, em seguida, o valor (HTML `value` atributo). Se o terceiro argumento é definido como true, a lista de elemento é selecionado automaticamente no navegador.

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample5.aspx)]

Ao carregar a página no navegador preencher a lista suspensa com três fornecedores, o outro sendo pré-selecionados.


[![A lista é preenchida e pré-selecionados automaticamente](presetting-list-entries-with-cascadingdropdown-cs/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-cs/_static/image1.png)

A lista é preenchida e automaticamente pré-selecionados ([clique para exibir a imagem em tamanho normal](presetting-list-entries-with-cascadingdropdown-cs/_static/image3.png))

>[!div class="step-by-step"]
[Anterior](using-cascadingdropdown-with-a-database-cs.md)
[Próximo](using-auto-postback-with-cascadingdropdown-cs.md)
