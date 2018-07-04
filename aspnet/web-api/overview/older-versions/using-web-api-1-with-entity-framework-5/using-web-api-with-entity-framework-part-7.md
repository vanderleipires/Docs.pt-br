---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
title: 'Parte 7: Criar a principal página | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: eb32a17b-626c-4373-9a7d-3387992f3c04
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
msc.type: authoredcontent
ms.openlocfilehash: b748813046637b9972bc96e22c35d2eeb9b57f9d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378891"
---
<a name="part-7-creating-the-main-page"></a>Parte 7: Criar a principal página
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-the-main-page"></a>Criando a principal página

Nesta seção, você criará a página principal do aplicativo. Essa página será mais complexa do que a página de administração, portanto, vou abordá-lo em várias etapas. Ao longo do caminho, você verá algumas técnicas mais avançadas do Knockout. js. Aqui está o layout básico da página:

![](using-web-api-with-entity-framework-part-7/_static/image1.png)

- "Produtos" contém uma matriz de produtos.
- "Carrinho" contém uma matriz de produtos com as quantidades. Clicar em "Adicionar ao carrinho" atualiza o carrinho.
- "Orders" contém uma matriz de IDs de pedido.
- "Detalhes" contém um detalhe de ordem, que é uma matriz de itens (produtos com quantidades)

Vamos começar definindo algumas layout básico em HTML, sem a associação de dados ou script. Abra o arquivo Views/Home/Index.cshtml e substitua todo o conteúdo pelo seguinte:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample1.html)]

Em seguida, adicione uma seção de Scripts e criar um modelo de exibição vazio:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-7/samples/sample2.cshtml)]

Com base no design elaborado anteriormente, nosso modelo de exibição precisa observáveis para carrinho, produtos, pedidos e detalhes. Adicione as seguintes variáveis para o `AppViewModel` objeto:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample3.js)]

Os usuários podem adicionar itens da lista de produtos no carrinho e remover itens do carrinho de. Para encapsular essas funções, vamos criar outra classe de modelo de exibição que representa um produto. Adicione o seguinte código ao `AppViewModel`:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample4.js?highlight=4)]

O `ProductViewModel` classe contém duas funções que são usadas para mover o produto de e para o carrinho: `addItemToCart` adiciona uma unidade do produto ao carrinho, e `removeAllFromCart` remove todas as quantidades do produto.

Os usuários podem selecionar um pedido existente e obter os detalhes do pedido. Vamos encapsular essa funcionalidade em outro modelo de exibição:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample5.js?highlight=4)]

O `OrderDetailsViewModel` é inicializada com um pedido, e ele busca os detalhes do pedido, enviando uma solicitação AJAX ao servidor.

Além disso, observe o `total` propriedade no `OrderDetailsViewModel`. Esta propriedade é um tipo especial de observável chamado um [computado observável](http://knockoutjs.com/documentation/computedObservables.html). Como o nome implica, um observável computado lhe permite associar dados a um valor computado&#8212;nesse caso, o custo total do pedido.

Em seguida, adicione essas funções para `AppViewModel`:

- `resetCart` Remove todos os itens do carrinho.
- `getDetails` Obtém os detalhes de um pedido (por pusing uma nova `OrderDetailsViewModel` até o `details` lista).
- `createOrder` cria um novo pedido e esvazia o carrinho.


[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample6.js?highlight=4)]

Por fim, inicialize o modelo de exibição, fazendo solicitações AJAX para os produtos e pedidos:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample7.js)]

Okey, que é um monte de código, mas nós o construímos up passo a passo, portanto, espero que o design é claro. Agora podemos adicionar algumas associações do Knockout. js no HTML.

**Produtos**

Aqui estão as vinculações para a lista de produtos:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample8.html)]

Isso itera na matriz de produtos e exibe o nome e o preço. O botão "Adicionar ao Order" só é visível quando o usuário está conectado.

As chamadas do botão "Adicionar a ordem de" `addItemToCart` sobre o `ProductViewModel` instância para o produto. Isso demonstra um recurso interessante do Knockout. js: quando um modelo de exibição contiver outros modelos de exibição, você pode aplicar as associações ao modelo interno. Neste exemplo, as associações de dentro de `foreach` são aplicadas a cada uma da `ProductViewModel` instâncias. Essa abordagem é muito mais fácil do que colocar toda a funcionalidade em um único modelo de exibição.

**Carrinho**

Aqui estão as associações para o carrinho de:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample9.html)]

Isso itera na matriz de carrinho e exibe o nome, preço e quantidade. Observe que o link "Remover" e o botão "Criar Order" são associados a funções de modelo de exibição.

**Pedidos**

Aqui estão as vinculações para a lista de pedidos:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample10.html)]

Isso itera sobre os pedidos e mostra a ID da ordem. O evento de clique no link associado para o `getDetails` função.

**Detalhes do pedido**

Aqui estão as associações para os detalhes do pedido:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample11.html)]

Isso itera sobre os itens na ordem e exibe o produto, preço e Quantity. O div ao redor é visível somente se a matriz de detalhes contém um ou mais itens.

## <a name="conclusion"></a>Conclusão

Neste tutorial, você criou um aplicativo que usa o Entity Framework para se comunicar com o banco de dados e a API Web do ASP.NET para fornecer uma interface para o público sobre a camada de dados. Podemos usar o ASP.NET MVC 4 para renderizar páginas HTML e Knockout. js e jQuery para oferecer interações dinâmicas sem recargas de página.

Recursos adicionais:

- [Mapa de conteúdo de acesso de dados do ASP.NET](https://msdn.microsoft.com/library/6759sth4.aspx)
- [Entity Framework Developer Center](https://msdn.microsoft.com/data/ef)

> [!div class="step-by-step"]
> [Anterior](using-web-api-with-entity-framework-part-6.md)
