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
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: e4342e634ef8c4bcf4e0085650a28f414ab23736
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30885075"
---
<a name="part-5-business-logic"></a>Parte 5: Lógica de negócios
====================
por [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks demonstra como extraordinariamente simples é criar aplicativos eficientes e escalonáveis para a plataforma .NET. Ele mostra como usar os novos recursos no ASP.NET 4 para criar uma loja online, inclusive de compras, check-out e administração.
> 
> Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks. Parte 5 adiciona alguma lógica de negócios.


## <a id="_Toc260221671"></a>  Adicionando alguma lógica de negócios

Queremos que nossa experiência de compra estejam disponíveis sempre que alguém visitar nosso site. Os visitantes poderão procurar e adicionar itens ao carrinho de compras, mesmo se eles não são registrados ou registrados no. Quando estiverem prontas para check-out eles recebem a opção de autenticar e se não estiverem ainda membros poderão criar uma conta.

Isso significa que será necessário implementar a lógica para converter o carrinho de compras de um estado anônimo para um estado de "Usuário registrado".

Vamos criar um diretório chamado "Classes", em seguida, com o botão direito na pasta e criar um novo arquivo de "Classe" denominado MyShoppingCart.cs

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

Conforme mencionado anteriormente, estender a classe que implementa a página MyShoppingCart.aspx e podemos faz isso usando. Construir avançada "classe parcial" do NET.

A chamada gerada para nosso arquivo MyShoppingCart.aspx.cf tem esta aparência.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

Observe o uso da palavra-chave "parcial".

O arquivo de classe que acabou de gerar tem esta aparência.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

Podemos mesclará nossas implementações adicionando a palavra-chave partial para este arquivo.

Nosso novo arquivo de classe agora tem esta aparência.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

O primeiro método que vamos adicionar à nossa classe é o método "AddItem". Esse é o método que será chamado, por fim, quando o usuário clicar nos links "Adicionar a arte" nas páginas de lista de produtos e os detalhes do produto.

Acrescente o seguinte ao usando instruções na parte superior da página.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

E adicione esse método à classe MyShoppingCart.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

Estamos usando LINQ to Entities para ver se o item já está no carrinho. Se assim, podemos atualizar a quantidade da ordem do item, caso contrário, crie uma nova entrada para o item selecionado

Para chamar esse método Vamos implementar uma página AddToCart.aspx não apenas esse método de classe, mas, em seguida, exibidos atual um = carrinho de compras depois que o item foi adicionado.

Clique com botão direito no nome da solução no Gerenciador de soluções e adicionar e nova página chamada AddToCart.aspx como fizemos anteriormente.

Enquanto podemos usar essa página para exibir os resultados intermediários como baixos problemas de estoque, etc., em nossa implementação, a página será, na verdade, não renderizar, mas em vez disso, chame a lógica de "Adicionar" e redirecionar.

Para fazer isso, adicionaremos o código a seguir para a página\_evento de carregamento.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

Observe que o produto para adicionar ao carrinho de compras de um parâmetro de QueryString e chamar o método AddItem de nossa classe está sendo recuperado.

Supondo que nenhum erro for encontrado controle é passado para a página de SHoppingCart.aspx que totalmente Vamos implementar em seguida. Se houver um erro, lançamos uma exceção.

No momento não ainda implementamos um manipulador de erro global para passariam essa exceção não tratada pelo nosso aplicativo, mas podemos corrigirá isso em breve.

Observe também o uso da instrução Debug.Fail() (disponíveis por meio de `using System.Diagnostics;)`

É o aplicativo é executado dentro do depurador, este método exibirá uma caixa de diálogo detalhada com informações sobre o estado de aplicativos junto com a mensagem de erro que especificamos.

Quando executado em produção, a instrução Debug.Fail() será ignorado.

Você observará no código acima de uma chamada para um método em nossos nomes de classe carrinho compras "GetShoppingCartId".

Adicione o código para implementar o método da seguinte maneira.

Observe que também adicionamos botões de atualização e o check-out e um rótulo em que podemos exibir carrinho de "total".

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

Agora podemos pode adicionar itens ao nosso carrinho de compras, mas não implementamos a lógica para exibir o carrinho depois da adição de um produto.

Portanto, a página MyShoppingCart.aspx adicionaremos um controle EntityDataSource e o controle GridVire da seguinte maneira.

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

Chame o formulário no designer de forma que você pode clicar duas vezes em um botão Atualizar carrinho e gerar o manipulador de eventos de clique que é especificado na declaração na marcação.

Implementaremos os detalhes mais tarde, mas isso vamos compilar e executar o nosso aplicativo sem erros.

Quando você executa o aplicativo e adiciona um item ao carrinho de compras, você verá isso.

![](tailspin-spyworks-part-5/_static/image2.jpg)

Observe que estamos ter deviated de exibição de grade "padrão" Implementando três colunas personalizadas.

A primeira é uma editável, o campo "Associado" para a quantidade:

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

O próximo é uma coluna "calculada" que exibe o total de item de linha (o item de custo vezes a quantidade a ser ordenada):

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

Por fim, temos uma coluna personalizada que contém um controle de caixa de seleção que o usuário usará para indicar que o item deve ser removido do gráfico de compras.

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

Como você pode ver, a ordem Total da linha é vazia por isso vamos adicionar alguns lógica para calcular o Total de pedidos.

Primeiro, implementaremos um método "GetTotal genérica" à nossa classe MyShoppingCart.

No arquivo MyShoppingCart.cs, adicione o código a seguir.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

Em seguida, na página\_manipulador de eventos de carga pode chamaremos nosso método GetTotal genérica. Ao mesmo tempo, adicionaremos um teste para verificar se o carrinho de compras está vazio e ajustar a exibição adequadamente, se ele for.

Agora o carrinho de compras está vazio se isso:

![](tailspin-spyworks-part-5/_static/image4.jpg)

E se não, podemos ver nosso total.

![](tailspin-spyworks-part-5/_static/image5.jpg)

No entanto, essa página ainda não está completa.

Precisaremos lógica adicional para recalcular o carrinho de compras, removendo itens marcados para remoção e determinando novos valores de quantidade como alguns podem ter sido alterado na grade pelo usuário.

Permite adicionar um método "RemoveItem" a nossa classe de carrinho de compras em MyShoppingCart.cs para lidar com o caso quando um usuário marca um item para remoção.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

Agora vamos ad um método para tratar o caso quando um usuário altera simplesmente a qualidade seja ordenado em GridView.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

Com os recursos básicos de remoção e atualização in-loco, podemos implementar a lógica que atualiza, na verdade, o carrinho de compras no banco de dados. (Em MyShoppingCart.cs)

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

Você observará que este método espera dois parâmetros. Uma é o Id do carrinho de compras e a outra é uma matriz de objetos do tipo definido pelo usuário.

Para minimizar a dependência de nossa lógica em especificações de interface do usuário, definimos uma estrutura de dados que podemos usar para passar os itens do carrinho de compras para nosso código sem nosso método precisar acessar diretamente o controle GridView.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

Em nosso arquivo MyShoppingCart.aspx.cs podemos usar esta estrutura em nosso manipulador de eventos de clique de botão de atualização da seguinte maneira. Observe que, além de atualizar o carrinho atualizaremos total carrinho.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

Observe, com interesse em particular, esta linha de código:

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

GetValues() é uma função auxiliar especial que vamos implementar em MyShoppingCart.aspx.cs da seguinte maneira.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

Isso fornece uma maneira simples para acessar os valores dos elementos associados no nosso controle GridView. Como nosso controle de caixa de seleção "Remover o Item" não está ligado vamos acessá-lo por meio do método FindControl().

Neste estágio do desenvolvimento do seu projeto estamos obtendo prontos para implementar o processo de check-out.

Antes de fazer isso permite use o Visual Studio para gerar o banco de dados de associação e adicionar um usuário para o repositório de associação.

> [!div class="step-by-step"]
> [Anterior](tailspin-spyworks-part-4.md)
> [Próximo](tailspin-spyworks-part-6.md)
