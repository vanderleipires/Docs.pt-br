---
uid: mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-vb
title: Noções básicas sobre filtros de ação (VB) | Microsoft Docs
author: microsoft
description: O objetivo deste tutorial é explicar os filtros de ação. Um filtro de ação é um atributo que você pode aplicar a uma ação do controlador – ou um controlador inteiro...
ms.author: riande
ms.date: 10/16/2008
ms.assetid: e83812f2-c53e-4a43-a7c1-d64c59ecf694
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-vb
msc.type: authoredcontent
ms.openlocfilehash: 0116306afdf21cb24a374013bb54ada54e5699ea
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830505"
---
<a name="understanding-action-filters-vb"></a>Noções básicas sobre filtros de ação (VB)
====================
por [Microsoft](https://github.com/microsoft)

[Baixar PDF](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_14_VB.pdf)

> O objetivo deste tutorial é explicar os filtros de ação. Um filtro de ação é um atributo que você pode aplicar uma ação do controlador – ou um controlador inteiro – que modifica a maneira na qual a ação é executada.


## <a name="understanding-action-filters"></a>Noções básicas sobre filtros de ação

O objetivo deste tutorial é explicar os filtros de ação. Um filtro de ação é um atributo que você pode aplicar uma ação do controlador – ou um controlador inteiro – que modifica a maneira na qual a ação é executada. A estrutura ASP.NET MVC inclui vários filtros de ação:

- OutputCache – esse filtro de ação armazena em cache a saída de uma ação do controlador para um período de tempo especificado.
- HandleError – esse filtro de ação manipula erros gerados quando uma ação do controlador é executado.
- Autorizar – esse filtro de ação lhe permite restringir o acesso a um determinado usuário ou função.

Você também pode criar seus próprios filtros de ação personalizada. Por exemplo, você talvez queira criar um filtro de ação personalizada para implementar um sistema de autenticação personalizada. Ou, você talvez queira criar um filtro de ação que modifica os dados de exibição retornados por uma ação do controlador.

Neste tutorial, você aprenderá como criar um filtro de ação desde o. Podemos criar um filtro de ação de Log que registra os diferentes estágios do processamento de uma ação para a janela de saída do Visual Studio.

### <a name="using-an-action-filter"></a>Usando um filtro de ação

Um filtro de ação é um atributo. Você pode aplicar a maioria dos filtros de ação para uma ação de controlador individual ou um controlador inteiro.

Por exemplo, o controlador de dados na listagem 1 expõe uma ação chamada `Index()` que retorna a hora atual. Essa ação é decorada com o `OutputCache` filtro de ação. Esse filtro faz com que o valor retornado pela ação a ser armazenado em cache por 10 segundos.

**Listagem 1 – `Controllers\DataController.vb`**

[!code-vb[Main](understanding-action-filters-vb/samples/sample1.vb)]

Se você invocar repetidamente a `Index()` ação inserindo a URL/dados/índice na barra de endereços do navegador e atingir a atualização do botão várias vezes, em seguida, você verá o mesmo tempo para 10 segundos. A saída a `Index()` ação é armazenado em cache por 10 segundos (veja a Figura 1).


[![Tempo em cache](understanding-action-filters-vb/_static/image2.png)](understanding-action-filters-vb/_static/image1.png)

**Figura 01**: armazenado em cache o tempo ([clique para exibir a imagem em tamanho normal](understanding-action-filters-vb/_static/image3.png))


Na listagem 1, um filtro de ação única – o `OutputCache` filtro de ação – é aplicada para o `Index()` método. Se você precisar, você pode aplicar vários filtros de ação para a mesma ação. Por exemplo, você pode querer aplicar ambas as `OutputCache` e `HandleError` filtros de ação para a mesma ação.

Na listagem 1, o `OutputCache` ação de filtro é aplicada a `Index()` ação. Você também pode aplicar esse atributo para o `DataController` própria classe. Nesse caso, o resultado retornado por qualquer ação exposta pelo controlador deve ser armazenado em cache por 10 segundos.

### <a name="the-different-types-of-filters"></a>Os diferentes tipos de filtros

O ASP.NET MVC framework oferece suporte a quatro tipos diferentes de filtros:

1. Filtros de autorização – implementa o `IAuthorizationFilter` atributo.
2. Filtros de ação – implementa o `IActionFilter` atributo.
3. Filtros – implementa de resultado a `IResultFilter` atributo.
4. Filtros de exceção – implementa o `IExceptionFilter` atributo.

Filtros são executados na ordem listada acima. Por exemplo, filtros de autorização são sempre executados antes dos filtros de ação e os filtros de exceção são sempre executados após todos os outros tipos de filtro.

Filtros de autorização são usados para implementar a autenticação e autorização para ações do controlador. Por exemplo, o filtro Authorize é um exemplo de um filtro de autorização.

Filtros de ação contêm a lógica que é executada antes e depois executa uma ação do controlador. Você pode usar um filtro de ação, por exemplo, para modificar os dados de exibição que retorna uma ação do controlador.

Filtros de resultado contêm a lógica que é executada antes e depois que um resultado de exibição é executado. Por exemplo, você talvez queira modificar o modo de exibição resultado imediatamente antes que a exibição é renderizada no navegador.

Filtros de exceção são o último tipo de filtro a ser executado. Você pode usar um filtro de exceção para tratar erros gerados por seus resultados de ação do controlador ou ações do controlador. Você também pode usar filtros de exceção para erros de log.

Todos os diferentes tipos de filtro é executado em uma ordem específica. Se você quiser controlar a ordem na qual os filtros do mesmo tipo são executados, em seguida, você pode definir a propriedade de ordem de um filtro.

A classe base para todos os filtros de ação é o `System.Web.Mvc.FilterAttribute` classe. Se você quiser implementar um determinado tipo de filtro, em seguida, você precisará criar uma classe que herda da classe base do filtro e implementa uma ou mais do IAuthorizationFilter, IActionFilter e IResultFilter, ou interfaces de ExceptionFilter.

### <a name="the-base-actionfilterattribute-class"></a>A classe de ActionFilterAttribute Base

Para tornar mais fácil de implementar um filtro de ação personalizada, a estrutura ASP.NET MVC inclui uma base de `ActionFilterAttribute` classe. Essa classe implementa ambos os `IActionFilter` e `IResultFilter` interfaces e herda o `Filter` classe.

A terminologia usada aqui não é totalmente consistente. Tecnicamente, uma classe que herda da classe ActionFilterAttribute é um filtro de ação e um filtro de resultado. No entanto, no sentido flexível, o filtro de ação do word é usado para se referir a qualquer tipo de filtro na estrutura do ASP.NET MVC.

A classe de ActionFilterAttribute base tem os seguintes métodos que você pode substituir:

- OnActionExecuting – esse método é chamado antes de uma ação do controlador é executada.
- OnActionExecuted – esse método é chamado depois que uma ação do controlador é executada.
- OnResultExecuting – esse método é chamado antes de um resultado de ação do controlador é executado.
- OnResultExecuted – esse método é chamado depois que um resultado de ação do controlador é executado.

Na próxima seção, veremos como você pode implementar cada um desses métodos diferentes.

### <a name="creating-a-log-action-filter"></a>Criando um filtro de ação do Log

Para ilustrar como você pode criar um filtro de ação personalizada, vamos criar um filtro de ação personalizada que registra em log os estágios do processamento de uma ação do controlador para a janela de saída do Visual Studio. Nosso `LogActionFilter` está contida na listagem 2.

**Listagem 2 – `ActionFilters\LogActionFilter.vb`**

[!code-vb[Main](understanding-action-filters-vb/samples/sample2.vb)]

Na listagem 2, o `OnActionExecuting()`, `OnActionExecuted()`, `OnResultExecuting()`, e `OnResultExecuted()` todos os métodos de chamar o `Log()` método. O nome do método e os dados de rota atuais é passado para o `Log()` método. O `Log()` método grava uma mensagem na janela de saída do Visual Studio (veja a Figura 2).


[![Escrever na janela de saída do Visual Studio](understanding-action-filters-vb/_static/image5.png)](understanding-action-filters-vb/_static/image4.png)

**Figura 02**: escrevendo para a janela de saída do Visual Studio ([clique para exibir a imagem em tamanho normal](understanding-action-filters-vb/_static/image6.png))


O controlador Home na listagem 3 ilustra como você pode aplicar o filtro de ação do Log para uma classe de controlador inteiro. Sempre que qualquer uma das ações expostas pelo controlador Home são invocados – ou o `Index()` método ou o `About()` método – os estágios do processamento de ação são registradas para a janela de saída do Visual Studio.

**Listagem 3 – `Controllers\HomeController.vb`**

[!code-vb[Main](understanding-action-filters-vb/samples/sample3.vb)]

### <a name="summary"></a>Resumo

Neste tutorial, você foi apresentado para filtros de ação do ASP.NET MVC. Você aprendeu sobre os quatro tipos diferentes de filtros: filtros de autorização, filtros de ação, filtros de resultado e filtros de exceção. Você também aprendeu sobre a base `ActionFilterAttribute` classe.

Por fim, você aprendeu como implementar um filtro de ação simples. Criamos um filtro de ação de Log que registra em log os estágios do processamento de uma ação do controlador para a janela de saída do Visual Studio.

> [!div class="step-by-step"]
> [Anterior](asp-net-mvc-routing-overview-vb.md)
> [Próximo](improving-performance-with-output-caching-vb.md)
