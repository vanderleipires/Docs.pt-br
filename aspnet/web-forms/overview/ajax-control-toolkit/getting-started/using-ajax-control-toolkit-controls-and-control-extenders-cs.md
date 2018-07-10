---
uid: web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-cs
title: Usando controles do Kit de ferramentas de controle AJAX e extensores de controle (c#) | Microsoft Docs
author: microsoft
description: Saiba como adicionar controles do AJAX Control Toolkit e extensores para suas páginas ASP.NET.
ms.author: aspnetcontent
ms.date: 05/12/2009
ms.assetid: c1e6b51c-3bc3-4bf7-9916-9991197af3dd
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-cs
msc.type: authoredcontent
ms.openlocfilehash: 2c978bec3780ef2e83aee32a084d1baf5066e0e2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817220"
---
<a name="using-ajax-control-toolkit-controls-and-control-extenders-c"></a>Usando controles do Kit de ferramentas de controle AJAX e extensores de controle (c#)
====================
por [Microsoft](https://github.com/microsoft)

> Saiba como adicionar controles do AJAX Control Toolkit e extensores para suas páginas ASP.NET.


O AJAX Control Toolkit contém um conjunto de controles e extensores de controle. Este breve tutorial, você aprenderá como adicionar controles e extensores de controle a uma página ASP.NET.

> [!NOTE] 
> 
> Para obter instruções sobre como instalar o AJAX Control Toolkit e adicionando o AJAX Control Toolkit para a caixa de ferramentas do Visual Studio/Visual Web Developer, consulte o tutorial [Introdução ao AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs.md).


## <a name="using-ajax-control-toolkit-controls"></a>Usando controles do Kit de ferramentas de controle AJAX

Um controle do AJAX Control Toolkit funciona como um controle ASP.NET normal. Você pode arrastar o controle da caixa de ferramentas em uma página ASP.NET. Você pode adicionar o controle para a página no modo de exibição de Design ou exibição da fonte.

Há um requisito especial ao usar os controles do AJAX Control Toolkit. A página deve conter um controle ScriptManager. O controle ScriptManager é responsável por incluir todos o JavaScript necessário necessário pelos controles AJAX Control Toolkit.

Por exemplo, na guia do AJAX Control Toolkit inclui um controle chamado o controle do Editor. Esse controle exibe um rico editor de HTML. Siga estas etapas para adicionar o controle de Editor para uma página:

1. Criar uma nova página do ASP.NET chamada ShowEditor.aspx
2. Selecione o controle ScriptManager do guia Extensões AJAX na caixa de ferramentas e arraste o controle para a página.
3. Selecione o controle de Editor do AJAX Control Toolkit guia na caixa de ferramentas e arraste o controle para a página (consulte a Figura 1). O Designer deve ser semelhante a Figura 2.
4. Executar o site selecionando a opção de menu **depurar, iniciar depuração** ou pressionando a tecla F5.
5. Você verá a página na Figura 3.


[![Selecionando o controle de Editor de HTML](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.png)

**Figura 01**: selecionando o controle de Editor de HTML ([clique para exibir a imagem em tamanho normal](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.png))


[![Designer do Visual Studio com o controle ScriptManager e editar](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.png)

**Figura 02**: Designer do Visual Studio com o controle ScriptManager e edição ([clique para exibir a imagem em tamanho normal](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.png))


[![A página DisplayEditor.aspx](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.png)

**Figura 03**: DisplayEditor.aspx a página ([clique para exibir a imagem em tamanho normal](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.png))


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


[![A opção de adicionar extensor de tarefas](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.png)

**Figura 04**: opção de tarefa a adicionar extensor ([clique para exibir a imagem em tamanho normal](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image8.png))


[![Selecionando o extensor ConfirmButton do controle](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image9.png)

**Figura 05**: selecionando o extensor ConfirmButton do controle ([clique para exibir a imagem em tamanho normal](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image10.png))


[![Definir uma propriedade ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image11.png)

**Figura 06**: definir uma propriedade ConfirmButton ([clique para exibir a imagem em tamanho normal](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image12.png))


Quando a página é aberta, você verá um botão. Quando você clica no botão, você obtém a caixa de diálogo de confirmação na Figura 7.


[![Exibir a caixa de diálogo de confirmação](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image13.png)

**Figura 07**: exibir a caixa de diálogo de confirmação ([clique para exibir a imagem em tamanho normal](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image14.png))


Observe que você normalmente não arrastar um extensor de controle para uma página. Em vez disso, você usa o **adicionar extensor** opção para adicionar um extensor a um controle que você já tiver adicionado a uma página de tarefas. Além disso, observe que você defina controle extensor propriedades abrindo a folha de propriedades para o controle que está sendo estendido.

Um único controle ASP.NET pode ser estendido por vários extensores de controle. A folha de propriedades para o controle que está sendo estendido lista todos os extensores de controle associados ao controle.

> [!div class="step-by-step"]
> [Anterior](get-started-with-the-ajax-control-toolkit-cs.md)
> [Próximo](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)
