---
uid: mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-vb
title: Criando testes de unidade para aplicativos ASP.NET MVC (VB) | Microsoft Docs
author: StephenWalther
description: Saiba como criar testes de unidade para ações do controlador. Neste tutorial, Stephen Walther demonstra como testar se uma ação do controlador retorna um parti...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: eb35710d-1d99-44ac-b61f-e50af8cb328a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-vb
msc.type: authoredcontent
ms.openlocfilehash: 299665f45d72fee33f92344ed53c87dfb1a76d60
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="creating-unit-tests-for-aspnet-mvc-applications-vb"></a>Criando testes de unidade para aplicativos ASP.NET MVC (VB)
====================
por [Stephen Walther](https://github.com/StephenWalther)

[Baixar PDF](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_07_VB.pdf)

> Saiba como criar testes de unidade para ações do controlador. Neste tutorial, Stephen Walther demonstra como testar se uma ação do controlador retorna uma exibição específica, retorna um conjunto específico de dados ou retorna um tipo diferente de resultado da ação.


O objetivo deste tutorial é demonstrar como você pode escrever o testes de unidade para os controladores no seu ASP.NET MVC aplicativos. Vamos discutir como compilar três tipos diferentes de testes de unidade. Você aprenderá como a exibição retornada por uma ação do controlador de teste, os dados de exibição retornado por uma ação do controlador de teste e testar se uma ação de controlador redireciona para uma segunda ação de controlador ou não.

## <a name="creating-the-controller-under-test"></a>Criando o controlador de teste

Vamos começar criando o controlador que pretendemos para testar. O controlador, chamado de `ProductController`, está contida na listagem 1.

**Listando 1 – `ProductController.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample1.vb)]

O `ProductController` contém dois métodos de ação denominados `Index()` e `Details()`. Ambos os métodos de ação retornam uma exibição. Observe que o `Details()` ação aceita um parâmetro chamado ID.

## <a name="testing-the-view-returned-by-a-controller"></a>O modo de exibição de teste retornado por um controlador

Imagine que queremos testar se ou não o `ProductController` retorna a exibição à direita. Certifique-se de que, quando o `ProductController.Details()` ação é invocada, a exibição de detalhes é retornada. A classe de teste na listagem 2 contém um teste de unidade para testar o modo de exibição retornado pelo `ProductController.Details()` ação.

**A listagem 2 – `ProductControllerTest.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample2.vb)]

A classe na lista 2 inclui um método de teste chamado `TestDetailsView()`. Este método contém três linhas de código. A primeira linha de código cria uma nova instância do `ProductController` classe. A segunda linha do código invoca o controlador `Details()` método de ação. Por fim, a última linha do código verifica ou não o modo de exibição é retornado pelo `Details()` ação é o modo de exibição de detalhes.

O `ViewResult.ViewName` propriedade representa o nome da exibição retornado por um controlador. Um aviso grande sobre essa propriedade de teste. Há duas maneiras que um controlador pode retornar uma exibição. Um controlador pode retornar explicitamente uma exibição como este:

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample3.vb)]

Como alternativa, o nome do modo de exibição pode ser inferido do nome da ação do controlador como este:

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample4.vb)]

Esta ação de controlador também retorna uma exibição chamada `Details`. No entanto, o nome do modo de exibição é inferido do nome da ação. Se você quiser testar o nome de exibição, explicitamente deve retornar o nome de exibição de ação do controlador.

Você pode executar o teste de unidade na lista 2 inserindo a combinação de teclado **Ctrl-R, A** ou clicando o **executar todos os testes na solução** botão (consulte a Figura 1). Se o teste é aprovado, você verá a janela de resultados do teste na Figura 2.


[![Executar todos os testes na solução](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image1.png)

**Figura 01**: executar todos os testes na solução ([clique para exibir a imagem em tamanho normal](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image3.png))


[![Sucesso!](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image4.png)

**Figura 02**: êxito! ([Clique para exibir a imagem em tamanho normal](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image6.png))


## <a name="testing-the-view-data-returned-by-a-controller"></a>Os dados de exibição de teste retornado por um controlador

Um controlador MVC transmite dados a uma exibição usando algo chamado *`View Data`*. Por exemplo, imagine que você deseja exibir os detalhes de um produto em particular, quando você invoca o `ProductController Details()` ação. Nesse caso, você pode criar uma instância de um `Product` classe (definido no modelo) e passe a instância para o `Details` exibição aproveitando `View Data`.

A modificação `ProductController` na listagem 3 inclui atualizada `Details()` ação que retorna um produto.

**A listagem 3 – `ProductController.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample5.vb)]

Primeiro, o `Details()` ação cria uma nova instância do `Product` a classe que representa um laptop. Em seguida, a instância do `Product` classe é passada como o segundo parâmetro para o `View()` método.

Você pode escrever testes de unidade testar se os dados esperados são contido na exibição dados. O teste de unidade em testes de listagem 4 ou não um produto que representa um computador laptop é retornado ao chamar o `ProductController Details()` método de ação.

**A listagem 4 – `ProductControllerTest.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample6.vb)]

Na listagem 4, o `TestDetailsView()` método testa a exibir dados retornados ao invocar o `Details()` método. O `ViewData` é exposto como uma propriedade de `ViewResult` retornado ao chamar o `Details()` método. O `ViewData.Model` propriedade contém o produto passado para o modo de exibição. O teste simplesmente verifica se o produto contido nos dados de exibição tem o nome do Laptop.

## <a name="testing-the-action-result-returned-by-a-controller"></a>O resultado da ação de teste retornado por um controlador

Uma ação do controlador mais complexa pode retornar tipos diferentes de resultados de ação dependendo dos valores dos parâmetros passados para a ação de controlador. Uma ação do controlador pode retornar uma variedade de tipos de ação resultados incluindo uma `ViewResult`, `RedirectToRouteResult`, ou `JsonResult`.

Por exemplo, a modificação `Details()` ação na listagem 5 retorna o `Details` exibir quando você passar um Id de produto válida para a ação. Se você passar um produto inválida Id – uma Id com um valor menor que 1 – em seguida, você será redirecionado para a `Index()` ação.

**Listando 5 – `ProductController.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample7.vb)]

Você pode testar o comportamento do `Details()` ação com o teste de unidade na listagem 6. O teste de unidade na listagem 6 verifica se você é redirecionado para a `Index` exibir quando um Id com o valor -1 é passado para o `Details()` método.

**Listando 6 – `ProductControllerTest.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample8.vb)]

Quando você chama o `RedirectToAction()` método em uma ação de controlador, a ação de controlador retorna um `RedirectToRouteResult`. O teste verifica se o `RedirectToRouteResult` redirecionará o usuário para uma ação de controlador chamada `Index`.

## <a name="summary"></a>Resumo

Neste tutorial, você aprendeu como criar testes de unidade para ações do controlador MVC. Primeiro, você aprendeu como verificar se a exibição à direita é retornada por uma ação do controlador. Você aprendeu a usar o `ViewResult.ViewName` propriedade para verificar o nome de uma exibição.

Em seguida, examinamos como você pode testar o conteúdo de `View Data`. Você aprendeu como verificar se o produto correto foi retornado em `View Data` depois de chamar uma ação do controlador.

Por fim, discutimos como você pode testar se os tipos diferentes de resultados de ação são retornados de uma ação do controlador. Você aprendeu como testar se um controlador retorna um `ViewResult` ou `RedirectToRouteResult`.

> [!div class="step-by-step"]
> [Anterior](creating-unit-tests-for-asp-net-mvc-applications-cs.md)
