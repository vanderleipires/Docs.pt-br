---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
title: Dando suporte a ações de OData em ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: 'Em OData, as ações são uma maneira de adicionar os comportamentos do servidor que não são facilmente definidos como operações CRUD nas entidades. Alguns usos para ações incluem: implemente...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 2d7b3aa2-aa47-4e6e-b0ce-3d65a1c6fe02
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
msc.type: authoredcontent
ms.openlocfilehash: acb369ca8f1bab8d7cad14c15f46cfd44beb9fdd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508385"
---
<a name="supporting-odata-actions-in-aspnet-web-api-2"></a>Dando suporte a ações de OData em ASP.NET Web API 2
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> Em OData, *ações* são uma maneira de adicionar os comportamentos do servidor que não são facilmente definidos como operações CRUD nas entidades. Alguns usos para ações incluem:
> 
> - Implementando transações complexas.
> - Manipulando várias entidades de uma vez.
> - Permitir atualizações apenas para determinadas propriedades de uma entidade.
> - Enviando informações para o servidor que não está definido em uma entidade.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Web API 2
> - OData versão 3
> - Entity Framework 6


## <a name="example-rating-a-product"></a>Exemplo: Classificação de um produto

Neste exemplo, queremos que permitem aos usuários classificar os produtos e expor as classificações médias para cada produto. No banco de dados, armazenaremos uma lista de classificações, mapeadas para os produtos.

Aqui está o modelo, pode usar para representar as classificações no Entity Framework:

[!code-csharp[Main](odata-actions/samples/sample1.cs)]

Mas não queremos que os clientes POST um `ProductRating` objeto para uma coleção de "Classificações". Intuitivamente, a classificação está associada com a coleção de produtos, e o cliente só será necessário lançar o valor de classificação.

Portanto, em vez de usar as operações CRUD normais, definimos uma ação que um cliente pode chamar em um produto. Na terminologia do OData, a ação é *associado* para entidades do produto.

>As ações têm efeitos colaterais no servidor. Por esse motivo, eles são chamados usando solicitações HTTP POST. Ações podem ter parâmetros e tipos de retorno, que são descritos nos metadados do serviço. O cliente envia os parâmetros no corpo da solicitação e o servidor envia o valor de retorno no corpo da resposta. Para chamar a ação de "Taxa de produto", o cliente envia uma mensagem para um URI semelhante ao seguinte:

[!code-console[Main](odata-actions/samples/sample2.cmd)]

Os dados na solicitação POST são simplesmente a classificação de produto:

[!code-console[Main](odata-actions/samples/sample3.cmd)]

## <a name="declare-the-action-in-the-entity-data-model"></a>Declarar a ação no modelo de dados de entidade

Em sua configuração de API da Web, adicione a ação para o modelo de dados de entidade (EDM):

[!code-csharp[Main](odata-actions/samples/sample4.cs)]

Esse código define "RateProduct" como uma ação que pode ser executada em entidades de produto. Ele também declara que a ação leva um **int** parâmetro denominado "Classificação" e retorna um **int** valor.

## <a name="add-the-action-to-the-controller"></a>Adicione a ação para o controlador

A ação "RateProduct" está associada a entidades do produto. Para implementar a ação, adicione um método chamado `RateProduct` para o controlador de produtos:

[!code-csharp[Main](odata-actions/samples/sample5.cs)]

Observe que o nome do método corresponde ao nome da ação no EDM. O método tem dois parâmetros:

- *chave*: A chave do produto para a taxa.
- *parâmetros*: um dicionário de valores de parâmetro de ação.

Se você estiver usando as convenções de roteamento padrão, o parâmetro de chave deve ser chamado "chave". Também é importante incluir a **[FromOdataUri]** atributo, conforme mostrado. Esse atributo diz API da Web para usar regras de sintaxe do OData quando ele analisa a chave do URI da solicitação.

Use o *parâmetros* dicionário para obter os parâmetros de ação:

[!code-csharp[Main](odata-actions/samples/sample6.cs)]

Se o cliente envia os parâmetros de ação o correto no formato, o valor de **ModelState** é verdadeiro. Nesse caso, você pode usar o **ODataActionParameters** dicionário para obter os valores de parâmetro. Neste exemplo, o `RateProduct` ação entrará em um único parâmetro denominado "Classificação".

## <a name="action-metadata"></a>Metadados de ação

Para exibir os metadados de serviço, envie uma solicitação GET para /odata/$ metadados. Aqui está a parte dos metadados que declaram o `RateProduct` ação:

[!code-xml[Main](odata-actions/samples/sample7.xml)]

O **FunctionImport** elemento declara a ação. A maioria dos campos são auto-explicativos, mas os dois são vale a pena observar:

- **For IsBindable** significa que a ação pode ser invocada na entidade de destino, pelo menos parte desse tempo.
- **IsAlwaysBindable** significa que a ação sempre pode ser invocada na entidade de destino.

A diferença é que algumas ações estão sempre disponíveis para clientes, mas outras ações podem depender do estado da entidade. Por exemplo, suponha que você definir uma ação de "Comprar". Você só poderá comprar um item que está no estoque. Se o item de estoque, um cliente não é possível invocar essa ação.

Quando você define o EDM a **ação** método cria uma ação sempre associável:

[!code-csharp[Main](odata-actions/samples/sample8.cs?highlight=1)]

Vou falar sobre ações nem sempre-associável (também chamado de *transitório* ações) mais adiante neste tópico.

## <a name="invoking-the-action"></a>Invocação de ação

Agora vamos ver como um cliente deve chamar a ação. Suponha que o cliente deseja fornecer uma classificação de 2 para o produto com ID = 4. Aqui está uma mensagem de solicitação de exemplo, usando o formato JSON para o corpo da solicitação:

[!code-console[Main](odata-actions/samples/sample9.cmd)]

Aqui está a mensagem de resposta:

[!code-console[Main](odata-actions/samples/sample10.cmd)]

## <a name="binding-an-action-to-an-entity-set"></a>Associação de uma ação a um conjunto de entidades

No exemplo anterior, a ação está associada a uma única entidade: O cliente classifica um único produto. Você também pode associar uma ação para uma coleção de entidades. Verifique as seguintes alterações:

No EDM, adicione a ação para a entidade **coleção** propriedade.

[!code-csharp[Main](odata-actions/samples/sample11.cs?highlight=1)]

O método do controlador, omita o *chave* parâmetro.

[!code-csharp[Main](odata-actions/samples/sample12.cs)]

Agora o cliente chama a ação no conjunto de entidades de produtos:

[!code-console[Main](odata-actions/samples/sample13.cmd)]

## <a name="actions-with-collection-parameters"></a>Ações com parâmetros de coleção

Ações podem ter parâmetros que usam uma coleção de valores. No EDM, use **CollectionParameter&lt;T&gt;**  para declarar o parâmetro.

[!code-csharp[Main](odata-actions/samples/sample14.cs)]

Isso declara um parâmetro denominado "Classificações" que usa uma coleção de **int** valores. O método do controlador, você ainda receber o valor do parâmetro de **ODataActionParameters** objeto, mas agora é o valor é um **ICollection&lt;int&gt;**  valor:

[!code-csharp[Main](odata-actions/samples/sample15.cs)]

## <a name="transient-actions"></a>Ações transitórias

No exemplo "RateProduct", os usuários sempre podem classificar um produto para a ação sempre estará disponível. Mas, algumas ações dependem do estado da entidade. Por exemplo, em um serviço de aluguel de vídeo, a ação "Check-out" não está sempre disponível. (Isso depende se uma cópia do que o vídeo está disponível). Esse tipo de ação é chamado um *transitório* ação.

Nos metadados do serviço, tem uma ação transitória **IsAlwaysBindable** igual a false. Que é, na verdade, o valor padrão, portanto os metadados terá esta aparência:

[!code-xml[Main](odata-actions/samples/sample16.xml)]

Aqui por que isso é importante: se uma ação é transitória, o servidor precisa dizer ao cliente quando a ação está disponível. Ele faz isso, incluindo um link para a ação na entidade. Aqui está um exemplo de uma entidade de filme:

[!code-console[Main](odata-actions/samples/sample17.cmd)]

A propriedade "#CheckOut" contém um link para a ação de check-out. Se a ação não estiver disponível, o servidor omite o link.

Para declarar uma ação transitória no EDM, chame o **TransientAction** método:

[!code-csharp[Main](odata-actions/samples/sample18.cs)]

Além disso, você deve fornecer uma função que retorna um link de ação para uma determinada entidade. Defina essa função chamando **HasActionLink**. Você pode escrever a função como uma expressão lambda:

[!code-csharp[Main](odata-actions/samples/sample19.cs)]

Se a ação está disponível, a expressão lambda retorna um link para a ação. O serializador do OData inclui esse link quando ele serializa a entidade. Quando a ação não estiver disponível, a função retornará `null`.

## <a name="additional-resources"></a>Recursos adicionais

[Exemplo de ações de OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v3/ODataActionsSample/)
