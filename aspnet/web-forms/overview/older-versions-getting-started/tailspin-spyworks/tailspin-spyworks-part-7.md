---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: 'Parte 7: Adicionando recursos | Microsoft Docs'
author: JoeStagner
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks. Parte 7 adiciona recursos adicionais, como a conta revie...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: 17f068155f6726047901e2f7d580d375a4e07c87
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="part-7-adding-features"></a>Parte 7: Adicionar recursos
====================
por [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks demonstra como extraordinariamente simples é criar aplicativos eficientes e escalonáveis para a plataforma .NET. Ele mostra como usar os novos recursos no ASP.NET 4 para criar uma loja online, inclusive de compras, check-out e administração.
> 
> Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks. Parte 7 adiciona recursos adicionais, como análise de conta, análises de produtos e "itens populares" e controles de usuário "também comprado".


## <a id="_Toc260221673"></a>  Adicionando recursos

Embora os usuários podem procurar nosso catálogo, colocar itens no carrinho de compras e concluir o processo de check-out, há que um número de dar suporte a recursos que serão incluídos para melhorar o nosso site.

1. Revisão de conta (lista de pedidos colocados e exibir mais detalhes.)
2. Acrescente conteúdo específico de contexto para a página inicial.
3. Adicione um recurso para permitir que usuários revisar os produtos no catálogo.
4. Crie um controle de usuário para exibir itens populares e local que controlam na página inicial.
5. Criar um controle de usuário "Também adquirido" e adicione-o para a página de detalhes do produto.
6. Adicionar um contato de página.
7. Adicionar um sobre a página.
8. Erro global

## <a id="_Toc260221674"></a>  Revisão de conta

Na pasta "Conta", crie duas páginas. aspx um OrderList.aspx nomeado e a outros OrderDetails.aspx nomeado

OrderList.aspx aproveitará os controles GridView e EntityDataSoure como temos anteriormente.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

O EntityDataSoure seleciona os registros da tabela filtrada no nome de usuário (consulte o WhereParameter) que é definido em uma variável de sessão quando o logon do usuário 's.

Observe também esses parâmetros no HyperlinkField de GridView:

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

Elas especificam o link para a exibição de detalhes do pedido para cada produto especificando o campo OrderID como um parâmetro QueryString para a página OrderDetails.aspx.

## <a id="_Toc260221675"></a>  OrderDetails.aspx

Usaremos um controle EntityDataSource para acessar os pedidos e um FormView para exibir os dados de pedidos e outro EntityDataSource com um controle GridView para exibir os itens de linha de todos os da ordem.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

O código por trás de arquivo (OrderDetails.aspx.cs) temos dois bits pouco de manutenção do sistema.

Primeiro, precisamos garantir que OrderDetails sempre obtém um OrderId.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

Também precisamos calcular e exibir o total de itens de linha do pedido.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a>  A Home Page

Vamos adicionar alguns conteúdo estático para a página Default.aspx.

Primeiro, vou criar uma pasta "Conteúda" e dentro de uma pasta de imagens (e incluirá uma imagem a ser usada na página inicial).

Para o espaço reservado de parte inferior da página Default.aspx, adicione a seguinte marcação.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a>  Análises de produtos

Primeiro, adicionaremos um botão com um link para um formulário que podemos usar para inserir uma revisão do produto.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

Observe que, passa o ProductID na cadeia de caracteres de consulta

Avançar vamos adicionar página chamada ReviewAdd.aspx

Essa página usará o Kit de ferramentas de controle do ASP.NET AJAX. Se você ainda não tiver feito para você pode baixá-lo do [DevExpress](http://devexpress.com/act) e há diretrizes sobre como configurar o Kit de ferramentas para uso com o Visual Studio aqui [ https://www.asp.net/learn/ajax-videos/video-76.aspx ](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).

No modo design, arraste validadores e controles da caixa de ferramentas e criar um formulário como a mostrada abaixo.

![](tailspin-spyworks-part-7/_static/image2.jpg)

A marcação será parecida com isto.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

Agora que podemos inserir revisões, permite exibir as revisões na página do produto.

Adicione esta marcação para a página ProductDetails.aspx.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

Executando o nosso aplicativo agora e navegar para um produto mostram as informações de produto, incluindo revisões de cliente.

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a>  Controle de itens populares (criação de controles de usuário)

Para aumentar as vendas em seu site da web vamos adicionar alguns recursos para produtos de "venda que sugere" populares ou relacionados.

O primeiro desses recursos será uma lista do produto mais popular do nosso catálogo de produtos.

Vamos criar um "controle de usuário" para exibir o itens na página inicial do nosso aplicativo vendido. Já que isso será um controle, podemos usar isso em qualquer página simplesmente arrastando e soltando o controle no designer do Visual Studio para qualquer página que gostamos.

No Gerenciador de soluções do Visual Studio, clique com botão direito no nome da solução e crie um novo diretório chamado "Controles". Embora não seja necessário fazer isso, nós o ajudaremos manter nosso projeto criando todos os controles de usuário no diretório "Controles".

Com o botão direito na pasta de controles e escolha "New Item":

![](tailspin-spyworks-part-7/_static/image4.jpg)

Especifique um nome para o nosso controle de "PopularItems". Observe que a extensão de arquivo para controles de usuário é. ascx não. aspx.

Nosso controle de usuário populares de itens será definido da seguinte maneira.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

Aqui, estamos usando um método que não usamos ainda neste aplicativo. Estamos usando o controle repetidor e em vez de usar um controle de fonte de dados estamos vinculando controle repetidor para os resultados de uma consulta LINQ to Entities.

No code-behind de nosso controle podemos fazer isso da seguinte maneira.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

Observe também esta linha importante na parte superior da marcação do nosso controle.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

Como os itens mais populares não será alterada em uma base de minuto a minuto podemos adicionar uma diretiva dor para melhorar o desempenho do nosso aplicativo. Essa diretiva fará com que o código de controles ser executado apenas quando a saída do controle do cache expira. Caso contrário, a versão armazenada em cache de saída do controle será usada.

Agora tudo o que precisamos fazer é inclua nosso novo controle em nossa página de Default.aspc.

Use arrastar e soltar para colocar uma instância do controle na coluna de nosso formulário de padrão aberta.

![](tailspin-spyworks-part-7/_static/image5.jpg)

Agora, quando executamos nosso aplicativo home page exibe os itens mais populares.

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a>  "Também adquirido" controla (controles de usuário com parâmetros)

O segundo controle de usuário que vamos criar terão que sugere de vendas para o próximo nível com a adição de especificidade de contexto.

A lógica para calcular os principais itens "Também adquirido" é incomum.

Nosso controle "Também adquirido" selecionar os registros de OrderDetails (tenha comprados) para o ProductID selecionado no momento e obter OrderIDs para cada pedido exclusivo que é encontrado.

Em seguida, selecionaremos todos os produtos de todos esses pedidos e soma as quantidades compradas. Vamos classificar os produtos que totalizam quantidade e exibir os cinco principais itens.

Dada a complexidade da lógica do programa, vamos implementar esse algoritmo como um procedimento armazenado.

O T-SQL para o procedimento armazenado é o seguinte.

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

Observe que esse procedimento armazenado (SelectPurchasedWithProducts) existe no banco de dados quando é incluído em nosso aplicativo e é gerado o modelo de dados de entidade especificamos que, além das tabelas e exibições que são necessários, o modelo de dados de entidade deve incluir esse procedimento armazenado.

Para acessar o procedimento armazenado do modelo de dados de entidade, é preciso importar a função.

Clique duas vezes no modelo de dados de entidade no Gerenciador de soluções para abri-lo no designer e abrir o navegador de modelo, em seguida, com o botão direito no designer e selecione "Adicionar importação de função".

![](tailspin-spyworks-part-7/_static/image1.png)

Ao fazer isso, esta caixa de diálogo será aberta.

![](tailspin-spyworks-part-7/_static/image2.png)

Preencha os campos que você vê acima, selecionando "SelectPurchasedWithProducts" e use o nome do procedimento para o nome da nossa função importada.

Clique em "Okey".

Ter feito isso, simplesmente pode programar o procedimento armazenado é como qualquer outro item no modelo.

Portanto, em nossa pasta de "Controles" Crie um novo controle de usuário chamado AlsoPurchased.ascx.

A marcação para este controle parecerá muito familiar para o controle PopularItems.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

A diferença notável é que não armazenados em cache de saída desde que o item a ser renderizado variam por produto.

O ProductId será "property" para o controle.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

No manipulador de evento PreRender do controle é sangramento três itens.

1. Verifique se que o ProductID está definido.
2. Consulte se há quaisquer produtos foram comprados com a atual.
3. Saída de alguns itens conforme determinado na #2.

Observe como é fácil chamar o procedimento armazenado por meio do modelo.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

Depois de determinar o que há "também comprados" podemos simplesmente associar repetidor aos resultados retornados pela consulta.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

Se não havia nenhum item "também adquirido" simplesmente exibiremos outros itens populares do nosso catálogo.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

Para exibir os itens "Também adquirido", abra a página ProductDetails.aspx e arraste o controle de AlsoPurchased do Gerenciador de soluções para que ele apareça nessa posição na marcação.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

Isso criará uma referência para o controle na parte superior da página ProductDetails.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

Como o controle de usuário AlsoPurchased requer um número de ProductId, vamos definir a propriedade ProductID de nosso controle usando uma instrução Eval em relação ao item de modelo de dados atual da página.

![](tailspin-spyworks-part-7/_static/image3.png)

Quando podemos criar e executar agora e navegue até um produto, podemos ver os itens "Também adquirido".

![](tailspin-spyworks-part-7/_static/image7.jpg)

> [!div class="step-by-step"]
> [Anterior](tailspin-spyworks-part-6.md)
> [Próximo](tailspin-spyworks-part-8.md)
