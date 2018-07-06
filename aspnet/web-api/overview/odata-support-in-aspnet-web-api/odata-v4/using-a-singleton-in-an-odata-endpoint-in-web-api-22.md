---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
title: Criar um Singleton no OData v4 usando a API Web 2.2 | Microsoft Docs
author: rick-anderson
description: Este tópico mostra como definir um singleton em um ponto de extremidade OData na Web API 2.2.
ms.author: aspnetcontent
ms.date: 06/27/2014
ms.assetid: 4064ab14-26ee-4d5c-ae58-1bdda525ad06
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: baccfed79949ae4efd45395bed3ad0058549cb4c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830510"
---
<a name="create-a-singleton-in-odata-v4-using-web-api-22"></a>Criar um Singleton no OData v4 usando a API Web 2.2
====================
por Zoe Luo

> Tradicionalmente, uma entidade pode ser acessada somente se ele foi encapsulado dentro de um conjunto de entidades. Mas o OData v4 fornece duas opções adicionais, Singleton e contenção, que oferece suporte da API Web 2.2.


Este artigo mostra como definir um singleton em um ponto de extremidade OData na Web API 2.2. Para obter informações sobre quais um singleton é e como você pode se beneficiar de usá-lo, consulte [usando um singleton para definir sua entidade especial](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx). Para criar um ponto de extremidade OData V4 no API da Web, consulte [criar um OData v4 ponto de extremidade usando API Web ASP.NET 2.2](create-an-odata-v4-endpoint.md). 

Vamos criar um singleton em seu projeto de API da Web usando o modelo de dados a seguir:

![Modelo de dados](using-a-singleton-in-an-odata-endpoint-in-web-api-22/_static/image1.png)

Um singleton chamado `Umbrella` serão definidos com base no tipo `Company`e um conjunto nomeado de entidades `Employees` será definida com base no tipo `Employee`.

A solução usada neste tutorial pode ser baixada em [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).

## <a name="define-the-data-model"></a>Definir o modelo de dados

1. Defina os tipos CLR.

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample1.cs)]
2. Gere o modelo EDM com base nos tipos de CLR.

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample2.cs)]

    Aqui, `builder.Singleton<Company>("Umbrella")` informa o construtor de modelo para criar um singleton chamado `Umbrella` no modelo de EDM.

    Os metadados gerados terão a seguinte aparência:

    [!code-xml[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample3.xml)]

    Metadados do podemos ver que a propriedade de navegação `Company` no `Employees` conjunto de entidades está associado ao singleton `Umbrella`. A associação é feita automaticamente pelo `ODataConventionModelBuilder`, já que só `Umbrella` tem o `Company` tipo. Se houver qualquer ambiguidade no modelo, você pode usar `HasSingletonBinding` para associar explicitamente uma propriedade de navegação em um singleton; `HasSingletonBinding` tem o mesmo efeito que usar o `Singleton` atributo na definição de tipo CLR:

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample4.cs)]

## <a name="define-the-singleton-controller"></a>Definir o controlador de singleton

Como o controlador de EntitySet, o controlador de singleton herda `ODataController`, e o nome do controlador de singleton deve ser `[singletonName]Controller`.

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample5.cs)]

Para lidar com diferentes tipos de solicitações, ações devem ser predefinidos no controlador. **Roteamento de atributo** é habilitado por padrão na API Web 2.2. Por exemplo, para definir uma ação para tratar a consultar `Revenue` de `Company` usando o roteamento de atributo, use o seguinte:

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample6.cs)]

Se você não estiver disposto a definir os atributos de cada ação, basta definir as ações a seguir [convenções de roteamento OData](../odata-routing-conventions.md). Como uma chave não é necessária para consultar um singleton, as ações definidas no controlador singleton são ligeiramente diferentes das ações definidas no controlador entityset.

Para referência, as assinaturas de método para cada definição de ação no controlador singleton estão listadas abaixo.

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample7.cs)]

Basicamente, isso é tudo o que você precisa fazer no lado do serviço. O [projeto de exemplo](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) contém todo o código para a solução e o cliente do OData que mostra como usar o singleton. O cliente é criado seguindo as etapas em [criar um aplicativo de cliente do OData v4](create-an-odata-v4-client-app.md).

. 

*Graças ao Leo Hu para o conteúdo original deste artigo.*
