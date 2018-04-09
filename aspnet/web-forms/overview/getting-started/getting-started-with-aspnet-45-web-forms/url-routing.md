---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
title: Roteamento de URL | Microsoft Docs
author: Erikre
description: Esta série de tutorial irá ensiná-lo as Noções básicas de criação de um aplicativo de Web Forms do ASP.NET usando o ASP.NET 4.5 e o Microsoft Visual Studio Express 2013 para nós...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 4f4bf092-c400-471f-a876-78fda0417890
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
msc.type: authoredcontent
ms.openlocfilehash: a195b36517bcae4bbeaf43fe7386e7787fd00212
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="url-routing"></a>Roteamento de URL
====================
por [Erik Reitan](https://github.com/Erikre)

[Baixe o projeto de exemplo do Wingtip Toys (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [baixar livro eletrônico (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Esta série de tutorial irá ensiná-lo as Noções básicas de criação de um aplicativo de Web Forms do ASP.NET usando o ASP.NET 4.5 e o Microsoft Visual Studio Express 2013 para Web. Um Visual Studio 2013 [projeto com o código-fonte c#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) está disponível para acompanhar esta série de tutoriais.


Neste tutorial, você modificará o aplicativo de exemplo do Wingtip Toys para oferecer suporte ao roteamento de URL. Roteamento permite que seu aplicativo web para usar URLs que são mais fáceis de lembrar, melhor com suporte por mecanismos de pesquisa e amigável. Este tutorial se baseia no tutorial anterior "Associação e administração" e faz parte da série tutorial Wingtip Toys.

## <a name="what-youll-learn"></a>O que você aprenderá:

- Como registrar rotas para um aplicativo de Web Forms do ASP.NET.
- Como adicionar rotas a uma página da web.
- Como selecionar dados de um banco de dados para dar suporte a rotas.

## <a name="aspnet-routing-overview"></a>Visão geral sobre o roteamento do ASP.NET

Roteamento de URL permite que você configure um aplicativo para aceitar solicitações de URLs que não mapeiam para arquivos físicos. Uma URL de solicitação é simplesmente a URL de um usuário entra no seu navegador para localizar uma página no site da web. Use o roteamento para definir URLs que são semanticamente significativo para usuários e que podem ajudar com o mecanismo de pesquisa SEO (otimização).

Por padrão, o modelo de Web Forms inclui [URLs amigáveis do ASP.NET](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/). A maioria do trabalho roteamento básico será implementada usando *URLs amigáveis*. No entanto, neste tutorial, você irá adicionar recursos personalizados de roteamento.

Antes de personalizar o roteamento de URL, o aplicativo de exemplo Wingtip Toys pode vincular a um produto usando a seguinte URL:

`https://localhost:44300/ProductDetails.aspx?productID=2`

Personalizando o roteamento de URL, o aplicativo de exemplo Wingtip Toys vinculará o mesmo produto usando um mais fáceis de ler a URL:

`https://localhost:44300/Product/Convertible%20Car`

### <a name="routes"></a>Rotas

Uma rota é um padrão de URL que é mapeado para um manipulador. O identificador pode ser um arquivo físico, como um arquivo. aspx em um aplicativo de Web Forms. Um manipulador também pode ser uma classe que processa a solicitação. Para definir uma rota, você pode criar uma instância da classe de rota, especificando o padrão de URL, o manipulador e, opcionalmente, um nome para a rota.

Adicionar a rota para o aplicativo adicionando o `Route` objeto estático `Routes` propriedade o `RouteTable` classe. A propriedade de rotas é uma `RouteCollection` objeto que armazena todas as rotas para o aplicativo.

### <a name="url-patterns"></a>Padrões de URL

Um padrão de URL pode conter valores literais e os espaços reservados de variável (conhecidos como parâmetros de URL). Os literais e os espaços reservados estão localizados em segmentos da URL que são delimitados por barra (`/`) caracteres.

Quando é feita uma solicitação para o aplicativo da web, a URL é analisada em segmentos e espaços reservados e os valores das variáveis são fornecidos para o manipulador de solicitações. Esse processo é semelhante à forma como os dados em uma cadeia de caracteres de consulta são analisados e passados para o manipulador de solicitação. Em ambos os casos, informações de variáveis são incluídas na URL e passadas para o manipulador na forma de pares chave-valor. Cadeias de caracteres de consulta, as chaves e os valores são na URL. Para rotas, as chaves são os nomes de espaço reservado definidos no padrão de URL e apenas os valores estão na URL.

Em um padrão de URL, você deve definir espaços reservados, colocando-os entre chaves ( `{` e `}` ). Você pode definir mais de um espaço reservado em um segmento, mas os espaços reservados que devem ser separados por um valor literal. Por exemplo, `{language}-{country}/{action}` é um padrão de uma rota válida. No entanto, `{language}{country}/{action}` não é um padrão válido, porque não há nenhum valor literal ou o delimitador entre os espaços reservados. Portanto, o roteamento não pode determinar onde separar o valor para o espaço reservado de idioma do valor do espaço reservado de país.

### <a name="mapping-and-registering-routes"></a>Mapeamento e registrando rotas

Antes de você pode incluir rotas para páginas do aplicativo de exemplo Wingtip Toys, você deve registrar as rotas quando o aplicativo for iniciado. Para registrar as rotas, você modificará o `Application_Start` manipulador de eventos.

1. Em **Solution Explorer**do Visual Studio, localize e abra a *Global.asax.cs* arquivo.
2. Adicione o código realçado em amarelo para o *Global.asax.cs* arquivos da seguinte maneira:   

    [!code-csharp[Main](url-routing/samples/sample1.cs?highlight=30-31,34-46)]

Quando inicia do aplicativo de exemplo Wingtip Toys, ele chama o `Application_Start` manipulador de eventos. No final do manipulador de eventos, o `RegisterCustomRoutes` método é chamado. O `RegisterCustomRoutes` método adiciona cada rota chamando o `MapPageRoute` método o `RouteCollection` objeto. Rotas são definidas usando um nome de rota, uma URL da rota e uma URL física.

O primeiro parâmetro ("`ProductsByCategoryRoute`") é o nome da rota. Ele é usado para chamar a rota quando ela for necessária. O segundo parâmetro ("`Category/{categoryName}`") define a URL que pode ser dinâmica com base no código de substituição amigável. Você usa essa rota quando você estiver preenchendo um controle de dados com links que são gerados com base nos dados. Uma rota é mostrada a seguir:

[!code-csharp[Main](url-routing/samples/sample2.cs)]

O segundo parâmetro da rota inclui um valor dinâmico especificado por chaves (`{ }`). Nesse caso, o `categoryName` é uma variável que será usada para determinar o caminho de roteamento adequado.

> [!NOTE] 
> 
> **Opcional**
> 
> Talvez seja mais fácil de gerenciar seu código, movendo o `RegisterCustomRoutes` método para uma classe separada. No *lógica* pasta, crie um separado `RouteActions` classe. Mover acima `RegisterCustomRoutes` método a partir de *Global.asax.cs* arquivo para a nova `RoutesActions` classe. Use o `RoleActions` classe e o `createAdmin` método como um exemplo de como chamar o `RegisterCustomRoutes` método do *Global.asax.cs* arquivo.


Você também deve ter notado o `RegisterRoutes` chamada de método usando o `RouteConfig` objeto no início do `Application_Start` manipulador de eventos. Esta chamada é feita para implementar roteamento padrão. Foi incluída como código padrão quando você criou o aplicativo usando o modelo de formulários da Web do Visual Studio.

## <a name="retrieving-and-using-route-data"></a>Recuperar e usar dados de rota

Conforme mencionado acima, as rotas podem ser definidas. O código que você adicionou ao `Application_Start` manipulador de eventos de *Global.asax.cs* arquivo carrega as rotas definidas pelo.

### <a name="setting-routes"></a>Configuração de rotas

Rotas exigem que você adicione código adicional. Neste tutorial, você usará a associação de modelo para recuperar um `RouteValueDictionary` objeto que é usado ao gerar as rotas usando dados de um controle de dados. O `RouteValueDictionary` objeto conterá uma lista de nomes de produtos que pertencem a uma categoria específica de produtos. Um link é criado para cada produto com base nos dados de rota.

#### <a name="enable-routes-for-categories-and-products"></a>Habilitar rotas para categorias e produtos

Em seguida, você atualizará o aplicativo para usar o `ProductsByCategoryRoute` para determinar a rota correta para incluir para cada link de categoria de produto. Você também deverá atualizar o *ProductList. aspx* página para incluir um link roteado para cada produto. Os links serão exibidos como estavam antes da alteração, no entanto, os links usará o roteamento de URL.

1. Em **Solution Explorer**, abra o *Site.Master* página se ainda não estiver aberto.
2. Atualização de **ListView** controle chamado "`categoryList`" com as alterações destacadas em amarelo, portanto a marcação aparece da seguinte maneira:   

    [!code-aspx[Main](url-routing/samples/sample3.aspx?highlight=7-9)]
3. Em **Solution Explorer**, abra o *ProductList. aspx* página.
4. Atualização de `ItemTemplate` elemento o *ProductList. aspx* página com as atualizações realçados em amarelo, então a marcação aparece da seguinte maneira:   

    [!code-aspx[Main](url-routing/samples/sample4.aspx?highlight=6-9,14-16)]
5. Abra o code-behind de *ProductList.aspx.cs* e adicione o seguinte namespace como realçada em amarelo:  

    [!code-csharp[Main](url-routing/samples/sample5.cs?highlight=9)]
6. Substitua o `GetProducts` método de code-behind (*ProductList.aspx.cs*) com o código a seguir:   

    [!code-csharp[Main](url-routing/samples/sample6.cs)]

#### <a name="add-code-for-product-details"></a>Adicione código para obter detalhes do produto

Agora, atualize o code-behind (*ProductDetails.aspx.cs*) para o *ProductDetails.aspx* página para usar dados de rota. Observe que o novo `GetProduct` método também aceita um valor de cadeia de caracteres de consulta para o caso em que o usuário tem um link de indicador que usa a URL amigável não, não roteado mais antiga.

1. Substitua o `GetProduct` método de code-behind (*ProductDetails.aspx.cs*) com o código a seguir:   

    [!code-csharp[Main](url-routing/samples/sample7.cs)]

## <a name="running-the-application"></a>Executando o aplicativo

Você pode executar o aplicativo agora para ver as rotas atualizadas.

1. Pressione **F5** para executar o aplicativo de exemplo Wingtip Toys.  
 O navegador é aberto e mostra o *Default.aspx* página.
2. Clique o **produtos** link na parte superior da página.  
 Todos os produtos são exibidos na *ProductList. aspx* página. A URL a seguir (usando o seu número de porta) é exibida para o navegador:  
    `https://localhost:44300/ProductList`
3. Em seguida, clique o **carros** link de categoria na parte superior da página.  
 Somente carros são exibidos na *ProductList. aspx* página. A URL a seguir (usando o seu número de porta) é exibida para o navegador:  
    `https://localhost:44300/Category/Cars`
4. Clique no link que contém o nome do primeiro carro listado na página ("**conversíveis carro**") para exibir os detalhes do produto.  
 A URL a seguir (usando o seu número de porta) é exibida para o navegador:  
    `https://localhost:44300/Product/Convertible%20Car`
5. Em seguida, insira a seguinte URL não roteado (usando o seu número de porta) no navegador:  
    `https://localhost:44300/ProductDetails.aspx?productID=2`  
 O código reconhece uma URL que inclui uma cadeia de caracteres de consulta, para o caso em que um usuário tem um link de indicador.

## <a name="summary"></a>Resumo

Neste tutorial, você adicionou rotas para categorias e produtos. Você aprendeu como rotas podem ser integradas com controles de dados que usam a associação de modelo. O seguinte tutorial, você implementará o tratamento de erro global.

## <a name="additional-resources"></a>Recursos adicionais

[URLs amigáveis do ASP.NET](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/)  
[Implantar um aplicativo de formulários da Web do ASP.NET seguro com associação, OAuth e o banco de dados SQL para serviço de aplicativo do Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure - versão de avaliação gratuita](https://azure.microsoft.com/pricing/free-trial/)

> [!div class="step-by-step"]
> [Anterior](membership-and-administration.md)
> [Próximo](aspnet-error-handling.md)
