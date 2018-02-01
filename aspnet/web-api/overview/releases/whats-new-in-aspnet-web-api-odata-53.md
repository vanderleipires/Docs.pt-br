---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
title: "O que há de novo no ASP.NET Web API OData 5.3 | Microsoft Docs"
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/16/2014
ms.topic: article
ms.assetid: e39eaa25-83ff-41dc-869d-3818d59a88ae
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
msc.type: authoredcontent
ms.openlocfilehash: e918f86ebd813f31ad0c21566e617482fc7dd963
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-web-api-odata-53"></a>O que há de novo no ASP.NET Web API OData 5.3
====================
por [Microsoft](https://github.com/microsoft)

Este tópico descreve o que há de novo para ASP.NET Web API OData 5.3.

- [Baixar](#download)
- [Documentação](#documentation)
- [Bibliotecas do núcleo do OData](#corelib)
- [Novos recursos](#newf)
- [Problemas conhecidos e as alterações recentes](#known-issues)
- [Correções de bugs](#bug-fixes)
- [ASP.NET Web API OData 5.3.1](#OD)

<a id="download"></a>
## <a name="download"></a>Baixar

Os recursos de tempo de execução são lançados como pacotes do NuGet na Galeria do NuGet. Você pode instalar ou atualizar para os pacotes do NuGet lançados usando o NuGet Package Manager Console:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Documentação

Você pode encontrar tutoriais e outra documentação sobre o ASP.NET Web API OData no [site da web ASP.NET](../odata-support-in-aspnet-web-api/index.md).

<a id="corelib"></a>
## <a name="odata-core-libraries"></a>Bibliotecas do núcleo do OData

Para OData v4, API da Web usa o ODataLib versão 6.5.0 agora

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-odata-53"></a>Novos recursos no ASP.NET Web API OData 5.3

### <a name="support-for-levels-in-expand"></a>Suporte para $levels em $Expandir

Você pode usar o $levels opção em $expanda consultas de consulta. Por exemplo:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample2.cmd)]

Essa consulta é equivalente a:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample3.cmd)]

<a id="open-entity-types"></a>
### <a name="support-for-open-entity-types"></a>Suporte para tipos de entidade aberto

Um *abrir tipo* é um tipo de stuctured que contém as propriedades dinâmicas, além de quaisquer propriedades que são declaradas na definição do tipo. Tipos abertos permitem adicionar flexibilidade a seus modelos de dados. Para obter mais informações, consulte xxxx.

### <a name="support-for-dynamic-collection-properties-in-open-types"></a>Suporte para propriedades de coleção dinâmica em tipos abertos

Anteriormente, uma propriedade dinâmica tiveram de ser um único valor. Em 5.3, propriedades dinâmicas podem ter valores de coleção. Por exemplo, na seguinte carga JSON, o `Emails` propriedade é uma propriedade dinâmica e de coleção de tipo de cadeia de caracteres:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample4.cmd)]

### <a name="support-for-inheritance-for-complex-types"></a>Suporte de herança para tipos complexos

Agora os tipos complexos podem herdar de um tipo base. Por exemplo, um serviço OData pode definir os seguintes tipos complexos:

[!code-csharp[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample5.cs)]

Aqui está o EDM para este exemplo:

[!code-xml[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample6.xml?highlight=8,15)]

Para obter mais informações, consulte [exemplo de herança de tipo complexo do OData](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemas conhecidos e as alterações recentes

Esta seção descreve problemas conhecidos e as alterações recentes no ASP.NET Web API OData 5.3.

### <a name="odata-v4"></a>OData v4

#### <a name="query-options"></a>Opções de consulta

Problema: Usando aninhada $expand com $levels = max resulta em uma profundidade de expansão incorreto.

Por exemplo, dada a seguinte solicitação:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample7.cmd)]

Se `MaxExpansionDepth` for 5, essa consulta pode resultar em uma profundidade de expansão de 6.

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Correções de bugs e atualizações de recursos secundários

Esta versão também inclui várias correções de bugs e recursos secundários atualizações. Você pode encontrar a lista completa aqui:

- [Correções de bug](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.3%20Beta&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="OD"></a>
## <a name="aspnet-web-api-odata-531"></a>ASP.NET Web API OData 5.3.1

Nesta versão fizemos um [correção de bug](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.3.1%20Beta&amp;assignedTo=All&amp;component=Web%20API%20OData&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=All) para algumas das enumerações de AllowedFunctions. Esta versão não tem outras correções de bugs ou novos recursos.
