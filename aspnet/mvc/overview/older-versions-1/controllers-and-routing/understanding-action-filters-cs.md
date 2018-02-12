---
uid: mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-cs
title: "Noções básicas sobre filtros de ação (c#) | Microsoft Docs"
author: microsoft
description: "O objetivo deste tutorial é explicar filtros de ação. Um filtro de ação é um atributo que você pode aplicar uma ação do controlador – ou um controlador inteiro..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/16/2008
ms.topic: article
ms.assetid: a94e4e81-40c1-47b7-8613-126a1a6cc93d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-cs
msc.type: authoredcontent
ms.openlocfilehash: 86d5d429d9900d4c04391804598626705e6c88b4
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/11/2018
---
<a name="understanding-action-filters-c"></a>Noções básicas sobre filtros de ação (c#)
====================
por [Microsoft](https://github.com/microsoft)

[Baixar PDF](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_14_CS.pdf)

> O objetivo deste tutorial é explicar filtros de ação. Um filtro de ação é um atributo que você pode aplicar uma ação do controlador – ou um inteiro controller--que modifica o modo no qual a ação é executada.


## <a name="understanding-action-filters"></a>Noções básicas sobre filtros de ação

O objetivo deste tutorial é explicar filtros de ação. Um filtro de ação é um atributo que você pode aplicar uma ação do controlador – ou um inteiro controller--que modifica o modo no qual a ação é executada. A estrutura ASP.NET MVC inclui vários filtros de ação:

- OutputCache – esse filtro de ação armazena em cache a saída de uma ação do controlador para um período de tempo especificado.
- HandleError – esse filtro de ação manipula erros gerados quando uma ação do controlador é executado.
- Autorizar – esse filtro de ação permite restringir o acesso a um determinado usuário ou função.

Você também pode criar seus próprios filtros de ação personalizada. Por exemplo, você talvez queira criar um filtro de ação personalizada para implementar um sistema de autenticação personalizada. Ou, você talvez queira criar um filtro de ação que modifica os dados de exibição retornados por uma ação do controlador.

Neste tutorial, você aprenderá como criar um filtro de ação de ponta a ponta. Podemos criar um filtro de ação de Log que registra diferentes estágios do processamento de uma ação para a janela de saída do Visual Studio.

### <a name="using-an-action-filter"></a>Usando um filtro de ação

Um filtro de ação é um atributo. Você pode aplicar a maioria dos filtros de ação para uma ação de controlador individual ou um controlador de inteiro.

Por exemplo, o controlador de dados na listagem 1 expõe uma ação chamada `Index()` que retorna a hora atual. Essa ação é decorada com o `OutputCache` filtro de ação. Esse filtro faz com que o valor retornado a ação a ser armazenado em cache por 10 segundos.

**Listando 1 –`Controllers\DataController.cs`**

[!code-csharp[Main](understanding-action-filters-cs/samples/sample1.cs)]

Se você chamar repetidamente o `Index()` ação inserindo o URL/dados/índice na barra de endereços do navegador e atingir a atualização botão várias vezes, em seguida, você verá a mesma hora de 10 segundos. A saída de `Index()` ação é armazenado em cache por 10 segundos (consulte a Figura 1).


[![Tempo em cache](understanding-action-filters-cs/_static/image2.png)](understanding-action-filters-cs/_static/image1.png)

**Figura 01**: armazenado em cache o tempo ([clique para exibir a imagem em tamanho normal](understanding-action-filters-cs/_static/image3.png))


Na listagem 1, um filtro de ação única – o `OutputCache` filtro de ação – é aplicado a `Index()` método. Se você precisar, você pode aplicar vários filtros de ação para a mesma ação. Por exemplo, você pode querer aplicar ambas as `OutputCache` e `HandleError` filtros de ação para a mesma ação.

Na listagem 1, o `OutputCache` ação de filtro é aplicada a `Index()` ação. Você também pode aplicar esse atributo para o `DataController` classe em si. Nesse caso, o resultado retornado por qualquer ação exposta pelo controlador de deve ser armazenada em cache por 10 segundos.

### <a name="the-different-types-of-filters"></a>Os diferentes tipos de filtros

A estrutura ASP.NET MVC oferece suporte a quatro tipos diferentes de filtros:

1. Filtros de autorização – implementa a `IAuthorizationFilter` atributo.
2. Filtros de ação – implementa a `IActionFilter` atributo.
3. Resultar filtros – implementa a `IResultFilter` atributo.
4. Filtros de exceção – implementa a `IExceptionFilter` atributo.

Os filtros são executados na ordem listada acima. Por exemplo, filtros de autorização são sempre executados antes dos filtros de ação e filtros de exceção sempre são executados depois que todos os outros tipos de filtro.

Filtros de autorização são usados para implementar a autenticação e autorização para ações do controlador. Por exemplo, o filtro de autorização é um exemplo de um filtro de autorização.

Filtros de ação contêm a lógica que é executada antes e depois executa uma ação do controlador. Você pode usar um filtro de ação, por exemplo, para modificar os dados de exibição retorna uma ação do controlador.

Filtros de resultado contêm a lógica que é executada antes e depois de um resultado de exibição é executado. Por exemplo, você talvez queira modificar o resultado de exibição antes que a exibição é renderizada no navegador.

Filtros de exceção são o último tipo de filtro a ser executado. Você pode usar um filtro de exceção para tratar erros gerados pela ações do controlador ou resultados de ação do controlador. Você também pode usar filtros de exceção para registrar erros.

Cada tipo diferente de filtro é executado em uma ordem específica. Se você quiser controlar a ordem na qual os filtros do mesmo tipo são executados, em seguida, você pode definir a propriedade de ordem do filtro.

A classe base para todos os filtros de ação é o `System.Web.Mvc.FilterAttribute` classe. Se você deseja implementar um tipo específico de filtro, você precisa criar uma classe que herda da classe base do filtro e implementa uma ou mais do `IAuthorizationFilter`, `IActionFilter`, `IResultFilter`, ou `IExceptionFilter` interfaces.

### <a name="the-base-actionfilterattribute-class"></a>A classe de ActionFilterAttribute Base

Para tornar mais fácil para você implementar um filtro de ação personalizada, a estrutura ASP.NET MVC inclui uma base de `ActionFilterAttribute` classe. Essa classe implementa ambos o `IActionFilter` e `IResultFilter` interfaces e herda o `Filter` classe.

A terminologia usada aqui não é totalmente consistente. Tecnicamente, uma classe que herda da classe ActionFilterAttribute é um filtro de ação e um filtro de resultados. No entanto, no sentido flexível, o filtro de ação do word é usado para se referir a qualquer tipo de filtro na estrutura do ASP.NET MVC.

A base de `ActionFilterAttribute` classe possui os seguintes métodos que você pode substituir:

- OnActionExecuting – este método é chamado antes de uma ação do controlador é executada.
- OnActionExecuted – este método é chamado depois que uma ação do controlador é executada.
- OnResultExecuting – este método é chamado antes de um resultado de ação do controlador é executado.
- OnResultExecuted – este método é chamado depois que um resultado de ação do controlador é executado.

Na próxima seção, veremos como você pode implementar cada um desses métodos diferentes.

### <a name="creating-a-log-action-filter"></a>Criar um filtro de ação de Log

Para ilustrar como você pode criar um filtro de ação personalizada, vamos criar um filtro de ação personalizada que registra os estágios de processamento de uma ação do controlador para a janela de saída do Visual Studio. Nosso `LogActionFilter` está contida na listagem 2.

**A listagem 2 –`ActionFilters\LogActionFilter.cs`**

[!code-csharp[Main](understanding-action-filters-cs/samples/sample2.cs)]

Na listagem 2, o `OnActionExecuting()`, `OnActionExecuted()`, `OnResultExecuting()`, e `OnResultExecuted()` todos os métodos de chamar o `Log()` método. O nome do método e os dados da rota atual é passado para o `Log()` método. O `Log()` método grava uma mensagem para a janela de saída do Visual Studio (consulte a Figura 2).


[![Gravando para a janela de saída do Visual Studio](understanding-action-filters-cs/_static/image5.png)](understanding-action-filters-cs/_static/image4.png)

**Figura 02**: gravando para a janela de saída do Visual Studio ([clique para exibir a imagem em tamanho normal](understanding-action-filters-cs/_static/image6.png))


O controlador Home na listagem 3 ilustra como você pode aplicar o filtro de ação de Log para uma classe inteira de controlador. Sempre que qualquer uma das ações expostas pelo controlador Home são invocados – ou o `Index()` método ou o `About()` método – os estágios de processamento que efetuou a ação para a janela de saída do Visual Studio.

**A listagem 3 –`Controllers\HomeController.cs`**

[!code-csharp[Main](understanding-action-filters-cs/samples/sample3.cs)]

### <a name="summary"></a>Resumo

Neste tutorial, você foram introduzidos para filtros de ação do ASP.NET MVC. Você aprendeu sobre os quatro tipos diferentes de filtros: filtros de autorização, filtros de ação, filtros de resultado e filtros de exceção. Você também aprendeu sobre a base `ActionFilterAttribute` classe.

Por fim, você aprendeu como implementar um filtro de ação simples. Criamos um filtro de ação de Log que registra os estágios de processamento de uma ação do controlador para a janela de saída do Visual Studio.

>[!div class="step-by-step"]
[Anterior](asp-net-mvc-routing-overview-cs.md)
[Próximo](improving-performance-with-output-caching-cs.md)
