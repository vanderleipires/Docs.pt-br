---
uid: web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-cs
title: Armazenar em cache dados na arquitetura (c#) | Microsoft Docs
author: rick-anderson
description: No tutorial anterior, aprendemos como aplicar o cache na camada de apresentação. Neste tutorial, saiba como tirar proveito do nosso architectu em camadas...
ms.author: riande
ms.date: 05/30/2007
ms.assetid: d29a7c41-0628-4a23-9dfc-bfea9c6c1054
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-cs
msc.type: authoredcontent
ms.openlocfilehash: 3971140aa7a6c829287e74df804694c19e34adcf
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830420"
---
<a name="caching-data-in-the-architecture-c"></a>Armazenar dados em cache na arquitetura (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_59_CS.exe) ou [baixar PDF](caching-data-in-the-architecture-cs/_static/datatutorial59cs1.pdf)

> No tutorial anterior, aprendemos como aplicar o cache na camada de apresentação. Neste tutorial, saiba como tirar proveito da nossa arquitetura em camadas em cache os dados na camada de lógica de negócios. Podemos fazer isso, estendendo a arquitetura para incluir uma camada de armazenamento em cache.


## <a name="introduction"></a>Introdução

Como vimos no tutorial anterior, o armazenamento em cache os dados de s ObjectDataSource é tão simple quanto definir algumas propriedades. Infelizmente, o ObjectDataSource aplica-se na camada de apresentação, que acople estritamente as políticas de cache com a página ASP.NET de cache. Um dos motivos para a criação de uma arquitetura em camadas é permitir que tal acoplamentos de maneira a ser interrompido. A camada de lógica de negócios, por exemplo, separa a lógica de negócios em páginas ASP.NET, enquanto a camada de acesso a dados separa os detalhes de acesso de dados. Essa desassociação de detalhes de acesso de lógica e os dados de negócios é preferido, em parte, pois ele torna o sistema mais sustentável, mais legível e mais flexível para alterar. Ele também permite o conhecimento de domínio e de divisão de trabalho, um desenvolvedor que trabalha em t camada de apresentação precisa estar familiarizado com os detalhes de s de banco de dados para fazer seu trabalho. Desacoplando a política de cache da camada de apresentação oferece benefícios semelhantes.

Neste tutorial, estamos será aumentar nossa arquitetura para incluir um *camada de armazenamento em cache* (ou CL de forma abreviada) que emprega nossa política de cache. A camada de cache incluirá uma `ProductsCL` classe que fornece acesso às informações de produto com métodos como `GetProducts()`, `GetProductsByCategoryID(categoryID)`e assim por diante, que, quando chamado, será a primeira tentativa de recuperar os dados do cache. Se o cache estiver vazio, esses métodos invocará apropriado `ProductsBLL` método na BLL, que por sua vez deve obter os dados da DAL. O `ProductsCL` métodos armazenar em cache os dados recuperados da BLL antes de retorná-lo.

Como mostra a Figura 1, o CL reside entre a apresentação e as camadas de lógica comercial.


![O cache de camada (CL) é outra camada na arquitetura nosso](caching-data-in-the-architecture-cs/_static/image1.png)

**Figura 1**: O cache de camada (CL) é outra camada na arquitetura nosso


## <a name="step-1-creating-the-caching-layer-classes"></a>Etapa 1: Criando as Classes de camada de cache

Neste tutorial, vamos criar uma CL muito simple com uma única classe `ProductsCL` que tem apenas um punhado de métodos. Criando uma camada de cache completo para o aplicativo inteiro exigiria a criação `CategoriesCL`, `EmployeesCL`, e `SuppliersCL` classes e fornecendo um método nessas classes de camada de armazenamento em cache para cada método de acesso ou modificação de dados na BLL. Assim como acontece com a BLL e DAL, a camada de armazenamento em cache o ideal é que deve ser implementada como um projeto de biblioteca de classes separado; No entanto, podemos irão implementá-lo como uma classe no `App_Code` pasta.

Para mais classes separadas corretamente o CL das classes DAL e BLL, let s criar uma nova subpasta no `App_Code` pasta. Clique com botão direito no `App_Code` pasta no Gerenciador de soluções, escolha a nova pasta e nomeie a nova pasta `CL`. Depois de criar essa pasta, adicione a ela uma nova classe chamada `ProductsCL.cs`.


![Adicionar uma nova pasta chamada CL e uma classe chamada ProductsCL.cs](caching-data-in-the-architecture-cs/_static/image2.png)

**Figura 2**: adicionar uma nova pasta chamada `CL` e uma classe chamada `ProductsCL.cs`


O `ProductsCL` classe deve incluir o mesmo conjunto de métodos de acesso e modificação de dados que se encontra na sua classe de camada de lógica comercial correspondente (`ProductsBLL`). Em vez de criar todos esses métodos, let s apenas crie algumas aqui para obter uma ideia para os padrões usados por CL. Em particular, vamos adicionar o `GetProducts()` e `GetProductsByCategoryID(categoryID)` métodos na etapa 3 e um `UpdateProduct` sobrecarga na etapa 4. Você pode adicionar o restante `ProductsCL` métodos e `CategoriesCL`, `EmployeesCL`, e `SuppliersCL` classes em seu tempo livre.

## <a name="step-2-reading-and-writing-to-the-data-cache"></a>Etapa 2: Lendo e gravando no Cache de dados

O ObjectDataSource explorado internamente no tutorial anterior de recurso de cache usa o cache de dados do ASP.NET para armazenar os dados recuperados da BLL. O cache de dados também pode ser acessado por meio de programação de classes de code-behind de páginas ASP.NET ou de classes em arquitetura de s do aplicativo web. Para ler e gravar no cache de dados de uma classe de code-behind de s de página ASP.NET, use o seguinte padrão:


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample1.cs)]

O [ `Cache` classe](https://msdn.microsoft.com/library/system.web.caching.cache.aspx) s [ `Insert` método](https://msdn.microsoft.com/library/system.web.caching.cache.insert.aspx) tem várias sobrecargas. `Cache["key"] = value` e `Cache.Insert(key, value)` são sinônimos e ambos adicionam um item ao cache usando a chave especificada sem um vencimento definido. Normalmente, queremos especificar uma expiração, ao adicionar um item ao cache, como uma dependência, uma expiração com base no tempo ou ambos. Use um dos outros `Insert` sobrecargas do método s para fornecer informações baseadas em dependência ou em tempo de expiração.

A camada de armazenamento em cache métodos s precisam primeiro, verifique se os dados solicitados estiverem no cache e, nesse caso, retorná-lo a partir daí. Se os dados solicitados não estiverem no cache, o método apropriado de BLL precisa ser invocado. Seu valor de retorno deve ser armazenado em cache e, em seguida, retornado, conforme ilustra o diagrama a seguir.


![Os camada de armazenamento em cache s métodos retornaram dados do Cache se ele s disponível](caching-data-in-the-architecture-cs/_static/image3.png)

**Figura 3**: A camada de armazenamento em cache s métodos retornaram dados do Cache se ele s disponível


A sequência descrita na Figura 3 é realizada nas classes CL usando o seguinte padrão:


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample2.cs)]

Aqui, *tipo* é o tipo de dados armazenados no cache `Northwind.ProductsDataTable`, por exemplo, *chave* é a chave que identifica exclusivamente o item de cache. Se o item com a especificada *chave* não está no cache, em seguida, *instância* será `null` e os dados serão recuperados do método BLL apropriado e adicionados ao cache. No momento `return instance` for atingido, *instância* contém uma referência para os dados do cache ou extraída da BLL.

Certifique-se de usar o padrão acima ao acessar dados do cache. O padrão a seguir, que, à primeira vista, parece equivalente, contém uma diferença sutil que apresenta uma condição de corrida. Condições de corrida são difíceis de depurar porque elas se revelam esporadicamente e são difíceis de reproduzir.


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample3.cs)]

A diferença neste segundo, o trecho de código incorreto é que, em vez de armazenar uma referência para o item em cache em uma variável local, o cache de dados é acessado diretamente na instrução condicional *e* no `return`. Imagine que, quando esse código é atingido, `Cache["key"]` não é`null`, mas antes o `return` instrução for atingida, o sistema remove *chave* do cache. Nesse caso raro, o código retornará um `null` valor em vez de um objeto do tipo esperado.

> [!NOTE]
> O cache de dados é thread-safe, para que você não precisa para sincronizar o acesso de thread para simples leituras ou gravações. No entanto, se você precisar executar várias operações em dados no cache que precisam ser atômicas, você é responsável por implementar um bloqueio ou algum outro mecanismo para garantir acesso thread-safe. Ver [sincronizando acesso ao Cache ASP.NET](http://www.ddj.com/184406369) para obter mais informações.


Um item por meio de programação pode ser removido do cache de dados usando o [ `Remove` método](https://msdn.microsoft.com/library/system.web.caching.cache.remove.aspx) desta forma:


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample4.cs)]

## <a name="step-3-returning-product-information-from-theproductsclclass"></a>Etapa 3: Retornando informações de produto a`ProductsCL`classe

Para este tutorial deixe s implementar dois métodos para retornar informações de produto a `ProductsCL` classe: `GetProducts()` e `GetProductsByCategoryID(categoryID)`. Como com o `ProductsBL` classe na camada de lógica de negócios, o `GetProducts()` método na CL retorna informações sobre todos os produtos como um `Northwind.ProductsDataTable` objeto, enquanto `GetProductsByCategoryID(categoryID)` retorna todos os produtos de uma categoria especificada.

O código a seguir mostra uma parte dos métodos a `ProductsCL` classe:


[!code-vb[Main](caching-data-in-the-architecture-cs/samples/sample5.vb)]

Primeiro, observe os `DataObject` e `DataObjectMethodAttribute` atributos aplicados a métodos e classe. Esses atributos fornecem informações para o assistente ObjectDataSource s, que indica o que as classes e métodos devem aparecer nas etapas do Assistente de s. Uma vez que os métodos e classes de CL serão acessados a partir um ObjectDataSource na camada de apresentação, eu adicionei esses atributos para aprimorar a experiência de tempo de design. Voltar para o [criando uma camada de lógica de negócios](../introduction/creating-a-business-logic-layer-cs.md) tutorial para obter uma descrição mais completa sobre esses atributos e seus efeitos.

No `GetProducts()` e `GetProductsByCategoryID(categoryID)` métodos, os dados retornados do `GetCacheItem(key)` método é atribuído a uma variável local. O `GetCacheItem(key)` método, que examinaremos em breve, retorna um item específico do cache com base em especificado *chave*. Se esses dados não for encontrados no cache, ele é recuperado do correspondente `ProductsBLL` método de classe e, em seguida, adicionado ao cache usando o `AddCacheItem(key, value)` método.

O `GetCacheItem(key)` e `AddCacheItem(key, value)` métodos de interface com o cache de dados, lendo e gravando valores, respectivamente. O `GetCacheItem(key)` método é mais simples dos dois. Ele simplesmente retorna o valor da classe de Cache usando passado *chave*:


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample6.cs)]

`GetCacheItem(key)` não usa *chave* valor conforme fornecido, mas em vez disso, chama o `GetCacheKey(key)` método, que retorna o *chave* prefixados por ProductsCache-. O `MasterCacheKeyArray`, que contém a cadeia de caracteres ProductsCache, também é usado pelo `AddCacheItem(key, value)` método, como veremos momentaneamente.

De uma classe de code-behind de s de página ASP.NET, o cache de dados pode ser acessado usando o `Page` classe s [ `Cache` propriedade](https://msdn.microsoft.com/library/system.web.ui.page.cache.aspx)e permite uma sintaxe como `Cache["key"] = value`, conforme descrito na etapa 2. De uma classe dentro da arquitetura, o cache de dados pode ser acessado usando um `HttpRuntime.Cache` ou `HttpContext.Current.Cache`. [Peter Johnson](https://weblogs.asp.net/pjohnson/default.aspx)da entrada de blog [HttpRuntime.Cache vs. HttpContext.Current.Cache](https://weblogs.asp.net/pjohnson/httpruntime-cache-vs-httpcontext-current-cache) observa a pequena vantagem de desempenho usando `HttpRuntime` em vez de `HttpContext.Current`; Consequentemente, `ProductsCL` usa `HttpRuntime`.

> [!NOTE]
> Se sua arquitetura é implementada usando os projetos de biblioteca de classes, você precisará adicionar uma referência para o `System.Web` assembly para usar o [HttpRuntime](https://msdn.microsoft.com/library/system.web.httpruntime.aspx) e [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) classes.


Se o item não for encontrado no cache, o `ProductsCL` métodos de classe s obter os dados da BLL e adicioná-lo ao cache usando o `AddCacheItem(key, value)` método. Para adicionar *valor* ao cache poderíamos usar o código a seguir, que usa uma expiração de tempo de 60 segundos:


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample7.cs)]

`DateTime.Now.AddSeconds(CacheDuration)` Especifica a expiração do tempo de 60 segundos While futuras [ `System.Web.Caching.Cache.NoSlidingExpiration` ](https://msdn.microsoft.com/library/system.web.caching.cache.noslidingexpiration(vs.80).aspx) indica que há s sem prazo de expiração deslizante. Embora isso `Insert` tem sobrecarga de método de entrada parâmetros para os dois absoluta e deslizante expiração, você só pode fornecer um dos dois. Se você tentar especificar um tempo absoluto e um período de tempo, o `Insert` método lançará um `ArgumentException` exceção.

> [!NOTE]
> Essa implementação do `AddCacheItem(key, value)` método atualmente tem algumas limitações. Vamos abordar e resolver esses problemas na etapa 4.


## <a name="step-4-invalidating-the-cache-when-the-data-is-modified-through-the-architecture"></a>Etapa 4: Invalidar o Cache quando os dados é modificada por meio de arquitetura a

Junto com os métodos de recuperação de dados, a camada de cache precisa fornecer os mesmos métodos que a BLL para inserção, atualização e exclusão de dados. Os métodos de modificação de dados do CL s não modificam os dados armazenados em cache, mas em vez disso, chame o método de modificação de dados correspondente do BLL s e, em seguida, invalida o cache. Como vimos no tutorial anterior, isso é o mesmo comportamento que o ObjectDataSource aplica-se quando seus recursos de cache estão habilitados e sua `Insert`, `Update`, ou `Delete` métodos são invocados.

O seguinte `UpdateProduct` sobrecarga ilustra como implementar os métodos de modificação de dados em CL:


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample8.cs)]

A método da camada de lógica comercial de modificação de dados apropriado é invocada, mas antes que sua resposta seja retornada precisamos invalidar o cache. Infelizmente, invalidar o cache não é simples porque o `ProductsCL` classe s `GetProducts()` e `GetProductsByCategoryID(categoryID)` métodos cada adicionam itens ao cache com chaves diferentes e o `GetProductsByCategoryID(categoryID)` método adiciona um item de cache diferentes para cada exclusivo *categoryID*.

Ao invalidar o cache, é necessário remover *todos os* dos itens que podem ter sido adicionados pelo `ProductsCL` classe. Isso pode ser feito por meio da associação um *dependência de cache* com a cada item adicionado ao cache no servidor a `AddCacheItem(key, value)` método. Em geral, uma dependência de cache pode ser outro item em cache, um arquivo no sistema de arquivos ou dados de um banco de dados do Microsoft SQL Server. Quando a dependência foi alterado ou removido do cache, os itens de cache é associado automaticamente são removidos do cache. Para este tutorial, queremos criar um item adicional no cache que serve como uma dependência de cache para todos os itens adicionados por meio de `ProductsCL` classe. Dessa forma, todos esses itens podem ser removidos do cache, simplesmente removendo a dependência de cache.

Atualização de s permitem que o `AddCacheItem(key, value)` método para que cada item adicionado ao cache por meio desse método é associado uma dependência de cache único:


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample9.cs)]

`MasterCacheKeyArray` é uma matriz de cadeia de caracteres que contém um único valor, ProductsCache. Primeiro, um item de cache é adicionado ao cache e atribuído a data e hora atuais. Se o item de cache já existir, ele será atualizado. Em seguida, uma dependência de cache é criada. O [ `CacheDependency` classe](https://msdn.microsoft.com/library/system.web.caching.cachedependency(VS.80).aspx) construtor s tem um número de sobrecargas, mas o que está sendo usada aqui espera duas `string` entradas de matriz. A primeira delas Especifica o conjunto de arquivos a serem usados como dependências. Já que estamos não não desejo usar quaisquer dependências com base em arquivo, um valor de `null` é usado para o primeiro parâmetro de entrada. O segundo parâmetro de entrada especifica o conjunto de chaves de cache a ser usado como dependências. Aqui, especificamos nosso única dependência, `MasterCacheKeyArray`. O `CacheDependency` , em seguida, é passado para o `Insert` método.

Com essa modificação para `AddCacheItem(key, value)`, invaliding o cache é tão simple quanto removendo a dependência.


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample10.cs)]

## <a name="step-5-calling-the-caching-layer-from-the-presentation-layer"></a>Etapa 5: Chamar a camada de cache da camada de apresentação

Os métodos e classes de s da camada de armazenamento em cache podem ser usados para trabalhar com dados usando as técnicas podemos ve examinado durante esses tutoriais. Para ilustrar a trabalhar com dados armazenados em cache, salve suas alterações para o `ProductsCL` de classe e, em seguida, abra o `FromTheArchitecture.aspx` página no `Caching` pasta e adicione um GridView. De GridView s marca inteligente, crie um novo ObjectDataSource. A primeira etapa do assistente s, você verá o `ProductsCL` da classe como uma das opções na lista suspensa.


[![A classe ProductsCL está incluída na lista suspensa de objeto comercial](caching-data-in-the-architecture-cs/_static/image5.png)](caching-data-in-the-architecture-cs/_static/image4.png)

**Figura 4**: O `ProductsCL` classe está incluída na lista suspensa de objeto comercial ([clique para exibir a imagem em tamanho normal](caching-data-in-the-architecture-cs/_static/image6.png))


Depois de selecionar `ProductsCL`, clique em Avançar. A lista suspensa na guia SELECT possui dois itens - `GetProducts()` e `GetProductsByCategoryID(categoryID)` e a guia de atualização tem o único `UpdateProduct` de sobrecarga. Escolha o `GetProducts()` método a partir da guia SELECT e o `UpdateProducts` método a partir de guia de atualização e clique em Concluir.


[![Os métodos de classe ProductsCL s estão listados no menu suspenso lista](caching-data-in-the-architecture-cs/_static/image8.png)](caching-data-in-the-architecture-cs/_static/image7.png)

**Figura 5**: O `ProductsCL` métodos da classe s estão listados no menu suspenso lista ([clique para exibir a imagem em tamanho normal](caching-data-in-the-architecture-cs/_static/image9.png))


Depois de concluir o assistente, o Visual Studio definirá o s ObjectDataSource `OldValuesParameterFormatString` propriedade para `original_{0}` e adicione os campos apropriados para o GridView. Alterar o `OldValuesParameterFormatString` propriedade de volta para seu valor padrão, `{0}`e configurar o GridView para dar suporte à paginação, classificação e de edição. Uma vez que o `UploadProducts` sobrecarga usada por CL aceita apenas o nome do produto editado s e o preço, limitar o GridView para que somente esses campos são editáveis.

No tutorial anterior, definimos um GridView para incluir campos para o `ProductName`, `CategoryName`, e `UnitPrice` campos. Fique à vontade replicar essa formatação e estrutura, caso em que o GridView e ObjectDataSource s declarativo marcação deve ser semelhante ao seguinte:


[!code-aspx[Main](caching-data-in-the-architecture-cs/samples/sample11.aspx)]

Neste ponto, temos uma página que usa a camada de armazenamento em cache. Para ver o cache em ação, definir pontos de interrupção a `ProductsCL` classe s `GetProducts()` e `UpdateProduct` métodos. Visite a página em um navegador e percorrer o código durante a classificação e paginação para ver os dados extraídos do cache. Em seguida, atualizar um registro e observe que o cache é invalidado e, consequentemente, ele é recuperado da BLL quando os dados são reassociados ao GridView.

> [!NOTE]
> A camada de cache fornecida no download que acompanha este artigo não foi concluída. Ele contém apenas uma classe, `ProductsCL`, que tem apenas um punhado de métodos. Além disso, apenas uma única página do ASP.NET usa o CL (`~/Caching/FromTheArchitecture.aspx`) todos os outros ainda faça referência a BLL diretamente. Se você planeja usar uma CL em seu aplicativo, todas as chamadas da camada de apresentação devem ir para CL, que exige que as classes de s CL, e métodos abordados essas classes e métodos na BLL usado atualmente pela camada de apresentação.


## <a name="summary"></a>Resumo

Enquanto o cache pode ser aplicado na camada de apresentação com ASP.NET 2.0 s SqlDataSource e ObjectDataSource controles, o ideal é que as responsabilidades de cache seria ser delegado a uma camada separada na arquitetura. Neste tutorial, criamos uma camada de cache que reside entre a camada de apresentação e a camada de lógica de negócios. A camada de cache precisa fornecer o mesmo conjunto de classes e métodos que existem na BLL e são chamados na camada de apresentação.

Os exemplos de camada de cache que exploramos neste e os tutoriais anteriores apresentou *reativo carregamento*. Com o carregamento reativo, os dados são carregados no cache somente quando é feita uma solicitação para os dados e que os dados estão ausentes do cache. Dados também podem ser *proativamente carregado* no cache, uma técnica que carrega os dados no cache antes que seja realmente necessário. O próximo tutorial, veremos um exemplo de carregamento pró-ativo quando olhamos como armazenar valores estáticos em cache na inicialização do aplicativo.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Revisor de avanço para este tutorial foi Teresa Murph. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](caching-data-with-the-objectdatasource-cs.md)
> [Próximo](caching-data-at-application-startup-cs.md)
