---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: 'Parte 4: Lista os produtos | Microsoft Docs'
author: JoeStagner
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks. Parte 4 aborda a listagem de produtos com o GridView contr....
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: 929d88d747c25ca7c4f6f991421cb3aa9456aa45
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368803"
---
<a name="part-4-listing-products"></a>Parte 4: Listagem de produtos
====================
por [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks demonstra como incrivelmente simples é criar aplicativos avançados e escalonáveis para a plataforma .NET. Ele mostra como usar os novos recursos no ASP.NET 4 para criar uma loja online, incluindo as compras, check-out e administração.
> 
> Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks. Parte 4 aborda a listagem de produtos com o controle GridView.


## <a id="_Toc260221670"></a>  Listagem de produtos com o controle GridView

Vamos começar a implementar nossa página ProductsList.aspx clicando em"com o botão direito" em nossa solução e selecionando "Adicionar" e "Novo Item".

![](tailspin-spyworks-part-4/_static/image1.jpg)

Escolha "Formulário usando página mestra dos Web" e insira um nome de página de ProductsList.aspx".

Clique em "Adicionar".

![](tailspin-spyworks-part-4/_static/image2.jpg)

Em seguida, escolha a pasta "Estilos" onde colocamos a página Master e selecioná-lo na janela de "Conteúdo da pasta".

![](tailspin-spyworks-part-4/_static/image3.jpg)

Clique em "Okey" para criar a página.

Nosso banco de dados é preenchido com dados do produto, conforme mostrado abaixo.

![](tailspin-spyworks-part-4/_static/image4.jpg)

Depois que nossa página é criada, usaremos novamente uma fonte de dados de entidade para acessar esses dados de produto, mas nesta instância, precisamos selecionar as entidades Product e precisamos restringir os itens que são retornados a apenas aqueles para a categoria selecionada.

Para fazer isso, informaremos o EntityDataSource para gerar automaticamente a cláusula WHERE e especificaremos o WhereParameter.

Você irá se lembrar que quando criamos os itens de Menu no nosso "Menu de categoria de produto" é criada dinamicamente o link, adicionando o CatagoryID a cadeia de consulta para cada link. A fonte de dados de entidade para derivar o parâmetro de onde esse parâmetro QueryString será informado.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

Em seguida, configuraremos o controle ListView para exibir uma lista de produtos. Para criar uma experiência ideal de compra que podemos irá compactar vários recursos concisos para cada produto individual exibido em nossa ListVew.

- O nome do produto será um link para a exibição de detalhes do produto.
- O preço do produto será exibido.
- Uma imagem do produto será exibida e dinamicamente selecionaremos a imagem de um diretório de imagens de catálogo em nosso aplicativo.
- Incluiremos um link para adicionar imediatamente o produto específico ao carrinho de compras.

Aqui está a marcação para a nossa instância do controle ListView.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

Estamos criando vários links para cada produto exibido dinamicamente.

Além disso, antes que podemos testar a nova página do própria precisamos criar a estrutura de diretórios para o produto imagens do catálogo da seguinte maneira.

![](tailspin-spyworks-part-4/_static/image1.png)

Depois que as nossas imagens de produto são acessíveis podemos testar nossa página de lista do produto.

![](tailspin-spyworks-part-4/_static/image5.jpg)

Na home page do site, clique em um dos Links da lista de categoria.

![](tailspin-spyworks-part-4/_static/image6.jpg)

Agora, precisamos implementar a página ProductDetials.apsx e a funcionalidade de AddToCart.

Use o arquivo -&gt;New para criar um nome de página ProductDetails.aspx usando o página mestra do site, como fizemos anteriormente.

Nós usaremos novamente um controle EntityDataSource para acessar o registro de produto específico no banco de dados e usaremos um controle ASP.NET FormView para exibir os dados de produto da seguinte maneira.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

Não se preocupe se a formatação se parece um pouco estranho para você. A marcação acima deixa espaço no layout de exibição para alguns recursos, implementaremos posteriormente.

O carrinho de compras representará uma lógica mais complexa em nosso aplicativo. Para começar, use o arquivo -&gt;novo para criar uma página chamada MyShoppingCart.aspx.

Observe que não estamos escolhendo o nome ShoppingCart.aspx.

Nosso banco de dados contém uma tabela chamada "ShoppingCart". Quando geramos um modelo de dados de entidade foi criada uma classe para cada tabela no banco de dados. Portanto, o modelo de dados de entidade gerado de uma classe de entidade chamada "ShoppingCart". Podemos pode editar o modelo para que podemos pode usar esse nome para nossa implementação de carrinho de compras ou estendê-lo para nossas necessidades, mas optamos por em vez disso, para selecionar apenas um nome que evitará o conflito.

Também vale a pena observar que estamos criando um carrinho de compras simple e incorporar a lógica de carrinho de compras com a exibição de carrinho de compras. Podemos também pode optar por implementar nosso carrinho de compras em uma camada de negócios completamente separada.

> [!div class="step-by-step"]
> [Anterior](tailspin-spyworks-part-3.md)
> [Próximo](tailspin-spyworks-part-5.md)
