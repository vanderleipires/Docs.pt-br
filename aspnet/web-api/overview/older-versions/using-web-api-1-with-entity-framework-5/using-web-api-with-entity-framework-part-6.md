---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: 'Parte 6: Criar controladores de pedido e produto | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 07/04/2012
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: 6ba7c20d9f529ccee83ce4fd85a1294047643f85
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841073"
---
<a name="part-6-creating-product-and-order-controllers"></a>Parte 6: Criar o produto e controladores de ordem
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a>Adicionar um controlador de produtos

O controlador de administrador é para usuários que têm privilégios de administrador. Os clientes, por outro lado, podem visualizar os produtos, mas não é possível criar, atualizar ou excluí-los.

Podemos facilmente pode restringir o acesso aos métodos Post, Put e Delete, deixando os métodos Get aberto. Mas analisar os dados que são retornados para um produto:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

O `ActualCost` propriedade não deve ser visível aos clientes! A solução é definir um *objeto de transferência de dados* (DTO) que inclui um subconjunto de propriedades que devem ser visíveis aos clientes. Usamos LINQ ao projeto `Product` para instâncias `ProductDTO` instâncias.

Adicione uma classe chamada `ProductDTO` na pasta modelos.

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

Agora, adicione o controlador. No Gerenciador de soluções, clique com botão direito na pasta controladores. Selecione **Add**, em seguida, selecione **controlador**. Na caixa de diálogo **Adicionar controlador**, nomeie o controlador como &quot;ProductsController&quot;. Sob **modelo**, selecione **controlador da API vazio**.

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

Substitua tudo no arquivo de origem com o código a seguir:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

O controlador ainda usa o `OrdersContext` para consultar o banco de dados. Mas, em vez de retornar `Product` instâncias diretamente, chamamos `MapProducts` para o projeto-las para `ProductDTO` instâncias:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

O `MapProducts` método retorna um **IQueryable**, portanto, é possível compor o resultado com outros parâmetros de consulta. Você pode ver isso na `GetProduct` método, que adiciona um **onde** cláusula para a consulta:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a>Adicionar um controlador de pedidos

Em seguida, adicione um controlador que permite aos usuários criar e exibir ordens.

Vamos começar com outro DTO. No Gerenciador de soluções, clique com botão direito na pasta modelos e adicione uma classe chamada `OrderDTO` usar a implementação a seguir:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

Agora, adicione o controlador. No Gerenciador de soluções, clique com botão direito na pasta controladores. Selecione **Add**, em seguida, selecione **controlador**. No **Adicionar controlador** caixa de diálogo, defina as seguintes opções:

- Sob **nome do controlador**, insira "OrdersController".
- Sob **modelo**, selecione "Controlador de API com ações de leitura/gravação, usando o Entity Framework".
- Sob **classe de modelo**, selecione &quot;ordem (ProductStore.Models)&quot;.
- Sob **classe de contexto de dados**, selecione &quot;OrdersContext (ProductStore.Models)&quot;.

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

Clique em **Adicionar**. Isso adiciona um arquivo chamado OrdersController.cs. Em seguida, precisamos modificar a implementação padrão do controlador.

Primeiro, exclua o `PutOrder` e `DeleteOrder` métodos. Para este exemplo, os clientes não podem modificar ou excluir pedidos existentes. Em um aplicativo real, você precisaria muita lógica de back-end para lidar com esses casos. (Por exemplo, a ordem já vinha?)

Alterar o `GetOrders` método para retornar apenas os pedidos que pertencem ao usuário:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

Alterar o `GetOrder` método da seguinte maneira:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

Aqui estão as alterações que fizemos para o método:

- O valor retornado é um `OrderDTO` instância, em vez de um `Order`.
- Ao consultar o banco de dados para o pedido, podemos usar o [DbQuery.Include](https://msdn.microsoft.com/library/gg696395) método para buscar relacionado `OrderDetail` e `Product` entidades.
- Podemos nivelar o resultado usando uma projeção.

A resposta HTTP conterá uma matriz de produtos com quantidades:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

Esse formato é mais fácil para os clientes consumam que o grafo de objeto original, que contém entidades aninhadas (ordem, os detalhes e produtos).

O último método considerá-la `PostOrder`. No momento, esse método usa um `Order` instância. Mas considere o que acontece se um cliente envia um corpo de solicitação como esta:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

Esta é uma ordem bem estruturada e Entity Framework Felizmente irá inseri-lo no banco de dados. Mas ele contém uma entidade de produto que não existia anteriormente. O cliente acabou de criar um novo produto em nosso banco de dados! Isso será uma surpresa para o departamento de efetivação de pedidos, quando eles veem um pedido para ursos koala. A moral é, tenha muito cuidado sobre os dados a que aceitar uma solicitação POST ou PUT.

Para evitar esse problema, altere a `PostOrder` método para levar um `OrderDTO` instância. Use o `OrderDTO` para criar o `Order`.

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

Observe que podemos usar o `ProductID` e `Quantity` propriedades e podemos ignorar todos os valores que o cliente enviou para o nome do produto ou no preço. Se a ID do produto não for válida, ele irá violar a restrição de chave estrangeira no banco de dados e a inserção falhará, como deveria.

Aqui está o completo `PostOrder` método:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

Por fim, adicione a **autorizar** de atributo para o controlador:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

Agora, somente usuários registrados podem criar ou exibir ordens.

> [!div class="step-by-step"]
> [Anterior](using-web-api-with-entity-framework-part-5.md)
> [Próximo](using-web-api-with-entity-framework-part-7.md)
