---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
title: Preenchimento dinâmico de um controle com código de JavaScript (c#) | Microsoft Docs
author: wenz
description: O controle de DynamicPopulate no ASP.NET AJAX Control Toolkit chama um serviço web (ou o método de página) e preenche o valor resultante em um controle de destino em t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: cc4c2def-e88c-4456-ae8b-a6ae0ff8cc2d
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
msc.type: authoredcontent
ms.openlocfilehash: e8b49f41f132cc31ca458ce0af3b74dbb54f225e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833063"
---
<a name="dynamically-populating-a-control-using-javascript-code-c"></a>Preenchimento dinâmico de um controle com código de JavaScript (c#)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1CS.pdf)

> O controle de DynamicPopulate no ASP.NET AJAX Control Toolkit chama um serviço web (ou o método de página) e preenche o valor resultante em um controle de destino na página, sem uma atualização de página. Também é possível disparar a população usando código personalizado de JavaScript do lado do cliente.


## <a name="overview"></a>Visão geral

O `DynamicPopulate` controle no ASP.NET AJAX Control Toolkit chama um serviço web (ou o método de página) e preenche o valor resultante em um controle de destino na página, sem uma atualização de página. Também é possível disparar a população usando código personalizado de JavaScript do lado do cliente.

## <a name="steps"></a>Etapas

Em primeiro lugar, você precisa de um serviço Web do ASP.NET que implementa o método a ser chamado pelo `DynamicPopulateExtender` controle. O serviço da web implementa o método `getDate()` que espera um argumento de cadeia de caracteres de tipo, chamado `contextKey`, uma vez que o `DynamicPopulate` controle envia uma parte das informações de contexto com cada chamada de serviço web. Aqui está o código (arquivo `DynamicPopulate.cs.asmx`) que recupera a data atual em um dos três formatos:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample1.aspx)]

Na próxima etapa, crie um novo site do ASP.NET e iniciar com o controle ScriptManager do AJAX ASP.NET:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample2.aspx)]

Em seguida, adicione um controle de rótulo (por exemplo, usando o controle HTML de mesmo nome, ou o `<asp:Label />` controle da web) mais tarde, que mostrará o resultado da chamada de serviço web.

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample3.aspx)]

Em seguida, inclua um `DynamicPopulateExtender` controlar e fornecer informações do serviço web, o controle de destino, mas não o nome do controle que dispara a população que isso será feito no futuro, usando o JavaScript personalizado!

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample4.aspx)]

Agora à parte de JavaScript. O `$find()` função, definida pela biblioteca do AJAX ASP.NET, retorna uma referência a objetos do lado do servidor do ASP.NET AJAX Control Toolkit, como `DynamicPopulateExtender`. No arquivo atual, `$find("dpe")` retorna uma referência para aquele `DynamicPopulateExtender` controle na página. Ela expõe um método chamado `populate()` que dispara o processo de preenchimento dinâmico. O `populate()` método requer um argumento: a chave de contexto que servirá como argumento para o `getDate()` método web. Por exemplo, `$find("dpe").populate("format1")` preencheria o rótulo com a data atual no formato mês / dia / ano.

Para tornar o exemplo um pouco mais flexível, o usuário pode escolher entre vários formatos de data. Para cada um deles, um botão de opção é exibido. Uma vez o usuário clica em um botão de opção, o código JavaScript preenche dinamicamente o rótulo com o formato da data selecionada. Aqui estão os botões de opção:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample5.aspx)]

Observe que dentro do contexto de um botão de opção, a expressão JavaScript `this.value` refere-se ao valor do botão atual, o que vem a ser exatamente as mesmas informações de `getDate()` método pode trabalhar com.


[![Um clique no botão recupera a data do servidor, no formato especificado](dynamically-populating-a-control-using-javascript-code-cs/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-cs/_static/image1.png)

Um clique no botão recupera a data do servidor, no formato especificado ([clique para exibir a imagem em tamanho normal](dynamically-populating-a-control-using-javascript-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](dynamically-populating-a-control-cs.md)
> [Próximo](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
