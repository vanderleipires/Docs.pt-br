---
uid: web-forms/overview/moving-to-aspnet-20/caching
title: Cache | Microsoft Docs
author: microsoft
description: Uma compreensão de armazenamento em cache é importante para um aplicativo ASP.NET de bom desempenho. ASP.NET 1.x oferecidos três opções diferentes para armazenamento em cache. o cache de saída,...
ms.author: aspnetcontent
ms.date: 02/20/2005
ms.assetid: 2bb109d2-e299-46ea-9054-fa0263b59165
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/caching
msc.type: authoredcontent
ms.openlocfilehash: 0c38092c47060e6d02791f9672df6703852f4b5a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829480"
---
<a name="caching"></a>Cache
====================
por [Microsoft](https://github.com/microsoft)

> Uma compreensão de armazenamento em cache é importante para um aplicativo ASP.NET de bom desempenho. ASP.NET 1.x oferecidos três opções diferentes para armazenamento em cache. o cache de saída, cache de fragmento e a API de cache.


Uma compreensão de armazenamento em cache é importante para um aplicativo ASP.NET de bom desempenho. ASP.NET 1.x oferecidos três opções diferentes para armazenamento em cache. o cache de saída, cache de fragmento e a API de cache. O ASP.NET 2.0 oferece todos os três métodos, mas adiciona alguns recursos adicionais significativos. Há várias novas dependências de cache e os desenvolvedores agora têm a opção de criar dependências de cache personalizadas também. A configuração de armazenamento em cache também foi aprimorada significativamente no ASP.NET 2.0.

## <a name="new-features"></a>Novos recursos

## <a name="cache-profiles"></a>Perfis de cache

Perfis de cache permitem que os desenvolvedores definam as configurações de cache específica, em seguida, podem ser aplicadas a páginas individuais. Por exemplo, se você tiver algumas páginas que devem ser expiradas do cache depois de 12 horas, você pode facilmente criar um perfil de cache que pode ser aplicado a essas páginas. Para adicionar um novo perfil de cache, use o &lt;outputCacheSettings&gt; seção no arquivo de configuração. Por exemplo, abaixo está a configuração de um perfil de cache chamado *twoday* que configura uma duração de cache de 12 horas.

[!code-xml[Main](caching/samples/sample1.xml)]

Para aplicar a este perfil de cache para uma página específica, use o atributo CacheProfile da diretiva @ OutputCache conforme mostrado abaixo:

[!code-aspx[Main](caching/samples/sample2.aspx)]

## <a name="custom-cache-dependencies"></a>Dependências de Cache personalizada

Os desenvolvedores do ASP.NET 1.x chorei-out para dependências de cache personalizado. No ASP.NET 1. x, a classe CacheDependency foi lacrado que os desenvolvedores foi impedidos de derivar suas próprias classes dele. No ASP.NET 2.0, essa limitação foi removida e os desenvolvedores são livres para desenvolver suas próprias dependências de cache personalizado. A classe CacheDependency permite a criação de uma dependência de cache personalizado com base em arquivos, diretórios ou chaves de cache.

Por exemplo, o código a seguir cria uma nova dependência de cache personalizado com base em um arquivo chamado stuff.xml localizado na raiz do aplicativo Web:

[!code-csharp[Main](caching/samples/sample3.cs)]

Nesse cenário, quando o arquivo stuff.xml for alterado, o item em cache é invalidado.

Também é possível criar uma dependência de cache personalizado usando as chaves de cache. Usando esse método, a remoção da chave de cache invalidará os dados em cache. O exemplo a seguir ilustra isto:

[!code-csharp[Main](caching/samples/sample4.cs)]

Para invalidar o item que foi inserido acima, basta remova o item que foi inserido no cache para atuar como a chave de cache.

[!code-csharp[Main](caching/samples/sample5.cs)]

Observe que a chave do item que atua como a chave de cache deve ser o mesmo que o valor adicionado à matriz de chaves de cache.

## <a name="polling-based-sql-cache-dependenciesemalso-called-table-based-dependenciesem"></a>Dependências de Cache de SQL baseadas em sondagem<em>(também chamado de dependências baseadas em tabela)</em>

SQL Server 7 e 2000 usam o modelo baseado em sondagem para dependências de cache SQL. O modelo baseado em sondagem usa um gatilho em uma tabela de banco de dados que é disparada quando a alteração de dados na tabela. Que disparar atualizações de um **changeId** campo na tabela de notificação que o ASP.NET verifica periodicamente. Se o **changeId** campo foi atualizado, o ASP.NET sabe que os dados foram alterados e invalida os dados em cache.

> [!NOTE]
> SQL Server 2005 também pode usar o modelo baseado em sondagem, mas como o modelo de sondagem não é o modelo mais eficiente, é aconselhável usar um modelo baseado em consulta (discutido posteriormente) com o SQL Server 2005.


Em ordem de uma dependência de cache SQL usando o modelo de sondagem para funcionar corretamente, as tabelas devem ter as notificações habilitadas. Isso pode ser feito por meio de programação usando a classe SqlCacheDependencyAdmin ou usando o aspnet\_regsql.exe utilitário.

A linha de comando a seguir registra a tabela Produtos no banco de dados Northwind, localizado em uma instância do SQL Server denominada *dbase* para SQL dependência de cache.

[!code-console[Main](caching/samples/sample6.cmd)]

Veja a seguir uma explicação sobre as opções de linha de comando usada no comando acima:

| **Opção de linha de comando** | **Finalidade** |
| --- | --- |
| -S *server* | Especifica o nome do servidor. |
| -ed | Especifica que o banco de dados deve estar habilitado para a dependência de cache SQL. |
| -d *banco de dados\_nome* | Especifica o nome do banco de dados que deve ser habilitado para a dependência de cache SQL. |
| -E | Especifica que aspnet\_regsql deve usar a autenticação do Windows ao conectar-se ao banco de dados. |
| -et | Especifica que nós estamos habilitando uma tabela de banco de dados para a dependência de cache SQL. |
| -t *tabela\_nome* | Especifica o nome da tabela de banco de dados para habilitar a dependência de cache SQL. |

> [!NOTE]
> Há outras opções disponíveis para aspnet\_regsql.exe. Para obter uma lista completa, execute aspnet\_regsql.exe-? em uma linha de comando.


Quando esse comando executa as seguintes alterações são feitas no banco de dados do SQL Server:

- Uma **AspNet\_SqlCacheTablesForChangeNotification** tabela é adicionada. Esta tabela contém uma linha para cada tabela no banco de dados para o qual uma dependência de cache SQL foi habilitada.
- Os seguintes procedimentos armazenados são criados dentro do banco de dados:


| AspNet\_SqlCachePollingStoredProcedure | Consulta o AspNet\_SqlCacheTablesForChangeNotification tabela e retorna todas as tabelas habilitadas para a dependência de cache SQL e o valor de changeId para cada tabela. Esse procedimento armazenado é usado para sondar para determinar se os dados foram alterados. |
| --- | --- |
| AspNet\_SqlCacheQueryRegisteredTablesStoredProcedure | Retorna todas as tabelas habilitadas para a dependência de cache SQL, consultando o AspNet\_SqlCacheTablesForChangeNotification tabela e retorna todas as tabelas habilitadas para o SQL a dependência de cache. |
| AspNet\_SqlCacheRegisterTableStoredProcedure | Registra uma tabela para a dependência de cache SQL, adicionando a entrada necessária na tabela de notificação e adiciona o gatilho. |
| AspNet\_SqlCacheUnRegisterTableStoredProcedure | Cancela o registro de uma tabela para a dependência de cache SQL, removendo a entrada na tabela de notificação e remove o gatilho. |
| AspNet\_SqlCacheUpdateChangeIdStoredProcedure | Atualiza a tabela de notificação ao incrementar o changeId para a tabela alterada. O ASP.NET usa esse valor para determinar se os dados foram alterados. Conforme indicado a seguir, esse procedimento armazenado é executado pelo gatilho criado quando a tabela está habilitada. |


- Um gatilho do SQL Server chamado ***tabela\_nome *\_AspNet\_SqlCacheNotification\_gatilho** é criado para a tabela. Esse gatilho executa o AspNet\_SqlCacheUpdateChangeIdStoredProcedure quando um INSERT, UPDATE ou DELETE é executada na tabela.
- Uma função de servidor SQL chamado **aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess** é adicionado ao banco de dados.

O **aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess** função do SQL Server tem permissões de EXEC para o AspNet\_SqlCachePollingStoredProcedure. Para o modelo de sondagem funcionar corretamente, você deve adicionar sua conta de processo para o aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess função. O aspnet\_regsql.exe ferramenta não fará isso para você.

### <a name="configuring-polling-based-sql-cache-dependencies"></a>Configurar dependências de Cache baseadas em sondagem SQL

Há várias etapas que são necessárias para configurar dependências de cache SQL baseadas em sondagem. A primeira etapa é habilitar o banco de dados e a tabela como discutido acima. Quando essa etapa for concluída, o restante da configuração é o seguinte:

- Configurando o arquivo de configuração do ASP.NET.
- Configurando o SqlCacheDependency

### <a name="configuring-the-aspnet-configuration-file"></a>Configurando o arquivo de configuração do ASP.NET

Além de adicionar uma cadeia de caracteres de conexão, conforme discutido em um módulo anterior, você também deve configurar uma &lt;cache&gt; elemento com um &lt;sqlCacheDependency&gt; elemento conforme mostrado abaixo:

[!code-xml[Main](caching/samples/sample7.xml)]

Essa configuração permite que uma dependência de cache SQL no *pubs* banco de dados. Observe que o pollTime atributo na &lt;sqlCacheDependency&gt; elemento padrão é 60000 milissegundos ou 1 minuto. (Esse valor não pode ser menor que 500 milissegundos). Neste exemplo, o &lt;adicionar&gt; elemento adiciona um novo banco de dados e substitui o pollTime, configurando-a como 9000000 milissegundos.

#### <a name="configuring-the-sqlcachedependency"></a>Configurando o SqlCacheDependency

A próxima etapa é configurar o SqlCacheDependency. A maneira mais fácil de fazer isso é especificar o valor do atributo SqlDependency na diretiva @ Outcache da seguinte maneira:

[!code-aspx[Main](caching/samples/sample8.aspx)]

A diretiva @ OutputCache acima, uma dependência de cache SQL é configurada para o *autores* na tabela a *pubs* banco de dados. Várias dependências podem ser configuradas, separando-os com ponto e vírgula da seguinte forma:

[!code-aspx[Main](caching/samples/sample9.aspx)]

Outro método de configuração de SqlCacheDependency é para fazer isso programaticamente. O código a seguir cria uma nova dependência de cache SQL no *autores* na tabela a *pubs* banco de dados.

[!code-csharp[Main](caching/samples/sample10.cs)]

Um dos benefícios de definir programaticamente a dependência de cache SQL é que você pode manipular todas as exceções que podem ocorrer. Por exemplo, se você tentar definir uma dependência de cache SQL para um banco de dados que não foi habilitada para a notificação, uma **DatabaseNotEnabledForNotificationException** exceção será lançada. Nesse caso, você pode tentar habilitar o banco de dados para as notificações ao chamar o **SqlCacheDependencyAdmin.EnableNotifications** método e passando o nome do banco de dados.

Da mesma forma, se você tentar definir uma dependência de cache SQL para uma tabela que não foi habilitada para notificação, uma **TableNotEnabledForNotificationException** será lançada. Em seguida, você pode chamar o **SqlCacheDependencyAdmin** método passando-o nome de banco de dados e o nome da tabela.

O exemplo de código a seguir ilustra como configurar corretamente o tratamento de exceção ao configurar uma dependência de cache SQL.

[!code-csharp[Main](caching/samples/sample11.cs)]

Obter mais informações: [https://msdn.microsoft.com/library/t9x04ed2.aspx](https://msdn.microsoft.com/library/t9x04ed2.aspx)

## <a name="query-based-sql-cache-dependencies-sql-server-2005-only"></a>Dependências de Cache SQL com base em consulta (somente SQL Server 2005)

Ao usar o SQL Server 2005 para a dependência de cache SQL, o modelo de sondagem não é necessário. Quando usado com o SQL Server 2005, dependências de cache SQL se comunicam diretamente por meio de conexões do SQL para a instância do SQL Server (nenhuma configuração adicional é necessária) usando as notificações de consulta do SQL Server 2005.

A maneira mais simples de habilitar a notificação de consulta é fazer então declarativamente, definindo o **SqlCacheDependency** atributo do objeto de fonte de dados para **CommandNotification** e definindo o **EnableCaching** atributo **verdadeira**. Usando esse método, nenhum código é necessário. Se o resultado de um comando executado contra os dados de alterações de código-fonte, ele invalidará os dados do cache.

O exemplo a seguir configura um controle de fonte de dados para a dependência de cache SQL:

[!code-aspx[Main](caching/samples/sample12.aspx)]

Nesse caso, se a consulta especificada na **SelectCommand** retorna um resultado diferente do que foi originalmente, os resultados são armazenados em cache são invalidados.

Você também pode especificar que todas as suas fontes de dados ser habilitada para dependências de cache SQL, definindo o **SqlDependency** atributo da **@ OutputCache** diretiva **CommandNotification** . O exemplo a seguir ilustra isso.

[!code-aspx[Main](caching/samples/sample13.aspx)]

> [!NOTE]
> Para obter mais informações sobre notificações de consulta no SQL Server 2005, consulte o SQL Server Books Online.


Outro método de configuração de uma dependência de cache baseada em consulta SQL é para fazer isso programaticamente usando a classe de SqlCacheDependency. O exemplo de código a seguir ilustra como isso é feito.

[!code-csharp[Main](caching/samples/sample14.cs)]

Obter mais informações: [https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp](https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp)

## <a name="post-cache-substitution"></a>Substituição do Cache de postagem

Uma página de cache pode aumentar drasticamente o desempenho de um aplicativo Web. No entanto, em alguns casos, você precisa mais da página a ser armazenado em cache e alguns fragmentos dentro da página ser dinâmica. Por exemplo, se você criar uma página de artigos de notícias que é totalmente estática para definir períodos de tempo, você pode definir a página inteira seja armazenada em cache. Se você quisesse incluir uma faixa de ad girar alterado em cada solicitação de página, a parte da página que contém o anúncio precisa ser dinâmica. Para permitir que você armazena em cache uma página, mas substituir algum conteúdo dinamicamente, você pode usar a substituição de POST-cache do ASP.NET. Com a substituição do cache de postagem, a página inteira é armazenada em cache com partes específicas, marcadas como isenta de cache de saída. No exemplo das faixas de anúncios, o controle AdRotator permite que você se beneficie de substituição do cache de postagem, para que os anúncios criados dinamicamente para cada usuário em cada atualização de página.

Há três maneiras de implementar a substituição do cache de postagem:

- Declarativamente, usando o controle de substituição.
- Programaticamente, usando a API de controle de substituição.
- Implicitamente, usando o controle AdRotator.

### <a name="substitution-control"></a>Controle de substituição

O controle Substitution do ASP.NET especifica uma seção de uma página em cache que é criada dinamicamente em vez de ser armazenado em cache. Você pode colocar um controle Substitution no local na página onde você deseja que o conteúdo dinâmico seja exibido. Em tempo de execução, o controle Substitution chama um método que você especificar com a propriedade MethodName. O método deve retornar uma cadeia de caracteres, que, em seguida, substitui o conteúdo do controle de substituição. O método deve ser um método estático em contendo o controle página ou UserControl. Usar o controle substitution faz com que o armazenamento em cache do lado do cliente seja alterado por armazenamento de servidor, para que a página não será armazenada no cliente. Isso garante que as solicitações futuras para a página de chamar o método novamente para gerar conteúdo dinâmico.

### <a name="substitution-api"></a>Substituição de API

Para criar conteúdo dinâmico para uma página em cache por meio de programação, você pode chamar o [WriteSubstitution](https://msdn.microsoft.com/library/system.web.httpresponse.writesubstitution.aspx) método em seu código de página, passando o nome de um método como um parâmetro. O método que manipula a criação de conteúdo dinâmico usa um único [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) parâmetro e retorna uma cadeia de caracteres. A cadeia de caracteres de retornada é o conteúdo que será substituído no local especificado. Uma vantagem de chamar o método WriteSubstitution em vez de usar o controle Substitution declarativamente é que você pode chamar um método de qualquer objeto arbitrário em vez de chamar um método estático da página ou o objeto de UserControl.

Chamar o método WriteSubstitution faz com que o armazenamento em cache do lado do cliente seja alterado por armazenamento de servidor, para que a página não será armazenada no cliente. Isso garante que as solicitações futuras para a página de chamar o método novamente para gerar conteúdo dinâmico.

### <a name="adrotator-control"></a>Controle AdRotator

O AdRotator controle de servidor implementa suporte para substituição do cache de postagem internamente. Se você colocar um controle AdRotator em sua página, ele processará anúncios exclusivos em cada solicitação, independentemente da página pai é armazenada em cache. Como resultado, uma página que inclui um controle AdRotator é apenas em cache no lado do servidor.

## <a name="controlcachepolicy-class"></a>Classe ControlCachePolicy

A classe ControlCachePolicy permite o controle programático de fragmento de cache usando os controles de usuário. ASP.NET incorpora controles de usuário dentro de um [BasePartialCachingControl](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.aspx) instância. A classe BasePartialCachingControl representa um controle de usuário que tem cache de saída habilitado.

Quando você acessa o [BasePartialCachingControl.CachePolicy](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.cachepolicy.aspx) propriedade de uma [PartialCachingControl](https://msdn.microsoft.com/library/system.web.ui.partialcachingcontrol.aspx) controle, você sempre receberá um objeto ControlCachePolicy válido. No entanto, se você acessar o [UserControl.CachePolicy](https://msdn.microsoft.com/library/system.web.ui.usercontrol.cachepolicy.aspx) propriedade de uma [UserControl](https://msdn.microsoft.com/library/system.web.ui.usercontrol.aspx) controle, você recebe um objeto ControlCachePolicy válido somente se o controle de usuário já é encapsulado por um Controle BasePartialCachingControl. Se não estão envolvido, o objeto ControlCachePolicy retornado pela propriedade gerará exceções quando você tenta manipulá-lo porque ele não tem um BasePartialCachingControl associado. Para determinar se uma instância de UserControl dá suporte ao cache sem gerar exceções, inspecione a [SupportsCaching](https://msdn.microsoft.com/library/system.web.ui.controlcachepolicy.supportscaching.aspx) propriedade.

Usando a classe ControlCachePolicy é uma das várias maneiras que você pode habilitar o cache de saída. A lista a seguir descreve os métodos que você pode usar para habilitar o cache de saída:

- Use o [@ OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx) diretiva para permitir que o cache em cenários declarativos de saída.
- Use o [PartialCachingAttribute](https://msdn.microsoft.com/library/system.web.ui.partialcachingattribute.aspx) atributo para habilitar o cache para um controle de usuário em um arquivo code-behind.
- Use a classe ControlCachePolicy para especificar as configurações de cache em cenários de programação no qual você está trabalhando com instâncias de BasePartialCachingControl que foram habilitados de cache usando um dos métodos anteriores e carregado dinamicamente usando o [System.Web.UI.TemplateControl.LoadControl](https://msdn.microsoft.com/library/system.web.ui.templatecontrol.loadcontrol.aspx) método.

Uma instância de ControlCachePolicy pode ser manipulada com êxito apenas entre os estágios de Init e PreRender do ciclo de vida do controle. Se você modificar um objeto ControlCachePolicy após a fase PreRender, o ASP.NET gera uma exceção porque as alterações feitas após o controle é processado, na verdade, não podem afetar as configurações de cache (um controle em cache durante o estágio de renderização). Por fim, uma instância de controle de usuário (e, portanto, seu objeto ControlCachePolicy) está apenas disponíveis para manipulação programática quando, na verdade, ele é renderizado.

## <a name="changes-to-caching-configuration---the-ltcachinggt-element"></a>Altera a configuração de cache - a &lt;caching&gt; elemento

Há várias alterações a configuração de cache no ASP.NET 2.0. O &lt;caching&gt; elemento é novo no ASP.NET 2.0 e permite que você faça alterações de configuração de cache no arquivo de configuração. Os seguintes atributos estão disponíveis.

| **Elemento** | **Descrição** |
| --- | --- |
| **cache** | Elemento opcional. Define as configurações de cache de aplicativo global. |
| **outputCache** | Elemento opcional. Especifica as configurações de cache de saída de todo o aplicativo. |
| **outputCacheSettings** | Elemento opcional. Especifica as configurações de cache de saída que podem ser aplicadas a páginas do aplicativo. |
| **sqlCacheDependency** | Elemento opcional. Configura as dependências de cache do SQL para um aplicativo ASP.NET. |

### <a name="the-ltcachegt-element"></a>O &lt;cache&gt; elemento

Os seguintes atributos estão disponíveis na &lt;cache&gt; elemento:

| **Atributo** | **Descrição** |
| --- | --- |
| **disableMemoryCollection** | Opcional **Boolean** atributo. Obtém ou define um valor que indica se a coleta de memória de cache que ocorre quando a máquina estiver sob pressão de memória está desabilitada. |
| **disableExpiration** | Opcional **Boolean** atributo. Obtém ou define um valor que indica se a expiração do cache está desabilitada. Quando desabilitado, os itens armazenados em cache não expiram e eliminação de plano de fundo dos itens de cache expirados não ocorrerá. |
| **privateBytesLimit** | Opcional **Int64** atributo. Obtém ou define um valor que indica o tamanho máximo de bytes particulares de um aplicativo antes do início de cache de liberação expirado itens e tentar recuperar memória. Esse limite inclui a memória usada pelo cache bem como memória normal sobrecarga de aplicativo em execução. Uma configuração de zero indica que o ASP.NET usará sua própria heurística para determinar quando iniciar a recuperação de memória. |
| **percentagePhysicalMemoryUsedLimit** | Opcional **Int32** atributo. Obtém ou define um valor que indica a porcentagem máxima de memória física de uma máquina que pode ser consumida por um aplicativo antes do início de cache de liberação expirado itens e tentar recuperar a memória que esse uso de memória inclui a ambas as memória usada pelo cache também como o uso de memória normal do aplicativo em execução. Uma configuração de zero indica que o ASP.NET usará sua própria heurística para determinar quando iniciar a recuperação de memória. |
| **privateBytesPollTime** | Opcional **TimeSpan** atributo. Obtém ou define um valor que indica o intervalo de tempo entre as sondagens para uso de memória de bytes particulares do aplicativo. |

### <a name="the-ltoutputcachegt-element"></a>O &lt;outputCache&gt; elemento

Os seguintes atributos estão disponíveis para o &lt;outputCache&gt; elemento.


|       <strong>Atributo</strong>        |                                                                                                                                                                                                                                                       <strong>Descrição</strong>                                                                                                                                                                                                                                                       |
|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   <strong>enableOutputCache</strong>    |                                                                                                                                                          Opcional <strong>Boolean</strong> atributo. Habilita/desabilita o cache de saída de página. Se desabilitada, nenhuma página é armazenados em cache independentemente das configurações de declarativa ou por programação. Valor padrão é <strong>verdadeira</strong>.                                                                                                                                                           |
|  <strong>enableFragmentCache</strong>   |                                                Opcional <strong>Boolean</strong> atributo. Habilita/desabilita o cache de fragmento do aplicativo. Se desabilitada, nenhuma página é armazenados em cache independentemente do [@ OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx) diretiva ou o perfil usado de caching. Inclui um cabeçalho cache-control indicando que os servidores proxy upstream, bem como os clientes de navegador não devem tentar saída de página de cache. Valor padrão é <strong>falsos</strong>.                                                 |
| <strong>sendCacheControlHeader</strong> |                                                                                                                                                      Opcional <strong>Boolean</strong> atributo. Obtém ou define um valor que indica se o <strong>cache-controle: private</strong> cabeçalho é enviado pelo módulo de cache de saída por padrão. Valor padrão é <strong>falsos</strong>.                                                                                                                                                      |
|      <strong>omitVaryStar</strong>      | Opcional <strong>Boolean</strong> atributo. Habilita/desabilita o envio de um Http "<strong>Vary: \</ strong ><em>" cabeçalho na resposta. Com a configuração padrão de false, um "</em>* Vary: \* <strong>" cabeçalho é enviado para as páginas de saída armazenadas em cache. Quando o cabeçalho Vary é enviado, ele permite que diferentes versões sejam armazenados em cache com base no que é especificado no cabeçalho Vary. Por exemplo, <em>Vary: usuário-agentes</em> armazenará versões diferentes de uma página com base no agente do usuário emitir a solicitação. Valor padrão é * * false</strong>. |

### <a name="the-ltoutputcachesettingsgt-element"></a>O &lt;outputCacheSettings&gt; elemento

O &lt;outputCacheSettings&gt; elemento permite a criação de perfis de cache conforme descrito anteriormente. O elemento filho único para o &lt;outputCacheSettings&gt; elemento é o &lt;outputCacheProfiles&gt; elemento para a configuração de perfis de cache.

### <a name="the-ltsqlcachedependencygt-element"></a>O &lt;sqlCacheDependency&gt; elemento

Os seguintes atributos estão disponíveis para o &lt;sqlCacheDependency&gt; elemento.

| **Atributo** | **Descrição** |
| --- | --- |
| **habilitado** | Exigido **Boolean** atributo. Indica se as alterações estão sendo pesquisadas para. |
| **pollTime** | Opcional **Int32** atributo. Define a frequência com a qual o SqlCacheDependency sonda a tabela de banco de dados para que as alterações. Esse valor corresponde ao número de milissegundos entre pollings sucessivas. Ele não pode ser definido como menor que 500 milissegundos. Valor padrão é 1 minuto. |

### <a name="more-information"></a>Mais informações

Há algumas informações adicionais que você deve estar atento em relação à configuração de cache.

- Se o limite de bytes particulares do processo de trabalho não for definido, o cache usará um dos limites a seguir: 

    - x86 GB 2: 800 MB ou 60% da RAM física, o que for menor
    - x86 GB: 1800 3 MB ou 60% da RAM física, o que for menor
    - x 64: 1 terabyte ou 60% da RAM física, o que for menor
- Se os dois o processo de trabalho privada limite de bytes e &lt;privateBytesLimit do cache /&gt; forem definidos, o cache usará o mínimo dos dois.
- Assim como no 1.x, podemos remover entradas de cache e chamar GC. Colete por dois motivos: 

    - Estamos muito próximo ao limite de bytes particulares
    - A memória disponível é próxima a ou menor que 10%
- Efetivamente, você pode desabilitar corte e armazenar em cache para condições de pouca memória disponível, definindo &lt;percentagePhysicalMemoryUseLimit do cache /&gt; a 100.
- Ao contrário do 1.x, 2.0 suspenderá o corte e coletar chamadas se o último GC. Não reduzir a coletar bytes particulares ou o tamanho dos heaps gerenciados por mais de 1% do limite de memória (cache).

## <a name="lab1-custom-cache-dependencies"></a>Lab1: Dependências de Cache personalizado

1. Crie um novo site.
2. Adicione um novo arquivo XML chamado cache.xml e salve-o para a raiz do aplicativo Web.
3. Adicione o seguinte código para a página\_método no code-behind de Default. aspx de carga: 

    [!code-csharp[Main](caching/samples/sample15.cs)]
4. Adicione o seguinte à parte superior de Default. aspx no modo de exibição de fonte: 

    [!code-aspx[Main](caching/samples/sample16.aspx)]
5. Procure default. aspx. O que o tempo de dizer?
6. Atualize o navegador. O que o tempo de dizer?
7. Abra cache.xml e adicione o seguinte código: 

    [!code-xml[Main](caching/samples/sample17.xml)]
8. Salve cache.xml.
9. Atualize seu navegador. O que o tempo de dizer?
10. Explique por que a hora é atualizado em vez de exibir os valores armazenados em cache anteriormente:

## <a name="lab-2-using-polling-based-cache-dependencies"></a>Laboratório 2: Usando dependências de Cache baseadas em sondagem

Este laboratório usa o projeto criado no módulo anterior que permite a edição de dados no banco de dados Northwind, por meio de um controle GridView e DetailsView.

1. Abra o projeto no Visual Studio 2005.
2. Execute o aspnet\_regsql utilitário contra o banco de dados Northwind para habilitar o banco de dados e a tabela produtos. Use o seguinte comando em um Prompt de comando Visual Studio: 

    [!code-console[Main](caching/samples/sample18.cmd)]
3. Adicione o seguinte ao seu arquivo Web. config: 

    [!code-xml[Main](caching/samples/sample19.xml)]
4. Adicione um novo formulário da Web chamado showdata.aspx.
5. Adicione o seguinte @ diretiva outputcache à página showdata.aspx: 

    [!code-aspx[Main](caching/samples/sample20.aspx)]
6. Adicione o seguinte código para a página\_carga dos showdata.aspx: 

    [!code-html[Main](caching/samples/sample21.html)]
7. Adicione um novo controle SqlDataSource para showdata.aspx e configurá-lo para usar a conexão de banco de dados Northwind. Clique em Avançar.
8. Selecione as caixas de seleção ProductName e ProductID e clique em Avançar.
9. Clique em Concluir.
10. Adicione um novo GridView à página showdata.aspx.
11. Escolha SqlDataSource1 na lista suspensa.
12. Salve e procurar showdata.aspx. Anote o tempo exibido.
