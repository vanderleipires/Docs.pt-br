---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-cs
title: Preenchimento dinamicamente de um controle (c#) | Microsoft Docs
author: wenz
description: "O controle DynamicPopulate no Kit de ferramentas de controle AJAX ASP.NET chama um serviço da web (ou método de página) e preenche o valor resultante em um controle de destino em t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e1fec43e-1daf-49d2-b0c7-7f1b930455cc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: a1868a0e4cec4a95d4175ce255fea2e200692075
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="dynamically-populating-a-control-c"></a>Preenchimento dinamicamente de um controle (c#)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0CS.pdf)

> O controle DynamicPopulate no Kit de ferramentas de controle AJAX ASP.NET chama um serviço da web (ou método de página) e preenche o valor resultante em um controle de destino na página, sem uma atualização de página.


## <a name="overview"></a>Visão Geral

O `DynamicPopulate` controle no Kit de ferramentas de controle AJAX ASP.NET chama um serviço da web (ou método de página) e preenche o valor resultante em um controle de destino na página, sem uma atualização de página. Este tutorial mostra como configurar isso.

## <a name="steps"></a>Etapas

Em primeiro lugar, você precisa de um serviço Web do ASP.NET que implementa o método a ser chamado `DynamicPopulate`. A classe de serviço da web requer o `ScriptService` atributo que é definido em `Microsoft.Web.Script.Services`; caso contrário, o ASP.NET AJAX não é possível criar proxy JavaScript do lado do cliente para o serviço web, que por sua vez, é necessário por `DynamicPopulate`.

O método da web deve esperar que um argumento do tipo cadeia de caracteres chamado `contextKey`, pois o `DynamicPopulate` controle envia um pedaço de informações de contexto com cada chamada de serviço web. O serviço da web a seguir retorna a data atual em um formato representado pelo `contextKey` argumento:

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample1.aspx)]

O serviço da web, em seguida, é salvo como `DynamicPopulate.cs.asmx`. Como alternativa, você pode implementar o `getDate()` método como um método de página dentro da página real do ASP.NET com o `DynamicPopulate` controle.

Na próxima etapa, crie um novo arquivo do ASP.NET. Como sempre, a primeira etapa é incluir o `ScriptManager` na página atual para carregar a biblioteca ASP.NET AJAX e fazer o trabalho do Kit de ferramentas:

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample2.aspx)]

Em seguida, adicione um controle de rótulo (por exemplo, usando o controle HTML de mesmo nome, ou o &lt; `asp:Label`  / &gt; controle web) que mostrará o resultado da chamada de serviço web mais tarde.

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample3.aspx)]

Um botão HTML (como um controle HTML, desde que não exigimos um postback para o servidor) será usado para disparar a população dinâmica:

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample4.aspx)]

Por fim, é necessário o `DynamicPopulateExtender` controle as coisas durante a transmissão. Os seguintes atributos serão definidos (além das óbvias, `ID` e `runat` = `"server"`):

- `TargetControlID`onde colocar o resultado da chamada de serviço web
- `ServicePath`caminho para o serviço da web (omitir se você quiser usar um método de página)
- `ServiceMethod`nome do método da web ou método de página
- `ContextKey`informações de contexto a ser enviado ao serviço web
- `PopulateTriggerControlID`elemento que dispara a chamada de serviço web
- `ClearContentsDuringUpdate`Se deseja esvaziar o elemento de destino durante a chamada de serviço web

Como você pode ver, o controle requer algumas informações mas colocar tudo em vigor é bastante direta. Aqui está a marcação para o `DynamicPopulateExtender` controle no cenário atual:

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample5.aspx)]

Execute a página ASP.NET no navegador e clique no botão; Você receberá a data atual no formato mês / dia / ano.


[![Um clique no botão recupera a data do servidor](dynamically-populating-a-control-cs/_static/image2.png)](dynamically-populating-a-control-cs/_static/image1.png)

Um clique no botão recupera a data do servidor ([clique para exibir a imagem em tamanho normal](dynamically-populating-a-control-cs/_static/image3.png))

>[!div class="step-by-step"]
[Avançar](dynamically-populating-a-control-using-javascript-code-cs.md)
