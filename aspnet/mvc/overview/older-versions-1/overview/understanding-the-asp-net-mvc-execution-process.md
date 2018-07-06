---
uid: mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
title: Noções básicas sobre o processo de execução do ASP.NET MVC | Microsoft Docs
author: microsoft
description: Saiba como o ASP.NET MVC framework processa uma solicitação do navegador passo a passo.
ms.author: aspnetcontent
ms.date: 01/27/2009
ms.assetid: d1608db3-660d-4079-8c15-f452ff01f1db
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
msc.type: authoredcontent
ms.openlocfilehash: 34699c314eb862e91ecba8831c5092af9d47dc70
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823729"
---
<a name="understanding-the-aspnet-mvc-execution-process"></a>Noções básicas sobre o processo de execução do ASP.NET MVC
====================
por [Microsoft](https://github.com/microsoft)

> Saiba como o ASP.NET MVC framework processa uma solicitação do navegador passo a passo.


As solicitações para um aplicativo Web com base no ASP.NET MVC primeiro passam por meio de **UrlRoutingModule** objeto, que é um módulo HTTP. Esse módulo analisa a solicitação e executa a seleção de rota. O **UrlRoutingModule** objeto seleciona o primeiro objeto de rota que corresponde à solicitação atual. (Um objeto de rota é uma classe que implementa **RouteBase**, e normalmente é uma instância das **rota** classe.) Se nenhuma rota corresponde a, o **UrlRoutingModule** objeto não faz nada e permite que a solicitação de fallback para a solicitação ASP.NET ou IIS regular processamento.

De selecionado **rota** objeto, o **UrlRoutingModule** objeto obtém os **IRouteHandler** objeto que está associado com o **rota**objeto. Normalmente, em um aplicativo MVC, isso será uma instância do **MvcRouteHandler**. O **IRouteHandler** instância cria um **IHttpHandler** do objeto e o passa a **IHttpContext** objeto. Por padrão, o **IHttpHandler** da instância para o MVC é a **MvcHandler** objeto. O **MvcHandler** objeto, em seguida, seleciona o controlador que, por fim, tratará a solicitação.

> [!NOTE]
> Quando um aplicativo Web ASP.NET MVC é executado no IIS 7.0, sem extensão de nome de arquivo é necessário para projetos MVC. No entanto, no IIS 6.0, o manipulador requer que você mapear a extensão de nome de arquivo. MVC para a DLL ISAPI do ASP.NET.


O módulo e o manipulador são os pontos de entrada para a estrutura MVC do ASP.NET. Eles executam as seguintes ações:

- Selecione o controlador apropriado em um aplicativo Web do MVC.
- Obtenha uma instância de controlador específico.
- Chame o controlador **Execute** método.

O exemplo a seguir lista os estágios da execução de um projeto Web do MVC:

- Receber a primeira solicitação para o aplicativo 

    - No arquivo global. asax, **rota** objetos são adicionados para o **RouteTable** objeto.
- Executar roteamento 

    - O **UrlRoutingModule** módulo usa a primeira correspondência **rota** objeto o **RouteTable** coleção para criar o **RouteData** objeto, que, em seguida, ele usa para criar uma **RequestContext** (**IHttpContext**) objeto.
- Criar um manipulador de solicitação do MVC 

    - O **MvcRouteHandler** objeto cria uma instância das **MvcHandler** de classe e a passa a **RequestContext** instância.
- Criar um controlador 

    - O **MvcHandler** objeto usa o **RequestContext** instância para identificar o **IControllerFactory** objeto (geralmente uma instância do  **DefaultControllerFactory** classe) para criar a instância de controlador com.
- Controlador execute - o **MvcHandler** instância chama o controlador s **Execute** método. |
- Invocar ação 

    - A maioria dos controladores herdam de **controlador** classe base. Para controladores que fazer isso, o **ControllerActionInvoker** objeto que está associado com o controlador determina qual método de ação da classe do controlador para chamar e, em seguida, chama esse método.
- Resultado de execução 

    - Um método de ação típica pode receber entrada do usuário, preparar os dados de resposta apropriada e, em seguida, o resultado de execução, retornando um tipo de resultado. Os tipos de resultado internos que podem ser executados incluem o seguinte: **ViewResult** (que renderiza uma exibição e é o tipo de resultado usados com mais frequência), **RedirectToRouteResult**,  **RedirectResult**, **ContentResult**, **JsonResult**, e **EmptyResult**.
