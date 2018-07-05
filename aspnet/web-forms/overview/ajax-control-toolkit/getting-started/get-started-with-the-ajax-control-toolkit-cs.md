---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
title: Introdução ao AJAX Control Toolkit (c#) | Microsoft Docs
author: microsoft
description: Aprenda tudo o que você precisa saber para começar a usar o AJAX Control Toolkit.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 16dc5c11-65be-4eae-a818-9fad7f8259c6
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
msc.type: authoredcontent
ms.openlocfilehash: 120213ce27858e2f0650b71c82f689ae4493d8a8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399591"
---
<a name="get-started-with-the-ajax-control-toolkit-c"></a>Introdução ao AJAX Control Toolkit (c#)
====================
por [Microsoft](https://github.com/microsoft)

> Aprenda tudo o que você precisa saber para começar a usar o AJAX Control Toolkit.


O AJAX Control Toolkit contém mais de 30 controles gratuitos que você pode usar em seus aplicativos ASP.NET. Neste tutorial, você aprenderá como baixar o AJAX Control Toolkit e adicione os controles do Kit de ferramentas para sua caixa de ferramentas do Visual Studio/Visual Web Developer Express.

## <a name="downloading-the-ajax-control-toolkit"></a>Baixando o AJAX Control Toolkit

O [AJAX Control Toolkit](http://devexpress.com/act) é um projeto de código-fonte aberto desenvolvido por membros da comunidade do ASP.NET e a equipe do ASP.NET. 


[![Baixando o AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image1.png)

**Figura 01**: baixar o AJAX Control Toolkit ([clique para exibir a imagem em tamanho normal](get-started-with-the-ajax-control-toolkit-cs/_static/image2.png))


Depois de baixar o arquivo, você precisará desbloquear o arquivo. O arquivo com o botão direito, selecione Propriedades e clique o **Unblock** botão (consulte a Figura 2).


[![Desbloquear o arquivo ZIP de kit de ferramentas de controle do AJAX](get-started-with-the-ajax-control-toolkit-cs/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image3.png)

**Figura 02**: desbloquear o arquivo ZIP de kit de ferramentas de controle do AJAX ([clique para exibir a imagem em tamanho normal](get-started-with-the-ajax-control-toolkit-cs/_static/image4.png))


Depois de desbloquear o arquivo, você pode descompactar o arquivo: o arquivo com o botão direito e selecione o **extrair tudo** opção de menu. Agora, estamos prontos para adicionar o Kit de ferramentas para a caixa de ferramentas do Visual Studio/Visual Web Developer.

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a>Adicionando o AJAX Control Toolkit à caixa de ferramentas

A maneira mais fácil de usar o AJAX Control Toolkit é adicionar o Kit de ferramentas para sua caixa de ferramentas do Visual Studio/Visual Web Developer (veja a Figura 3). Dessa forma, você pode simplesmente arrastar um controle do Kit de ferramentas em uma página quando você deseja usá-lo.


[![AJAX Control Toolkit é exibido na caixa de ferramentas](get-started-with-the-ajax-control-toolkit-cs/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image5.png)

**Figura 03**: AJAX Control Toolkit é exibido na caixa de ferramentas ([clique para exibir a imagem em tamanho normal](get-started-with-the-ajax-control-toolkit-cs/_static/image6.png))


Primeiro, você precisará adicionar uma guia do AJAX Control Toolkit à caixa de ferramentas. Siga estas etapas.

1. Crie um novo site do ASP.NET, selecionando a opção de menu Arquivo, novo site. Clique duas vezes em Default. aspx na janela do Gerenciador de soluções para abrir o arquivo no editor.
2. A caixa de ferramentas sob a guia geral com o botão direito e selecione a opção de menu **Adicionar guia** (veja a Figura 4).
3. Insira uma nova guia chamada AJAX Control Toolkit.


[![Adicionar uma nova guia](get-started-with-the-ajax-control-toolkit-cs/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image7.png)

**Figura 04**: adicionar uma nova guia ([clique para exibir a imagem em tamanho normal](get-started-with-the-ajax-control-toolkit-cs/_static/image8.png))


Em seguida, você precisa adicionar os controles do AJAX Control Toolkit para a nova guia. Siga estas etapas:

- Clique com botão direito sob a guia do AJAX Control Toolkit e selecionar a opção de menu **escolher itens (consulte a Figura 5)**.
- Navegue até o local onde você descompactou o AJAX Control Toolkit e selecione o assembly AjaxControlToolkit.


[![Escolha os itens a serem adicionados à caixa de ferramentas](get-started-with-the-ajax-control-toolkit-cs/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image9.png)

**Figura 05**: escolha os itens a serem adicionados à caixa de ferramentas ([clique para exibir a imagem em tamanho normal](get-started-with-the-ajax-control-toolkit-cs/_static/image10.png))


Depois de concluir essas etapas, todos os controles do Kit de ferramentas serão exibida na caixa de ferramentas.

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a>Atualizando para uma nova versão do Kit de ferramentas

Se você estivesse usando uma versão mais antiga do Kit de ferramentas e agora preciso migrar para uma versão mais recente aqui são as etapas recomendadas:

- Binários - excluir a versão antiga do assembly AjaxControlToolkit da pasta Bin do site.
- Itens de caixa de ferramentas - excluir a guia do AJAX Control Toolkit e siga as etapas acima para recriar a guia com a nova versão do assembly AjaxControlToolkit.

> [!div class="step-by-step"]
> [Avançar](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
