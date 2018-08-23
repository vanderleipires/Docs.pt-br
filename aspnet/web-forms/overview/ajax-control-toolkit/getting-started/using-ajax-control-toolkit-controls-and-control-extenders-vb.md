---
uid: web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-vb
title: Usando controles do Kit de ferramentas de controle AJAX e extensores de controle (VB) | Microsoft Docs
author: microsoft
description: Saiba como adicionar controles do AJAX Control Toolkit e extensores para suas páginas ASP.NET.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 763650a9-ffde-46a9-b779-7a9145dd5d88
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-vb
msc.type: authoredcontent
ms.openlocfilehash: f1cf40ec3ba299ee2702258a93aa1192a2112e7f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832440"
---
<a name="using-ajax-control-toolkit-controls-and-control-extenders-vb"></a>Usando controles do Kit de ferramentas de controle AJAX e extensores de controle (VB)
====================
por [Microsoft](https://github.com/microsoft)

> Saiba como adicionar controles do AJAX Control Toolkit e extensores para suas páginas ASP.NET.


O AJAX Control Toolkit contém um conjunto de controles e extensores de controle. Este breve tutorial, você aprenderá como adicionar controles e extensores de controle a uma página ASP.NET.

> [!NOTE] 
> 
> Para obter instruções sobre como instalar o AJAX Control Toolkit e adicionando o AJAX Control Toolkit para a caixa de ferramentas do Visual Studio/Visual Web Developer, consulte o tutorial [Introdução ao AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-vb.md).


## <a name="using-ajax-control-toolkit-controls"></a>Usando controles do Kit de ferramentas de controle AJAX

Um controle do AJAX Control Toolkit funciona como um controle ASP.NET normal. Você pode arrastar o controle da caixa de ferramentas em uma página ASP.NET. Você pode adicionar o controle para a página no modo de exibição de Design ou exibição da fonte.

Há um requisito especial ao usar os controles do AJAX Control Toolkit. A página deve conter um controle ScriptManager. O controle ScriptManager é responsável por incluir todos o JavaScript necessário necessário pelos controles AJAX Control Toolkit.

Por exemplo, na guia do AJAX Control Toolkit inclui um controle chamado o controle do Editor. Esse controle exibe um rico editor de HTML. Siga estas etapas para adicionar o controle de Editor para uma página:

1. Criar uma nova página do ASP.NET chamada ShowEditor.aspx
2. Selecione o controle ScriptManager do guia Extensões AJAX na caixa de ferramentas e arraste o controle para a página.
3. Selecione o controle de Editor do AJAX Control Toolkit guia na caixa de ferramentas e arraste o controle para a página (consulte a Figura 1). O Designer deve ser semelhante a Figura 2.
4. Executar o site selecionando a opção de menu **depurar, iniciar depuração** ou pressionando a tecla F5.
5. Você verá a página na Figura 3.


[![Selecionando o controle de Editor de HTML](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.png)

**Figura 01**: selecionando o controle de Editor de HTML ([clique para exibir a imagem em tamanho normal](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.png))


[![Designer do Visual Studio com o controle ScriptManager e editar](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.png)

**Figura 02**: Designer do Visual Studio com o controle ScriptManager e edição ([clique para exibir a imagem em tamanho normal](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.png))


[![A página DisplayEditor.aspx](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.png)

**Figura 03**: DisplayEditor.aspx a página ([clique para exibir a imagem em tamanho normal](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.png))


## <a name="using-ajax-control-toolkit-control-extenders"></a>Usando extensores de controle do Kit de ferramentas de controle do AJAX

O AJAX Control Toolkit também contém os extensores de controle. Como o nome sugere, um extensor de controle estende a funcionalidade de um controle existente. Por exemplo, o extensor ConfirmButton do controle estende o controle de botão do ASP.NET padrão. O extensor altera o comportamento de s de controle de botão para que o botão exibe uma caixa de diálogo de confirmação quando você clica nele.

Um extensor de controle, assim como um controle do AJAX Control Toolkit, requer um controle ScriptManager. Você deve adicionar um controle ScriptManager para uma página para começar a usar extensores de controle na página.

Siga estas etapas para usar o extensor do controle ConfirmButton:

1. Criar uma nova página do ASP.NET chamada ShowConfirmButton.aspx
2. Adicione um controle ScriptManager à página, arrastando o controle para a página de guia Extensões AJAX.
3. Adicione um controle de botão padrão para a página, arrastando o botão de guia padrão na caixa de ferramentas para a superfície de Designer.
4. Clique o **adicionar extensor** opção de tarefa (consulte a Figura 4).
5. Na caixa de diálogo Choose Extender, selecione ConfirmButtonExtender (consulte a Figura 5) e clique no botão Okey.
6. Selecione o controle de botão no Designer e expanda os extensores, Button1\_ConfirmButtonExtender nó na janela Propriedades (veja a Figura 6). Atribua o valor *realmente?* para a propriedade ConfirmText.
7. Execute a página, selecionando a opção de menu **depurar, iniciar depuração** ou pressionar a tecla F5.


[![A opção de adicionar extensor de tarefas](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.png)

**Figura 04**: opção de tarefa a adicionar extensor ([clique para exibir a imagem em tamanho normal](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image8.png))


[![Selecionando o extensor ConfirmButton do controle](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image9.png)

**Figura 05**: selecionando o extensor ConfirmButton do controle ([clique para exibir a imagem em tamanho normal](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image10.png))


[![Definir uma propriedade ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image11.png)

**Figura 06**: definir uma propriedade ConfirmButton ([clique para exibir a imagem em tamanho normal](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image12.png))


Quando a página é aberta, você verá um botão. Quando você clica no botão, você obtém a caixa de diálogo de confirmação na Figura 7.


[![Exibir a caixa de diálogo de confirmação](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image13.png)

**Figura 07**: exibir a caixa de diálogo de confirmação ([clique para exibir a imagem em tamanho normal](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image14.png))


Observe que você normalmente não arrastar um extensor de controle para uma página. Em vez disso, você usa o **adicionar extensor** opção para adicionar um extensor a um controle que você já tiver adicionado a uma página de tarefas. Além disso, observe que você defina controle extensor propriedades abrindo a folha de propriedades para o controle que está sendo estendido.

Um único controle ASP.NET pode ser estendido por vários extensores de controle. A folha de propriedades para o controle que está sendo estendido lista todos os extensores de controle associados ao controle.

> [!div class="step-by-step"]
> [Anterior](get-started-with-the-ajax-control-toolkit-vb.md)
> [Próximo](creating-a-custom-ajax-control-toolkit-control-extender-vb.md)
