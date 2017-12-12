---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: Exibir dados de itens e detalhes | Microsoft Docs
author: Erikre
description: "Esta série de tutorial irá ensiná-lo as Noções básicas de criação de um aplicativo de Web Forms do ASP.NET usando o ASP.NET 4.5 e o Microsoft Visual Studio Express 2013 para nós..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 809d7a9c21a3ddf5dfd07d079eb8fe0d1d81712d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="display-data-items-and-details"></a>Exibir dados de itens e detalhes
====================
Por [Erik Reitan](https://github.com/Erikre)

[Baixe o projeto de exemplo do Wingtip Toys (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [baixar livro eletrônico (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Esta série de tutorial irá ensiná-lo as Noções básicas de criação de um aplicativo de Web Forms do ASP.NET usando o ASP.NET 4.5 e o Microsoft Visual Studio Express 2013 para Web. Um Visual Studio 2013 [projeto com o código-fonte c#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) está disponível para acompanhar esta série de tutoriais.


Este tutorial descreve como exibir itens de dados e detalhes de item de dados usando o Web Forms do ASP.NET e o Entity Framework Code First. Este tutorial se baseia no tutorial anterior "Da interface do usuário e a navegação" e é parte da série de tutorial da loja de brinquedos Wingtip. Depois de concluir este tutorial, você poderá ver os produtos no *ProductsList.aspx* página e detalhes sobre um produto individual no *ProductDetails.aspx* página.

## <a name="what-youll-learn"></a>O que você aprenderá:

- Como adicionar um controle de dados para exibir produtos do banco de dados.
- Como se conectar a um controle de dados para os dados selecionados.
- Como adicionar um controle de dados para exibir detalhes do produto do banco de dados.
- Como recuperar um valor de cadeia de caracteres de consulta e use esse valor para limitar os dados recuperados do banco de dados.

### <a name="these-are-the-features-introduced-in-the-tutorial"></a>Estes são os recursos apresentados no tutorial de:

- Associação de modelo
- Provedores de valor

## <a name="adding-a-data-control-to-display-products"></a>Adicionando um controle de dados para exibir produtos

Ao associar dados a um controle de servidor, há algumas opções diferentes, que você pode usar. As opções mais comuns incluem adicionando um controle de fonte de dados, adicionando o código manualmente ou usando a associação de modelo.

### <a name="using-a-data-source-control-to-bind-data"></a>Usando um controle de fonte de dados para associar dados

Adicionando um controle de fonte de dados permite que você vincular o controle de fonte de dados para o controle que exibe os dados. Essa abordagem permite a conexão declarativamente controles do lado do servidor diretamente a fontes de dados, em vez de usar uma abordagem programática.

### <a name="coding-by-hand-to-bind-data"></a>Codificação manual para associar dados

Adicionando código manualmente envolve ler o valor, procurando um valor nulo, tentar convertê-lo para o tipo apropriado, verificando se a conversão foi bem-sucedida e por fim, usando o valor na consulta. Você usaria essa abordagem quando você precisar manter controle total sobre a lógica de acesso a dados.

### <a name="using-model-binding-to-bind-data"></a>Usando o modelo de associação para associar dados

Usando a associação de modelo permite que você vincule os resultados usando muito menos código e oferece a capacidade de reutilizar a funcionalidade em seu aplicativo. Associação de modelo tem como objetivo para simplificar o trabalho com a lógica de acesso a dados focados no código enquanto ainda retém os benefícios de uma estrutura de vinculação de dados avançado.

## <a name="displaying-products"></a>Exibindo produtos

Neste tutorial, você usará a associação de modelo para associar dados. Para configurar um controle de dados para usar a associação de modelo para selecionar os dados, você define o controle `SelectMethod` propriedade para o nome de um método no código da página. O controle de dados chama o método no momento apropriado no ciclo de vida da página e vincula automaticamente os dados retornados. Não é necessário chamar explicitamente o `DataBind` método.

Usando as etapas a seguir, você modificará a marcação no *ProductList. aspx* página para que a página pode exibir produtos.

1. Em **Solution Explorer**, abra o *ProductList. aspx* página.
2. Substitua a marcação existente com a seguinte marcação:   

    [!code-aspx[Main](display_data_items_and_details/samples/sample1.aspx)]

Esse código usa um **ListView** controle chamado "verificação" para exibir os produtos.

[!code-aspx[Main](display_data_items_and_details/samples/sample2.aspx)]

O **ListView** controle exibe dados em um formato que você define usando modelos e estilos. É útil para dados em qualquer estrutura de repetição. Isso **ListView** exemplo simplesmente mostra dados de banco de dados, no entanto, você pode habilitar usuários para editar, inserir e excluir dados e para classificar e dados da página, tudo sem código.

Definindo o `ItemType` propriedade o **ListView** controlar, a expressão de associação de dados `Item` está disponível e o controle se torna fortemente tipado. Conforme mencionado no tutorial anterior, você pode selecionar os detalhes do objeto de Item usando o IntelliSense, como especificar o `ProductName`:

![Exibir dados de itens e detalhes - IntelliSense](display_data_items_and_details/_static/image1.png)

Além disso, você estiver usando a associação de modelo para especificar um `SelectMethod` valor. Esse valor (`GetProducts`) corresponderá ao método que você adicionará ao code-behind para exibir produtos na próxima etapa.

### <a name="adding-code-to-display-products"></a>Adicionando código para exibir produtos

Nesta etapa, você adicionará código para preencher o **ListView** controle com dados de produto do banco de dados. O código dará suporte a produtos de exibição por categoria individual, bem como mostrando todos os produtos.

1. Em **Solution Explorer**, clique com botão direito *ProductList. aspx* e, em seguida, clique em **Exibir código**.
2. Substitua o código existente no *ProductList.aspx.cs* arquivo com o código a seguir:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

Este código mostra o `GetProducts` método que é referenciado pelo `ItemType` propriedade do **ListView** controlar o *ProductList. aspx* página. Para limitar os resultados a uma categoria específica no banco de dados, o código define o `categoryId` valor a partir do valor de cadeia de caracteres de consulta passado para o *ProductList. aspx* página quando o *ProductList. aspx* página é para onde navegar. O `QueryStringAttribute` classe no `System.Web.ModelBinding` namespace é usado para recuperar o valor da id de variável de cadeia de caracteres de consulta. Isso instrui a associação de modelo para tentar associar um valor de cadeia de caracteres de consulta para o `categoryId` parâmetro em tempo de execução.

Quando uma categoria válida é passada como uma cadeia de caracteres de consulta para a página, os resultados da consulta são limitados a esses produtos no banco de dados que correspondem a `categoryId` valor. Por exemplo, se a URL para o *ProductsList.aspx* página é o seguinte:

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

A página exibe apenas os produtos onde o `category` é igual a `1`.

Se nenhuma cadeia de caracteres de consulta está incluída ao navegar para o *ProductList. aspx* página, todos os produtos serão exibidos.

As fontes de valores para esses métodos são chamadas de *valor provedores* (como *QueryString*), e os atributos de parâmetro que indicam qual provedor de valor para usar são chamados de valor atributos de provedor (como "`id`"). O ASP.NET inclui provedores de valor e os atributos correspondentes para todas as fontes típicas de entrada do usuário em um aplicativo de Web Forms, como a cadeia de caracteres de consulta, cookies, valores de formulário, controles, estado de exibição, estado de sessão e propriedades de perfil. Você também pode escrever provedores de valor personalizado.

### <a name="running-the-application"></a>Executando o aplicativo

Execute o aplicativo agora para ver como você pode exibir todos os produtos ou apenas um conjunto de produtos limitado por categoria.

1. No **Solution Explorer**, com o botão direito do *Default.aspx* página e selecione **exibir no navegador**.  
 O navegador será aberto e mostre o *Default.aspx* página.
2. Selecione **carros** no menu de navegação da categoria de produto.  
 O *ProductList. aspx* página é exibida, mostrando apenas os produtos incluídos na categoria "Carro". Posteriormente neste tutorial, você exibirá detalhes do produto.  

    ![Exibir dados de itens e detalhes - carros](display_data_items_and_details/_static/image2.png)
3. Selecione **produtos** no menu de navegação na parte superior.  
 Novamente, o *ProductList. aspx* página é exibida, mas desta vez, ele mostra a lista completa de produtos.   

    ![Exibir dados de itens e detalhes - produtos](display_data_items_and_details/_static/image3.png)
4. Feche o navegador e retorne ao Visual Studio.

### <a name="adding-a-data-control-to-display-product-details"></a>Adicionando um controle de dados para exibir detalhes do produto

Em seguida, você modificará a marcação no *ProductDetails.aspx* página que você adicionou no tutorial anterior, para que a página pode exibir informações sobre um produto individual.

1. Em **Solution Explorer**, abra o *ProductDetails.aspx* página.
2. Substitua a marcação existente com a seguinte marcação:   

    [!code-aspx[Main](display_data_items_and_details/samples/sample5.aspx)]

Esse código usa um **FormView** controle para exibir detalhes sobre um produto individual. Essa marcação usa métodos, como aqueles que são usados para exibir dados de *ProductList. aspx* página. O **FormView** controle é usado para exibir um único registro em vez de uma fonte de dados. Quando você usa o **FormView** controle, você cria modelos para exibir e editar valores de associação de dados. Os modelos contém controles, expressões de associação e a formatação que definem a aparência e a funcionalidade do formulário.

Para conectar-se a marcação acima para o banco de dados, você deve adicionar o código adicional para o *ProductDetails.aspx* código.

1. Em **Solution Explorer**, clique com botão direito *ProductDetails.aspx* e, em seguida, clique em **Exibir código**.  
 O *ProductDetails.aspx.cs* arquivo será exibido.
2. Substitua o código existente pelo seguinte código:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

Esse código verifica um "`productID`" valor de cadeia de caracteres de consulta. Se um valor de cadeia de consulta válido for encontrado, o produto será exibido. Se nenhuma cadeia de consulta é encontrada, ou o valor de cadeia de caracteres de consulta não é válido, nenhum produto é exibido no *ProductDetails.aspx* página.

### <a name="running-the-application"></a>Executando o aplicativo

Agora você pode executar o aplicativo para ver um produto individual exibido com base na id do produto.

1. Pressione **F5** enquanto estiver no Visual Studio para executar o aplicativo.  
 O navegador será aberto e mostre o *Default.aspx* página.
2. Selecione "Barcos" no menu de navegação de categoria.  
 O *ProductList. aspx* página é exibida.
3. Selecione o produto de "Documento barco" na lista de produtos.  
 O *ProductDetails.aspx* página é exibida.   

    ![Exibir dados de itens e detalhes - produtos](display_data_items_and_details/_static/image4.png)
4. Feche o navegador.

## <a name="summary"></a>Resumo

Neste tutorial da série de você ter adicionar marcação e código para exibir uma lista de produtos e exibir detalhes do produto. Durante esse processo, você aprendeu sobre controles de dados fortemente tipados, associação de modelo e provedores de valor. O seguinte tutorial, você adicionará um carrinho de compras para o aplicativo de exemplo Wingtip Toys.

## <a name="additional-resources"></a>Recursos adicionais

[Recuperando e exibindo dados com o modelo de associação e formulários da web](../../presenting-and-managing-data/model-binding/retrieving-data.md)

>[!div class="step-by-step"]
[Anterior](ui_and_navigation.md)
[Próximo](shopping-cart.md)
