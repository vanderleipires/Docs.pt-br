---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
title: Executar animações usando o código do lado do cliente (c#) | Microsoft Docs
author: wenz
description: O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. A execução de animação...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0270e0df-6fde-4a8f-a2cb-2cacc55143f2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: cfaed745b4c547d04033ee89d2c6549c5989cb73
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="executing-animations-using-client-side-code-c"></a>Executar animações usando o código do lado do cliente (c#)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10CS.pdf)

> O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. A execução da animação também pode ser disparada usando código personalizado de JavaScript do lado do cliente.


## <a name="overview"></a>Visão geral

O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. A execução da animação também pode ser disparada usando código personalizado de JavaScript do lado do cliente.

## <a name="steps"></a>Etapas

Em primeiro lugar, incluem o `ScriptManager` na página; em seguida, a biblioteca ASP.NET AJAX é carregada, tornando possível usar o Kit de ferramentas:

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample1.aspx)]

A animação será aplicada a um painel de texto que tem esta aparência:

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample2.aspx)]

Na classe CSS associada para o painel, definir uma cor de plano de fundo adequado e também uma largura fixa para o painel:

[!code-css[Main](executing-animations-using-client-side-code-cs/samples/sample3.css)]

Em seguida, adicione o `AnimationExtender` para a página, fornecendo um `ID`, o `TargetControlID` atributo e o obrigatórias `runat="server"`:

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample4.aspx)]

Dentro de `<Animations>` nó, use `<OnClick>` executar as animações uma vez, o usuário clica no painel. Adicione duas animações a serem executados parallelly:

[!code-xml[Main](executing-animations-using-client-side-code-cs/samples/sample5.xml)]

Para fins de demonstração, essa animação (e qualquer outra animação criado usando o Kit de ferramentas de controle) são executados usando o código JavaScript, depois que a página é executada. Em primeiro lugar, precisamos acessem o `AnimationExtender` controle. A biblioteca ASP.NET AJAX fornece o `$find()` função para esta tarefa:

[!code-csharp[Main](executing-animations-using-client-side-code-cs/samples/sample6.cs)]

O `AnimationExtender` controle expõe uma API avançada, incluindo métodos com nomes idênticos aos manipuladores de eventos usados na marcação XML: `OnClick()`, `OnLoad()`e assim por diante. Por exemplo, uma chamada do `OnClick()` método executa a animação dentro a `<OnClick>` elemento do `AnimationExtender` controle:

[!code-javascript[Main](executing-animations-using-client-side-code-cs/samples/sample7.js)]

Aqui está o código completo de JavaScript do lado do cliente que emula o clique no painel depois que a página foi totalmente carregada Observe que o `pageLoad()` nome da função é usado que é chamado pelo ASP.NET AJAX uma vez a página e todos os incluídos JavaScript bibliotecas foram carregado.

[!code-html[Main](executing-animations-using-client-side-code-cs/samples/sample8.html)]


[![A animação é executada imediatamente, sem um clique do mouse](executing-animations-using-client-side-code-cs/_static/image2.png)](executing-animations-using-client-side-code-cs/_static/image1.png)

A animação é executada imediatamente, sem um clique do mouse ([clique para exibir a imagem em tamanho normal](executing-animations-using-client-side-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](modifying-animations-from-the-server-side-cs.md)
> [Próximo](changing-an-animation-using-client-side-code-cs.md)
