---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: 'Parte 6: Criando o produto e controladores de ordem | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: 6bd485d29821af12b9ebe31b2d04a2d9ab826731
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869381"
---
<a name="part-6-creating-product-and-order-controllers"></a>Parte 6: Criação de produto e os controladores de ordem
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a>Adicionar um controlador de produtos

O controlador de administrador é para usuários que têm privilégios de administrador. Os clientes, por outro lado, podem exibir produtos, mas não é possível criar, atualizar ou excluí-los.

Podemos facilmente pode restringir o acesso aos métodos Post, Put e Delete, deixando os métodos Get aberto. Mas, veja os dados que são retornados de um produto:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

O `ActualCost` propriedade não deve estar visível para os clientes! A solução é definir um *o objeto de transferência de dados* (DTO) que inclui um subconjunto das propriedades que devem ser visíveis aos clientes. Usaremos o LINQ para projeto `Product` instâncias `ProductDTO` instâncias.

Adicione uma classe denominada `ProductDTO` para a pasta de modelos.

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

Agora adicione o controlador. No Gerenciador de soluções, clique na pasta de controladores. Selecione **adicionar**, em seguida, selecione **controlador**. Na caixa de diálogo **Adicionar controlador**, nomeie o controlador como &quot;ProductsController&quot;. Em **modelo**, selecione **controlador API vazio**.

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

Substitua tudo no arquivo de origem com o código a seguir:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

O controlador ainda usa o `OrdersContext` para consultar o banco de dados. Mas em vez de retornar `Product` instâncias diretamente, podemos chamar `MapProducts` ao projeto-os no `ProductDTO` instâncias:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

O `MapProducts` método retorna um **IQueryable**, portanto, é possível compor o resultado com outros parâmetros de consulta. Você pode ver isso no `GetProduct` método, que adiciona um **onde** cláusula para a consulta:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a>Adicionar um controlador de pedidos

Em seguida, adicione um controlador que permite aos usuários criar e exibir ordens.

Vamos começar com outro DTO. No Gerenciador de soluções, clique com botão direito na pasta modelos e adicione uma classe denominada `OrderDTO` usar a implementação a seguir:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

Agora adicione o controlador. No Gerenciador de soluções, clique na pasta de controladores. Selecione **adicionar**, em seguida, selecione **controlador**. No **Adicionar controlador** caixa de diálogo, defina as seguintes opções:

- Em **nome do controlador**, digite "OrdersController".
- Em **modelo**, selecione "Controlador API com ações de leitura/gravação, usando o Entity Framework".
- Em **classe modelo**, selecione &quot;ordem (ProductStore.Models)&quot;.
- Em **classe de contexto de dados**, selecione &quot;OrdersContext (ProductStore.Models)&quot;.

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

Clique em **Adicionar**. Isso adiciona um arquivo chamado OrdersController.cs. Em seguida, é preciso modificar a implementação padrão do controlador.

Primeiro, exclua o `PutOrder` e `DeleteOrder` métodos. Para este exemplo, os clientes não podem modificar ou excluir ordens existentes. Em um aplicativo real, você precisaria muita lógica de back-end para lidar com esses casos. (Por exemplo, a ordem já vinha?)

Alterar o `GetOrders` método para retornar apenas os pedidos que pertencem ao usuário:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

Alterar o `GetOrder` método da seguinte maneira:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

Aqui estão as alterações que fizemos para o método:

- O valor de retorno é um `OrderDTO` instância, em vez de um `Order`.
- Quando é consultar o banco de dados para a ordem, usamos o [DbQuery.Include](https://msdn.microsoft.com/library/gg696395) método para buscar relacionado `OrderDetail` e `Product` entidades.
- Podemos mesclar o resultados usando uma projeção.

A resposta HTTP conterá uma matriz de produtos com quantidades:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

Esse formato é mais fácil para os clientes usarem que o gráfico de objeto original, que contém entidades aninhadas (ordem, os detalhes e produtos).

O último método considerá-la `PostOrder`. No momento, esse método usa um `Order` instância. Porém, considere o que acontece se um cliente envia um corpo de solicitação como este:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

Esta é uma ordem bem estruturada e Felizmente irá inserir do Entity Framework para o banco de dados. Mas ele contém uma entidade de produto que não existia anteriormente. O cliente que acabou de criar um novo produto no nosso banco de dados! Isso será uma surpresa para o departamento de preenchimento do pedido, quando eles veem uma ordem para koala ursos. É a moral, tenha muito cuidado sobre os dados que você aceitar uma solicitação POST ou PUT.

Para evitar esse problema, altere o `PostOrder` método tenham uma `OrderDTO` instância. Use o `OrderDTO` para criar o `Order`.

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

Observe que usamos o `ProductID` e `Quantity` propriedades e vamos ignorar quaisquer valores que o cliente enviou para o nome do produto ou no preço. Se a ID do produto não é válida, ele violarão a restrição de chave estrangeira no banco de dados e a inserção falhará, como deveria.

Aqui está o completo `PostOrder` método:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

Finalmente, adicione o **autorizar** de atributo para o controlador:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

Agora, somente usuários registrados podem criar ou exibir ordens.

> [!div class="step-by-step"]
> [Anterior](using-web-api-with-entity-framework-part-5.md)
> [Próximo](using-web-api-with-entity-framework-part-7.md)
