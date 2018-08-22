---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs
title: Visão geral o roteamento do ASP.NET MVC (c#) | Microsoft Docs
author: StephenWalther
description: Neste tutorial, Stephen Walther mostra como o ASP.NET MVC framework mapeia as solicitações do navegador para ações do controlador.
ms.author: riande
ms.date: 08/19/2008
ms.assetid: 5b39d2d5-4bf9-4d04-94c7-81b84dfeeb31
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: 188490c5ca075710dcbdcd1c325808f7c1d383bc
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825601"
---
<a name="aspnet-mvc-routing-overview-c"></a>Visão geral o roteamento do ASP.NET MVC (c#)
====================
por [Stephen Walther](https://github.com/StephenWalther)

> Neste tutorial, Stephen Walther mostra como o ASP.NET MVC framework mapeia as solicitações do navegador para ações do controlador.


Neste tutorial, você será apresentado a um recurso importante de todos os aplicativos ASP.NET MVC chamado *roteamento do ASP.NET*. O módulo de roteamento do ASP.NET é responsável por mapear solicitações recebidas do navegador para determinadas ações do controlador MVC. No final deste tutorial, você entenderá como a tabela de rota padrão mapeia solicitações para ações do controlador.

## <a name="using-the-default-route-table"></a>Usando a tabela de rota padrão

Quando você cria um novo aplicativo ASP.NET MVC, o aplicativo já está configurado para usar o roteamento do ASP.NET. O roteamento do ASP.NET está configurado em dois locais.

Primeiro, o roteamento do ASP.NET está habilitado no arquivo de configuração do seu aplicativo Web (arquivo Web. config). Há quatro seções no arquivo de configuração que são relevantes para roteamento: seção system.web.httpModules, a seção system.web.httpHandlers, a seção system.webserver.modules e a seção system.webserver.handlers. Tenha cuidado para não excluir essas seções, porque sem essas seções roteamento deixarão de funcionar.

Em segundo lugar e mais importante, uma tabela de rotas é criada no arquivo global asax do aplicativo. O arquivo global asax é um arquivo especial que contém os manipuladores de eventos para eventos de ciclo de vida do aplicativo ASP.NET. A tabela de rotas é criada durante o evento de início do aplicativo.

O arquivo na listagem 1 contém o arquivo global asax padrão para um aplicativo ASP.NET MVC.

**Listagem 1 - Global.asax.cs**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample1.cs)]

Quando um aplicativo MVC é iniciado, o aplicativo\_método Start () é chamado. Esse método, por sua vez, chama o método de RegisterRoutes(). O método RegisterRoutes() cria a tabela de rotas.

A tabela de rotas padrão contém uma única rota (denominada Default). A rota padrão mapeia o primeiro segmento de URL para um nome de controlador, o segundo segmento de URL para uma ação do controlador e o terceiro segmento para um parâmetro denominado **id**.

Imagine que você insira a seguinte URL na barra de endereços do navegador da web:

Página inicial/3/índice

A rota padrão mapeia essa URL para os seguintes parâmetros:

- controlador = início

- ação = índice

- id = 3

Quando você solicita 3/índice/URL /Home, o código a seguir é executado:

HomeController.Index(3)

A rota padrão inclui os padrões para todos os três parâmetros. Se você não fornecer um controlador, em seguida, o parâmetro de controlador padrão para o valor **Home**. Se você não fornecer uma ação, o parâmetro de ação padrão para o valor **índice**. Por fim, se você não fornecer uma id, o parâmetro id padrão é uma cadeia de caracteres vazia.

Vamos examinar alguns exemplos de como a rota padrão mapeia as URLs para ações do controlador. Imagine que você insira a seguinte URL na barra de endereços do navegador:

/ Home

Por causa dos padrões de parâmetro de rota padrão, inserir esta URL fará com que o método Index () da classe HomeController na listagem 2 a ser chamado.

**Listagem 2 - HomeController.cs**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample2.cs)]

Na listagem 2, a classe HomeController inclui um método chamado Index () que aceita um parâmetro único chamado ID. A URL /Home faz com que o método Index () seja chamado com uma cadeia de caracteres vazia como o valor do parâmetro Id.

Por causa da maneira que a estrutura do MVC invoca as ações do controlador, /Home a URL também corresponde o método Index () da classe HomeController na listagem 3.

**Listagem 3 - HomeController.cs (ação de índice sem nenhum parâmetro)**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample3.cs)]

O método Index () na listagem 3 não aceita parâmetros. O /Home URL fará com que esse método Index () seja chamado. 3/índice/URL /Home também invoca esse método (a Id é ignorada).

O /Home URL também corresponde o método Index () da classe HomeController na listagem 4.

**Listagem 4 - HomeController.cs (ação de índice com o parâmetro anulável)**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample4.cs)]

Na listagem 4, o método Index () tem um parâmetro de número inteiro. Como o parâmetro é um parâmetro anulável (pode ter o valor Null), o Index () pode ser chamado sem gerar um erro.

Por fim, invocando o método Index () na listagem 5 com a URL /Home faz com que uma exceção desde o parâmetro de Id *não é* um parâmetro que permite valor nulo. Se você tentar invocar o método Index (), em seguida, você obterá o erro exibido na Figura 1.

**Listagem 5 - HomeController.cs (ação de índice com o parâmetro de Id)**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample5.cs)]


[![Invocar uma ação do controlador que espera um valor de parâmetro](asp-net-mvc-routing-overview-cs/_static/image1.jpg)](asp-net-mvc-routing-overview-cs/_static/image1.png)

**Figura 01**: invocar uma ação do controlador que espera um valor de parâmetro ([clique para exibir a imagem em tamanho normal](asp-net-mvc-routing-overview-cs/_static/image2.png))


O URL /Home/índice/3, por outro lado, funciona muito bem com a ação de controlador de índice na listagem 5. A solicitação /Home/Index/3 faz com que o método Index () seja chamado com um parâmetro de Id que tem o valor 3.

## <a name="summary"></a>Resumo

O objetivo deste tutorial era fornecer uma breve introdução ao roteamento ASP.NET. Examinamos a tabela de rotas padrão que você obtém com um novo aplicativo ASP.NET MVC. Você aprendeu como a rota padrão mapeia as URLs para ações do controlador.

> [!div class="step-by-step"]
> [Avançar](understanding-action-filters-cs.md)
