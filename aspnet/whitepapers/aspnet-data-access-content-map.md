---
uid: whitepapers/aspnet-data-access-content-map
title: Acesso a dados do ASP.NET - recomendado recursos | Microsoft Docs
author: rick-anderson
description: "Este tópico fornece links para recursos de documentação sobre como acessar dados em aplicativos web ASP.NET, principalmente usando o Entity Framework e o SQL Server..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/25/2013
ms.topic: article
ms.assetid: f8157be1-4ab9-469e-ad3a-0ccc80b56c00
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-data-access-content-map
msc.type: content
ms.openlocfilehash: bf94b3dd6a1c450bf534f93747f217808ed3a30c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-data-access---recommended-resources"></a>Acesso a dados do ASP.NET - recomendado de recursos
====================
> Este tópico fornece links para recursos de documentação sobre como acessar dados em aplicativos web ASP.NET, principalmente usando o Entity Framework e o SQL Server.
> 
> Se você souber que um blog excelente postagem, [stackoverflow](http://stackoverflow.com) outros links que seriam úteis, ou thread [envie-em um email](mailto:aspnetue@microsoft.com?subject=Data Access Content Map) com o link.
> 
> Última atualização 4/3/2014


Este tópico contém as seguintes seções:

- [Introdução ao acesso a dados no ASP.NET](#gettingstarted)
- [Usando o Entity Framework](#ef)

    - [Usando o código do Entity Framework primeiro](#cf)
    - [Usando o Entity Framework Code First Migrations.](#efcfmigrations)
    - [Usando o banco de dados do Entity Framework primeiro ou o modelo primeiro (EF Designer)](#efdbf)
    - [Carregamento de dados relacionados no Entity Framework (carregamento preguiçoso, carregamento adiantado e carregamento explícito)](#efrelateddata)
    - [Otimizando o desempenho do Entity Framework](#optimizingef)
    - [Tratamento de simultaneidade em um aplicativo do Entity Framework](#efconcurrency)
    - [Manuais sobre o Entity Framework](#efbooks)
    - [Recursos adicionais do Entity Framework](#otherefresources)
- [Aplicativos de associação de dados em ASP.NET Web Forms](#wfdatabinding)

    - [Usar o Web Forms de associação de modelo](#wfmodelbinding)
    - [Usar o Web Forms controles de fonte de dados](#wfdsc)
    - [Usar o Web Forms controles associados a dados e expressões de associação de dados](#wfdbc)
- [Trabalhando com bancos de dados do SQL Server](#sqlserver)

    - [Trabalhando com bancos de dados do SQL Server Express LocalDB](#sslocaldb)
    - [Trabalhando com bancos de dados do SQL Server Express](#sse)
    - [Trabalhando com o banco de dados do Microsoft SQL Azure](#ssdb)
    - [Escolhendo entre o SQL Server e banco de dados do Microsoft SQL Azure](#ssdbchoosing)
- [Trabalhando com sistemas de gerenciamento de banco de dados NoSQL](#nosql)
- [Usando consultas LINQ em aplicativos ASP.NET](#linq)
- [Usando o scaffolding de dados dinâmicos](#dd)
- [Protegendo o acesso a dados](#securing)
- [Otimizar desempenho de acesso a dados](#optimizingdataaccess)
- [Implantando um banco de dados](#deploying)
- [Acessando dados através de um serviço Web](#webservice)
- [Recursos adicionais](#additional)

<a id="gettingstarted"></a>

## <a name="getting-started-with-data-access-in-aspnet"></a>Introdução ao acesso a dados no ASP.NET

- [Opções de armazenamento de dados (Criando aplicativos de nuvem do mundo Real com o Windows Azure)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md). Capítulo de um livro eletrônico sobre o desenvolvimento para a nuvem. Apresenta os bancos de dados NoSQL como uma alternativa que muitos desenvolvedores familiarizados com bancos de dados relacionais tendem a ignorar. Apresenta diretrizes sobre o que considerar ao escolher relacional ou NoSQL ou escolher uma plataforma específica.
- [Opções de acesso de dados do ASP.NET](https://msdn.microsoft.com/en-us/library/ms178359.aspx) (MSDN). Uma introdução às opções de acesso de dados para bancos de dados relacionais para o ASP.NET e orientação sobre como escolher plataformas e métodos que são apropriados para seu cenário de acesso.
- [Banco de dados relacional](http://en.wikipedia.org/wiki/Relational_database). Wikipédia). Se você não tiver trabalhado com bancos de dados relacionais, consulte esta página para obter uma introdução aos conceitos e terminologia de banco de dados relacional. Para obter uma introdução ao SQL Server em particular consulte [trabalhando com bancos de dados do SQL Server](#sqlserver) mais adiante neste tópico.

<a id="ef"></a>

## <a name="using-the-entity-framework"></a>Usando o Entity Framework

- [Abordagens de desenvolvimento do Entity Framework](https://msdn.microsoft.com/en-us/library/ms178359.aspx#dbfmfcf) (MSDN). Orientação sobre como escolher uma Entity Framework abordagem de desenvolvimento Database First, Model First ou Code First.

<a id="cf"></a>

### <a name="using-entity-framework-code-first"></a>Usando o código do Entity Framework primeiro
  

Os tutoriais a seguir oferecem aplicativos de exemplo para download:

- [Guia de Introdução ao EF 6 usando MVC 5](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Abrange uma ampla variedade de cenários de Entity Framework Code First, incluindo migrações e 6 EF recursos como resiliência de conexão, interceptação de comando e assíncrono. Isso é uma versão atualizada do [EF 5 / MVC 4 série](../mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). A série anterior inclui um tutorial no repositório e padrões de unidade de trabalho que não está incluído na nova série.
- [Introdução ao ASP.NET MVC 5](../mvc/overview/getting-started/introduction/getting-started.md). Abrange um cenários de primeiro intervalo de Entity Framework código mais estreita, mas faz um trabalho mais abrangente de introduzir recursos MVC.
- [Modelo de associação e formulários da Web](https://go.microsoft.com/fwlink/?LinkId=286117). Usa o Code First em um aplicativo de Web Forms.
- [Introdução ao ASP.NET 4.5 Web Forms](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Uma introdução aos formulários da Web com alguns cobertura do Code First. Usa associação de modelo.
- [Repositório de música MVC](../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1.md). Usa o Code First em um aplicativo MVC 3 de comércio eletrônico que também implementa a associação e a autorização. A versão do MVC e o ASP.NET (autenticação e autorização) sistema da associação usada aqui estão desatualizados; Para obter informações mais atualizadas sobre a associação do ASP.NET, consulte [https://asp.net/identity](https://asp.net/identity).

Outros recursos:

- [Entity Framework - código primeiro para um banco de dados](https://msdn.microsoft.com/en-us/data/jj200620). MSDN. Vídeo e instruções passo a passo que mostra como usar o Code First com um banco de dados existente.
- [Centro de desenvolvedores de dados - Entity Framework](https://msdn.microsoft.com/data/ef). MSDN. Para um guia de documentação do Entity Framework que foi criado e mantido pela equipe do Entity Framework, consulte o [começar](https://msdn.microsoft.com/en-us/data/ee712907) link.

Consulte também [manuais sobre o Entity Framework](#efbooks) e [recursos adicionais do Entity Framework](#otherefresources) mais adiante neste tópico.

<a id="efcfmigrations"></a>

### <a name="using-entity-framework-code-first-migrations"></a>Usando o Entity Framework Code First Migrations.
  

Maioria dos tutoriais do Code First listados acima migrações de cobertura. Consulte também os seguintes recursos.

- [Implantação de Web do ASP.NET usando o Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). parte 2 série tutorial que mostra como usar as migrações do Code First para implantar um banco de dados.
- [Implantar um aplicativo de seguro ASP.NET MVC 5 com associação, OAuth e o banco de dados SQL em um Site do Azure do Windows](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Microsoft Azure). Como usar as migrações para implantar o aplicativo e associação de dados no Azure.
- [Visão geral de implantação de Web do Visual Studio e ASP.NET](https://msdn.microsoft.com/en-us/library/dd394698.aspx#dbdeployment). Consulte o **configurar implantação de banco de dados no Visual Studio** seção para obter uma explicação de como as migrações do Code First é integrada ao recursos de implantação de web do Visual Studio.
- [Centro de desenvolvedores de dados - migrações do Code First](https://msdn.microsoft.com/en-us/data/jj591621) (MSDN). Documentação de migrações da equipe do Entity Framework.
- [A série Screencast migrações](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx). Blog EF). Três vídeos sobre tópicos avançados em migrações do Code First.
- [Migrações do Code First com Sites de páginas da Web do ASP.NET](http://www.mikesdotnetting.com/Article/217/Code-First-Migrations-With-ASP.NET-Web-Pages-Sites). Blog de Mikesdotnetting). Mostra como usar as migrações do Code First com um site de páginas da Web ASP.NET, colocando o contexto de dados em um projeto de biblioteca de classe do Visual Studio.

<a id="efdbf"></a>

### <a name="using-entity-framework-database-first-or-model-first-the-ef-designer"></a>Usando o banco de dados do Entity Framework primeiro ou o modelo primeiro (EF Designer)

- [Guia de Introdução ao Entity Framework 6 Database First usando MVC 5](../mvc/overview/getting-started/database-first-development/setting-up-database.md). Executar um script no Gerenciador de servidores para criar um banco de dados e, em seguida, usar o designer do Entity Framework para criar o modelo de dados. Mostra como criar web simple de CRUD páginas e para outros funções que você pode seguir um dos tutoriais do Code First, desde que todos os fluxos de trabalho do EF usam a mesma API DbContext de manipulação de dados.

Os seguintes recursos são mais antigos. Eles são úteis se você deseja usar a versão 4.0 do Entity Framework, e você deseja usar um controle de fonte de dados para associação de dados em um aplicativo de Web Forms.

- [Guia de Introdução com o Entity Framework 4.0](../web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). Mostra como usar o **EntityDataSource** controle.
- [Continuando com o Entity Framework](../web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)(mostra como usar o **ObjectDataSource** controle. Inclui um tutorial sobre manipulação de simultaneidade, um tutorial sobre o desempenho de EF e um tutorial sobre o que há de novo no EF 4.0.

<a id="efrelateddata"></a>

### <a name="handling-related-data-in-entity-framework-lazy-loading-eager-loading-and-explicit-loading"></a>Manipulação de dados relacionados em Entity Framework (carregamento preguiçoso, carregamento adiantado e carregamento explícito)

- [Lendo dados relacionados com o Entity Framework em um aplicativo ASP.NET MVC](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md). Primeiro, código de aplicativo de exemplo do MVC. Os métodos mostrados também se aplicam à associação de modelo de formulários da Web e o banco de dados primeiro fluxo de trabalho.
- [Centro de desenvolvedores de dados - carregar entidades relacionadas](https://msdn.microsoft.com/en-us/data/jj574232) (MSDN). Documentação da equipe do Entity Framework sobre carregamento de dados relacionados.

<a id="optimizingef"></a>

### <a name="optimizing-entity-framework-performance"></a>Otimizando o desempenho do Entity Framework

- [Entity Framework cenários avançados de um aplicativo ASP.NET](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application.md). Mostra como executar instruções SQL ou chamar procedimentos armazenados, como desabilitar a detecção de alteração e como desabilitar a validação ao salvar as alterações.
- [Considerações de desempenho para Entity Framework 5](https://msdn.microsoft.com/en-us/data/hh949853) (MSDN).
- [Considerações de desempenho (Entity Framework)](https://msdn.microsoft.com/en-us/library/cc853327) (MSDN).
- [Maximizar o desempenho com o Entity Framework em um aplicativo Web ASP.NET](../web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md). Aplica-se ao Entity Framework 4.0.
- Consulte também [acesso a dados ASP.NET otimizando](#optimizingdataaccess) mais adiante neste tópico.

<a id="efconcurrency"></a>

### <a name="handling-concurrency-in-an-entity-framework-application"></a>Tratamento de simultaneidade em um aplicativo do Entity Framework

- [Tratamento de simultaneidade com o Entity Framework em um aplicativo ASP.NET MVC](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md). Primeiro, o código DbContext API, usando um aplicativo de exemplo do MVC.
- [Centro de desenvolvedores de dados – padrões de simultaneidade otimista](https://msdn.microsoft.com/en-us/data/jj592904) (MSDN). Documentação de simultaneidade da equipe do Entity Framework.
- [Tratamento de simultaneidade com o Entity Framework em um aplicativo Web ASP.NET](../web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md). Aplica-se ao Entity Framework 4.0. Banco de dados pela primeira vez, API de ObjectContext, usando um aplicativo de exemplo de formulários da Web.

<a id="efrepository"></a><a id="efbooks"></a>

### <a name="books-about-the-entity-framework"></a>Manuais sobre o Entity Framework

- [Programação do Entity Framework: DbContext](http://shop.oreilly.com/product/0636920022237.do) Julie Lerman e Rowan Miller.
- [Programação do Entity Framework: Código primeiro](http://shop.oreilly.com/product/0636920022220.do) Julie Lerman e Rowan Miller.

Ambos são atualizados com as técnicas recomendadas atuais. Eles fornecem uma introdução mais abrangente e fácil de seguir para a estrutura de entidades que nada disponível na Internet. Outro catálogo, [Programming Entity Framework](http://shop.oreilly.com/product/9780596807252.do) por Julie Lerman é maior e mais abrangente, mas é mais antigas e muitas das técnicas abrange não são a maneira recomendada para usar o Entity Framework. Consulte também a lista de livros recomendado pela equipe do Entity Framework em [Data Developer Center - manuais](https://msdn.microsoft.com/en-us/data/aa937716) no site do MSDN.

<a id="otherefresources"></a>

### <a name="other-entity-framework-resources"></a>Outros recursos do Entity Framework

- [Blog da equipe do Entity Framework (ADO.NET)](https://blogs.msdn.com/b/adonet/). Um dos melhores recursos para as informações mais atuais e os lançamentos de novos aprimoramentos. Para outros blogs relacionados ao EF, consulte a rolagem de blog em [começar com o Entity Framework](https://msdn.microsoft.com/en-us/data/ee712907).
- [MSDN Magazine](https://msdn.microsoft.com/en-us/magazine/default.aspx). Consulte o **pontos de dados** coluna, que é frequentemente sobre tópicos relacionados à Entity Framework.

<a id="wfdatabinding"></a>

## <a name="data-binding-in-aspnet-web-forms-applications"></a>Aplicativos de associação de dados em ASP.NET Web Forms

- [Opções de acesso de dados de formulários da Web ASP.NET](https://msdn.microsoft.com/en-us/library/jj822927.aspx) (MSDN)<a id="wfmodelbinding"></a>.

<a id="wfmodelbinding"></a>

### <a name="using-web-forms-model-binding"></a>Usar o Web Forms de associação de modelo

- [Modelo de associação e formulários da Web](https://go.microsoft.com/fwlink/?LinkId=286117). Série de tutoriais usando EF Code First.
- [Web Forms modelo associação parte 1: Selecionar dados](https://weblogs.asp.net/scottgu/archive/2011/09/06/web-forms-model-binding-part-1-selecting-data-asp-net-vnext-series.aspx) (blog de Scott Guthrie). Nessas postagens de blog mais antigas, a propriedade que atualmente é denominada ItemType foi nomeada ModelType, mas caso contrário, as informações que eles contêm são válidas.
- [Web Forms modelo associação parte 2: Filtragem de dados](https://weblogs.asp.net/scottgu/archive/2011/09/12/web-forms-model-binding-part-2-filtering-data-asp-net-vnext-series.aspx) (blog de Scott Guthrie).
- [Web Forms modelo associação parte 3: Atualização e validação](https://weblogs.asp.net/scottgu/archive/2011/10/30/web-forms-model-binding-part-3-updating-and-validation-asp-net-4-5-series.aspx) (blog de Scott Guthrie).
- [Associação de modelo 4.5 Web Forms do ASP.NET](../web-forms/videos/aspnet-web-forms-vnext/aspnet-45-web-forms-model-binding.md). (vídeo).
- [Associação de parte 1: selecionar dados de modelo](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-1-selecting-data.md) (vídeo).
- [Modelo de associação parte 2 - filtragem](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-2-filtering.md) (vídeo).
- [Obtendo Introdução ao ASP.NET 4.5 Web Forms - exibir dados de itens e detalhes](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md).

<a id="wfdsc"></a>

### <a name="using-web-forms-data-source-controls"></a>Usar o Web Forms controles de fonte de dados

- [Controles de servidor de Web de fonte de dados](https://msdn.microsoft.com/en-us/library/ms247258.aspx) (MSDN).
- [Anunciando o lançamento do provedor de dados dinâmicos e controle de EntityDataSource para Entity Framework 6](https://blogs.msdn.com/b/webdev/archive/2014/02/28/announcing-the-release-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx) (blog de desenvolvimento da Web da Microsoft).

<a id="wfdbc"></a>

### <a name="using-web-forms-data-bound-controls-and-data-binding-expressions"></a>Usar o Web Forms controles associados a dados e expressões de associação de dados

- [Modelo de associação e formulários da Web](https://go.microsoft.com/fwlink/?LinkId=286117). Série de tutoriais que usa EF Code First.
- [Obtendo Introdução ao ASP.NET 4.5 Web Forms - exibir dados de itens e detalhes](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md).
- [Fortemente tipado dados controles](https://weblogs.asp.net/scottgu/archive/2011/09/02/strongly-typed-data-controls-asp-net-vnext-series.aspx) (blog de Scott Guthrie).
- [Fortemente tipado dados controles](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) (vídeo).
- [ASP.NET 4.5 controles de dados digitados do Web Forms forte](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) (vídeo).
- [Associação de dados de servidor Web](https://msdn.microsoft.com/en-us/library/ms228214.aspx) (MSDN).
- [Visão geral de expressões de associação de dados](https://msdn.microsoft.com/en-us/library/ms178366.aspx) (MSDN). Esta página só cobre **Eval** e **associar**; ele não foi atualizado para incluir **Item** e **BindItem**.

<a id="sqlserver"></a>

## <a name="working-with-sql-server-databases"></a>Trabalhando com bancos de dados do SQL Server

- [Recursos de banco de dados do SQL Server](https://msdn.microsoft.com/en-us/library/hh230827.aspx) (MSDN). Para obter uma introdução geral a uma ampla variedade de tópicos do SQL Server, consulte as entradas abaixo no Sumário.
- [Edições do SQL Server](https://msdn.microsoft.com/en-us/library/ms178359.aspx#sqlserver) (MSDN). Um resumo das edições do SQL Server disponíveis, com links para obter mais informações sobre cada um.)
- [Cadeias de Conexão do SQL Server para aplicativos Web ASP.NET](https://msdn.microsoft.com/en-us/library/jj653752.aspx) (MSDN).
- [Usando o SQL Server Compact para aplicativos Web ASP.NET](https://msdn.microsoft.com/en-us/library/ms247257.aspx) (MSDN).
- [Do Microsoft SQL Server: Exemplos de produtos de banco de dados](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). Bancos de dados de exemplo AdventureWorks.
- [Instalando bancos de dados de exemplo](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). Além dos métodos mostrados aqui, você também pode baixar um dos arquivos. mdf de exemplo para o aplicativo\_a pasta dados de um projeto da web, converter o banco de dados para o LocalDB e criar uma cadeia de caracteres de conexão do LocalDB. Para obter informações sobre como fazer isso, consulte [como: atualizar para o LocalDB](https://msdn.microsoft.com/en-us/library/hh873188.aspx).

Consulte também as seguintes seções em trabalhar com o SQL Server Express e LocalDB e escolhendo entre o SQL Server e banco de dados SQL.

<a id="sslocaldb"></a>

### <a name="working-with-sql-server-express-localdb-databases"></a>Trabalhando com bancos de dados do SQL Server Express LocalDB

- [O SQL Server Express LocalDB 2012](https://msdn.microsoft.com/en-us/library/hh510202(v=sql.110).aspx) (MSDN). A introdução oficial do MSDN para o LocalDB.
- [Cadeias de Conexão do SQL Server para aplicativos Web ASP.NET](https://msdn.microsoft.com/en-us/library/jj653752.aspx) (MSDN).
- [Como: atualizar para o LocalDB](https://msdn.microsoft.com/en-us/library/hh873188.aspx) (MSDN). Como migrar um arquivo. mdf de uma versão anterior do SQL Server Express para o LocalDB. Você também precisa passar por este processo se você baixar um do [bancos de dados de exemplo do SQL Server 2012](https://go.microsoft.com/fwlink/?linkid=117483).
- [Apresentando o LocalDB, uma melhor SQL Express](https://go.microsoft.com/fwlink/?LinkId=234375) (blog do SQL Server Express). Tem mais informações sobre por que o LocalDB foi criado que está incluída no MSDN.
- [LocalDB: Onde está meu banco de dados?](https://go.microsoft.com/fwlink/?LinkId=234376) (Blog do SQL Server Express). Informações sobre onde os arquivos de banco de dados LocalDB são criados.
- [Usando o LocalDB com o IIS completo, parte 1: perfil de usuário](https://blogs.msdn.com/b/sqlexpress/archive/2011/12/09/using-localdb-with-full-iis-part-1-user-profile.aspx) (blog do SQL Server Express). LocalDB não foi projetado para trabalhar com o IIS. Esta série de postagens de blog explica os problemas e algumas soluções alternativas.

<a id="sse"></a>

### <a name="working-with-sql-server-express-databases"></a>Trabalhando com bancos de dados do SQL Server Express

- [Cadeias de Conexão do SQL Server para aplicativos Web ASP.NET](https://msdn.microsoft.com/en-us/library/jj653752.aspx) (MSDN). Se você usar a configuração de cadeia de caracteres de conexão AttachDBFileName com o SQL Server Express, consulte especialmente a seção de instância de usuário desta página.
- [Como se apropriar do seu local do SQL Server Express 2008](https://blogs.msdn.com/b/sqlexpress/archive/2010/02/23/how-to-take-ownership-of-your-local-sql-server-2008-express.aspx) (blog do SQL Server Express). Um problema comum não é a possibilidade de trabalhar com bancos de dados SQL Server Express, pois você não for um administrador na instância do SQL Server Express. Por padrão, somente a pessoa que instalou o SQL Server Express é um administrador. Este blog explica como tornar-se um administrador do SQL Server Express se você for um administrador no computador.
- [Meu aplicativo web do ASP.NET pode usar um banco de dados do SQL Server Express em produção?](https://msdn.microsoft.com/en-us/library/jj653753.aspx#sql_express_in_production) (MSDN).

<a id="ssdb"></a>

### <a name="working-with-windows-azure-sql-database"></a>Trabalhando com o banco de dados do Microsoft SQL Azure

- [Implantar um aplicativo de proteger o ASP.NET MVC com associação, OAuth e o banco de dados SQL para um Site do Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data) (site do Microsoft Azure).
- [Bancos de dados SQL](https://docs.microsoft.com/azure/sql-database/) (site do Microsoft Azure). Obtendo tutoriais de Introdução e guias de instruções.
- [Windows Azure SQL Database](https://msdn.microsoft.com/en-us/library/windowsazure/ee336279.aspx) (MSDN). O nó de nível superior do índice de conteúdo para o banco de dados SQL no MSDN.
- [Windows Azure SQL Database TechNet Wiki artigos índice](https://social.technet.microsoft.com/wiki/contents/articles/2267.windows-azure-sql-database-technet-wiki-articles-index-en-us.aspx) (site Microsoft TechNet).
- [Transient Fault Handling Application Block](https://msdn.microsoft.com/en-us/library/hh680934(v=PandP.50).aspx). Uma estrutura que permite a você para lidar com falhas de rede transitório e erros de conexão que resultam da limitação. Disponível em um pacote do NuGet: [Enterprise Library 5.0 - Transient Fault Handling Application Block](http://nuget.org/packages/EnterpriseLibrary.WindowsAzure.TransientFaultHandling).
- [Guia de Introdução ao banco de dados SQL e o Entity Framework](https://msdn.microsoft.com/en-us/data/jj556244) (MSDN).
- [Kit de treinamento do Azure Windows](https://www.microsoft.com/en-us/download/details.aspx?id=8396) (Centro de Download da Microsoft). Inclui laboratórios práticos para o banco de dados SQL.
- [Fórum da comunidade do banco de dados SQL do Azure Windows](https://social.msdn.microsoft.com/Forums/en-US/ssdsgetstarted/threads).
- [Mover para o banco de dados do Windows Azure SQL](https://msdn.microsoft.com/en-us/library/ff803375.aspx) (MSDN). Um capítulo de um cenário de ponta a ponta abrangente pela equipe do Microsoft Patterns e práticas recomendadas. Abrange por que você talvez queira migrar e como migrar do SQL Server para o banco de dados SQL.
- [Migrando bancos de dados do SQL Server para Windows Azure SQL Database](https://msdn.microsoft.com/en-us/library/windowsazure/jj156160.aspx) (MSDN).
- [Assistente de migração de banco de dados do SQL](http://sqlazuremw.codeplex.com/). Uma ferramenta de software livre para a migração de bancos de dados para e de banco de dados SQL.

<a id="ssdbchoosing"></a>

### <a name="choosing-between-sql-server-and-windows-azure-sql-database"></a>Escolhendo entre o SQL Server e banco de dados do Microsoft SQL Azure

- [Compare o SQL Server com o banco de dados SQL do Windows Azure](https://social.technet.microsoft.com/wiki/contents/articles/996.compare-sql-server-with-windows-azure-sql-database-en-us.aspx) (site Microsoft TechNet).
- [Migração de dados no banco de dados do Microsoft SQL Azure: ferramentas e técnicas](https://msdn.microsoft.com/en-us/library/windowsazure/hh694043.aspx) (MSDN). Inclui as seções que comparam o SQL Server para o banco de dados SQL e fornecem orientação sobre quando a migração do SQL Server para o banco de dados SQL.
- [Guia de entrega de banco de dados do Windows Azure SQL](https://social.technet.microsoft.com/wiki/contents/articles/3398.windows-azure-sql-database-delivery-guide-en-us.aspx) (site Microsoft TechNet).
- [Limitações de recursos do SQL Server (banco de dados do Microsoft SQL Azure)](https://msdn.microsoft.com/en-us/library/windowsazure/ff394115.aspx) (MSDN).
- [Armazenamento de tabela do Azure do Windows e o banco de dados do Microsoft SQL Azure - semelhanças e diferenças](https://msdn.microsoft.com/en-us/library/jj553018.aspx) (MSDN). Para um aplicativo que você implanta no Windows Azure, o armazenamento de tabela do Windows Azure pode ser uma alternativa para o banco de dados SQL do Windows Azure. Este tópico ajuda você a decidir entre essas alternativas.
- [Windows Azure SQL Database](https://msdn.microsoft.com/en-us/library/windowsazure/ee336279) (MSDN).
- [Diretrizes e limitações (banco de dados do Microsoft SQL Azure)](https://msdn.microsoft.com/en-us/library/windowsazure/ff394102.aspx)

<a id="nosql"></a>

## <a name="working-with-nosql-database-management-systems"></a>Trabalhando com sistemas de gerenciamento de banco de dados NoSQL

- [Serviços de dados do Windows Azure](https://www.windowsazure.com/en-us/develop/net/data/) (site do Microsoft Azure). Consulte o [guia do recurso de serviço tabela](https://docs.microsoft.com/azure/) e **Big Data** seção da página.
- [Blobs, filas e tabelas de armazenamento ASP.NET multicamadas aplicativo usando](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) (site do Microsoft Azure). Tutorial de ponta a ponta com o aplicativo de exemplo disponível para download que usa tabelas de NoSQL de armazenamento do Windows Azure.

<a id="linq"></a>

## <a name="using-linq-queries-in-aspnet-applications"></a>Usando consultas LINQ em aplicativos ASP.NET

- [Opções de acesso de dados do ASP.NET](https://msdn.microsoft.com/en-us/library/ms178359.aspx#linq) (MSDN). Inclui uma introdução ao LINQ.
- [Vídeos do treinamento do LINQ](http://www.misfitgeek.com/windows-client-linq-training-videos-20/) (blog de Joe Stagner).
- [Threads de Fórum do ASP.NET com links para recursos dinâmicos de LINQ](https://forums.asp.net/p/1961037/5601994.aspx?Please+suggest+good+books+so+that+one+can+write+and+understand+dynamic+linq).

<a id="dd"></a>

## <a name="using-dynamic-data-scaffolding"></a>Usando o Scaffolding de dados dinâmicos

- [Modelos de projeto de dados dinâmicos](https://msdn.microsoft.com/en-us/library/jj822927.aspx#dynamicdata) (MSDN). Orientação sobre quando usar os projetos de dados dinâmicos.
- [Dados dinâmicos do ASP.NET](https://msdn.microsoft.com/en-us/library/ee845452.aspx) (MSDN).

<a id="securing"></a>

## <a name="securing-data-access"></a>Protegendo o acesso a dados

- [Protegendo o acesso a dados no ASP.NET](https://msdn.microsoft.com/en-us/library/ms178375.aspx) (MSDN).
- [Considerações de segurança (Entity Framework)](https://msdn.microsoft.com/en-us/library/cc716760.aspx) (MSDN).
- [Como: Proteger cadeias de caracteres de Conexão ao usar controles de fonte de dados](https://msdn.microsoft.com/en-us/library/ms178372.aspx) (MSDN).

<a id="optimizingdataaccess"></a>

## <a name="optimizing-data-access-performance"></a>Otimizar desempenho de acesso a dados

- [Visão geral de desempenho do ASP.NET](https://msdn.microsoft.com/en-us/library/cc668225.aspx) (MSDN).
- [Cache ASP.NET](https://msdn.microsoft.com/en-us/library/xsbfdd8c.aspx) (MSDN).
- [Melhorando o desempenho do ASP.NET](https://msdn.microsoft.com/en-us/library/ff647787) (MSDN). Há um aviso "Conteúdo obsoleto" na parte superior desta página, mas a maioria das informações ainda são relevantes e não há nenhum recurso atualizado comparável.
- [Melhorando o desempenho do SQL Server](https://msdn.microsoft.com/en-us/library/ff647793) (MSDN). Comentário mesmo como o link anterior.

Consulte também [desempenho otimizando o Entity Framework](#optimizingef) anteriormente neste tópico.

<a id="deploying"></a>

## <a name="deploying-a-database"></a>Implantando um banco de dados

- [Implantação da Web do ASP.NET - recomendado recursos](aspnet-web-deployment-content-map.md) (MSDN).

<a id="webservice"></a>

## <a name="accessing-data-through-a-web-service"></a>Acessando dados através de um serviço Web

- [Acessando dados através de um serviço Web](https://msdn.microsoft.com/en-us/library/ms178359.aspx#webservice) (MSDN). Orientação sobre quando usar a API da Web versus WCF.
- [Introdução ao ASP.NET Web API](../web-api/index.md).
- [WCF Data Services](https://msdn.microsoft.com/en-us/data/bb931106) (MSDN).

<a id="additional"></a>

## <a name="additional-resources"></a>Recursos adicionais

- [Acesso a dados ASP.NET perguntas frequentes sobre](https://msdn.microsoft.com/en-us/library/jj653753.aspx) (MSDN).
- [Web Forms do ASP.NET tutoriais - dados](../web-forms/overview/data-access/index.md). A maioria destes tutoriais é relativamente antiga; Certifique-se de ler [opções acesso a dados ASP.NET](https://msdn.microsoft.com/en-us/library/ms178359.aspx) e [opções de armazenamento de dados (Criando aplicativos de nuvem de mundo Real com o Windows Azure)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md) primeiro para que você não obtiver muito em um método de acesso de dados que não está correto para seu cenário.
- [Mapa de conteúdo do ASP.NET MVC](../mvc/overview/getting-started/recommended-resources-for-mvc.md).
- [ASP.NET Web Pages tutoriais - dados](../web-pages/overview/data/index.md).
- [Acessando dados no Visual Studio](https://msdn.microsoft.com/en-us/library/wzabh8c4.aspx) (MSDN). Fornece uma lista de links semelhante a esse mapa de conteúdo, mas com um foco no Visual Studio, em vez de ASP.NET.
