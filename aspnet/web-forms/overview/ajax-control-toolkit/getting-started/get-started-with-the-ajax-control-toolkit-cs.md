---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
title: Introdução ao Kit de ferramentas de controle AJAX (c#) | Microsoft Docs
author: microsoft
description: Saiba tudo o que precisa saber para começar a usar o Kit de ferramentas de controle AJAX.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 16dc5c11-65be-4eae-a818-9fad7f8259c6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
msc.type: authoredcontent
ms.openlocfilehash: e6a7a8d45f32a33eaacf3c42b52a02d2ada1aab6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="get-started-with-the-ajax-control-toolkit-c"></a>Introdução ao Kit de ferramentas de controle AJAX (c#)
====================
por [Microsoft](https://github.com/microsoft)

> Saiba tudo o que precisa saber para começar a usar o Kit de ferramentas de controle AJAX.


O Kit de ferramentas de controle AJAX contém mais de 30 controles livres que você pode usar em seus aplicativos ASP.NET. Neste tutorial, você aprenderá como baixar o Kit de ferramentas de controle AJAX e adicionar os controles do Kit de ferramentas para sua caixa de ferramentas do Visual Studio/Visual Web Developer Express.

## <a name="downloading-the-ajax-control-toolkit"></a>Baixar o Kit de ferramentas de controle AJAX

O [AJAX Control Toolkit](http://devexpress.com/act) é um projeto de software livre desenvolvido por membros da comunidade do ASP.NET e a equipe do ASP.NET. 


[![Baixar o Kit de ferramentas de controle AJAX](get-started-with-the-ajax-control-toolkit-cs/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image1.png)

**Figura 01**: baixar o Kit de ferramentas de controle AJAX ([clique para exibir a imagem em tamanho normal](get-started-with-the-ajax-control-toolkit-cs/_static/image2.png))


Depois de baixar o arquivo, você precisa desbloquear o arquivo. Clique no arquivo, selecione Propriedades e clique no **desbloquear** botão (consulte a Figura 2).


[![Desbloquear o arquivo ZIP de kit de ferramentas de controle AJAX](get-started-with-the-ajax-control-toolkit-cs/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image3.png)

**Figura 02**: desbloquear o arquivo ZIP de kit de ferramentas de controle AJAX ([clique para exibir a imagem em tamanho normal](get-started-with-the-ajax-control-toolkit-cs/_static/image4.png))


Depois de desbloquear o arquivo, você pode descompactar o arquivo: o arquivo e selecione o **extrair tudo** opção de menu. Agora, você está pronto para adicionar o Kit de ferramentas para a caixa de ferramentas do Visual Studio/Visual Web Developer.

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a>Adicionando o Kit de ferramentas de controle AJAX para a caixa de ferramentas

É a maneira mais fácil de usar o Kit de ferramentas de controle AJAX adicionar o Kit de ferramentas para sua caixa de ferramentas do Visual Studio/Visual Web Developer (consulte a Figura 3). Dessa forma, você pode simplesmente arraste um kit de ferramentas controle para uma página quando você deseja usá-lo.


[![Kit de ferramentas de controle AJAX aparece na caixa de ferramentas](get-started-with-the-ajax-control-toolkit-cs/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image5.png)

**Figura 03**: AJAX Control Toolkit é exibido na caixa de ferramentas ([clique para exibir a imagem em tamanho normal](get-started-with-the-ajax-control-toolkit-cs/_static/image6.png))


Primeiro, você precisa adicionar uma guia do Kit de ferramentas de controle AJAX para a caixa de ferramentas. Siga estas etapas.

1. Crie um novo site da Web ASP.NET, selecionando a opção de menu Arquivo, novo site. Clique duas vezes em Default.aspx na janela do Gerenciador de soluções para abrir o arquivo no editor.
2. A caixa de ferramentas sob a guia Geral e selecione a opção de menu **Adicionar guia** (consulte a Figura 4).
3. Insira uma nova guia chamada AJAX Control Toolkit.


[![Adicionar uma nova guia](get-started-with-the-ajax-control-toolkit-cs/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image7.png)

**Figura 04**: adicionando uma nova guia ([clique para exibir a imagem em tamanho normal](get-started-with-the-ajax-control-toolkit-cs/_static/image8.png))


Em seguida, você precisa adicionar os controles do Kit de ferramentas de controle AJAX para a nova guia. Siga estas etapas:

- Sob a guia AJAX Control Toolkit e selecione a opção de menu **escolher itens (consulte a Figura 5)**.
- Navegue até o local onde você descompactou o Kit de ferramentas de controle AJAX e selecione o assembly AjaxControlToolkit.


[![Escolha os itens a serem adicionados à caixa de ferramentas](get-started-with-the-ajax-control-toolkit-cs/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image9.png)

**Figura 05**: escolha os itens a serem adicionados à caixa de ferramentas ([clique para exibir a imagem em tamanho normal](get-started-with-the-ajax-control-toolkit-cs/_static/image10.png))


Depois de concluir essas etapas, todos os controles do Kit de ferramentas serão exibida na caixa de ferramentas.

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a>Atualizar para uma nova versão do Kit de ferramentas

Se você estivesse usando uma versão mais antiga do Kit de ferramentas e agora precisa mover para uma versão posterior aqui estão as etapas recomendadas:

- Binários - excluir a versão antiga do assembly AjaxControlToolkit da pasta Bin do site.
- Itens de caixa de ferramentas - Excluir guia AJAX Control Toolkit e siga as etapas acima para recriar a guia com a nova versão do assembly AjaxControlToolkit.

> [!div class="step-by-step"]
> [Avançar](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
