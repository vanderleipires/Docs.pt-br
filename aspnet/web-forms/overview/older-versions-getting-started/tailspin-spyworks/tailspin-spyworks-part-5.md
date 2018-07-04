---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: 'Parte 5: Lógica de negócios | Microsoft Docs'
author: JoeStagner
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks. Parte 5 adiciona alguma lógica de negócios.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: 719d56e0764e2f66b8813c9487119bbc700d738c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377980"
---
<a name="part-5-business-logic"></a>Parte 5: Lógica de negócios
====================
por [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks demonstra como incrivelmente simples é criar aplicativos avançados e escalonáveis para a plataforma .NET. Ele mostra como usar os novos recursos no ASP.NET 4 para criar uma loja online, incluindo as compras, check-out e administração.
> 
> Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks. Parte 5 adiciona alguma lógica de negócios.


## <a id="_Toc260221671"></a>  Adicionar alguma lógica de negócios

Queremos que nossa experiência de compra esteja disponível sempre que alguém visitar nosso site da web. Os visitantes será capazes de procurar e adicionar itens ao carrinho de compras, mesmo se eles são registrados ou não conectados. Quando eles estão prontos para fazer check-out eles recebem a opção para autenticar e se eles não estiverem ainda membros poderão criar uma conta.

Isso significa que precisamos implementar a lógica para converter o carrinho de compras de um estado anônimo para um estado de "Usuário registrado".

Vamos criar um diretório chamado "Classes", em seguida, clique com botão direito na pasta e crie um novo arquivo de "Class" chamado MyShoppingCart.cs

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

Como mencionado anteriormente estenderemos a classe que implementa a página MyShoppingCart.aspx e faremos isso usar. Construção poderosa parcial "Class do NET".

A chamada gerada para nosso arquivo MyShoppingCart.aspx.cf tem esta aparência.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

Observe o uso da palavra-chave "parcial".

O arquivo de classe que acabou de gerar esta aparência.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

Mesclaremos nossas implementações, adicionando a palavra-chave partial para esse arquivo também.

Agora, o nosso novo arquivo de classe se parece com isso.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

O primeiro método, adicionaremos à nossa classe é o método "AddItem". Esse é o método que será chamado, por fim, quando o usuário clica nos links de "Adicionar a arte" nas páginas de lista de produtos e os detalhes do produto.

Acrescente o seguinte ao usando as instruções na parte superior da página.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

E adicione esse método à classe MyShoppingCart.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

Estamos usando LINQ to Entities para ver se o item já está no carrinho. Se assim, podemos atualizar a quantidade de pedido do item, caso contrário, podemos criar uma nova entrada para o item selecionado

Para chamar esse método, implementaremos uma página AddToCart.aspx não apenas este método de classe, mas, em seguida, exibidos atual um = carrinho de compras depois que o item foi adicionado.

Clique com botão direito no nome da solução no Gerenciador de soluções e adicione e nova página chamada AddToCart.aspx como fizemos anteriormente.

Enquanto estamos poderia usar esta página para exibir os resultados intermediários, como problemas de estoque baixo, etc., em nossa implementação, a página, na verdade não renderizar, mas em vez disso, chamará a lógica de "Adicionar" e redirecionar.

Para fazer isso, adicionaremos o código a seguir para a página\_evento Load.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

Observe que o produto para adicionar ao carrinho de compras de um parâmetro QueryString e chamando o método AddItem de nossa classe está sendo recuperado.

Supondo que nenhum erro for encontrado controle é passado para a página de SHoppingCart.aspx que estamos totalmente implementará em seguida. Se deve haver um erro, geramos uma exceção.

No momento, estamos ainda não implementou um manipulador de erro global para que iria essa exceção não tratada pelo nosso aplicativo, mas podemos corrigirá isso em breve.

Observe também o uso da instrução Debug.Fail() (disponível por meio de `using System.Diagnostics;)`

É o aplicativo é executado dentro do depurador, este método exibirá uma caixa de diálogo detalhada com informações sobre o estado de aplicativos junto com a mensagem de erro que especificamos.

Quando em execução na produção, a instrução Debug.Fail() será ignorada.

Você observará no código acima de uma chamada para um método em nossos nomes de classe compras do carrinho "GetShoppingCartId".

Adicione o código para implementar o método da seguinte maneira.

Observe que também adicionamos botões de atualização e o check-out e um rótulo em que podemos exibir o carrinho "total".

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

Podemos agora adicionar itens ao nosso carrinho de compras, mas não implementamos a lógica para exibir o carrinho depois da adição de um produto.

Então, na página MyShoppingCart.aspx, adicionaremos um controle EntityDataSource e um controle de GridVire da seguinte maneira.

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

Chame o formulário no designer para que você possa clique duas vezes no botão Atualizar carrinho e gerar o manipulador de eventos de clique que é especificado na declaração na marcação.

Implementaremos os detalhes mais tarde, mas fazer isso será vamos compilar e executar nosso aplicativo sem erros.

Quando você executa o aplicativo e adiciona um item ao carrinho de compras, você verá isso.

![](tailspin-spyworks-part-5/_static/image2.jpg)

Observe que nós desviaram de exibição de grade "default" com a implementação de três colunas personalizadas.

A primeira é um editável, o campo "Ligado" para a quantidade:

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

A próxima é uma coluna "calculada" que exibe o total de item de linha (o item de custo vezes a quantidade a ser ordenada):

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

Por fim, temos uma coluna personalizada que contém um controle de caixa de seleção que o usuário utilizará para indicar que o item deve ser removido do gráfico de compras.

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

Como você pode ver, a ordem de linha Total estiver vazia então, vamos adicionar alguma lógica para calcular o Total de pedidos.

Primeiro, implementaremos um método "GetTotal genérica" à nossa classe MyShoppingCart.

No arquivo MyShoppingCart.cs, adicione o código a seguir.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

Em seguida, na página\_pode chamaremos nosso método GetTotal genérica de manipulador de eventos de carga. Ao mesmo tempo, adicionaremos um teste para verificar se o carrinho de compras está vazio e ajustar a exibição adequadamente, se ele for.

Agora o carrinho de compras está vazio se isso:

![](tailspin-spyworks-part-5/_static/image4.jpg)

E se não for, podemos ver nosso total.

![](tailspin-spyworks-part-5/_static/image5.jpg)

No entanto, essa página ainda não está completa.

Precisaremos lógica adicional para recalcular o carrinho de compras, removendo os itens marcados para remoção e determinando os novos valores de quantidade conforme alguns podem ter sido alterado na grade pelo usuário.

Permite adicionar um método de "RemoveItem" à nossa classe de carrinho de compras em MyShoppingCart.cs para lidar com o caso quando um usuário marca um item para remoção.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

Agora vamos ad um método para manipular o caso quando um usuário altera simplesmente a qualidade para serem ordenados no GridView.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

Com os recursos básicos de remover e atualização em vigor, podemos implementar a lógica que realmente atualiza o carrinho de compras no banco de dados. (Em MyShoppingCart.cs)

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

Você observará que este método espera dois parâmetros. Uma é o Id do carrinho de compras e a outra é uma matriz de objetos do tipo definido pelo usuário.

Para reduzir a dependência de nossa lógica de especificações de interface do usuário, definimos uma estrutura de dados que podemos usar para passar os itens do carrinho de compras para nosso código sem a necessidade de acessar diretamente o controle GridView ao nosso método.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

Em nosso arquivo MyShoppingCart.aspx.cs podemos usar essa estrutura em nosso manipulador de eventos de clique de botão de atualização da seguinte maneira. Observe que, além de atualizar o carrinho atualizaremos também o total do carrinho.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

Observe com interesse particular esta linha de código:

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

GetValues() é uma função auxiliar especial que vamos implementar em MyShoppingCart.aspx.cs da seguinte maneira.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

Isso fornece uma maneira simples para acessar os valores dos elementos associados em nosso controle GridView. Como nosso controle de caixa de seleção "Remover o Item" não está associado vamos acessá-lo por meio do método FindControl().

Neste estágio no desenvolvimento do seu projeto estamos nos Preparando implementar o processo de check-out.

Antes de fazer isso vamos use o Visual Studio para gerar o banco de dados de associação e adicionar um usuário para o repositório de associação.

> [!div class="step-by-step"]
> [Anterior](tailspin-spyworks-part-4.md)
> [Próximo](tailspin-spyworks-part-6.md)
