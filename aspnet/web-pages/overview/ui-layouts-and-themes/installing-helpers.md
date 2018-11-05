---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: Instalação de um auxiliar em uma Web do ASP.NET (Razor) sites de páginas | Microsoft Docs
author: Rick-Anderson
description: Este artigo descreve como instalar um auxiliar em um site de páginas da Web do ASP.NET (Razor). Um auxiliar é um componente reutilizável que inclui código e marcação para por...
ms.author: riande
ms.date: 02/18/2014
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: 5ad717cd7c64e830ce66d5e1361d0eb6ef3cbbec
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021359"
---
<a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a>Instalação de um auxiliar em um Site do ASP.NET Web Pages (Razor)
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo descreve como instalar um auxiliar em um site de páginas da Web do ASP.NET (Razor). Um *auxiliar* é um componente reutilizável que inclui código e marcação para executar uma tarefa que pode ser entediante ou complexos.
> 
> O que você aprenderá:
> 
> - Como instalar um auxiliar em um site criado usando o WebMatrix 3.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - O WebMatrix 3


## <a name="overview-of-helpers"></a>Visão geral dos auxiliares

Algumas tarefas que as pessoas frequentemente desejam fazer em páginas da web exigem um monte de código ou exigem conhecimento extra. Exemplos incluem a exibição de um gráfico para dados; colocar um botão "Seguir" do Twitter em uma página. envio de email do seu site; recortando ou redimensionamento de imagens; usando PayPal para o seu site. Para que seja fácil fazer esses tipos de coisas, o ASP.NET Web Pages permite que você use *auxiliares*. Os auxiliares são componentes que você instale para um site e que permitem que você realize tarefas típicas usando apenas uma ou duas linhas de código do Razor.

Páginas da Web do ASP.NET tem alguns auxiliares internos. No entanto, muitos auxiliares estão disponíveis em pacotes (Suplementos) que são fornecidos usando o Gerenciador de pacotes do NuGet. O NuGet permite que você selecione um pacote para instalar e, em seguida, ele cuida de todos os detalhes da instalação.

## <a name="installing-a-helper-in-webmatrix-3"></a>Instalação de um auxiliar no WebMatrix 3

1. O WebMatrix 3, clique no **NuGet** botão.

    ![Caixa de diálogo de galeria do NuGet no WebMatrix](installing-helpers/_static/image1.png)
2. Isso inicia o Gerenciador de pacotes do NuGet e exibe os pacotes disponíveis. Na caixa de pesquisa, insira uma palavra-chave para o auxiliar que você deseja instalar.

    ![Caixa de diálogo de galeria do NuGet no WebMatrix](installing-helpers/_static/image2.png)
3. Selecione o pacote e, em seguida, clique em **instalar**. Clique em **Sim** quando for perguntado se você deseja instalar o pacote e indicar que você aceite os termos.

     Se essa for a primeira vez em que você instalou um auxiliar, o NuGet cria pastas no seu site para o código que compõe o auxiliar.
4. Para desinstalar um auxiliar, clique o **galeria** , clique no **instalado** guia e, em seguida, selecione o pacote que você deseja desinstalar.

## <a name="installing-the-twitter-helper"></a>Instalando o auxiliar do Twitter

A versão mais recente da API do Twitter não é compatível com o auxiliar do Twitter que instalar por meio do NuGet. Em vez disso, consulte a [auxiliar do Twitter com o WebMatrix](twitter-helper.md) tópico para obter informações sobre como configurar o auxiliar do Twitter em seu projeto.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionais


[Introdução ao ASP.NET Web Pages 2 - Noções básicas de programação](../getting-started/introducing-razor-syntax-c.md)

[Auxiliar do Twitter com o WebMatrix](twitter-helper.md)
