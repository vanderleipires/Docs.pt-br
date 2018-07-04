---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-vb
title: Consultando dados com o controle SqlDataSource (VB) | Microsoft Docs
author: rick-anderson
description: Nos tutoriais anteriores, usamos o controle ObjectDataSource para separar totalmente a camada de apresentação da camada de acesso a dados. Iniciando com este tutorial...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: b12f752d-3502-40a4-b695-fc7b7d08cfd3
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-vb
msc.type: authoredcontent
ms.openlocfilehash: d7ec182325d609877ac0d3603a5124b5fbe5a229
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365705"
---
<a name="querying-data-with-the-sqldatasource-control-vb"></a>Consultando dados com o controle SqlDataSource (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_47_VB.exe) ou [baixar PDF](querying-data-with-the-sqldatasource-control-vb/_static/datatutorial47vb1.pdf)

> Nos tutoriais anteriores, usamos o controle ObjectDataSource para separar totalmente a camada de apresentação da camada de acesso a dados. Estamos começando com este tutorial, saiba como o controle SqlDataSource pode ser usado para aplicativos simples que não exigem uma rigorosa separação da apresentação e acesso a dados.


## <a name="introduction"></a>Introdução

Todos os tutoriais, ve examinei até agora tem usado uma arquitetura em camadas que consiste de apresentação, lógica de negócios e as camadas de acesso a dados. A camada de acesso de dados (DAL) foi criado no primeiro tutorial ([criando uma camada de acesso de dados](../introduction/creating-a-data-access-layer-vb.md)) e a camada de lógica de negócios no segundo ([criando uma camada de lógica de negócios](../introduction/creating-a-business-logic-layer-vb.md)). Começando com o [exibindo dados com o ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md) tutorial vimos como usar o controle do ASP.NET 2.0 s novo ObjectDataSource para declarativamente a interface com a arquitetura da camada de apresentação.

Embora todos os tutoriais até o momento tem usado a arquitetura para trabalhar com dados, também é possível acessar, inserir, atualizar e excluir dados de banco de dados diretamente de uma página ASP.NET, ignorando a arquitetura. Isso coloca as consultas de banco de dados específico e a lógica de negócios diretamente na página da web. Para aplicativos suficientemente grandes ou complexos, desenvolvendo, implementando e usando uma arquitetura em camadas são extremamente importante para o sucesso, a capacidade de atualização e a facilidade de manutenção do aplicativo. Desenvolver uma arquitetura robusta, no entanto, pode ser desnecessário durante a criação de aplicativos extremamente simples e únicos.

O ASP.NET 2.0 fornece controles de fonte de dados internos cinco [SqlDataSource](https://msdn.microsoft.com/library/dz12d98w%28vs.80%29.aspx), [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx), [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), [XmlDataSource](https://msdn.microsoft.com/library/e8d8587a%28en-US,VS.80%29.aspx), e [SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x%28en-US,VS.80%29.aspx). O SqlDataSource pode ser usado para acessar e modificar dados diretamente de um banco de dados relacional, incluindo o Microsoft SQL Server, Microsoft Access, Oracle, MySQL e outros. Este tutorial e os próximos três, examinaremos como trabalhar com o controle SqlDataSource, explorar como consultar e filtrar dados do banco de dados, bem como usar o SqlDataSource para inserir, atualizar e excluir dados.


![O ASP.NET 2.0 inclui cinco controles de fonte de dados internos](querying-data-with-the-sqldatasource-control-vb/_static/image1.gif)

**Figura 1**: ASP.NET 2.0 inclui cinco controles de fonte de dados internos


## <a name="comparing-the-objectdatasource-and-sqldatasource"></a>Comparação entre o ObjectDataSource e SqlDataSource

Conceitualmente, o ObjectDataSource e SqlDataSource controles são simplesmente os proxies aos dados. Conforme discutido na [exibindo dados com o ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md) tutorial, o ObjectDataSource tem propriedades que indicam o tipo de objeto que fornece os dados e os métodos para invocar para selecionar, inserir, atualizar e excluir dados do tipo de objeto subjacente. Após configurar as propriedades de s ObjectDataSource, um controle de Web como um GridView, DetailsView ou DataList de dados podem ser associados ao controle, usando o s ObjectDataSource `Select()`, `Insert()`, `Delete()`, e `Update()` métodos para interagir com a arquitetura subjacente.

O SqlDataSource fornece a mesma funcionalidade, mas opera em relação a um banco de dados relacional em vez de uma biblioteca de objetos. Com o SqlDataSource, podemos deve especificar a cadeia de caracteres de conexão de banco de dados e consultas SQL ad hoc ou procedimentos armazenados para executar para inserir, atualizar, excluir e recuperar dados. O s SqlDataSource `Select()`, `Insert()`, `Update()`, e `Delete()` métodos, quando invocada, conecte-se ao banco de dados especificado e emita a consulta SQL apropriada. Como ilustra o diagrama a seguir, esses métodos fazem o trabalho pesado de se conectar a um banco de dados, emitindo uma consulta e retornar os resultados.


![O SqlDataSource serve como um Proxy para o banco de dados](querying-data-with-the-sqldatasource-control-vb/_static/image2.gif)

**Figura 2**: SqlDataSource serve como um Proxy para o banco de dados


> [!NOTE]
> Neste tutorial, nos concentraremos na recuperação dos dados do banco de dados. No [inserindo, atualizando e excluindo dados com o controle SqlDataSource](inserting-updating-and-deleting-data-with-the-sqldatasource-vb.md) tutorial, veremos como configurar o SqlDataSource para dar suporte à inserção, atualização e exclusão.


## <a name="the-sqldatasource-and-accessdatasource-controls"></a>Os controles de AccessDataSource e SqlDataSource

Além do controle SqlDataSource, o ASP.NET 2.0 também inclui um controle AccessDataSource. Esses dois controles diferentes levam muitos desenvolvedores novos no ASP.NET 2.0 suspeitar que o controle AccessDataSource foi projetado para trabalhar exclusivamente com o Microsoft Access com o controle SqlDataSource projetado para trabalhar exclusivamente com o Microsoft SQL Server. Enquanto o AccessDataSource foi projetado para trabalhar especificamente com o Microsoft Access, o controle SqlDataSource funciona com *qualquer* banco de dados relacional que pode ser acessado por meio do .NET. Isso inclui qualquer compatível com OLE DB ou ODBC armazenamentos de dados, como Microsoft Access, Microsoft SQL Server, Informix, Oracle, MySQL e PostgreSQL, entre muitas outras.

A única diferença entre os controles AccessDataSource e SqlDataSource é como as informações de conexão de banco de dados são especificadas. O controle AccessDataSource precisa apenas o caminho de arquivo para o arquivo de banco de dados do Access. O SqlDataSource, por outro lado, requer uma cadeia de conexão completa.

## <a name="step-1-creating-the-sqldatasource-web-pages"></a>Etapa 1: Criando páginas da Web do SqlDataSource

Antes de começarmos a explorar como trabalhar diretamente com os dados do banco de dados usando o controle SqlDataSource, permitem que s levar alguns instantes para criar as páginas do ASP.NET em nosso projeto de site que precisaremos para este tutorial e os próximos três pela primeira vez. Comece adicionando uma nova pasta chamada `SqlDataSource`. Em seguida, adicione as seguintes páginas do ASP.NET para essa pasta, certificando-se de associar cada página com o `Site.master` página mestra:

- `Default.aspx`
- `Querying.aspx`
- `ParameterizedQueries.aspx`
- `InsertUpdateDelete.aspx`
- `OptimisticConcurrency.aspx`


![Adicione as páginas do ASP.NET para que os tutoriais relacionados SqlDataSource](querying-data-with-the-sqldatasource-control-vb/_static/image3.gif)

**Figura 3**: adicionar as páginas do ASP.NET para que os tutoriais relacionados SqlDataSource


Como em outras pastas `Default.aspx` no `SqlDataSource` pasta listará os tutoriais em sua seção. Lembre-se de que o `SectionLevelTutorialListing.ascx` controle de usuário fornece essa funcionalidade. Portanto, adicionar esse controle de usuário `Default.aspx` arrastando-no Gerenciador de soluções para a página de exibição de Design de s.


[![Adicionar o controle de usuário SectionLevelTutorialListing.ascx para default. aspx](querying-data-with-the-sqldatasource-control-vb/_static/image5.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image4.gif)

**Figura 4**: Adicione o `SectionLevelTutorialListing.ascx` controle de usuário `Default.aspx` ([clique para exibir a imagem em tamanho normal](querying-data-with-the-sqldatasource-control-vb/_static/image6.gif))


Por fim, adicione esses quatro páginas como entradas para o `Web.sitemap` arquivo. Especificamente, adicione a seguinte marcação após o personalizado adicionando botões à DataList e Repeater `<siteMapNode>`:


[!code-sql[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample1.sql)]

Depois de atualizar `Web.sitemap`, reserve um tempo para exibir o site de tutoriais através de um navegador. No menu à esquerda agora inclui itens para a edição, inserindo e excluindo tutoriais.


![O mapa do Site agora inclui entradas para os tutoriais do SqlDataSource](querying-data-with-the-sqldatasource-control-vb/_static/image7.gif)

**Figura 5**: O mapa do Site agora inclui entradas para os tutoriais do SqlDataSource


## <a name="step-2-adding-and-configuring-the-sqldatasource-control"></a>Etapa 2: Adicionando e configurando o controle SqlDataSource

Comece abrindo o `Querying.aspx` página o `SqlDataSource` pasta e alterne para a exibição de Design. Arraste um controle SqlDataSource Toolbox para o Designer e defina suas `ID` para `ProductsDataSource`. Assim como acontece com o ObjectDataSource, SqlDataSource não produza qualquer saída renderizada e, portanto, é exibido como uma caixa cinza na superfície de design. Para configurar o SqlDataSource, clique no link configurar fonte de dados da marca inteligente SqlDataSource s.


![Clique em de configurar o vínculo da fonte de dados da marca inteligente s SqlDataSource](querying-data-with-the-sqldatasource-control-vb/_static/image8.gif)

**Figura 6**: clique em de configurar o vínculo da fonte de dados da marca inteligente s SqlDataSource


Isso abre o Assistente para configurar a fonte de dados do SqlDataSource controle s. Embora as etapas do assistente s seja diferente do controle ObjectDataSource s, o objetivo final é o mesmo para fornecer os detalhes sobre como recuperar, inserir, atualizar e excluir dados por meio da fonte de dados. Para o SqlDataSource isso envolve a especificação de um banco de dados subjacente para usar e fornecendo as instruções SQL ad hoc ou procedimentos armazenados.

A primeira etapa do assistente nos solicita o banco de dados. A lista suspensa inclui esses bancos de dados encontrados em web application s `App_Data` pasta e aqueles que foram adicionados para o nó de conexões de dados no Gerenciador de servidores. Desde que criamos já adicionou uma cadeia de caracteres de conexão para o `NORTHWIND.MDF` banco de dados de `App_Data` pasta para nosso projeto s `Web.config` arquivo, a lista suspensa inclui uma referência a essa cadeia de caracteres de conexão, `NORTHWINDConnectionString`. Escolha este item na lista suspensa e clique em Avançar.


![Escolha o NORTHWINDConnectionString na lista suspensa](querying-data-with-the-sqldatasource-control-vb/_static/image9.gif)

**Figura 7**: escolha a `NORTHWINDConnectionString` na lista suspensa


Depois de escolher o banco de dados, o assistente solicita a consulta retornar dados. Podemos pode especificar as colunas de uma tabela ou exibição para retornar ou pode inserir uma instrução SQL personalizada ou especificar um procedimento armazenado. Você pode alternar entre essa escolha por meio de especificar uma instrução SQL personalizada ou procedimento armazenado e especificar colunas de uma tabela ou exibir botões de opção.

> [!NOTE]
> Para este primeiro exemplo, deixe s use as especificar colunas de uma opção de tabela ou exibição. Vamos voltar ao Assistente posteriormente no tutorial e explorar a especificar uma opção de procedimento armazenado ou uma instrução SQL personalizada.


Figura 8 mostra o configurar a tela de instrução Select quando as especificar colunas de um botão de opção de tabela ou exibição está selecionada. A lista suspensa contém o conjunto de tabelas e exibições no banco de dados Northwind, com a tabela selecionada ou colunas de exibição de s exibidas na lista de caixa de seleção abaixo. Neste exemplo, let s retornar os `ProductID`, `ProductName`, e `UnitPrice` colunas do `Products` tabela. Como mostra a Figura 8, depois de fazer essas seleções o assistente mostra a instrução SQL resultante `SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`.


![Retornar dados da tabela produtos](querying-data-with-the-sqldatasource-control-vb/_static/image10.gif)

**Figura 8**: retornar dados do `Products` tabela


Depois de configurar o Assistente para retornar o `ProductID`, `ProductName`, e `UnitPrice` colunas para o `Products` da tabela, clique no botão Avançar. Essa tela final fornece uma oportunidade para examinar os resultados da consulta configurada na etapa anterior. Clicar no botão consulta de teste executa configurado `SELECT` instrução e exibe os resultados em uma grade.


![Clique no botão de consulta de teste para examinar a consulta SELECT](querying-data-with-the-sqldatasource-control-vb/_static/image11.gif)

**Figura 9**: clique no botão de consulta de teste para revisar seu `SELECT` consulta


Para concluir o assistente, clique em Concluir.

Como com o ObjectDataSource, o Assistente de s SqlDataSource simplesmente atribui valores às propriedades do controle s, ou seja, o [ `ConnectionString` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.connectionstring.aspx) e [ `SelectCommand` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommand.aspx) propriedades. Depois de concluir o assistente, sua marcação declarativa de s de controle SqlDataSource deve ser semelhante ao seguinte:


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample2.aspx)]

O `ConnectionString` propriedade fornece informações sobre como conectar-se ao banco de dados. Esta propriedade pode ser atribuída um valor de cadeia de conexão completa embutido em código, ou pode apontar para uma cadeia de caracteres de conexão no `Web.config`. Para fazer referência a um valor de cadeia de caracteres de conexão em Web. config, use a sintaxe `<%$ expressionPrefix:expressionValue %>`. Normalmente, *expressionPrefix* é ConnectionStrings e *expressionValue* é o nome da cadeia de caracteres de conexão no `Web.config` [ `<connectionStrings>` seção](https://msdn.microsoft.com/library/bf7sd233.aspx). No entanto, a sintaxe pode ser usada para referência `<appSettings>` elementos ou conteúdo dos arquivos de recurso. Ver [visão geral do ASP.NET expressões](https://msdn.microsoft.com/library/d5bd1tad.aspx) para obter mais informações sobre essa sintaxe.

O `SelectCommand` propriedade especifica a instrução de SQL ad hoc ou o procedimento armazenado a ser executada para retornar os dados.

## <a name="step-3-adding-a-data-web-control-and-binding-it-to-the-sqldatasource"></a>Etapa 3: Adicionando um controle de Web de dados e associá-lo ao SqlDataSource

Depois que o SqlDataSource tiver sido configurado, ele pode ser ligado a um controle da Web, como um GridView ou DetailsView de dados. Para este tutorial, deixe s exibir os dados em um GridView. Na caixa de ferramentas, arraste um controle GridView à página e associá-lo para o `ProductsDataSource` SqlDataSource escolhendo-se a fonte de dados na lista suspensa na marca inteligente s GridView.


[![Adicionar um controle GridView e associá-lo para o controle SqlDataSource](querying-data-with-the-sqldatasource-control-vb/_static/image13.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image12.gif)

**Figura 10**: adicionar um controle GridView e associá-lo para o controle SqlDataSource ([clique para exibir a imagem em tamanho normal](querying-data-with-the-sqldatasource-control-vb/_static/image14.gif))


Depois que você tiver selecionado o controle SqlDataSource na lista suspensa na marca inteligente s GridView, Visual Studio adicionará automaticamente um BoundField ou CheckBoxField a GridView para cada uma das colunas retornadas pelo controle de fonte de dados. Como o SqlDataSource retorna três colunas de banco de dados `ProductID`, `ProductName`, e `UnitPrice` há três campos em GridView.

Reserve um tempo para configurar o s GridView três BoundFields. Alterar o `ProductName` campo s `HeaderText` propriedade para o nome do produto e o `UnitPrice` s de campo ao preço. Também formatar o `UnitPrice` campo como uma moeda. Depois de fazer essas modificações, sua marcação declarativa do GridView s deve ser semelhante ao seguinte:


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample3.aspx)]

Visite esta página por meio de um navegador. Como mostra a Figura 11, GridView lista cada produto s `ProductID`, `ProductName`, e `UnitPrice` valores.


[![O GridView exibe cada produto s ProductID, ProductName e valores de UnitPrice](querying-data-with-the-sqldatasource-control-vb/_static/image16.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image15.gif)

**Figura 11**: O produto cada exibe de GridView s `ProductID`, `ProductName`, e `UnitPrice` valores ([clique para exibir a imagem em tamanho normal](querying-data-with-the-sqldatasource-control-vb/_static/image17.gif))


Quando a página é visitada GridView invoca seu controle de fonte de dados s `Select()` método. Quando estávamos usando o controle ObjectDataSource, isso chama o `ProductsBLL` classe s `GetProducts()` método. Com o SqlDataSource, no entanto, o `Select()` método estabelece uma conexão para o banco de dados especificado e os problemas a `SelectCommand` (`SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`, neste exemplo). O SqlDataSource retorna seus resultados que o GridView, em seguida, enumera, criando uma linha em um GridView para cada registro de banco de dados retornado.

## <a name="the-built-in-data-web-control-features-and-the-sqldatasource-control"></a>Os recursos de controle de Web de dados interno e o controle SqlDataSource

Em geral, os recursos inerentes aos dados de que controles da Web de paginação, classificação, editar, excluir, inserir e assim por diante são específicas para o controle de Web de dados em não são dependentes do controle de fonte de dados usado. Ou seja, o GridView pode utilizar seu interno de paginação, classificação, editando e excluindo se ele está associado a um ObjectDataSource ou um SqlDataSource. No entanto, certos recursos de controle da Web de dados são confidenciais para o controle de fonte de dados que está sendo usado ou para a configuração de s de controle de fonte de dados.

Por exemplo, nos [com eficiência por meio de grandes quantidades de dados de paginação](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md) tutorial discutimos como, por padrão, a lógica de paginação para os dados da Web controla ingenuamente retorna *todos os* registros da subjacente fonte de dados e, em seguida, exibe somente o subconjunto apropriado de registros, considerando o índice da página atual e o número de registros a serem exibidos por página. Esse modelo é muito ineficiente quando a paginação por meio de conjuntos de resultados grande o suficiente. Felizmente, o ObjectDataSource pode ser configurado para dar suporte a paginação personalizada, que retorna somente o subconjunto exato de registros a serem exibidos. O controle SqlDataSource, no entanto, não tem as propriedades para implementar a paginação personalizada.

Surge outra sutileza com paginação e classificação com o SqlDataSource. Por padrão, os dados retornados de um SqlDataSource podem ser paginados ou classificados por meio de GridView. Para demonstrar isso, verifique as opções de habilitar a paginação e habilitar a classificação na marca inteligente s GridView no `Querying.aspx` e verifique se que isso funciona conforme o esperado.

Classificação e paginação funcionam porque o SqlDataSource recupera os dados do banco de dados em um conjunto de dados com tipagem. O número total de registros retornados pela consulta um aspecto essencial para implementação da paginação pode ser determinado no conjunto de dados. Além disso, os resultados do conjunto de dados s podem ser classificados por meio de um DataView. Esses recursos são usados automaticamente pelo SqlDataSource quando as solicitações de GridView paginada ou dados classificados.

O SqlDataSource pode ser configurado para retornar um DataReader, em vez de um conjunto de dados alterando sua [ `DataSourceMode` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx) de `DataSet` (o padrão) para `DataReader`. Usando um DataReader pode ser preferencial em situações ao passar os resultados de s SqlDataSource para código existente que espera um DataReader. Além disso, como DataReaders são consideravelmente mais simples objetos de conjuntos de dados, eles oferecem melhor desempenho. Se você fizer essa alteração, no entanto, o controle de Web de dados não pode classificar nem página, pois o SqlDataSource não pode determinar quantos registros é retornado pela consulta, nem o DataReader oferecem qualquer técnicas para classificar os dados retornados.

## <a name="step-4-using-a-custom-sql-statement-or-stored-procedure"></a>Etapa 4: Usando uma instrução SQL personalizada ou procedimento armazenado

Ao configurar o controle SqlDataSource, a consulta usada para retornar dados pode ser especificada em uma das duas abordagens, como uma instrução SQL personalizada ou procedimento armazenado, ou colunas de uma tabela ou exibição existente. Na etapa 2, examinamos selecionando colunas do `Products` tabela. Deixe o s examinar o uso de uma instrução SQL personalizada.

Adicione outro controle GridView para o `Querying.aspx` página e optar por criar uma nova fonte de dados na lista suspensa na marca inteligente. Em seguida, indique que os dados serão extraídos de um banco de dados Isso criará um novo controle SqlDataSource. Nomeie o controle `ProductsWithCategoryInfoDataSource`.


![Criar um novo controle SqlDataSource chamado ProductsWithCategoryInfoDataSource](querying-data-with-the-sqldatasource-control-vb/_static/image18.gif)

**Figura 12**: criar um novo controle SqlDataSource chamado `ProductsWithCategoryInfoDataSource`


A próxima tela pergunta para especificar o banco de dados. Como fizemos na Figura 7, selecione o `NORTHWINDConnectionString` na lista suspensa lista e clique em Avançar. Em configurar a tela de instrução Select, escolha a especificar uma instrução SQL personalizada ou um botão de opção de procedimento armazenado e clique em Avançar. Isso abrirá a tela definir instruções personalizadas ou procedimentos armazenados, que oferece guias rotulada como SELECT, UPDATE, INSERT e DELETE. Em cada guia, você pode inserir uma instrução SQL personalizada na caixa de texto ou escolha um procedimento armazenado na lista suspensa. Neste tutorial veremos inserir uma instrução SQL personalizada; o próximo tutorial inclui um exemplo que usa um procedimento armazenado.


![Insira uma instrução SQL personalizada ou escolha um procedimento armazenado](querying-data-with-the-sqldatasource-control-vb/_static/image19.gif)

**Figura 13**: insira uma instrução SQL personalizada ou escolha um procedimento armazenado


A instrução SQL personalizada pode ser inserida manualmente na caixa de texto ou pode ser construída graficamente clicando no botão Construtor de consultas. O construtor de consultas ou a caixa de texto, use a seguinte consulta para retornar os `ProductID` e `ProductName` campos da `Products` tabela usando um `JOIN` para recuperar o produto s `CategoryName` do `Categories` tabela:


[!code-sql[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample4.sql)]


![Você pode graficamente construir a consulta usando o construtor de consultas](querying-data-with-the-sqldatasource-control-vb/_static/image20.gif)

**Figura 14**: você pode construir graficamente a consulta usando o construtor de consultas


Depois de especificar a consulta, clique em Avançar para prosseguir para a tela de consulta de teste. Clique em Concluir para concluir o assistente SqlDataSource.

Depois de concluir o assistente, o GridView terá três BoundFields adicionados a ele exibindo o `ProductID`, `ProductName`, e `CategoryName` colunas retornados da consulta e resultante na marcação declarativa a seguir:


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample5.aspx)]


[![O GridView mostra cada ID de produto s, o nome da categoria e o nome associado](querying-data-with-the-sqldatasource-control-vb/_static/image22.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image21.gif)

**Figura 15**: O GridView mostra cada produto s ID, nome e nome de categoria associados ([clique para exibir a imagem em tamanho normal](querying-data-with-the-sqldatasource-control-vb/_static/image23.gif))


## <a name="summary"></a>Resumo

Neste tutorial vimos como consultar e exibir dados usando o controle SqlDataSource. Como o ObjectDataSource, SqlDataSource serve como um proxy, fornecendo uma abordagem declarativa para acessar os dados. Especificam de suas propriedades para se conectar ao banco de dados e o SQL `SELECT` consultar para executar; elas podem ser especificadas na janela Propriedades ou usando o Assistente Configurar fonte de dados.

O `SELECT` , examinamos neste tutorial de exemplos de consulta retornados todos os registros da consulta especificada. No entanto, o controle SqlDataSource, pode incluir um `WHERE` cláusula com parâmetros cujos valores são atribuídos por meio de programação ou automaticamente são extraídos de uma origem especificada. Vamos examinar como criar e usar consultas parametrizadas no próximo tutorial!

Boa programação!

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Acessando dados do banco de dados relacional](http://aspnet.4guysfromrolla.com/articles/022206-1.aspx)
- [Visão geral do controle SqlDataSource](https://msdn.microsoft.com/library/dz12d98w.aspx)
- [Tutoriais de início rápido do ASP.NET: O controle SqlDataSource](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/sqldatasource.aspx)
- [O Web. config `<connectionStrings>` elemento](https://msdn.microsoft.com/library/bf7sd233.aspx)
- [Referência de cadeia de caracteres de Conexão de banco de dados](http://www.connectionstrings.com/)

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores de avanço para este tutorial foram Connery Susan, Bernadette Leigh e David Suru. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](implementing-optimistic-concurrency-with-the-sqldatasource-cs.md)
> [Próximo](using-parameterized-queries-with-the-sqldatasource-vb.md)
