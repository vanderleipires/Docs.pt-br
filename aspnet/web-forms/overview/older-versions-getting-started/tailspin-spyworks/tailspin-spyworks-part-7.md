---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: 'Parte 7: Adicionando recursos | Microsoft Docs'
author: JoeStagner
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks. Parte 7 adiciona recursos adicionais, como a conta revie...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: cada8d9aee649e4f2a5afc1ca2b46863ea458207
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833041"
---
<a name="part-7-adding-features"></a>Parte 7: Adição de recursos
====================
por [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks demonstra como incrivelmente simples é criar aplicativos avançados e escalonáveis para a plataforma .NET. Ele mostra como usar os novos recursos no ASP.NET 4 para criar uma loja online, incluindo as compras, check-out e administração.
> 
> Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks. Parte 7 adiciona recursos adicionais, como análise de conta, análises de produtos e "itens populares" e controles de usuário "também adquiridas".


## <a id="_Toc260221673"></a>  Adição de recursos

Embora os usuários podem procurar nosso catálogo, colocar itens no carrinho de compras e concluir o processo de check-out, há que um número de dar suporte a recursos que será incluído para melhorar nosso site.

1. Revisão de conta (lista de pedidos colocados e exibir detalhes.)
2. Adicione algum conteúdo específico do contexto para a página inicial.
3. Adicione um recurso para permitir que os usuários de revisão de produtos no catálogo.
4. Crie um controle de usuário para exibir itens populares e lugar que controlam na página inicial.
5. Criar um controle de usuário "Também adquiridos" e adicione-o para a página de detalhes do produto.
6. Adicionar um contato de página.
7. Adicionar uma página sobre.
8. Erro global

## <a id="_Toc260221674"></a>  Revisão de conta

Na pasta "Conta", crie duas páginas. aspx um OrderList.aspx nomeado e a outros OrderDetails.aspx nomeado

OrderList.aspx aproveitará os controles GridView e EntityDataSoure quanto temos anteriormente.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

O EntityDataSoure seleciona os registros da tabela filtrada no nome de usuário (consulte o WhereParameter) que definimos em uma variável de sessão quando o log do usuário do dentro.

Observe também esses parâmetros no HyperlinkField de GridView:

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

Eles especificam o link para a exibição de detalhes do pedido para cada produto especificando o campo de OrderID como um parâmetro QueryString para a página OrderDetails.aspx.

## <a id="_Toc260221675"></a>  OrderDetails.aspx

Usaremos um controle EntityDataSource para acessar os pedidos e um FormView para exibir os dados de pedidos e outro EntityDataSource com um GridView para exibir todos os itens da ordem de linha.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

No arquivo Code-Behind (OrderDetails.aspx.cs), temos dois bits pouco de manutenção do sistema.

Primeiro, precisamos garantir que OrderDetails sempre obtém um OrderId.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

Também precisamos calcular e exibir o total de itens de linha do pedido.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a>  A Home Page

Vamos adicionar algum conteúdo estático para a página Default. aspx.

Primeiro vou criar uma pasta de "Conteúdo" e dentro de uma pasta de imagens (e vou incluir uma imagem a ser usada na home page).

Para o espaço reservado de parte inferior da página Default. aspx, adicione a seguinte marcação.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a>  Análises de produtos

Primeiro, adicionaremos um botão com um link para um formulário que podemos usar para inserir uma revisão do produto.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

Observe que estamos passando o ProductID na cadeia de caracteres de consulta

Próxima vamos adicionar a página chamada ReviewAdd.aspx

Esta página será usar o ASP.NET AJAX Control Toolkit. Se você ainda não tiver feito para que você pode baixá-lo partir [DevExpress](http://devexpress.com/act) e há diretrizes sobre como configurar o Kit de ferramentas para uso com o Visual Studio aqui [ https://www.asp.net/learn/ajax-videos/video-76.aspx ](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).

No modo de design, arraste os controles e validadores da caixa de ferramentas e compilar um formulário como a mostrada abaixo.

![](tailspin-spyworks-part-7/_static/image2.jpg)

A marcação será algo parecido com isso.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

Agora que podemos inserir análises, vamos exibir essas análises na página do produto.

Adicione esta marcação à página ProductDetails.aspx.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

Executando agora nosso aplicativo e navegando até um produto mostram as informações de produto, incluindo as revisões do cliente.

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a>  Controle de itens populares (Criando controles de usuário)

Para aumentar as vendas em seu site da web, adicionaremos alguns recursos para produtos populares ou relacionados de "origem de venda sugestivo".

O primeiro desses recursos será uma lista do produto no nosso catálogo de produtos mais popular.

Criaremos um "controle de usuário" para exibir os itens na home page do nosso aplicativo mais vendidos. Uma vez que esse será um controle, podemos usar isso em qualquer página simplesmente arrastando e soltando o controle no designer do Visual Studio para qualquer página que gostamos de.

No Gerenciador de soluções do Visual Studio, clique com botão direito no nome da solução e crie um novo diretório chamado "Controles". Embora não seja necessário fazer isso, podemos ajudará a manter nosso projeto, organizado por criar todos os nossos controles de usuário no diretório "Controles".

Clique com botão direito na pasta de controles e escolha "Novo Item":

![](tailspin-spyworks-part-7/_static/image4.jpg)

Especifique um nome para o nosso controle de "PopularItems". Observe que a extensão de arquivo para controles de usuário ASCX não. aspx.

Nosso controle de usuário populares de itens será definido da seguinte maneira.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

Aqui, estamos usando um método que não usamos ainda neste aplicativo. Estamos usando o controle repeater e, em vez de usar um controle de fonte de dados estamos vinculando o controle Repeater aos resultados de uma consulta LINQ to Entities.

No code-behind do nosso controle fazemos isso da seguinte maneira.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

Observe também esta linha importante na parte superior da marcação do nosso controle.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

Uma vez que os itens mais populares não será alterada em uma base de minuto a minuto, podemos adicionar uma diretiva dor para melhorar o desempenho do nosso aplicativo. Essa diretiva fará com que o código de controles para ser executado apenas quando a saída do controle do cache expira. Caso contrário, a versão em cache de saída do controle será usada.

Agora tudo o que precisamos fazer é incluir nosso novo controle na nossa página Default.aspc.

Use arrastar e soltar para colocar uma instância do controle na coluna de nosso formulário de padrão aberta.

![](tailspin-spyworks-part-7/_static/image5.jpg)

Agora, quando executamos nosso aplicativo a home page exibe os itens mais populares.

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a>  "Também adquiridos" controlam (controles de usuário com parâmetros)

O segundo controle de usuário que vamos criar levará sugestivo de venda para o próximo nível com a adição de especificidade do contexto.

A lógica para calcular os principais itens "Também adquiridos" é não trivial.

Nosso controle "Também adquiridos" será selecionar os registros de OrderDetails (comprados anteriormente) para o ProductID selecionado no momento e pegue o OrderIDs para cada pedido exclusivo que é encontrado.

Em seguida, selecionaremos al os produtos de todos esses pedidos e soma as quantidades compradas. Vamos classificar os produtos que totalizam de quantidade e exibir os cinco principais itens.

Dada a complexidade da lógica do programa, podemos irá implementar esse algoritmo como um procedimento armazenado.

O T-SQL para o procedimento armazenado é da seguinte maneira.

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

Observe que esse procedimento armazenado (SelectPurchasedWithProducts) existia no banco de dados quando é incluído em nosso aplicativo e quando geramos o modelo de dados de entidade que especificamos que, além de tabelas e exibições que precisávamos fazer isso, o modelo de dados de entidade deve incluir esse procedimento armazenado.

Para acessar o procedimento armazenado do modelo de dados de entidade, é preciso importar a função.

Clique duas vezes no modelo de dados de entidade no Gerenciador de soluções para abri-lo no designer e abra o navegador de modelo, e em seguida, clique com botão direito no designer e selecione "Adicionar importação de função".

![](tailspin-spyworks-part-7/_static/image1.png)

Ao fazer isso, essa caixa de diálogo será aberta.

![](tailspin-spyworks-part-7/_static/image2.png)

Preencha os campos, como você pode ver acima, selecionando "SelectPurchasedWithProducts" e use o nome do procedimento para o nome da nossa função importada.

Clique em "Okey".

Tendo feito isso, que podemos simplesmente pode programar o procedimento armazenado como nós podem qualquer outro item no modelo.

Portanto, em nossa pasta de "Controles" Crie um novo controle de usuário chamado AlsoPurchased.ascx.

A marcação para esse controle parecerá muito familiar para o controle PopularItems.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

A diferença notável é que não armazenados em cache a saída, pois o item a ser renderizado variam por produto.

O ProductId será "property" para o controle.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

No manipulador do evento PreRender do controle é eed fazer três coisas.

1. Verifique se que o ProductID está definido.
2. Consulte se houver produtos que foram adquiridos com a atual.
3. Alguns itens, conforme determinado na #2 de saída.

Observe como é fácil chamar o procedimento armazenado por meio do modelo.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

Depois de determinar que há são "também adquiridos" podemos simplesmente fazer a ligação repetidor aos resultados retornados pela consulta.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

Se não houvesse todos os itens "também adquiridos" Vamos simplesmente exibir outros itens populares do nosso catálogo.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

Para exibir os itens "Também comprou", abra a página ProductDetails.aspx e arraste o controle de AlsoPurchased do Gerenciador de soluções, para que ele apareça nessa posição na marcação.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

Ao fazer isso, você criará uma referência ao controle na parte superior da página ProductDetails.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

Uma vez que o controle de usuário AlsoPurchased requer um número de ProductId, definimos a propriedade ProductID do nosso controle por meio de uma instrução Eval no item de modelo de dados atual da página.

![](tailspin-spyworks-part-7/_static/image3.png)

Quando criamos e executar agora e navegue até um produto, podemos ver os itens "Também adquiridos".

![](tailspin-spyworks-part-7/_static/image7.jpg)

> [!div class="step-by-step"]
> [Anterior](tailspin-spyworks-part-6.md)
> [Próximo](tailspin-spyworks-part-8.md)
