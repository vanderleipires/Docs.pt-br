---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-vb
title: "O ASP.NET MVC visão geral de roteamento (VB) | Microsoft Docs"
author: StephenWalther
description: "Neste tutorial, Stephen Walther mostra como a estrutura ASP.NET MVC mapeia solicitações do navegador para ações do controlador."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: 4bc8d19a-80f1-44b4-adbf-95ed22d691ca
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: 1e4c74e61b1a0d5f5020154756e34dd2fa507034
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-routing-overview-vb"></a>O ASP.NET MVC visão geral de roteamento (VB)
====================
por [Stephen Walther](https://github.com/StephenWalther)

> Neste tutorial, Stephen Walther mostra como a estrutura ASP.NET MVC mapeia solicitações do navegador para ações do controlador.


Neste tutorial, você conheça um recurso importante de todos os aplicativos ASP.NET MVC chamado *roteamento ASP.NET*. O módulo de roteamento do ASP.NET é responsável por mapeamento de solicitações do navegador para determinadas ações do controlador MVC. No final deste tutorial, você entenderá como a tabela de rotas padrão mapeia solicitações para ações do controlador.

## <a name="using-the-default-route-table"></a>Usando a tabela de rota padrão

Quando você cria um novo aplicativo ASP.NET MVC, o aplicativo já está configurado para usar o roteamento do ASP.NET. Roteamento ASP.NET está configurado em dois locais.

Primeiro, o roteamento ASP.NET está habilitado no arquivo de configuração do aplicativo Web (arquivo Web. config). Há quatro seções no arquivo de configuração que são relevantes para roteamento: seção system.web.httpModules, seção system.web.httpHandlers, seção system.webserver.modules e a seção system.webserver.handlers. Tenha cuidado para não excluir essas seções porque sem essas seções roteamento deixará de funcionar.

Segundo e mais importante, uma tabela de rota é criada no arquivo global asax do aplicativo. O arquivo global. asax é um arquivo especial que contém manipuladores de eventos para eventos de ciclo de vida do aplicativo ASP.NET. A tabela de rotas é criada durante o início do aplicativo do evento.

O arquivo na listagem 1 contém o arquivo global asax padrão para um aplicativo ASP.NET MVC.

**Listando 1 - Global.asax.vb**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample1.vb)]

Quando um aplicativo MVC iniciado pela primeira vez, o aplicativo\_método Start () é chamado. Esse método, por sua vez, chama o método RegisterRoutes(). O método RegisterRoutes() cria a tabela de rotas.

A tabela de rota padrão contém uma única rota (denominada Default). A rota padrão mapeia o primeiro segmento de URL para um nome de controlador, o segundo segmento de URL para uma ação do controlador e o terceiro segmento para um parâmetro denominado **id**.

Imagine que você digite a seguinte URL na barra de endereços do navegador da web:

Home/3/índice

A rota padrão mapeia essa URL para os seguintes parâmetros:

- controlador = início

- ação = índice

- ID = 3

Quando você solicita 3/índice/URL /Home, o código a seguir é executado:

HomeController.Index(3)

A rota padrão inclui padrões para todos os três parâmetros. Se você não fornecer um controlador, em seguida, o parâmetro de controlador padrão para o valor **início**. Se você não fornecer uma ação, o parâmetro de ação padrão para o valor **índice**. Por fim, se você não fornecer uma id, o parâmetro de id padrão é uma cadeia de caracteres vazia.

Vamos examinar alguns exemplos de como a rota padrão mapeia URLs para ações do controlador. Imagine que você digite a seguinte URL na barra de endereços do navegador:

/ Início

Devido os padrões de parâmetro de rota padrão, inserir esta URL fará com que o método Index () da classe HomeController na listagem 2 seja chamado.

**A listagem 2 - HomeController.vb**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample2.vb)]

Na listagem 2, a classe HomeController inclui um método chamado Index () que aceita um parâmetro único chamado ID. A URL /Home faz com que o método Index () seja chamado com o valor nada como o valor do parâmetro Id.

Por causa da forma a estrutura MVC invoca ações do controlador, o URL /Home também corresponde o método Index () da classe HomeController na listagem 3.

**A listagem 3 - HomeController.vb (ação de índice sem nenhum parâmetro)**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample3.vb)]

O método Index () na listagem 3 não aceita todos os parâmetros. A URL /Home fará com que esse método Index () seja chamado. 3/índice/URL /Home também chama esse método (a Id é ignorada).

A URL /Home também corresponde o método Index () da classe HomeController na listagem 4.

**A listagem 4 - HomeController.vb (ação de índice com parâmetro anulável)**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample4.vb)]

Na listagem 4, o método Index () tem um parâmetro de número inteiro. Como o parâmetro é um parâmetro nulo (pode ter o valor Nothing), o Index () pode ser chamado sem gerar um erro.

Por fim, chamar o método Index () na listagem 5 com a URL /Home gera uma exceção desde o parâmetro Id *não* um parâmetro nulo. Se você tentar chamar o método Index (), em seguida, você obterá o erro exibido na Figura 1.

**Listagem 5 - HomeController.vb (ação de índice com o parâmetro Id)**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample5.vb)]


[![Invocar uma ação do controlador que espera um valor de parâmetro](asp-net-mvc-routing-overview-vb/_static/image1.jpg)](asp-net-mvc-routing-overview-vb/_static/image1.png)

**Figura 01**: invocar uma ação do controlador que espera um valor de parâmetro ([clique para exibir a imagem em tamanho normal](asp-net-mvc-routing-overview-vb/_static/image2.png))


O URL /Home/índice/3, por outro lado, funciona muito bem com a ação de controlador de índice na listagem 5. A solicitação /Home/Index/3 faz com que o método Index () seja chamado com um parâmetro de Id que tem o valor 3.

## <a name="summary"></a>Resumo

O objetivo deste tutorial era fornecerá uma breve introdução ao roteamento ASP.NET. Examinamos a tabela de rota padrão que você obtém com um novo aplicativo ASP.NET MVC. Você aprendeu como a rota padrão mapeia URLs para ações do controlador.

>[!div class="step-by-step"]
[Anterior](creating-an-action-cs.md)
[Próximo](understanding-action-filters-vb.md)
