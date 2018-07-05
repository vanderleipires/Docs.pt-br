---
uid: mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
title: Implementar a paginação de dados eficiente | Microsoft Docs
author: microsoft
description: Etapa 8 mostra como adicionar suporte à paginação para nossa URL /Dinners para que, em vez de exibir 1000s de jantares ao mesmo tempo, vamos apenas exibir jantares próximos 10 em...
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: adea836d-dbc2-4005-94ea-53aef09e9e34
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
msc.type: authoredcontent
ms.openlocfilehash: bcd7fdf59fac8328752aa2ebab61c1d50a8b6b0d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37842137"
---
<a name="implement-efficient-data-paging"></a>Implementar a paginação eficiente de dados
====================
por [Microsoft](https://github.com/microsoft)

[Baixar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Esta é a etapa 8 de um livre [aplicativo "NerdDinner"](introducing-the-nerddinner-tutorial.md) que orienta-through como criar um pequeno, mas concluir, o aplicativo web usando ASP.NET MVC 1.
> 
> Etapa 8 mostra como adicionar suporte à paginação para nossa URL /Dinners para que, em vez de exibir 1000s de jantares ao mesmo tempo, vamos apenas exibir jantares próximos 10 por vez – e permitir que os usuários finais para frente por toda a lista de uma maneira amigável de SEO e página de volta.
> 
> Se você estiver usando o ASP.NET MVC 3, recomendamos que você siga a [obtendo iniciado com o MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [Store de música do MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriais.


## <a name="nerddinner-step-8-paging-support"></a>NerdDinner etapa 8: Suporte à paginação

Se nosso site for bem-sucedida, ela tem milhares de jantares futuros. É necessário certificar-se de que a nossa interface do usuário é dimensionado para lidar com todos esses jantares e permite que os usuários procurem-los. Para habilitar isso, vamos adicionar suporte a paginação a nossa */Dinners* URL para que em vez de exibir 1000s de jantares ao mesmo tempo, podemos apenas vai exibir jantares próximos 10 em um tempo - e permitir que os usuários finais para a página voltar e Avançar por toda a lista no uma maneira SEO amigável.

### <a name="index-action-method-recap"></a>Recapitulação de método de ação index)

O método de ação Index () dentro de nossa classe DinnersController atualmente se parece com abaixo:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample1.cs)]

Quando uma solicitação é feita para o */Dinners* URL, ele recupera uma lista de todos os futuros jantares e, em seguida, renderiza uma listagem de todos eles para fora:

![](implement-efficient-data-paging/_static/image1.png)

### <a name="understanding-iquerablelttgt"></a>Noções básicas sobre IQuerable&lt;T&gt;

*IQueryable&lt;T&gt;*  é uma interface que foi introduzida com o LINQ como parte do .NET 3.5. Ele possibilita cenários poderosos "execução adiada" que podemos pode aproveitar para implementar o suporte de paginação.

Em nosso DinnerRepository estamos retornando um IQueryable&lt;Dinner&gt; sequência a partir de nosso método FindUpcomingDinners():

[!code-csharp[Main](implement-efficient-data-paging/samples/sample2.cs)]

O IQueryable&lt;Dinner&gt; encapsula o objeto retornado pelo método nosso FindUpcomingDinners() uma consulta para recuperar objetos de jantar do nosso banco de dados usando LINQ to SQL. É importante, ele não execute a consulta no banco de dados até que podemos tentar acesso/iterar os dados na consulta ou podemos chamar o método de ToList() nele. O código de chamada de nosso método FindUpcomingDinners() pode optar por adicionar operações "encadeados" / filtros adicionais ao IQueryable&lt;Dinner&gt; objeto antes de executar a consulta. LINQ to SQL, em seguida, é inteligente o suficiente para executar a consulta combinada no banco de dados quando os dados são solicitados.

Para implementar a lógica de paginação podemos atualizar método de ação do nosso DinnersController Index () para que se aplica a outros operadores "Skip" e "Executar" ao IQueryable retornado&lt;Dinner&gt; sequência antes de chamar ToList():

[!code-csharp[Main](implement-efficient-data-paging/samples/sample3.cs)]

O código acima ignora os primeiros 10 jantares futuros no banco de dados e retorna os 20 jantares. LINQ to SQL é inteligente o suficiente para construir uma consulta otimizada do SQL que executa essa ignorando a lógica no banco de dados SQL – e não no servidor web. Isso significa que, mesmo se tivermos milhões de jantares futuros no banco de dados, somente os 10 que queremos serão recuperados como parte dessa solicitação (tornando eficientes e escalonáveis).

### <a name="adding-a-page-value-to-the-url"></a>Adicionando um valor de "página" à URL

Em vez de embutir um intervalo de páginas específicas, desejaremos nossas URLs para incluir um parâmetro de "página" que indica a qual um usuário está solicitando o intervalo jantar.

#### <a name="using-a-querystring-value"></a>Usando um valor de cadeia de consulta

O código a seguir demonstra como podemos atualizar nosso método de ação Index () para dar suporte a um parâmetro querystring e habilitar URLs, como */Dinners? página = 2*:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample4.cs)]

O método de ação Index () acima tem um parâmetro chamado "página". O parâmetro é declarado como um inteiro anulável (que é o que int? indica). Isso significa que o */Dinners? página = 2* URL fará com que um valor de "2" a ser passado como o valor do parâmetro. O */Dinners* URL (sem um valor de querystring) fará com que um valor nulo seja passado.

Podemos são multiplicar o valor da página pelo tamanho da página (nesse caso, 10 linhas) para determinar quantos jantares ignorar. Estamos usando o [c# "" operador de união nulo (?) ](https://weblogs.asp.net/scottgu/archive/2007/09/20/the-new-c-null-coalescing-operator-and-using-it-with-linq.aspx) que é útil ao lidar com tipos anuláveis. O código acima atribui o valor de 0 página se o parâmetro de página é nulo.

#### <a name="using-embedded-url-values"></a>Usando valores da URL inserida

Uma alternativa ao uso de um valor de querystring seria incorporar o parâmetro de página na URL real em si. Por exemplo: */Dinners/Page/2* ou *jantares/2*. O ASP.NET MVC inclui um poderoso mecanismo de roteamento de URL que torna mais fácil para dar suporte a cenários como esse.

Podemos pode registrar regras de roteamentos personalizadas que são mapeados qualquer URL de entrada ou a URL de formato para qualquer método de ação ou classe de controlador queremos. Tudo que precisamos de tarefas pendentes é abrir o arquivo global. asax em nosso projeto:

![](implement-efficient-data-paging/_static/image2.png)

E, em seguida, registre uma nova regra de mapeamento usando o método auxiliar de MapRoute() como a primeira chamada para rotas. MapRoute() abaixo:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample5.cs)]

Acima estamos registrando uma nova regra de roteamento denominada "UpcomingDinners". Podemos está indicando que ele tem o formato de URL "página jantares / / {página}" – em que {page} é um valor de parâmetro embutido na URL. O terceiro parâmetro para o método MapRoute() indica que estamos deve mapear as URLs que correspondam a este formato para o método de ação Index () na classe DinnersController.

Podemos usar o mesmo Index () código que tínhamos antes com o nosso cenário de Querystring – exceto agora nosso parâmetro de "página" virão a URL e não a querystring:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample6.cs)]

E agora quando executar o aplicativo e digite */Dinners* , veremos as 10 primeiras jantares futuros:

![](implement-efficient-data-paging/_static/image3.png)

E quando podemos digitar */Dinners/Page/1* , veremos que a próxima página de jantares:

![](implement-efficient-data-paging/_static/image4.png)

### <a name="adding-page-navigation-ui"></a>Adicionando navegação de página da interface do usuário

A última etapa para concluir o cenário de paginação será implementar "próxima" e "anterior" a navegação da interface do usuário dentro do nosso modelo de exibição para permitir aos usuários ignorar facilmente os dados de jantar.

Para implementar isso corretamente, precisaremos saber o número total de jantares no banco de dados, bem como muitas páginas de dados isso se traduz em. Em seguida, precisaremos calcular se o valor de "página" solicitada no momento é no início ou fim dos dados e mostrar ou ocultar a interface do usuário "anterior" e "próximo" adequadamente. Poderíamos implementar esta lógica dentro do nosso método de ação Index (). Como alternativa, podemos adicionar uma classe auxiliar para nosso projeto encapsulado por essa lógica de uma maneira mais reutilizável.

Abaixo está uma classe auxiliar "PaginatedList" simples que é derivada da lista&lt;T&gt; classe de coleção integrada ao .NET Framework. Ele implementa uma classe de coleção reutilizável que pode ser usada para paginar a qualquer sequência de dados IQueryable. Em nosso aplicativo NerdDinner, teremos ele trabalha em IQueryable&lt;Dinner&gt; resultados, mas ele poderia facilmente ser usado em IQueryable&lt;produto&gt; ou IQueryable&lt;cliente&gt;resulta em outros cenários de aplicativo:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample7.cs)]

Observe que acima como ela calcula e, em seguida, expõe propriedades, como "PageIndex", "PageSize", "TotalCount" e "TotalPages". Ele também expõe duas propriedades de auxiliar "HasPreviousPage" e "HasNextPage" que indicam se a página de dados na coleção é no início ou fim da sequência original. O código acima fará com que duas consultas SQL ser executado - o primeiro para recuperar a contagem do número total de objetos de jantar (isso não retorna os objetos – em vez disso, ele executa uma instrução "SELECT COUNT" que retorna um inteiro) e o segundo para recuperar apenas as linhas de dados que precisamos do nosso banco de dados para a página de dados atual.

Em seguida, atualizamos nosso método auxiliar de DinnersController.Index() para criar um PaginatedList&lt;Dinner&gt; do nosso DinnerRepository.FindUpcomingDinners() resultar e passá-lo para o nosso modelo de exibição:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample8.cs)]

Em seguida, podemos atualizar o modelo de exibição \Views\Dinners\Index.aspx herde ViewPage&lt;NerdDinner.Helpers.PaginatedList&lt;Dinner&gt; &gt; em vez de ViewPage&lt;IEnumerable&lt;Dinner&gt;&gt;e, em seguida, adicione o seguinte código à parte inferior de nosso modelo de exibição para mostrar ou ocultar a interface do usuário de navegação próxima e anterior:

[!code-aspx[Main](implement-efficient-data-paging/samples/sample9.aspx)]

Observe acima como estamos usando o método auxiliar Html.RouteLink() para gerar nosso hiperlinks. Esse método é semelhante ao método auxiliar Html.ActionLink() que usamos anteriormente. A diferença é que nós estão gerando a URL usando o "UpcomingDinners" nós configuramos dentro do nosso arquivo global asax a regra de roteamento. Isso garante que iremos gerar URLs para nosso método de ação Index () que têm o formato: */Dinners/página / {page}* – onde o valor de {page} é uma variável, estamos oferecendo acima com base em PageIndex atual.

E agora quando executamos nosso aplicativo novamente veremos 10 jantares por vez em nosso navegador:

![](implement-efficient-data-paging/_static/image5.png)

Também temos &lt; &lt; &lt; e &gt; &gt; &gt; da interface do usuário na parte inferior da página que nos permite ignorar a frente e para trás em relação ao nosso uso de dados de pesquisa URLs acessível do mecanismo de navegação:

![](implement-efficient-data-paging/_static/image6.png)

| **Tópico de lado: Noções básicas sobre as implicações de IQueryable&lt;T&gt;** |
| --- |
| IQueryable&lt;T&gt; é um recurso muito poderoso que permite uma variedade de cenários interessantes de execução adiada (como paginação e composição com base em consultas). Como com todos os recursos avançados, você deve ter cuidado com como usá-lo e verifique se que ele não foi violado. É importante reconhecer que retornar um IQueryable&lt;T&gt; resultado do seu repositório permite que o código de chamada acrescentar nos métodos de operador encadeada a ele e então participar na execução da consulta ultimate. Se você não deseja fornecer essa capacidade de código de chamada, você deve retornar IList de volta&lt;T&gt; ou IEnumerable&lt;T&gt; resultados – que contêm os resultados de uma consulta que já foi executada. Para cenários de paginação, isso exigiria que você enviar por push a lógica de paginação de dados reais para o método de repositório que está sendo chamado. Esse cenário pode atualizamos nosso método de localizador FindUpcomingDinners() para ter uma assinatura que seja retornado um PaginatedList: PaginatedList&lt; Dinner&gt; {FindUpcomingDinners (pageIndex int, int pageSize)} ou retornar um IList &lt;Dinner&gt;e use um "totalCount" out param para retornar a contagem total de jantares: IList&lt;Dinner&gt; FindUpcomingDinners (pageIndex int, int pageSize, out int totalCount) {} |

### <a name="next-step"></a>Próxima etapa

Agora vejamos como é possível adicionar suporte a autenticação e autorização para nosso aplicativo.

> [!div class="step-by-step"]
> [Anterior](re-use-ui-using-master-pages-and-partials.md)
> [Próximo](secure-applications-using-authentication-and-authorization.md)
