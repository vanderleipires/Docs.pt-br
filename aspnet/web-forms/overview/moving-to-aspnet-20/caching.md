---
uid: web-forms/overview/moving-to-aspnet-20/caching
title: Cache | Microsoft Docs
author: microsoft
description: "Um entendimento de cache é importante para um aplicativo ASP.NET bom desempenho. ASP.NET 1. x oferecidos três opções diferentes para armazenamento em cache; o cache de saída,..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 2bb109d2-e299-46ea-9054-fa0263b59165
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/caching
msc.type: authoredcontent
ms.openlocfilehash: 9b229de60e09b94189f62a6bb6fa61a9973d637b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="caching"></a>Cache
====================
por [Microsoft](https://github.com/microsoft)

> Um entendimento de cache é importante para um aplicativo ASP.NET bom desempenho. ASP.NET 1. x oferecidos três opções diferentes para armazenamento em cache; cache de saída, o cache de fragmento e a API de cache.


Um entendimento de cache é importante para um aplicativo ASP.NET bom desempenho. ASP.NET 1. x oferecidos três opções diferentes para armazenamento em cache; cache de saída, o cache de fragmento e a API de cache. O ASP.NET 2.0 oferece todos os três métodos, mas adiciona alguns recursos adicionais importantes. Há várias novas dependências de cache e os desenvolvedores agora tem a opção de criar dependências de cache personalizada. A configuração de armazenamento em cache também foram aprimorada significativamente no ASP.NET 2.0.

## <a name="new-features"></a>Novos recursos

## <a name="cache-profiles"></a>Perfis de cache

Perfis de cache permitem aos desenvolvedores definir as configurações de cache específico podem então ser aplicadas a páginas individuais. Por exemplo, se você tiver algumas páginas que devem ser expiradas do cache depois de 12 horas, você pode facilmente criar um perfil de cache que pode ser aplicado a essas páginas. Para adicionar um novo perfil de cache, use o &lt;outputCacheSettings&gt; seção no arquivo de configuração. Por exemplo, abaixo está a configuração de um perfil de cache chamado *twoday* que define a duração do cache de 12 horas.

[!code-xml[Main](caching/samples/sample1.xml)]

Para aplicar a este perfil de cache para uma página específica, use o atributo CacheProfile da diretiva @ OutputCache conforme mostrado abaixo:

[!code-aspx[Main](caching/samples/sample2.aspx)]

## <a name="custom-cache-dependencies"></a>Dependências de Cache personalizada

Os desenvolvedores do ASP.NET 1. x gritou para dependências de cache personalizada. No ASP.NET 1. x, a classe CacheDependency estava selado que os desenvolvedores impedidos derivem suas próprias classes-lo. No ASP.NET 2.0, essa limitação foi removida e os desenvolvedores estão livres para desenvolver suas próprias dependências de cache personalizada. A classe CacheDependency permite a criação de uma dependência de cache personalizada com base em arquivos, diretórios ou chaves de cache.

Por exemplo, o código a seguir cria uma nova dependência de cache personalizada com base em um arquivo chamado stuff.xml localizado na raiz do aplicativo Web:

[!code-csharp[Main](caching/samples/sample3.cs)]

Nesse cenário, quando o arquivo de stuff.xml for alterado, o item em cache é invalidado.

Também é possível criar uma dependência de cache personalizada usando chaves de cache. Usando esse método, a remoção da chave de cache invalidará os dados em cache. O exemplo a seguir ilustra isto:

[!code-csharp[Main](caching/samples/sample4.cs)]

Para invalidar o item que foi inserido acima, basta remova o item que foi inserido no cache para atuar como a chave de cache.

[!code-csharp[Main](caching/samples/sample5.cs)]

Observe que a chave do item que atua como a chave de cache deve ser o mesmo que o valor adicionado ao conjunto de chaves de cache.

## <a name="polling-based-sql-cache-dependenciesalso-called-table-based-dependencies"></a>Dependências de Cache de SQL com base em sondagem*(também chamado de dependências com base em tabela)*

SQL Server 7 e 2000 usar o modelo de sondagem para dependências de cache SQL. O modelo baseado em pesquisa usa um gatilho em uma tabela de banco de dados que é disparada quando alterar dados na tabela. Que devem disparar atualizações um **changeId** campo na tabela de notificação que o ASP.NET verifica periodicamente. Se o **changeId** campo foi atualizado, o ASP.NET sabe que os dados foram alterados e invalida os dados armazenados em cache.

> [!NOTE]
> SQL Server 2005 também pode usar o modelo de sondagem, mas como o modelo de sondagem não é o modelo mais eficiente, recomenda-se usar um modelo baseado em consulta (abordado posteriormente) com o SQL Server 2005.


Em ordem de uma dependência de cache SQL usando o modelo baseado em pesquisa funcione corretamente, as tabelas devem ter notificações habilitadas. Isso pode ser feito por meio de programação usando a classe SqlCacheDependencyAdmin ou usando o aspnet\_regsql.exe utilitário.

A seguinte linha de comando registra a tabela Produtos no banco de dados Northwind, localizado em uma instância do SQL Server denominada *dbase* dependência de cache do SQL.

[!code-console[Main](caching/samples/sample6.cmd)]

Esta é uma explicação sobre as opções de linha de comando usada no comando acima:

| **Opção de linha de comando** | **Finalidade** |
| --- | --- |
| -S *server* | Especifica o nome do servidor. |
| -ed | Especifica que o banco de dados deve estar habilitado para dependência de cache SQL. |
| -d *banco de dados\_nome* | Especifica o nome do banco de dados que deve ser habilitado para dependência de cache SQL. |
| -E | Especifica que aspnet\_regsql deve usar a autenticação do Windows ao conectar-se ao banco de dados. |
| -et | Especifica que nós estiver habilitando uma tabela de banco de dados para dependência de cache SQL. |
| -t *tabela\_nome* | Especifica o nome da tabela de banco de dados para habilitar a dependência de cache do SQL. |

> [!NOTE]
> Há outras opções disponíveis para aspnet\_regsql.exe. Para obter uma lista completa, execute aspnet\_regsql.exe-? em uma linha de comando.


Quando esse comando executa as seguintes alterações são feitas no banco de dados do SQL Server:

- Um **AspNet\_SqlCacheTablesForChangeNotification** tabela será adicionada. Esta tabela contém uma linha para cada tabela no banco de dados para o qual uma dependência de cache SQL foi habilitada.
- Os seguintes procedimentos armazenados são criados dentro do banco de dados:


| AspNet\_SqlCachePollingStoredProcedure | Consulta o AspNet\_SqlCacheTablesForChangeNotification tabela e retorna todas as tabelas habilitadas para dependência de cache SQL e o valor de changeId para cada tabela. Esse procedimento armazenado é usado para que a sondagem para determinar se os dados foram alterados. |
| --- | --- |
| AspNet\_SqlCacheQueryRegisteredTablesStoredProcedure | Retorna todas as tabelas habilitadas para dependência de cache do SQL, consultando o AspNet\_SqlCacheTablesForChangeNotification tabela e retorna todas as tabelas habilitadas para o SQL a dependência de cache. |
| AspNet\_SqlCacheRegisterTableStoredProcedure | Registra uma tabela para dependência de cache SQL, adicionando a entrada necessária na tabela de notificação e adiciona o gatilho. |
| AspNet\_SqlCacheUnRegisterTableStoredProcedure | Cancela o registro de uma tabela para dependência de cache SQL, removendo a entrada na tabela de notificação e remove o gatilho. |
| AspNet\_SqlCacheUpdateChangeIdStoredProcedure | Atualiza a tabela de notificação, incrementando changeId para a tabela alterada. O ASP.NET usa este valor para determinar se os dados foram alterados. Como indicado abaixo, este procedimento armazenado é executado pelo gatilho criado quando a tabela está habilitada. |


- Um gatilho de servidor SQL chamado ***tabela\_nome *\_AspNet\_SqlCacheNotification\_gatilho** é criado para a tabela. Esse gatilho é executado o AspNet\_SqlCacheUpdateChangeIdStoredProcedure quando um INSERT, UPDATE ou DELETE é executada na tabela.
- Uma função de servidor SQL chamado **aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess** é adicionado ao banco de dados.

O **aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess** função do SQL Server tem permissões de EXEC para a AspNet\_SqlCachePollingStoredProcedure. Para o modelo de sondagem para funcionar corretamente, você deve adicionar sua conta de processo para o aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess função. O aspnet\_regsql.exe ferramenta não fará isso para você.

### <a name="configuring-polling-based-sql-cache-dependencies"></a>Configurando dependências de Cache SQL com base em pesquisa

Há várias etapas que são necessárias para configurar dependências de cache SQL com base em pesquisa. A primeira etapa é habilitar o banco de dados e a tabela como discutido acima. Quando essa etapa for concluída, o restante da configuração é o seguinte:

- Configurando o arquivo de configuração do ASP.NET.
- Configurando o SqlCacheDependency

### <a name="configuring-the-aspnet-configuration-file"></a>O arquivo de configuração do ASP.NET

Além de adicionar uma cadeia de caracteres de conexão, conforme discutido em um módulo anterior, você também deve configurar um &lt;cache&gt; elemento com um &lt;sqlCacheDependency&gt; elemento conforme mostrado abaixo:

[!code-xml[Main](caching/samples/sample7.xml)]

Essa configuração permite que uma dependência de cache SQL no *pubs* banco de dados. Observe que o pollTime atributo o &lt;sqlCacheDependency&gt; elemento padrões para 60.000 milissegundos ou 1 minuto. (Esse valor não pode ser menor que 500 milissegundos). Neste exemplo, o &lt;adicionar&gt; elemento adiciona um novo banco de dados e substituições pollTime, configurá-lo como 9000000 milissegundos.

#### <a name="configuring-the-sqlcachedependency"></a>Configurando o SqlCacheDependency

A próxima etapa é configurar o SqlCacheDependency. A maneira mais fácil de fazer isso é especificar o valor para o atributo SqlDependency na diretiva @ Outcache da seguinte maneira:

[!code-aspx[Main](caching/samples/sample8.aspx)]

A diretiva @ OutputCache acima, uma dependência de cache SQL é configurada para o *autores* tabela o *pubs* banco de dados. Várias dependências podem ser configuradas, separando-os com um ponto e vírgula da seguinte forma:

[!code-aspx[Main](caching/samples/sample9.aspx)]

Outro método de configuração de SqlCacheDependency é para fazer isso programaticamente. O código a seguir cria uma nova dependência de cache SQL no *autores* tabela o *pubs* banco de dados.

[!code-csharp[Main](caching/samples/sample10.cs)]

Uma das vantagens de definir programaticamente a dependência de cache do SQL é que você pode manipular as exceções que podem ocorrer. Por exemplo, se você tentar definir uma dependência de cache SQL para um banco de dados que não foi habilitado para notificação, um **DatabaseNotEnabledForNotificationException** exceção será lançada. Nesse caso, você pode tentar habilitar o banco de dados para notificações chamando o **SqlCacheDependencyAdmin.EnableNotifications** método e passando o nome do banco de dados.

Da mesma forma, se você tentar definir uma dependência de cache SQL para uma tabela que não foi habilitada para notificação, um **TableNotEnabledForNotificationException** será lançada. Em seguida, você pode chamar o **SqlCacheDependencyAdmin** método passando o nome do banco de dados e o nome da tabela.

O exemplo de código a seguir ilustra como configurar corretamente o tratamento de exceção ao configurar uma dependência de cache SQL.

[!code-csharp[Main](caching/samples/sample11.cs)]

Mais informações: [https://msdn.microsoft.com/library/t9x04ed2.aspx](https://msdn.microsoft.com/library/t9x04ed2.aspx)

## <a name="query-based-sql-cache-dependencies-sql-server-2005-only"></a>Dependências de Cache SQL com base em consulta (somente para SQL Server 2005)

Ao usar o SQL Server 2005 para dependência de cache SQL, o modelo de sondagem não é necessário. Quando usado com o SQL Server 2005, as dependências de cache do SQL se comunicam diretamente por meio de conexões do SQL para a instância do SQL Server (nenhuma configuração adicional é necessária) usando as notificações de consulta do SQL Server 2005.

É a maneira mais simples para habilitar a notificação de consulta fazer assim declarativamente definindo o **SqlCacheDependency** atributo do objeto de fonte de dados para **CommandNotification** e definindo o **EnableCaching** atributo **true**. Nenhum código usando esse método, é necessário. Se o resultado de um comando executado em relação aos dados as alterações da fonte, ele invalidará os dados do cache.

O exemplo a seguir configura um controle de fonte de dados para dependência de cache SQL:

[!code-aspx[Main](caching/samples/sample12.aspx)]

Nesse caso, se a consulta especificada no **SelectCommand** retorna um resultado diferente do que foi originalmente, os resultados são armazenados em cache são invalidados.

Você também pode especificar que todas as fontes de dados esteja habilitada para dependências de cache SQL, definindo o **SqlDependency** atributo do **@ OutputCache** diretiva **CommandNotification** . O exemplo a seguir ilustra isso.

[!code-aspx[Main](caching/samples/sample13.aspx)]

> [!NOTE]
> Para obter mais informações sobre notificações de consulta no SQL Server 2005, consulte os Manuais Online SQL Server.


Outro método de configuração de uma dependência de cache com base em consulta SQL é para fazer isso programaticamente usando a classe SqlCacheDependency. O exemplo de código a seguir ilustra como isso é feito.

[!code-csharp[Main](caching/samples/sample14.cs)]

Mais informações: [https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp](https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp)

## <a name="post-cache-substitution"></a>Substituição POST-Cache

Armazenar em cache uma página pode aumentar significativamente o desempenho de um aplicativo Web. No entanto, em alguns casos, você precisa mais da página a ser armazenado em cache e alguns fragmentos dentro da página para ser dinâmicos. Por exemplo, se você criar uma página de artigos de notícias que é totalmente estática para definir períodos de tempo, você pode definir a página inteira para ser armazenada em cache. Se você quiser incluir um cabeçalho de ad de rotação que foram alteradas em cada solicitação de página, a parte da página que contém o anúncio precisa ser dinâmico. Para permitir que uma página do cache, mas substituir algum conteúdo dinamicamente, você pode usar a substituição de POST-cache do ASP.NET. Com a substituição post-cache, a página inteira é armazenada em cache com partes específicas marcadas como isentas de cache de saída. No exemplo das faixas de anúncios, o controle AdRotator permite que você se beneficie da substituição post-cache para que anúncios criados dinamicamente para cada usuário e para cada atualização de página.

Há três maneiras de implementar a substituição post-cache:

- Declarativamente, usando o controle Substitution.
- Programaticamente, usando a API de controle de substituição.
- Implicitamente, usando o controle AdRotator.

### <a name="substitution-control"></a>Controle de substituição

O controle Substitution do ASP.NET especifica uma seção de uma página em cache que é criada dinamicamente em vez de ser armazenado em cache. Você pode colocar um controle Substitution no local na página onde você deseja que apareça o conteúdo dinâmico. Em tempo de execução, o controle Substitution chama um método que você especificar com a propriedade MethodName. O método deve retornar uma cadeia de caracteres, que substitui o conteúdo do controle de substituição. O método deve ser um método estático em contendo o controle Page ou UserControl. Usando o controle de substituição faz com que o armazenamento em cache do lado do cliente seja alterado para armazenamento em cache do servidor, para que a página não serão ser armazenadas em cache no cliente. Isso garante que as solicitações futuras para a página de chamar o método novamente para gerar conteúdo dinâmico.

### <a name="substitution-api"></a>Substituição de API

Para criar conteúdo dinâmico para uma página em cache programaticamente, você pode chamar o [WriteSubstitution](https://msdn.microsoft.com/library/system.web.httpresponse.writesubstitution.aspx) método no seu código de página, passando o nome de um método como um parâmetro. O método que trata a criação de conteúdo dinâmico usa um único [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) parâmetro e retorna uma cadeia de caracteres. A cadeia de caracteres de retorno é o conteúdo que será substituído no local especificado. Uma vantagem de chamar o método WriteSubstitution em vez de usar o controle Substitution declarativamente é que você pode chamar um método de qualquer objeto arbitrário em vez de chamar um método estático da página ou o objeto UserControl.

Chamar o método WriteSubstitution faz com que o armazenamento em cache do lado do cliente seja alterado para armazenamento em cache do servidor, para que a página não serão ser armazenadas em cache no cliente. Isso garante que as solicitações futuras para a página de chamar o método novamente para gerar conteúdo dinâmico.

### <a name="adrotator-control"></a>Controle AdRotator

O AdRotator controle de servidor implementa suporte para substituição post-cache internamente. Se você colocar um controle AdRotator em sua página, ele processará anúncios exclusivos em cada solicitação, independentemente se a página pai é armazenada em cache. Como resultado, uma página que inclui um controle AdRotator é somente armazenada em cache no lado do servidor.

## <a name="controlcachepolicy-class"></a>Classe ControlCachePolicy

A classe ControlCachePolicy permite o controle programático de fragmento de cache usando os controles de usuário. ASP.NET incorpora controles de usuário dentro de um [BasePartialCachingControl](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.aspx) instância. A classe BasePartialCachingControl representa um controle de usuário que tenha habilitado o cache de saída.

Quando você acessa o [BasePartialCachingControl.CachePolicy](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.cachepolicy.aspx) propriedade de um [PartialCachingControl](https://msdn.microsoft.com/library/system.web.ui.partialcachingcontrol.aspx) controle, você sempre recebe um objeto ControlCachePolicy válido. No entanto, se você acessar o [UserControl.CachePolicy](https://msdn.microsoft.com/library/system.web.ui.usercontrol.cachepolicy.aspx) propriedade de um [UserControl](https://msdn.microsoft.com/library/system.web.ui.usercontrol.aspx) controle, você recebe um objeto ControlCachePolicy válido somente se o controle de usuário já é encapsulado por um Controle de BasePartialCachingControl. Se não está quebrado, o objeto ControlCachePolicy retornado pela propriedade gerará exceções quando você tentar manipulá-lo porque ele não tem um BasePartialCachingControl associado. Para determinar se uma instância de UserControl dá suporte ao cache sem gerar exceções, inspecione o [SupportsCaching](https://msdn.microsoft.com/library/system.web.ui.controlcachepolicy.supportscaching.aspx) propriedade.

Usando a classe ControlCachePolicy é uma das várias maneiras que você pode habilitar o cache de saída. A lista a seguir descreve os métodos que você pode usar para habilitar o cache de saída:

- Use o [@ OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx) diretiva para habilitar o cache em cenários declarativos de saída.
- Use o [PartialCachingAttribute](https://msdn.microsoft.com/library/system.web.ui.partialcachingattribute.aspx) atributo para habilitar o cache para um controle de usuário em um arquivo code-behind.
- Use a classe ControlCachePolicy para especificar configurações de cache em cenários de programação na qual você está trabalhando com instâncias de BasePartialCachingControl que foram habilitados cache usando um dos métodos anteriores e carregados dinamicamente usando a [System.Web.UI.TemplateControl.LoadControl](https://msdn.microsoft.com/library/system.web.ui.templatecontrol.loadcontrol.aspx) método.

Uma instância de ControlCachePolicy pode ser manipulada com êxito apenas entre os estágios Init e PreRender do ciclo de vida de controle. Se você modificar um objeto ControlCachePolicy após a fase PreRender, o ASP.NET gera uma exceção porque todas as alterações feitas depois que o controle é processado, na verdade, não podem afetar as configurações de cache (um controle é armazenado em cache durante o estágio de processamento). Por fim, uma instância do controle de usuário (e, portanto, seu objeto ControlCachePolicy) só estará disponível para manipulação programática quando na verdade ele é renderizado.

## <a name="changes-to-caching-configuration---the-ltcachinggt-element"></a>Altera a configuração de cache - o &lt;cache&gt; elemento

Há várias alterações a configuração de cache no ASP.NET 2.0. O &lt;cache&gt; elemento é novo no ASP.NET 2.0 e permite que você faça alterações de configuração de cache no arquivo de configuração. Os atributos a seguir estão disponíveis.

| **Elemento** | **Descrição** |
| --- | --- |
| **cache** | Elemento opcional. Define as configurações de cache de aplicativo global. |
| **outputCache** | Elemento opcional. Especifica as configurações de cache de saída de todo o aplicativo. |
| **outputCacheSettings** | Elemento opcional. Especifica as configurações de cache de saída podem ser aplicadas às páginas do aplicativo. |
| **sqlCacheDependency** | Elemento opcional. Configura as dependências de cache do SQL para um aplicativo ASP.NET. |

### <a name="the-ltcachegt-element"></a>O &lt;cache&gt; elemento

Os atributos a seguir estão disponíveis no &lt;cache&gt; elemento:

| **Atributo** | **Descrição** |
| --- | --- |
| **disableMemoryCollection** | Opcional **booliano** atributo. Obtém ou define um valor que indica se a coleção de memória de cache que ocorre quando o computador estiver sob pressão de memória está desabilitada. |
| **disableExpiration** | Opcional **booliano** atributo. Obtém ou define um valor que indica se a expiração de cache está desabilitada. Quando desabilitado, os itens armazenados em cache não expirar e eliminação de plano de fundo dos itens expirados do cache não ocorrerá. |
| **privateBytesLimit** | Opcional **Int64** atributo. Obtém ou define um valor que indica o tamanho máximo de bytes particulares de um aplicativo antes do cache começa a liberar expirado itens e tentar recuperar memória. Esse limite inclui memória usada pelo cache, bem como memória normal sobrecarga de aplicativo em execução. Uma configuração de zero indica que o ASP.NET usará sua própria heurística para determinar quando iniciar a recuperação de memória. |
| **percentagePhysicalMemoryUsedLimit** | Opcional **Int32** atributo. Obtém ou define um valor que indica a porcentagem máxima de memória física de uma máquina que pode ser consumida por um aplicativo antes do cache começa a liberar expirado itens e tentar recuperar o uso de memória inclui a memória usada pelo cache também de memória como o uso de memória normal do aplicativo em execução. Uma configuração de zero indica que o ASP.NET usará sua própria heurística para determinar quando iniciar a recuperação de memória. |
| **privateBytesPollTime** | Opcional **TimeSpan** atributo. Obtém ou define um valor que indica o intervalo de tempo entre as sondagens para uso de memória de bytes particulares do aplicativo. |

### <a name="the-ltoutputcachegt-element"></a>O &lt;outputCache&gt; elemento

Os seguintes atributos estão disponíveis para o &lt;outputCache&gt; elemento.

| **Atributo** | **Descrição** |
| --- | --- |
| **enableOutputCache** | Opcional **booliano** atributo. Habilita/desabilita o cache de saída de página. Se desabilitada, nenhuma página é armazenados em cache independentemente das configurações de declarativa ou por programação. Valor padrão é **true**. |
| **enableFragmentCache** | Opcional **booliano** atributo. Habilita/desabilita o cache de fragmento do aplicativo. Se desabilitada, nenhuma página é armazenados em cache independentemente do [@ OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx) diretiva ou cache perfil usado. Inclui um cabeçalho cache-control indicando que os servidores proxy upstream, bem como os clientes de navegador não devem tentar saída de página do cache. Valor padrão é **false**. |
| **sendCacheControlHeader** | Opcional **booliano** atributo. Obtém ou define um valor que indica se o **cache-controle: privada** cabeçalho é enviado pelo módulo de cache de saída por padrão. Valor padrão é **false**. |
| **omitVaryStar** | Opcional **booliano** atributo. Habilita/desabilita o envio de um Http "**Vary: \*** " cabeçalho na resposta. Com a configuração padrão de false, um "**Vary: \*** " cabeçalho é enviado para páginas de saída em cache. Quando o cabeçalho Vary é enviado, ele permite para diferente versões sejam armazenados em cache com base no que é especificado no cabeçalho Vary. Por exemplo, *Vary: usuário-agentes* irá armazenar versões diferentes de uma página com base no agente do usuário que emite a solicitação. Valor padrão é **false**. |

### <a name="the-ltoutputcachesettingsgt-element"></a>O &lt;outputCacheSettings&gt; elemento

O &lt;outputCacheSettings&gt; elemento permite a criação de perfis de cache conforme descrito anteriormente. O elemento filho único para o &lt;outputCacheSettings&gt; elemento é o &lt;outputCacheProfiles&gt; elemento para configurar perfis de cache.

### <a name="the-ltsqlcachedependencygt-element"></a>O &lt;sqlCacheDependency&gt; elemento

Os seguintes atributos estão disponíveis para o &lt;sqlCacheDependency&gt; elemento.

| **Atributo** | **Descrição** |
| --- | --- |
| **enabled** | Necessário **booliano** atributo. Indica se as alterações estão sendo pesquisadas para. |
| **pollTime** | Opcional **Int32** atributo. Define a frequência com que o SqlCacheDependency pesquisa a tabela de banco de dados de alterações. Esse valor corresponde ao número de milissegundos entre pollings sucessivas. Ele não pode ser definido para menos de 500 milissegundos. Valor padrão é 1 minuto. |

### <a name="more-information"></a>Mais informações

Há algumas informações adicionais que você deve conhecer sobre a configuração de cache.

- Se o limite de bytes particulares do processo de trabalho não for definido, o cache usará um dos seguintes limites: 

    - x86 2 GB: 800 MB ou 60% da RAM física, o que for menor
    - x86 3 GB: 1800 MB ou 60% da RAM física, o que for menor
    - x 64: 1 terabyte ou 60% da RAM física, o que for menor
- Se ambos o processo de trabalho particular limitam de bytes e &lt;privateBytesLimit de cache /&gt; estão configurados, o cache será usado o mínimo dos dois.
- Assim como 1. x, vamos remover as entradas de cache e chamar GC. Colete por dois motivos: 

    - Estamos muito perto do limite de bytes particulares
    - A memória disponível é próxima a ou menor que 10%
- Efetivamente, você pode desabilitar corte e armazenar em cache para condições de memória insuficiente disponível definindo &lt;percentagePhysicalMemoryUseLimit de cache /&gt; a 100.
- Ao contrário de 1. x, 2.0 suspenderá as corte e coletar chamadas se o último GC. Não reduzir a coletar bytes particulares ou o tamanho de pilhas gerenciadas por mais de 1% do limite de memória (cache).

## <a name="lab1-custom-cache-dependencies"></a>Lab1: Dependências de Cache personalizada

1. Crie um novo site.
2. Adicione um novo arquivo XML chamado cache.xml e salvá-lo para a raiz do aplicativo Web.
3. Adicione o seguinte código para a página\_método no code-behind de default.aspx de carga: 

    [!code-csharp[Main](caching/samples/sample15.cs)]
4. Adicione o seguinte à parte superior do default.aspx no modo de exibição de fonte: 

    [!code-aspx[Main](caching/samples/sample16.aspx)]
5. Procure Default.aspx. O que o tempo de dizer?
6. Atualize o navegador. O que o tempo de dizer?
7. Abra cache.xml e adicione o seguinte código: 

    [!code-xml[Main](caching/samples/sample17.xml)]
8. Salve cache.xml.
9. Atualize o navegador. O que o tempo de dizer?
10. Explica por que o tempo é atualizado em vez de exibir os valores armazenados em cache anteriormente:

## <a name="lab-2-using-polling-based-cache-dependencies"></a>Laboratório 2: Dependências de Cache com base na pesquisa usando

Este laboratório usa o projeto que você criou no módulo anterior que permite a edição de dados no banco de dados Northwind por meio de um controle GridView e DetailsView.

1. Abra o projeto no Visual Studio 2005.
2. Execute o aspnet\_utilitário regsql no banco de dados Northwind para habilitar o banco de dados e a tabela produtos. Use o comando a seguir de um Prompt de comando Visual Studio: 

    [!code-console[Main](caching/samples/sample18.cmd)]
3. Adicione o seguinte ao arquivo Web. config: 

    [!code-xml[Main](caching/samples/sample19.xml)]
4. Adicione um novo formulário da Web chamado showdata.aspx.
5. Adicione o seguinte @ diretiva outputcache à página showdata.aspx: 

    [!code-aspx[Main](caching/samples/sample20.aspx)]
6. Adicione o seguinte código para a página\_carga de showdata.aspx: 

    [!code-html[Main](caching/samples/sample21.html)]
7. Adicione um novo controle SqlDataSource para showdata.aspx e configurá-lo para usar a conexão de banco de dados Northwind. Clique em Avançar.
8. Selecione as caixas de seleção ProductName e ProductID e clique em Avançar.
9. Clique em Concluir.
10. Adicione um novo GridView à página showdata.aspx.
11. Escolha SqlDataSource1 na lista suspensa.
12. Salve e procurar showdata.aspx. Anote o tempo exibido.
