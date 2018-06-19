---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
title: Contenção em OData v4 usando Web API 2.2 | Microsoft Docs
author: rick-anderson
description: Tradicionalmente, uma entidade pode ser acessada apenas se ele foi encapsulado dentro de um conjunto de entidades. Mas o OData v4 fornece duas opções adicionais, Singleton e Con...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 5fbfefad-a17a-4c46-8646-f1ccd154cd56
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 7d3c81bf3d2a43faa3e71155637e031f81143782
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26507995"
---
<a name="containment-in-odata-v4-using-web-api-22"></a>Contenção em OData v4 usando 2.2 da API da Web
====================
por Jinfu Tan

> Tradicionalmente, uma entidade pode ser acessada apenas se ele foi encapsulado dentro de um conjunto de entidades. Mas o OData v4 fornece duas opções adicionais, Singleton e contenção, que suporta WebAPI 2.2.


Este tópico mostra como definir um conjunto de contenção em um ponto de extremidade OData nas WebApi 2.2. Para obter mais informações sobre contenção, consulte [contenção vem com o OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx). Para criar um ponto de extremidade OData V4 na API da Web, consulte [criar uma instalação do OData v4 ponto de extremidade usando ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).

Primeiro, vamos criar um modelo de domínio de contenção no serviço do OData, usando esse modelo de dados:

![Modelo de dados](odata-containment-in-web-api-22/_static/image1.png)

Uma conta contém muitos PaymentInstruments PI (), mas não definem uma conjunto de entidades para um PI. Em vez disso, os PIs só podem ser acessados através de uma conta.

Você pode baixar a solução usada neste tópico de [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).

## <a name="defining-the-data-model"></a>Definindo o modelo de dados

1. Defina os tipos CLR.

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample1.cs)]

    O `Contained` atributo é usado para propriedades de navegação de contenção.
2. Gere o modelo EDM com base nos tipos de CLR.

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample2.cs)]

    O `ODataConventionModelBuilder` tratará a criação do modelo EDM se o `Contained` atributo é adicionado à propriedade de navegação correspondente. Se a propriedade é um tipo de coleção, um `GetCount(string NameContains)` função também será criada.

    Os metadados gerado serão a seguinte aparência:

    [!code-xml[Main](odata-containment-in-web-api-22/samples/sample3.xml?highlight=10)]

    O `ContainsTarget` atributo indica que a propriedade de navegação é um conjunto de contenção.

## <a name="define-the-containing-entity-set-controller"></a>Defina o controlador de conjunto de entidade que contém

Entidades independentes não tem seu próprio controlador; a ação é definida no controlador de conjunto de entidade que contém. Neste exemplo, há um AccountsController, mas não PaymentInstrumentsController.

[!code-csharp[Main](odata-containment-in-web-api-22/samples/sample4.cs)]

Se o caminho OData é 4 ou mais segmentos, atributo somente roteamento funciona, como `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` no controlador acima. Caso contrário, o atributo e o roteamento convencional funciona: por exemplo, `GetPayInPIs(int key)` corresponde `GET ~/Accounts(1)/PayinPIs`.

*Agradecimentos Leo Hu para o conteúdo original deste artigo.*
