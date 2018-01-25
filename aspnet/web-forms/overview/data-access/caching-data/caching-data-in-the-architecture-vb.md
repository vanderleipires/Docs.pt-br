---
uid: web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-vb
title: Cache de dados na arquitetura (VB) | Microsoft Docs
author: rick-anderson
description: "No tutorial anterior aprendemos como aplicar o cache na camada de apresentação. Neste tutorial, saiba como aproveitar nossa architectu em camadas..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2007
ms.topic: article
ms.assetid: 5e189dd7-f4f9-4f28-9b3a-6cb7d392e9c7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-vb
msc.type: authoredcontent
ms.openlocfilehash: 1aca89b022bb3bb7e4154ab575b5bb5513144cd5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="caching-data-in-the-architecture-vb"></a>Cache de dados na arquitetura (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_59_VB.exe) ou [baixar PDF](caching-data-in-the-architecture-vb/_static/datatutorial59vb1.pdf)

> No tutorial anterior aprendemos como aplicar o cache na camada de apresentação. Neste tutorial, saiba como tirar proveito da nossa arquitetura em camadas em cache os dados na camada de lógica de negócios. Para fazer isso, estendendo a arquitetura para incluir uma camada de cache.


## <a name="introduction"></a>Introdução

Como vimos no tutorial anterior, os dados de s ObjectDataSource o cache é tão simples quanto definir algumas propriedades. Infelizmente, o ObjectDataSource aplica caching na camada de apresentação, que agrupa rigidamente as políticas de cache com a página ASP.NET. Um dos motivos para a criação de uma arquitetura em camadas é permitir que esses acoplamentos ser interrompido. A camada de lógica de negócios, por exemplo, separa a lógica de negócios de páginas ASP.NET, enquanto a camada de acesso a dados separa os detalhes de acesso de dados. Essa desassociação de negócios lógica e os dados de detalhes de acesso é preferido, em parte, pois ele torna o sistema mais legível, mais passível de manutenção e mais flexível para alterar. Ele também permite o conhecimento de domínio e de divisão de trabalho, um desenvolvedor trabalhando na camada de apresentação t precisa estar familiarizado com os detalhes de s do banco de dados para fazer seu trabalho. Separando a política de cache da camada de apresentação oferece benefícios semelhantes.

Neste tutorial, será aumentar nossa arquitetura para incluir um *camada cache* (ou CL abreviada) que emprega nossa política de cache. A camada de cache incluirá um `ProductsCL` classe que fornece acesso às informações de produto com métodos, como `GetProducts()`, `GetProductsByCategoryID(categoryID)`e assim por diante, que, quando chamado, será a primeira tentativa de recuperar os dados do cache. Se o cache está vazio, esses métodos invocará apropriada `ProductsBLL` método na BLL, que por sua vez deve obter os dados da DAL. O `ProductsCL` métodos armazenar em cache os dados recuperados da BLL antes de retorná-lo.

Como mostra a Figura 1, o CL reside entre as camadas de lógica de negócios e apresentação.


![O cache de camada (CL) é outra camada na arquitetura de nosso](caching-data-in-the-architecture-vb/_static/image1.png)

**Figura 1**: O cache de camada (CL) é outra camada na arquitetura de nosso


## <a name="step-1-creating-the-caching-layer-classes"></a>Etapa 1: Criar as Classes de camada de cache

Neste tutorial, vamos criar uma CL muito simple com uma única classe `ProductsCL` que tem apenas uma série de métodos. Criando uma camada de cache concluída para o aplicativo inteiro exigiria criando `CategoriesCL`, `EmployeesCL`, e `SuppliersCL` classes e fornecer um método nessas classes de camada de cache para cada método de acesso ou modificação de dados na BLL. Com o BLL e DAL, a camada de cache ideal deve ser implementada como um projeto de biblioteca de classes separado; No entanto, vamos implementá-lo como uma classe no `App_Code` pasta.

Para mais classes separadas corretamente o CL das classes DAL e BLL, permitem s cria uma nova subpasta no `App_Code` pasta. Clique com botão direito no `App_Code` pasta no Gerenciador de soluções, escolha a nova pasta e nomeie a nova pasta `CL`. Depois de criar essa pasta, adicione a ela uma nova classe chamada `ProductsCL.vb`.


![Adicionar uma nova pasta chamada CL e uma classe denominada ProductsCL.vb](caching-data-in-the-architecture-vb/_static/image2.png)

**Figura 2**: adicionar uma nova pasta chamada `CL` e uma classe denominada`ProductsCL.vb`


O `ProductsCL` classe deve incluir o mesmo conjunto de métodos de acesso e modificação de dados como encontrado em sua classe de camada de lógica comercial correspondente (`ProductsBLL`). Em vez de criar a todos esses métodos, s permitem apenas compilação aqui alguns para conhecer os padrões usados pelo CL. Em particular, adicionaremos o `GetProducts()` e `GetProductsByCategoryID(categoryID)` métodos na etapa 3 e um `UpdateProduct` sobrecarga na etapa 4. Você pode adicionar o restante `ProductsCL` métodos e `CategoriesCL`, `EmployeesCL`, e `SuppliersCL` classes como quiser.

## <a name="step-2-reading-and-writing-to-the-data-cache"></a>Etapa 2: Lendo e gravando no Cache de dados

ObjectDataSource cache recurso explorado internamente no tutorial anterior usa o cache de dados do ASP.NET para armazenar os dados recuperados da BLL. O cache de dados também pode ser acessado por meio de programação de classes de code-behind de páginas ASP.NET ou de classes na arquitetura de s do aplicativo web. Para ler e gravar no cache de dados de uma classe de code-behind de s de página ASP.NET, use o seguinte padrão:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample1.vb)]

O [ `Cache` classe](https://msdn.microsoft.com/library/system.web.caching.cache.aspx) s [ `Insert` método](https://msdn.microsoft.com/library/system.web.caching.cache.insert.aspx) tem várias sobrecargas. `Cache("key") = value`e `Cache.Insert(key, value)` são sinônimos e ambos adicionam um item ao cache usando a chave especificada sem um vencimento definido. Normalmente, queremos especificar uma expiração, ao adicionar um item ao cache, como uma dependência, uma expiração baseada em tempo ou ambos. Use um dos outros `Insert` sobrecargas de método s para fornecer informações com base em dependência ou tempo de expiração.

A camada de cache métodos precisam primeiro verificar se os dados solicitados no cache e, nesse caso, retorná-lo de lá. Se os dados solicitados não estiverem no cache, o método BLL apropriado deve ser invocado. O valor de retorno deve ser armazenado em cache e, em seguida, retornado, como mostra o diagrama de sequência a seguir.


![Os camada de cache métodos retornam dados do Cache se ele s disponível](caching-data-in-the-architecture-vb/_static/image3.png)

**Figura 3**: A camada de cache métodos retornam dados do Cache se ele s disponível


A sequência ilustrada na Figura 3 é realizada nas classes CL usando o seguinte padrão:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample2.vb)]

Aqui, *tipo* é o tipo de dados armazenados no cache de `Northwind.ProductsDataTable`, por exemplo *chave* é a chave que identifica exclusivamente o item de cache. Se o item com especificado *chave* não está no cache, em seguida, *instância* será `Nothing` e os dados serão recuperados do método BLL apropriado e adicionados ao cache. No momento `Return instance` for atingido, *instância* contém uma referência para os dados do cache ou retirados do BLL.

Certifique-se de usar o padrão acima ao acessar dados do cache. O seguinte padrão, o que, a princípio, parece equivalente, contém uma diferença sutil que apresenta uma condição de corrida. Condições de corrida são difíceis de depurar porque eles se revelam esporadicamente e difíceis de reproduzir.


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample3.vb)]

A diferença neste segundo, o trecho de código incorreto é que, em vez de armazenar uma referência para o item em cache em uma variável local, o cache de dados é acessado diretamente na instrução condicional *e* no `Return`. Imagine que, quando esse código é alcançado, `Cache("key")` não é `Nothing`, mas antes de `Return` instrução for atingida, o sistema remove *chave* do cache. Nesse caso raro, o código irá retornar `Nothing` em vez de um objeto do tipo esperado.

> [!NOTE]
> O cache de dados é thread-safe, então não é necessário sincronizar o acesso de thread para simples leituras ou gravações. No entanto, se você precisar executar várias operações em dados em cache que precisam ser atômica, você é responsável por implementar um bloqueio ou outro mecanismo para garantir a segurança do thread. Consulte [sincronizar o acesso ao Cache ASP.NET](http://www.ddj.com/184406369) para obter mais informações.


Um item pode ser removido por meio de programação do cache de dados usando o [ `Remove` método](https://msdn.microsoft.com/library/system.web.caching.cache.remove.aspx) da seguinte forma:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample4.vb)]

## <a name="step-3-returning-product-information-from-theproductsclclass"></a>Etapa 3: Retornando informações de produto de`ProductsCL`classe

Para este tutorial permitem s implemente dois métodos para retornar informações de produto o `ProductsCL` classe: `GetProducts()` e `GetProductsByCategoryID(categoryID)`. Como com o `ProductsBL` classe na camada de lógica de negócios, o `GetProducts()` método CL retorna informações sobre todos os produtos como um `Northwind.ProductsDataTable` objeto, enquanto `GetProductsByCategoryID(categoryID)` retorna todos os produtos de uma categoria especificada.

O código a seguir mostra uma parte dos métodos na `ProductsCL` classe:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample5.vb)]

Primeiro, observe o `DataObject` e `DataObjectMethodAttribute` atributos aplicados para a classe e métodos. Esses atributos fornecem informações ao Assistente de s ObjectDataSource, indicando que as classes e métodos devem aparecer nas etapas do assistente s. Como as classes de CI e os métodos serão acessados de um ObjectDataSource na camada de apresentação, adicionei esses atributos para melhorar a experiência de tempo de design. Voltar para o [criando uma camada de lógica de negócios](../introduction/creating-a-business-logic-layer-vb.md) tutorial para obter uma descrição mais completa sobre esses atributos e seus efeitos.

No `GetProducts()` e `GetProductsByCategoryID(categoryID)` métodos, os dados retornados do `GetCacheItem(key)` método é atribuído a uma variável local. O `GetCacheItem(key)` método examinaremos em breve, retorna um item específico do cache com base em especificado *chave*. Se esses dados não foi encontrados no cache, ele será recuperado do correspondente `ProductsBLL` método de classe e, em seguida, adicionados ao cache usando o `AddCacheItem(key, value)` método.

O `GetCacheItem(key)` e `AddCacheItem(key, value)` métodos de interface com o cache de dados, lendo e gravando valores, respectivamente. O `GetCacheItem(key)` método é mais simples das duas. Ele simplesmente retorna o valor da classe de Cache usando passado na *chave*:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample6.vb)]

`GetCacheItem(key)`Não use *chave* valor fornecido, mas em vez disso, chama o `GetCacheKey(key)` método, que retorna o *chave* anexado com ProductsCache-. O `MasterCacheKeyArray`, que contém a cadeia de caracteres ProductsCache, também é usado pelo `AddCacheItem(key, value)` método, conforme veremos momentaneamente.

De uma classe de code-behind de páginas ASP.NET, o cache de dados pode ser acessado usando o `Page` classe s [ `Cache` propriedade](https://msdn.microsoft.com/library/system.web.ui.page.cache.aspx)e permite uma sintaxe semelhante à `Cache("key") = value`, conforme descrito na etapa 2. De uma classe dentro da arquitetura, o cache de dados pode ser acessado usando um `HttpRuntime.Cache` ou `HttpContext.Current.Cache`. [Peter Johnson](https://weblogs.asp.net/pjohnson/default.aspx)da entrada de blog [HttpRuntime.Cache vs. HttpContext.Current.Cache](https://weblogs.asp.net/pjohnson/httpruntime-cache-vs-httpcontext-current-cache) observa a pequena vantagem de desempenho usando `HttpRuntime` em vez de `HttpContext.Current`; Consequentemente, `ProductsCL` usa `HttpRuntime`.

> [!NOTE]
> Se a arquitetura é implementada usando projetos de biblioteca de classe, você precisará adicionar uma referência para o `System.Web` assembly para usar o [ `HttpRuntime` ](https://msdn.microsoft.com/library/system.web.httpruntime.aspx) e [ `HttpContext` ](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) classes.


Se o item não for encontrado no cache, o `ProductsCL` métodos da classe s obter os dados de BLL e adicioná-lo ao cache usando o `AddCacheItem(key, value)` método. Para adicionar *valor* ao cache podemos usar o código a seguir, que usa uma expiração de tempo de 60 segundos:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample7.vb)]

`DateTime.Now.AddSeconds(CacheDuration)`Especifica a expiração do tempo 60 segundos de tempo futuras [ `System.Web.Caching.Cache.NoSlidingExpiration` ](https://msdn.microsoft.com/library/system.web.caching.cache.noslidingexpiration(vs.80).aspx) indica que existem s sem expiração deslizante. Embora isso `Insert` sobrecarga do método tem parâmetros para ambos os um absoluto de entrada e deslizante expiração, você pode apenas fornecer um dos dois. Se você tentar especificar um tempo absoluto e um período de tempo, o `Insert` método lançará um `ArgumentException` exceção.

> [!NOTE]
> Essa implementação do `AddCacheItem(key, value)` método atualmente tem algumas limitações. Vamos endereço e resolver esses problemas na etapa 4.


## <a name="step-4-invalidating-the-cache-when-the-data-is-modified-through-the-architecture"></a>Etapa 4: Invalidar o Cache quando os dados é modificada a arquitetura

Junto com os métodos de recuperação de dados, a camada de cache precisa fornecer os mesmos métodos que BLL para inserir, atualizar e excluir dados. Os métodos de modificação de dados de s CL não modificam os dados em cache, mas em vez disso, chame o método de modificação de dados correspondente do BLL s e, em seguida, invalida o cache. Como vimos no tutorial anterior, esse é o mesmo comportamento que se aplica o ObjectDataSource quando seus recursos de cache estão habilitados e sua `Insert`, `Update`, ou `Delete` métodos forem invocados.

O seguinte `UpdateProduct` sobrecarga ilustra como implementar os métodos de modificação de dados em CL:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample8.vb)]

A método de camada de lógica comercial de modificação de dados apropriado é chamada, mas antes que sua resposta seja retornada, precisamos invalidar o cache. Infelizmente, invalidar o cache não é simples porque o `ProductsCL` classe s `GetProducts()` e `GetProductsByCategoryID(categoryID)` métodos cada adicionam itens ao cache com chaves diferentes e o `GetProductsByCategoryID(categoryID)` método adiciona um item de cache diferentes para cada exclusivo *categoryID*.

Quando invalidar o cache, é preciso remover *todos os* os itens que foram adicionados pela `ProductsCL` classe. Isso pode ser feito por meio da associação um *a dependência de cache* com cada item adicionado ao cache de `AddCacheItem(key, value)` método. Em geral, uma dependência de cache pode ser outro item no cache, um arquivo no sistema de arquivos ou dados de um banco de dados do Microsoft SQL Server. Quando a dependência foi alterado ou removido do cache, os itens de cache é associado automaticamente são removidos do cache. Para este tutorial, queremos criar um item adicional no cache que serve como uma dependência de cache para todos os itens adicionados por meio de `ProductsCL` classe. Dessa forma, todos esses itens podem ser removidos do cache, simplesmente removendo a dependência de cache.

Atualização de s permitem que o `AddCacheItem(key, value)` método para que cada item adicionado ao cache por esse método é associado uma dependência de cache único:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample9.vb)]

`MasterCacheKeyArray`é uma matriz de cadeia de caracteres que contém um único valor, ProductsCache. Primeiro, um item de cache é adicionado ao cache e atribuído a data e hora atuais. Se o item de cache já existir, ela será atualizada. Em seguida, uma dependência de cache é criada. O [ `CacheDependency` classe](https://msdn.microsoft.com/library/system.web.caching.cachedependency(VS.80).aspx) construtor s tem várias sobrecargas, mas está sendo usada aqui espera dois `String` entradas de matriz. A primeira delas Especifica o conjunto de arquivos a ser usado como dependências. Já que estamos não não desejo usar quaisquer dependências com base em arquivo, um valor de `Nothing` é usado para o primeiro parâmetro de entrada. O segundo parâmetro de entrada especifica o conjunto de chaves de cache para usar como dependências. Especificar aqui é nossa única dependência, `MasterCacheKeyArray`. O `CacheDependency` , em seguida, é passado para o `Insert` método.

Com essa modificação `AddCacheItem(key, value)`, invaliding o cache é tão simple quanto a remoção da dependência.


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample10.vb)]

## <a name="step-5-calling-the-caching-layer-from-the-presentation-layer"></a>Etapa 5: Chamada a camada de cache da camada de apresentação

As classes da camada de cache de s e os métodos podem ser usados para trabalhar com dados usando as técnicas ve examinado durante esses tutoriais. Para ilustrar a trabalhar com dados armazenados em cache, salvar suas alterações para o `ProductsCL` classe e, em seguida, abra o `FromTheArchitecture.aspx` página o `Caching` pasta e adicionar um controle GridView. A partir de GridView s marca inteligente, crie um novo ObjectDataSource. A primeira etapa do assistente s, você verá o `ProductsCL` da classe como uma das opções na lista suspensa.


[![A classe ProductsCL está incluída na lista suspensa de objeto comercial](caching-data-in-the-architecture-vb/_static/image5.png)](caching-data-in-the-architecture-vb/_static/image4.png)

**Figura 4**: O `ProductsCL` classe está incluída na lista suspensa de objeto comercial ([clique para exibir a imagem em tamanho normal](caching-data-in-the-architecture-vb/_static/image6.png))


Depois de selecionar `ProductsCL`, clique em Avançar. A lista suspensa na guia selecione tem dois itens - `GetProducts()` e `GetProductsByCategoryID(categoryID)` e a guia de atualização tem a única `UpdateProduct` de sobrecarga. Escolha o `GetProducts()` método da guia SELECT e o `UpdateProducts` método do guia de atualização e clique em Concluir.


[![Os métodos de classe ProductsCL s estão listados no menu suspenso lista](caching-data-in-the-architecture-vb/_static/image8.png)](caching-data-in-the-architecture-vb/_static/image7.png)

**Figura 5**: O `ProductsCL` métodos de classe s estão listados no menu suspenso lista ([clique para exibir a imagem em tamanho normal](caching-data-in-the-architecture-vb/_static/image9.png))


Depois de concluir o assistente, o Visual Studio definirá o s ObjectDataSource `OldValuesParameterFormatString` propriedade `original_{0}` e adicione os campos apropriados para o GridView. Alterar o `OldValuesParameterFormatString` propriedade para o seu valor padrão, `{0}`e configure o GridView para oferecer suporte a paginação, classificação e edição. Desde o `UploadProducts` sobrecarga usada por CL aceita apenas o nome de produto editado s e preço, limitar o GridView para que somente esses campos são editáveis.

No tutorial anterior, definimos um GridView para incluir campos para o `ProductName`, `CategoryName`, e `UnitPrice` campos. Fique à vontade para replicar essa estrutura e a formatação, caso em que o GridView e ObjectDataSource s declarativo marcação deve ser semelhante ao seguinte:


[!code-aspx[Main](caching-data-in-the-architecture-vb/samples/sample11.aspx)]

Neste ponto, temos uma página que usa a camada de cache. Para ver o cache em ação, definir pontos de interrupção de `ProductsCL` classe s `GetProducts()` e `UpdateProduct` métodos. Visite a página em um navegador e percorrer o código quando a classificação e paginação para ver os dados extraídos de cache. Em seguida, atualizar um registro e observe que o cache é invalidado e, consequentemente, ele será recuperado da BLL quando os dados é vinculada outra vez a GridView.

> [!NOTE]
> A camada de cache fornecida no download que acompanha este artigo não está completa. Ele contém apenas uma classe, `ProductsCL`, que tem apenas uma série de métodos. Além disso, uma única página ASP.NET usa CL (`~/Caching/FromTheArchitecture.aspx`) todos os outros ainda referenciam BLL diretamente. Se você planeja usar uma CL em seu aplicativo, todas as chamadas da camada de apresentação devem ir para CL, que exige que as classes de s CL e métodos abordados essas classes e métodos BLL usado atualmente pela camada de apresentação.


## <a name="summary"></a>Resumo

Enquanto o cache pode ser aplicado na camada de apresentação com o ASP.NET 2.0 s SqlDataSource e ObjectDataSource controles, idealmente cache responsabilidades seria delegado para uma camada separada na arquitetura. Neste tutorial, criamos uma camada de cache que reside entre a camada de apresentação e a camada de lógica de negócios. A camada de cache precisa fornecer o mesmo conjunto de classes e métodos que existem na BLL e são chamados da camada de apresentação.

Os exemplos de camada de cache é explorados neste e os tutoriais anteriores exibidos *carregamento reativo*. Carregamento reativo, os dados são carregados no cache somente quando é feita uma solicitação para os dados e dados estão ausentes do cache. Dados também podem ser *proativamente carregado* no cache, uma técnica que carrega os dados no cache antes que seja realmente necessário. O tutorial Avançar veremos um exemplo de carregamento pró-ativo quando vamos examinar como armazenar valores estáticos em cache na inicialização do aplicativo.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisor levar para este tutorial foi Teresa Murphy. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](caching-data-with-the-objectdatasource-vb.md)
[Próximo](caching-data-at-application-startup-vb.md)
