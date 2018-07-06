---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
title: Preenchimento dinâmico de um controle (VB) | Microsoft Docs
author: wenz
description: O controle de DynamicPopulate no ASP.NET AJAX Control Toolkit chama um serviço web (ou o método de página) e preenche o valor resultante em um controle de destino em t...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 27305347-7b5d-4519-97b7-197a357e7f6e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: d82f8302ddd861531ba517d785a8695d7b914fa0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37806339"
---
<a name="dynamically-populating-a-control-vb"></a>Preenchimento dinâmico de um controle (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0VB.pdf)

> O controle de DynamicPopulate no ASP.NET AJAX Control Toolkit chama um serviço web (ou o método de página) e preenche o valor resultante em um controle de destino na página, sem uma atualização de página.


## <a name="overview"></a>Visão geral

O `DynamicPopulate` controle no ASP.NET AJAX Control Toolkit chama um serviço web (ou o método de página) e preenche o valor resultante em um controle de destino na página, sem uma atualização de página. Este tutorial mostra como configurá-la.

## <a name="steps"></a>Etapas

Em primeiro lugar, você precisa de um serviço Web do ASP.NET que implementa o método a ser chamado pelo `DynamicPopulate`. A classe de serviço da web requer o `ScriptService` atributo que é definido dentro `Microsoft.Web.Script.Services`; caso contrário, o ASP.NET AJAX não é possível criar o proxy de JavaScript do lado do cliente para o serviço web, que por sua vez, é necessário para `DynamicPopulate`.

O método da web deve esperar que um argumento do tipo cadeia de caracteres, chamado `contextKey`, uma vez que o `DynamicPopulate` controle envia uma parte das informações de contexto com cada chamada de serviço web. O serviço web a seguir retorna a data atual em um formato representado pelo `contextKey` argumento:

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample1.aspx)]

O serviço web, em seguida, é salvo como `DynamicPopulate.vb.asmx`. Como alternativa, você pode implementar o `getDate()` método como um método de página dentro da página ASP.NET real com o `DynamicPopulate` controle.

Na próxima etapa, crie um novo arquivo do ASP.NET. Como sempre, a primeira etapa é incluir o `ScriptManager` na página atual ao carregar a biblioteca AJAX do ASP.NET e para fazer o trabalho do Kit de ferramentas de controle:

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample2.aspx)]

Em seguida, adicione um controle de rótulo (por exemplo, usando o controle HTML de mesmo nome, ou o &lt; `asp:Label`  / &gt; controle da web) mais tarde, que mostrará o resultado da chamada de serviço web.

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample3.aspx)]

Um botão HTML (como um controle HTML, pois não exigimos um postback para o servidor), em seguida, será usado para disparar o preenchimento dinâmico:

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample4.aspx)]

Por fim, precisamos de `DynamicPopulateExtender` controle conecte tudo. Os atributos a seguir serão definidos (além do óbvio aqueles, `ID` e `runat` = `"server"`):

- `TargetControlID` onde colocar o resultado da chamada de serviço web
- `ServicePath` caminho para o serviço web (omitir se você quiser usar um método de página)
- `ServiceMethod` nome do método da web ou do método de página
- `ContextKey` informações de contexto a ser enviado ao serviço web
- `PopulateTriggerControlID` elemento que dispara a chamada de serviço web
- `ClearContentsDuringUpdate` Se deseja esvaziar o elemento de destino durante a chamada de serviço web

Como você pode ver, o controle requer algumas informações, mas colocar tudo no lugar é bastante simples. Aqui está a marcação para o `DynamicPopulateExtender` controle no cenário atual:

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample5.aspx)]

Execute a página ASP.NET no navegador e clique no botão; Você receberá a data atual no formato mês / dia / ano.


[![Um clique no botão recupera a data do servidor](dynamically-populating-a-control-vb/_static/image2.png)](dynamically-populating-a-control-vb/_static/image1.png)

Um clique no botão recupera a data do servidor ([clique para exibir a imagem em tamanho normal](dynamically-populating-a-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
> [Próximo](dynamically-populating-a-control-using-javascript-code-vb.md)
