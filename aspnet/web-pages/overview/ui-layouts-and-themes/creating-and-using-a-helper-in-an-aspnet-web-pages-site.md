---
uid: web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
title: Criando e usando um auxiliar em uma Web do ASP.NET (Razor) sites de páginas | Microsoft Docs
author: tfitzmac
description: Este artigo descreve como criar um auxiliar em um site de páginas da Web do ASP.NET (Razor). Um auxiliar é um componente reutilizável que inclui código e marcação para perf...
ms.author: riande
ms.date: 02/17/2014
ms.assetid: 46bff772-01e0-40f0-9ae6-9e18c5442ee6
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: a6d3074f39a1d7b81b173813a73380fe2a536e7a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824274"
---
<a name="creating-and-using-a-helper-in-an-aspnet-web-pages-razor-site"></a>Criando e usando um auxiliar em um Site do ASP.NET Web Pages (Razor)
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo descreve como criar um auxiliar em um site de páginas da Web do ASP.NET (Razor). Um *auxiliar* é um componente reutilizável que inclui código e marcação para executar uma tarefa que pode ser entediante ou complexos.
> 
> **O que você aprenderá:** 
> 
> - Como criar e usar um auxiliar simple.
> 
> Estes são os recursos do ASP.NET introduzidos no artigo:
> 
> - O `@helper` sintaxe.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Páginas da Web do ASP.NET (Razor) 3
>   
> 
> Este tutorial também funciona com ASP.NET Web Pages 2.


## <a name="overview-of-helpers"></a>Visão geral dos auxiliares

Se você precisar executar as mesmas tarefas em diferentes páginas em seu site, você pode usar um auxiliar. Páginas da Web ASP.NET inclui uma série de auxiliares, e há muito mais que você pode baixar e instalar. (Uma lista dos auxiliares internos em páginas da Web do ASP.NET está listada na [referência rápida de API ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202907).) Se nenhum dos auxiliares existentes atender às suas necessidades, você pode criar seu próprios auxiliar.

Um auxiliar permite que você use um bloco comum de código em várias páginas. Suponha que em sua página você geralmente deseja criar um item de observação é definido, além de parágrafos normais. Talvez a anotação é criada como uma `<div>` elemento que tem o estilo como uma caixa com uma borda. Em vez de adicionar essa mesma marcação para uma página sempre que você deseja exibir uma anotação, você pode empacotar a marcação como um auxiliar. Em seguida, você pode inserir a anotação com uma única linha de código em qualquer lugar você precisa.

Uso de um auxiliar, como isso torna o código em cada uma das suas páginas mais simples e fácil de ler. Ele também torna mais fácil de manter seu site, como se você precisar alterar a aparência de notas, você pode alterar a marcação em um só lugar.

## <a name="creating-a-helper"></a>Criar um auxiliar

Este procedimento mostra como criar o auxiliar que cria a observação, como acabamos de descrever. Este é um exemplo simples, mas o auxiliar personalizado pode incluir qualquer marcação e código do ASP.NET que você precisa.

1. Na pasta raiz do site, crie uma pasta chamada *App\_código*. Isso é um nome de pasta reservado no ASP.NET, onde você pode colocar o código para componentes como auxiliares.
2. No *App\_código* pasta criar uma nova *. cshtml* de arquivo e nomeie-a *MyHelpers.cshtml*.
3. Substitua o conteúdo existente pelo seguinte:

    [!code-cshtml[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    O código usa o `@helper` sintaxe para declarar um novo auxiliar denominado `MakeNote`. Esse auxiliar particular permite que você passe um parâmetro chamado `content` que pode conter uma combinação de texto e marcação. O auxiliar insere a cadeia de caracteres no corpo de Observação usando o `@content` variável.

    Observe que o arquivo é denominado *MyHelpers.cshtml*, mas o auxiliar chamado `MakeNote`. Você pode colocar vários auxiliares personalizados em um único arquivo.
4. Salve e feche o arquivo.

## <a name="using-the-helper-in-a-page"></a>Usando o auxiliar em uma página

1. Na pasta raiz, crie um novo arquivo vazio chamado *TestHelper.cshtml*.
2. Adicione o seguinte código ao arquivo:

    [!code-html[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample2.html)]

    Para chamar o auxiliar que você criou, use `@` seguido pelo nome do arquivo onde é o auxiliar, um ponto e, em seguida, o nome do auxiliar. (Se você tiver várias pastas na *App\_código* pasta, você poderá usar a sintaxe `@FolderName.FileName.HelperName` chamar o auxiliar em qualquer aninhados no nível de pasta). O texto que você adicione aspas dentro dos parênteses é o texto que o auxiliar será exibido como parte da nota, na página da web.
3. Salve a página e executá-lo em um navegador. O auxiliar gera o item de Observação à direita no qual você chamou o auxiliar: entre dois parágrafos.

    ![Captura de tela mostrando a página no navegador e como o auxiliar de geração de marcação que coloca uma caixa ao redor do texto especificado.](creating-and-using-a-helper-in-an-aspnet-web-pages-site/_static/image1.jpg)

## <a name="additional-resources"></a>Recursos adicionais


[Menu horizontal como um auxiliar de Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341). Essa entrada de blog por Mike Pope mostra como criar um menu horizontal como um auxiliar usando marcação, CSS e código.

[Aproveitar o HTML5 na Web do ASP.NET páginas auxiliares para o WebMatrix e do ASP.NET MVC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx). Essa entrada de blog por Sam Abraham mostra um auxiliar que renderiza um HTML5 `Canvas` elemento.

[A diferença entre @Helpers e @Functions no WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix). Descreve essa entrada de blog por Mike Brind `@helper` sintaxe e `@function` sintaxe e quando usar cada um.
