---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
title: Permitir somente determinados caracteres em uma caixa de texto (VB) | Microsoft Docs
author: wenz
description: Controles de validação ASP.NET podem garantir que somente determinados caracteres são permitidos em entrada do usuário. No entanto isso ainda não impede que os usuários digitem inválidos...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 33af23f1-4016-4740-8fb2-37d1773452cd
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
msc.type: authoredcontent
ms.openlocfilehash: aec5a3af98cf40e460f4164fb8950e8029002937
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834913"
---
<a name="allowing-only-certain-characters-in-a-text-box-vb"></a>Permitir somente determinados caracteres em uma caixa de texto (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0VB.pdf)

> Controles de validação ASP.NET podem garantir que somente determinados caracteres são permitidos em entrada do usuário. No entanto isso ainda não impedir que os usuários digitando caracteres inválidos e tentar enviar o formulário.


## <a name="overview"></a>Visão geral

Controles de validação ASP.NET podem garantir que somente determinados caracteres são permitidos em entrada do usuário. No entanto isso ainda não impedir que os usuários digitando caracteres inválidos e tentar enviar o formulário.

## <a name="steps"></a>Etapas

O ASP.NET AJAX Control Toolkit contém o `FilteredTextBox` controle que se estende de uma caixa de texto. Uma vez ativado, somente um determinado conjunto de caracteres pode ser inserido no campo.

Para isso funcionar, primeiro precisamos como de costume ASP.NET AJAX `ScriptManager` que carrega as bibliotecas JavaScript que também são usadas pelo ASP.NET AJAX Control Toolkit:

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample1.aspx)]

Em seguida, precisamos de uma caixa de texto:

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample2.aspx)]

Por fim, o `FilteredTextBoxExtender` controle se encarrega de restringir os caracteres que o usuário tem permissão para o tipo. Primeiro, defina as `TargetControlID` de atributo para o `ID` da `TextBox` controle. Em seguida, escolha uma das disponíveis `FilterType` valores:

- `Custom` padrão; Você precisa fornecer uma lista de caracteres válidas
- `LowercaseLetters` apenas letras minúsculas
- `Numbers` somente dígitos
- `UppercaseLetters` somente as letras maiusculas

Se o `Custom FilterType` for usado, o `ValidChars` propriedade deve ser definido e fornecer uma lista de caracteres que podem ser digitados. A propósito: se você tentar colar o texto na caixa de texto, todos os caracteres inválidos são removidos.

Aqui está a marcação para o `FilteredTextBoxExtender` controle que permite somente dígitos (algo que também seria possível com `FilterType="Numbers"`):

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample3.aspx)]

Executar a página e tente digitar uma letra, se o JavaScript estiver habilitado, ele não funcionará; No entanto, dígitos exibidos na página. No entanto, observe que a proteção `FilteredTextBox` fornece não é à prova de marcador: JavaScript se estiver habilitada, todos os dados podem ser inseridos na caixa de texto, portanto, você precisa usar meios de validação adicional, ou seja, o ASP. Controles de validação da rede.


[![Somente dígitos podem ser inseridos.](allowing-only-certain-characters-in-a-text-box-vb/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-vb/_static/image1.png)

Somente dígitos podem ser inseridos ([clique para exibir a imagem em tamanho normal](allowing-only-certain-characters-in-a-text-box-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](allowing-only-certain-characters-in-a-text-box-cs.md)
