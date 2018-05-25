---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
title: Suporte a relações de entidade no OData v3 com Web API 2 | Microsoft Docs
author: MikeWasson
description: 'A maioria dos conjuntos de dados definem relações entre entidades: os clientes tiverem pedidos; os livros têm autores; os produtos têm fornecedores. Usando o OData, os clientes podem navegar por...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2014
ms.topic: article
ms.assetid: 1e4c2eb4-b6cf-42ff-8a65-4d71ddca0394
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
msc.type: authoredcontent
ms.openlocfilehash: dec7e10e59cc2441c967afe062df227b105106a1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="supporting-entity-relations-in-odata-v3-with-web-api-2"></a>Suporte a relações de entidade no OData v3 com Web API 2
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> A maioria dos conjuntos de dados definem relações entre entidades: os clientes tiverem pedidos; os livros têm autores; os produtos têm fornecedores. Usando o OData, os clientes podem navegar em relações de entidade. Um produto, você pode encontrar o fornecedor. Você também pode criar ou remover relações. Por exemplo, você pode definir o fornecedor para um produto.
> 
> Este tutorial mostra como dar suporte a essas operações na API da Web do ASP.NET. O tutorial se baseia o tutorial [criando um ponto de extremidade OData v3 com a API Web 2](creating-an-odata-endpoint.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Web API 2
> - OData versão 3
> - Entity Framework 6


## <a name="add-a-supplier-entity"></a>Adicionar uma entidade do fornecedor

Primeiro, precisamos adicionar um novo tipo de entidade para nosso feed OData. Vamos adicionar um `Supplier` classe.

[!code-csharp[Main](working-with-entity-relations/samples/sample1.cs)]

Essa classe usa uma cadeia de caracteres para a chave de entidade. Na prática, o que pode ser menos comuns que usando uma chave de inteiro. Mas vale a pena vendo como OData lida com outros tipos de chave além de inteiros.

Em seguida, vamos criar uma relação com a adição de um `Supplier` propriedade para o `Product` classe:

[!code-csharp[Main](working-with-entity-relations/samples/sample2.cs)]

Adicionar um novo **DbSet** para o `ProductServiceContext` de classe, para que o Entity Framework incluirá o `Supplier` tabela no banco de dados.

[!code-csharp[Main](working-with-entity-relations/samples/sample3.cs?highlight=9)]

Em WebApiConfig.cs, adicione uma entidade de "Fornecedores" para o modelo EDM:

[!code-csharp[Main](working-with-entity-relations/samples/sample4.cs?highlight=4)]

## <a name="navigation-properties"></a>Propriedades de navegação

Para obter o fornecedor para um produto, o cliente envia uma solicitação GET:

[!code-console[Main](working-with-entity-relations/samples/sample5.cmd)]

Aqui "Fornecedor" é uma propriedade de navegação no `Product` tipo. Nesse caso, `Supplier` se refere a um único item, mas uma navegação propriedade também pode retornar uma coleção (relação um-para-muitos ou muitos-para-muitos).

Para dar suporte a essa solicitação, adicione o seguinte método para o `ProductsController` classe:

[!code-csharp[Main](working-with-entity-relations/samples/sample6.cs)]

O *chave* parâmetro é a chave do produto. O método retorna a entidade relacionada & #8212 nesse caso, um `Supplier` instância. O nome do método e o nome de parâmetro são importantes. Em geral, se a propriedade de navegação é denominada "X", você precisa adicionar um método chamado "GetX". O método deve ter um parâmetro denominado "*chave*" que corresponde ao tipo de dados da chave do pai.

Também é importante incluir a **[FromOdataUri]** atributo o *chave* parâmetro. Esse atributo diz API da Web para usar regras de sintaxe do OData quando ele analisa a chave do URI da solicitação.

## <a name="creating-and-deleting-links"></a>Criação e exclusão de Links

OData oferece suporte ao criar ou remover relações entre duas entidades. Na terminologia de OData, a relação é um "link". Cada link tem um URI com o formulário *entidade*/$links /*entidade*. Por exemplo, o link do produto para fornecedor tem esta aparência:

[!code-console[Main](working-with-entity-relations/samples/sample7.cmd)]

Para criar um novo link, o cliente envia uma solicitação POST para o URI do link. O corpo da solicitação é o URI da entidade de destino. Por exemplo, suponha que há um fornecedor com a chave "CTSO". Para criar um link de "Product(1)" para "Supplier('CTSO')", o cliente envia uma solicitação semelhante ao seguinte:

[!code-console[Main](working-with-entity-relations/samples/sample8.cmd)]

Para excluir um link, o cliente envia uma solicitação de exclusão para o URI do link.

**Criação de Links**

Para habilitar um cliente criar links do fornecedor do produto, adicione o seguinte código para o `ProductsController` classe:

[!code-csharp[Main](working-with-entity-relations/samples/sample9.cs)]

Este método usa três parâmetros:

- *chave*: A chave para a entidade pai (produto)
- *navigationProperty*: O nome da propriedade de navegação. Neste exemplo, a propriedade de navegação só é válida é "Fornecedor".
- *link*: O URI do OData da entidade relacionada. Esse valor é obtido do corpo da solicitação. Por exemplo, o link URI pode ser "`http://localhost/odata/Suppliers('CTSO')`, que significa que o fornecedor com ID = 'CTSO'.

O método usa o link para consultar o fornecedor. Se o fornecedor de correspondência for encontrado, o método define o `Product.Supplier` propriedade e salva o resultado para o banco de dados.

A parte mais difícil é analisar o URI do link. Basicamente, você precisa simular o resultado de enviar uma solicitação GET para esse URI. O método auxiliar a seguir mostra como fazer isso. O método invoca o processo de roteamento de API da Web e obtém de volta um **ODataPath** instância que representa o caminho de OData analisado. Para um link de URI, um dos segmentos deve ser a chave de entidade. (Caso contrário, o cliente enviou um URI inválido.)

[!code-csharp[Main](working-with-entity-relations/samples/sample10.cs)]

**Excluir Links**

Para excluir um link, adicione o seguinte código para o `ProductsController` classe:

[!code-csharp[Main](working-with-entity-relations/samples/sample11.cs)]

Neste exemplo, a propriedade de navegação é um único `Supplier` entidade. Se a propriedade de navegação é uma coleção, o URI para excluir um link deve incluir uma chave para a entidade relacionada. Por exemplo:

[!code-console[Main](working-with-entity-relations/samples/sample12.cmd)]

Essa solicitação remove a ordem 1 cliente 1. Nesse caso, o método DeleteLink tem a seguinte assinatura:

[!code-csharp[Main](working-with-entity-relations/samples/sample13.cs)]

O *relatedKey* parâmetro fornece a chave para a entidade relacionada. Assim, na sua `DeleteLink` método, procure a entidade principal pelo *chave* parâmetro, localizar a entidade relacionada, a *relatedKey* parâmetro e, em seguida, remova a associação. Dependendo de seu modelo de dados, você talvez precise implementar ambas as versões do `DeleteLink`. API da Web se chamará a versão correta com base no URI da solicitação.
