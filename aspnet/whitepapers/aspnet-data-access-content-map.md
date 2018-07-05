---
uid: whitepapers/aspnet-data-access-content-map
title: Recursos recomendados de acesso a dados do ASP.NET – | Microsoft Docs
author: rick-anderson
description: Este tópico fornece links para recursos de documentação sobre como acessar dados em aplicativos web ASP.NET, principalmente usando o Entity Framework e o SQL Se...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/25/2013
ms.topic: article
ms.assetid: f8157be1-4ab9-469e-ad3a-0ccc80b56c00
ms.technology: ''
msc.legacyurl: /whitepapers/aspnet-data-access-content-map
msc.type: content
ms.openlocfilehash: 3a8d7de622fd2d0b229dba3af31557a172b90df8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396358"
---
<a name="aspnet-data-access---recommended-resources"></a>Acesso a dados do ASP.NET – recursos recomendado
====================
> Este tópico fornece links para recursos de documentação sobre como acessar dados em aplicativos web ASP.NET, principalmente usando o Entity Framework e do SQL Server.
> 
> Se você souber que um blog excelente postagem, [stackoverflow](http://stackoverflow.com) thread ou qualquer outro link que pode ser útil, [envie-em um email](mailto:aspnetue@microsoft.com?subject=Data Access Content Map) com o link.
> 
> Última atualização 4/3/2014


Este tópico contém as seguintes seções:

- [Introdução ao acesso a dados no ASP.NET](#gettingstarted)
- [Usando o Entity Framework](#ef)

    - [Usando o Entity Framework Code First](#cf)
    - [Usando o Entity Framework Code First Migrations.](#efcfmigrations)
    - [Usando o Entity Framework Database First ou Model First (o EF Designer)](#efdbf)
    - [Carregando dados relacionados no Entity Framework (o carregamento lento, o carregamento adiantado e explícito de carregamento)](#efrelateddata)
    - [Otimizando o desempenho do Entity Framework](#optimizingef)
    - [Tratamento de simultaneidade em um aplicativo do Entity Framework](#efconcurrency)
    - [Livros sobre o Entity Framework](#efbooks)
    - [Recursos adicionais do Entity Framework](#otherefresources)
- [Aplicativos de vinculação de dados no ASP.NET Web Forms](#wfdatabinding)

    - [Usando o Web Forms a associação de modelos](#wfmodelbinding)
    - [Usando o Web Forms a controles de fonte de dados](#wfdsc)
    - [Usando o Web Forms controles ligados a dados e expressões de associação de dados](#wfdbc)
- [Trabalhando com bancos de dados do SQL Server](#sqlserver)

    - [Trabalhando com bancos de dados do SQL Server Express LocalDB](#sslocaldb)
    - [Trabalhando com bancos de dados do SQL Server Express](#sse)
    - [Trabalhando com o Windows Azure SQL Database](#ssdb)
    - [Escolhendo entre o SQL Server e Windows Azure SQL Database](#ssdbchoosing)
- [Trabalhando com sistemas de gerenciamento de banco de dados NoSQL](#nosql)
- [Usando consultas LINQ em aplicativos ASP.NET](#linq)
- [Usando o scaffolding de dados dinâmicos](#dd)
- [Protegendo o acesso a dados](#securing)
- [Otimizando desempenho de acesso a dados](#optimizingdataaccess)
- [Implantando um banco de dados](#deploying)
- [Acessando dados através de um serviço Web](#webservice)
- [Recursos adicionais](#additional)

<a id="gettingstarted"></a>

## <a name="getting-started-with-data-access-in-aspnet"></a>Introdução ao acesso a dados no ASP.NET

- [Opções de armazenamento de dados (compilando aplicativos de nuvem do mundo Real com o Windows Azure)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md). Capítulo de um livro eletrônico sobre o desenvolvimento para a nuvem. Apresenta os bancos de dados NoSQL como uma alternativa que muitos desenvolvedores familiarizados com bancos de dados relacionais tendem a ignorar. Apresenta diretrizes sobre o que pensar ao escolher relacional ou NoSQL ou escolher uma plataforma específica.
- [Opções de acesso de dados do ASP.NET](https://msdn.microsoft.com/library/ms178359.aspx) (MSDN). Uma introdução para opções de acesso de dados para bancos de dados relacionais para o ASP.NET e orientações sobre como escolher as plataformas e acessar os métodos que são apropriados para seu cenário.
- [Banco de dados relacional](http://en.wikipedia.org/wiki/Relational_database). Wikipédia). Se você ainda não tiver trabalhado com bancos de dados relacionais, consulte esta página para obter uma introdução aos conceitos e terminologia de banco de dados relacional. Para obter uma introdução ao SQL Server em particular ver [trabalhando com bancos de dados do SQL Server](#sqlserver) mais adiante neste tópico.

<a id="ef"></a>

## <a name="using-the-entity-framework"></a>Usando o Entity Framework

- [Abordagens de desenvolvimento do Entity Framework](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf) (MSDN). Orientação sobre como escolher uma Entity Framework abordagem de desenvolvimento Database First, Model First ou Code First.

<a id="cf"></a>

### <a name="using-entity-framework-code-first"></a>Usando o Entity Framework Code First
  

Os tutoriais a seguir oferecem aplicativos de exemplo para download:

- [Introdução ao EF 6 usando o MVC 5](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Abrange uma ampla variedade de cenários de Entity Framework Code First, incluindo migrações e EF 6 recursos como resiliência de conexão e interceptação de comando assíncrono. Isso é uma versão atualizada do [EF 5 / MVC 4 série](../mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). A série anterior inclui um tutorial sobre o repositório e padrões de unidade de trabalho que não está incluído na nova série.
- [Introdução ao ASP.NET MVC 5](../mvc/overview/getting-started/introduction/getting-started.md). Aborda cenários primeiro intervalo de código do Entity Framework mais estreita, mas faz um trabalho mais abrangente de introduzir recursos de MVC.
- [Associação de modelos e formulários da Web](https://go.microsoft.com/fwlink/?LinkId=286117). Usa o Code First em um aplicativo de formulários da Web.
- [Introdução ao ASP.NET 4.5 do Web Forms](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Uma introdução ao Web Forms com alguns cobertura do Code First. Usos de associação de modelos.
- [Store de música do MVC](../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1.md). Usa o Code First em um aplicativo MVC 3 de comércio eletrônico que também implementa a associação e a autorização. A versão do MVC e ASP.NET (autenticação e autorização) sistema da associação usada aqui estão desatualizados; Para obter informações mais atualizadas sobre a associação do ASP.NET, consulte [ https://asp.net/identity ](https://asp.net/identity).

Outros recursos:

- [Entity Framework - Code First para um banco de dados](https://msdn.microsoft.com/data/jj200620). MSDN. Vídeo e instruções passo a passo que mostra como usar o Code First com um banco de dados existente.
- [Central de desenvolvedores de dados - Entity Framework](https://msdn.microsoft.com/data/ef). MSDN. Para obter um guia para a documentação do Entity Framework que foi criado e mantido pela equipe do Entity Framework, consulte o [começar](https://msdn.microsoft.com/data/ee712907) link.

Consulte também [livros sobre o Entity Framework](#efbooks) e [recursos adicionais do Entity Framework](#otherefresources) mais adiante neste tópico.

<a id="efcfmigrations"></a>

### <a name="using-entity-framework-code-first-migrations"></a>Usando o Entity Framework Code First Migrations.
  

Maioria dos tutoriais do Code First listados acima migrações de capa. Consulte também os recursos a seguir.

- [Implantação de Web do ASP.NET usando o Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). parte 2 série de tutoriais que mostra como usar as migrações Code First para implantar um banco de dados.
- [Implantar um aplicativo ASP.NET MVC 5 seguro com associação, OAuth e banco de dados SQL para um Site da Web do Azure Windows](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Microsoft Azure). Como usar as migrações para implantar dados de associação e o aplicativo no Azure.
- [Visão geral de implantação de Web para Visual Studio e o ASP.NET](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment). Consulte a **Configurando a implantação de banco de dados no Visual Studio** seção para obter uma explicação de como as migrações do Code First se integra ao recursos de implantação de web do Visual Studio.
- [Central de desenvolvedores de dados - migrações do Code First](https://msdn.microsoft.com/data/jj591621) (MSDN). Documentação de migrações da equipe do Entity Framework.
- [A série Screencast migrações](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx). Blog do EF). Três vídeos sobre tópicos avançados em migrações do Code First.
- [Code First Migrations com Sites de páginas da Web do ASP.NET](http://www.mikesdotnetting.com/Article/217/Code-First-Migrations-With-ASP.NET-Web-Pages-Sites). Blog de Mikesdotnetting). Mostra como usar migrações do Code First com um site de páginas da Web ASP.NET, colocando o contexto de dados em um projeto de biblioteca de classes do Visual Studio.

<a id="efdbf"></a>

### <a name="using-entity-framework-database-first-or-model-first-the-ef-designer"></a>Usando o Entity Framework Database First ou Model First (o EF Designer)

- [Introdução ao Entity Framework 6 Database First usando MVC 5](../mvc/overview/getting-started/database-first-development/setting-up-database.md). Executar um script no Gerenciador de servidores para criar um banco de dados e, em seguida, usar o designer do Entity Framework para criar o modelo de dados. Mostra como criar web CRUD simples de páginas e para outros funções que você pode seguir um dos tutoriais Code First, pois todos os fluxos de trabalho do EF usam a mesma API DbContext de manipulação de dados.

Os seguintes recursos são mais antigos. Eles são úteis se você deseja usar a versão 4.0 do Entity Framework, e você deseja usar um controle de fonte de dados para associação de dados em um aplicativo Web Forms.

- [Introdução ao Entity Framework 4.0](../web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). Mostra como usar o **EntityDataSource** controle.
- [Continuando com o Entity Framework](../web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)(mostra como usar o **ObjectDataSource** controle. Inclui um tutorial sobre o tratamento de simultaneidade, um tutorial sobre o desempenho do EF e um tutorial sobre o que há de novo no EF 4.0.

<a id="efrelateddata"></a>

### <a name="handling-related-data-in-entity-framework-lazy-loading-eager-loading-and-explicit-loading"></a>Manipulação de dados relacionados em Entity Framework (o carregamento lento, o carregamento adiantado e explícito de carregamento)

- [Lendo dados relacionados com o Entity Framework em um aplicativo ASP.NET MVC](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md). O Code First, aplicativo de exemplo do MVC. Os métodos mostrados também se aplicam a associação de modelos de formulários da Web e o banco de dados primeiro fluxo de trabalho.
- [Central de desenvolvedores de dados - Carregando entidades relacionadas](https://msdn.microsoft.com/data/jj574232) (MSDN). Documentação da equipe do Entity Framework sobre carregamento de dados relacionados.

<a id="optimizingef"></a>

### <a name="optimizing-entity-framework-performance"></a>Otimizando o desempenho do Entity Framework

- [Avançada de cenários de Entity Framework para um aplicativo ASP.NET](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application.md). Mostra como executar instruções SQL ou chamar seus próprios procedimentos armazenados, como desabilitar a detecção de alteração e como desabilitar a validação ao salvar as alterações.
- [Considerações sobre desempenho para o Entity Framework 5](https://msdn.microsoft.com/data/hh949853) (MSDN).
- [Considerações de desempenho (Entity Framework)](https://msdn.microsoft.com/library/cc853327) (MSDN).
- [Maximizar o desempenho com o Entity Framework em um aplicativo Web ASP.NET](../web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md). Aplica-se para o Entity Framework 4.0.
- Consulte também [ASP.NET otimizando o acesso a dados](#optimizingdataaccess) mais adiante neste tópico.

<a id="efconcurrency"></a>

### <a name="handling-concurrency-in-an-entity-framework-application"></a>Tratamento de simultaneidade em um aplicativo do Entity Framework

- [Tratamento de simultaneidade com o Entity Framework em um aplicativo ASP.NET MVC](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md). O Code First, a API DbContext, usando um aplicativo de exemplo do MVC.
- [Centro de desenvolvedores de dados – padrões de simultaneidade otimista](https://msdn.microsoft.cus/data/jj592904) (MSDN). Documentação de simultaneidade da equipe do Entity Framework.
- [Tratamento de simultaneidade com o Entity Framework em um aplicativo Web ASP.NET](../web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md). Aplica-se para o Entity Framework 4.0. Banco de dados em primeiro lugar, a API ObjectContext, usando um aplicativo de exemplo de formulários da Web.

<a id="efrepository"></a><a id="efbooks"></a>

### <a name="books-about-the-entity-framework"></a>Livros sobre o Entity Framework

- [Programming Entity Framework: DbContext](http://shop.oreilly.com/product/0636920022237.do) por Julie Lerman e Rowan Miller.
- [Programming Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) por Julie Lerman e Rowan Miller.

Esses dois livros são atualizados com as técnicas recomendadas atuais. Eles fornecem uma introdução mais abrangente e fácil de seguir para o Entity Framework do que qualquer coisa disponível na Internet. Outro livro [Programming Entity Framework](http://shop.oreilly.com/product/9780596807252.do) por Julie Lerman é maior e mais abrangente, mas ele é mais antigas e muitas das técnicas que ele abrange não são a maneira recomendada para usar o Entity Framework. Consulte também a lista de livros recomendados pela equipe do Entity Framework [Central de desenvolvedores de dados - manuais](https://msdn.microsoft.com/data/aa937716) no site do MSDN.

<a id="otherefresources"></a>

### <a name="other-entity-framework-resources"></a>Outros recursos do Entity Framework

- [Blog da equipe do Entity Framework (ADO.NET)](https://blogs.msdn.com/b/adonet/). Um dos melhores recursos para as informações mais atuais e os anúncios de novos aprimoramentos. Para outros blogs relacionadas ao EF, consulte o conjunto de blogs em [Introdução ao Entity Framework](https://msdn.microsoft.com/data/ee712907).
- [MSDN Magazine](https://msdn.microsoft.com/magazine/default.aspx). Consulte a **pontos de dados** coluna, que é frequentemente sobre tópicos relacionados à Entity Framework.

<a id="wfdatabinding"></a>

## <a name="data-binding-in-aspnet-web-forms-applications"></a>Aplicativos de vinculação de dados no ASP.NET Web Forms

- [Opções de acesso de dados de formulários da Web ASP.NET](https://msdn.microsoft.com/library/jj822927.aspx) (MSDN)<a id="wfmodelbinding"></a>.

<a id="wfmodelbinding"></a>

### <a name="using-web-forms-model-binding"></a>Usando o Web Forms a associação de modelos

- [Associação de modelos e formulários da Web](https://go.microsoft.com/fwlink/?LinkId=286117). Série de tutoriais usando o EF Code First.
- [Web Forms modelo associação parte 1: Selecionar dados](https://weblogs.asp.net/scottgu/archive/2011/09/06/web-forms-model-binding-part-1-selecting-data-asp-net-vnext-series.aspx) (blog de Guthrie). Essas postagens de blog mais antigo, a propriedade que atualmente é denominada ItemType foi nomeada ModelType, mas caso contrário, as informações que eles contêm são válidas.
- [Web Forms modelo associação parte 2: Filtragem de dados](https://weblogs.asp.net/scottgu/archive/2011/09/12/web-forms-model-binding-part-2-filtering-data-asp-net-vnext-series.aspx) (blog de Guthrie).
- [Web Forms modelo associação parte 3: Atualizando e validação](https://weblogs.asp.net/scottgu/archive/2011/10/30/web-forms-model-binding-part-3-updating-and-validation-asp-net-4-5-series.aspx) (blog de Guthrie).
- [Associação de modelos do ASP.NET 4.5 do Web Forms](../web-forms/videos/aspnet-web-forms-vnext/aspnet-45-web-forms-model-binding.md). (vídeo).
- [Associação de modelos parte 1: selecionar dados](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-1-selecting-data.md) (vídeo).
- [Associação de modelos parte 2 - filtragem](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-2-filtering.md) (vídeo).
- [Introdução aos formulários do ASP.NET Web 4.5 - exibir dados de itens e detalhes](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md).

<a id="wfdsc"></a>

### <a name="using-web-forms-data-source-controls"></a>Usando o Web Forms a controles de fonte de dados

- [Controles de servidor de Web de fonte de dados](https://msdn.microsoft.com/library/ms247258.aspx) (MSDN).
- [Anunciando a versão do provedor de dados dinâmicos e controle EntityDataSource para Entity Framework 6](https://blogs.msdn.com/b/webdev/archive/2014/02/28/announcing-the-release-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx) (blog de desenvolvimento da Web da Microsoft).

<a id="wfdbc"></a>

### <a name="using-web-forms-data-bound-controls-and-data-binding-expressions"></a>Usando o Web Forms controles ligados a dados e expressões de associação de dados

- [Associação de modelos e formulários da Web](https://go.microsoft.com/fwlink/?LinkId=286117). Série de tutoriais que usa o EF Code First.
- [Introdução aos formulários do ASP.NET Web 4.5 - exibir dados de itens e detalhes](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md).
- [Fortemente tipados dados controles](https://weblogs.asp.net/scottgu/archive/2011/09/02/strongly-typed-data-controls-asp-net-vnext-series.aspx) (blog de Guthrie).
- [Fortemente tipados dados controles](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) (vídeo).
- [ASP.NET 4.5 controles de dados tipados do Web Forms forte](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) (vídeo).
- [Controles de servidor Web associado a dados](https://msdn.microsoft.com/library/ms228214.aspx) (MSDN).
- [Visão geral de expressões de associação de dados](https://msdn.microsoft.com/library/ms178366.aspx) (MSDN). Esta página aborda apenas **Eval** e **associar**; ele não foi atualizado para incluir **Item** e **BindItem**.

<a id="sqlserver"></a>

## <a name="working-with-sql-server-databases"></a>Trabalhando com bancos de dados do SQL Server

- [Recursos de banco de dados do SQL Server](https://msdn.microsoft.com/library/hh230827.aspx) (MSDN). Para obter uma introdução geral para uma ampla variedade de tópicos do SQL Server, consulte as entradas abaixo no Sumário.
- [Edições do SQL Server](https://msdn.microsoft.com/library/ms178359.aspx#sqlserver) (MSDN). Um resumo das edições do SQL Server disponíveis, com links para obter mais informações sobre cada um deles.)
- [Cadeias de Conexão do SQL Server para aplicativos Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [Usando o SQL Server Compact para aplicativos Web ASP.NET](https://msdn.microsoft.com/library/ms247257.aspx) (MSDN).
- [Microsoft SQL Server: Amostras de produto do banco de dados](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). Bancos de dados AdventureWorks de exemplo.
- [Instalando bancos de dados de exemplo](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). Além dos métodos mostrados aqui, você também pode baixar um dos arquivos. mdf exemplo para o aplicativo\_pasta de dados de um projeto da web, converter o banco de dados para o LocalDB e criar uma cadeia de caracteres de conexão LocalDB. Para obter informações sobre como fazer isso, consulte [como: atualizar para o LocalDB](https://msdn.microsoft.com/library/hh873188.aspx).

Consulte também as seções a seguir sobre trabalhar com o SQL Server Express e LocalDB e escolhendo entre o SQL Server e banco de dados SQL.

<a id="sslocaldb"></a>

### <a name="working-with-sql-server-express-localdb-databases"></a>Trabalhando com bancos de dados do SQL Server Express LocalDB

- [SQL Server Express LocalDB 2012](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (MSDN). A introdução oficial do MSDN para o LocalDB.
- [Cadeias de Conexão do SQL Server para aplicativos Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [Como: atualizar para o LocalDB](https://msdn.microsoft.com/library/hh873188.aspx) (MSDN). Como migrar de um arquivo. mdf de uma versão anterior do SQL Server Express para o LocalDB. Você também precisa passar por esse processo se você baixar um dos [bancos de dados de exemplo do SQL Server 2012](https://go.microsoft.com/fwlink/?linkid=117483).
- [Introdução ao LocalDB, um SQL Express aprimorado](https://go.microsoft.com/fwlink/?LinkId=234375) (blog do SQL Server Express). Tem mais informações sobre por que o LocalDB foi criado do que está incluído no MSDN.
- [LocalDB: Onde está meu banco de dados?](https://go.microsoft.com/fwlink/?LinkId=234376) (Blog do SQL Server Express). Informações sobre onde os arquivos de banco de dados LocalDB são criados.
- [Usando LocalDB com o IIS completo, parte 1: perfil de usuário](https://blogs.msdn.com/b/sqlexpress/archive/2011/12/09/using-localdb-with-full-iis-part-1-user-profile.aspx) (blog do SQL Server Express). LocalDB não foi projetado para trabalhar com o IIS. Esta série de postagens de blog explica os problemas e algumas soluções alternativas.

<a id="sse"></a>

### <a name="working-with-sql-server-express-databases"></a>Trabalhando com bancos de dados do SQL Server Express

- [Cadeias de Conexão do SQL Server para aplicativos Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN). Se você usar a configuração de cadeia de conexão AttachDBFileName com o SQL Server Express, consulte especialmente a seção de instância de usuário desta página.
- [Como se apropriar de seu local do SQL Server Express 2008](https://blogs.msdn.com/b/sqlexpress/archive/2010/02/23/how-to-take-ownership-of-your-local-sql-server-2008-express.aspx) (blog do SQL Server Express). Um problema comum não está sendo capaz de trabalhar com bancos de dados SQL Server Express porque você não é um administrador na instância do SQL Server Express. Por padrão, somente a pessoa que instalou o SQL Server Express é um administrador. Este blog explica como a se tornar um administrador do SQL Server Express se você for um administrador no computador.
- [Meu aplicativo web do ASP.NET pode usar um banco de dados do SQL Server Express em produção?](https://msdn.microsoft.com/library/jj653753.aspx#sql_express_in_production) (MSDN).

<a id="ssdb"></a>

### <a name="working-with-windows-azure-sql-database"></a>Trabalhando com o Windows Azure SQL Database

- [Implantar um aplicativo MVC do ASP.NET seguro com associação, OAuth e banco de dados SQL para um Site do Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data) (site do Microsoft Azure).
- [Bancos de dados SQL](https://docs.microsoft.com/azure/sql-database/) (site do Microsoft Azure). Obter tutoriais de Introdução e guias de instruções.
- [Windows Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/ee336279.aspx) (MSDN). O nó de nível superior do índice de conteúdo para o banco de dados SQL no MSDN.
- [Índice de artigos do Windows Azure SQL banco de dados TechNet Wiki](https://social.technet.microsoft.com/wiki/contents/articles/2267.windows-azure-sql-database-technet-wiki-articles-index-en-us.aspx) (site do Microsoft TechNet).
- [Transient Fault Handling Application Block](https://msdn.microsoft.com/library/hh680934(v=PandP.50).aspx). Uma estrutura que permite a você para lidar com falhas de rede transitórios e erros de conexão que resultam da limitação. Disponível em um pacote do NuGet: [Enterprise Library 5.0 - Transient Fault Handling Application Block](http://nuget.org/packages/EnterpriseLibrary.WindowsAzure.TransientFaultHandling).
- [Introdução ao banco de dados SQL e Entity Framework](https://msdn.microsoft.com/data/jj556244) (MSDN).
- [Kit de treinamento do Azure Windows](https://www.microsoft.com/download/details.aspx?id=8396) (Centro de Download da Microsoft). Inclui laboratórios práticos para o banco de dados SQL.
- [Fórum da comunidade de banco de dados do Windows Azure SQL](https://social.msdn.microsoft.com/Forums/ssdsgetstarted/threads).
- [Mover para o Windows Azure SQL Database](https://msdn.microsoft.com/library/ff803375.aspx) (MSDN). Um capítulo de um cenário de ponta a ponta abrangente pela equipe do Microsoft Patterns and Practices. Por que você desejaria migrar de bastidores e como migrar do SQL Server para o banco de dados SQL.
- [Migrando bancos de dados do SQL Server para Windows Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx) (MSDN).
- [Assistente de migração de banco de dados SQL](http://sqlazuremw.codeplex.com/). Uma ferramenta de software livre para a migração de bancos de dados para e do banco de dados SQL.

<a id="ssdbchoosing"></a>

### <a name="choosing-between-sql-server-and-windows-azure-sql-database"></a>Escolhendo entre o SQL Server e Windows Azure SQL Database

- [Compare o SQL Server com o banco de dados SQL do Windows Azure](https://social.technet.microsoft.com/wiki/contents/articles/996.compare-sql-server-with-windows-azure-sql-database-en-us.aspx) (site do Microsoft TechNet).
- [Migração de dados para o Windows Azure SQL Database: ferramentas e técnicas](https://msdn.microsoft.com/library/windowsazure/hh694043.aspx) (MSDN). Inclui as seções que comparam o SQL Server para o banco de dados SQL e fornecem orientações sobre quando migrar do SQL Server para o banco de dados SQL.
- [Guia de entrega de banco de dados do Windows Azure SQL](https://social.technet.microsoft.com/wiki/contents/articles/3398.windows-azure-sql-database-delivery-guide-en-us.aspx) (site do Microsoft TechNet).
- [Limitações de recursos do SQL Server (Windows Azure SQL Database)](https://msdn.microsoft.com/library/windowsazure/ff394115.aspx) (MSDN).
- [Armazenamento de tabela do Azure do Windows e Windows Azure SQL Database - comparações e contrastes](https://msdn.microsoft.com/library/jj553018.aspx) (MSDN). Para um aplicativo que você implanta no Windows Azure, armazenamento de tabela do Windows Azure pode ser uma alternativa para o banco de dados SQL do Windows Azure. Este tópico ajuda você a decidir entre essas alternativas.
- [Windows Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/ee336279) (MSDN).
- [Diretrizes e limitações (banco de dados SQL do Azure do Windows)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx)

<a id="nosql"></a>

## <a name="working-with-nosql-database-management-systems"></a>Trabalhando com sistemas de gerenciamento de banco de dados NoSQL

- [Serviços de dados do Windows Azure](https://www.windowsazure.com/develop/net/data/) (site do Microsoft Azure). Consulte a [guia de recursos do serviço de tabela](https://docs.microsoft.com/azure/) e o **Big Data** seção da página.
- [ASP.NET multicamadas aplicativo usando o armazenamento de tabelas, filas e Blobs](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) (site do Microsoft Azure). Tutorial de ponta a ponta com o aplicativo de exemplo para download que usa tabelas de NoSQL do armazenamento do Windows Azure.

<a id="linq"></a>

## <a name="using-linq-queries-in-aspnet-applications"></a>Usando consultas LINQ em aplicativos ASP.NET

- [Opções de acesso de dados do ASP.NET](https://msdn.microsoft.com/library/ms178359.aspx#linq) (MSDN). Inclui uma introdução ao LINQ.
- [Vídeos de treinamento do LINQ](http://www.misfitgeek.com/windows-client-linq-training-videos-20/) (blog de Joe Stagner).
- [Thread de fórum ASP.NET com links para recursos do dynamic LINQ](https://forums.asp.net/p/1961037/5601994.aspx?Please+suggest+good+books+so+that+one+can+write+and+understand+dynamic+linq).

<a id="dd"></a>

## <a name="using-dynamic-data-scaffolding"></a>Usando o Scaffolding de dados dinâmicos

- [Modelos de projeto de dados dinâmicos](https://msdn.microsoft.com/library/jj822927.aspx#dynamicdata) (MSDN). Orientação sobre quando usar os projetos de dados dinâmicos.
- [Dados dinâmicos do ASP.NET](https://msdn.microsoft.com/library/ee845452.aspx) (MSDN).

<a id="securing"></a>

## <a name="securing-data-access"></a>Protegendo o acesso a dados

- [Protegendo o acesso a dados no ASP.NET](https://msdn.microsoft.com/library/ms178375.aspx) (MSDN).
- [Considerações de segurança (Entity Framework)](https://msdn.microsoft.com/library/cc716760.aspx) (MSDN).
- [Como: Proteger cadeias de caracteres de Conexão ao usar controles de fonte de dados](https://msdn.microsoft.com/library/ms178372.aspx) (MSDN).

<a id="optimizingdataaccess"></a>

## <a name="optimizing-data-access-performance"></a>Otimizando desempenho de acesso a dados

- [Visão geral de desempenho do ASP.NET](https://msdn.microsoft.com/library/cc668225.aspx) (MSDN).
- [O cache ASP.NET](https://msdn.microsoft.com/library/xsbfdd8c.aspx) (MSDN).
- [Melhorando o desempenho do ASP.NET](https://msdn.microsoft.com/library/ff647787) (MSDN). Há um aviso de "Conteúdo desativado" na parte superior desta página, mas a maioria das informações ainda são relevantes e não há nenhum recurso atualizado comparável.
- [Melhorando o desempenho do SQL Server](https://msdn.microsoft.com/library/ff647793) (MSDN). Comentário mesmo como o link anterior.

Consulte também [desempenho otimizando o Entity Framework](#optimizingef) anteriormente neste tópico.

<a id="deploying"></a>

## <a name="deploying-a-database"></a>Implantando um banco de dados

- [Recursos recomendados de implantação da Web do ASP.NET -](aspnet-web-deployment-content-map.md) (MSDN).

<a id="webservice"></a>

## <a name="accessing-data-through-a-web-service"></a>Acessando dados através de um serviço Web

- [Acessando dados através de um serviço Web](https://msdn.microsoft.com/library/ms178359.aspx#webservice) (MSDN). Orientação sobre quando usar a API da Web em comparação com o WCF.
- [Introdução à API Web ASP.NET](../web-api/index.md).
- [WCF Data Services](https://msdn.microsoft.com/data/bb931106) (MSDN).

<a id="additional"></a>

## <a name="additional-resources"></a>Recursos adicionais

- [Perguntas Frequentes de acesso de dados do ASP.NET](https://msdn.microsoft.com/library/jj653753.aspx) (MSDN).
- [Web Forms do ASP.NET tutoriais - dados](../web-forms/overview/data-access/index.md). A maioria destes tutoriais é relativamente antiga; não deixe de ler [opções de acesso de dados do ASP.NET](https://msdn.microsoft.com/library/ms178359.aspx) e [opções de armazenamento de dados (Criando aplicativos de nuvem de mundo Real com o Windows Azure)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md) primeiro para que você não vai muito longe um método de acesso de dados que não está correto para o seu cenário.
- [Mapa de conteúdo do ASP.NET MVC](../mvc/overview/getting-started/recommended-resources-for-mvc.md).
- [Tutoriais – dados de páginas da Web ASP.NET](../web-pages/overview/data/index.md).
- [Acessando dados no Visual Studio](https://msdn.microsoft.com/library/wzabh8c4.aspx) (MSDN). Fornece uma lista de links semelhantes a este mapa de conteúdo, mas com um foco no Visual Studio em vez de ASP.NET.
