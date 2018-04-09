---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
title: 'Parte 7: Criar principal página | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: eb32a17b-626c-4373-9a7d-3387992f3c04
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
msc.type: authoredcontent
ms.openlocfilehash: 2c378e68a1e6600daf655c19afbfe355e89496d4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="part-7-creating-the-main-page"></a>Parte 7: Criar principal página
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-the-main-page"></a>Criando principal página

Nesta seção, você criará a página principal do aplicativo. Esta página será mais complexa do que a página de administração, portanto será a abordamos em várias etapas. Ao longo do processo, você verá algumas técnicas mais avançadas do Knockout. js. Aqui está o layout básico da página:

![](using-web-api-with-entity-framework-part-7/_static/image1.png)

- "Produtos" contém uma matriz de produtos.
- "Carrinho" contém uma matriz de produtos com quantidades. Clicando em "Adicionar ao carrinho" atualiza o carrinho.
- "Orders" contém uma matriz de IDs do pedido.
- "Detalhes" contém um detalhe de ordem, que é uma matriz de itens (produtos com quantidades)

Vamos começar definindo algumas layout básico em HTML, sem associação de dados ou script. Abra o arquivo Views/Home/Index.cshtml e substitua todo o conteúdo com o seguinte:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample1.html)]

Em seguida, adicionar uma seção de Scripts e criar um modelo vazio de exibição:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-7/samples/sample2.cshtml)]

Com base no design elaborado anteriormente, nosso modelo de exibição precisa observáveis carrinho, produtos, pedidos e detalhes. Adicione as seguintes variáveis para o `AppViewModel` objeto:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample3.js)]

Os usuários podem adicionar itens de lista de produtos no carrinho de e remover itens do carrinho. Para encapsular essas funções, vamos criar outra classe de modelo de exibição que representa um produto. Adicione o seguinte código ao `AppViewModel`:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample4.js?highlight=4)]

O `ProductViewModel` classe contém duas funções que são usadas para mover o produto para e do carrinho: `addItemToCart` adiciona uma unidade do produto ao carrinho, e `removeAllFromCart` remove todas as quantidades de produto.

Os usuários podem selecionar um pedido existente e obter os detalhes do pedido. Podemos será encapsular essa funcionalidade em outro modelo de exibição:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample5.js?highlight=4)]

O `OrderDetailsViewModel` é inicializada com uma ordem, e ele busca dos detalhes do pedido, enviando uma solicitação AJAX para o servidor.

Além disso, observe o `total` propriedade o `OrderDetailsViewModel`. Esta propriedade é um tipo especial de observável chamado um [computada observável](http://knockoutjs.com/documentation/computedObservables.html). Como o nome implica, um computada observável permite associação de dados para um valor computado&#8212;nesse caso, o total de custo da ordem.

Em seguida, adicionar essas funções para `AppViewModel`:

- `resetCart` Remove todos os itens do carrinho.
- `getDetails` Obtém os detalhes de um pedido (por pusing um novo `OrderDetailsViewModel` até o `details` lista).
- `createOrder` cria um novo pedido e esvazia o carrinho.


[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample6.js?highlight=4)]

Finalmente, inicialize o modelo de exibição, fazendo solicitações do AJAX para os produtos e pedidos:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample7.js)]

Okey, que é muito código, mas podemos compilá-lo passo a passo, esperamos o design é criptografado. Agora podemos adicionar algumas associações Knockout. js para HTML.

**Produtos**

Aqui estão as ligações para a lista de produtos:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample8.html)]

Isso é iterado sobre a matriz de produtos e exibe o nome e o preço. O botão "Adicionar a ordem" só é visível quando o usuário está conectado.

As chamadas do botão "Adicionar a ordem" `addItemToCart` no `ProductViewModel` instância para o produto. Isso demonstra um recurso interessante de Knockout. js: quando um modelo de exibição contiver outros modelos de exibição, você pode aplicar as ligações para o modelo interno. Neste exemplo, as associações de dentro do `foreach` são aplicadas a cada uma da `ProductViewModel` instâncias. Essa abordagem é muito mais fácil do que colocar todos os recursos em um único modelo de exibição.

**Cart**

Aqui estão as associações de carrinho de:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample9.html)]

Isso é iterado sobre a matriz de carrinho e exibe o nome, preço e quantidade. Observe que o link "Remover" e o botão "Criar pedido" estão associados a funções de modelo de exibição.

**Pedidos**

Aqui estão as ligações para obter a lista de pedidos:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample10.html)]

Isso é iterado sobre os pedidos e mostra a ID da ordem. O evento de clique no link está vinculado para o `getDetails` função.

**Detalhes do pedido**

Aqui estão as associações com os detalhes do pedido:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample11.html)]

Isso é iterado sobre os itens na ordem e exibe o produto, preço e quantidade. Div ao redor é visível somente se a matriz de detalhes contém um ou mais itens.

## <a name="conclusion"></a>Conclusão

Neste tutorial, você criou um aplicativo que usa o Entity Framework para se comunicar com o banco de dados e a API Web do ASP.NET para fornecer uma interface de público sobre a camada de dados. Podemos usar o ASP.NET MVC 4 para renderizar páginas HTML e Knockout. js e jQuery para oferecer interações dinâmicas sem recargas de página.

Recursos adicionais:

- [Mapa de conteúdo de acesso de dados do ASP.NET](https://msdn.microsoft.com/library/6759sth4.aspx)
- [Entity Framework Developer Center](https://msdn.microsoft.com/data/ef)

> [!div class="step-by-step"]
> [Anterior](using-web-api-with-entity-framework-part-6.md)
