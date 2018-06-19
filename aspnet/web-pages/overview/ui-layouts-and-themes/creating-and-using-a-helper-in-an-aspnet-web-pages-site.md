---
uid: web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
title: Criando e usando um auxiliar em uma Web ASP.NET páginas Site (Razor) | Microsoft Docs
author: tfitzmac
description: Este artigo descreve como criar um auxiliar em um site de páginas da Web do ASP.NET (Razor). Um auxiliar é um componente reutilizável que inclui o código e a marcação para o desempenho...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 46bff772-01e0-40f0-9ae6-9e18c5442ee6
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 5d0c1ae09d8fbc91ff76cd4045d439abafee7736
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26529995"
---
<a name="creating-and-using-a-helper-in-an-aspnet-web-pages-razor-site"></a>Criando e usando um auxiliar em um Site de páginas da Web (Razor) do ASP.NET
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo descreve como criar um auxiliar em um site de páginas da Web do ASP.NET (Razor). Um *auxiliar* é um componente reutilizável que inclui o código e marcação para executar uma tarefa que pode ser entediante ou complexos.
> 
> **O que você aprenderá:** 
> 
> - Como criar e usar um assistente simple.
> 
> Estes são os recursos ASP.NET introduzidos no artigo:
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
> Este tutorial também funciona com 2 de páginas da Web do ASP.NET.


## <a name="overview-of-helpers"></a>Visão geral de auxiliares

Se você precisar executar as mesmas tarefas em diferentes páginas em seu site, você pode usar um auxiliar. Páginas da Web ASP.NET inclui uma série de auxiliares, e há muito mais que você pode baixar e instalar. (Uma lista dos internas auxiliares em páginas da Web do ASP.NET está listada no [referência rápida de API ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202907).) Se nenhum dos auxiliares existentes atender às suas necessidades, você pode criar seu próprios auxiliar.

Um auxiliar permite que você use um bloco comum de código em várias páginas. Suponha que em sua página você geralmente deseja criar um item de observação que é definido além de parágrafos normais. Talvez a anotação é criada como uma `<div>` elemento que tem o estilo como uma caixa com uma borda. Em vez de adicionar essa mesma marcação para uma página toda vez que você deseja exibir uma anotação, você pode empacotar a marcação como um auxiliar. Você pode inserir a anotação com uma única linha de código em qualquer local necessário.

Usar um auxiliar de como isso torna o código em cada uma das suas páginas e facilitar a leitura. Ele também torna mais fácil manter seu site, como se você precisar alterar a aparência de notas, você pode alterar a marcação em um único local.

## <a name="creating-a-helper"></a>Criando um auxiliar

Este procedimento mostra como criar o auxiliar que cria a observação, conforme descrito apenas. Este é um exemplo simples, mas o auxiliar personalizado pode incluir qualquer marcação e código ASP.NET que você precisa.

1. Na pasta raiz do site, crie uma pasta chamada *aplicativo\_código*. Este é um nome de pasta reservado no ASP.NET, onde você pode colocar o código para componentes como auxiliares.
2. No *aplicativo\_código* pasta criar uma nova *. cshtml* de arquivo e nomeie-o *MyHelpers.cshtml*.
3. Substitua o conteúdo existente com o seguinte:

    [!code-cshtml[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    O código usa o `@helper` a sintaxe para declarar um auxiliar novo chamado `MakeNote`. Este auxiliar específico permite que você passe um parâmetro denominado `content` que pode conter uma combinação de texto e marcação. O auxiliar insere a cadeia de caracteres no corpo de Observação usando o `@content` variável.

    Observe que o arquivo é nomeado *MyHelpers.cshtml*, mas o auxiliar é denominado `MakeNote`. Você pode colocar vários auxiliares personalizados em um único arquivo.
4. Salve e feche o arquivo.

## <a name="using-the-helper-in-a-page"></a>Usando o auxiliar em uma página

1. Na pasta raiz, crie um novo arquivo vazio chamado *TestHelper.cshtml*.
2. Adicione o seguinte código ao arquivo:

    [!code-html[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample2.html)]

    Para chamar o auxiliar que você criou, use `@` seguido pelo nome do arquivo onde está o auxiliar, um ponto e, em seguida, o nome de auxiliar. (Se você tiver várias pastas *aplicativo\_código* pasta, você pode usar a sintaxe `@FolderName.FileName.HelperName` chamar o auxiliar em qualquer aninhados no nível de pasta). O texto que você adicionar aspas dentro dos parênteses é o texto que o auxiliar será exibido como parte da anotação na página da web.
3. Salve a página e executá-lo em um navegador. O auxiliar gera o item de anotação direita onde você chamou o auxiliar: entre dois parágrafos.

    ![Captura de tela mostrando a página no navegador e como o auxiliar gerado marcação que coloca uma caixa ao redor do texto especificado.](creating-and-using-a-helper-in-an-aspnet-web-pages-site/_static/image1.jpg)

## <a name="additional-resources"></a>Recursos adicionais


[Menu horizontal como um auxiliar de Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341). Essa entrada de blog, Mike Pope mostra como criar um menu horizontal como um auxiliar usando marcação, CSS e código.

[Aproveitar o HTML5 com o ASP.NET Web páginas auxiliares para o WebMatrix e ASP.NET MVC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx). Essa entrada de blog pelo Sam Abraham mostra um auxiliar que renderiza uma HTML5 `Canvas` elemento.

[A diferença entre @Helpers e @Functions no WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix). Descreve essa entrada de blog, Mike Brind `@helper` sintaxe e `@function` sintaxe e quando usar cada um.
