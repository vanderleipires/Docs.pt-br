---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
title: Criar um Singleton em OData v4 usando Web API 2.2 | Microsoft Docs
author: rick-anderson
description: "Este tópico mostra como definir um singleton em um ponto de extremidade OData na Web API 2.2."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 4064ab14-26ee-4d5c-ae58-1bdda525ad06
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 92c5056548b1e39defb28ac36f83b001dd108f5f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="create-a-singleton-in-odata-v4-using-web-api-22"></a>Criar um Singleton em OData v4 usando 2.2 da API da Web
====================
por Zoe Luo

> Tradicionalmente, uma entidade pode ser acessada apenas se ele foi encapsulado dentro de um conjunto de entidades. Mas o OData v4 fornece duas opções adicionais, Singleton e contenção, que suporta WebAPI 2.2.


Este artigo mostra como definir um singleton em um ponto de extremidade OData na Web API 2.2. Para obter informações sobre quais um singleton é e como você pode se beneficiar de utilizá-las, consulte [usando um singleton para definir sua entidade especial](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx). Para criar um ponto de extremidade OData V4 na API da Web, consulte [criar uma instalação do OData v4 ponto de extremidade usando ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md). 

Vamos criar um singleton em seu projeto de API da Web usando o modelo de dados a seguir:

![Modelo de dados](using-a-singleton-in-an-odata-endpoint-in-web-api-22/_static/image1.png)

Um singleton chamado `Umbrella` será definida com base no tipo `Company`e um conjunto nomeado de entidades `Employees` será definida com base no tipo `Employee`.

A solução usada neste tutorial pode ser baixada do [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).

## <a name="define-the-data-model"></a>Definir o modelo de dados

1. Defina os tipos CLR.

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample1.cs)]
2. Gere o modelo EDM com base nos tipos de CLR.

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample2.cs)]

    Aqui, `builder.Singleton<Company>("Umbrella")` informa o construtor de modelo para criar um singleton chamado `Umbrella` no modelo de EDM.

    Os metadados gerado serão a seguinte aparência:

    [!code-xml[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample3.xml)]

    Os metadados podemos ver que a propriedade de navegação `Company` no `Employees` conjunto de entidades é associado ao singleton `Umbrella`. A associação é feita automaticamente pelo `ODataConventionModelBuilder`, já que somente `Umbrella` tem o `Company` tipo. Se houver qualquer ambiguidade no modelo, você pode usar `HasSingletonBinding` para associar explicitamente uma propriedade de navegação a um singleton; `HasSingletonBinding` tem o mesmo efeito que usar o `Singleton` atributo na definição do tipo CLR:

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample4.cs)]

## <a name="define-the-singleton-controller"></a>Defina o controlador de singleton

Como o controlador de EntitySet, o controlador de singleton herda de `ODataController`, e o nome do controlador de singleton deve ser `[singletonName]Controller`.

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample5.cs)]

Para lidar com diferentes tipos de solicitações, as ações são necessárias para ser predefinidos no controlador. **Roteamento de atributo** é habilitado por padrão em WebApi 2.2. Por exemplo, para definir uma ação para tratar consultando `Revenue` de `Company` usando roteamento de atributo, use o seguinte:

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample6.cs)]

Se você não estiver disposto a definir os atributos de cada ação, basta definir as ações a seguir [convenções de roteamento OData](../odata-routing-conventions.md). Como uma chave não é necessária para consultar um singleton, as ações definidas no controlador singleton são ligeiramente diferentes das ações definidas no controlador entityset.

Para referência, assinaturas de método para cada definição de ação no controlador singleton estão listadas abaixo.

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample7.cs)]

Basicamente, isso é tudo o que você precisa fazer no lado do serviço. O [projeto de exemplo](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) contém todo o código para a solução e o cliente do OData que mostra como usar o singleton. O cliente é criado seguindo as etapas em [criar um aplicativo de cliente do OData v4](create-an-odata-v4-client-app.md).

. 

*Agradecimentos Leo Hu para o conteúdo original deste artigo.*
