---
uid: mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
title: Implementar a paginação de dados eficiente | Microsoft Docs
author: microsoft
description: Etapa 8 mostra como adicionar suporte à paginação para nossa URL /Dinners para que, em vez de exibir 1000s de jantares ao mesmo tempo, exibiremos somente 10 jantares futuros em...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: adea836d-dbc2-4005-94ea-53aef09e9e34
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
msc.type: authoredcontent
ms.openlocfilehash: 0188e21438820adf2adbe05b047fdb772540e1a0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873242"
---
<a name="implement-efficient-data-paging"></a>Implementar a paginação de dados eficiente
====================
por [Microsoft](https://github.com/microsoft)

[Baixar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Esta é a etapa 8 de livremente [aplicativo "NerdDinner"](introducing-the-nerddinner-tutorial.md) que aborda-por meio de como criar um pequeno, mas concluir, o aplicativo web usando o ASP.NET MVC 1.
> 
> Etapa 8 mostra como adicionar suporte à paginação para nossa URL /Dinners para que, em vez de exibir 1000s de jantares uma vez, vamos apenas exibir 10 jantares futuros por vez - e permitir que os usuários finais a página de volta e encaminhá-los por toda a lista de uma maneira SEO amigável.
> 
> Se você estiver usando o ASP.NET MVC 3, recomendamos que você siga o [obtendo iniciado com MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [repositório de música MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriais.


## <a name="nerddinner-step-8-paging-support"></a>NerdDinner etapa 8: Suporte à paginação

Se nosso site for bem-sucedida, ela tem milhares de jantares futuros. É preciso certificar-se de que a nossa interface do usuário pode ser dimensionado para tratar todas essas jantares e permite que os usuários procurem-los. Para habilitar isso, vamos adicionar suporte a paginação a nossa */Dinners* URL para que em vez de exibir 1000s de jantares em uma vez, é somente vai exibir 10 jantares futuros por vez - e permitir que os usuários finais a página back e encaminhá-los por toda a lista de uma maneira SEO amigável.

### <a name="index-action-method-recap"></a>Resumo de método de ação index)

O método de ação Index () dentro de nossa classe DinnersController atualmente parece abaixo:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample1.cs)]

Quando uma solicitação é feita para o */Dinners* URL, ele recupera uma lista de todos os futuros jantares e, em seguida, apresenta uma lista de todos-los:

![](implement-efficient-data-paging/_static/image1.png)

### <a name="understanding-iquerablelttgt"></a>Noções básicas sobre IQuerable&lt;T&gt;

*IQueryable&lt;T&gt;*  é uma interface que foi introduzida com o LINQ como parte do .NET 3.5. Ele permite cenários avançados "execução adiada" que podemos pode aproveitar para implementar o suporte à paginação.

Em nosso DinnerRepository estamos retornando um IQueryable&lt;refeição&gt; sequência do nosso método FindUpcomingDinners():

[!code-csharp[Main](implement-efficient-data-paging/samples/sample2.cs)]

O IQueryable&lt;refeição&gt; objeto retornado pelo método nosso FindUpcomingDinners() encapsula uma consulta para recuperar objetos de uma refeição de nosso banco de dados usando LINQ to SQL. É importante, ele não executar a consulta no banco de dados até que podemos tentar acesso/iterar os dados na consulta, ou podemos chamar o método ToList() nele. O código de chamada nosso método FindUpcomingDinners() pode optar por adicionar mais "cadeia" operações de filtros ao IQueryable&lt;refeição&gt; objeto antes de executar a consulta. O LINQ to SQL, em seguida, é inteligente o suficiente para executar a consulta combinada com o banco de dados quando os dados são solicitados.

Para implementar a lógica de paginação podemos atualizar método de ação do nosso DinnersController Index () para que ele aplica operadores adicionais "Ignorar" e "Usa" ao IQueryable retornado&lt;refeição&gt; sequência antes de chamar ToList():

[!code-csharp[Main](implement-efficient-data-paging/samples/sample3.cs)]

O código acima ignora os primeiros 10 jantares futuras no banco de dados e retorna 20 jantares. O LINQ to SQL é inteligente para construir uma consulta otimizada do SQL que realiza isso ignorando lógica no banco de dados SQL – e não no servidor web. Isso significa que mesmo que temos milhões de jantares futuras no banco de dados, apenas os 10 que queremos serão recuperados como parte dessa solicitação (fazendo eficiente e escalonável).

### <a name="adding-a-page-value-to-the-url"></a>Adicionando um valor de "página" à URL

Em vez de codificar um intervalo de página específica, será queremos que nossas URLs para incluir um parâmetro de "página" que indica qual um usuário está solicitando uma refeição o intervalo.

#### <a name="using-a-querystring-value"></a>Usando um valor de Querystring

O código a seguir demonstra como podemos atualizar nosso método de ação Index () para oferecer suporte a um parâmetro querystring e habilitar URLs como */Dinners? página = 2*:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample4.cs)]

O método de ação Index () acima tiver um parâmetro denominado "página". O parâmetro seja declarado como um inteiro anulável (que é o que int? indica). Isso significa que o */Dinners? página = 2* URL fará com que um valor de "2" a ser passado como o valor do parâmetro. O */Dinners* URL (sem um valor de querystring) fará com que um valor nulo a serem passados.

Nós são multiplicar o valor de página pelo tamanho da página (nesse caso, 10 linhas) para determinar quantos jantares ignorar. Estamos usando o [c# nulo "" operador de união (?) ](https://weblogs.asp.net/scottgu/archive/2007/09/20/the-new-c-null-coalescing-operator-and-using-it-with-linq.aspx) que é útil ao lidar com tipos anuláveis. O código acima atribui o valor de 0 página se o parâmetro de página é nulo.

#### <a name="using-embedded-url-values"></a>Usando valores de URL inseridos

Uma alternativa ao uso de um valor de querystring seria inserir o parâmetro de página dentro da própria URL. Por exemplo: */Dinners/Page/2* ou *jantares/2*. O ASP.NET MVC inclui um poderoso mecanismo de roteamento de URL que torna mais fácil dar suporte a cenários como esse.

Podemos pode registrar regras personalizadas de roteamentos que qualquer entrada URL ou formatar a qualquer método de classe ou uma ação de controlador queremos são mapeados. Tudo o que precisamos fazer é abrir o arquivo global. asax em nosso projeto:

![](implement-efficient-data-paging/_static/image2.png)

E, em seguida, registre uma nova regra de mapeamento usando o método auxiliar de MapRoute() como a primeira chamada para rotas. MapRoute() abaixo:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample5.cs)]

Acima, podemos estiver registrando uma nova regra de roteamento denominada "UpcomingDinners". Nós são indicando que ela tem o formato de URL "página jantares / / {página}" – onde {page} é incorporado na URL de um valor de parâmetro. O terceiro parâmetro para o método MapRoute() indica que vamos deve mapear as URLs que correspondem a esse formato para o método de ação Index () na classe DinnersController.

Podemos usar exatamente o mesmo Index () código que tivemos antes com nosso cenário de Querystring – exceto agora nosso parâmetro de "página" virão a URL e não querystring:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample6.cs)]

E agora quando executar o aplicativo e digite */Dinners* veremos os 10 primeiros jantares futuros:

![](implement-efficient-data-paging/_static/image3.png)

E quando podemos digitar */Dinners/Page/1* veremos a próxima página de jantares:

![](implement-efficient-data-paging/_static/image4.png)

### <a name="adding-page-navigation-ui"></a>Adicionando navegação de página da interface do usuário

A última etapa para concluir o cenário de paginação seria implementar o "próxima" e "anterior" a navegação da interface do usuário em nosso modelo de exibição para permitir que os usuários facilmente ignorar os dados de uma refeição.

Para implementar isso corretamente, precisaremos saber o número total de jantares no banco de dados, bem como o número de páginas de dados isso se traduz em. Em seguida, precisaremos calcular se o valor de "página" solicitada no momento está no início ou no final dos dados e mostrar ou ocultar a interface do usuário "anterior" e "próximo" adequadamente. Poderíamos implementar essa lógica em nosso método de ação Index (). Como alternativa, pode adicionar uma classe auxiliar ao nosso projeto que encapsula a lógica de uma maneira mais reutilizável.

Abaixo está uma classe auxiliar "PaginatedList" simples que deriva da lista&lt;T&gt; classe de coleção integrado ao .NET Framework. Ele implementa uma classe de coleção reutilizável que pode ser usada para qualquer sequência de dados IQueryable de paginação. Em nosso aplicativo NerdDinner teremos ele trabalha em IQueryable&lt;refeição&gt; resultados, mas foi tão facilmente ser usada em IQueryable&lt;produto&gt; ou IQueryable&lt;cliente&gt;resulta em outros cenários de aplicativo:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample7.cs)]

Observe acima como ela calcula e, em seguida, expõe propriedades como "PageIndex", "PageSize", "TotalCount" e "TotalPages". Ele também expõe duas propriedades de auxiliar "HasPreviousPage" e "HasNextPage" que indicam se a página de dados na coleção no início ou no final da sequência de original. O código acima fará com que duas consultas SQL ser executado - o primeiro para recuperar a contagem do número total de objetos de uma refeição (isso não retorna os objetos – em vez disso, ele executa uma instrução "Selecione contagem" que retorna um número inteiro) e a segunda para recuperar apenas as linhas de dados que necessários de nosso banco de dados para a página de dados atual.

Em seguida, atualizamos nosso método auxiliar de DinnersController.Index() para criar um PaginatedList&lt;refeição&gt; do nosso DinnerRepository.FindUpcomingDinners() resultar e passá-lo ao nosso modelo de exibição:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample8.cs)]

Em seguida, podemos atualizar o modelo de exibição \Views\Dinners\Index.aspx herde ViewPage&lt;NerdDinner.Helpers.PaginatedList&lt;refeição&gt; &gt; em vez de ViewPage&lt;IEnumerable&lt;Uma refeição&gt;&gt;e, em seguida, adicione o seguinte código à parte inferior do nosso modelo de exibição para mostrar ou ocultar navegação próxima e anterior da interface do usuário:

[!code-aspx[Main](implement-efficient-data-paging/samples/sample9.aspx)]

Observe acima como estamos usando o método auxiliar Html.RouteLink() para gerar os hiperlinks. Esse método é semelhante ao método auxiliar Html.ActionLink() que usamos anteriormente. A diferença é que podemos são gerar a URL usando o "UpcomingDinners" roteamento regra que nós configuramos em nosso arquivo global. asax. Isso garante que estamos irá gerar URLs para nosso método de ação Index () que têm o formato: */Dinners/página / {page}* – o valor de {page} é uma variável que estamos fornecendo acima com base em PageIndex atual.

E agora quando executamos nosso aplicativo novamente veremos 10 jantares por vez em nosso navegador:

![](implement-efficient-data-paging/_static/image5.png)

Também temos &lt; &lt; &lt; e &gt; &gt; &gt; interface na parte inferior da página que nos permite vá em frente e para trás sobre nossos dados usando Pesquisar URLs acessível do mecanismo de navegação:

![](implement-efficient-data-paging/_static/image6.png)

| **Tópico de lado: Noções básicas sobre as implicações de IQueryable&lt;T&gt;** |
| --- |
| IQueryable&lt;T&gt; é um recurso muito poderoso que permite uma variedade de cenários interessantes de execução adiada (como paginação e composição com base em consultas). Como com todos os recursos avançados, você deseja ter cuidado com como usá-lo e verifique se que ele não é seja explorado. É importante reconhecer que um IQueryable retornando&lt;T&gt; resultados do seu repositório permite que o código de chamada acrescentar em métodos de operador encadeado a ele e então participam da execução de consulta final. Se você não deseja fornecer código de chamada essa capacidade, você deve retornar fazer IList&lt;T&gt; ou IEnumerable&lt;T&gt; resultados - que contém os resultados de uma consulta que já foi executado. Para cenários de paginação isso exigiria que você enviar por push a lógica de paginação de dados reais para o método de repositório que está sendo chamado. Neste cenário, poderá atualizar nosso método do localizador FindUpcomingDinners() para ter uma assinatura que seja retornado um PaginatedList: PaginatedList&lt; refeição&gt; {FindUpcomingDinners (pageIndex int, int pageSize)} ou retorno IList &lt;Refeição&gt;e use um "totalCount" out param para retornar a contagem total de jantares: IList&lt;refeição&gt; FindUpcomingDinners (pageIndex int, int pageSize, out int totalCount) {} |

### <a name="next-step"></a>Próxima etapa

Agora vamos examinar como é possível adicionar suporte a autenticação e autorização para nosso aplicativo.

> [!div class="step-by-step"]
> [Anterior](re-use-ui-using-master-pages-and-partials.md)
> [Próximo](secure-applications-using-authentication-and-authorization.md)
