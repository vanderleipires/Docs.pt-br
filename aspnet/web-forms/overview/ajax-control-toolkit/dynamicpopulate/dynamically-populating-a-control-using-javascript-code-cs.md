---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
title: "Preenchendo dinamicamente um controle com o código JavaScript (c#) | Microsoft Docs"
author: wenz
description: "O controle DynamicPopulate no Kit de ferramentas de controle AJAX ASP.NET chama um serviço da web (ou método de página) e preenche o valor resultante em um controle de destino em t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: cc4c2def-e88c-4456-ae8b-a6ae0ff8cc2d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 5b3b23e16f2e4dd26f50eb3e07f35d078dd19a19
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="dynamically-populating-a-control-using-javascript-code-c"></a>Preenchendo dinamicamente um controle com o código JavaScript (c#)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1CS.pdf)

> O controle DynamicPopulate no Kit de ferramentas de controle AJAX ASP.NET chama um serviço da web (ou método de página) e preenche o valor resultante em um controle de destino na página, sem uma atualização de página. Também é possível disparar a população usando código personalizado de JavaScript do lado do cliente.


## <a name="overview"></a>Visão Geral

O `DynamicPopulate` controle no Kit de ferramentas de controle AJAX ASP.NET chama um serviço da web (ou método de página) e preenche o valor resultante em um controle de destino na página, sem uma atualização de página. Também é possível disparar a população usando código personalizado de JavaScript do lado do cliente.

## <a name="steps"></a>Etapas

Em primeiro lugar, você precisa de um serviço Web do ASP.NET que implementa o método a ser chamado `DynamicPopulateExtender` controle. O serviço web implementa o método `getDate()` que espera um argumento do tipo cadeia de caracteres chamado `contextKey`, pois o `DynamicPopulate` controle envia um pedaço de informações de contexto com cada chamada de serviço web. Aqui está o código (arquivo `DynamicPopulate.cs.asmx`) que recupera a data atual em um dos três formatos:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample1.aspx)]

Na próxima etapa, crie um novo site do ASP.NET e começar com o ASP.NET AJAX ScriptManager:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample2.aspx)]

Em seguida, adicione um controle de rótulo (por exemplo, usando o controle HTML de mesmo nome, ou o `<asp:Label />` controle web) que mostrará o resultado da chamada de serviço web mais tarde.

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample3.aspx)]

Em seguida, inclua um `DynamicPopulateExtender` controlar e fornecer informações do serviço web, o controle de destino, mas não o nome do controle que dispara a população isso será feito posteriormente em, usando JavaScript personalizado!

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample4.aspx)]

Como parte de JavaScript. O `$find()` função, definida pela biblioteca ASP.NET AJAX, retorna uma referência a objetos do servidor do Kit de ferramentas de controle AJAX ASP.NET, como `DynamicPopulateExtender`. O arquivo atual, `$find("dpe")` retorna uma referência ao `DynamicPopulateExtender` controle na página. Expõe um método chamado `populate()` que aciona o processo de população dinâmico. O `populate()` método exige um argumento: a chave de contexto que servirá como argumento para o `getDate()` método web. Portanto, por exemplo, `$find("dpe").populate("format1")` preenche o rótulo com a data atual no formato mês / dia / ano.

Para tornar o exemplo um pouco mais flexível, o usuário pode escolher entre vários formatos de data. Para cada um deles, um botão de opção é exibido. Uma vez o usuário clica em um botão de opção, o código JavaScript preenche dinamicamente a etiqueta com o formato de data selecionado. Aqui estão os botões de opção:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample5.aspx)]

Observe que dentro do contexto de um botão de opção, a expressão JavaScript `this.value` se refere ao valor do botão atual, que é exatamente a mesma informação de `getDate()` método pode trabalhar com.


[![Um clique no botão recupera a data do servidor, no formato especificado](dynamically-populating-a-control-using-javascript-code-cs/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-cs/_static/image1.png)

Um clique no botão recupera a data do servidor, no formato especificado ([clique para exibir a imagem em tamanho normal](dynamically-populating-a-control-using-javascript-code-cs/_static/image3.png))

>[!div class="step-by-step"]
[Anterior](dynamically-populating-a-control-cs.md)
[Próximo](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
