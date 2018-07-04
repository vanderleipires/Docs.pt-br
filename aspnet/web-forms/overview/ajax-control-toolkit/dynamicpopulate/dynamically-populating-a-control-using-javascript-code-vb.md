---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-vb
title: Preenchimento dinâmico de um controle usando código JavaScript (VB) | Microsoft Docs
author: wenz
description: O controle de DynamicPopulate no ASP.NET AJAX Control Toolkit chama um serviço web (ou o método de página) e preenche o valor resultante em um controle de destino em t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 90582e54-3e90-432a-9da5-689fb39ed56b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 9df2a66bd49ecba52b0dd8b1d52a65b36c38a5dc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366841"
---
<a name="dynamically-populating-a-control-using-javascript-code-vb"></a>Preenchimento dinâmico de um controle usando código JavaScript (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1VB.pdf)

> O controle de DynamicPopulate no ASP.NET AJAX Control Toolkit chama um serviço web (ou o método de página) e preenche o valor resultante em um controle de destino na página, sem uma atualização de página. Também é possível disparar a população usando código personalizado de JavaScript do lado do cliente.


## <a name="overview"></a>Visão geral

O `DynamicPopulate` controle no ASP.NET AJAX Control Toolkit chama um serviço web (ou o método de página) e preenche o valor resultante em um controle de destino na página, sem uma atualização de página. Também é possível disparar a população usando código personalizado de JavaScript do lado do cliente.

## <a name="steps"></a>Etapas

Em primeiro lugar, você precisa de um serviço Web do ASP.NET que implementa o método a ser chamado pelo `DynamicPopulateExtender` controle. O serviço da web implementa o método `getDate()` que espera um argumento de cadeia de caracteres de tipo, chamado `contextKey`, uma vez que o `DynamicPopulate` controle envia uma parte das informações de contexto com cada chamada de serviço web. Aqui está o código (arquivo `DynamicPopulate.vb.asmx`) que recupera a data atual em um dos três formatos:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample1.aspx)]

Na próxima etapa, crie um novo site do ASP.NET e iniciar com o controle ScriptManager do AJAX ASP.NET:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample2.aspx)]

Em seguida, adicione um controle de rótulo (por exemplo, usando o controle HTML de mesmo nome, ou o `<asp:Label />` controle da web) mais tarde, que mostrará o resultado da chamada de serviço web.

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample3.aspx)]

Em seguida, inclua um `DynamicPopulateExtender` controlar e fornecer informações do serviço web, o controle de destino, mas não o nome do controle que dispara a população que isso será feito no futuro, usando o JavaScript personalizado!

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample4.aspx)]

Agora à parte de JavaScript. O `$find()` função, definida pela biblioteca do AJAX ASP.NET, retorna uma referência a objetos do lado do servidor do ASP.NET AJAX Control Toolkit, como `DynamicPopulateExtender`. No arquivo atual, `$find("dpe")` retorna uma referência para aquele `DynamicPopulateExtender` controle na página. Ela expõe um método chamado `populate()` que dispara o processo de preenchimento dinâmico. O `populate()` método requer um argumento: a chave de contexto que servirá como argumento para o `getDate()` método web. Por exemplo, `$find("dpe").populate("format1")` preencheria o rótulo com a data atual no formato mês / dia / ano.

Para tornar o exemplo um pouco mais flexível, o usuário pode escolher entre vários formatos de data. Para cada um deles, um botão de opção é exibido. Uma vez o usuário clica em um botão de opção, o código JavaScript preenche dinamicamente o rótulo com o formato da data selecionada. Aqui estão os botões de opção:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample5.aspx)]

Observe que dentro do contexto de um botão de opção, a expressão JavaScript `this.value` refere-se ao valor do botão atual, o que vem a ser exatamente as mesmas informações de `getDate()` método pode trabalhar com.


[![Um clique no botão recupera a data do servidor, no formato especificado](dynamically-populating-a-control-using-javascript-code-vb/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-vb/_static/image1.png)

Um clique no botão recupera a data do servidor, no formato especificado ([clique para exibir a imagem em tamanho normal](dynamically-populating-a-control-using-javascript-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](dynamically-populating-a-control-vb.md)
> [Próximo](using-dynamicpopulate-with-a-user-control-and-javascript-vb.md)
