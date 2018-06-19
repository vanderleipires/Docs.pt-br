---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
title: Preencher dinamicamente um controle (VB) | Microsoft Docs
author: wenz
description: O controle DynamicPopulate no Kit de ferramentas de controle AJAX ASP.NET chama um serviço da web (ou método de página) e preenche o valor resultante em um controle de destino em t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 27305347-7b5d-4519-97b7-197a357e7f6e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: e2031a80be71a406e632955583d83920dd0f3ef7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879732"
---
<a name="dynamically-populating-a-control-vb"></a>Preencher dinamicamente um controle (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0VB.pdf)

> O controle DynamicPopulate no Kit de ferramentas de controle AJAX ASP.NET chama um serviço da web (ou método de página) e preenche o valor resultante em um controle de destino na página, sem uma atualização de página.


## <a name="overview"></a>Visão Geral

O `DynamicPopulate` controle no Kit de ferramentas de controle AJAX ASP.NET chama um serviço da web (ou método de página) e preenche o valor resultante em um controle de destino na página, sem uma atualização de página. Este tutorial mostra como configurar isso.

## <a name="steps"></a>Etapas

Em primeiro lugar, você precisa de um serviço Web do ASP.NET que implementa o método a ser chamado `DynamicPopulate`. A classe de serviço da web requer o `ScriptService` atributo que é definido em `Microsoft.Web.Script.Services`; caso contrário, o ASP.NET AJAX não é possível criar proxy JavaScript do lado do cliente para o serviço web, que por sua vez, é necessário por `DynamicPopulate`.

O método da web deve esperar que um argumento do tipo cadeia de caracteres chamado `contextKey`, pois o `DynamicPopulate` controle envia um pedaço de informações de contexto com cada chamada de serviço web. O serviço da web a seguir retorna a data atual em um formato representado pelo `contextKey` argumento:

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample1.aspx)]

O serviço da web, em seguida, é salvo como `DynamicPopulate.vb.asmx`. Como alternativa, você pode implementar o `getDate()` método como um método de página dentro da página real do ASP.NET com o `DynamicPopulate` controle.

Na próxima etapa, crie um novo arquivo do ASP.NET. Como sempre, a primeira etapa é incluir o `ScriptManager` na página atual para carregar a biblioteca ASP.NET AJAX e fazer o trabalho do Kit de ferramentas:

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample2.aspx)]

Em seguida, adicione um controle de rótulo (por exemplo, usando o controle HTML de mesmo nome, ou o &lt; `asp:Label`  / &gt; controle web) que mostrará o resultado da chamada de serviço web mais tarde.

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample3.aspx)]

Um botão HTML (como um controle HTML, desde que não exigimos um postback para o servidor) será usado para disparar a população dinâmica:

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample4.aspx)]

Por fim, é necessário o `DynamicPopulateExtender` controle as coisas durante a transmissão. Os seguintes atributos serão definidos (além das óbvias, `ID` e `runat` = `"server"`):

- `TargetControlID` onde colocar o resultado da chamada de serviço web
- `ServicePath` caminho para o serviço da web (omitir se você quiser usar um método de página)
- `ServiceMethod` nome do método da web ou método de página
- `ContextKey` informações de contexto a ser enviado ao serviço web
- `PopulateTriggerControlID` elemento que dispara a chamada de serviço web
- `ClearContentsDuringUpdate` Se deseja esvaziar o elemento de destino durante a chamada de serviço web

Como você pode ver, o controle requer algumas informações mas colocar tudo em vigor é bastante direta. Aqui está a marcação para o `DynamicPopulateExtender` controle no cenário atual:

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample5.aspx)]

Execute a página ASP.NET no navegador e clique no botão; Você receberá a data atual no formato mês / dia / ano.


[![Um clique no botão recupera a data do servidor](dynamically-populating-a-control-vb/_static/image2.png)](dynamically-populating-a-control-vb/_static/image1.png)

Um clique no botão recupera a data do servidor ([clique para exibir a imagem em tamanho normal](dynamically-populating-a-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
> [Próximo](dynamically-populating-a-control-using-javascript-code-vb.md)
