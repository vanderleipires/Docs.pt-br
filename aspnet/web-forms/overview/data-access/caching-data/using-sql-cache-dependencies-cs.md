---
uid: web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-cs
title: "Usando as dependências de Cache SQL (c#) | Microsoft Docs"
author: rick-anderson
description: "A estratégia de cache mais simples é permitir que os dados armazenados em cache expirar após um período de tempo especificado. Mas essa abordagem simples significa que os dados armazenados em cache maintai..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2007
ms.topic: article
ms.assetid: 0e91842c-7f10-4aed-8c23-4ee3e2774014
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-cs
msc.type: authoredcontent
ms.openlocfilehash: a6089b847dfd662e9b32128036170322823aac97
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="using-sql-cache-dependencies-c"></a>Usando as dependências de Cache SQL (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_61_CS.zip) ou [baixar PDF](using-sql-cache-dependencies-cs/_static/datatutorial61cs1.pdf)

> A estratégia de cache mais simples é permitir que os dados armazenados em cache expirar após um período de tempo especificado. Mas essa abordagem simples significa que os dados em cache não mantém nenhuma associação com sua fonte de dados subjacente, resultando em dados obsoletos são mantidos muito longos ou dados atuais que expiraram muito em breve. Uma abordagem melhor é usar a classe SqlCacheDependency para que os dados permanecerão armazenados em cache até que seus dados base foi modificados no banco de dados SQL. Este tutorial mostra como fazer isso.


## <a name="introduction"></a>Introdução

As técnicas de cache examinados no [cache de dados com o ObjectDataSource](caching-data-with-the-objectdatasource-cs.md) e [cache de dados na arquitetura do](caching-data-in-the-architecture-cs.md) tutoriais usado uma expiração baseada em tempo para remover os dados do cache depois de um especificado período. Essa abordagem é a maneira mais simples para equilibrar os ganhos de desempenho de armazenamento em cache em relação a desatualização associada a dados. Selecionando uma expiração de tempo de *x* segundos, um desenvolvedor de página concedes para aproveitar os benefícios de desempenho de cache somente para *x* segundos, mas pode ficar fácil que seus dados nunca serão atualizados mais que um máximo de *x* segundos. É claro que, para dados estáticos, *x* pode ser estendido para o tempo de vida do aplicativo da web, que foi examinado no [cache de dados na inicialização do aplicativo](caching-data-at-application-startup-cs.md) tutorial.

Ao armazenar em cache dados do banco de dados, uma expiração baseada em tempo geralmente é escolhida para sua facilidade de uso, mas frequentemente é uma solução inadequada. Idealmente, os dados do banco de dados permanecerá em cache até que os dados subjacentes foi modificados no banco de dados; somente então o cache deve ser removido. Essa abordagem maximiza os benefícios de desempenho do cache e minimiza a duração dos dados obsoletos. No entanto, para obter esses benefícios existe deve ser algumas que sabe quando o banco de dados subjacente foi modificado e remove os itens correspondentes do cache do sistema. Antes do ASP.NET 2.0, os desenvolvedores de página foram responsáveis por implementar neste sistema.

O ASP.NET 2.0 fornece um [ `SqlCacheDependency` classe](https://msdn.microsoft.com/en-us/library/system.web.caching.sqlcachedependency.aspx) e a infraestrutura necessária para determinar quando ocorreu uma alteração no banco de dados para que os itens correspondentes em cache pode ser removida. Existem duas técnicas para determinar quando os dados subjacentes mudou: notificação e sondagem. Após discutir as diferenças entre a notificação e sondagem, vamos criar a infraestrutura necessária para dar suporte a pesquisa e, em seguida, explorar como usar o `SqlCacheDependency` cenários classe declarativa e programaticamente.

## <a name="understanding-notification-and-polling"></a>Notificação de consulta e compreensão

Há duas técnicas que podem ser usadas para determinar quando os dados em um banco de dados foi modificados: notificação e sondagem. Com a notificação, o banco de dados alertas automaticamente o tempo de execução do ASP.NET quando os resultados de uma determinada consulta tem sido alterados desde a consulta foi executada pela última vez, no ponto em que os itens em cache associados à consulta são removidos. Com a sondagem, o servidor de banco de dados mantém informações sobre quando determinadas tabelas última foi atualizadas. O tempo de execução do ASP.NET periodicamente pesquisa o banco de dados para verificar se as tabelas foram alteradas desde que foram inseridas no cache. As tabelas cujos dados foram modificados tem seus itens de cache associada removidos.

A opção de notificação exige menos configuração de pesquisa e é mais granular, pois ele rastreia as alterações no nível da consulta em vez de em nível de tabela. Infelizmente, as notificações estão disponíveis somente nas edições completas do Microsoft SQL Server 2005 (ou seja, as edições não Express). No entanto, a opção de sondagem pode ser usada para todas as versões do Microsoft SQL Server 7.0 para 2005. Como esses tutoriais usam a edição Express do SQL Server 2005, nos concentraremos em configurar e usar a opção de sondagem. Consulte a seção de leitura adicional no final deste tutorial para obter mais recursos em recursos de notificação de s do SQL Server 2005.

Com a sondagem, o banco de dados deve ser configurado para incluir uma tabela chamada `AspNet_SqlCacheTablesForChangeNotification` que tem três colunas - `tableName`, `notificationCreated`, e `changeId`. Esta tabela contém uma linha para cada tabela que tem dados que talvez precisem ser usados em uma dependência de cache SQL no aplicativo web. O `tableName` coluna especifica o nome da tabela ao `notificationCreated` indica a data e hora que a linha foi adicionada à tabela. O `changeId` coluna é do tipo `int` e tem um valor inicial de 0. Seu valor é incrementado com cada modificação à tabela.

Além de `AspNet_SqlCacheTablesForChangeNotification` tabela, o banco de dados também precisa incluir gatilhos em cada uma das tabelas que podem aparecer em uma dependência de cache SQL. Esses gatilhos são executados sempre que uma linha é inserida, atualizada ou excluída e incrementar a tabela s `changeId` valor em `AspNet_SqlCacheTablesForChangeNotification`.

Controla o tempo de execução do ASP.NET atual `changeId` para uma tabela ao armazenar em cache dados usando um `SqlCacheDependency` objeto. O banco de dados é verificado periodicamente e qualquer `SqlCacheDependency` objetos cujo `changeId` difere do valor no banco de dados são removidos desde um difere `changeId` valor indica que houve uma alteração na tabela desde que foi armazenado em cache os dados.

## <a name="step-1-exploring-theaspnetregsqlexecommand-line-program"></a>Etapa 1: Explorar o`aspnet_regsql.exe`programa de linha de comando

Com a abordagem de sondagem, o banco de dados deve ser configurado para conter a infraestrutura descrita acima: uma tabela predefinida (`AspNet_SqlCacheTablesForChangeNotification`), uma série de procedimentos armazenados e gatilhos em cada uma das tabelas que podem ser usadas em dependências de cache SQL na web aplicativo. Essas tabelas, procedimentos armazenados e gatilhos podem ser criados por meio do programa de linha de comando `aspnet_regsql.exe`, que se encontra no `$WINDOWS$\Microsoft.NET\Framework\version` pasta. Para criar o `AspNet_SqlCacheTablesForChangeNotification` tabela e procedimentos armazenados associados, executados o seguinte na linha de comando:


[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample1.cmd)]

> [!NOTE]
> Para executar esses comandos, o logon de banco de dados especificado deve estar no [ `db_securityadmin` ](https://msdn.microsoft.com/en-us/library/ms188685.aspx) e [ `db_ddladmin` ](https://msdn.microsoft.com/en-us/library/ms190667.aspx) funções. Para examinar o T-SQL enviado ao banco de dados, o `aspnet_regsql.exe` programa de linha de comando, consulte [essa entrada de blog](http://scottonwriting.net/sowblog/posts/10709.aspx).


Por exemplo, para adicionar a infraestrutura de sondagem para um banco de dados do Microsoft SQL Server chamada `pubs` em um servidor de banco de dados denominado `ScottsServer` usando a autenticação do Windows, navegue até o diretório apropriado e, na linha de comando, digite:


[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample2.cmd)]

Depois que a infraestrutura de nível de banco de dados tiver sido adicionada, precisamos adicionar os gatilhos para as tabelas que serão usadas em dependências de cache SQL. Use o `aspnet_regsql.exe` linha de comando do programa novamente, mas especificar o nome de tabela usando o `-t` alternar e, em vez de usar o `-ed` alternar use `-et`, da seguinte forma:


[!code-html[Main](using-sql-cache-dependencies-cs/samples/sample3.html)]

Para adicionar os gatilhos para o `authors` e `titles` tabelas no `pubs` banco de dados `ScottsServer`, use:


[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample4.cmd)]

Para este tutorial, adicione os gatilhos para o `Products`, `Categories`, e `Suppliers` tabelas. Vamos examinar a sintaxe de linha de comando determinado na etapa 3.

## <a name="step-2-referencing-a-microsoft-sql-server-2005-express-edition-database-inappdata"></a>Etapa 2: Fazendo referência a um Microsoft SQL Server 2005 Express Edition banco de dados em`App_Data`

O `aspnet_regsql.exe` programa de linha de comando requer o nome do banco de dados e do servidor para adicionar a infraestrutura necessária sondagem. Mas qual é o nome de banco de dados e servidor para um banco de dados Microsoft SQL Server 2005 Express que reside o `App_Data` pasta? Em vez de precisar descobrir quais são os nomes de banco de dados e servidor, eu ve descobriu que a abordagem mais simples é anexar o banco de dados para o `localhost\SQLExpress` instância de banco de dados e renomear os dados usando [SQL Server Management Studio](https://msdn.microsoft.com/en-us/library/ms174173.aspx). Se você tiver uma das versões completas do SQL Server 2005 está instalado em seu computador, em seguida, você provavelmente já instalado no computador do SQL Server Management Studio. Se você tiver apenas a edição Express, você pode baixar a versão gratuita [Microsoft SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796).

Iniciar fechar o Visual Studio. Em seguida, abra o SQL Server Management Studio e optar por conectar-se para o `localhost\SQLExpress` server usando a autenticação do Windows.


![Anexar o servidor localhost\SQLExpress](using-sql-cache-dependencies-cs/_static/image1.gif)

**Figura 1**: anexar o `localhost\SQLExpress` Server


Depois de se conectar ao servidor, o Management Studio mostrará o servidor e ter subpastas para os bancos de dados, segurança e assim por diante. Com o botão direito na pasta de bancos de dados e escolha a opção de anexar. Isso abrirá a caixa de diálogo anexar bancos de dados caixa (veja a Figura 2). Clique no botão Adicionar e selecione o `NORTHWND.MDF` pasta do banco de dados em seu s do aplicativo web `App_Data` pasta.


[![Anexe o NORTHWND. Banco de dados MDF da pasta App_Data](using-sql-cache-dependencies-cs/_static/image2.gif)](using-sql-cache-dependencies-cs/_static/image1.png)

**Figura 2**: anexar o `NORTHWND.MDF` do banco de dados de `App_Data` pasta ([clique para exibir a imagem em tamanho normal](using-sql-cache-dependencies-cs/_static/image2.png))


Isso adicionará o banco de dados para a pasta de bancos de dados. O nome do banco de dados pode ser o caminho completo para o arquivo de banco de dados ou o caminho completo é anexado com um [GUID](http://en.wikipedia.org/wiki/Globally_Unique_Identifier). Para evitar a necessidade de digitar este nome de banco de dados longo ao usar o aspnet\_regsql.exe ferramenta de linha de comando, renomear o banco de dados para um nome mais amigável humanos clicando no banco de dados apenas anexado e escolha Renomear. Eu ve renomeado meu banco de dados para DataTutorials.


![Renomear o banco de dados anexado a um nome amigável mais humanos](using-sql-cache-dependencies-cs/_static/image3.gif)

**Figura 3**: renomear o banco de dados anexado a um nome amigável mais humanos


## <a name="step-3-adding-the-polling-infrastructure-to-the-northwind-database"></a>Etapa 3: Adicionando a infraestrutura de sondagem para o banco de dados Northwind

Agora que nós já foi anexado a `NORTHWND.MDF` do banco de dados de `App_Data` pasta, podemos re pronto para adicionar a infraestrutura de sondagem. Supondo que você tiver renomeado o banco de dados para DataTutorials, execute os seguintes quatro comandos:


[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample5.cmd)]

Depois de executar esses quatro comandos, com o botão direito no nome do banco de dados no Management Studio, vá para o submenu de tarefas e escolha desanexar. Em seguida, feche o Management Studio e reabra o Visual Studio.

Depois que o Visual Studio tem reaberto, analise o banco de dados por meio do Gerenciador de servidores. Observe a nova tabela (`AspNet_SqlCacheTablesForChangeNotification`), os novos procedimentos armazenados e gatilhos no `Products`, `Categories`, e `Suppliers` tabelas.


![O banco de dados agora inclui a infraestrutura necessária sondagem](using-sql-cache-dependencies-cs/_static/image4.gif)

**Figura 4**: O banco de dados agora inclui a infraestrutura necessária sondagem


## <a name="step-4-configuring-the-polling-service"></a>Etapa 4: Configurar o serviço de pesquisa

Depois de criar as tabelas necessárias, gatilhos e procedimentos armazenados no banco de dados, a etapa final é configurar o serviço de pesquisa, que é feito por meio de `Web.config` especificando os bancos de dados de uso e a frequência de sondagem em milissegundos. A seguinte marcação sonda o banco de dados Northwind uma vez por segundo.


[!code-xml[Main](using-sql-cache-dependencies-cs/samples/sample6.xml)]

O `name` valor o `<add>` (NorthwindDB) do elemento associa um nome legível com um determinado banco de dados. Ao trabalhar com dependências de cache SQL, precisaremos para se referir ao nome do banco de dados definido aqui, bem como os dados em cache com base na tabela. Veremos como usar o `SqlCacheDependency` classe para associar programaticamente as dependências de cache SQL com armazenado em cache dados na etapa 6.

Após o estabelecimento de uma dependência de cache SQL, o sistema de sondagem se conectará aos bancos de dados definidos no `<databases>` elementos cada `pollTime` milissegundos e execute o `AspNet_SqlCachePollingStoredProcedure` procedimento armazenado. Esse procedimento armazenado - que foi adicionado de volta na etapa 3 usando a `aspnet_regsql.exe` ferramenta de linha de comando - retorna o `tableName` e `changeId` valores para cada registro em `AspNet_SqlCacheTablesForChangeNotification`. Dependências de cache SQL desatualizadas são removidas do cache.

O `pollTime` configuração apresenta um equilíbrio entre desempenho e deterioração de dados. Um pequeno `pollTime` valor aumenta o número de solicitações para o banco de dados, mas mais rapidamente remove dados obsoletos do cache. Uma maior `pollTime` valor reduz o número de solicitações de banco de dados, mas aumenta o atraso entre quando os dados de back-end são alterados e quando os itens relacionados do cache são removidos. Felizmente, a solicitação de banco de dados é executar um procedimento armazenado simples que s retornando apenas algumas linhas de uma tabela simple e leve. Mas experimentar diferentes `pollTime` valores para localizar um equilíbrio ideal entre envelhecimento de acesso e os dados para seu aplicativo de banco de dados. O menor `pollTime` valor permitido é de 500.

> [!NOTE]
> O exemplo acima fornece um único `pollTime` valor no `<sqlCacheDependency>` elemento, mas você pode opcionalmente especificar o `pollTime` valor no `<add>` elemento. Isso é útil se você tiver vários bancos de dados especificados e para personalizar a frequência de sondagem por banco de dados.


## <a name="step-5-declaratively-working-with-sql-cache-dependencies"></a>Etapa 5: Declarativamente trabalhando com dependências de Cache do SQL

As etapas 1 a 4 examinamos como configurar a infraestrutura de banco de dados necessária e configurar o sistema de sondagem. Com essa infra-estrutura, podemos agora adicionar itens ao cache de dados com uma dependência de cache SQL associada usando técnicas programáticas e declarativas. Nesta etapa, examinaremos como declarativamente trabalhar com dependências de cache SQL. Etapa 6, examinaremos a abordagem programática.

O [cache de dados com o ObjectDataSource](caching-data-with-the-objectdatasource-cs.md) tutorial explorados as capacidades declarativas de cache do ObjectDataSource. Simplesmente definindo o `EnableCaching` propriedade `true` e `CacheDuration` propriedade para um intervalo de tempo, o ObjectDataSource armazenará automaticamente em cache os dados retornados de seu objeto subjacente para o intervalo especificado. ObjectDataSource também pode usar uma ou mais dependências de cache SQL.

Para demonstrar o uso de dependências de cache SQL declarativamente, abra o `SqlCacheDependencies.aspx` página o `Caching` pasta e arraste um controle GridView da caixa de ferramentas para o Designer. Definir o GridView s `ID` para `ProductsDeclarative` e, na sua marca inteligente, escolha para associá-lo para um novo ObjectDataSource denominado `ProductsDataSourceDeclarative`.


[![Criar um novo ObjectDataSource denominado ProductsDataSourceDeclarative](using-sql-cache-dependencies-cs/_static/image5.gif)](using-sql-cache-dependencies-cs/_static/image3.png)

**Figura 5**: criar um novo ObjectDataSource nomeado `ProductsDataSourceDeclarative` ([clique para exibir a imagem em tamanho normal](using-sql-cache-dependencies-cs/_static/image4.png))


Configurar o ObjectDataSource para usar o `ProductsBLL` de classe e defina a lista suspensa na guia SELECT para `GetProducts()`. Na guia de atualização, escolha o `UpdateProduct` sobrecarga com três parâmetros de entrada - `productName`, `unitPrice`, e `productID`. Defina as listas suspensas para (nenhum) nas guias de INSERT e DELETE.


[![Use a sobrecarga de UpdateProduct com três parâmetros de entrada](using-sql-cache-dependencies-cs/_static/image6.gif)](using-sql-cache-dependencies-cs/_static/image5.png)

**Figura 6**: usar a sobrecarga de UpdateProduct com três parâmetros de entrada ([clique para exibir a imagem em tamanho normal](using-sql-cache-dependencies-cs/_static/image6.png))


[![Definir a lista suspensa como (nenhum) para a inserção e excluir guias](using-sql-cache-dependencies-cs/_static/image7.gif)](using-sql-cache-dependencies-cs/_static/image7.png)

**Figura 7**: definir a lista suspensa como (nenhum) para a inserção e excluir guias ([clique para exibir a imagem em tamanho normal](using-sql-cache-dependencies-cs/_static/image8.png))


Depois de concluir o Assistente Configurar fonte de dados, o Visual Studio criará BoundFields e CheckBoxFields em GridView para cada um dos campos de dados. Remover todos os campos, mas `ProductName`, `CategoryName`, e `UnitPrice`e formatar esses campos como desejar. De GridView s marca inteligente, marque as caixas de seleção Habilitar paginação, Habilitar classificação e habilitar edição. O Visual Studio definirá o s ObjectDataSource `OldValuesParameterFormatString` propriedade `original_{0}`. Em ordem para o recurso de edição do GridView s funcione corretamente, remova essa propriedade inteiramente a sintaxe declarativa ou conjunto de volta ao seu valor padrão, `{0}`.

Finalmente, adicione um controle de rótulo Web acima a GridView e defina seu `ID` propriedade `ODSEvents` e sua `EnableViewState` propriedade `false`. Depois de fazer essas alterações, a marcação declarativa de s de página deve ser semelhante ao seguinte. Observe que, var feitas a um número de estéticos personalizações para os campos de GridView que não são necessários para demonstrar a funcionalidade de dependência de cache SQL.


[!code-aspx[Main](using-sql-cache-dependencies-cs/samples/sample7.aspx)]

Em seguida, crie um manipulador de eventos para o s ObjectDataSource `Selecting` eventos e no-adicione o seguinte código:


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample8.cs)]

Lembre-se de que o s ObjectDataSource `Selecting` evento é acionado somente quando a recuperação de dados de seu objeto subjacente. Se o ObjectDataSource acessa os dados de seu próprio cache, esse evento não é acionado.

Agora, visite esta página por meio de um navegador. Desde é ve ainda para implementar o armazenamento em cache, cada vez página, classificar ou editar a grade de página deve exibir o texto, o evento Selecting acionado, como mostra a Figura 8.


[![O s ObjectDataSource evento Selecting dispara a cada hora GridView é paginada, editar, ou Sorted](using-sql-cache-dependencies-cs/_static/image8.gif)](using-sql-cache-dependencies-cs/_static/image9.png)

**Figura 8**: s ObjectDataSource o `Selecting` evento é acionado cada vez, GridView é paginada, editada ou Sorted ([clique para exibir a imagem em tamanho normal](using-sql-cache-dependencies-cs/_static/image10.png))


Como vimos no [cache de dados com o ObjectDataSource](caching-data-with-the-objectdatasource-cs.md) tutorial, definindo o `EnableCaching` propriedade para `true` faz com que o ObjectDataSource para armazenar em cache os dados para a duração especificada por seu `CacheDuration` propriedade. ObjectDataSource também tem um [ `SqlCacheDependency` propriedade](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasource.sqlcachedependency.aspx), que adiciona um ou mais dependências de cache SQL para os dados em cache usando o padrão:


[!code-css[Main](using-sql-cache-dependencies-cs/samples/sample9.css)]

Onde *databaseName* é o nome do banco de dados conforme especificado no `name` atributo do `<add>` elemento `Web.config`, e *tableName* é o nome da tabela de banco de dados. Por exemplo criar um ObjectDataSource que armazena em cache os dados indefinidamente, com base em uma dependência de cache SQL em relação a s Northwind `Products` de tabela, defina o s ObjectDataSource `EnableCaching` propriedade `true` e sua `SqlCacheDependency` propriedade NorthwindDB:Products.

> [!NOTE]
> Você pode usar uma dependência de cache SQL *e* uma expiração baseada em tempo, definindo `EnableCaching` para `true`, `CacheDuration` para o intervalo de tempo e `SqlCacheDependency` os nomes de banco de dados e tabela. ObjectDataSource irá remover seus dados quando a expiração baseada em tempo for atingida ou quando o sistema de sondagem observa que o banco de dados subjacente foi alterado, o que ocorrer primeiro.


O GridView no `SqlCacheDependencies.aspx` exibe dados de duas tabelas - `Products` e `Categories` (produto s `CategoryName` campo é recuperado por meio de um `JOIN` em `Categories`). Portanto, queremos especificar duas dependências de cache SQL: NorthwindDB:Products; NorthwindDB:Categories.


[![Configurar o ObjectDataSource para dar suporte a cache usando as dependências de Cache SQL em produtos e categorias](using-sql-cache-dependencies-cs/_static/image9.gif)](using-sql-cache-dependencies-cs/_static/image11.png)

**Figura 9**: configurar o ObjectDataSource para suporte de cache usando Cache dependências do SQL em `Products` e `Categories` ([clique para exibir a imagem em tamanho normal](using-sql-cache-dependencies-cs/_static/image12.png))


Depois de configurar o ObjectDataSource para dar suporte a armazenamento em cache, revise a página por meio de um navegador. Novamente, o evento Selecting de texto acionado deve aparecer na primeira visita de página, mas deve desaparecer quando a paginação, classificação ou clicando nos botões de edição ou em Cancelar. Isso ocorre porque depois que os dados são carregados no cache s ObjectDataSource, ela permanece lá até que o `Products` ou `Categories` tabelas são modificadas ou os dados são atualizados por meio de GridView.

Depois de disparado paginação através da grade e observar a falta do evento Selecting texto, abra uma nova janela do navegador e navegue até o tutorial Noções básicas sobre a edição, inserção e exclusão de seção (`~/EditInsertDelete/Basics.aspx`). Atualize o nome ou o preço de um produto. Em seguida, de, para a primeira janela de navegador, exibir uma página de dados diferente, classificar a grade ou clique em um botão de edição de linha s. Neste momento, o evento Selecting acionado deve aparecer como o banco de dados subjacente que dados tiverem sido modificados (veja a Figura 10). Se o texto não for exibido, aguarde alguns instantes e tente novamente. Lembre-se de que o serviço de pesquisa está verificando se há alterações para o `Products` tabela cada `pollTime` milissegundos, para que haja um atraso entre quando os dados subjacentes são atualizados e quando os dados em cache seja removidos.


[![Modificando a tabela Produtos remove os dados do produto armazenado em cache](using-sql-cache-dependencies-cs/_static/image10.gif)](using-sql-cache-dependencies-cs/_static/image13.png)

**Figura 10**: modificar a tabela Produtos remove os dados do produto armazenada em cache ([clique para exibir a imagem em tamanho normal](using-sql-cache-dependencies-cs/_static/image14.png))


## <a name="step-6-programmatically-working-with-thesqlcachedependencyclass"></a>Etapa 6: Trabalhar programaticamente com a`SqlCacheDependency`classe

O [cache de dados na arquitetura](caching-data-in-the-architecture-cs.md) tutorial examinamos os benefícios do uso de uma camada de cache separada na arquitetura em vez de acoplamento totalmente o armazenamento em cache com o ObjectDataSource. Neste tutorial criamos um `ProductsCL` classe para demonstrar a trabalhar programaticamente com o cache de dados. Para utilizar as dependências de cache SQL na camada de armazenamento em cache, use o `SqlCacheDependency` classe.

Com o sistema de sondagem, um `SqlCacheDependency` objeto deve ser associado um determinado par de banco de dados e tabela. O código a seguir, por exemplo, cria um `SqlCacheDependency` objeto com base no banco de dados Northwind s `Products` tabela:


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample10.cs)]

Os dois parâmetros de entrada de `SqlCacheDependency` construtor s são os nomes de banco de dados e tabela, respectivamente. Como com o s ObjectDataSource `SqlCacheDependency` propriedade, o nome de banco de dados usado é o mesmo que o valor especificado no `name` atributo do `<add>` elemento `Web.config`. O nome da tabela é o nome real da tabela de banco de dados.

Para associar um `SqlCacheDependency` com um item adicionado ao cache de dados, use um do `Insert` sobrecargas do método que aceita uma dependência. O código a seguir adiciona *valor* no cache de dados durante um período indefinido, mas associa-o com um `SqlCacheDependency` no `Products` tabela. Em resumo, *valor* permanecerão no cache até que ele é removido devido a restrições de memória ou porque o sistema de sondagem detectou que o `Products` tabela foi alterado desde que ele foi armazenado em cache.


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample11.cs)]

A camada de cache s `ProductsCL` classe atualmente armazena em cache dados do `Products` tabela usando uma expiração baseada em tempo de 60 segundos. Permitir que o s atualizar esta classe para que ele usa as dependências de cache SQL em vez disso. O `ProductsCL` classe s `AddCacheItem` método, que é responsável por adicionar os dados no cache, contém o código a seguir:


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample12.cs)]

Atualizar este código para usar um `SqlCacheDependency` do objeto, em vez do `MasterCacheKeyArray` a dependência de cache:


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample13.cs)]

Para testar essa funcionalidade, adicione um controle GridView à página abaixo existente `ProductsDeclarative` GridView. Definir esse novo s GridView `ID` para `ProductsProgrammatic` e, por meio de sua marca inteligente, associá-lo a um novo ObjectDataSource denominado `ProductsDataSourceProgrammatic`. Configurar o ObjectDataSource para usar o `ProductsCL` classe, definindo as listas suspensas na selecionar e guias de atualização para `GetProducts` e `UpdateProduct`, respectivamente.


[![Configurar o ObjectDataSource para usar a classe ProductsCL](using-sql-cache-dependencies-cs/_static/image11.gif)](using-sql-cache-dependencies-cs/_static/image15.png)

**Figura 11**: configurar o ObjectDataSource para usar o `ProductsCL` classe ([clique para exibir a imagem em tamanho normal](using-sql-cache-dependencies-cs/_static/image16.png))


[![Selecione o método GetProducts na lista suspensa Selecione guia s](using-sql-cache-dependencies-cs/_static/image12.gif)](using-sql-cache-dependencies-cs/_static/image17.png)

**Figura 12**: selecione o `GetProducts` método de s selecione guia lista suspensa ([clique para exibir a imagem em tamanho normal](using-sql-cache-dependencies-cs/_static/image18.png))


[![Escolha o método UpdateProduct da lista de suspensa s do guia de atualização](using-sql-cache-dependencies-cs/_static/image13.gif)](using-sql-cache-dependencies-cs/_static/image19.png)

**Figura 13**: escolha o método UpdateProduct de guia de atualização s lista suspensa ([clique para exibir a imagem em tamanho normal](using-sql-cache-dependencies-cs/_static/image20.png))


Depois de concluir o Assistente Configurar fonte de dados, o Visual Studio criará BoundFields e CheckBoxFields em GridView para cada um dos campos de dados. Como com o GridView primeiro adicionado a esta página, remova todos os campos, mas `ProductName`, `CategoryName`, e `UnitPrice`e formatar esses campos como desejar. De GridView s marca inteligente, marque as caixas de seleção Habilitar paginação, Habilitar classificação e habilitar edição. Assim como acontece com o `ProductsDataSourceDeclarative` ObjectDataSource, o Visual Studio definirá o `ProductsDataSourceProgrammatic` ObjectDataSource s `OldValuesParameterFormatString` propriedade `original_{0}`. Para que o recurso de edição s GridView funcione corretamente, defina essa propriedade de volta para `{0}` (ou remover a atribuição de propriedade da sintaxe declarativa completamente).

Depois de concluir essas tarefas, a marcação de declarativa resultante GridView e ObjectDataSource deve parecer com o seguinte:


[!code-aspx[Main](using-sql-cache-dependencies-cs/samples/sample14.aspx)]

Para testar o SQL dependência de cache na camada de cache de definir um ponto de interrupção no `ProductCL` classe s `AddCacheItem` método e, em seguida, para começar a depuração. Quando você visita primeiro `SqlCacheDependencies.aspx`, o ponto de interrupção deve ser atingido, os dados são solicitados pela primeira vez e colocados no cache. Em seguida, ir para outra página em GridView ou uma das colunas de classificação. Isso faz com que o GridView repetir a consulta de seus dados, mas os dados devem ser encontrados no cache desde o `Products` tabela de banco de dados não foi modificada. Se os dados repetidamente não for encontrados no cache, verifique se há memória suficiente disponível em seu computador e tente novamente.

Depois de paginação por meio de algumas páginas de GridView, abra uma segunda janela do navegador e navegue até o tutorial Noções básicas sobre a edição, inserção e exclusão de seção (`~/EditInsertDelete/Basics.aspx`). Atualizar um registro da tabela Produtos e, em seguida, na primeira janela de navegador, exibir uma nova página ou clicar em um dos cabeçalhos de classificação.

Neste cenário você verá uma das duas coisas: o ponto de interrupção será atingido, indicando que os dados em cache foi removidos devido à alteração no banco de dados; ou, o ponto de interrupção não será alcançado, significando que `SqlCacheDependencies.aspx` estiver mostrando dados obsoletos. Se o ponto de interrupção não for atingido, provavelmente é porque o serviço de pesquisa ainda não foi disparado desde que os dados forem alterados. Lembre-se de que o serviço de pesquisa está verificando se há alterações para o `Products` tabela cada `pollTime` milissegundos, para que haja um atraso entre quando os dados subjacentes são atualizados e quando os dados em cache seja removidos.

> [!NOTE]
> Esse atraso é mais provável aparecer ao editar um dos produtos através de GridView no `SqlCacheDependencies.aspx`. No [cache de dados na arquitetura](caching-data-in-the-architecture-cs.md) tutorial adicionamos a `MasterCacheKeyArray` dependência para garantir que os dados que está sendo editados por meio de cache a `ProductsCL` classe s `UpdateProduct` método foi removido do cache. No entanto, essa dependência de cache é substituído ao modificar o `AddCacheItem` método anteriormente nesta etapa e, portanto, o `ProductsCL` classe continuará a exibir os dados em cache até que o sistema de sondagem notas a alteração a `Products` tabela. Veremos como reintroduzir o `MasterCacheKeyArray` a dependência na etapa 7 de cache.


## <a name="step-7-associating-multiple-dependencies-with-a-cached-item"></a>Etapa 7: Associar várias dependências um Item em cache

Lembre-se de que o `MasterCacheKeyArray` dependência de cache é usada para garantir que *todos os* dados relacionados ao produto são removidos do cache quando qualquer item único associado dentro dele é atualizada. Por exemplo, o `GetProductsByCategoryID(categoryID)` método caches `ProductsDataTables` instâncias para cada exclusivo *categoryID* valor. Se um desses objetos é removido, o `MasterCacheKeyArray` dependência de cache garante que os outros também serão removidos. Sem essa dependência de cache, quando os dados em cache são modificados existe a possibilidade de que outros dados de produto em cache podem estar desatualizados. Consequentemente, ele importante que mantemos a `MasterCacheKeyArray` a dependência de cache ao usar as dependências de cache SQL. No entanto, os dados de cache s `Insert` método permite apenas um objeto de dependência único.

Além disso, ao trabalhar com dependências de cache SQL é preciso associar várias tabelas de banco de dados como dependências. Por exemplo, o `ProductsDataTable` em cache no `ProductsCL` classe contém os nomes de categoria e fornecedor para cada produto, mas o `AddCacheItem` método usa apenas uma dependência no `Products`. Nessa situação, se o usuário atualiza o nome de uma categoria ou o fornecedor, os dados do produto armazenada em cache serão permanecem no cache e estar desatualizado. Portanto, nós queremos ter os dados armazenados em cache produto depende não apenas o `Products` tabela, mas no `Categories` e `Suppliers` tabelas também.

O [ `AggregateCacheDependency` classe](https://msdn.microsoft.com/en-us/library/system.web.caching.aggregatecachedependency.aspx) fornece um meio para associar várias dependências de um item de cache. Comece criando um `AggregateCacheDependency` instância. Em seguida, adicione o conjunto de dependências usando o `AggregateCacheDependency` s `Add` método. Ao inserir o item em cache de dados posteriormente, passar o `AggregateCacheDependency` instância. Quando *qualquer* do `AggregateCacheDependency` dependências de instância s alterar, o item em cache sejam removido.

A seguir mostra o código atualizado para o `ProductsCL` classe s `AddCacheItem` método. O método cria o `MasterCacheKeyArray` dependência de cache junto com `SqlCacheDependency` objetos para o `Products`, `Categories`, e `Suppliers` tabelas. Eles são combinados em uma `AggregateCacheDependency` objeto chamado `aggregateDependencies`, que é passado para o `Insert` método.


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample15.cs)]

Teste este novo código de saída. Agora é alterado para o `Products`, `Categories`, ou `Suppliers` tabelas fazer com que os dados em cache a ser removido. Além disso, o `ProductsCL` classe s `UpdateProduct` método, que é chamado durante a edição de um produto por meio do GridView, remove o `MasterCacheKeyArray` dependência, que faz com que o cache de cache `ProductsDataTable` a ser removido e os dados a serem recuperados novamente na próxima solicitação.

> [!NOTE]
> Dependências de cache SQL também podem ser usadas com [cache de saída](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/output.aspx). Para ver uma demonstração dessa funcionalidade, consulte: [usando o cache de saída do ASP.NET com o SQL Server](https://msdn.microsoft.com/en-us/library/e3w8402y(VS.80).aspx).


## <a name="summary"></a>Resumo

Ao armazenar em cache dados do banco de dados, os dados idealmente permanecerão no cache até que ele seja modificado no banco de dados. Com o ASP.NET 2.0, as dependências de cache do SQL podem ser criadas e usadas nos cenários programáticos e declarativos. Um dos desafios com essa abordagem é descobrir quando os dados foram modificados. Versões completas do Microsoft SQL Server 2005 fornecem recursos de notificação que um aplicativo podem gerar alertas quando um resultado de consulta foi alterada. Para o Express Edition do SQL Server 2005 e versões mais antigas do SQL Server, um sistema de pesquisa deve ser usado. Felizmente, configurando a infraestrutura necessária sondagem é bastante simples.

Boa programação!

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Usando notificações de consulta no Microsoft SQL Server 2005](https://msdn.microsoft.com/en-us/library/ms175110.aspx)
- [Criando uma notificação de consulta](https://msdn.microsoft.com/en-us/library/ms188669.aspx)
- [Armazenamento em cache no ASP.NET com o `SqlCacheDependency` classe](https://msdn.microsoft.com/en-us/library/ms178604(VS.80).aspx)
- [Ferramenta de registro do servidor SQL do ASP.NET (`aspnet_regsql.exe`)](https://msdn.microsoft.com/en-us/library/ms229862(vs.80).aspx)
- [Visão geral do`SqlCacheDependency`](http://www.aspnetresources.com/blog/sql_cache_depedency_overview.aspx)

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisores levar para este tutorial foram Marko Rangel Teresa Murphy e Giesenow Hilton. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](caching-data-at-application-startup-cs.md)
[Próximo](caching-data-with-the-objectdatasource-vb.md)
