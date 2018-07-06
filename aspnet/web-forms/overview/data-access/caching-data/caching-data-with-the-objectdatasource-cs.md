---
uid: web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-cs
title: Cache de dados com o ObjectDataSource (c#) | Microsoft Docs
author: rick-anderson
description: Armazenamento em cache pode significar a diferença entre lento e rápido de um aplicativo Web. Este tutorial é a primeira das quatro que dê uma olhada detalhada de cache no ASP.NET...
ms.author: aspnetcontent
ms.date: 05/30/2007
ms.assetid: bd87413c-8160-4520-a8a2-43b555c4183a
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: f6ca84dc11eb8ecd03aee91198e74b723cfdce7c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803039"
---
<a name="caching-data-with-the-objectdatasource-c"></a>Cache de dados com o ObjectDataSource (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_58_CS.exe) ou [baixar PDF](caching-data-with-the-objectdatasource-cs/_static/datatutorial58cs1.pdf)

> Armazenamento em cache pode significar a diferença entre lento e rápido de um aplicativo Web. Este tutorial é a primeira das quatro que dê uma olhada detalhada de cache no ASP.NET. Aprenda os principais conceitos de armazenamento em cache e como aplicar o cache para a camada de apresentação por meio do controle ObjectDataSource.


## <a name="introduction"></a>Introdução

Na ciência da computação, *caching* é o processo de pegar dados ou informações que é caras para obter e armazenar uma cópia dele em um local que é mais rápido de acesso. Para aplicativos orientados a dados, consultas grandes e complexas normalmente consomem a maior parte do tempo de execução do aplicativo s. Tal um s desempenho do aplicativo, em seguida, geralmente pode ser melhorado, armazenando os resultados das consultas caras de banco de dados na memória do aplicativo de s.

O ASP.NET 2.0 oferece uma variedade de opções de cache. Uma página da web inteira ou a marcação de s renderizado do controle de usuário pode ser armazenados em cache por meio *cache de saída*. Os controles ObjectDataSource e SqlDataSource fornecem recursos de cache também e, portanto, permitindo que os dados sejam armazenados em cache no nível de controle. E o ASP.NET s *cache de dados* fornece uma API de cache avançada que permite que os desenvolvedores de páginas por meio de programação de objetos no cache. Este tutorial e os próximos três que nós examinaremos usando o s ObjectDataSource cache recursos, bem como o cache de dados. Também exploraremos como armazenar em cache dados de todo o aplicativo na inicialização e como manter os dados armazenados em cache atualizado com o uso de dependências de cache SQL. Esses tutoriais exploram não o cache de saída. Para obter uma visão detalhada de cache de saída, consulte [cache de saída no ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx).

Armazenamento em cache pode ser aplicado em qualquer lugar na arquitetura, da camada de acesso de dados de backup por meio da camada de apresentação. Neste tutorial, examinaremos a aplicação de cache para a camada de apresentação por meio do controle ObjectDataSource. No próximo tutorial, que vamos examinar em cache os dados na camada de lógica de negócios.

## <a name="key-caching-concepts"></a>Conceitos de cache de chave

Armazenamento em cache pode melhorar muito um aplicativo s geral de desempenho e escalabilidade utilizando dados que é caros gerar e armazenar uma cópia dele em um local que pode ser acessado com mais eficiência. Uma vez que o cache contém apenas uma cópia dos dados subjacentes, reais, podem ficar desatualizado, ou *obsoletos*, se os dados subjacentes forem alterados. Para combater isso, um desenvolvedor de páginas pode indicar que os critérios pelos quais o item de cache serão *removidos* do cache, usando:

- **Critérios de tempo** um item pode ser adicionado ao cache para absoluta ou deslizante de duração. Por exemplo, um desenvolvedor de páginas pode indicar uma duração de, digamos, 60 segundos. Com uma duração absoluta, o item em cache seja removido 60 segundos depois que ele foi adicionado ao cache, independentemente da frequência com a qual ele foi acessado. Com uma duração deslizante, o item em cache seja removido 60 segundos depois do último acesso.
- **Critérios com base na dependência** uma dependência pode ser associada um item quando é adicionado ao cache. Quando altera a dependência de item s é removido do cache. A dependência pode ser um arquivo, outro item do cache ou uma combinação dos dois. O ASP.NET 2.0 também permite que as dependências de cache do SQL, que permitem aos desenvolvedores adicionar um item ao cache e que ele seja removido quando o banco de dados subjacente é alterado. Vamos examinar as dependências de cache SQL na próxima [dependências de Cache de SQL usando](using-sql-cache-dependencies-cs.md) tutorial.

Os critérios de remoção especificados, independentemente de um item no cache pode ser *eliminado* antes que os critérios com base no tempo ou em dependência foi atendida. Se o cache atingiu sua capacidade, os itens existentes devem ser removidos antes que novos podem ser adicionados. Consequentemente, ao programaticamente trabalhar com dados armazenados em cache-s vitais que você sempre pressupõe que os dados em cache não podem estar presentes. Vamos examinar o padrão a ser usado ao acessar dados do cache por meio de programação em nosso próximo tutorial *dados em cache na arquitetura*.

Armazenamento em cache fornece uma maneira econômica para apertando mais o desempenho de um aplicativo. Como [Steven Smith](http://aspadvice.com/blogs/ssmith/) Enfatize em seu artigo [o cache do ASP.NET: técnicas e práticas recomendadas](https://msdn.microsoft.com/library/aa478965.aspx):

Armazenamento em cache pode ser uma boa maneira de obter bom desempenho suficiente sem exigir muito tempo e análise. A memória é barata, portanto, se você pode obter o desempenho armazenando em cache a saída para 30 segundos em vez de gastar um dia ou uma semana tentando otimizar seu código ou o banco de dados necessário, faça a solução de cache (supondo que os dados 30 - antigos de segundo é okey) e prosseguir. Eventualmente, design inadequado será provavelmente alcançada para você, portanto, claro, você deve tentar criar seus aplicativos corretamente. Mas se você apenas precisar obter bom desempenho suficiente hoje em dia, o cache pode ser uma maneira excelente de [], comprando tempo refatorar seu aplicativo em uma data posterior, quando você tem tempo para fazer isso.

Enquanto o cache pode fornecer aprimoramentos de desempenho considerável, ela não é aplicável em todas as situações, como com aplicativos que usam dados em tempo real, atualizando com frequência, ou onde os dados obsoletos, mesmo em breve vivia são inaceitáveis. Mas, para a maioria dos aplicativos, armazenamento em cache deve ser usado. Para obter mais informações sobre armazenamento em cache no ASP.NET 2.0, consulte o [cache para desempenho](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/default.aspx) seção o [tutoriais de início rápido do ASP.NET 2.0](https://quickstarts.asp.net/QuickStartv20/aspnet/).

## <a name="step-1-creating-the-caching-web-pages"></a>Etapa 1: Criando páginas da Web do cache

Antes de começarmos a nossa exploração dos recursos de cache s ObjectDataSource, permitem que s levar alguns instantes para criar as páginas do ASP.NET em nosso projeto de site que precisaremos para este tutorial e os próximos três pela primeira vez. Comece adicionando uma nova pasta chamada `Caching`. Em seguida, adicione as seguintes páginas do ASP.NET para essa pasta, certificando-se de associar cada página com o `Site.master` página mestra:

- `Default.aspx`
- `ObjectDataSource.aspx`
- `FromTheArchitecture.aspx`
- `AtApplicationStartup.aspx`
- `SqlCacheDependencies.aspx`


![Adicione as páginas do ASP.NET para que os tutoriais relacionados ao armazenamento em cache](caching-data-with-the-objectdatasource-cs/_static/image1.png)

**Figura 1**: adicionar as páginas do ASP.NET para que os tutoriais relacionados ao armazenamento em cache


Como em outras pastas `Default.aspx` no `Caching` pasta listará os tutoriais em sua seção. Lembre-se de que o `SectionLevelTutorialListing.ascx` controle de usuário fornece essa funcionalidade. Portanto, adicionar esse controle de usuário `Default.aspx` arrastando-no Gerenciador de soluções para a página de exibição de Design de s.


[![Figura 2: Adicionar o controle de usuário SectionLevelTutorialListing.ascx para default. aspx](caching-data-with-the-objectdatasource-cs/_static/image3.png)](caching-data-with-the-objectdatasource-cs/_static/image2.png)

**Figura 2**: a Figura 2: adicionar a `SectionLevelTutorialListing.ascx` controle de usuário `Default.aspx` ([clique para exibir a imagem em tamanho normal](caching-data-with-the-objectdatasource-cs/_static/image4.png))


Por fim, adicione essas páginas como entradas para o `Web.sitemap` arquivo. Especificamente, adicione a marcação a seguir após o trabalho com dados binários `<siteMapNode>`:


[!code-xml[Main](caching-data-with-the-objectdatasource-cs/samples/sample1.xml)]

Depois de atualizar `Web.sitemap`, reserve um tempo para exibir o site de tutoriais através de um navegador. No menu à esquerda agora inclui itens para os tutoriais do cache.


![O mapa do Site agora inclui entradas para os tutoriais do cache](caching-data-with-the-objectdatasource-cs/_static/image5.png)

**Figura 3**: O mapa do Site agora inclui entradas para os tutoriais do cache


## <a name="step-2-displaying-a-list-of-products-in-a-web-page"></a>Etapa 2: Exibindo uma lista de produtos em uma página da Web

Este tutorial explora como usar os ObjectDataSource controle s internos recursos de cache. Antes que possamos observar esses recursos, no entanto, primeiro precisamos de uma página para trabalhar da. Let s criar uma página da web que usa uma GridView para listar as informações de produto recuperadas por um ObjectDataSource do `ProductsBLL` classe.

Comece abrindo o `ObjectDataSource.aspx` página o `Caching` pasta. Arraste um controle GridView na caixa de ferramentas para o Designer, defina suas `ID` propriedade para `Products`e, na marca inteligente, de escolha para associá-lo para um novo controle ObjectDataSource chamado `ProductsDataSource`. Configurar o ObjectDataSource para trabalhar com o `ProductsBLL` classe.


[![Configurar o ObjectDataSource para usar a classe ProductsBLL](caching-data-with-the-objectdatasource-cs/_static/image7.png)](caching-data-with-the-objectdatasource-cs/_static/image6.png)

**Figura 4**: configurar o ObjectDataSource para usar o `ProductsBLL` classe ([clique para exibir a imagem em tamanho normal](caching-data-with-the-objectdatasource-cs/_static/image8.png))


Para esta página, deixe s criar um GridView editável para que podemos examinar o que acontece quando os dados armazenados em cache em ObjectDataSource são modificados por meio da interface de s GridView. Deixe a lista suspensa na guia selecione Definir como seu padrão `GetProducts()`, mas alterar o item selecionado na guia de atualização para o `UpdateProduct` sobrecarga que aceita `productName`, `unitPrice`, e `productID` como seus parâmetros de entrada.


[![Defina a lista de lista suspensa de s de guia de atualização para a sobrecarga apropriada UpdateProduct](caching-data-with-the-objectdatasource-cs/_static/image10.png)](caching-data-with-the-objectdatasource-cs/_static/image9.png)

**Figura 5**: defina o guia de atualização de s lista suspensa para a adequadas `UpdateProduct` sobrecarregar ([clique para exibir a imagem em tamanho normal](caching-data-with-the-objectdatasource-cs/_static/image11.png))


Por fim, defina as listas suspensas nas guias INSERT e DELETE como (nenhum) e clique em Concluir. Após a conclusão do Assistente Configurar fonte de dados, o Visual Studio define o s ObjectDataSource `OldValuesParameterFormatString` propriedade para `original_{0}`. Conforme discutido na [uma visão geral de inserção de, atualizando e excluindo dados](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) tutorial, essa propriedade precisa ser removido da sintaxe declarativa ou definidas para seu valor padrão, `{0}`, para que nosso fluxo de trabalho de atualização para continue sem erro.

Além disso, após a conclusão do Assistente para Visual Studio adiciona um campo para o GridView para cada um dos campos de dados do produto. Remover tudo, exceto os `ProductName`, `CategoryName`, e `UnitPrice` BoundFields. Em seguida, atualize o `HeaderText` propriedades de cada um desses BoundFields produto, categoria e preço, respectivamente. Uma vez que o `ProductName` campo é obrigatório, converter o BoundField em um TemplateField e adicionar um RequiredFieldValidator para o `EditItemTemplate`. Da mesma forma, converta o `UnitPrice` BoundField em um TemplateField e adicione um CompareValidator para garantir que o valor inserido pelo usuário é o valor de uma moeda válida que s maior que ou igual a zero. Além dessas alterações, fique à vontade realizar quaisquer alterações estéticas, como alinhamento à direita do `UnitPrice` valor ou especificar a formatação para o `UnitPrice` texto em suas interfaces somente leitura e edição.

Tornar o GridView editável, marcando a caixa de seleção Habilitar edição na marca inteligente s GridView. Além disso, verifique as caixas de seleção Habilitar paginação e habilitar a classificação.

> [!NOTE]
> Precisa de uma revisão de como personalizar a interface de edição de GridView s? Nesse caso, consulte o [Personalizando a Interface de modificação de dados](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) tutorial.


[![Habilitar o suporte de GridView para edição, classificação e paginação](caching-data-with-the-objectdatasource-cs/_static/image13.png)](caching-data-with-the-objectdatasource-cs/_static/image12.png)

**Figura 6**: habilitar o suporte de GridView para edição, classificação e paginação ([clique para exibir a imagem em tamanho normal](caching-data-with-the-objectdatasource-cs/_static/image14.png))


Depois de fazer essas modificações de GridView, GridView e ObjectDataSource s marcação declarativa deve ser semelhante ao seguinte:


[!code-aspx[Main](caching-data-with-the-objectdatasource-cs/samples/sample2.aspx)]

Como mostra a Figura 7, o GridView editável lista o nome, categoria e preço de cada um dos produtos no banco de dados. Reserve um tempo para testar a classificação de funcionalidade s de página a página por eles, resultados e editar um registro.


[![Cada produto s nome, categoria e preço está listado em um classificável, Pageable, GridView editável](caching-data-with-the-objectdatasource-cs/_static/image16.png)](caching-data-with-the-objectdatasource-cs/_static/image15.png)

**Figura 7**: cada produto s nome, categoria e preço está listado em um classificável, Pageable, GridView editável ([clique para exibir a imagem em tamanho normal](caching-data-with-the-objectdatasource-cs/_static/image17.png))


## <a name="step-3-examining-when-the-objectdatasource-is-requesting-data"></a>Etapa 3: Examinar quando ObjectDataSource o está solicitando dados

O `Products` GridView recupera seus dados a serem exibidos, invocando o `Select` método o `ProductsDataSource` ObjectDataSource. Este ObjectDataSource cria uma instância de s a camada de lógica comercial `ProductsBLL` classe e chama seu `GetProducts()` método, que por sua vez chama a camada de acesso a dados s `ProductsTableAdapter` s `GetProducts()` método. O método DAL se conecta ao banco de dados Northwind e emite configurado `SELECT` consulta. Esses dados são retornados, em seguida, a DAL, que empacota em um `NorthwindDataTable`. O objeto DataTable é retornado para a BLL, que retorna para o ObjectDataSource, que retorna ao GridView. O GridView, em seguida, cria uma `GridViewRow` para cada objeto `DataRow` na DataTable e cada `GridViewRow` , eventualmente, ser renderizado em HTML que é retornada ao cliente e exibido no navegador do visitante s.

Essa sequência de eventos ocorre cada vez que o GridView precisa se associar aos seus dados subjacentes. Isso acontece quando a página é visitada primeiro, ao mover de uma página de dados para outro, ao classificar o GridView, ou ao modificar dados s GridView por meio de seu interno, editar ou excluir interfaces. Se o estado de exibição GridView s estiver desabilitado, o GridView será sejam associadas novamente em cada postback também. O GridView pode também ser explicitamente se a seus dados, chamando seu `DataBind()` método.

Para apreciar totalmente a frequência com a qual os dados são recuperados do banco de dados, deixe s exibirá uma mensagem indicando quando os dados estão sendo recuperados novamente. Adicionar um controle de Web de rótulo acima GridView chamado `ODSEvents`. Limpar seus `Text` propriedade e defina sua `EnableViewState` propriedade para `false`. Sob o rótulo, adicione um controle da Web de botão e defina seu `Text` propriedade para Postback.


[![Adicione um rótulo e um botão à página acima GridView](caching-data-with-the-objectdatasource-cs/_static/image19.png)](caching-data-with-the-objectdatasource-cs/_static/image18.png)

**Figura 8**: adicionar um rótulo e um botão para a página acima. o controle GridView ([clique para exibir a imagem em tamanho normal](caching-data-with-the-objectdatasource-cs/_static/image20.png))


Durante o fluxo de trabalho de acesso de dados, o s ObjectDataSource `Selecting` evento é acionado antes que o objeto subjacente é criado e seu método configurado invocado. Crie um manipulador de eventos para esse evento e adicione o seguinte código:


[!code-csharp[Main](caching-data-with-the-objectdatasource-cs/samples/sample3.cs)]

Cada vez que o ObjectDataSource faz uma solicitação para a arquitetura de dados, o rótulo exibirá o evento de seleção de texto acionado.

Visite essa página em um navegador. Quando a página é visitada primeiro, o evento de seleção de texto acionado é mostrado. Clique no botão de Postback e observe que o texto desaparece (supondo que o s GridView `EnableViewState` estiver definida como `true`, o padrão). Isso ocorre porque, no postback, GridView é reconstruído com seu estado de exibição e, portanto, t ativar a ObjectDataSource para seus dados. Classificação, paginação ou edição de dados, no entanto, faz com que o GridView reassociar a fonte de dados e, portanto, o evento Selecting acionado texto reaparece.


[![Sempre que o GridView é reassociado para sua fonte de dados, selecionar evento disparado é exibido](caching-data-with-the-objectdatasource-cs/_static/image22.png)](caching-data-with-the-objectdatasource-cs/_static/image21.png)

**Figura 9**: sempre que o controle GridView é Reassociada para sua fonte de dados, selecionar evento disparado é exibido ([clique para exibir a imagem em tamanho normal](caching-data-with-the-objectdatasource-cs/_static/image23.png))


[![Clicar o Postback botão faz com que o GridView para ser reconstruído com seu estado de exibição](caching-data-with-the-objectdatasource-cs/_static/image25.png)](caching-data-with-the-objectdatasource-cs/_static/image24.png)

**Figura 10**: clicando no botão de Postback faz com que o GridView ser reconstruído com seu estado de exibição ([clique para exibir a imagem em tamanho normal](caching-data-with-the-objectdatasource-cs/_static/image26.png))


Pode parecer inútil recuperar o banco de dados sempre que os dados são paginados por meio de ou classificados. Afinal de contas, desde que criamos re usando paginação padrão, o ObjectDataSource recuperou todos os registros ao exibir a primeira página. Mesmo que o GridView não fornece classificação e paginação de suporte, os dados devem ser recuperados do banco de dados cada vez que a página é visitada primeiro por qualquer usuário (e em cada postback, se o estado de exibição estiver desabilitado). Mas se o GridView está mostrando os mesmos dados para todos os usuários, essas solicitações de banco de dados extra supérfluas. Por que não armazenar em cache os resultados retornados do `GetProducts()` método e associar o GridView aos resultados armazenados em cache?

## <a name="step-4-caching-the-data-using-the-objectdatasource"></a>Etapa 4: Armazenamento em cache os dados usando o ObjectDataSource

Simplesmente definindo algumas propriedades, o ObjectDataSource pode ser configurado para armazenar em cache automaticamente seus dados recuperados no cache de dados do ASP.NET. A lista a seguir resume as propriedades relacionadas ao cache do ObjectDataSource:

- [EnableCaching](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.enablecaching.aspx) deve ser definida como `true` para habilitar o cache. O padrão é `false`.
- [CacheDuration](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cacheduration.aspx) a quantidade de tempo, em segundos, que é armazenado em cache os dados. O padrão é 0. O ObjectDataSource será apenas armazenar dados em cache se `EnableCaching` está `true` e `CacheDuration` é definido como um valor maior que zero.
- [CacheExpirationPolicy](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cacheexpirationpolicy.aspx) pode ser definido como `Absolute` ou `Sliding`. Se `Absolute`, o ObjectDataSource armazena em cache os dados recuperados para `CacheDuration` segundos; se `Sliding`, os dados expirarem somente depois que ele não tiver sido acessado para `CacheDuration` segundos. O padrão é `Absolute`.
- [CacheKeyDependency](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cachekeydependency.aspx) usar essa propriedade para associar as entradas de cache de s ObjectDataSource com uma dependência de cache existente. As entradas de dados do ObjectDataSource s podem ser prematuramente removidas do cache por meio da expiração de seus associados `CacheKeyDependency`. Essa propriedade é mais comumente usada para associar uma dependência de cache SQL com o cache de s ObjectDataSource, um tópico, exploraremos futuramente [dependências de Cache de SQL usando](using-sql-cache-dependencies-cs.md) tutorial.

Permitir que o s configure o `ProductsDataSource` ObjectDataSource para armazenar em cache seus dados por 30 segundos em uma escala absoluta. Definir o s ObjectDataSource `EnableCaching` propriedade para `true` e seu `CacheDuration` propriedade para 30. Deixe o `CacheExpirationPolicy` propriedade definida como seu padrão, `Absolute`.


[![Configurar o ObjectDataSource para armazenar seus dados em Cache por 30 segundos](caching-data-with-the-objectdatasource-cs/_static/image28.png)](caching-data-with-the-objectdatasource-cs/_static/image27.png)

**Figura 11**: configurar o ObjectDataSource para armazenar seus dados em Cache por 30 segundos ([clique para exibir a imagem em tamanho normal](caching-data-with-the-objectdatasource-cs/_static/image29.png))


Salve suas alterações e examine esta página em um navegador. O texto do evento disparado selecionando aparecerá quando visitar a página, pois inicialmente os dados não estão no cache. Mas postbacks subsequentes disparados ao clicar no botão de Postback, classificação, paginação, ou clicando nos botões de edição ou Cancelar faz *não* nova exibição de evento Selecting acionado texto. Isso ocorre porque o `Selecting` evento só é acionado quando o ObjectDataSource obtém seus dados do seu objeto subjacente; o `Selecting` evento não é disparado se os dados são recuperados do cache de dados.

Após 30 segundos, os dados serão removidos do cache. Os dados também serão removidos do cache se o s ObjectDataSource `Insert`, `Update`, ou `Delete` métodos são invocados. Consequentemente, após 30 segundos ou o botão de atualização foi clicado, classificação, paginação, ou clicando nos botões de edição ou o cancelamento fará com que o ObjectDataSource colocar os dados de seu objeto subjacente, exibir o evento Selecting disparadas texto quando o `Selecting` evento é acionado. Os resultados retornados são colocados de volta para o cache de dados.

> [!NOTE]
> Se você vir o texto do evento disparado selecionando com frequência, mesmo quando você espera que o ObjectDataSource para trabalhar com dados armazenados em cache, pode ser devido a restrições de memória. Se não houver memória suficiente livre, os dados adicionados ao cache por ObjectDataSource podem ter foi eliminados. Se t ObjectDataSource parece estar corretamente armazenando em cache os dados ou somente caches de dados esporadicamente, feche alguns aplicativos para liberar memória e tente novamente.


Figura 12 ilustra o s ObjectDataSource cache de fluxo de trabalho. Quando o evento Selecting disparado texto aparece na tela, é porque os dados não estavam no cache e precisava ser recuperada do objeto subjacente. Quando este texto está ausente, no entanto, ele s porque os dados estavam disponíveis do cache. Quando os dados são retornados do cache lá s executadas nenhuma chamada para o objeto subjacente e, portanto, nenhuma consulta de banco de dados.


![O ObjectDataSource armazena e recupera seus dados do Cache de dados](caching-data-with-the-objectdatasource-cs/_static/image30.png)

**Figura 12**: O ObjectDataSource armazena e recupera os dados do Cache de dados


Cada aplicativo ASP.NET tem seu próprio cache de dados da instância que s compartilhados entre todas as páginas e os visitantes. Isso significa que os dados armazenados no cache de dados por ObjectDataSource da mesma forma são compartilhados entre todos os usuários que visitam a página. Para verificar isso, abra o `ObjectDataSource.aspx` página em um navegador. Quando o primeiro visitando a página, o texto do evento disparado selecionando aparecerá (supondo que os dados adicionados ao cache por testes anteriores, agora, foi removidos). Abra uma segunda instância do navegador e copie e cole a URL da primeira instância de navegador para o segundo. Na segunda instância do navegador, o texto do evento disparado selecionando não é mostrado porque ela s usando o mesmo em cache os dados como o primeiro.

Ao inserir seus dados recuperados em cache, o ObjectDataSource usa um valor de chave de cache que inclui: o `CacheDuration` e `CacheExpirationPolicy` valores de propriedade; o tipo de objeto comercial subjacente que está sendo usado por ObjectDataSource, que é especificado por meio do [ `TypeName` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.typename.aspx) (`ProductsBLL`, neste exemplo); o valor da `SelectMethod` propriedade e o nome e os valores dos parâmetros no `SelectParameters` coleção; e os valores das suas `StartRowIndex`e `MaximumRows` propriedades, que são usadas ao implementar [paginação personalizada.](../paging-and-sorting/paging-and-sorting-report-data-cs.md)

Criar o valor de chave de cache como uma combinação dessas propriedades garante uma entrada de cache exclusivo como esses valores são alterados. Por exemplo, nos tutoriais anteriores, ve examinou usando o `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)`, que retorna todos os produtos para uma categoria especificada. Um usuário pode vir para bebidas página e exibição, que tem um `CategoryID` de 1. Se o ObjectDataSource armazenado em cache seus resultados sem levar em consideração o `SelectParameters` valores, quando veio de outro usuário para a página Exibir Condimentos enquanto os produtos de bebidas estava em cache, d veem a produtos de bebidas armazenados em cache em vez de Condimentos. Variando a chave de cache por essas propriedades, que incluem os valores da `SelectParameters`, o ObjectDataSource mantém uma entrada de cache separado para bebidas e Condimentos.

## <a name="stale-data-concerns"></a>Preocupações de dados obsoletos

O ObjectDataSource remove automaticamente seus itens do cache quando qualquer um dos seus `Insert`, `Update`, ou `Delete` métodos é invocado. Isso ajuda a proteger contra dados obsoletos, limpando as entradas de cache quando os dados são modificados por meio da página. No entanto, é possível que um ObjectDataSource usando o cache ainda exibir dados obsoletos. No caso mais simples, ele pode ser devido a dados alterando diretamente no banco de dados. Talvez um administrador de banco de dados acaba de executar um script que modifica alguns dos registros no banco de dados.

Este cenário também poderia Desdobrar de forma mais sutil. Embora o ObjectDataSource remove seus itens do cache quando um de seus métodos de modificação de dados é chamado, removidos os itens armazenados em cache são para a ObjectDataSource s determinada combinação de valores de propriedade (`CacheDuration`, `TypeName`, `SelectMethod`, e assim por diante). Se você tiver dois ObjectDataSources que usam diferentes `SelectMethods` ou `SelectParameters`, mas ainda poderá atualizar os mesmos dados, em seguida, um ObjectDataSource pode atualizar uma linha e invalidar suas própria entradas de cache, mas a linha correspondente para o segundo ObjectDataSource ainda será servido do cache. Eu recomendo que você crie páginas para apresentar essa funcionalidade. Criar uma página que exibe um GridView editável que recebe os dados de um ObjectDataSource que usa o cache e é configurado para obter dados do `ProductsBLL` classe s `GetProducts()` método. Adicione outro ObjectDataSource para essa página (ou outro), mas para este segundo ObjectDataSource e GridView editável que ele use o `GetProductsByCategoryID(categoryID)` método. Desde os duas ObjectDataSources `SelectMethod` propriedades forem diferentes, eles ll cada têm seus próprios valores armazenados em cache. Se você editar um produto em uma grade, na próxima vez que você associar os dados para a grade de outra (por paginação, classificação e assim por diante), ele ainda servir os dados armazenados em cache, antigos e não refletir a alteração foi feita da grade de.

Em resumo, apenas use expirações de tempo com base em se você estiver disposto a têm o potencial de dados obsoletos e usar expirações mais curtas para cenários em que a atualização de dados é importante. Se os dados obsoletos não forem aceitáveis, abrir mão de cache ou usar as dependências de cache SQL (supondo que ele é o banco de dados você está de cache). Vamos explorar as dependências de cache SQL em um tutorial futuro.

## <a name="summary"></a>Resumo

Neste tutorial, examinamos os recursos de cache ObjectDataSource s internos. Simplesmente definindo algumas propriedades, podemos pode instruir o ObjectDataSource para armazenar em cache os resultados retornados da especificado `SelectMethod` no cache de dados do ASP.NET. O `CacheDuration` e `CacheExpirationPolicy` propriedades indicam a duração em que o item é armazenado em cache e seja absoluta ou a expiração deslizante. O `CacheKeyDependency` propriedade todas as entradas de cache do ObjectDataSource s associa uma dependência de cache existente. Isso pode ser usado para remover as entradas de s ObjectDataSource do cache antes da expiração do tempo for atingida e normalmente é usada com dependências de cache SQL.

Uma vez que o ObjectDataSource simplesmente armazena em cache seus valores para o cache de dados, podemos, replique a funcionalidade interna do ObjectDataSource s programaticamente. -T faz sentido fazer isso na camada de apresentação, pois o ObjectDataSource oferece essa funcionalidade fora da caixa, mas podemos implementar recursos de cache em uma camada separada da arquitetura. Para fazer isso, é necessário repetir a mesma lógica usada pelo ObjectDataSource. Vamos explorar como trabalhar programaticamente com o cache de dados de dentro da arquitetura em nosso próximo tutorial.

Boa programação!

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [O cache do ASP.NET: Técnicas e práticas recomendadas](https://msdn.microsoft.com/library/aa478965.aspx)
- [Guia de arquitetura de armazenamento em cache para aplicativos do .NET Framework](https://msdn.microsoft.com/library/ee817645.aspx)
- [Cache de saída no ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx)

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Revisor de avanço para este tutorial foi Teresa Murphy. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Avançar](caching-data-in-the-architecture-cs.md)
