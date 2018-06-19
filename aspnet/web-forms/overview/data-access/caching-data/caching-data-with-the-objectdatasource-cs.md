---
uid: web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-cs
title: Cache de dados com o ObjectDataSource (c#) | Microsoft Docs
author: rick-anderson
description: O cache pode significar a diferença entre lento e um aplicativo Web rápido. Este tutorial é o primeiro de quatro que dê uma olhada detalhada em cache no ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2007
ms.topic: article
ms.assetid: bd87413c-8160-4520-a8a2-43b555c4183a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: c30dcba7d6ff9849371ef92c84d4ec32aa1dc73d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879186"
---
<a name="caching-data-with-the-objectdatasource-c"></a>Cache de dados com o ObjectDataSource (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_58_CS.exe) ou [baixar PDF](caching-data-with-the-objectdatasource-cs/_static/datatutorial58cs1.pdf)

> O cache pode significar a diferença entre lento e um aplicativo Web rápido. Este tutorial é o primeiro de quatro que dê uma olhada detalhada em cache no ASP.NET. Aprenda os principais conceitos de cache e como aplicar o cache para a camada de apresentação por meio do controle ObjectDataSource.


## <a name="introduction"></a>Introdução

Na ciência da computação, *cache* é o processo de colocar dados ou informações é caras para obter e armazenar uma cópia em um local que é mais rápido para acessar. Para aplicativos orientados a dados, consultas grandes e complexas geralmente consomem a maior parte do tempo de execução do aplicativo s. Tal um aplicativo s, em seguida, geralmente possível melhorar o desempenho armazenando os resultados das consultas caras de banco de dados na memória s do aplicativo.

O ASP.NET 2.0 oferece uma variedade de opções de cache. Uma página inteira da web ou marcação de s renderizado de controle de usuário pode ser armazenados em cache por meio de *cache de saída*. Os controles ObjectDataSource e SqlDataSource fornecem recursos de cache também e, portanto, permitindo que os dados sejam armazenados em cache no nível de controle. E ASP.NET s *cache de dados* oferece uma API de cache avançada que permite que os desenvolvedores de página para objetos do cache programaticamente. Este tutorial e as próximas três que examinaremos usando o s ObjectDataSource cache recursos, bem como o cache de dados. Também exploraremos como armazenar em cache dados de todo o aplicativo na inicialização e como manter os dados armazenados em cache atualizado com o uso de dependências de cache SQL. Esses tutoriais não explorar o cache de saída. Para obter uma visão detalhada de cache de saída, consulte [cache de saída no ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx).

O cache pode ser aplicado em qualquer lugar na arquitetura, da camada de acesso a dados backup por meio da camada de apresentação. Neste tutorial, examinaremos a aplicação de cache para a camada de apresentação por meio do controle ObjectDataSource. No próximo tutorial, examinaremos cache de dados na camada de lógica de negócios.

## <a name="key-caching-concepts"></a>Conceitos de cache de chave

O cache pode melhorar significativamente a um aplicativo s geral de desempenho e escalabilidade considerando os dados que são caros para gerar e armazenar uma cópia em um local que possa ser acessado com mais eficiência. Desde que o cache contém apenas uma cópia dos dados reais, subjacentes, ele pode se tornar desatualizado, ou *obsoletos*, se os dados subjacentes forem alterados. Para combater isso, um desenvolvedor de página pode indicar que os critérios pelos quais o item de cache será *removidos* do cache, usando:

- **Critérios de tempo** um item pode ser adicionado ao cache para um absoluto ou deslizante duração. Por exemplo, um desenvolvedor de página pode indicar uma duração de, digamos, 60 segundos. Com uma duração absoluta, o item em cache é removido 60 segundos depois que ele foi adicionado ao cache, independentemente da frequência com a qual ele foi acessado. Com uma duração deslizante, o item em cache é removido 60 segundos depois do último acesso.
- **Critérios com base na dependência** uma dependência pode ser associada um item quando adicionado ao cache. Quando altera a dependência do item s é removido do cache. A dependência pode ser um arquivo, outro item de cache ou uma combinação dos dois. O ASP.NET 2.0 também permite que dependências de cache SQL, que permitem aos desenvolvedores adicionar um item ao cache e que ele seja removido quando o banco de dados subjacente é alterado. Vamos examinar as dependências de cache SQL em que a próxima [dependências de Cache de SQL usando](using-sql-cache-dependencies-cs.md) tutorial.

Os critérios de remoção especificados, independentemente de um item no cache pode ser *eliminado* antes dos critérios com base em dependência ou tempo foram atendidos. Se o cache atingiu sua capacidade, itens existentes devem ser removidos antes novos podem ser adicionados. Consequentemente, quando programaticamente trabalhar com dados armazenados em cache s essencial que você sempre pressupõe que os dados em cache podem não estar presentes. Vamos examinar o padrão a ser usado ao acessar dados do cache programaticamente em nosso tutorial próximo *cache de dados na arquitetura*.

O cache fornece uma maneira econômica para apertando mais o desempenho de um aplicativo. Como [Steven Smith](http://aspadvice.com/blogs/ssmith/) articula em seu artigo [cache ASP.NET: técnicas e práticas recomendadas](https://msdn.microsoft.com/library/aa478965.aspx):

O cache pode ser uma boa maneira de obter bom desempenho suficiente sem exigir muito tempo e análise. Memória seja barata, portanto, se você pode obter o desempenho necessários, a saída de cache para 30 segundos, em vez de gastar um dia ou uma semana tentar otimizar seu código ou o banco de dados, faça a solução de cache (supondo que dados 30 - antigos segundos é okey) e mover. Eventualmente, design ruim deverá fazer parte, portanto obviamente, você deve tentar criar seus aplicativos adequadamente. Mas se você precisar obter bom desempenho hoje, o cache pode ser excelente [abordagem], compra de tempo para refatorar o aplicativo em uma data posterior, quando você tem o tempo para fazer isso.

Enquanto o cache pode oferecer melhorias de desempenho considerável, ela não é aplicável em todas as situações, como com aplicativos que usam dados em tempo real, com frequência de atualização, ou em que até mesmo em breve viver dados obsoletos são inaceitáveis. Mas, para a maioria dos aplicativos, o cache deve ser usado. Para obter mais informações sobre o cache no ASP.NET 2.0, consulte o [cache para desempenho](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/default.aspx) seção o [tutoriais de início rápido do ASP.NET 2.0](https://quickstarts.asp.net/QuickStartv20/aspnet/).

## <a name="step-1-creating-the-caching-web-pages"></a>Etapa 1: Criando páginas da Web do cache

Antes de começar nosso exploração dos recursos de cache s ObjectDataSource, permitem s primeiro dedicar um tempo para criar as páginas ASP.NET em nosso projeto de site que vamos precisar para este tutorial e as próximas três. Comece adicionando uma nova pasta chamada `Caching`. Em seguida, adicione as seguintes páginas do ASP.NET para a pasta, certificando-se de associar cada página com o `Site.master` página mestre:

- `Default.aspx`
- `ObjectDataSource.aspx`
- `FromTheArchitecture.aspx`
- `AtApplicationStartup.aspx`
- `SqlCacheDependencies.aspx`


![Adicionar as páginas ASP.NET para os tutoriais relacionados ao armazenamento em cache](caching-data-with-the-objectdatasource-cs/_static/image1.png)

**Figura 1**: adicionar as páginas ASP.NET para os tutoriais relacionados ao armazenamento em cache


Como em outras pastas, `Default.aspx` no `Caching` pasta listará os tutoriais em sua seção. Lembre-se de que o `SectionLevelTutorialListing.ascx` controle de usuário fornece essa funcionalidade. Portanto, adicione este controle de usuário para `Default.aspx` arrastando-o no Gerenciador de soluções para a página de exibição de Design de s.


[![Figura 2: Adicionar o controle de usuário SectionLevelTutorialListing.ascx a Default.aspx](caching-data-with-the-objectdatasource-cs/_static/image3.png)](caching-data-with-the-objectdatasource-cs/_static/image2.png)

**Figura 2**: Figura 2: adicionar o `SectionLevelTutorialListing.ascx` controle de usuário `Default.aspx` ([clique para exibir a imagem em tamanho normal](caching-data-with-the-objectdatasource-cs/_static/image4.png))


Por fim, adicione estas páginas como entradas para o `Web.sitemap` arquivo. Especificamente, adicione a seguinte marcação depois de trabalhar com dados binários `<siteMapNode>`:


[!code-xml[Main](caching-data-with-the-objectdatasource-cs/samples/sample1.xml)]

Após a atualização `Web.sitemap`, reserve um tempo para exibir o site de tutoriais através de um navegador. No menu à esquerda agora inclui itens para os tutoriais de cache.


![O mapa do Site agora inclui entradas para os tutoriais de cache](caching-data-with-the-objectdatasource-cs/_static/image5.png)

**Figura 3**: O mapa de Site agora inclui entradas para os tutoriais de cache


## <a name="step-2-displaying-a-list-of-products-in-a-web-page"></a>Etapa 2: Exibindo uma lista de produtos em uma página da Web

Este tutorial explora como usar os ObjectDataSource controle s internos recursos de cache. Antes, podemos ver esses recursos, no entanto, primeiro precisamos de uma página para trabalhar. S de permitem criar uma página da web que usa um GridView para listar as informações de produto recuperadas por um ObjectDataSource do `ProductsBLL` classe.

Comece abrindo o `ObjectDataSource.aspx` página o `Caching` pasta. Arraste um controle GridView da caixa de ferramentas para o Designer, defina seu `ID` propriedade `Products`e, na sua marca inteligente, escolha para associá-lo para um novo controle ObjectDataSource chamado `ProductsDataSource`. Configurar o ObjectDataSource para trabalhar com o `ProductsBLL` classe.


[![Configurar o ObjectDataSource para usar a classe ProductsBLL](caching-data-with-the-objectdatasource-cs/_static/image7.png)](caching-data-with-the-objectdatasource-cs/_static/image6.png)

**Figura 4**: configurar o ObjectDataSource para usar o `ProductsBLL` classe ([clique para exibir a imagem em tamanho normal](caching-data-with-the-objectdatasource-cs/_static/image8.png))


Para essa página, permitem s criar um GridView editável para que podemos examinar o que acontece quando os dados armazenados em cache em ObjectDataSource são modificados por meio da interface de s GridView. Deixe a lista suspensa na guia SELECT definida para seu padrão, `GetProducts()`, mas alterar o item selecionado na guia de atualização para o `UpdateProduct` sobrecarga aceita `productName`, `unitPrice`, e `productID` como seus parâmetros de entrada.


[![Definir a lista de lista suspensa de s de guia de atualização para a sobrecarga apropriada UpdateProduct](caching-data-with-the-objectdatasource-cs/_static/image10.png)](caching-data-with-the-objectdatasource-cs/_static/image9.png)

**Figura 5**: definir o guia de atualização de s lista suspensa para o adequado `UpdateProduct` de sobrecarga ([clique para exibir a imagem em tamanho normal](caching-data-with-the-objectdatasource-cs/_static/image11.png))


Finalmente, defina as listas suspensas nas guias de INSERT e DELETE para (nenhum) e clique em Concluir. Após concluir o Assistente Configurar fonte de dados, o Visual Studio define o s ObjectDataSource `OldValuesParameterFormatString` propriedade `original_{0}`. Como discutido o [uma visão geral de inserção de, atualizando e excluindo dados](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) tutorial, essa propriedade precisa ser removido da sintaxe declarativa ou definido novamente como seu valor padrão, `{0}`, na ordem para o nosso fluxo de trabalho de atualização para continue sem erro.

Além disso, após a conclusão do Assistente para Visual Studio adiciona um campo à GridView para cada um dos campos de dados de produto. Remover tudo, exceto o `ProductName`, `CategoryName`, e `UnitPrice` BoundFields. Em seguida, atualize o `HeaderText` propriedades de cada um desses BoundFields produto, categoria e preço, respectivamente. Como o `ProductName` campo é obrigatório, converter o BoundField em um TemplateField e adicionar um controle RequiredFieldValidator para o `EditItemTemplate`. Da mesma forma, converter o `UnitPrice` BoundField em um TemplateField e adicione um CompareValidator para garantir que o valor inserido pelo usuário é o valor de uma moeda válida que s maior que ou igual a zero. Além dessas modificações, fique à vontade para executar todas as alterações estéticos, como alinhamento à direita o `UnitPrice` valor ou especificar a formatação para o `UnitPrice` texto em suas interfaces somente leitura e edição.

Tornar o GridView editável, marcando a caixa de seleção Habilitar edição na marca inteligente s GridView. Além disso, verifique as caixas de seleção Habilitar paginação e habilitar a classificação.

> [!NOTE]
> Precisa de uma revisão de como personalizar a interface de edição de GridView s? Nesse caso, consultar o [Personalizando a Interface de modificação de dados](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) tutorial.


[![Habilitar o suporte de GridView para edição, classificação e paginação](caching-data-with-the-objectdatasource-cs/_static/image13.png)](caching-data-with-the-objectdatasource-cs/_static/image12.png)

**Figura 6**: habilitar o suporte de GridView para edição, classificação e paginação ([clique para exibir a imagem em tamanho normal](caching-data-with-the-objectdatasource-cs/_static/image14.png))


Depois de fazer essas modificações de GridView, a marcação de declarativa s GridView e ObjectDataSource deve ser semelhante ao seguinte:


[!code-aspx[Main](caching-data-with-the-objectdatasource-cs/samples/sample2.aspx)]

Como mostra a Figura 7, GridView editável lista o nome, a categoria e o preço de cada um dos produtos no banco de dados. Reserve um tempo para testar os resultados, a página por eles, a classificação de funcionalidade de página s e editar um registro.


[![Cada produto s nome, a categoria e o preço é listado em um Sortable, Pageable, GridView editável](caching-data-with-the-objectdatasource-cs/_static/image16.png)](caching-data-with-the-objectdatasource-cs/_static/image15.png)

**Figura 7**: cada produto s nome, categoria e preço está listado em um Sortable, Pageable, GridView editável ([clique para exibir a imagem em tamanho normal](caching-data-with-the-objectdatasource-cs/_static/image17.png))


## <a name="step-3-examining-when-the-objectdatasource-is-requesting-data"></a>Etapa 3: Examinar ObjectDataSource o quando está solicitando dados

O `Products` GridView recupera seus dados a serem exibidos ao chamar o `Select` método o `ProductsDataSource` ObjectDataSource. Este ObjectDataSource cria uma instância de s a camada de lógica comercial `ProductsBLL` classe e chama seu `GetProducts()` método, que por sua vez chama a camada de acesso a dados s `ProductsTableAdapter` s `GetProducts()` método. O método DAL se conecta ao banco de dados Northwind e emite configurado `SELECT` consulta. Esses dados são retornados, em seguida, a DAL, que empacota em um `NorthwindDataTable`. O objeto DataTable é retornado para BLL, que retorna a ObjectDataSource, que retorna a GridView. O GridView, em seguida, cria um `GridViewRow` objeto para cada `DataRow` na DataTable e cada `GridViewRow` eventualmente é renderizado em HTML que é retornado ao cliente e exibido no navegador do visitante s.

Esta sequência de eventos acontece sempre que GridView precisa associar-se aos dados subjacentes. Isso acontece quando a página for visitada primeiro, ao mover de uma página de dados para outro, ao classificar GridView ou ao modificar dados GridView s por meio de seu interno edição ou exclusão de interfaces. Se o estado de exibição do GridView s estiver desabilitado, o GridView será religado em cada postback também. GridView pode também ser explicitamente religada para seus dados chamando seu `DataBind()` método.

Para aproveitar totalmente a frequência com que os dados são recuperados do banco de dados, permitem s exibirá uma mensagem indicando quando os dados estão sendo recuperados novamente. Adicionar um controle de rótulo Web acima GridView denominado `ODSEvents`. Limpar seu `Text` propriedade e defina seu `EnableViewState` propriedade `false`. Sob o rótulo, adicione um controle de botão Web e defina seu `Text` propriedade Postback.


[![Adicionar um rótulo e um botão à página acima GridView](caching-data-with-the-objectdatasource-cs/_static/image19.png)](caching-data-with-the-objectdatasource-cs/_static/image18.png)

**Figura 8**: adicionar um botão e o rótulo para a página acima a GridView ([clique para exibir a imagem em tamanho normal](caching-data-with-the-objectdatasource-cs/_static/image20.png))


Durante o fluxo de trabalho de acesso de dados, o s ObjectDataSource `Selecting` evento ser acionado antes do objeto subjacente é criado e seu método de configurado invocado. Criar um manipulador de eventos para esse evento e adicione o seguinte código:


[!code-csharp[Main](caching-data-with-the-objectdatasource-cs/samples/sample3.cs)]

Cada vez que o ObjectDataSource faz uma solicitação para a arquitetura de dados, o rótulo será exibido o evento Selecting de texto foi acionado.

Visite essa página em um navegador. Quando a página for visitada primeiro, o evento Selecting de texto acionado é mostrado. Clique no botão de postagem e observe que o texto desaparece (supondo que o GridView s `EnableViewState` está definida como `true`, o padrão). Isso ocorre porque, em um postback, GridView é reconstruída com seu estado de exibição e, portanto, t ativar a ObjectDataSource para seus dados. Classificação, paginação, ou editar os dados, no entanto, faz com que o GridView associar novamente a fonte de dados, e, portanto, o evento Selecting disparado texto será exibida novamente.


[![Sempre que o GridView é vinculada outra vez para a fonte de dados, o evento Selecting acionado é exibido](caching-data-with-the-objectdatasource-cs/_static/image22.png)](caching-data-with-the-objectdatasource-cs/_static/image21.png)

**Figura 9**: sempre que a GridView é vinculada outra vez para a fonte de dados, selecionar evento disparado é exibido ([clique para exibir a imagem em tamanho normal](caching-data-with-the-objectdatasource-cs/_static/image23.png))


[![Clicando o Postback botão faz com que o GridView para ser reconstruído com seu estado de exibição](caching-data-with-the-objectdatasource-cs/_static/image25.png)](caching-data-with-the-objectdatasource-cs/_static/image24.png)

**Figura 10**: clicar no botão Postback faz com que o GridView ser reconstruído com seu estado de exibição ([clique para exibir a imagem em tamanho normal](caching-data-with-the-objectdatasource-cs/_static/image26.png))


Pode parecer desnecessário recuperar o banco de dados sempre que os dados são paginados por meio ou classificados. Afinal, desde que re usando paginação padrão, o ObjectDataSource recuperou todos os registros ao exibir a primeira página. Mesmo que o GridView não fornece classificação e paginação suporte, os dados devem ser recuperados do banco de dados cada vez que a página for visitada primeiro por qualquer usuário (e em cada postback, se o estado de exibição estiver desabilitado). Mas se GridView está mostrando os mesmos dados para todos os usuários, essas solicitações de banco de dados extras são supérfluas. Por que não armazenar em cache os resultados retornados pelo `GetProducts()` método e ligação GridView aos resultados armazenados em cache?

## <a name="step-4-caching-the-data-using-the-objectdatasource"></a>Etapa 4: Armazenamento em cache os dados usando o ObjectDataSource

Ao simplesmente definir algumas propriedades, ObjectDataSource pode ser configurado para armazenar em cache os dados recuperados no cache de dados do ASP.NET automaticamente. A lista a seguir resume as propriedades relacionadas a cache do ObjectDataSource:

- [EnableCaching](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.enablecaching.aspx) deve ser definido como `true` para habilitar o cache. O padrão é `false`.
- [CacheDuration](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cacheduration.aspx) a quantidade de tempo, em segundos, que é armazenado em cache os dados. O padrão é 0. ObjectDataSource será somente dados armazenados em cache se `EnableCaching` é `true` e `CacheDuration` é definido como um valor maior que zero.
- [CacheExpirationPolicy](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cacheexpirationpolicy.aspx) pode ser definido como `Absolute` ou `Sliding`. Se `Absolute`, ObjectDataSource armazena em cache os dados recuperados para `CacheDuration` segundos; se `Sliding`, os dados expirarem somente depois que ele não tiver sido acessado para `CacheDuration` segundos. O padrão é `Absolute`.
- [CacheKeyDependency](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cachekeydependency.aspx) usar essa propriedade para associar as entradas de cache de s ObjectDataSource com uma dependência de cache existente. As entradas de dados s ObjectDataSource podem ser prematuramente removidas do cache por meio da expiração seus associados `CacheKeyDependency`. Esta propriedade é geralmente usada para associar uma dependência de cache SQL com o cache de s ObjectDataSource, um tópico, exploraremos futuramente [dependências de Cache de SQL usando](using-sql-cache-dependencies-cs.md) tutorial.

Permitir que o s configure o `ProductsDataSource` ObjectDataSource para armazenar em cache seus dados em uma escala absoluto de 30 segundos. Definir o s ObjectDataSource `EnableCaching` propriedade `true` e sua `CacheDuration` propriedade a 30. Deixe o `CacheExpirationPolicy` propriedade definida como padrão, `Absolute`.


[![Configurar o ObjectDataSource para armazenar seus dados em Cache para 30 segundos](caching-data-with-the-objectdatasource-cs/_static/image28.png)](caching-data-with-the-objectdatasource-cs/_static/image27.png)

**Figura 11**: configurar o ObjectDataSource para armazenar seus dados em Cache para 30 segundos ([clique para exibir a imagem em tamanho normal](caching-data-with-the-objectdatasource-cs/_static/image29.png))


Salve suas alterações e revise esta página em um navegador. O texto do evento disparado selecionando aparecerá quando visitar a página, como inicialmente os dados não estão no cache. Mas subsequentes postbacks disparados clicando no botão de Postback, classificação, paginação, ou clicando no botão Editar ou cancelar *não* reexibição o evento Selecting acionado texto. Isso ocorre porque o `Selecting` evento é acionado somente quando o ObjectDataSource obtém seus dados de seu objeto subjacente; o `Selecting` evento não será acionado se os dados são recuperados do cache de dados.

Depois de 30 segundos, os dados sejam removidos do cache. Os dados também serão removidos do cache se o s ObjectDataSource `Insert`, `Update`, ou `Delete` métodos forem invocados. Consequentemente, depois de 30 segundos ou o botão de atualização foi clicado, classificação, paginação, ou clicando no botão Editar ou Cancelar fará com que o ObjectDataSource obter os dados de seu objeto subjacente, exibir o evento Selecting acionado texto quando o `Selecting` evento ser acionado. Os resultados retornados são colocados de volta para o cache de dados.

> [!NOTE]
> Se você vir o texto do evento disparado selecionando com frequência, mesmo quando você espera que o ObjectDataSource para trabalhar com dados armazenados em cache, pode ser devido a restrições de memória. Se não houver memória suficiente disponível, os dados adicionados ao cache por ObjectDataSource podem ter foi eliminados. Se o ObjectDataSource parecer estar corretamente armazenando em cache os dados ou caches somente os dados esporadicamente, feche alguns aplicativos para liberar memória e tente novamente.


Figura 12 ilustra o s ObjectDataSource cache de fluxo de trabalho. Quando o evento Selecting acionado texto aparece na tela, isso ocorre porque os dados não estavam no cache e tinham de ser recuperado do objeto subjacente. Quando este texto está ausente, no entanto, ele s porque os dados disponíveis do cache. Quando os dados são retornados do cache lá s executadas nenhuma chamada para o objeto subjacente e, portanto, nenhuma consulta de banco de dados.


![Os repositórios de ObjectDataSource e recupera seus dados do Cache de dados](caching-data-with-the-objectdatasource-cs/_static/image30.png)

**Figura 12**: ObjectDataSource armazena e recupera os dados do Cache de dados


Cada aplicativo ASP.NET tem seu próprio cache de dados de instância que s compartilhados em todas as páginas e os visitantes. Isso significa que os dados armazenados no cache de dados por ObjectDataSource da mesma forma são compartilhados entre todos os usuários que visitam a página. Para verificar isso, abra o `ObjectDataSource.aspx` página em um navegador. Quando o primeiro visitando a página, o texto do evento disparado selecionando aparecerá (supondo que os dados adicionados ao cache por testes anteriores, agora, foi removidos). Abra uma segunda instância do navegador e copiar e colar a URL da primeira instância de navegador para o segundo. Na segunda instância do navegador, o texto do evento disparado selecionando não é mostrado porque ele s usando o mesmo em cache dados como o primeiro.

Ao inserir seus dados recuperados em cache, o ObjectDataSource usa um valor de chave de cache que inclui: o `CacheDuration` e `CacheExpirationPolicy` valores de propriedade; o tipo do objeto comercial subjacente que está sendo usado por ObjectDataSource, que é especificado via o [ `TypeName` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.typename.aspx) (`ProductsBLL`, neste exemplo); o valor da `SelectMethod` propriedade e o nome e os valores dos parâmetros do `SelectParameters` coleção; e os valores de suas `StartRowIndex`e `MaximumRows` propriedades, que são usadas durante a implementação [paginação personalizada.](../paging-and-sorting/paging-and-sorting-report-data-cs.md)

Criar o valor de chave do cache como uma combinação dessas propriedades garante uma entrada de cache exclusivo como alterar esses valores. Por exemplo, nos últimos tutoriais é var visto usando o `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)`, que retorna todos os produtos para uma categoria especificada. Um usuário pode vir para bebidas página e exibição, que tem um `CategoryID` de 1. Se o ObjectDataSource armazenado em cache os resultados sem levar em consideração o `SelectParameters` valores, quando outro usuário fornecida para a página Exibir Condimentos enquanto os produtos de bebidas no cache, d verem os produtos de bebidas em cache em vez de Condimentos. Variando a chave de cache por estas propriedades, que incluem os valores de `SelectParameters`, ObjectDataSource mantém uma entrada de cache separado para bebidas e Condimentos.

## <a name="stale-data-concerns"></a>Problemas de dados obsoletos

ObjectDataSource remove automaticamente seus itens do cache quando qualquer um dos seus `Insert`, `Update`, ou `Delete` métodos é invocado. Isso ajuda a proteger contra dados obsoletos limpando entradas de cache quando os dados são modificados por meio da página. No entanto, é possível que um ObjectDataSource usando o cache ainda exibir dados obsoletos. No caso mais simples, ele pode ser devido aos dados de alteração diretamente no banco de dados. Talvez um administrador de banco de dados acabou de executar um script que modifica alguns dos registros no banco de dados.

Este cenário também poderia Desdobrar de maneira mais sutil. Enquanto o ObjectDataSource remove seus itens do cache quando um dos seus métodos de modificação de dados é chamado, removidos os itens armazenados em cache são para a ObjectDataSource s determinada combinação de valores de propriedade (`CacheDuration`, `TypeName`, `SelectMethod`, e assim por diante). Se você tiver dois ObjectDataSources que usam diferentes `SelectMethods` ou `SelectParameters`, mas ainda poderá atualizar os mesmos dados, em seguida, um ObjectDataSource pode atualizar uma linha e invalidar suas próprias entradas de cache, mas a linha correspondente para o segundo ObjectDataSource ainda será servido do cache. Recomendo que você crie páginas para apresentar essa funcionalidade. Criar uma página que exibe um GridView editável que recebe os dados de um ObjectDataSource que usa o cache e é configurado para obter dados a partir de `ProductsBLL` classe s `GetProducts()` método. Adicione outro editável GridView e ObjectDataSource nesta página (ou outro), mas para este segundo ObjectDataSource têm-lo a usar o `GetProductsByCategoryID(categoryID)` método. Desde os duas ObjectDataSources `SelectMethod` propriedades forem diferentes, eles ll cada têm seus próprios valores armazenados em cache. Se você editar um produto em uma grade, na próxima vez que você vincular os dados de volta para a grade de (, paginação, classificação e assim por diante), ele ainda servir os dados antigos, armazenados em cache e não refletem a alteração foi feita da grade de.

Em resumo, só use Expirações do tempo se você estiver disposto a têm o potencial de dados obsoletos e use expirações mais curto para cenários em que a atualização de dados é importante. Se dados obsoletos não forem aceitáveis, abrir mão de cache ou usar as dependências de cache SQL (supondo que ele é o banco de dados estiver cache). Vamos explorar as dependências de cache do SQL em um tutorial futuras.

## <a name="summary"></a>Resumo

Neste tutorial, examinamos os recursos de cache ObjectDataSource s internos. Simplesmente definindo algumas propriedades, é possível instruir o ObjectDataSource para armazenar em cache os resultados retornados especificado `SelectMethod` no cache de dados do ASP.NET. O `CacheDuration` e `CacheExpirationPolicy` propriedades indicam a duração em que o item é armazenado em cache e se é um absoluto ou a expiração deslizante. O `CacheKeyDependency` propriedade todas as entradas de cache de s ObjectDataSource associa uma dependência de cache existente. Isso pode ser usado para remover as entradas de s ObjectDataSource do cache antes da expiração do tempo é atingida e é normalmente usada com dependências de cache SQL.

Desde o ObjectDataSource simplesmente armazena seus valores para o cache de dados, pode replicar a funcionalidade interna de s ObjectDataSource programaticamente. -T faz sentido fazer isso na camada de apresentação, desde que o ObjectDataSource oferece essa funcionalidade pronta, mas podemos implementar recursos de cache em uma camada separada da arquitetura. Para fazer isso, é necessário repetir a mesma lógica usada por ObjectDataSource. Exploraremos como trabalhar programaticamente com o cache de dados de dentro da arquitetura em nosso tutorial Avançar.

Boa programação!

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [O cache do ASP.NET: Técnicas e práticas recomendadas](https://msdn.microsoft.com/library/aa478965.aspx)
- [Guia de arquitetura de cache para aplicativos do .NET Framework](https://msdn.microsoft.com/library/ee817645.aspx)
- [Cache de saída no ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx)

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisor levar para este tutorial foi Teresa Murphy. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Avançar](caching-data-in-the-architecture-cs.md)
