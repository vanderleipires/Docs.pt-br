---
uid: mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-vb
title: Criando testes de unidade para aplicativos ASP.NET MVC (VB) | Microsoft Docs
author: StephenWalther
description: Saiba como criar testes de unidade para ações do controlador. Neste tutorial, Stephen Walther demonstra como testar se uma ação do controlador retorna um parti...
ms.author: riande
ms.date: 08/19/2008
ms.assetid: eb35710d-1d99-44ac-b61f-e50af8cb328a
msc.legacyurl: /mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-vb
msc.type: authoredcontent
ms.openlocfilehash: 2bce5456d9c5465156daf511d0f75a68b35cf7d9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834089"
---
<a name="creating-unit-tests-for-aspnet-mvc-applications-vb"></a>Criando testes de unidade para aplicativos ASP.NET MVC (VB)
====================
por [Stephen Walther](https://github.com/StephenWalther)

[Baixar PDF](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_07_VB.pdf)

> Saiba como criar testes de unidade para ações do controlador. Neste tutorial, Stephen Walther demonstra como testar se uma ação do controlador retorna uma exibição específica, retorna um conjunto de dados específico ou retorna um tipo diferente de resultado da ação.


O objetivo deste tutorial é demonstrar como você pode escrever o testes de unidade para os controladores no ASP.NET MVC aplicativos. Vamos discutir como criar três tipos diferentes de testes de unidade. Você aprenderá como testar a retornado por uma ação do controlador de exibição, como os dados de exibição retornados por uma ação do controlador de teste e como testar se uma ação de controlador redireciona você para uma segunda ação de controlador.

## <a name="creating-the-controller-under-test"></a>Criando o controlador de teste

Vamos começar criando o controlador que temos a intenção de teste. O controlador, chamado de `ProductController`, está contida na listagem 1.

**Listagem 1 – `ProductController.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample1.vb)]

O `ProductController` contém dois métodos de ação chamados `Index()` e `Details()`. Ambos os métodos de ação retornam uma exibição. Observe que o `Details()` ação aceita um parâmetro chamado ID.

## <a name="testing-the-view-returned-by-a-controller"></a>Testando a exibição retornada por um controlador

Imagine que queremos testar ou não o `ProductController` retorna a exibição à direita. Certifique-se de que, quando o `ProductController.Details()` ação é invocada, a exibição de detalhes é retornada. A classe de teste na listagem 2 contém um teste de unidade para testar a exibição retornada pelo `ProductController.Details()` ação.

**Listagem 2 – `ProductControllerTest.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample2.vb)]

A classe na listagem 2 inclui um método de teste chamado `TestDetailsView()`. Este método contém três linhas de código. A primeira linha de código cria uma nova instância do `ProductController` classe. A segunda linha de código invoca o controlador `Details()` método de ação. Por fim, a última linha de verificações de código ou não o modo de exibição é retornado pelo `Details()` ação é a exibição de detalhes.

O `ViewResult.ViewName` propriedade representa o nome da exibição retornada por um controlador. Uma importante advertência sobre como testar essa propriedade. Há duas maneiras que um controlador pode retornar uma exibição. Um controlador explicitamente pode retornar uma exibição como esta:

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample3.vb)]

Como alternativa, o nome do modo de exibição pode ser inferido do nome da ação de controlador, como este:

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample4.vb)]

Esta ação do controlador também retorna uma exibição nomeada `Details`. No entanto, o nome da exibição é inferido do nome da ação. Se você quiser testar o nome da exibição, explicitamente deve retornar o nome de exibição da ação de controlador.

Você pode executar o teste de unidade na listagem 2 digitando a combinação de teclado **Ctrl-R, A** ou clicando o **executar todos os testes na solução** botão (consulte a Figura 1). Se o teste for bem-sucedido, você verá a janela Test Results na Figura 2.


[![Executar todos os testes na solução](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image1.png)

**Figura 01**: executar todos os testes na solução ([clique para exibir a imagem em tamanho normal](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image3.png))


[![Sucesso!](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image4.png)

**Figura 02**: sucesso! ([Clique para exibir a imagem em tamanho normal](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image6.png))


## <a name="testing-the-view-data-returned-by-a-controller"></a>Teste os dados de exibição retornada por um controlador

Um controlador MVC passa dados para um modo de exibição usando algo chamado *`View Data`*. Por exemplo, imagine que você deseja exibir os detalhes de um produto específico, quando você invoca o `ProductController Details()` ação. Nesse caso, você pode criar uma instância de um `Product` classe (definido no modelo) e passar a instância para o `Details` exibição aproveitando `View Data`.

Modificado `ProductController` na listagem 3 inclui atualizada `Details()` ação que retorna um produto.

**Listagem 3 – `ProductController.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample5.vb)]

Primeiro, o `Details()` ação cria uma nova instância do `Product` classe que representa um computador laptop. Em seguida, a instância das `Product` classe é passado como o segundo parâmetro para o `View()` método.

Você pode escrever testes de unidade testar se os dados esperados estão contidos na exibição dados. O teste de unidade na listagem 4 testes se um produto que representa um computador laptop é retornado ao chamar o `ProductController Details()` método de ação.

**Listagem 4 – `ProductControllerTest.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample6.vb)]

Na listagem 4, o `TestDetailsView()` método testa os dados de exibição retornado ao invocar o `Details()` método. O `ViewData` é exposta como uma propriedade em de `ViewResult` retornado ao invocar o `Details()` método. O `ViewData.Model` propriedade contém o produto passado para o modo de exibição. O teste simplesmente verifica que o produto contido nos dados do modo de exibição tem o nome do Laptop.

## <a name="testing-the-action-result-returned-by-a-controller"></a>Testar o resultado de ação retornada por um controlador

Uma ação do controlador mais complexa pode retornar tipos diferentes de resultados de ação, dependendo dos valores dos parâmetros passados para a ação do controlador. Uma ação do controlador pode retornar uma variedade de tipos de resultados de ação, incluindo uma `ViewResult`, `RedirectToRouteResult`, ou `JsonResult`.

Por exemplo, a modificação `Details()` ação na listagem 5 retorna o `Details` exibir ao passar um Id de produto válida para a ação. Se você passar um produto inválida Id – uma Id com um valor menor que 1-- em seguida, você será redirecionado para o `Index()` ação.

**Listagem 5 – `ProductController.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample7.vb)]

Você pode testar o comportamento do `Details()` ação com o teste de unidade na listagem 6. O teste de unidade na listagem 6 verifica que você será redirecionado para o `Index` exibir quando uma Id com o valor -1 for passada para o `Details()` método.

**Listagem 6 – `ProductControllerTest.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample8.vb)]

Quando você chama o `RedirectToAction()` método em uma ação de controlador, a ação do controlador retorna um `RedirectToRouteResult`. O teste verifica se o `RedirectToRouteResult` redirecionará o usuário a uma ação de controlador chamada `Index`.

## <a name="summary"></a>Resumo

Neste tutorial, você aprendeu como criar testes de unidade para ações do controlador MVC. Primeiro, você aprendeu como verificar se a exibição à direita é retornada por uma ação do controlador. Você aprendeu a usar o `ViewResult.ViewName` propriedade para verificar o nome de uma exibição.

Em seguida, examinamos como você pode testar o conteúdo de `View Data`. Você aprendeu como verificar se o produto certo foi retornado em `View Data` depois de chamar uma ação do controlador.

Por fim, discutimos como você pode testar se os tipos diferentes de resultados de ação são retornados de uma ação do controlador. Você aprendeu como testar se um controlador retorna um `ViewResult` ou um `RedirectToRouteResult`.

> [!div class="step-by-step"]
> [Anterior](creating-unit-tests-for-asp-net-mvc-applications-cs.md)
