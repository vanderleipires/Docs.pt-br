---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-cs
title: Consultando dados com o controle SqlDataSource (c#) | Microsoft Docs
author: rick-anderson
description: "Os tutoriais anteriores usamos o controle ObjectDataSource totalmente separar a camada de apresentação da camada de acesso a dados. Iniciando com este tutorial..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: 60512d6a-b572-4b7a-beb3-3e44b4d2020c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 4652e5820e621a7b2ad3b03bb5a1d2cb4968fadd
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="querying-data-with-the-sqldatasource-control-c"></a>Consultando dados com o controle SqlDataSource (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_47_CS.exe) ou [baixar PDF](querying-data-with-the-sqldatasource-control-cs/_static/datatutorial47cs1.pdf)

> Os tutoriais anteriores usamos o controle ObjectDataSource totalmente separar a camada de apresentação da camada de acesso a dados. Estamos começando com este tutorial, saiba como o controle SqlDataSource pode ser usado para aplicativos simples que não exigem uma separação estrita de apresentação e de acesso a dados.


## <a name="introduction"></a>Introdução

Todos os tutoriais usou uma arquitetura em camadas que consiste em camadas de acesso a dados, lógica de negócios e apresentação ve examinou até o momento. A camada de acesso de dados (DAL) foi criado no primeiro tutorial ([criando uma camada de acesso a dados](../introduction/creating-a-data-access-layer-cs.md)) e a camada de lógica de negócios no segundo ([criando uma camada de lógica de negócios](../introduction/creating-a-business-logic-layer-cs.md)). Começando com o [exibindo dados com o ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) tutorial, vimos como usar o controle ObjectDataSource para novo do ASP.NET 2.0 s declarativamente interajam com a arquitetura da camada de apresentação.

Enquanto todos os tutoriais até tem usado a arquitetura para trabalhar com dados, também é possível acessar, inserir, atualizar e excluir dados de banco de dados diretamente de uma página ASP.NET, ignorando a arquitetura. Isso coloca as consultas de banco de dados específico e a lógica de negócios diretamente na página da web. Para aplicativos suficientemente grandes ou complexos, criar, implementar e usando uma arquitetura em camadas são extremamente importante para o sucesso, a atualização e a facilidade de manutenção do aplicativo. Desenvolvendo uma arquitetura robusta, no entanto, pode ser desnecessário ao criar aplicativos muito simples, únicos.

O ASP.NET 2.0 fornece controles de fonte de dados internos cinco [SqlDataSource](https://msdn.microsoft.com/library/dz12d98w%28vs.80%29.aspx), [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx), [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), [XmlDataSource](https://msdn.microsoft.com/library/e8d8587a%28en-US,VS.80%29.aspx), e [SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x%28en-US,VS.80%29.aspx). O SqlDataSource pode ser usado para acessar e modificar dados diretamente de um banco de dados relacional, incluindo o Microsoft SQL Server, Microsoft Access, Oracle, MySQL e outros. Este tutorial e as próximas três, examinaremos como trabalhar com o controle SqlDataSource, explorar como consultar e filtrar dados do banco de dados, bem como usar o SqlDataSource para inserir, atualizar e excluir dados.


![ASP.NET 2.0 inclui cinco controles de fonte de dados internos](querying-data-with-the-sqldatasource-control-cs/_static/image1.gif)

**Figura 1**: ASP.NET 2.0 inclui cinco controles de fonte de dados internos


## <a name="comparing-the-objectdatasource-and-sqldatasource"></a>Comparando o ObjectDataSource e SqlDataSource

Conceitualmente, controles de ObjectDataSource e SqlDataSource são simplesmente os proxies aos dados. Como discutido o [exibindo dados com o ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) tutorial, o ObjectDataSource tem propriedades que indicam o tipo de objeto que fornece os dados e os métodos para chamar para selecionar, inserir, atualizar e excluir dados do tipo de objeto subjacente. Depois de configurar as propriedades de s ObjectDataSource, um controle de Web como um GridView, DetailsView ou DataList de dados podem ser associados ao controle, usando o s ObjectDataSource `Select()`, `Insert()`, `Delete()`, e `Update()` métodos interagir com a arquitetura subjacente.

O SqlDataSource fornece a mesma funcionalidade, mas funciona em um banco de dados relacional em vez de uma biblioteca de objetos. Com o SqlDataSource, podemos deve especificar a cadeia de caracteres de conexão do banco de dados e consultas SQL ad hoc ou procedimentos armazenados para executar para inserir, atualizar, excluir e recuperar dados. O SqlDataSource s `Select()`, `Insert()`, `Update()`, e `Delete()` métodos, quando chamado, conecte-se ao banco de dados especificado e execute a consulta SQL apropriada. Como o diagrama a seguir ilustra, esses métodos fazem o trabalho pesado de se conectar a um banco de dados, emitindo uma consulta e retornar os resultados.


![O SqlDataSource serve como um Proxy para o banco de dados](querying-data-with-the-sqldatasource-control-cs/_static/image2.gif)

**Figura 2**: SqlDataSource serve como um Proxy para o banco de dados


> [!NOTE]
> Neste tutorial, nos concentraremos na recuperação de dados do banco de dados. No [inserindo, atualizando e exclusão de dados com o controle SqlDataSource](inserting-updating-and-deleting-data-with-the-sqldatasource-cs.md) tutorial, veremos como configurar o SqlDataSource para dar suporte a inserir, atualizar e excluir.


## <a name="the-sqldatasource-and-accessdatasource-controls"></a>O SqlDataSource e AccessDataSource

Além do controle SqlDataSource, o ASP.NET 2.0 também inclui um controle AccessDataSource. Esses dois controles diferentes levam muitos desenvolvedores novo para o ASP.NET 2.0 suspeitar que o controle AccessDataSource foi projetado para trabalhar exclusivamente com o Microsoft Access com o controle SqlDataSource projetado para trabalhar exclusivamente com o Microsoft SQL Server. Embora o AccessDataSource é projetado para funcionar especificamente com o Microsoft Access, o controle SqlDataSource funciona com *qualquer* banco de dados relacional que pode ser acessado por meio do .NET. Isso inclui qualquer compatível com OLE DB ou ODBC armazenamentos de dados, como Microsoft SQL Server, Microsoft Access, Oracle, Informix, MySQL e PostgreSQL, entre outros.

A única diferença entre os controles AccessDataSource e SqlDataSource é como as informações de conexão de banco de dados são especificadas. O controle AccessDataSource precisa apenas o caminho de arquivo para o arquivo de banco de dados do Access. O SqlDataSource, por outro lado, requer uma cadeia de conexão completa.

## <a name="step-1-creating-the-sqldatasource-web-pages"></a>Etapa 1: Criando páginas da Web SqlDataSource

Antes de começar a exploração de como trabalhar diretamente com o banco de dados usando o controle SqlDataSource, permitem s primeiro dedicar um tempo para criar as páginas ASP.NET em nosso projeto de site que vamos precisar para este tutorial e as próximas três. Comece adicionando uma nova pasta chamada `SqlDataSource`. Em seguida, adicione as seguintes páginas do ASP.NET para a pasta, certificando-se de associar cada página com o `Site.master` página mestre:

- `Default.aspx`
- `Querying.aspx`
- `ParameterizedQueries.aspx`
- `InsertUpdateDelete.aspx`
- `OptimisticConcurrency.aspx`


![Adicionar as páginas ASP.NET para os tutoriais de SqlDataSource](querying-data-with-the-sqldatasource-control-cs/_static/image3.gif)

**Figura 3**: adicionar as páginas ASP.NET para os tutoriais de SqlDataSource


Como em outras pastas, `Default.aspx` no `SqlDataSource` pasta listará os tutoriais em sua seção. Lembre-se de que o `SectionLevelTutorialListing.ascx` controle de usuário fornece essa funcionalidade. Portanto, adicione este controle de usuário para `Default.aspx` arrastando-o no Gerenciador de soluções para a página de exibição de Design de s.


[![Adicionar o controle de usuário SectionLevelTutorialListing.ascx a Default.aspx](querying-data-with-the-sqldatasource-control-cs/_static/image5.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image4.gif)

**Figura 4**: adicionar o `SectionLevelTutorialListing.ascx` controle de usuário `Default.aspx` ([clique para exibir a imagem em tamanho normal](querying-data-with-the-sqldatasource-control-cs/_static/image6.gif))


Por fim, adicione esses quatro páginas como entradas para o `Web.sitemap` arquivo. Especificamente, adicione a seguinte marcação após os botões personalizados adicionando o DataList e repetidor `<siteMapNode>`:


[!code-sql[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample1.sql)]

Após a atualização `Web.sitemap`, reserve um tempo para exibir o site de tutoriais através de um navegador. No menu à esquerda agora inclui itens para a edição, inserção e exclusão tutoriais.


![O mapa do Site agora inclui entradas para os tutoriais de SqlDataSource](querying-data-with-the-sqldatasource-control-cs/_static/image7.gif)

**Figura 5**: O mapa de Site agora inclui entradas para os tutoriais de SqlDataSource


## <a name="step-2-adding-and-configuring-the-sqldatasource-control"></a>Etapa 2: Adicionando e configurando o controle SqlDataSource

Comece abrindo o `Querying.aspx` página o `SqlDataSource` pasta e alterne para a exibição de Design. Arraste um controle SqlDataSource da caixa de ferramentas para o Designer e defina seu `ID` para `ProductsDataSource`. Assim como acontece com ObjectDataSource, SqlDataSource não produzir qualquer saída renderizada e, portanto, é exibido como uma caixa cinza na superfície de design. Para configurar o SqlDataSource, clique no link configurar fonte de dados de marca inteligente SqlDataSource s.


![Clique em de configurar o vínculo da fonte de dados de marca inteligente s SqlDataSource](querying-data-with-the-sqldatasource-control-cs/_static/image8.gif)

**Figura 6**: clique em de configurar o vínculo da fonte de dados de marca inteligente s SqlDataSource


Isso abre o Assistente para configurar a fonte de dados do SqlDataSource controle s. Embora as etapas do assistente s diferem do controle ObjectDataSource s, o objetivo final é o mesmo para fornecer os detalhes sobre como recuperar, inserir, atualizar e excluir dados por meio da fonte de dados. Para o SqlDataSource isso envolve a especificação de banco de dados subjacente para usar e fornecendo as instruções SQL ad hoc ou procedimentos armazenados.

A primeira etapa do assistente solicita nos para o banco de dados. A lista suspensa inclui os bancos de dados encontrados em web aplicativo s `App_Data` pasta e aqueles que foram adicionados para o nó de conexões de dados no Gerenciador de servidores. Desde ve já adicionado a uma cadeia de caracteres de conexão para o `NORTHWIND.MDF` banco de dados de `App_Data` pasta ao nosso projeto s `Web.config` arquivo, a lista suspensa inclui uma referência a essa cadeia de caracteres de conexão, `NORTHWINDConnectionString`. Selecione este item da lista suspensa e clique em Avançar.


![Escolha NORTHWINDConnectionString na lista suspensa](querying-data-with-the-sqldatasource-control-cs/_static/image9.gif)

**Figura 7**: escolha o `NORTHWINDConnectionString` na lista suspensa


Depois de escolher o banco de dados, o assistente solicita a consulta retornar dados. É possível especificar as colunas de uma tabela ou exibição para retornar ou pode inserir uma instrução SQL personalizada ou especificar um procedimento armazenado. Você pode alternar entre essa escolha por meio de especificar uma instrução SQL personalizada ou procedimento armazenado e especificar colunas de uma tabela ou exibir botões de opção.

> [!NOTE]
> Este primeiro exemplo, permitem s usam as especificar colunas de uma opção de tabela ou exibição. Vamos voltar ao Assistente posteriormente neste tutorial e explorar a especificar uma opção de procedimento armazenado ou uma instrução SQL personalizada.


A Figura 8 mostra o configurar a tela de instrução Select quando as especificar colunas de uma tabela ou exibição de botão de opção é selecionada. A lista suspensa contém o conjunto de tabelas e exibições no banco de dados Northwind, com a tabela selecionada ou colunas da exibição s exibidas na lista de caixa de seleção abaixo. Neste exemplo, vamos s retornar o `ProductID`, `ProductName`, e `UnitPrice` colunas para o `Products` tabela. Como mostra a Figura 8, depois de fazer essas seleções o assistente mostra a instrução SQL resultante `SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`.


![Retornar dados da tabela produtos](querying-data-with-the-sqldatasource-control-cs/_static/image10.gif)

**Figura 8**: retornar dados a partir de `Products` tabela


Depois de configurar o Assistente para retornar o `ProductID`, `ProductName`, e `UnitPrice` colunas para o `Products` da tabela, clique no botão Avançar. Essa tela final fornece uma oportunidade para examinar os resultados da consulta configurada na etapa anterior. Clique no botão Testar consulta executa configurado `SELECT` instrução e exibe os resultados em uma grade.


![Clique no botão de consulta de teste para revisar a consulta SELECT](querying-data-with-the-sqldatasource-control-cs/_static/image11.gif)

**Figura 9**: clique no botão de consulta de teste para revisar seu `SELECT` consulta


Para concluir o assistente, clique em Concluir.

Como com ObjectDataSource, o Assistente de s SqlDataSource simplesmente atribui valores às propriedades do controle s, ou seja, o [ `ConnectionString` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.connectionstring.aspx) e [ `SelectCommand` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommand.aspx) propriedades. Depois de concluir o assistente, sua marcação declarativa de s de controle SqlDataSource deve ser semelhante ao seguinte:


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample2.aspx)]

O `ConnectionString` propriedade fornece informações sobre como conectar-se ao banco de dados. Essa propriedade pode ser atribuída um valor de cadeia de caracteres de conexão completa, embutido ou pode apontar para uma cadeia de caracteres de conexão em `Web.config`. Para fazer referência a um valor de cadeia de caracteres de conexão no Web. config, use a sintaxe `<%$ expressionPrefix:expressionValue %>`. Normalmente, *expressionPrefix* é ConnectionStrings e *expressionValue* é o nome da cadeia de conexão no `Web.config` [ `<connectionStrings>` seção](https://msdn.microsoft.com/library/bf7sd233.aspx). No entanto, a sintaxe pode ser usada para referência `<appSettings>` elementos ou o conteúdo dos arquivos de recursos. Consulte [ASP.NET Expressions Overview](https://msdn.microsoft.com/library/d5bd1tad.aspx) para obter mais informações sobre essa sintaxe.

O `SelectCommand` propriedade especifica a instrução de SQL ad hoc ou o procedimento armazenado a ser executada para retornar os dados.

## <a name="step-3-adding-a-data-web-control-and-binding-it-to-the-sqldatasource"></a>Etapa 3: Adicionar um controle de Web de dados e vinculá-lo a SqlDataSource

Depois que o SqlDataSource tiver sido configurado, ele pode ser ligado a um controle da Web, como um GridView ou DetailsView de dados. Para este tutorial, permitem s exibir os dados em um controle GridView. Na caixa de ferramentas, arraste um controle GridView à página e associá-lo para o `ProductsDataSource` SqlDataSource escolhendo a fonte de dados na lista suspensa na marca inteligente s GridView.


[![Adicionar um controle GridView e associá-lo ao controle SqlDataSource](querying-data-with-the-sqldatasource-control-cs/_static/image13.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image12.gif)

**Figura 10**: adicionar um controle GridView e associá-lo ao controle SqlDataSource ([clique para exibir a imagem em tamanho normal](querying-data-with-the-sqldatasource-control-cs/_static/image14.gif))


Depois que você tiver selecionado o controle SqlDataSource na lista suspensa na marca inteligente s GridView, Visual Studio adicionará automaticamente um BoundField ou CheckBoxField a GridView para cada uma das colunas retornadas pelo controle de fonte de dados. Como o SqlDataSource retorna três colunas de banco de dados `ProductID`, `ProductName`, e `UnitPrice` há três campos em GridView.

Reserve um momento para configurar a s GridView três BoundFields. Alterar o `ProductName` campo s `HeaderText` propriedade para o nome do produto e o `UnitPrice` s campo preço. Também formatar o `UnitPrice` campo como uma moeda. Depois de fazer essas modificações, sua marcação declarativa do GridView s deve ser semelhante ao seguinte:


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample3.aspx)]

Visite essa página através de um navegador. Como mostra a Figura 11, GridView lista cada produto s `ProductID`, `ProductName`, e `UnitPrice` valores.


[![O GridView exibe cada produto s ProductID, ProductName e valores de UnitPrice](querying-data-with-the-sqldatasource-control-cs/_static/image16.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image15.gif)

**Figura 11**: O produto cada exibe de GridView s `ProductID`, `ProductName`, e `UnitPrice` valores ([clique para exibir a imagem em tamanho normal](querying-data-with-the-sqldatasource-control-cs/_static/image17.gif))


Quando a página for visitada GridView invoca o controle de fonte de dados s `Select()` método. Quando é estava usando o controle ObjectDataSource, isso chamado o `ProductsBLL` classe s `GetProducts()` método. Com o SqlDataSource, no entanto, o `Select()` método estabelece uma conexão para o banco de dados especificado e problemas de `SelectCommand` (`SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`, neste exemplo). O SqlDataSource retorna os resultados que o GridView, em seguida, enumera, criando uma linha em GridView para cada registro de banco de dados retornado.

## <a name="the-built-in-data-web-control-features-and-the-sqldatasource-control"></a>Os recursos de controle de Web de dados interna e o controle SqlDataSource

Em geral, os recursos inerentes aos dados de que controles da Web de paginação, classificação, editar, excluir, inserir e assim por diante são específicas para o controle da Web de dados em não são dependentes do controle da fonte de dados usado. Ou seja, o GridView pode utilizar seu interno de paginação, classificação, edição e exclusão se ele está associado a um ObjectDataSource ou um SqlDataSource. No entanto, certos recursos de controle da Web de dados são importantes para o controle de fonte de dados que está sendo usado ou a configuração de controle s da fonte de dados.

Por exemplo, no [com eficiência por meio de grandes quantidades de dados de paginação](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md) tutorial discutimos como, por padrão, a lógica de paginação para os dados da Web controla ingenuamente retorna *todos os* registros subjacente fonte de dados e, em seguida, exibe somente o subconjunto apropriado de registros, considerando o índice da página atual e o número de registros a serem exibidos por página. Esse modelo é altamente ineficiente quando a paginação suficientemente grandes conjuntos de resultados. Felizmente, o ObjectDataSource pode ser configurado para dar suporte à paginação personalizada, que retorna somente o subconjunto preciso de registros a serem exibidos. O controle SqlDataSource, no entanto, não tem as propriedades para a implementação de paginação personalizada.

Outro recurso com paginação e classificação surge com SqlDataSource. Por padrão, os dados retornados de um SqlDataSource podem ser paginados ou classificados por meio de GridView. Para demonstrar isso, verifique as opções habilitar paginação e Habilitar classificação na marca inteligente s GridView no `Querying.aspx` e verifique se que isso funciona conforme o esperado.

Classificação e paginação funcionam porque o SqlDataSource recupera os dados do banco de dados em um DataSet tipadas vagamente. O número total de registros retornados pela consulta um aspecto essencial para a implementação de paginação pode ser determinado do conjunto de dados. Além disso, os resultados do conjunto de dados s podem ser classificados por meio de uma exibição de dados. Esses recursos serão automaticamente usados pelo SqlDataSource quando as solicitações de GridView paginável ou dados classificados.

O SqlDataSource pode ser configurado para retornar um DataReader em vez de um conjunto de dados alterando sua [ `DataSourceMode` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx) de `DataSet` (o padrão) para `DataReader`. Usando um DataReader pode ser preferencial em situações ao passar os resultados de s SqlDataSource para código existente que espera um DataReader. Além disso, como DataReaders são objetos consideravelmente mais simples de conjuntos de dados, eles oferecem um desempenho melhor. Se você fizer essa alteração, no entanto, o controle da Web de dados não pode classificar nem página desde o SqlDataSource não pode determinar quantos registros são retornados pela consulta, nem o DataReader oferecem qualquer técnicas para classificar os dados retornados.

## <a name="step-4-using-a-custom-sql-statement-or-stored-procedure"></a>Etapa 4: Usando uma instrução SQL personalizada ou procedimento armazenado

Ao configurar o controle SqlDataSource, a consulta usada para retornar dados pode ser especificada em uma das duas abordagens como uma instrução SQL personalizada ou procedimento armazenado, ou colunas de uma tabela ou exibição existente. Na etapa 2, examinamos selecionar colunas para o `Products` tabela. Permitir que o s examinar usando uma instrução SQL personalizada.

Adicione um controle GridView para o `Querying.aspx` página e optar por criar uma nova fonte de dados na lista suspensa na marca inteligente. Em seguida, indicar que os dados serão recebidos de um banco de dados Isso criará um novo controle SqlDataSource. Nome do controle `ProductsWithCategoryInfoDataSource`.


![Criar um novo controle SqlDataSource denominado ProductsWithCategoryInfoDataSource](querying-data-with-the-sqldatasource-control-cs/_static/image18.gif)

**Figura 12**: criar um novo controle SqlDataSource chamado`ProductsWithCategoryInfoDataSource`


A próxima tela pergunta para especificar o banco de dados. Como foi novamente na Figura 7, selecione o `NORTHWINDConnectionString` na lista suspensa lista e clique em Avançar. Em configurar a tela de instrução Select, escolha a especificar uma instrução SQL personalizada ou um botão de opção de procedimento armazenado e clique em Avançar. Isso abrirá a tela definir instruções personalizadas ou procedimentos armazenados, que oferece guias: SELECT, UPDATE, INSERT e DELETE. Em cada guia, você pode inserir uma instrução SQL personalizada na caixa de texto ou escolha um procedimento armazenado na lista suspensa. Neste tutorial, veremos inserindo uma instrução SQL personalizada; o seguinte tutorial inclui um exemplo que usa um procedimento armazenado.


![Insira uma instrução SQL personalizada ou escolha um procedimento armazenado](querying-data-with-the-sqldatasource-control-cs/_static/image19.gif)

**Figura 13**: insira uma instrução SQL personalizada ou escolha um procedimento armazenado


A instrução SQL personalizada pode ser inserida manualmente na caixa de texto ou pode ser construída graficamente clicando no botão Construtor de consultas. O construtor de consultas ou a caixa de texto, use a seguinte consulta para retornar o `ProductID` e `ProductName` os campos do `Products` tabela usando um `JOIN` para recuperar o produto s `CategoryName` do `Categories` tabela:


[!code-sql[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample4.sql)]


![Você pode graficamente construir a consulta usando o construtor de consultas](querying-data-with-the-sqldatasource-control-cs/_static/image20.gif)

**Figura 14**: você pode construir graficamente a consulta usando o construtor de consultas


Depois de especificar a consulta, clique em Avançar para ir para a tela de consulta de teste. Clique em Concluir para concluir o Assistente de SqlDataSource.

Depois de concluir o assistente, GridView terá três BoundFields adicionados a ele exibindo o `ProductID`, `ProductName`, e `CategoryName` colunas retornado pela consulta e resultante na marcação declarativa a seguir:


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample5.aspx)]


[![GridView mostra cada ID de produto s, o nome da categoria de nome e respectivos](querying-data-with-the-sqldatasource-control-cs/_static/image22.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image21.gif)

**Figura 15**: O GridView mostra cada produto s ID, o nome e o nome da categoria associada ([clique para exibir a imagem em tamanho normal](querying-data-with-the-sqldatasource-control-cs/_static/image23.gif))


## <a name="summary"></a>Resumo

Neste tutorial, vimos como consultar e exibir dados usando o controle SqlDataSource. Como o ObjectDataSource, SqlDataSource serve como um proxy, fornecendo uma abordagem declarativa para acessar dados. Suas propriedades especificam o banco de dados para se conectar ao e o SQL `SELECT` consultar para executar; elas podem ser especificadas usando a janela Propriedades ou usando o Assistente Configurar fonte de dados.

O `SELECT` examinamos neste tutorial de exemplos de consulta retornados todos os registros da consulta especificada. No entanto, o controle SqlDataSource, pode incluir um `WHERE` cláusula com parâmetros cujos valores são atribuídos por meio de programação ou são extraídos automaticamente de uma origem especificada. Vamos examinar como criar e usar consultas parametrizadas no tutorial próximo!

Boa programação!

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Acessando dados do banco de dados relacional](http://aspnet.4guysfromrolla.com/articles/022206-1.aspx)
- [Visão geral do controle SqlDataSource](https://msdn.microsoft.com/library/dz12d98w.aspx)
- [Tutoriais de início rápido do ASP.NET: O controle SqlDataSource](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/sqldatasource.aspx)
- [O Web. config `<connectionStrings>` elemento](https://msdn.microsoft.com/library/bf7sd233.aspx)
- [Referência de cadeia de caracteres de Conexão do banco de dados](http://www.connectionstrings.com/)

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisores levar para este tutorial foram Connery Susan, Bernadette Leigh e David Suru. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Avançar](using-parameterized-queries-with-the-sqldatasource-cs.md)
