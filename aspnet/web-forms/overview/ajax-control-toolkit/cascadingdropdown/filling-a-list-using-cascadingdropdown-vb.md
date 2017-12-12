---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
title: Preenchendo uma lista usando CascadingDropDown (VB) | Microsoft Docs
author: wenz
description: "O controle CascadingDropDown AJAX Control Toolkit estende um controle DropDownList para que as alterações em uma carga de DropDownList associados valores em anoth..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 5236695e-5c70-4887-baee-0bfb0afb3448
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: 1c5cb23be4366365c73ce4774e6a53e452e2a6c0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="filling-a-list-using-cascadingdropdown-vb"></a>Preenchendo uma lista usando CascadingDropDown (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0VB.pdf)

> O controle CascadingDropDown AJAX Control Toolkit estende um controle DropDownList para que as alterações em uma carga de DropDownList associados valores em outra DropDownList. (Por exemplo, uma lista fornece uma lista de nós estados e lista seguinte é então preenchida com cidades nesse estado.) O primeiro desafio para resolver é realmente preencher uma lista suspensa usando o controle.


## <a name="overview"></a>Visão Geral

O controle CascadingDropDown AJAX Control Toolkit estende um controle DropDownList para que as alterações em uma carga de DropDownList associados valores em outra DropDownList. (Por exemplo, uma lista fornece uma lista de nós estados e lista seguinte é então preenchida com cidades nesse estado.) O primeiro desafio para resolver é realmente preencher uma lista suspensa usando o controle.

## <a name="steps"></a>Etapas

Para ativar a funcionalidade do ASP.NET AJAX e o Kit de ferramentas de controle, o `ScriptManager` controle deve ser colocado em qualquer lugar na página (mas dentro do `<form>` elemento):

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample1.aspx)]

Em seguida, um controle DropDownList é necessário:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample2.aspx)]

Para obter essa lista, um extensor de CascadingDropDown é adicionado. Ele envia uma solicitação assíncrona para um serviço web que retornará uma lista de entradas a serem exibidos na lista. Para que isso funcione, os seguintes atributos CascadingDropDown precisam ser definidos:

- `ServicePath`: A URL de um serviço web fornecendo as entradas da lista
- `ServiceMethod`: Método da web fornecendo as entradas da lista
- `TargetControlID`: A ID da lista suspensa
- `Category`: Informações de categoria que são enviadas ao método da web quando chamado
- `PromptText`: Texto exibido quando assincronamente Carregando dados de lista do servidor

Aqui está a marcação para o `CascadingDropDown` elemento. A única diferença entre c# e VB é o nome do serviço da web associada:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample3.aspx)]

O código JavaScript provenientes de `CascadingDropDown` extensor chama um método de serviço web com a seguinte assinatura:

[!code-vb[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample4.vb)]

O aspecto importante é que o método deve retornar uma matriz do tipo `CascadingDropDownNameValue` (definido pelo Kit de ferramentas de controle AJAX ASP.NET). No `CascadingDropDownNameValue` construtor, primeiro texto da entrada da lista e, em seguida, seu valor devem ser fornecidos, assim como `<option value="VALUE">NAME</option>` faria em HTML. Eis aqui alguns dados de exemplo:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample5.aspx)]

Ao carregar a página no navegador disparará a lista a ser preenchida com três fornecedores.


[![A lista é preenchida automaticamente](filling-a-list-using-cascadingdropdown-vb/_static/image2.png)](filling-a-list-using-cascadingdropdown-vb/_static/image1.png)

A lista é preenchida automaticamente ([clique para exibir a imagem em tamanho normal](filling-a-list-using-cascadingdropdown-vb/_static/image3.png))

>[!div class="step-by-step"]
[Anterior](using-auto-postback-with-cascadingdropdown-cs.md)
[Próximo](using-cascadingdropdown-with-a-database-vb.md)
