---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
title: Preenchendo uma lista usando o CascadingDropDown (VB) | Microsoft Docs
author: wenz
description: O controle CascadingDropDown do AJAX Control Toolkit estende um controle DropDownList, de modo que as alterações em uma carga de DropDownList associado valores em anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 5236695e-5c70-4887-baee-0bfb0afb3448
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: edd6c330d21a7875b8f90fcf9bbde75978b6d08d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389593"
---
<a name="filling-a-list-using-cascadingdropdown-vb"></a>Preenchendo uma lista usando o CascadingDropDown (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0VB.pdf)

> O controle CascadingDropDown do AJAX Control Toolkit estende um controle DropDownList, de modo que as alterações em uma carga de DropDownList associadas a valores em outra DropDownList. (Por exemplo, uma lista fornece uma lista de estados dos EUA e a lista seguinte, em seguida, é preenchida com principais cidades nesse estado.) O primeiro desafio para resolver é, na verdade, preencher uma lista suspensa usando esse controle.


## <a name="overview"></a>Visão geral

O controle CascadingDropDown do AJAX Control Toolkit estende um controle DropDownList, de modo que as alterações em uma carga de DropDownList associadas a valores em outra DropDownList. (Por exemplo, uma lista fornece uma lista de estados dos EUA e a lista seguinte, em seguida, é preenchida com principais cidades nesse estado.) O primeiro desafio para resolver é, na verdade, preencher uma lista suspensa usando esse controle.

## <a name="steps"></a>Etapas

Para ativar a funcionalidade do AJAX ASP.NET e o Kit de ferramentas de controle, o `ScriptManager` controle deve ser colocada em qualquer lugar na página (mas dentro de `<form>` elemento):

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample1.aspx)]

Em seguida, um controle DropDownList é necessário:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample2.aspx)]

Para obter essa lista, um extensor de CascadingDropDown é adicionado. Ele envia uma solicitação assíncrona para um serviço web que, em seguida, retornará uma lista de entradas a ser exibido na lista. Para que isso funcione, os seguintes atributos de CascadingDropDown precisam ser definidas:

- `ServicePath`: A URL de um serviço web fornecendo as entradas da lista
- `ServiceMethod`: Método web fornecer as entradas da lista
- `TargetControlID`: A ID da lista suspensa
- `Category`: Informações de categoria que são enviadas para o método da web quando chamado
- `PromptText`: Texto exibido quando assincronamente Carregando dados da lista do servidor

Aqui está a marcação para o `CascadingDropDown` elemento. A única diferença entre c# e VB é o nome do serviço web associado:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample3.aspx)]

O código JavaScript provenientes de `CascadingDropDown` extensor chama um método de serviço web com a seguinte assinatura:

[!code-vb[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample4.vb)]

Portanto, o aspecto importante é que o método deve retornar uma matriz do tipo `CascadingDropDownNameValue` (definido pelo ASP.NET AJAX Control Toolkit). No `CascadingDropDownNameValue` construtor, primeiro texto da entrada de lista e, em seguida, seu valor devem ser fornecidas, assim como `<option value="VALUE">NAME</option>` faria em HTML. Aqui estão alguns dados de exemplo:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample5.aspx)]

Carregamento da página no navegador disparará a lista a ser preenchido com três fornecedores.


[![A lista é preenchida automaticamente](filling-a-list-using-cascadingdropdown-vb/_static/image2.png)](filling-a-list-using-cascadingdropdown-vb/_static/image1.png)

A lista é preenchida automaticamente ([clique para exibir a imagem em tamanho normal](filling-a-list-using-cascadingdropdown-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](using-auto-postback-with-cascadingdropdown-cs.md)
> [Próximo](using-cascadingdropdown-with-a-database-vb.md)
