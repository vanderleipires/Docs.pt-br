---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
title: Roteamento de URL | Microsoft Docs
author: Erikre
description: Esta série de tutoriais ensinará os conceitos básicos da criação de um aplicativo de Web Forms do ASP.NET usando o ASP.NET 4.5 e do Microsoft Visual Studio Express 2013 para nós...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 4f4bf092-c400-471f-a876-78fda0417890
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
msc.type: authoredcontent
ms.openlocfilehash: 556ef01304d0b5a3cca3606d71ef055ce4b2dc5c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389144"
---
<a name="url-routing"></a>Roteamento de URL
====================
por [Erik Reitan](https://github.com/Erikre)

[Baixe o projeto de exemplo do Wingtip Toys (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [Baixe o livro eletrônico (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Esta série de tutoriais ensinará os conceitos básicos da criação de um aplicativo de Web Forms do ASP.NET usando o ASP.NET 4.5 e do Microsoft Visual Studio Express 2013 para Web. Um Visual Studio 2013 [projeto com código-fonte c#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) está disponível para acompanhar esta série de tutoriais.


Neste tutorial, você modificará o aplicativo de exemplo Wingtip Toys para dar suporte ao roteamento de URL. O roteamento permite que seu aplicativo web para usar URLs que são mais fáceis de lembrar, amigável e melhor com suporte pelos mecanismos de pesquisa. Este tutorial se baseia no tutorial anterior "Associação e administração" e faz parte da série de tutoriais de Wingtip Toys.

## <a name="what-youll-learn"></a>O que você aprenderá:

- Como registrar as rotas para um aplicativo Web Forms do ASP.NET.
- Como adicionar rotas a uma página da web.
- Como selecionar dados de um banco de dados para dar suporte a rotas.

## <a name="aspnet-routing-overview"></a>Visão geral de roteamento do ASP.NET

Roteamento de URL permite que você configure um aplicativo para aceitar solicitações de URLs que não são mapeadas para arquivos físicos. Uma URL de solicitação é simplesmente a URL em que um usuário digita em seu navegador para encontrar uma página em seu site da web. Usar o roteamento para definir URLs que são semanticamente significativos para os usuários e que podem ajudar com a otimização do mecanismo de pesquisa (SEO).

Por padrão, o modelo de Web Forms inclui [URLs amigáveis do ASP.NET](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/). Grande parte do trabalho de roteamento básico será implementado usando *URLs amigáveis*. No entanto, neste tutorial, você adicionará recursos de roteamento personalizados.

Antes de personalizar o roteamento de URL, o aplicativo de exemplo Wingtip Toys pode vincular a um produto usando a seguinte URL:

`https://localhost:44300/ProductDetails.aspx?productID=2`

Personalizando o roteamento de URL, o aplicativo de exemplo Wingtip Toys vinculará para o mesmo produto usando um mais fácil de ler a URL:

`https://localhost:44300/Product/Convertible%20Car`

### <a name="routes"></a>Rotas

Uma rota é um padrão de URL que é mapeado para um manipulador. O manipulador pode ser um arquivo físico, como um arquivo. aspx em um aplicativo de formulários da Web. Um manipulador também pode ser uma classe que processa a solicitação. Para definir uma rota, você pode criar uma instância da classe Route, especificando o padrão de URL, o manipulador e, opcionalmente, um nome para a rota.

Adicionar a rota para o aplicativo adicionando o `Route` objeto estático `Routes` propriedade do `RouteTable` classe. A propriedade de rotas é uma `RouteCollection` objeto que armazena todas as rotas para o aplicativo.

### <a name="url-patterns"></a>Padrões de URL

Um padrão de URL pode conter valores literais e espaços reservados de variáveis (conhecidos como parâmetros de URL). Os literais e os espaços reservados estão localizados em segmentos da URL que são delimitados pela barra "/" (`/`) caracteres.

Quando é feita uma solicitação para seu aplicativo web, a URL é analisada em segmentos e os espaços reservados e os valores de variáveis são fornecidos para o manipulador de solicitação. Esse processo é semelhante à forma como os dados em uma cadeia de caracteres de consulta são analisados e passados para o manipulador de solicitação. Em ambos os casos, informações sobre a variável está incluído na URL e passado para o manipulador na forma de pares chave-valor. Para cadeias de caracteres de consulta, as chaves e os valores estão na URL. Para rotas, as chaves são os nomes de espaço reservado definidos no padrão de URL e apenas os valores estão na URL.

Em um padrão de URL, você deve definir espaços reservados, colocando-os entre chaves ( `{` e `}` ). Você pode definir mais de um espaço reservado em um segmento, mas os espaços reservados devem ser separados por um valor literal. Por exemplo, `{language}-{country}/{action}` é um padrão de rota válida. No entanto, `{language}{country}/{action}` não é um padrão válido, porque não há nenhum valor literal ou um delimitador entre os espaços reservados. Portanto, o roteamento não pode determinar onde separar o valor para o espaço reservado de idioma do valor para o espaço reservado de país.

### <a name="mapping-and-registering-routes"></a>Mapeamento e registrando as rotas

Para que possa incluir rotas às páginas do aplicativo de exemplo Wingtip Toys, você deve registrar as rotas quando o aplicativo é iniciado. Para registrar as rotas, você modificará o `Application_Start` manipulador de eventos.

1. Na **Gerenciador de soluções**do Visual Studio, localize e abra o *Global.asax.cs* arquivo.
2. Adicione o código realçado em amarelo para o *Global.asax.cs* arquivos da seguinte maneira:   

    [!code-csharp[Main](url-routing/samples/sample1.cs?highlight=30-31,34-46)]

Quando o aplicativo é iniciado de exemplo Wingtip Toys, ele chama o `Application_Start` manipulador de eventos. No final do manipulador de eventos, o `RegisterCustomRoutes` método é chamado. O `RegisterCustomRoutes` método adiciona cada rota chamando o `MapPageRoute` método o `RouteCollection` objeto. As rotas são definidas usando um nome de rota, uma URL da rota e uma URL física.

O primeiro parâmetro ("`ProductsByCategoryRoute`") é o nome da rota. Ele é usado para chamar a rota quando ela for necessária. O segundo parâmetro ("`Category/{categoryName}`") define a URL que pode ser dinâmico com base no código de substituição amigável. Use essa rota quando você estiver preenchendo um controle de dados com links que são gerados com base nos dados. Uma rota é mostrada da seguinte maneira:

[!code-csharp[Main](url-routing/samples/sample2.cs)]

O segundo parâmetro da rota inclui um valor dinâmico especificado por chaves (`{ }`). Nesse caso, o `categoryName` é uma variável que será usada para determinar o caminho de roteamento apropriado.

> [!NOTE] 
> 
> **Opcional**
> 
> Você talvez ache mais fácil de gerenciar seu código, movendo o `RegisterCustomRoutes` método para uma classe separada. No *lógica* pasta, crie uma `RouteActions` classe. Mover os itens acima `RegisterCustomRoutes` método a partir de *Global.asax.cs* arquivo para a nova `RoutesActions` classe. Use o `RoleActions` classe e o `createAdmin` método como um exemplo de como chamar o `RegisterCustomRoutes` método da *Global.asax.cs* arquivo.


Você talvez tenha observado também a `RegisterRoutes` chamada de método usando o `RouteConfig` objeto no início do `Application_Start` manipulador de eventos. Essa chamada é feita para implementar o roteamento padrão. Ele estava incluído como código padrão quando você criou o aplicativo usando o modelo de Web Forms do Visual Studio.

## <a name="retrieving-and-using-route-data"></a>Recuperar e usar dados de rota

Conforme mencionado acima, as rotas podem ser definidas. O código que você adicionou à `Application_Start` manipulador de eventos em de *Global.asax.cs* as rotas definidas pelo arquivo é carregado.

### <a name="setting-routes"></a>Configuração de rotas

Rotas exigem que você adicione código adicional. Neste tutorial, você usará a associação de modelo para recuperar um `RouteValueDictionary` objeto que é usado ao gerar as rotas usando dados de um controle de dados. O `RouteValueDictionary` objeto conterá uma lista de nomes de produtos que pertencem a uma categoria específica de produtos. Um link é criado para cada produto com base nos dados e rota.

#### <a name="enable-routes-for-categories-and-products"></a>Habilitar as rotas para categorias e produtos

Em seguida, você atualizará o aplicativo para usar o `ProductsByCategoryRoute` para determinar a rota correta para incluir para cada link de categoria de produto. Você também irá atualizar o *ProductList. aspx* página para incluir um link roteado para cada produto. Os links serão exibidos como estavam antes da alteração, no entanto, os links serão agora usam o roteamento de URL.

1. Na **Gerenciador de soluções**, abra o *Master* página se ela não ainda estiver aberta.
2. Atualizar o **ListView** controle denominado "`categoryList`" com as alterações destacadas em amarelo, portanto, a marcação aparece da seguinte maneira:   

    [!code-aspx[Main](url-routing/samples/sample3.aspx?highlight=7-9)]
3. Na **Gerenciador de soluções**, abra o *ProductList. aspx* página.
4. Atualizar o `ItemTemplate` elemento do *ProductList. aspx* página com as atualizações realçado em amarelo, portanto, a marcação aparece da seguinte maneira:   

    [!code-aspx[Main](url-routing/samples/sample4.aspx?highlight=6-9,14-16)]
5. Abra o code-behind da *ProductList.aspx.cs* e adicione o namespace a seguir como realçado em amarelo:  

    [!code-csharp[Main](url-routing/samples/sample5.cs?highlight=9)]
6. Substitua os `GetProducts` método de code-behind (*ProductList.aspx.cs*) com o código a seguir:   

    [!code-csharp[Main](url-routing/samples/sample6.cs)]

#### <a name="add-code-for-product-details"></a>Adicione código para obter detalhes do produto

Agora, atualize o code-behind (*ProductDetails.aspx.cs*) para o *ProductDetails.aspx* página use dados de rota. Observe que o novo `GetProduct` método também aceita um valor de cadeia de caracteres de consulta para o caso em que o usuário tem um link de um indicador que usa a URL não amigáveis e não-roteadas mais antiga.

1. Substitua os `GetProduct` método de code-behind (*ProductDetails.aspx.cs*) com o código a seguir:   

    [!code-csharp[Main](url-routing/samples/sample7.cs)]

## <a name="running-the-application"></a>Executando o aplicativo

Você pode executar o aplicativo agora para ver as rotas atualizadas.

1. Pressione **F5** para executar o aplicativo de exemplo Wingtip Toys.  
 O navegador é aberta e mostra a *default. aspx* página.
2. Clique o **produtos** link na parte superior da página.  
 Todos os produtos são exibidos na *ProductList. aspx* página. A URL a seguir (usando seu número de porta) é exibida para o navegador:  
    `https://localhost:44300/ProductList`
3. Em seguida, clique o **carros** categoria link próximo à parte superior da página.  
 Somente carros são exibidos na *ProductList. aspx* página. A URL a seguir (usando seu número de porta) é exibida para o navegador:  
    `https://localhost:44300/Category/Cars`
4. Clique no link que contém o nome do primeiro carro listado na página ("**conversível carro**") para exibir os detalhes do produto.  
 A URL a seguir (usando seu número de porta) é exibida para o navegador:  
    `https://localhost:44300/Product/Convertible%20Car`
5. Em seguida, insira a seguinte URL não roteado (usando seu número de porta) no navegador:  
    `https://localhost:44300/ProductDetails.aspx?productID=2`  
 O código ainda reconhece uma URL que inclui uma cadeia de caracteres de consulta, para o caso em que um usuário tem um link de um indicador.

## <a name="summary"></a>Resumo

Neste tutorial, você adicionou as rotas para categorias e produtos. Você aprendeu como rotas podem ser integradas com controles de dados que usam a associação de modelos. No próximo tutorial, você irá implementar o tratamento de erro global.

## <a name="additional-resources"></a>Recursos adicionais

[URLs amigáveis do ASP.NET](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/)  
[Implantar um aplicativo de formulários da Web do ASP.NET seguro com associação, OAuth e banco de dados SQL no serviço de aplicativo do Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure – avaliação gratuita](https://azure.microsoft.com/pricing/free-trial/)

> [!div class="step-by-step"]
> [Anterior](membership-and-administration.md)
> [Próximo](aspnet-error-handling.md)
