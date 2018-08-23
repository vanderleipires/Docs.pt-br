---
uid: web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-cs
title: Uso de dependências de Cache SQL (c#) | Microsoft Docs
author: rick-anderson
description: A estratégia de cache mais simples é permitir que os dados armazenados em cache expirar após um período de tempo especificado. Mas essa abordagem simples significa que os dados armazenados em cache maintai...
ms.author: riande
ms.date: 05/30/2007
ms.assetid: 0e91842c-7f10-4aed-8c23-4ee3e2774014
msc.legacyurl: /web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-cs
msc.type: authoredcontent
ms.openlocfilehash: ddd0ce9e8e0f69da6f9c0f65165e4842d460f0c0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824664"
---
<a name="using-sql-cache-dependencies-c"></a>Uso de dependências de Cache SQL (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_61_CS.zip) ou [baixar PDF](using-sql-cache-dependencies-cs/_static/datatutorial61cs1.pdf)

> A estratégia de cache mais simples é permitir que os dados armazenados em cache expirar após um período de tempo especificado. Mas essa abordagem simples significa que os dados em cache não mantém nenhuma associação com sua fonte de dados subjacente, resultando em dados obsoletos que são mantidos muito longos ou dados atuais que expiraram muito em breve. Uma abordagem melhor é usar a classe SqlCacheDependency para que os dados permanecem em cache até que seus dados base foi modificados no banco de dados SQL. Este tutorial mostra como fazer isso.


## <a name="introduction"></a>Introdução

As técnicas de armazenamento em cache é examinado na [armazenando dados com o ObjectDataSource](caching-data-with-the-objectdatasource-cs.md) e [dados em cache na arquitetura](caching-data-in-the-architecture-cs.md) tutoriais usado uma expiração com base no tempo para remover os dados do cache após um especificado período. Essa abordagem é a maneira mais simples para equilibrar os ganhos de desempenho de armazenamento em cache em relação a desatualização limitada de dados. Selecionando uma expiração de tempo de *x* segundos, um desenvolvedor de página concedes para aproveitar os benefícios de desempenho de cache para apenas *x* segundos, mas pode ficar tranquilo que seus dados nunca serão obsoletos além do máximo dos *x* segundos. É claro que, para dados estáticos, *x* pode ser estendido para o tempo de vida do aplicativo web, como foi examinado na [armazenando dados na inicialização do aplicativo](caching-data-at-application-startup-cs.md) tutorial.

Ao armazenar em cache de banco de dados, uma expiração com base no tempo geralmente é escolhida por sua facilidade de uso, mas com frequência é uma solução inadequada. O ideal é que o banco de dados permanecerão armazenados em cache até que os dados subjacentes foi modificados no banco de dados; somente assim seria o cache ser removido. Essa abordagem maximiza os benefícios de desempenho de armazenamento em cache e minimiza a duração dos dados obsoletos. No entanto, para aproveitar esses benefícios lá deve ser algum sistema em vigor que sabe quando o banco de dados subjacente foi modificado e remove os itens correspondentes do cache. Antes do ASP.NET 2.0, os desenvolvedores de páginas eram responsáveis por implementar esse sistema.

O ASP.NET 2.0 oferece uma [ `SqlCacheDependency` classe](https://msdn.microsoft.com/library/system.web.caching.sqlcachedependency.aspx) e a infraestrutura necessária para determinar quando uma alteração ocorreu no banco de dados para que os itens correspondentes em cache pode ser removida. Existem duas técnicas para determinar quando os dados subjacentes foi alterada: notificação e sondagem. Após discutir as diferenças entre a sondagem e de notificação, vamos criar a infraestrutura necessária para dar suporte a sondagem e, em seguida, explorar como usar o `SqlCacheDependency` cenários classe declarativa e programaticamente.

## <a name="understanding-notification-and-polling"></a>Notificação de consulta e entendimento

Há duas técnicas que podem ser usadas para determinar quando os dados em um banco de dados foi modificados: notificação e sondagem. Com a notificação, o banco de dados de alerta automaticamente o tempo de execução do ASP.NET quando os resultados de uma consulta específica tem sido alterados desde a consulta foi executada pela última vez, no ponto em que os itens em cache associados à consulta são removidos. Com a sondagem, o servidor de banco de dados mantém informações sobre quando determinadas tabelas pela última vez foram atualizadas. O tempo de execução do ASP.NET periodicamente sonda o banco de dados para verificar quais tabelas foram alteradas desde que eles foram inseridos no cache. As tabelas cujos dados foram modificados têm seus itens de cache associado removidos.

A opção de notificação requer uma configuração de menor que a sondagem e é mais granular, já que rastreiam as alterações no nível da consulta em vez de em nível de tabela. Infelizmente, as notificações só estão disponíveis nas edições completas do Microsoft SQL Server 2005 (ou seja, as edições não Express). No entanto, a opção de sondagem pode ser usada para todas as versões do Microsoft SQL Server do 7.0 para 2005. Como esses tutoriais usam a edição Express do SQL Server 2005, vamos nos concentrar em como configurar e usar a opção de sondagem. Consulte a seção leitura adicional no final deste tutorial para obter mais recursos em recursos de notificação do SQL Server 2005 s.

Com a sondagem, o banco de dados deve ser configurado para incluir uma tabela denominada `AspNet_SqlCacheTablesForChangeNotification` que tem três colunas - `tableName`, `notificationCreated`, e `changeId`. Esta tabela contém uma linha para cada tabela que tem dados que talvez precisem ser usados em uma dependência de cache SQL no aplicativo web. O `tableName` coluna especifica o nome da tabela de enquanto `notificationCreated` indica a data e hora que a linha foi adicionada à tabela. O `changeId` coluna é do tipo `int` e tem um valor inicial de 0. Seu valor é incrementado a cada modificação à tabela.

Além de `AspNet_SqlCacheTablesForChangeNotification` tabela, o banco de dados também precisa incluir gatilhos em cada uma das tabelas que podem aparecer em uma dependência de cache SQL. Esses gatilhos são executados sempre que uma linha é inserida, atualizada ou excluída e incrementar a tabela s `changeId` valor em `AspNet_SqlCacheTablesForChangeNotification`.

O tempo de execução do ASP.NET rastreia atual `changeId` para uma tabela ao armazenar em cache de dados usando um `SqlCacheDependency` objeto. O banco de dados é verificado periodicamente e qualquer `SqlCacheDependency` objetos cuja `changeId` difere do valor no banco de dados são removidos desde um diferindo `changeId` valor indica que houve uma alteração na tabela desde que foi armazenado em cache os dados.

## <a name="step-1-exploring-theaspnetregsqlexecommand-line-program"></a>Etapa 1: Explorando o`aspnet_regsql.exe`programa de linha de comando

Com a abordagem de sondagem, o banco de dados deve ser configurado para conter a infra-estrutura descrita acima: uma tabela predefinida (`AspNet_SqlCacheTablesForChangeNotification`), um punhado de procedimentos armazenados e gatilhos em cada uma das tabelas que podem ser usadas em dependências de cache SQL na web aplicativo. Essas tabelas, procedimentos armazenados e gatilhos podem ser criados por meio do programa de linha de comando `aspnet_regsql.exe`, que é encontrado no `$WINDOWS$\Microsoft.NET\Framework\version` pasta. Para criar o `AspNet_SqlCacheTablesForChangeNotification` tabela e procedimentos armazenados associados, executados o seguinte na linha de comando:


[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample1.cmd)]

> [!NOTE]
> Para executar esses comandos que o logon de banco de dados especificado deve estar na [ `db_securityadmin` ](https://msdn.microsoft.com/library/ms188685.aspx) e [ `db_ddladmin` ](https://msdn.microsoft.com/library/ms190667.aspx) funções. Para examinar o T-SQL enviado ao banco de dados pela `aspnet_regsql.exe` programa de linha de comando, consulte [essa entrada de blog](http://scottonwriting.net/sowblog/posts/10709.aspx).


Por exemplo, para adicionar a infraestrutura para sondagem de um banco de dados do Microsoft SQL Server chamado `pubs` em um servidor de banco de dados denominado `ScottsServer` usando a autenticação do Windows, navegue até o diretório apropriado e, na linha de comando, digite:


[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample2.cmd)]

Depois que a infraestrutura de nível de banco de dados tiver sido adicionada, precisamos adicionar os disparadores para as tabelas que serão usadas em dependências de cache SQL. Usar o `aspnet_regsql.exe` linha de comando do programa novamente, mas especifique o nome de tabela usando o `-t` alternar e, em vez de usar o `-ed` alternar use `-et`, da seguinte forma:


[!code-html[Main](using-sql-cache-dependencies-cs/samples/sample3.html)]

Para adicionar os gatilhos para o `authors` e `titles` tabelas na `pubs` banco de dados `ScottsServer`, use:


[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample4.cmd)]

Para este tutorial adicionará os gatilhos para o `Products`, `Categories`, e `Suppliers` tabelas. Vamos examinar a sintaxe de linha de comando específica na etapa 3.

## <a name="step-2-referencing-a-microsoft-sql-server-2005-express-edition-database-inappdata"></a>Etapa 2: Fazendo referência a um Microsoft SQL Server 2005 Express Edition Database em`App_Data`

O `aspnet_regsql.exe` programa de linha de comando requer o nome do banco de dados e servidor para adicionar a infraestrutura necessária de sondagem. Mas qual é o nome do banco de dados e servidor para um banco de dados Microsoft SQL Server 2005 Express que reside no `App_Data` pasta? Em vez de precisar descobrir quais são os nomes de banco de dados e servidor, eu ve encontrado que a abordagem mais simples é anexar o banco de dados para o `localhost\SQLExpress` instância de banco de dados e renomeie os dados usando [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx). Se você tiver uma das versões completas do SQL Server 2005 instalado em seu computador, em seguida, você provavelmente já terá instalado no computador do SQL Server Management Studio. Se você tiver apenas a edição Express, você pode baixar gratuitamente [Microsoft SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796).

Comece fechando o Visual Studio. Em seguida, abra o SQL Server Management Studio e optar por conectar-se para o `localhost\SQLExpress` server usando a autenticação do Windows.


![Anexar o servidor localhost\SQLExpress](using-sql-cache-dependencies-cs/_static/image1.gif)

**Figura 1**: anexar o `localhost\SQLExpress` Server


Depois de se conectar ao servidor, o Management Studio mostrará o servidor e ter subpastas para os bancos de dados, segurança e assim por diante. Clique com botão direito na pasta de bancos de dados e escolha a opção de anexar. Isso abrirá a caixa de diálogo anexar bancos de dados caixa (veja a Figura 2). Clique no botão Adicionar e selecione o `NORTHWND.MDF` pasta de banco de dados em seu s do aplicativo web `App_Data` pasta.


[![Anexe o northwnd não. Banco de dados MDF da pasta App_Data](using-sql-cache-dependencies-cs/_static/image2.gif)](using-sql-cache-dependencies-cs/_static/image1.png)

**Figura 2**: anexar a `NORTHWND.MDF` do banco de dados de `App_Data` pasta ([clique para exibir a imagem em tamanho normal](using-sql-cache-dependencies-cs/_static/image2.png))


Isso adicionará o banco de dados para a pasta de bancos de dados. O nome do banco de dados pode ser o caminho completo para o arquivo de banco de dados ou o caminho completo é anexado com um [GUID](http://en.wikipedia.org/wiki/Globally_Unique_Identifier). Para evitar a necessidade de digitar esse nome de banco de dados longos ao usar o aspnet\_regsql.exe ferramenta de linha de comando, renomear o banco de dados para um nome mais amigável a humanos clicando com o banco de dados de apenas anexado e escolhendo renomear. Eu ve renomeado meu banco de dados para DataTutorials.


![Renomear o banco de dados anexado a um nome mais amigável a humanos](using-sql-cache-dependencies-cs/_static/image3.gif)

**Figura 3**: renomear o banco de dados anexado a um nome mais amigável a humanos


## <a name="step-3-adding-the-polling-infrastructure-to-the-northwind-database"></a>Etapa 3: Adicionando a infraestrutura de sondagem para o banco de dados Northwind

Agora que podemos ter anexado a `NORTHWND.MDF` do banco de dados a `App_Data` pasta, podemos está pronto para adicionar a infraestrutura de sondagem. Supondo que você tiver renomeado o banco de dados para DataTutorials, execute os seguintes quatro comandos:


[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample5.cmd)]

Depois de executar essas quatro comandos, clique com botão direito no nome do banco de dados no Management Studio, vá para o submenu de tarefas e escolha desanexar. Em seguida, feche o Management Studio e reabra o Visual Studio.

Depois que o Visual Studio tem reaberto, analise o banco de dados por meio do Gerenciador de servidores. Observe a nova tabela (`AspNet_SqlCacheTablesForChangeNotification`), o novo procedimentos armazenados e gatilhos em de `Products`, `Categories`, e `Suppliers` tabelas.


![O banco de dados agora inclui a infraestrutura necessária sondagem](using-sql-cache-dependencies-cs/_static/image4.gif)

**Figura 4**: O banco de dados agora inclui a infraestrutura necessária sondagem


## <a name="step-4-configuring-the-polling-service"></a>Etapa 4: Configurar o serviço de sondagem

Depois de criar as tabelas necessárias, gatilhos e procedimentos armazenados no banco de dados, a etapa final é configurar o serviço de sondagem, o que é feito por meio de `Web.config` especificando os bancos de dados para uso e a frequência de sondagem, em milissegundos. A marcação a seguir sonda uma vez por segundo de banco de dados Northwind.


[!code-xml[Main](using-sql-cache-dependencies-cs/samples/sample6.xml)]

O `name` o valor de `<add>` (NorthwindDB) do elemento associa um nome legível por humanos com um determinado banco de dados. Ao trabalhar com dependências de cache SQL, precisaremos fazer referência ao nome do banco de dados definido aqui, bem como os dados em cache com base na tabela. Veremos como usar o `SqlCacheDependency` classe para associar programaticamente as dependências de cache SQL com armazenados em cache dados na etapa 6.

Quando uma dependência de cache SQL tiver sido estabelecida, o sistema de sondagem se conectará aos bancos de dados definidos na `<databases>` elementos cada `pollTime` milissegundos e executar o `AspNet_SqlCachePollingStoredProcedure` procedimento armazenado. Esse procedimento armazenado - que foi adicionado de volta na etapa 3 usando o `aspnet_regsql.exe` ferramenta de linha de comando - retorna o `tableName` e `changeId` valores para cada registro em `AspNet_SqlCacheTablesForChangeNotification`. Dependências de cache SQL desatualizadas serão removidas do cache.

O `pollTime` configuração apresenta um equilíbrio entre desempenho e desatualização limitada de dados. Um pequeno `pollTime` valor aumenta o número de solicitações para o banco de dados, mas muito mais rapidamente remove dados obsoletos do cache. Uma maior `pollTime` valor reduz o número de solicitações de banco de dados, mas aumenta o atraso entre quando os dados de back-end são alterados e quando os itens de cache relacionadas são removidos. Felizmente, a solicitação de banco de dados está executando um procedimento armazenado simples que s retornando apenas algumas linhas de uma tabela simple e leve. Mas fazer experiências com diferentes `pollTime` valores para encontrar um equilíbrio ideal entre desatualização limitada de dados e acesso para seu aplicativo de banco de dados. O menor `pollTime` valor permitido é de 500.

> [!NOTE]
> O exemplo acima fornece uma única `pollTime` valor em de `<sqlCacheDependency>` elemento, mas você pode opcionalmente especificar o `pollTime` valor no `<add>` elemento. Isso é útil se você tiver vários bancos de dados especificados e para personalizar a frequência de sondagem por banco de dados.


## <a name="step-5-declaratively-working-with-sql-cache-dependencies"></a>Etapa 5: Declarativamente trabalhando com dependências de Cache SQL

As etapas 1 a 4 examinamos como configurar a infraestrutura de banco de dados necessários e configurar o sistema de pesquisa. Com essa infra-estrutura in-loco, podemos agora pode adicionar itens ao cache de dados com uma dependência de cache SQL associada usando técnicas de programação ou declarativas. Nesta etapa, examinaremos como declarativamente trabalhar com dependências de cache SQL. Etapa 6, examinaremos a abordagem programática.

O [armazenando dados com o ObjectDataSource](caching-data-with-the-objectdatasource-cs.md) tutorial explorou as capacidades declarativas de cache do ObjectDataSource. Simplesmente definindo a `EnableCaching` propriedade para `true` e o `CacheDuration` propriedade algum intervalo de tempo, o ObjectDataSource armazenará em cache automaticamente os dados retornados de seu objeto subjacente para o intervalo especificado. O ObjectDataSource também pode usar uma ou mais dependências de cache SQL.

Para demonstrar o uso de dependências de cache SQL declarativamente, abra o `SqlCacheDependencies.aspx` página o `Caching` pasta e arraste um controle GridView na caixa de ferramentas para o Designer. Definir o s GridView `ID` à `ProductsDeclarative` e, na marca inteligente, de escolha para associá-lo para um novo ObjectDataSource chamado `ProductsDataSourceDeclarative`.


[![Criar um novo ObjectDataSource chamado ProductsDataSourceDeclarative](using-sql-cache-dependencies-cs/_static/image5.gif)](using-sql-cache-dependencies-cs/_static/image3.png)

**Figura 5**: criar um novo ObjectDataSource nomeado `ProductsDataSourceDeclarative` ([clique para exibir a imagem em tamanho normal](using-sql-cache-dependencies-cs/_static/image4.png))


Configurar o ObjectDataSource para usar o `ProductsBLL` de classe e defina a lista suspensa na guia SELECT para `GetProducts()`. Na guia de atualização, escolha o `UpdateProduct` sobrecarga com três parâmetros de entrada - `productName`, `unitPrice`, e `productID`. Defina as listas suspensas para (nenhum) nas guias INSERT e DELETE.


[![Use a sobrecarga de UpdateProduct com três parâmetros de entrada](using-sql-cache-dependencies-cs/_static/image6.gif)](using-sql-cache-dependencies-cs/_static/image5.png)

**Figura 6**: Use a sobrecarga de UpdateProduct com três parâmetros de entrada ([clique para exibir a imagem em tamanho normal](using-sql-cache-dependencies-cs/_static/image6.png))


[![Definir a lista suspensa como (nenhum) para a inserção e exclusão guias](using-sql-cache-dependencies-cs/_static/image7.gif)](using-sql-cache-dependencies-cs/_static/image7.png)

**Figura 7**: defina a lista suspensa como (nenhum) para a inserção e excluir guias ([clique para exibir a imagem em tamanho normal](using-sql-cache-dependencies-cs/_static/image8.png))


Depois de concluir o Assistente Configurar fonte de dados, o Visual Studio criará BoundFields e CheckBoxFields no GridView para cada um dos campos de dados. Remover todos os campos, mas `ProductName`, `CategoryName`, e `UnitPrice`e formatar esses campos conforme necessário. Da GridView s marca inteligente, marque as caixas de seleção Habilitar paginação, habilitar a classificação e habilitar edição. Visual Studio definirá o s ObjectDataSource `OldValuesParameterFormatString` propriedade para `original_{0}`. Em ordem para o recurso de edição de s GridView funcione corretamente, remova essa propriedade inteiramente da sintaxe declarativa ou conjunto de volta ao seu valor padrão, `{0}`.

Por fim, adicione um controle de rótulo Web acima a GridView e defina sua `ID` propriedade para `ODSEvents` e seu `EnableViewState` propriedade `false`. Depois de fazer essas alterações, sua marcação declarativa de s de página deve ser semelhante ao seguinte. Observe que eu chegou uma série de personalizações estéticas para os campos de GridView que não são necessários para demonstrar a funcionalidade de dependência de cache SQL.


[!code-aspx[Main](using-sql-cache-dependencies-cs/samples/sample7.aspx)]

Em seguida, crie um manipulador de eventos para o s ObjectDataSource `Selecting` eventos e em, adicione o código a seguir:


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample8.cs)]

Lembre-se de que o s ObjectDataSource `Selecting` evento é acionado somente quando a recuperação de dados de seu objeto subjacente. Se o ObjectDataSource acessa os dados de seu próprio cache, esse evento não é acionado.

Agora, visite esta página por meio de um navegador. Desde que criamos ve ainda devem implementar qualquer cache, sempre que a página, classificar ou editar a grade de página deve exibir o texto, o evento Selecting acionado, como mostra a Figura 8.


[![O s ObjectDataSource selecionando evento é acionado cada vez GridView é paginado, editados, ou Sorted](using-sql-cache-dependencies-cs/_static/image8.gif)](using-sql-cache-dependencies-cs/_static/image9.png)

**Figura 8**: s o ObjectDataSource `Selecting` evento é acionado cada vez, GridView é paginado, editada ou Sorted ([clique para exibir a imagem em tamanho normal](using-sql-cache-dependencies-cs/_static/image10.png))


Como vimos na [armazenando dados com o ObjectDataSource](caching-data-with-the-objectdatasource-cs.md) tutorial, definindo o `EnableCaching` propriedade a ser `true` faz com que o ObjectDataSource para armazenar em cache seus dados para a duração especificada por seu `CacheDuration` propriedade. O ObjectDataSource também tem um [ `SqlCacheDependency` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.sqlcachedependency.aspx), que adiciona uma ou mais dependências de cache SQL para os dados em cache usando o padrão:


[!code-css[Main](using-sql-cache-dependencies-cs/samples/sample9.css)]

Onde *databaseName* é o nome do banco de dados conforme especificado na `name` atributo do `<add>` elemento `Web.config`, e *tableName* é o nome da tabela de banco de dados. Por exemplo criar um ObjectDataSource que armazena em cache os dados indefinidamente, com base em uma dependência de cache SQL em relação a s Northwind `Products` da tabela, definir o s ObjectDataSource `EnableCaching` propriedade a ser `true` e sua `SqlCacheDependency` propriedade para NorthwindDB:Products.

> [!NOTE]
> Você pode usar uma dependência de cache SQL *e* uma expiração com base no tempo, definindo `EnableCaching` à `true`, `CacheDuration` para o intervalo de tempo e `SqlCacheDependency` para o nome de banco de dados e tabela (s). O ObjectDataSource removerá seus dados quando a expiração do tempo for atingida ou quando o sistema de sondagem observa que o banco de dados subjacente foi alterado, o que ocorrer primeiro.


O GridView no `SqlCacheDependencies.aspx` exibe dados de duas tabelas - `Products` e `Categories` (o produto s `CategoryName` campo é recuperado por meio de um `JOIN` em `Categories`). Portanto, queremos especificar duas dependências de cache SQL: NorthwindDB:Products; NorthwindDB:Categories.


[![Configurar o ObjectDataSource para dar suporte a cache usando as dependências de Cache SQL em categorias e produtos](using-sql-cache-dependencies-cs/_static/image9.gif)](using-sql-cache-dependencies-cs/_static/image11.png)

**Figura 9**: Configure o ObjectDataSource para suporte ao cache usando o SQL dependências de Cache no `Products` e `Categories` ([clique para exibir a imagem em tamanho normal](using-sql-cache-dependencies-cs/_static/image12.png))


Depois de configurar o ObjectDataSource para dar suporte a armazenamento em cache, revisita a página por meio de um navegador. Novamente, o evento de seleção de texto acionado deve aparecer na primeira visita de página, mas deve desaparecer durante a paginação, classificação ou clicando nos botões de edição ou em Cancelar. Isso ocorre porque depois que os dados são carregados no cache de s ObjectDataSource, ela permanece lá até que o `Products` ou `Categories` tabelas são modificadas ou os dados são atualizados por meio de GridView.

Depois de paginação por meio da grade e observar a falta do evento Selecting acionado texto, abra uma nova janela do navegador e navegue até o tutorial de conceitos básicos de edição, inserção e exclusão de seção (`~/EditInsertDelete/Basics.aspx`). Atualize o nome ou o preço de um produto. Em seguida, de, para a primeira janela do navegador, exibir uma página de dados diferente, classificar a grade ou clique em um botão de edição de linha s. Neste momento, o evento Selecting acionado deverá reaparecer, como o banco de dados subjacente que dados foram modificadas (veja a Figura 10). Se o texto não aparecer, aguarde alguns instantes e tente novamente. Lembre-se de que o serviço de sondagem está verificando se há alterações para o `Products` tabela cada `pollTime` milissegundos, portanto, não há um atraso entre quando os dados subjacentes são atualizados e quando os dados em cache seja removidos.


[![Modificando a tabela de produtos remove os dados do produto armazenada em cache](using-sql-cache-dependencies-cs/_static/image10.gif)](using-sql-cache-dependencies-cs/_static/image13.png)

**Figura 10**: modificando a tabela de produtos remove os dados do produto armazenada em cache ([clique para exibir a imagem em tamanho normal](using-sql-cache-dependencies-cs/_static/image14.png))


## <a name="step-6-programmatically-working-with-thesqlcachedependencyclass"></a>Etapa 6: Trabalhando programaticamente com o`SqlCacheDependency`classe

O [dados em cache na arquitetura](caching-data-in-the-architecture-cs.md) tutorial examinou os benefícios de usar uma camada separada do serviço de cache na arquitetura em vez de acoplando com segurança o armazenamento em cache com o ObjectDataSource. No tutorial, criamos um `ProductsCL` para demonstrar a trabalhar programaticamente com o cache de dados. Para utilizar as dependências de cache SQL na camada de armazenamento em cache, use o `SqlCacheDependency` classe.

Com o sistema de sondagem, um `SqlCacheDependency` objeto deve ser associado um determinado par de banco de dados e tabela. O código a seguir, por exemplo, cria uma `SqlCacheDependency` objeto com base no banco de dados Northwind s `Products` tabela:


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample10.cs)]

Os dois parâmetros de entrada para o `SqlCacheDependency` construtor s são os nomes de banco de dados e tabela, respectivamente. Como com o ObjectDataSource s `SqlCacheDependency` propriedade, o nome de banco de dados usado é o mesmo que o valor especificado em de `name` atributo do `<add>` elemento no `Web.config`. O nome da tabela é o nome real da tabela de banco de dados.

Para associar uma `SqlCacheDependency` com um item adicionado ao cache de dados, use um do `Insert` sobrecargas de método que aceita uma dependência. O código a seguir adiciona *valor* para o cache de dados para uma duração indefinido, mas associa a um `SqlCacheDependency` no `Products` tabela. Em resumo, *valor* permanecerão no cache até que ele é removido devido a restrições de memória ou porque o sistema de sondagem detectou que o `Products` tabela foi alterado desde que foi armazenado em cache.


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample11.cs)]

A camada de armazenamento em cache s `ProductsCL` classe atualmente armazena em cache dados do `Products` tabela usando uma expiração com base no tempo de 60 segundos. Deixe o s atualizar essa classe para que ele usa as dependências de cache SQL em vez disso. O `ProductsCL` classe s `AddCacheItem` método, que é responsável por adicionar os dados no cache, no momento, contém o código a seguir:


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample12.cs)]

Atualizar este código para usar um `SqlCacheDependency` do objeto, em vez do `MasterCacheKeyArray` a dependência de cache:


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample13.cs)]

Para testar essa funcionalidade, adicione um controle GridView à página abaixo existente `ProductsDeclarative` GridView. Definir esse novo s GridView `ID` à `ProductsProgrammatic` e, por meio de sua marca inteligente, associá-lo a um novo ObjectDataSource chamado `ProductsDataSourceProgrammatic`. Configurar o ObjectDataSource para usar o `ProductsCL` classe, definindo as listas suspensas em SELECT e guias de atualização para `GetProducts` e `UpdateProduct`, respectivamente.


[![Configurar o ObjectDataSource para usar a classe ProductsCL](using-sql-cache-dependencies-cs/_static/image11.gif)](using-sql-cache-dependencies-cs/_static/image15.png)

**Figura 11**: configurar o ObjectDataSource para usar o `ProductsCL` classe ([clique para exibir a imagem em tamanho normal](using-sql-cache-dependencies-cs/_static/image16.png))


[![Selecione o método GetProducts na lista suspensa Selecione guia s](using-sql-cache-dependencies-cs/_static/image12.gif)](using-sql-cache-dependencies-cs/_static/image17.png)

**Figura 12**: selecione o `GetProducts` método de s selecione guia lista suspensa ([clique para exibir a imagem em tamanho normal](using-sql-cache-dependencies-cs/_static/image18.png))


[![Escolha o método UpdateProduct na lista de lista suspensa da guia s atualização](using-sql-cache-dependencies-cs/_static/image13.gif)](using-sql-cache-dependencies-cs/_static/image19.png)

**Figura 13**: escolha o método UpdateProduct de guia de atualização s lista suspensa ([clique para exibir a imagem em tamanho normal](using-sql-cache-dependencies-cs/_static/image20.png))


Depois de concluir o Assistente Configurar fonte de dados, o Visual Studio criará BoundFields e CheckBoxFields no GridView para cada um dos campos de dados. Como com o GridView primeiro adicionado a esta página, remova todos os campos, mas `ProductName`, `CategoryName`, e `UnitPrice`e formatar esses campos conforme necessário. Da GridView s marca inteligente, marque as caixas de seleção Habilitar paginação, habilitar a classificação e habilitar edição. Assim como acontece com o `ProductsDataSourceDeclarative` ObjectDataSource, o Visual Studio definirá a `ProductsDataSourceProgrammatic` s ObjectDataSource `OldValuesParameterFormatString` propriedade `original_{0}`. Para que o recurso de edição de s GridView funcionar corretamente, defina essa propriedade de volta para `{0}` (ou remover a atribuição de propriedade da sintaxe declarativa completamente).

Depois de concluir essas tarefas, a marcação declarativa GridView e ObjectDataSource resultante deve ser semelhante ao seguinte:


[!code-aspx[Main](using-sql-cache-dependencies-cs/samples/sample14.aspx)]

Para testar o SQL dependência de cache na camada de cache definir um ponto de interrupção na `ProductCL` classe s `AddCacheItem` método e, em seguida, para começar a depuração. Quando você visita primeiro `SqlCacheDependencies.aspx`, o ponto de interrupção deve ser atingido conforme os dados são solicitados pela primeira vez e colocados no cache. Em seguida, mover para outra página do GridView ou uma das colunas de classificação. Isso faz com que o GridView repetir a consulta de seus dados, mas os dados devem ser encontrados no cache desde o `Products` tabela de banco de dados não tiver sido modificada. Se os dados repetidamente não for encontrados no cache, verifique se há memória suficiente disponível no seu computador e tente novamente.

Após a paginação por meio de algumas páginas de GridView, abra uma segunda janela do navegador e navegue até o tutorial de conceitos básicos de edição, inserção e exclusão de seção (`~/EditInsertDelete/Basics.aspx`). Atualizar um registro da tabela Products e em seguida, na primeira janela de navegador, exibir uma nova página ou clicar em um dos cabeçalhos de classificação.

Neste cenário você verá uma das duas coisas: o ponto de interrupção será atingido, indicando que os dados em cache foi removidos devido à alteração no banco de dados; ou, o ponto de interrupção não será atingido, o que significa que `SqlCacheDependencies.aspx` está mostrando dados obsoletos. Se o ponto de interrupção não for atingido, provavelmente porque o serviço de sondagem ainda não foi disparado desde que os dados foi alterados. Lembre-se de que o serviço de sondagem está verificando se há alterações para o `Products` tabela cada `pollTime` milissegundos, portanto, não há um atraso entre quando os dados subjacentes são atualizados e quando os dados em cache seja removidos.

> [!NOTE]
> Esse atraso é mais provável apareçam ao editar um dos produtos por meio de GridView no `SqlCacheDependencies.aspx`. No [dados em cache na arquitetura](caching-data-in-the-architecture-cs.md) tutorial, adicionamos o `MasterCacheKeyArray` dependência para garantir que os dados que está sendo editados por meio de cache a `ProductsCL` classe s `UpdateProduct` método foi removido do cache. No entanto, isso foi substituído a dependência de cache ao modificar o `AddCacheItem` método no início desta etapa e, portanto, o `ProductsCL` classe continuará mostrar os dados em cache até que o sistema de sondagem observa a alteração para o `Products` tabela. Veremos como reintroduzir a `MasterCacheKeyArray` a dependência na etapa 7 de cache.


## <a name="step-7-associating-multiple-dependencies-with-a-cached-item"></a>Etapa 7: Associando várias dependências um Item em cache

Lembre-se de que o `MasterCacheKeyArray` dependência de cache é usada para garantir que *todos os* dados relacionados ao produto são removidos do cache quando qualquer outro item associado a ele é atualizado. Por exemplo, o `GetProductsByCategoryID(categoryID)` método caches `ProductsDataTables` instâncias para cada exclusivo *categoryID* valor. Se um desses objetos for removido, o `MasterCacheKeyArray` dependência de cache garante que os outros também serão removidos. Sem essa dependência de cache, quando os dados armazenados em cache são modificados existe a possibilidade que outros dados de produto em cache podem estar desatualizados. Consequentemente, ele importante que podemos manter a `MasterCacheKeyArray` a dependência de cache ao usar dependências de cache SQL. No entanto, os dados de cache s `Insert` método permite apenas para um objeto de dependência única.

Além disso, ao trabalhar com dependências de cache SQL é necessário associar várias tabelas de banco de dados como dependências. Por exemplo, o `ProductsDataTable` armazenado em cache na `ProductsCL` classe contém os nomes de categoria e fornecedor para cada produto, mas o `AddCacheItem` método usa apenas uma dependência no `Products`. Nessa situação, se o usuário atualiza o nome de uma categoria ou o fornecedor, os dados do produto armazenada em cache serão permanecem no cache e estar desatualizados. Portanto, queremos tornar os dados do produto armazenada em cache depende não apenas o `Products` de tabela, mas na `Categories` e `Suppliers` tabelas também.

O [ `AggregateCacheDependency` classe](https://msdn.microsoft.com/library/system.web.caching.aggregatecachedependency.aspx) fornece um meio para associar várias dependências de um item de cache. Comece criando um `AggregateCacheDependency` instância. Em seguida, adicione o conjunto de dependências usando o `AggregateCacheDependency` s `Add` método. Ao inserir o item em cache de dados depois disso, passe o `AggregateCacheDependency` instância. Quando *qualquer* da `AggregateCacheDependency` alterar de dependências de instância s, o item em cache será removido.

A seguir mostra o código atualizado para o `ProductsCL` classe s `AddCacheItem` método. O método cria o `MasterCacheKeyArray` dependência de cache junto com `SqlCacheDependency` objetos para o `Products`, `Categories`, e `Suppliers` tabelas. Eles são combinados em uma `AggregateCacheDependency` objeto nomeado `aggregateDependencies`, que é então passado para o `Insert` método.


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample15.cs)]

Teste esse código novo limite. Agora muda para o `Products`, `Categories`, ou `Suppliers` tabelas fazem com que os dados em cache a ser removido. Além disso, o `ProductsCL` classe s `UpdateProduct` método, que é chamado durante a edição de um produto por meio de GridView, remove as `MasterCacheKeyArray` dependência, o que faz com que o cache de cache `ProductsDataTable` a ser removido e os dados a ser recuperado novamente na próxima solicitação.

> [!NOTE]
> Dependências de cache SQL também podem ser usadas com [cache de saída](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/output.aspx). Para ver uma demonstração dessa funcionalidade, consulte: [usando o cache de saída do ASP.NET com o SQL Server](https://msdn.microsoft.com/library/e3w8402y(VS.80).aspx).


## <a name="summary"></a>Resumo

Ao armazenar em cache de banco de dados, os dados idealmente permanecerão no cache até que ele seja modificado no banco de dados. Com o ASP.NET 2.0, as dependências de cache do SQL podem ser criadas e usadas em cenários programáticos e declarativos. Um dos desafios dessa abordagem é em descobrir quando os dados foram modificados. As versões completas do Microsoft SQL Server 2005 fornecem recursos de notificação que podem alertar um aplicativo quando um resultado de consulta foi alterada. Para o Express Edition do SQL Server 2005 e versões mais antigas do SQL Server, um sistema de pesquisa deve ser usado em vez disso. Felizmente, configurando a infraestrutura necessária sondagem é bastante simples.

Boa programação!

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Usando notificações de consulta no Microsoft SQL Server 2005](https://msdn.microsoft.com/library/ms175110.aspx)
- [Criando uma notificação de consulta](https://msdn.microsoft.com/library/ms188669.aspx)
- [Armazenamento em cache no ASP.NET com o `SqlCacheDependency` classe](https://msdn.microsoft.com/library/ms178604(VS.80).aspx)
- [Ferramenta de registro do servidor SQL do ASP.NET (`aspnet_regsql.exe`)](https://msdn.microsoft.com/library/ms229862(vs.80).aspx)
- [Visão geral do `SqlCacheDependency`](http://www.aspnetresources.com/blog/sql_cache_depedency_overview.aspx)

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Revisores líder para este tutorial foram Marko Rangel Teresa Murphy e Hilton Giesenow. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](caching-data-at-application-startup-cs.md)
> [Próximo](caching-data-with-the-objectdatasource-vb.md)
