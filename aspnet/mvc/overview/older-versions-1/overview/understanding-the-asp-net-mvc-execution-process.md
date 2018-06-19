---
uid: mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
title: Noções básicas sobre o processo de execução do ASP.NET MVC | Microsoft Docs
author: microsoft
description: Saiba como a estrutura ASP.NET MVC processa uma solicitação do navegador passo a passo.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: d1608db3-660d-4079-8c15-f452ff01f1db
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
msc.type: authoredcontent
ms.openlocfilehash: 5837c6e49709d6b86ee52cd88ffd4759c1850544
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26500725"
---
<a name="understanding-the-aspnet-mvc-execution-process"></a>Noções básicas sobre o processo de execução do ASP.NET MVC
====================
por [Microsoft](https://github.com/microsoft)

> Saiba como a estrutura ASP.NET MVC processa uma solicitação do navegador passo a passo.


Solicitações para um aplicativo Web do ASP.NET MVC primeiro passam o **UrlRoutingModule** objeto, que é um módulo HTTP. Este módulo analisa a solicitação e executa a seleção de rotas. O **UrlRoutingModule** objeto seleciona o primeiro objeto de rota que corresponda à solicitação atual. (Um objeto de rota é uma classe que implementa **RouteBase**, e normalmente é uma ocorrência da **rota** classe.) Se nenhum rotas corresponderem, o **UrlRoutingModule** objeto não faz nada e permite que a solicitação de fallback para a solicitação ASP.NET ou IIS regular processamento.

Do selecionado **rota** objeto, o **UrlRoutingModule** objeto obtém o **IRouteHandler** objeto que está associado com o **rota**objeto. Normalmente, em um aplicativo MVC, isso será uma instância de **MvcRouteHandler**. O **IRouteHandler** instância cria um **IHttpHandler** de objeto e transmite o **IHttpContext** objeto. Por padrão, o **IHttpHandler** instância para MVC é o **MvcHandler** objeto. O **MvcHandler** objeto, em seguida, seleciona o controlador que, por fim, tratará a solicitação.

> [!NOTE]
> Quando um aplicativo ASP.NET MVC é executado no IIS 7.0, nenhuma extensão de nome de arquivo é necessária para projetos MVC. No entanto, no IIS 6.0, o manipulador requer que você mapear a extensão de nome de arquivo. MVC para a DLL ISAPI do ASP.NET.


O módulo e o manipulador são os pontos de entrada para a estrutura ASP.NET MVC. Eles executar as seguintes ações:

- Selecione o controlador apropriado em um aplicativo MVC.
- Obtenha uma instância do controlador específico.
- Chame o controlador **Execute** método.

A seguir lista as etapas de execução para um projeto MVC Web:

- Receber a primeira solicitação para o aplicativo 

    - No arquivo global. asax, **rota** objetos são adicionados para o **RouteTable** objeto.
- Executar o roteamento 

    - O **UrlRoutingModule** módulo usa a primeira correspondência **rota** objeto o **RouteTable** coleção para criar o **RouteData** objeto que ele usa para criar um **RequestContext** (**IHttpContext**) objeto.
- Criar um manipulador de solicitação do MVC 

    - O **MvcRouteHandler** objeto cria uma instância do **MvcHandler** classe e transmite o **RequestContext** instância.
- Criar o controlador 

    - O **MvcHandler** objeto usa o **RequestContext** instância para identificar o **IControllerFactory** objeto (normalmente uma instância do  **DefaultControllerFactory** classe) para criar a instância do controlador com.
- Controlador execute - o **MvcHandler** instância chama o controlador s **Execute** método. |
- Invocação de ação 

    - A maioria dos controladores herdam o **controlador** classe base. Para controladores que fazê-lo, o **ControllerActionInvoker** objeto que está associado com o controlador determina qual método de ação da classe do controlador para chamar e, em seguida, chama esse método.
- Execute o resultado 

    - Um método de ação típica pode receber entrada do usuário, preparar os dados de resposta apropriado e, em seguida, execute o resultado ao retornar um tipo de resultado. Os tipos de resultados internos que podem ser executados incluem o seguinte: **ViewResult** (que renderiza uma exibição e é o tipo de resultado com mais frequência usado), **RedirectToRouteResult**,  **RedirectResult**, **ContentResult**, **JsonResult**, e **EmptyResult**.
