---
uid: web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-vb
title: Usando controles de kit de ferramentas de controle AJAX e extensores de controle (VB) | Microsoft Docs
author: microsoft
description: "Saiba como adicionar controles AJAX Control Toolkit e extensores para suas páginas ASP.NET."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 763650a9-ffde-46a9-b779-7a9145dd5d88
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-vb
msc.type: authoredcontent
ms.openlocfilehash: 7b248855a1b82f3e8f172b439ee36502f95a39ca
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="using-ajax-control-toolkit-controls-and-control-extenders-vb"></a>Usando controles de kit de ferramentas de controle AJAX e extensores de controle (VB)
====================
por [Microsoft](https://github.com/microsoft)

> Saiba como adicionar controles AJAX Control Toolkit e extensores para suas páginas ASP.NET.


O Kit de ferramentas de controle AJAX contém um conjunto de controles e extensores de controle. Este breve tutorial, você aprenderá como adicionar controles e extensores de controle a uma página ASP.NET.

> [!NOTE] 
> 
> Para obter instruções sobre como instalar o Kit de ferramentas de controle AJAX e adicionando o Kit de ferramentas de controle AJAX para a caixa de ferramentas do Visual Studio/Visual Web Developer, consulte o tutorial [começar com o Kit de ferramentas de controle AJAX](get-started-with-the-ajax-control-toolkit-vb.md).


## <a name="using-ajax-control-toolkit-controls"></a>Usando controles de kit de ferramentas de controle AJAX

Um controle AJAX Control Toolkit funciona como um controle normal do ASP.NET. Você pode arrastar o controle da caixa de ferramentas em uma página ASP.NET. Você pode adicionar o controle para a página no modo Design ou exibição da fonte.

Há um requisito especial ao usar os controles do Kit de ferramentas de controle AJAX. A página deve conter um controle ScriptManager. O controle ScriptManager é responsável por incluindo todos o JavaScript necessário necessário para os controles do Kit de ferramentas de controle AJAX.

Por exemplo, a guia de AJAX Control Toolkit inclui um controle chamado o controle de Editor. Esse controle exibe um poderoso editor de HTML. Siga estas etapas para adicionar o controle de Editor para uma página:

1. Criar uma nova página ASP.NET chamada ShowEditor.aspx
2. Selecione o controle ScriptManager do guia Extensões AJAX na caixa de ferramentas e arraste o controle para a página.
3. Selecione o controle de Editor do guia do Kit de ferramentas de controle AJAX na caixa de ferramentas e arraste o controle para a página (consulte a Figura 1). O Designer deve parecer com a Figura 2.
4. Executar o site selecionando a opção de menu **depurar, iniciar depuração** ou pressionar a tecla F5.
5. Você verá a página na Figura 3.


[![Selecionando o controle de Editor de HTML](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.png)

**Figura 01**: selecionando o controle de Editor de HTML ([clique para exibir a imagem em tamanho normal](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.png))


[![Designer do Visual Studio com o controle ScriptManager e editar](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.png)

**Figura 02**: Visual Studio Designer com controle ScriptManager e editar ([clique para exibir a imagem em tamanho normal](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.png))


[![A página DisplayEditor.aspx](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.png)

**Figura 03**: DisplayEditor.aspx a página ([clique para exibir a imagem em tamanho normal](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.png))


## <a name="using-ajax-control-toolkit-control-extenders"></a>Usando extensores de controle do Kit de ferramentas de controle AJAX

O Kit de ferramentas de controle AJAX também contém os Extensores do controle. Como o nome sugere, um extensor de controle estende a funcionalidade de um controle existente. Por exemplo, o extensor do controle ConfirmButton estende o controle de botão do ASP.NET padrão. O extensor altera o comportamento do botão controle s para que o botão exibe uma caixa de diálogo de confirmação quando você clica nele.

Um extensor de controle, assim como um controle AJAX Control Toolkit, requer um controle ScriptManager. Você deve adicionar um controle ScriptManager para uma página para começar a usar os Extensores do controle na página.

Siga estas etapas para usar o extensor do controle ConfirmButton:

1. Criar uma nova página ASP.NET chamada ShowConfirmButton.aspx
2. Adicione um controle ScriptManager à página, arrastando o controle para a página de guia Extensões AJAX.
3. Adicione um controle de botão padrão para a página arrastando o botão de guia padrão na caixa de ferramentas para a superfície de Designer.
4. Clique o **adicionar extensor** opção da tarefa (consulte a Figura 4).
5. Na caixa de diálogo Escolher extensor, selecione ConfirmButtonExtender (consulte a Figura 5) e clique no botão Okey.
6. Selecione o controle de botão no Designer e expanda os extensores, Button1\_ConfirmButtonExtender nó na janela Propriedades (consulte a Figura 6). Atribuir o valor *realmente?* para a propriedade ConfirmText.
7. Execute a página, selecionando a opção de menu **depurar, iniciar depuração** ou pressione a tecla F5.


[![A opção de adicionar extensor de tarefas](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.png)

**Figura 04**: opção de tarefas a adicionar extensor ([clique para exibir a imagem em tamanho normal](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image8.png))


[![Selecionando o extensor do controle ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image9.png)

**Figura 05**: selecionando o extensor do controle ConfirmButton ([clique para exibir a imagem em tamanho normal](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image10.png))


[![Definir uma propriedade ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image11.png)

**Figura 06**: definir uma propriedade ConfirmButton ([clique para exibir a imagem em tamanho normal](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image12.png))


Quando a página é aberta, você verá um botão. Quando você clicar no botão, você obtém a caixa de diálogo de confirmação na Figura 7.


[![Exibir a caixa de diálogo de confirmação](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image13.png)

**Figura 07**: exibindo a caixa de diálogo de confirmação ([clique para exibir a imagem em tamanho normal](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image14.png))


Observe que você normalmente não arrastar um extensor de controle para uma página. Em vez disso, você usar o **adicionar extensor** opção para adicionar um extensor para um controle que você já tiver adicionado a uma página de tarefas. Além disso, observe que você defina controle propriedades estendidas, abra a folha de propriedades para o controle que está sendo estendido.

Um único controle ASP.NET pode ser estendido por vários extensores de controle. A folha de propriedades para o controle que está sendo estendido listará todos os Extensores do controle associados ao controle.

>[!div class="step-by-step"]
[Anterior](get-started-with-the-ajax-control-toolkit-vb.md)
[Próximo](creating-a-custom-ajax-control-toolkit-control-extender-vb.md)
