---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
title: Relações de entidade no OData v4 usando a API Web ASP.NET 2.2 | Microsoft Docs
author: MikeWasson
description: 'A maioria dos conjuntos de dados definem relações entre entidades: os clientes tiverem pedidos; os livros têm autores; os produtos têm fornecedores. Usar o OData, os clientes podem navegar sobre...'
ms.author: aspnetcontent
ms.date: 06/26/2014
ms.assetid: 72657550-ec09-4779-9bfc-2fb15ecd51c7
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 98f65b068d8f22e3eeef48ca7fa441434939db8b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827940"
---
<a name="entity-relations-in-odata-v4-using-aspnet-web-api-22"></a>Relações de entidade no OData v4 usando a API Web ASP.NET 2.2
====================
por [Mike Wasson](https://github.com/MikeWasson)

> A maioria dos conjuntos de dados definem relações entre entidades: os clientes tiverem pedidos; os livros têm autores; os produtos têm fornecedores. Usar o OData, os clientes podem navegar sobre relações de entidade. Um produto, você pode encontrar o fornecedor. Você também pode criar ou remover relações. Por exemplo, você pode definir o fornecedor para um produto.
> 
> Este tutorial mostra como dar suporte a essas operações no OData v4 usando a API Web do ASP.NET. O tutorial se baseia no tutorial [criar um OData v4 ponto de extremidade usando API Web ASP.NET 2](create-an-odata-v4-endpoint.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - API Web 2.1
> - OData v4
> - [Visual Studio 2013 Atualização 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - Entity Framework 6
> - .NET 4.5
> 
> 
> ## <a name="tutorial-versions"></a>Versões de tutoriais
> 
> Para o OData versão 3, consulte [que dão suporte a relações de entidade no OData v3](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).


## <a name="add-a-supplier-entity"></a>Adicionar uma entidade do fornecedor

> [!NOTE]
> O tutorial se baseia no tutorial [criar um OData v4 ponto de extremidade usando API Web ASP.NET 2](create-an-odata-v4-endpoint.md).


Primeiro, precisamos de uma entidade relacionada. Adicione uma classe chamada `Supplier` na pasta de modelos.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample1.cs)]

Adicionar uma propriedade de navegação para o `Product` classe:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample2.cs?highlight=13-15)]

Adicione um novo **DbSet** para o `ProductsContext` de classe, para que o Entity Framework incluirá a tabela de fornecedor no banco de dados.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample3.cs?highlight=10)]

Em WebApiConfig.cs, adicione uma &quot;fornecedores&quot; ao modelo de dados de entidade do conjunto de entidades:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample4.cs?highlight=6)]

## <a name="add-a-suppliers-controller"></a>Adicionar um controlador de fornecedores

Adicionar um `SuppliersController` classe para a pasta controladores.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample5.cs)]

Não o mostrarei como adicionar operações CRUD para esse controlador. As etapas são as mesmas para o controlador de produtos (consulte [criar um ponto de extremidade OData v4](create-an-odata-v4-endpoint.md)).

## <a name="getting-related-entities"></a>Obtenção de entidades relacionadas

Para obter o fornecedor para um produto, o cliente envia uma solicitação GET:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample6.cmd)]

Para dar suporte a essa solicitação, adicione o seguinte método para o `ProductsController` classe:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample7.cs)]

Esse método usa uma convenção de nomenclatura padrão

- Nome do método: GetX, onde X é a propriedade de navegação.
- Nome do parâmetro: *chave*

Se você seguir essa convenção de nomenclatura, API Web mapeia automaticamente a solicitação HTTP para o método do controlador.

Solicitação HTTP de exemplo:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample8.cmd)]

Resposta HTTP de exemplo:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample9.cmd)]

### <a name="getting-a-related-collection"></a>Obtendo uma coleção relacionada

No exemplo anterior, um produto tem um fornecedor. Uma propriedade de navegação também pode retornar uma coleção. O código a seguir obtém os produtos para um fornecedor:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample10.cs)]

Nesse caso, o método retorna um **IQueryable** em vez de uma **SingleResult&lt;T&gt;**

Solicitação HTTP de exemplo:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample11.cmd)]

Resposta HTTP de exemplo:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample12.cmd)]

## <a name="creating-a-relationship-between-entities"></a>Criando um relacionamento entre entidades

OData dá suporte à criação ou remoção de relações entre duas entidades existentes. Na terminologia do OData v4, a relação é uma &quot;referência&quot;. (No OData v3, a relação era chamada de um *link*. As diferenças de protocolo não importam para este tutorial.)

Uma referência tem seu próprio URI, com o formulário `/Entity/NavigationProperty/$ref`. Por exemplo, aqui está o URI para endereçar a referência entre um produto e seus fornecedores:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample13.cmd)]

Para adicionar uma relação, o cliente envia uma solicitação POST ou PUT para esse endereço.

- Se a propriedade de navegação é uma única entidade, como `Product.Supplier`.
- Lançar se a propriedade de navegação é uma coleção, como `Supplier.Products`.

O corpo da solicitação contém o URI da entidade na relação. Aqui está um exemplo de solicitação:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample14.cmd)]

Neste exemplo, o cliente envia uma solicitação PUT para `/Products(6)/Supplier/$ref`, que é o URI de $ref para o `Supplier` do produto com ID = 6. Se a solicitação for bem-sucedida, o servidor envia uma resposta de 204 (sem conteúdo):

[!code-console[Main](entity-relations-in-odata-v4/samples/sample15.cmd)]

Aqui está o método do controlador para adicionar uma relação com um `Product`:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample16.cs)]

O *navigationProperty* parâmetro especifica quais relação definida. (Se houver mais de uma propriedade de navegação na entidade, você pode adicionar mais `case` instruções.)

O *link* parâmetro contém o URI do fornecedor. API da Web automaticamente analisa o corpo da solicitação para obter o valor para esse parâmetro.

Para consultar o fornecedor, é necessário o ID (ou chave), que é parte do *link* parâmetro. Para fazer isso, use o método auxiliar a seguir:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample17.cs)]

Basicamente, esse método usa a biblioteca OData para dividir o caminho URI em segmentos, localize o segmento que contém a chave e converter a chave no tipo correto.

## <a name="deleting-a-relationship-between-entities"></a>Excluir uma relação entre entidades

Para excluir uma relação, o cliente envia uma solicitação HTTP DELETE para o URI $ref:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample18.cmd)]

Aqui está o método do controlador para excluir a relação entre um produto e um fornecedor:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample19.cs)]

Nesse caso, `Product.Supplier` é o &quot;1&quot; final de uma relação de 1-para-muitos, portanto, você pode remover a relação apenas definindo `Product.Supplier` para `null`.

No &quot;muitos&quot; final de uma relação, o cliente deve especificar qual relacionados a entidade a ser removido. Para fazer isso, o cliente envia o URI da entidade relacionada na cadeia de caracteres de consulta da solicitação. Por exemplo, para remover "Produto 1" de "fornecedor 1":

[!code-console[Main](entity-relations-in-odata-v4/samples/sample20.cmd?highlight=1)]

Para suportar isso na API Web, precisamos incluir um parâmetro extra no `DeleteRef` método. Aqui está o método do controlador para remover um produto a `Supplier.Products` relação.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample21.cs)]

O *chave* parâmetro é a chave para o fornecedor e o *relatedKey* parâmetro é a chave do produto remover do `Products` relação. Observe que o API Web automaticamente obtém a chave da cadeia de consulta.
