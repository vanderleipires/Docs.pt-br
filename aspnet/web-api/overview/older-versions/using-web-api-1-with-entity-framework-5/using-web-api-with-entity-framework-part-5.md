---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
title: 'Parte 5: Criando uma interface do usuário dinâmica com Knockout. js | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 9d9cb3b0-f4a7-434e-a508-9fc0ad0eb813
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
msc.type: authoredcontent
ms.openlocfilehash: 1f645b853fba999ade11c420eceba1b310f80ca3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368422"
---
<a name="part-5-creating-a-dynamic-ui-with-knockoutjs"></a>Parte 5: Criando uma interface do usuário dinâmica com Knockout. js
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-a-dynamic-ui-with-knockoutjs"></a>Criando uma interface do usuário dinâmica com Knockout. js

Nesta seção, vamos usar Knockout. js para adicionar funcionalidade para a exibição de administração.

[Knockout. js](http://knockoutjs.com/) é uma biblioteca de Javascript que torna mais fácil associar controles HTML aos dados. Knockout. js usa o padrão Model-View-ViewModel (MVVM).

- O *modelo* é a representação do lado do servidor dos dados no domínio de negócios (no nosso caso, produtos e pedidos).
- O *exibição* é a camada de apresentação (HTML).
- O *modelo de exibição* é um objeto Javascript que contém os dados de modelo. O modelo de exibição é uma abstração de código da interface do usuário. Ele não tem conhecimento da representação HTML. Em vez disso, ele representa recursos abstratos da exibição, como "uma lista de itens".

O modo de exibição associado a dados para o modelo de exibição. Atualizações para o modelo de exibição são refletidas automaticamente no modo de exibição. O modelo de exibição também obtém os eventos de exibição, como cliques de botão e executa operações no modelo, como a criação de um pedido.

![](using-web-api-with-entity-framework-part-5/_static/image1.png)

Primeiro, vamos definir o modelo de exibição. Depois disso, podemos associará a marcação HTML para o modelo de exibição.

Adicione a seguinte seção de Razor para Admin.cshtml:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample1.cshtml)]

Você pode adicionar esta seção em qualquer lugar no arquivo. Quando o modo de exibição é processado, a seção aparece na parte inferior da página HTML, botão direito do mouse antes do fechamento &lt;/body&gt; marca.

Todos os scripts para esta página irão dentro da marca de script indicada pelo comentário:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample2.html)]

Primeiro, defina uma classe de modelo de exibição:

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample3.js)]

**ko.observableArray** é um tipo especial de objeto no Knockout, chamado de um *observável*. Dos [documentação do Knockout. js](http://knockoutjs.com/documentation/observables.html): um observável é um "objeto JavaScript que pode notificar assinantes sobre as alterações." Quando altera o conteúdo de um observável, o modo de exibição é atualizado automaticamente para corresponder.

Para popular o `products` de matriz, faça uma solicitação AJAX a API da web. Lembre-se de que armazenamos o URI de base para a API no recipiente de modo de exibição (consulte [parte 4](using-web-api-with-entity-framework-part-4.md) do tutorial).

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample4.js?highlight=5)]

Em seguida, adicione funções para o modelo de exibição para criar, atualizar e excluir produtos. Essas funções enviar chamadas AJAX para a API da web e usam os resultados para atualizar o modelo de exibição.

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample5.js?highlight=7)]

Agora a parte mais importante: quando DOM é fulled chamada carregada, o **ko.applyBindings** de função e passar uma nova instância do `ProductsViewModel`:

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample6.js)]

O **ko.applyBindings** método ativa o Knockout e conecta o modelo de exibição para o modo de exibição.

Agora que temos um modelo de exibição, podemos criar as associações. No Knockout. js, faça isso adicionando `data-bind` atributos para elementos HTML. Por exemplo, para associar uma lista de HTML para uma matriz, use o `foreach` associação:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample7.html?highlight=1)]

O `foreach` associação itera na matriz e cria filho elementos para cada objeto na matriz. Associações em elementos filho podem se referir às propriedades nos objetos de matriz.

Adicione as seguintes associações à lista de "produtos de atualização":

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample8.html)]

O `<li>` elemento ocorre dentro do escopo do **foreach** associação. Que significa que será o Knockout renderizar o elemento de uma vez para cada produto no `products` matriz. Todas as associações dentro a `<li>` elemento se referir a essa instância do produto. Por exemplo, `$data.Name` refere-se ao `Name` propriedade no produto.

Para definir os valores das entradas de texto, use o `value` associação. Os botões são associados a funções na exibição do modelo, usando o `click` associação. A instância de produto é passada como um parâmetro para cada função. Para obter mais informações, o [documentação do Knockout. js](http://knockoutjs.com/documentation/observables.html) tem boa descrições das várias associações.

Em seguida, adicione uma associação para o **enviar** eventos no formulário Adicionar produto:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample9.html)]

Essa associação chama o `create` função no modelo de exibição para criar um novo produto.

Aqui está o código completo para o modo de exibição do administrador:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample10.cshtml)]

Executar o aplicativo, faça logon com a conta de administrador e clique no link "Admin". Você deve ver a lista de produtos e ser capaz de criar, atualizar ou excluir produtos.

> [!div class="step-by-step"]
> [Anterior](using-web-api-with-entity-framework-part-4.md)
> [Próximo](using-web-api-with-entity-framework-part-6.md)
