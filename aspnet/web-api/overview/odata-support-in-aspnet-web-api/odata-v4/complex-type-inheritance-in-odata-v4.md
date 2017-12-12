---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: "Herança de tipo complexo no OData v4 com o ASP.NET Web API | Microsoft Docs"
author: microsoft
description: "Acordo com a especificação do OData v4, um tipo complexo pode herdar de outro tipo complexo. (Um tipo complexo é um tipo estruturado sem uma chave.) API da Web..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/16/2014
ms.topic: article
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: be2dbfa82b99b6c48928e4e767716852c14a463b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a>Herança de tipo complexo no v4 do OData com a API da Web do ASP.NET
====================
por [Microsoft](https://github.com/microsoft)

> Acordo com o OData v4 [especificação](http://www.odata.org/documentation/odata-version-4-0/), um tipo complexo pode herdar de outro tipo complexo. (Um *complexo* é um tipo estruturado sem uma chave.) API da Web OData 5.3 dá suporte a herança de tipo complexo.
> 
> Este tópico mostra como criar um modelo de dados de entidade (EDM) com tipos complexos de herança. Para o código-fonte completo, consulte [exemplo de herança de tipo complexo do OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - OData da API Web 5.3
> - OData v4


## <a name="model-hierarchy"></a>Hierarquia de modelo

Para ilustrar a herança de tipo complexo, vamos usar a seguinte hierarquia de classe.

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

`Shape`é um tipo complexo abstrato. `Rectangle`, `Triangle`, e `Circle` são tipos complexos derivados de `Shape`, e `RoundRectangle` deriva de `Rectangle`. `Window`é um tipo de entidade e contém um `Shape` instância.

Aqui estão as classes CLR que definem esses tipos.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a>Criar o modelo EDM

Para criar o EDM, você pode usar **ODataConventionModelBuilder**, qual infere as relações de herança dos tipos de CLR.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

Você também pode criar EDM explicitamente, usando **ODataModelBuilder**. Isso leva mais código, mas oferece mais controle sobre o EDM.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

Estes dois exemplos criam o mesmo esquema EDM.

## <a name="metadata-document"></a>Documento de metadados

Aqui está o documento de metadados OData, mostrando a herança de tipo complexo.

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

O documento de metadados, você pode ver que:

- O `Shape` tipo complexo é abstrato.
- O `Rectangle`, `Triangle`, e `Circle` tipo complexo de ter o tipo base `Shape`.
- O `RoundRectangle` tipo tem o tipo base `Rectangle`.

## <a name="casting-complex-types"></a>Conversão de tipos complexos

Agora há suporte para a conversão em tipos complexos. Por exemplo, a consulta a seguir conversões um `Shape` para um `Rectangle`.

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

Aqui está a carga de resposta:

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
