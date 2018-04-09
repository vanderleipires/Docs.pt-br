---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: Instalar um auxiliar em uma Web ASP.NET páginas Site (Razor) | Microsoft Docs
author: tfitzmac
description: Este artigo descreve como instalar um auxiliar em um site de páginas da Web do ASP.NET (Razor). Um auxiliar é um componente reutilizável que inclui o código e marcação para por...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2014
ms.topic: article
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: 766fbb87ae8bcb8917eb8fa7f552c00792242cf6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a>Instalando um auxiliar em um Site de páginas (Razor) da Web do ASP.NET
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo descreve como instalar um auxiliar em um site de páginas da Web do ASP.NET (Razor). Um *auxiliar* é um componente reutilizável que inclui o código e marcação para executar uma tarefa que pode ser entediante ou complexos.
> 
> O que você aprenderá:
> 
> - Como instalar um auxiliar em um site criado usando o WebMatrix 3.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - WebMatrix 3


## <a name="overview-of-helpers"></a>Visão geral de auxiliares

Algumas tarefas que as pessoas desejam em páginas da web geralmente exigem muito código ou exigem conhecimento extra. Exemplos incluem a exibição de um gráfico de dados. colocar um botão "Seguir" do Twitter em uma página. Enviar email de seu site. recortando ou redimensionamento de imagens; usando PayPal para o seu site. Para facilitar a esses tipos de coisas, o ASP.NET Web Pages permite que você use *auxiliares*. Auxiliares são componentes que você instale um site e que permitem a você executam tarefas comuns usando apenas uma linha ou duas de código Razor.

Páginas da Web do ASP.NET tem alguns auxiliares internos. No entanto, muitos auxiliares estão disponíveis em pacotes (Suplementos) que são fornecidos usando o NuGet package manager. O NuGet permite que você selecione um pacote para instalar e, em seguida, ele cuida de todos os detalhes da instalação.

## <a name="installing-a-helper-in-webmatrix-3"></a>Instalando um auxiliar no WebMatrix 3

1. No WebMatrix 3, clique no **NuGet** botão.

    ![Caixa de diálogo do NuGet galeria no WebMatrix](installing-helpers/_static/image1.png)
2. Isso inicia o NuGet package manager e exibe os pacotes disponíveis. Na caixa de pesquisa, digite uma palavra-chave para o auxiliar que você deseja instalar.

    ![Caixa de diálogo do NuGet galeria no WebMatrix](installing-helpers/_static/image2.png)
3. Selecione o pacote e, em seguida, clique em **instalar**. Clique em **Sim** quando for perguntado se deseja instalar o pacote e indicar que você aceita os termos.

     Se esta for a primeira vez em que você instalou um auxiliar, o NuGet cria pastas no seu site para o código que compõe o auxiliar.
4. Para desinstalar um auxiliar, clique o **galeria** , clique no **instalado** guia e selecione o pacote que deseja desinstalar.

## <a name="installing-the-twitter-helper"></a>Instalando o auxiliar do Twitter

A versão mais recente da API do Twitter não é compatível com o auxiliar do Twitter que instalar por meio do NuGet. Em vez disso, consulte o [auxiliar do Twitter com o WebMatrix](twitter-helper.md) tópico para obter informações sobre como configurar o auxiliar do Twitter em seu projeto.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionais


[Introdução ao ASP.NET Web Pages 2 - Noções básicas de programação](../getting-started/introducing-razor-syntax-c.md)

[Auxiliar do Twitter com o WebMatrix](twitter-helper.md)
