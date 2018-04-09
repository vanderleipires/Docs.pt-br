---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: 'Parte 4: Lista os produtos | Microsoft Docs'
author: JoeStagner
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks. Parte 4 abrange produtos de listagem com contr. o GridView.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: 69b26344e6dcdbf27e94da90ad5d6cd79f27ccd3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="part-4-listing-products"></a>Parte 4: Lista de produtos
====================
por [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks demonstra como extraordinariamente simples é criar aplicativos eficientes e escalonáveis para a plataforma .NET. Ele mostra como usar os novos recursos no ASP.NET 4 para criar uma loja online, inclusive de compras, check-out e administração.
> 
> Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks. Parte 4 abrange a listagem de produtos com o controle GridView.


## <a id="_Toc260221670"></a>  Lista os produtos com o controle GridView

Vamos começar a implementar nossa página ProductsList.aspx clicando em"direita" em nossa solução e selecionando "Adicionar" e "Novo Item".

![](tailspin-spyworks-part-4/_static/image1.jpg)

Escolha "Formulário usando página mestra dos Web" e insira um nome de página de ProductsList.aspx".

Clique em "Adicionar".

![](tailspin-spyworks-part-4/_static/image2.jpg)

Em seguida, escolha a pasta "Estilos" onde colocamos a página Site.Master e selecioná-lo na janela "Conteúdo da pasta".

![](tailspin-spyworks-part-4/_static/image3.jpg)

Clique em "Okey" para criar a página.

Nosso banco de dados é preenchido com dados de produto, conforme mostrado abaixo.

![](tailspin-spyworks-part-4/_static/image4.jpg)

Após a criação de nossa página novamente usaremos uma fonte de dados de entidade para acessar esses dados de produto, mas nesta instância, precisamos selecionar as entidades Product e é necessário restringir os itens que são retornados a apenas aqueles para a categoria selecionada.

Para fazer isso informaremos EntityDataSource para gerar automaticamente a cláusula WHERE e especificaremos a WhereParameter.

Você deve se lembrar que quando criamos os itens de Menu em nosso "Menu de categoria de produto" dinamicamente criamos o link, adicionando o CatagoryID para QueryString para cada link. A fonte de dados de entidade derivar o parâmetro onde esse parâmetro QueryString será informado.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

Em seguida, configuraremos o controle ListView para exibir uma lista de produtos. Para criar uma experiência ideal de compra que vai compactar vários recursos concisos para cada produto individual exibido em nosso ListVew.

- O nome do produto será um link para a exibição de detalhes do produto.
- O preço do produto será exibido.
- Uma imagem do produto será exibida e dinamicamente selecionaremos a imagem de um diretório de imagens de catálogo em nosso aplicativo.
- Podemos incluem um link para adicionar o produto específico imediatamente ao carrinho de compras.

Aqui está a marcação para a instância do controle ListView.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

Estamos criando vários links para cada produto exibido dinamicamente.

Além disso, antes de testamos própria nova página precisamos criar a estrutura de diretórios para o produto imagens de catálogo da seguinte maneira.

![](tailspin-spyworks-part-4/_static/image1.png)

Depois que a nossas imagens de produto estão acessíveis podemos testar nossa página de lista do produto.

![](tailspin-spyworks-part-4/_static/image5.jpg)

Na home page do site, clique em um dos Links de lista de categoria.

![](tailspin-spyworks-part-4/_static/image6.jpg)

Agora, precisamos implementar a página ProductDetials.apsx e a funcionalidade de AddToCart.

Use o arquivo -&gt;novo para criar um nome de página ProductDetails.aspx usando o site de página mestra como fizemos anteriormente.

Nós usaremos novamente um controle EntityDataSource para acessar o registro de produto específico no banco de dados e usaremos um controle FormView do ASP.NET para exibir os dados de produto da seguinte maneira.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

Não se preocupe se a formatação parecer um pouco estranho para você. A marcação acima deixa espaço no layout de exibição para alguns recursos que implementaremos posteriormente.

Representa uma lógica mais complexa no nosso aplicativo de carrinho de compras. Para começar, use o arquivo -&gt;novo para criar uma página chamada MyShoppingCart.aspx.

Observe que não estamos optando o nome ShoppingCart.aspx.

Nosso banco de dados contém uma tabela chamada "ShoppingCart". Quando é gerado um modelo de dados de entidade foi criada uma classe para cada tabela no banco de dados. Portanto, o modelo de dados de entidade gerado de uma classe de entidade nomeada "ShoppingCart". Podemos pode editar o modelo para que podemos pode usar esse nome para a implementação de carrinho de compras ou estendê-lo para nossas necessidades, mas é aceita em vez disso, para selecionar apenas um nome que pode evitar o conflito.

Também vale a pena observar que estamos criando um carrinho de compras simple e inserindo a lógica de carrinho de compras com a exibição do carrinho de compras. Podemos também pode optar por implementar nosso carrinho de compras em uma camada de negócios totalmente separada.

> [!div class="step-by-step"]
> [Anterior](tailspin-spyworks-part-3.md)
> [Próximo](tailspin-spyworks-part-5.md)
