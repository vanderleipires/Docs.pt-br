---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
title: Suporte a relações de entidade no OData v3 com a API Web 2 | Microsoft Docs
author: MikeWasson
description: 'A maioria dos conjuntos de dados definem relações entre entidades: os clientes tiverem pedidos; os livros têm autores; os produtos têm fornecedores. Usar o OData, os clientes podem navegar sobre...'
ms.author: riande
ms.date: 02/26/2014
ms.assetid: 1e4c2eb4-b6cf-42ff-8a65-4d71ddca0394
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
msc.type: authoredcontent
ms.openlocfilehash: f78b5cf36789032f90d3d073698f7a439507277f
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206855"
---
<a name="supporting-entity-relations-in-odata-v3-with-web-api-2"></a>Suporte a relações de entidade no OData v3 com a API Web 2
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> A maioria dos conjuntos de dados definem relações entre entidades: os clientes tiverem pedidos; os livros têm autores; os produtos têm fornecedores. Usar o OData, os clientes podem navegar sobre relações de entidade. Um produto, você pode encontrar o fornecedor. Você também pode criar ou remover relações. Por exemplo, você pode definir o fornecedor para um produto.
> 
> Este tutorial mostra como dar suporte a essas operações na API Web ASP.NET. O tutorial se baseia no tutorial [criando um ponto de extremidade OData v3 com a API Web 2](creating-an-odata-endpoint.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - API Web 2
> - OData versão 3
> - Entity Framework 6


## <a name="add-a-supplier-entity"></a>Adicionar uma entidade do fornecedor

Primeiro, precisamos adicionar um novo tipo de entidade para nosso feed OData. Vamos adicionar um `Supplier` classe.

[!code-csharp[Main](working-with-entity-relations/samples/sample1.cs)]

Essa classe usa uma cadeia de caracteres para a chave de entidade. Na prática, que pode ser menos comum do que usando uma chave de inteiro. Mas vale a pena vendo como OData lida com outros tipos de chave além de números inteiros.

Em seguida, vamos criar uma relação com a adição de um `Supplier` propriedade para o `Product` classe:

[!code-csharp[Main](working-with-entity-relations/samples/sample2.cs)]

Adicione um novo **DbSet** para o `ProductServiceContext` classe, para que o Entity Framework incluirá o `Supplier` tabela no banco de dados.

[!code-csharp[Main](working-with-entity-relations/samples/sample3.cs?highlight=9)]

Em WebApiConfig.cs, adicione uma entidade "Fornecedores" para o modelo EDM:

[!code-csharp[Main](working-with-entity-relations/samples/sample4.cs?highlight=4)]

## <a name="navigation-properties"></a>Propriedades de navegação

Para obter o fornecedor para um produto, o cliente envia uma solicitação GET:

[!code-console[Main](working-with-entity-relations/samples/sample5.cmd)]

Aqui o "Fornecedor" é uma propriedade de navegação no `Product` tipo. Nesse caso, `Supplier` refere-se a um único item, mas uma navegação de propriedade também pode retornar uma coleção (relação um-para-muitos ou muitos-para-muitos).

Para dar suporte a essa solicitação, adicione o seguinte método para o `ProductsController` classe:

[!code-csharp[Main](working-with-entity-relations/samples/sample6.cs)]

O *chave* parâmetro é a chave do produto. O método retorna a entidade relacionada&#8212;nesse caso, um `Supplier` instância. O nome do método e o nome de parâmetro são importantes. Em geral, se a propriedade de navegação é chamada "X", você precisará adicionar um método chamado "GetX". O método deve aceitar um parâmetro denominado "*chave*" que corresponde ao tipo de dados da chave do pai.

Também é importante incluir a **[FromOdataUri]** atributo na *chave* parâmetro. Esse atributo instrui a API da Web para usar regras de sintaxe do OData, quando ele analisa a chave do URI da solicitação.

## <a name="creating-and-deleting-links"></a>Criação e exclusão de Links

OData dá suporte à criação ou remover relações entre duas entidades. Na terminologia de OData, a relação é um "link". Cada link tem um URI com o formulário *entity*/$links /*entidade*. Por exemplo, o link do produto para fornecedor esta aparência:

[!code-console[Main](working-with-entity-relations/samples/sample7.cmd)]

Para criar um novo link, o cliente envia uma solicitação POST para o URI de link. O corpo da solicitação é o URI da entidade de destino. Por exemplo, suponha que há um fornecedor com a chave "CTSO". Para criar um link de "Product(1)" para "Supplier('CTSO')", o cliente envia uma solicitação semelhante ao seguinte:

[!code-console[Main](working-with-entity-relations/samples/sample8.cmd)]

Para excluir um link, o cliente envia uma solicitação de exclusão para o URI de link.

**Criação de Links**

Para habilitar um cliente criar links de fornecedor do produto, adicione o seguinte código para o `ProductsController` classe:

[!code-csharp[Main](working-with-entity-relations/samples/sample9.cs)]

Esse método usa três parâmetros:

- *chave*: A chave para a entidade pai (produto)
- *navigationProperty*: O nome da propriedade de navegação. Neste exemplo, a propriedade de navegação só é válida é "Fornecedor".
- *link*: O URI do OData da entidade relacionada. Esse valor é obtido do corpo da solicitação. Por exemplo, o link do URI pode ser "`http://localhost/odata/Suppliers('CTSO')`, que significa que o fornecedor com ID = 'CTSO'.

O método usa o link para pesquisar o fornecedor. Se o fornecedor correspondente for encontrado, o método define o `Product.Supplier` propriedade e salva o resultado para o banco de dados.

A parte mais difícil é analisar o URI de link. Basicamente, você precisa simular o resultado de enviar uma solicitação GET para esse URI. O método auxiliar a seguir mostra como fazer isso. O método invoca o processo de roteamento de API da Web e recebe de volta uma **ODataPath** instância que representa o caminho de OData analisado. Para um URI de link, um dos segmentos deve ser a chave de entidade. (Caso contrário, o cliente enviou um URI inválido.)

[!code-csharp[Main](working-with-entity-relations/samples/sample10.cs)]

**Exclusão de Links**

Para excluir um link, adicione o seguinte código para o `ProductsController` classe:

[!code-csharp[Main](working-with-entity-relations/samples/sample11.cs)]

Neste exemplo, a propriedade de navegação é um único `Supplier` entidade. Se a propriedade de navegação é uma coleção, o URI para excluir um link deve incluir uma chave para a entidade relacionada. Por exemplo:

[!code-console[Main](working-with-entity-relations/samples/sample12.cmd)]

Essa solicitação remove ordem 1 do cliente 1. Nesse caso, o método DeleteLink terá a seguinte assinatura:

[!code-csharp[Main](working-with-entity-relations/samples/sample13.cs)]

O *relatedKey* parâmetro fornece a chave para a entidade relacionada. Portanto, no seu `DeleteLink` método, pesquisar a entidade principal pelo *chave* parâmetro, encontrar a entidade relacionada pelo *relatedKey* parâmetro e, em seguida, remova a associação. Dependendo de seu modelo de dados, talvez você precise implementar ambas as versões do `DeleteLink`. API da Web chamará a versão correta com base no URI da solicitação.
