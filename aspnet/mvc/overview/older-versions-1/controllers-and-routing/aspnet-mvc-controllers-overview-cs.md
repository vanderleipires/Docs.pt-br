---
uid: mvc/overview/older-versions-1/controllers-and-routing/aspnet-mvc-controllers-overview-cs
title: Visão geral do controlador ASP.NET MVC (c#) | Microsoft Docs
author: StephenWalther
description: Neste tutorial, Stephen Walther apresenta controladores do ASP.NET MVC. Você aprenderá a criar novos controladores e retornar tipos diferentes de res de ação...
ms.author: aspnetcontent
ms.date: 02/16/2008
ms.assetid: b985c49a-3668-455c-a366-f85f6bc64b12
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/aspnet-mvc-controllers-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: f8ff1ed19e6357a9a4e98f755a677d05ae5c2ec4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37821716"
---
<a name="aspnet-mvc-controller-overview-c"></a>Visão geral do controlador ASP.NET MVC (c#)
====================
por [Stephen Walther](https://github.com/StephenWalther)

> Neste tutorial, Stephen Walther apresenta controladores do ASP.NET MVC. Você aprenderá a criar novos controladores e retornar tipos diferentes de resultados de ação.


Este tutorial explora o tópico de controladores, ações do controlador e resultados de ação do ASP.NET MVC. Depois de concluir este tutorial, você entenderá como os controladores são usados para controlar a maneira como um visitante interage com um site ASP.NET MVC.

## <a name="understanding-controllers"></a>Noções básicas sobre controladores

Controladores MVC serão responsáveis por responder a solicitações feitas em relação a um site ASP.NET MVC. Cada solicitação do navegador é mapeada para um controlador específico. Por exemplo, imagine que você insira a seguinte URL na barra de endereços do navegador:

`http://localhost/Product/Index/3`

Nesse caso, um controlador chamado ProductController é invocado. ProductController é responsável por gerar a resposta para a solicitação do navegador. Por exemplo, o controlador pode retornar uma exibição específica de volta para o navegador ou o controlador pode redirecionar o usuário para outro controlador.

Listagem 1 contém um controlador simples chamado ProductController.

**Listing1 - Controllers\ProductController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample1.cs)]

Como você pode ver na listagem 1, um controlador é apenas uma classe (uma classe Visual Basic .NET ou c#). Um controlador é uma classe que deriva da classe Controller base. Como um controlador herda dessa classe base, um controlador herda os vários métodos úteis gratuitamente (abordaremos esses métodos em instantes).

## <a name="understanding-controller-actions"></a>Noções básicas sobre ações do controlador

Um controlador expõe as ações do controlador. Uma ação é um método em um controlador que é chamado quando você inserir uma URL específica na barra de endereços do navegador. Por exemplo, imagine que você faça uma solicitação para a URL a seguir:

`http://localhost/Product/Index/3`

Nesse caso, o método Index () é chamado na classe ProductController. O método Index () é um exemplo de uma ação do controlador.

Uma ação do controlador deve ser um método público de uma classe de controlador. Métodos c#, por padrão, são métodos privados. Perceba que qualquer método público que você adicionar a uma classe de controlador é exposto como uma ação do controlador automaticamente (você deve ter cuidado isso vez que uma ação do controlador pode ser invocada por qualquer pessoa no universo simplesmente digitando a URL correta na barra de endereços do navegador).

Há alguns requisitos adicionais que devem ser atendidos por uma ação do controlador. Um método usado como uma ação de controlador não pode ser sobrecarregado. Além disso, uma ação de controlador não pode ser um método estático. Além disso, você pode usar qualquer método como uma ação do controlador.

## <a name="understanding-action-results"></a>Noções básicas sobre resultados de ação

Uma ação do controlador retorna algo chamado de um *resultado da ação*. Resultado de uma ação é o que uma ação do controlador retorna em resposta a uma solicitação do navegador.

O ASP.NET MVC framework oferece suporte a vários tipos de resultados de ação, incluindo:

1. ViewResult - representa HTML e marcação.
2. EmptyResult - não representa nenhum resultado.
3. RedirectResult - representa um redirecionamento para uma nova URL.
4. JsonResult - representa um resultado de JavaScript Object Notation que pode ser usado em um aplicativo AJAX.
5. JavaScriptResult - representa um script do JavaScript.
6. ContentResult - representa um resultado de texto.
7. FileContentResult - representa um arquivo para download (com o conteúdo binário).
8. FilePathResult - representa um arquivo para download (com um caminho).
9. FileStreamResult - representa um arquivo para download (com um fluxo de arquivos).

Todos esses resultados de ação herdam da classe ActionResult base.

Na maioria dos casos, uma ação do controlador retorna um ViewResult. Por exemplo, a ação do controlador de índice na listagem 2 retorna um ViewResult.

**Listagem 2 - Controllers\BookController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample2.cs)]

Quando uma ação retorna um ViewResult, HTML é retornado ao navegador. O método Index () na listagem 2 retorna uma exibição nomeada de índice para o navegador.

Observe que a ação Index () na listagem 2 não retorna um ViewResult(). Em vez disso, o método View() da classe Controller base é chamado. Normalmente, você não retornar um resultado de ação diretamente. Em vez disso, você chama um dos seguintes métodos da classe base do controlador:

1. Exibição - retorna um resultado de ação do ViewResult.
2. Redirecionar - retorna um resultado de ação RedirectResult.
3. RedirectToAction - retorna um resultado de ação RedirectToRouteResult.
4. RedirectToRoute - retorna um resultado de ação RedirectToRouteResult.
5. JSON - retorna um resultado de ação JsonResult.
6. JavaScriptResult - retorna um JavaScriptResult.
7. Conteúdo - retorna um resultado de ação ContentResult.
8. Arquivo - retorna um FileContentResult, FilePathResult ou FileStreamResult dependendo dos parâmetros passado para o método.

Portanto, se você quiser retornar uma exibição para o navegador, você chamar o método de View(). Se você deseja redirecionar o usuário da ação de um controlador para outro, você pode chamar o método de RedirectToAction(). Por exemplo, a ação de Details() na listagem 3 mostra uma exibição tanto redireciona o usuário para a ação Index (), dependendo se o parâmetro de Id tem um valor.

**Listagem 3 - CustomerController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample3.cs)]

O resultado da ação ContentResult é especial. Você pode usar o resultado da ação ContentResult para retornar o resultado de uma ação como texto sem formatação. Por exemplo, o método Index () na listagem 4 retorna uma mensagem como texto sem formatação e não como HTML.

**Listagem 4 - Controllers\StatusController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample4.cs)]

Quando a ação de StatusController.Index() é chamada, um modo de exibição não é retornado. Em vez disso, o texto não processado "Hello World!" é retornado ao navegador.

Se uma ação do controlador retorna um resultado que é não um resultado de ação - por exemplo, uma data ou um inteiro -, em seguida, o resultado é encapsulado em um ContentResult automaticamente. Por exemplo, quando a ação Index () do WorkController na listagem 5 é invocada, a data é retornada como um ContentResult automaticamente.

**Listagem 5 - WorkController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample5.cs)]

A ação Index () na listagem 5 retorna um objeto DateTime. O ASP.NET MVC framework converte o objeto DateTime em uma cadeia de caracteres e encapsula o valor de data e hora em um ContentResult automaticamente. O navegador recebe a data e hora como texto sem formatação.

## <a name="summary"></a>Resumo

A finalidade deste tutorial era apresentar a você os conceitos de controladores, ações do controlador e resultados de ação do controlador MVC do ASP.NET. Na primeira seção, você aprendeu a adicionar novos controladores para um projeto ASP.NET MVC. Em seguida, você aprendeu como públicos métodos de um controlador são expostos para o universo como ações do controlador. Por fim, discutimos os diferentes tipos de resultados de ações que podem ser retornados de uma ação do controlador. Em particular, discutimos como retornar um ViewResult, RedirectToActionResult e ContentResult de uma ação do controlador.

> [!div class="step-by-step"]
> [Anterior](creating-an-action-vb.md)
> [Próximo](creating-custom-routes-cs.md)
