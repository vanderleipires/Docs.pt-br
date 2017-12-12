---
uid: web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
title: "Configuração e instrumentação | Microsoft Docs"
author: microsoft
description: "Há grandes alterações na configuração e instrumentação no ASP.NET 2.0. A nova API de configuração do ASP.NET permite alterações de configuração a serem feitas pr..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 21ebbaee-7ed8-45ae-b6c1-c27c88342e48
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
msc.type: authoredcontent
ms.openlocfilehash: f8d378d3332669ae4606dad8ada06de37e7dfd20
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="configuration-and-instrumentation"></a>Configuração e instrumentação
====================
por [Microsoft](https://github.com/microsoft)

> Há grandes alterações na configuração e instrumentação no ASP.NET 2.0. A nova API de configuração do ASP.NET permite alterações de configuração sejam feitas por meio de programação. Além disso, existem a muitas novas configurações permitem que as novas configurações e instrumentação.


Há grandes alterações na configuração e instrumentação no ASP.NET 2.0. A nova API de configuração do ASP.NET permite alterações de configuração sejam feitas por meio de programação. Além disso, existem a muitas novas configurações permitem que as novas configurações e instrumentação.

Neste módulo, abordaremos a API de configuração do ASP.NET como ele se relaciona com leitura e gravação aos arquivos de configuração do ASP.NET, e também abordaremos instrumentação ASP.NET. Abordaremos também os novos recursos disponíveis no rastreamento do ASP.NET.

## <a name="aspnet-configuration-api"></a>API de configuração do ASP.NET

A API de configuração do ASP.NET permite que você desenvolver, implantar e gerenciar dados de configuração de aplicativo usando uma única interface de programação. Você pode usar a API de configuração para desenvolver e modificar configurações completas do ASP.NET de forma programável sem editar diretamente o XML nos arquivos de configuração. Além disso, você pode usar a API de configuração em aplicativos de console e scripts que você desenvolver, em ferramentas de gerenciamento baseado na Web e em snap-ins do Microsoft Management Console (MMC).

As duas ferramentas de gerenciamento de configuração a seguir usam a API de configuração e são incluídas com o .NET Framework versão 2.0:

- O snap-in do MMC do ASP.NET, que usa a API de configuração para simplificar as tarefas administrativas, fornecendo uma visão integrada de dados de configuração local de todos os níveis da hierarquia de configuração.
- A ferramenta de administração de Site, que permite que você gerencie definições de configuração para aplicativos locais e remotos, inclusive sites hospedados.

A API de configuração do ASP.NET inclui um conjunto de objetos de gerenciamento do ASP.NET que você pode usar para configurar sites e aplicativos de forma programática. Objetos de gerenciamento são implementados como uma biblioteca de classes do .NET Framework. O modelo de programação da API de configuração ajuda a garantir a consistência de código e confiabilidade através da aplicação de tipos de dados em tempo de compilação. Para tornar mais fácil de gerenciar configurações de aplicativo, a API de configuração permite que você exiba os dados que são mesclados de todos os pontos na hierarquia de configuração em uma única coleção, em vez de exibir os dados como coleções separadas de diferentes arquivos de configuração. Além disso, a API de configuração permite que você manipule as configurações de todo o aplicativo sem editar diretamente o XML nos arquivos de configuração. Finalmente, a API simplifica as tarefas de configuração apoiando ferramentas administrativas, como a ferramenta de administração de Site. A API de configuração simplifica a implantação dá suporte à criação de arquivos de configuração em um computador e executando scripts de configuração em vários computadores.

> [!NOTE]
> A API de configuração não oferece suporte à criação de aplicativos do IIS.


## <a name="working-with-local-and-remote-configuration-settings"></a>Trabalhando com definições de configuração Local e remota

Um objeto de configuração representa a visão mesclada dos parâmetros de configuração que se aplicam a uma entidade física específica, como um computador, ou a uma entidade lógica, como um aplicativo ou um site da Web. A entidade lógica especificada pode existir no computador local ou em um servidor remoto. Quando nenhum arquivo de configuração existe para uma entidade especificada, o objeto de configuração representa as configurações padrão, conforme definido no arquivo Machine. config.

Você pode obter um objeto de configuração usando um dos métodos configuração de abertura das seguintes classes:

1. A classe ConfigurationManager, se a entidade é um aplicativo cliente.
2. A classe WebConfigurationManager, se a entidade é um aplicativo Web.

Esses métodos retornará um objeto de configuração, que por sua vez, fornece os métodos e propriedades necessários para lidar com os arquivos de configuração subjacentes. Você pode acessar esses arquivos para leitura ou gravação.

### <a name="reading"></a>Leitura

Você pode usar o método GetSection ou GetSectionGroup para ler informações de configuração. O usuário ou processo que lê deve ter permissões de leitura em todos os arquivos de configuração na hierarquia.

> [!NOTE]
> Se você usar um método estático GetSection que usa um parâmetro de caminho, o parâmetro path deve se referir ao aplicativo no qual o código está sendo executado. Caso contrário, o parâmetro será ignorado e as informações de configuração para o aplicativo em execução no momento são retornadas.


### <a name="writing"></a>Gravação

Você pode usar um dos métodos de gravação para gravar informações de configuração. O usuário ou processo que grava deve ter permissões no arquivo de configuração e diretório no nível da hierarquia de configuração atual de gravação, bem como permissões de leitura em todos os arquivos de configuração na hierarquia.

Para gerar um arquivo de configuração que representa as definições de configuração herdados para uma entidade especificada, use um dos seguintes métodos de salvar configuração:

1. O método de salvar para criar um novo arquivo de configuração.
2. O método SaveAs para gerar um novo arquivo de configuração em outro local.

## <a name="configuration-classes-and-namespaces"></a>Namespaces e Classes de configuração

Muitas classes de configuração e métodos são semelhantes entre si. A tabela a seguir descreve as classes de configuração mais comumente usadas e namespaces.

| **Namespace ou classe de configuração** | **Descrição** |
| --- | --- |
| [System. Configuration](https://msdn.microsoft.com/en-us/library/system.configuration.aspx) namespace | Contém as classes de configuração principais para todos os aplicativos do .NET Framework. Classes de manipulador de seção são usadas para obter dados de configuração para uma seção de métodos, como GetSection e GetSectionGroup. Esses dois métodos são não-estático. |
| Classe System.Configuration.Configuration | Representa um conjunto de dados de configuração para um computador, aplicativo, diretório da Web ou outro recurso. Essa classe contém métodos úteis, como GetSection e GetSectionGroup, para atualizar as definições de configuração e obter referências para seções e grupos de seção. Essa classe é usada como um tipo de retorno para métodos que obtêm dados de configuração de tempo de design, como os métodos das classes WebConfigurationManager e ConfigurationManager. |
| Namespace System.Web.Configuration | Contém as classes de manipulador de seção para as seções de configuração do ASP.NET definidas em [definições de configuração do ASP.NET](https://msdn.microsoft.com/en-us/library/b5ysx397.aspx). Classes de manipulador de seção são usadas para obter dados de configuração para uma seção de métodos, como GetSection e GetSectionGroup. |
| Classe System.Web.Configuration.WebConfigurationManager | Fornece métodos úteis para obter referências para as definições de configuração de tempo de execução e tempo de design. Esses métodos usam a classe System.Configuration.Configuration como um tipo de retorno. Você pode usar o método estático GetSection dessa classe ou o método de GetSection não-estático da classe System.Configuration.ConfigurationManager alternadamente. Para configurações de aplicativo da Web, a classe System.Web.Configuration.WebConfigurationManager é recomendada em vez da classe System.Configuration.ConfigurationManager. |
| [System.Configuration.Provider](https://msdn.microsoft.com/en-us/library/system.configuration.provider.aspx) namespace | Fornece uma maneira de personalizar e estender o provedor de configuração. Esta é a classe base para todas as classes de provedor no sistema de configuração. |
| [System.Web.Management](https://msdn.microsoft.com/en-us/library/system.web.management.aspx) namespace | Contém classes e interfaces para gerenciar e monitorar a integridade de aplicativos da Web. Estritamente falando, esse namespace não é considerado parte da configuração de API. Por exemplo, o rastreamento e o acionamento do evento é realizado pelas classes neste namespace. |
| [System.Management.Instrumentation](https://msdn.microsoft.com/en-us/library/system.management.instrumentation.aspx) namespace | Fornece as classes necessárias para a instrumentação de aplicativos para expor suas informações de gerenciamento e os eventos por meio do Windows Management Instrumentation (WMI) para os consumidores em potencial. Monitoramento de integridade do ASP.NET usa WMI para entregar eventos. Estritamente falando, esse namespace não é considerado parte da configuração de API. |

## <a name="reading-from-aspnet-configuration-files"></a>Leitura de arquivos de configuração do ASP.NET

A classe WebConfigurationManager é a classe principal para a leitura de arquivos de configuração do ASP.NET. Há basicamente três etapas para ler arquivos de configuração do ASP.NET:

1. Obter um objeto de configuração usando o método OpenWebConfiguration.
2. Obter uma referência para a seção desejada no arquivo de configuração.
3. Leia as informações desejadas no arquivo de configuração.

A configuração do objeto representa não representa um arquivo de configuração específico. Em vez disso, ele representa uma exibição mesclada da configuração de um computador, aplicativo ou site da Web. O exemplo de código a seguir cria um objeto de configuração que representa a configuração de um aplicativo Web chamado *ProductInfo*.

[!code-csharp[Main](configuration-and-instrumentation/samples/sample1.cs)]

> [!NOTE]
> Observe que, se o caminho /ProductInfo não existir, o código acima retornará a configuração padrão, conforme especificado no arquivo Machine. config.


Uma vez que o objeto de configuração, você pode usar o método GetSection ou GetSectionGroup para analisar as definições de configuração. O exemplo a seguir obtém uma referência para as configurações de representação para o aplicativo ProductInfo acima:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample2.cs)]

## <a name="writing-to-aspnet-configuration-files"></a>Escrevendo em arquivos de configuração do ASP.NET

Como a leitura de arquivos de configuração, a classe WebConfigurationManager é o núcleo para gravar em arquivos de configuração do Asp.NET. Também há três etapas para gravar arquivos de configuração do ASP.NET.

1. Obter um objeto de configuração usando o método OpenWebConfiguration.
2. Obter uma referência para a seção desejada no arquivo de configuração.
3. As informações desejadas de gravação do arquivo de configuração usando a salvar ou salvar como método.

O código a seguir as alterações a **depurar** atributo do &lt;compilação&gt; elemento como false:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample3.cs)]

Quando esse código é executado, o **depurar** atributo do &lt;compilação&gt; elemento será definido como false para o *webApp* . config do aplicativo.

## <a name="systemwebmanagement-namespace"></a>Namespace System.Web.Management

O namespace System.Web.Management fornece as classes e interfaces para gerenciar e monitorar a integridade dos aplicativos ASP.NET.

Registro em log é feito definindo uma regra que associa a um provedor de eventos. A regra define o tipo de eventos que são enviados para o provedor. Os seguintes eventos base estão disponíveis para você fazer logon:

| **WebBaseEvent** | A classe de evento de base para todos os eventos. Contém as propriedades de todos os eventos, como o código de evento, o código de detalhes do evento, data e hora que o evento foi gerado, número de sequência, a mensagem de evento e detalhes do evento. |
| --- | --- |
| **WebManagementEvent** | A classe de evento de base para eventos de gerenciamento, como tempo de vida do aplicativo, solicitação, erros e eventos de auditoria. |
| **WebHeartbeatEvent** | O evento gerado pelo aplicativo em intervalos regulares para capturar informações de estado de tempo de execução útil. |
| **WebAuditEvent** | A classe base para eventos de auditoria de segurança, que são usados para marcar condições, como falha de autorização, falha de descriptografia, *etc.* |
| **WebRequestEvent** | A classe base para todos os eventos de informação de solicitação. |
| **WebBaseErrorEvent** | A classe base para todos os eventos indicando a condições de erro. |

Os tipos de provedores disponíveis permitem que você enviar a saída de evento para o Visualizador de eventos, o SQL Server, o Windows Management Instrumentation (WMI) e o email. Os mapeamentos de evento e provedores pré-configurado reduzem a quantidade de trabalho necessário para obter saída de evento registrada.

O ASP.NET 2.0 usa o Log de eventos provedor-do-prontos para registrar eventos com base em domínios de aplicativo, iniciando e parando, bem como registro em log todas as exceções sem tratamento. Isso ajuda a abordar alguns dos cenários básicos. Por exemplo, digamos que seu aplicativo lançará uma exceção, mas o usuário não salvar o erro e não é possível reproduzi-lo. Com a regra de Log de eventos padrão, você poderá coletar as informações de exceção e a pilha para obter uma ideia melhor do que tipo de erro ocorreu. Aplica-se outro exemplo, se seu aplicativo está perdendo estado da sessão. Nesse caso, você pode examinar o Log de eventos para determinar se a reciclagem de domínio do aplicativo, e por que o domínio de aplicativo foi interrompido em primeiro lugar.

Além disso, o sistema de monitoramento de integridade é extensível. Por exemplo, definir os eventos da Web personalizados, acioná-los dentro de seu aplicativo e, em seguida, definir uma regra para enviar as informações de evento para um provedor, como o email. Isso permite que você facilmente vincular sua instrumentação para provedores de monitoramento de integridade. Como outro exemplo, você pode disparar um evento sempre que um pedido é processado e configurar uma regra que envia cada evento no banco de dados do SQL Server. Você também pode disparar um evento quando um usuário não conseguir fazer logon várias vezes em uma linha e configurar o evento para usar os provedores baseados em email.

A configuração para os eventos e os provedores padrão é armazenada no arquivo Web. config global. O arquivo Web. config global armazena todas as baseado na Web configurações que foram armazenadas no arquivo Machine. config no ASP.NET 1 x. O arquivo global de Web. config está localizado no seguinte diretório:

`%windir%\Microsoft.Net\Framework\v2.0.*\config\Web.config`

O &lt;healthMonitoring&gt; seção do arquivo Web. config global fornece padrão definições de configuração. Você pode substituir essa configuração ou configurar suas próprias configurações implementando a &lt;healthMonitoring&gt; seção no arquivo Web. config para seu aplicativo.

O &lt;healthMonitoring&gt; seção do arquivo Web. config global contém os seguintes itens:

| **provedores** | Contém os provedores configurado para o Visualizador de eventos, o WMI e o SQL Server. |
| --- | --- |
| **eventMappings** | Contém mapeamentos para várias classes de WebBase. Você pode estender esta lista se você gerar sua própria classe de evento. Gerar sua própria classe de evento oferece granularidade mais fina sobre os provedores de para que enviar informações. Por exemplo, você pode configurar exceções sem tratamento a ser enviada ao SQL Server, ao enviar seus próprios eventos personalizados para enviar por email. |
| **regras** | Links eventMappings ao provedor. |
| **armazenamento em buffer** | Usada com provedores de email e o SQL Server para determinar a frequência de eventos de liberação para o provedor. |

Abaixo está um exemplo de código do arquivo Web. config global.

[!code-xml[Main](configuration-and-instrumentation/samples/sample4.xml)]

## <a name="how-to-store-events-to-event-viewer"></a>Como armazenar eventos no Visualizador de eventos

Como mencionado anteriormente, o provedor de eventos de log de eventos Visualizador é configurado por você no arquivo Web. config global. Por padrão, todos os eventos com base em **WebBaseErrorEvent** e **WebFailureAuditEvent** são registrados em log. Você pode adicionar regras adicionais para registrar informações adicionais no Log de eventos. Por exemplo, se você quiser que todos os eventos de log (*ou seja,*, todos os eventos com base em **WebBaseEvent**), você pode adicionar a regra a seguir ao arquivo Web. config:

[!code-xml[Main](configuration-and-instrumentation/samples/sample5.xml)]

Essa regra deve vincular o **todos os eventos** mapa de evento para o provedor de Log de eventos. EventMapping e o provedor são incluídos no arquivo Web. config global.

## <a name="how-to-store-events-to-sql-server"></a>Como armazenar os eventos para o SQL Server

Esse método usa o **ASPNETDB** banco de dados, que é gerado pelo Aspnet\_regsql.exe ferramenta. O provedor padrão usa a cadeia de caracteres de conexão do LocalSqlServer, que usa um baseado em arquivo de banco de dados no aplicativo\_pasta de dados ou instância SQLExpress local do SQL Server. A cadeia de caracteres de conexão LocalSqlServer e o SqlProvider são configurados no arquivo Web. config global.

A cadeia de caracteres de conexão LocalSqlServer no arquivo Web. config global tem esta aparência:

[!code-xml[Main](configuration-and-instrumentation/samples/sample6.xml)]

Se você quiser usar outra instância do SQL Server, você precisará usar o Aspnet\_regsql.exe ferramenta, que pode ser encontrada em % windir%\Microsoft.Net\Framework\v2.0.\* pasta. Use o Aspnet\_regsql.exe ferramenta para gerar um personalizado **ASPNETDB** banco de dados na instância do SQL Server, em seguida, adicionar a cadeia de caracteres de conexão para o arquivo de configuração de aplicativos e, em seguida, adicionar um provedor usando o novo cadeia de caracteres de conexão. Uma vez que o **ASPNETDB** banco de dados criado, você precisará definir uma regra para vincular um eventMapping o sqlProvider.

Se você usa o padrão SqlProvider ou configurar seu próprio provedor, você precisará adicionar uma regra que o provedor com um mapa de eventos de vinculação. A regra a seguir vincula o novo provedor criado anteriormente para o **todos os eventos** mapa de evento. Essa regra registrará em log todos os eventos com base em **WebBaseEvent** e enviá-los para o MySqlWebEventProvider que usará a cadeia de caracteres de conexão MYASPNETDB. O código a seguir adiciona uma regra para vincular o provedor com um mapa de eventos:

[!code-xml[Main](configuration-and-instrumentation/samples/sample7.xml)]

Se quiser só enviar erros para o SQL Server, você poderá adicionar a regra a seguir:

[!code-xml[Main](configuration-and-instrumentation/samples/sample8.xml)]

## <a name="how-to-forward-events-to-wmi"></a>Como o encaminhamento de eventos WMI

Você também pode encaminhar os eventos WMI. O provedor WMI é configurado por você no arquivo Web. config global por padrão.

O exemplo de código a seguir adiciona uma regra para encaminhar os eventos WMI:

[!code-xml[Main](configuration-and-instrumentation/samples/sample9.xml)]

Você precisará adicionar uma regra para associar um eventMapping para o provedor e um aplicativo de escuta do WMI para ouvir os eventos. O exemplo de código a seguir adiciona uma regra para vincular o provedor WMI para o **todos os eventos** mapa de evento:

[!code-xml[Main](configuration-and-instrumentation/samples/sample10.xml)]

## <a name="how-to-forward-events-to-e-mail"></a>Como encaminhar eventos para enviar por email

Você também pode encaminhar eventos para enviar por email. Tenha cuidado ao quais regras de evento que você mapear para o provedor de email, como você pode acidentalmente enviar por conta própria muitas informações que podem ser mais adequados para o SQL Server ou o Log de eventos. Há dois provedores de email; SimpleMailWebEventProvider e TemplatedMailWebEventProvider. Cada um tem os mesmos atributos de configuração, com exceção dos atributos "template" e "detailedTemplateErrors", que só estão disponíveis no TemplatedMailWebEventProvider.

> [!NOTE]
> Nenhum dos provedores de email é configurado por você. Você precisará adicioná-los ao seu arquivo Web. config.


A principal diferença entre esses provedores de duas email é que SimpleMailWebEventProvider envia emails em um modelo genérico que não pode ser modificado. O arquivo Web. config de exemplo adiciona esse provedor de email à lista de provedores configurados usando a seguinte regra:

[!code-xml[Main](configuration-and-instrumentation/samples/sample11.xml)]

A regra a seguir também é adicionada para ligar o provedor de email para o **todos os eventos** mapa de evento:

[!code-xml[Main](configuration-and-instrumentation/samples/sample12.xml)]

## <a name="aspnet-20-tracing"></a>O ASP.NET 2.0 de rastreamento

Há três principais aprimoramentos para o rastreamento no ASP.NET 2.0.

1. Funcionalidade de rastreamento integrado
2. Acesso programático para as mensagens de rastreamento
3. Rastreamento de aplicativo aprimorado

## <a name="integrated-tracing-functionality"></a>Integrado a funcionalidade de rastreamento

Você pode rotear mensagens emitidas pela classe Trace para rastreamento de saída ASP.NET e rotear mensagens emitidas pelo rastreamento do ASP.NET para Trace. Você também pode encaminhar eventos de instrumentação ASP.NET para Trace. Essa funcionalidade é fornecida pelo novo **writeToDiagnosticsTrace** atributo o &lt;rastreamento&gt; elemento. Quando esse valor booliano for verdadeiro, as mensagens de rastreamento do ASP.NET são encaminhadas para a infraestrutura de rastreamento de System. Diagnostics para uso por todos os ouvintes registrados para exibir mensagens de rastreamento.

## <a name="programmatic-access-to-trace-messages"></a>Acesso programático às mensagens de rastreamento

ASP.NET 2.0 permite acesso programático a todas as mensagens de rastreamento por meio de **TraceContextRecord** classe e o **TraceRecords** coleção. É a maneira mais eficiente de acessar as mensagens de rastreamento registrar um **TraceContextEventHandler** delegado (também novo no ASP.NET 2.0) para lidar com o novo **TraceFinished** eventos. Você pode, em seguida, fazer loop por meio de mensagens de rastreamento como desejar.

O exemplo de código a seguir ilustra isso:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample13.cs)]

No exemplo acima, eu loop pela coleção TraceRecords e, em seguida, gravar cada mensagem no fluxo de resposta.

## <a name="improved-application-level-tracing"></a>Rastreamento de aplicativo aprimorado

Rastreamento de aplicativo é aprimorado por meio da introdução do novo **mostRecent** atributo o &lt;rastreamento&gt; elemento. Esse atributo especifica se a saída de rastreamento de aplicativo mais recente é exibida e os dados antigos de rastreamento além dos limites que são indicados pelo requestLimit são descartados. Se for falso, os dados de rastreamento são exibidos para solicitações até que o atributo requestLimit seja atingido.

## <a name="aspnet-command-line-tools"></a>Ferramentas de linha de comando do ASP.NET

Há várias ferramentas de linha de comando para ajudar na configuração do ASP.NET. Os desenvolvedores do ASP.NET devem estar familiarizados com o aspnet\_regiis.exe ferramenta. O ASP.NET 2.0 fornece três outras ferramentas de linha de comando para ajudar na configuração.

As ferramentas de linha de comando a seguir estão disponíveis:

| **Ferramenta** | **Use** |
| --- | --- |
| **ASPNET\_regiis.exe** | Permite que o registro do ASP.NET com o IIS. Há duas versões dessas ferramentas fornecidos com o ASP.NET 2.0, uma para sistemas de 32 bits (na pasta do Framework) e outra para sistemas de 64 bits (na pasta Framework64.) A versão de 64 bits não será instalada em um sistema operacional de 32 bits. |
| **ASPNET\_regsql.exe** | A ferramenta de registro do SQL Server do ASP.NET é usada para criar um banco de dados do Microsoft SQL Server para uso pelos provedores do SQL Server no ASP.NET, ou para adicionar ou remover opções de um banco de dados existente. O Aspnet\_regsql.exe arquivo está localizado na [drive:]\WINDOWS\Microsoft.NET\Framework\versionNumber pasta no servidor Web. |
| **ASPNET\_regbrowsers.exe** | A ferramenta de registro de navegador ASP.NET analisa e compila todas as definições de navegador de todo o sistema em um assembly e instala o assembly no cache de assembly global. A ferramenta usa os arquivos de definição de navegador (. Arquivos de navegador) do subdiretório navegadores do .NET Framework. A ferramenta pode ser encontrada no diretório %SystemRoot%\Microsoft.NET\Framework\version\. |
| **ASPNET\_compiler.exe** | A ferramenta de compilação do ASP.NET permite que você compilar um aplicativo Web ASP.NET, no local ou para implantação em um local de destino como um servidor de produção. Compilação no local ajuda o desempenho do aplicativo porque os usuários finais não encontrar um atraso na primeira solicitação para o aplicativo enquanto o aplicativo é compilado. |

Porque o aspnet\_regiis.exe ferramenta não é nova no ASP.NET 2.0, não abordaremos-lo aqui.

## <a name="aspnet-sql-server-registration-tool---aspnetregsqlexe"></a>Ferramenta de registro do servidor SQL do ASP.NET - aspnet\_regsql.exe

Você pode definir vários tipos de opções usando a ferramenta de registro do SQL Server do ASP.NET. Você pode especificar uma conexão de SQL, especifique quais serviços de aplicativo ASP.NET usam SQL Server para gerenciar informações, indicar qual banco de dados ou tabela é usada para dependência de cache SQL e adicionar ou remover suporte para usar o SQL Server para armazenar o estado da sessão e procedimentos.

Vários serviços de aplicativos ASP.NET contam com um provedor para gerenciar, armazenar e recuperar dados de uma fonte de dados. Cada provedor é específico para a fonte de dados. O ASP.NET inclui um provedor do SQL Server para os seguintes recursos do ASP.NET:

- Associação (o [SqlMembershipProvider](https://msdn.microsoft.com/en-us/library/system.web.security.sqlmembershipprovider.aspx) classe).
- Gerenciamento de função (a [SqlRoleProvider](https://msdn.microsoft.com/en-us/library/system.web.security.sqlroleprovider.aspx) classe).
- Perfil (o [SqlProfileProvider](https://msdn.microsoft.com/en-us/library/system.web.profile.sqlprofileprovider.aspx) classe).
- Personalização de Web Parts (o [SqlPersonalizationProvider](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.webparts.sqlpersonalizationprovider.aspx) classe).
- Eventos da Web (o [SqlWebEventProvider](https://msdn.microsoft.com/en-us/library/system.web.management.sqlwebeventprovider.aspx) classe).

Quando você instala o ASP.NET, o arquivo Machine. config para seu servidor inclui elementos de configuração que especifiquem provedores SQL Server para cada um dos recursos do ASP.NET que contam com um provedor. Esses provedores são configurados, por padrão, para se conectar a uma instância de usuário local do SQL Server Express 2005. Se você alterar a cadeia de conexão padrão usada pelos provedores, em seguida, antes de usar os recursos do ASP.NET configurado na configuração do computador, você deve instalar o banco de dados do SQL Server e os elementos de banco de dados para o recurso escolhido usando Aspnet\_regsql.exe. Se o banco de dados que você especificar com a ferramenta de registro do SQL já não existe (aspnetdb será o banco de dados padrão se nenhuma for especificada na linha de comando), em seguida, o usuário atual deve ter direitos para criar bancos de dados no SQL Server, bem como criar e de esquema lements dentro de um banco de dados.

### <a name="sql-cache-dependency"></a>Dependência de Cache do SQL

Um recurso avançado do cache de saída do ASP.NET é a dependência de cache do SQL. Dependência de cache SQL dá suporte a dois modos diferentes de operação: um que usa uma implementação do ASP.NET de sondagem de tabela e um segundo modo que usa os recursos de notificação de consulta do SQL Server 2005. A ferramenta de registro do SQL pode ser usada para configurar o modo de tabela de pesquisa de operação.

### <a name="session-state"></a>Estado da sessão

Por padrão, informações e valores de estado de sessão são armazenados na memória no processo do ASP.NET. Como alternativa, você pode armazenar dados de sessão em um banco de dados do SQL Server, onde ele pode ser compartilhado por vários servidores Web. Se o banco de dados que você especificar para o estado de sessão com a ferramenta de registro do SQL ainda não existir, o usuário atual deve ter direitos para criar bancos de dados no SQL Server, bem como para criar elementos de esquema em um banco de dados. Se o banco de dados existir, o usuário atual deve ter direitos para criar elementos de esquema no banco de dados existente.

Para instalar o banco de dados de estado de sessão no SQL Server, execute o Aspnet\_regsql.exe ferramenta e fornecer as informações com o comando a seguir:

- O nome do SQL Server da instância, usando o **-S** opção.
- As credenciais de logon para uma conta que tenha permissão para criar um banco de dados em um computador executando o SQL Server. Use o **-E** opção para usar o usuário conectado no momento, ou usar o **- U** opção para especificar uma ID de usuário juntamente com o **-P** opção para especificar uma senha.
- O **- ssadd** opção de linha de comando para adicionar o banco de dados de estado de sessão.

Por padrão, você não pode usar o Aspnet\_regsql.exe ferramenta para instalar o banco de dados de estado de sessão em um computador executando o SQL Server 2005 Express Edition.

### <a name="the-aspnet-browser-registration-tool---aspnetregbrowsersexe"></a>A ferramenta de registro de navegador do ASP.NET - aspnet\_regbrowsers.exe

Na versão 1.1 do ASP.NET, o arquivo Machine. config continha uma seção chamada &lt;browserCaps&gt;. Esta seção continha uma série de entradas XML que definiu as configurações para vários navegadores com base em uma expressão regular. Para o ASP.NET versão 2.0, um novo. Arquivo de navegador define os parâmetros de um navegador específico usando as entradas de XML. Você pode adicionar informações em um novo navegador adicionando um novo. Arquivo de navegador para a pasta localizada em %SystemRoot%\Microsoft.NET\Framework\version\CONFIG\Browsers em seu sistema.

Como um aplicativo não está lendo um arquivo. config toda vez que ele requer informações do navegador, você pode criar um novo. Arquivo de navegador e execução Aspnet\_regbrowsers.exe para adicionar as alterações necessárias para o assembly. Isso permite que o servidor acessar as novas informações de navegador imediatamente, você não precisa desligar qualquer um dos seus aplicativos para obter as informações. Um aplicativo pode acessar os recursos do navegador por meio da propriedade de navegador de solicitação de HTTP atual.

As seguintes opções estão disponíveis durante a execução aspnet\_regbrowser.exe:

| **Opção** | **Descrição** |
| --- | --- |
| **-?** | Exibe o Aspnet\_regbbrowsers.exe texto de ajuda na janela de comando. |
| **-i** | Cria o assembly de recursos do navegador de tempo de execução e instala-o no cache de assembly global. |
| **-u** | Desinstala o assembly de recursos do navegador de tempo de execução do cache de assembly global. |

## <a name="the-aspnet-compilation-tool---aspnetcompilerexe"></a>A ferramenta de compilação do ASP.NET - aspnet\_compiler.exe

A ferramenta de compilação do ASP.NET pode ser usada de duas maneiras de gerais: para o local e a compilação para implantação, onde um diretório de saída de destino é especificado.

### <a name="compiling-an-application-in-placehttpsmsdnmicrosoftcomen-uslibraryms229863aspx"></a>[Compilar um aplicativo no local](https://msdn.microsoft.com/en-us/library/ms229863.aspx)

A ferramenta de compilação do ASP.NET pode compilar um aplicativo no local, ou seja, imita o comportamento de fazer várias solicitações para o aplicativo, fazendo com que a compilação normal. Os usuários de um site pré-compilado não experimentarão um atraso causado por compilar a página na primeira solicitação.

Quando você pré-compilar um site no local, os itens a seguir se aplicam:

- O site mantém seus arquivos e a estrutura de diretórios.
- Você deve ter compiladores para todas as linguagens de programação usadas pelo site no servidor.
- Se qualquer arquivo falhar compilação, todo o site na compilação.

Você também pode recompilar um aplicativo em vigor depois da adição de novos arquivos de origem para ele. A ferramenta compila somente os arquivos novos ou alterados, a menos que você incluir o **- c** opção.

> [!NOTE]
> Compilação de um aplicativo que contém um aplicativo aninhado não compila o aplicativo aninhado. O aplicativo aninhado deve ser compilado separadamente.


### <a name="compiling-an-application-for-deploymenthttpsmsdnmicrosoftcomen-uslibraryms229863aspx"></a>[Compilando um aplicativo para implantação](https://msdn.microsoft.com/en-us/library/ms229863.aspx)

Compilar um aplicativo para implantação (compilação de um local de destino), especificando o parâmetro targetDir. TargetDir pode ser o local final para o aplicativo Web ou aplicativo compilado pode ser implantado. Usando o **-u** opção compila o aplicativo de forma que você pode fazer alterações a certos arquivos no aplicativo compilado sem recompilar a ele. ASPNET\_compiler.exe faz uma distinção entre tipos de arquivo estático e dinâmico e trata-los de maneira diferente ao criar o aplicativo resultante.

- Tipos de arquivo estático são aqueles que não têm um compilador associado ou criar um provedor, como arquivos cujo nome tem extensões como. CSS,. gif,. htm,. HTML,. jpg,. js e assim por diante. Simplesmente, esses arquivos são copiados para o local de destino, com seus lugares relativos da estrutura de diretórios preservados.
- Tipos de arquivo dinâmicos são aqueles que têm um compilador associado ou criar um provedor, incluindo arquivos com extensões de nome de arquivo específico do ASP.NET, como. asax,. ascx,. ashx,. aspx,. browser,. master e assim por diante. A ferramenta de compilação do ASP.NET gera assemblies desses arquivos. Se o **-u** opção for omitida, a ferramenta também cria arquivos com a extensão de nome de arquivo. COMPILED que mapeiam os arquivos de origem para o seu assembly. Para garantir que a estrutura do diretório da fonte de aplicativo seja preservada, a ferramenta gera arquivos de espaço reservado nos locais correspondentes no aplicativo de destino.

Você deve usar o **-u** opção para indicar que o conteúdo do aplicativo compilado pode ser modificado. Caso contrário, as modificações subsequentes serão ignoradas ou causam erros de tempo de execução.

A tabela a seguir descreve como o compilação do ASP.NET ferramenta identificadores diferentes tipos de arquivos quando o **-u** opção estiver incluída.

| **Tipo de arquivo** | **Ação do compilador** |
| --- | --- |
| . ascx,. aspx,. master | Esses arquivos são divididos em marcação e código-fonte, que inclui os arquivos code-behind e qualquer código que é incluído na &lt;script runat = "server"&gt; elementos. Código-fonte é compilado em assemblies com nomes que são derivados de um algoritmo de hash, e os assemblies são colocados no diretório Bin. Qualquer código embutido, que é, o código colocado entre o  **&lt; %**  e  **% &gt;**  colchetes, incluído com a marcação e não compilado. Novos arquivos com o mesmo nome que os arquivos de origem são criados para conter a marcação e colocados nos diretórios de saída correspondente. |
| . ashx,. asmx | Esses arquivos não são compilados e são movidos para os diretórios de saída como está e não compilado. Se você quiser que o código de manipulador compilado, coloque o código em arquivos de código fonte no aplicativo\_diretório de código. |
| . cs,. vb,. jsl,. cpp (não incluindo os arquivos code-behind para os tipos de arquivo listados anteriormente) | Esses arquivos são compilados e incluídos como um recurso em assemblies que fazem referência a eles. Arquivos de origem não são copiados para o diretório de saída. Se um arquivo de código não for referenciado, ele não é compilado. |
| Tipos de arquivos personalizados | Esses arquivos não são compilados. Esses arquivos são copiados para os diretórios de saída correspondente. |
| Arquivos de código no aplicativo de origem\_subdiretório de código | Esses arquivos são compilados em assemblies e colocados no diretório Bin. |
| arquivos. resx e. Resource no aplicativo\_GlobalResources subdiretório | Esses arquivos são compilados em assemblies e colocados no diretório Bin. Nenhum aplicativo\_GlobalResources subdiretório é criado sob o diretório de saída principal, e nenhum arquivo. resx ou. Resources foi localizado no diretório de origem é copiado para os diretórios de saída. |
| arquivos. resx e. Resource no aplicativo\_subdiretório LocalResources | Esses arquivos não são compilados e são copiados para os diretórios de saída correspondente. |
| arquivos. skin no aplicativo\_subdiretório de temas | Os arquivos. skin e tema estáticos não são compilados e são copiados para os diretórios de saída correspondente. |
| . browser arquivo Web. config estático tipos de módulos (assemblies) já está presente no diretório Bin | Esses arquivos são copiados como nos diretórios de saída. |

A tabela a seguir descreve como o compilação do ASP.NET ferramenta identificadores diferentes tipos de arquivos quando o **-u** opção for omitida.

| **Tipo de arquivo** | **Ação do compilador** |
| --- | --- |
| . aspx,. asmx,. ashx,. master | Esses arquivos são divididos em marcação e código-fonte, que inclui os arquivos code-behind e qualquer código que é incluído na &lt;script runat = "server"&gt; elementos. Código-fonte é compilado em assemblies com nomes que são derivados de um algoritmo de hash. Os conjuntos resultantes são colocados no diretório Bin. Qualquer código embutido, que é, o código colocado entre o  **&lt; %**  e  **% &gt;**  colchetes, incluído com a marcação e não compilado. O compilador cria novos arquivos para conter a marcação com o mesmo nome que os arquivos de origem. Esses arquivos resultantes são colocados no diretório Bin. O compilador também cria arquivos com o mesmo nome que os arquivos de origem, mas com a extensão. COMPILED que contêm informações de mapeamento. A. Arquivos COMPILADOS são colocados em diretórios de saída correspondente para o local original dos arquivos de origem. |
| . ascx | Esses arquivos são divididos em marcação e código-fonte. Código-fonte é compilado em assemblies e colocado no diretório Bin, com nomes que são derivados de um algoritmo de hash. Nenhum arquivo de marcação é gerado. |
| . cs,. vb,. jsl,. cpp (não incluindo os arquivos code-behind para os tipos de arquivo listados anteriormente) | Código-fonte que é referenciado pelos assemblies gerados a partir de arquivos. aspx,. ashx ou. ascx é compilado em assemblies e colocado no diretório Bin. Não há arquivos de origem são copiados. |
| Tipos de arquivos personalizados | Esses arquivos são compilados como arquivos dinâmicos. Dependendo do tipo de arquivo, em que elas se baseiam, o compilador pode colocar arquivos de mapeamento em diretórios de saída. |
| Arquivos no aplicativo\_subdiretório de código | Arquivos de código fonte desse subdiretório são compilados em assemblies e colocados no diretório Bin. |
| Arquivos no aplicativo\_GlobalResources subdiretório | Esses arquivos são compilados em assemblies e colocados no diretório Bin. Nenhum aplicativo\_GlobalResources subdiretório é criado sob o diretório de saída principal. Se o arquivo de configuração especifica appliesTo = "All", os arquivos. resx e. resources são copiados para os diretórios de saída. Eles não são copiados se eles são referenciados por uma [BuildProvider](https://msdn.microsoft.com/en-us/library/system.web.configuration.buildprovider.aspx). |
| arquivos. resx e. Resource no aplicativo\_subdiretório LocalResources | Esses arquivos são compilados em assemblies com nomes exclusivos e colocados no diretório Bin. Nenhum arquivo. resx ou. Resource é copiado para os diretórios de saída. |
| arquivos. skin no aplicativo\_subdiretório de temas | Temas são compilados em assemblies e colocados no diretório Bin. Arquivos de stub são criados para arquivos. skin e colocados no diretório de saída correspondente. Arquivos estáticos (como. CSS) são copiados para os diretórios de saída. |
| . browser arquivo Web. config estático tipos de módulos (assemblies) já está presente no diretório Bin | Esses arquivos são copiados como é o diretório de saída. |

### <a name="fixed-assembly-nameshttpsmsdnmicrosoftcomen-uslibraryms229863aspx"></a>[Nomes de Assembly fixa](https://msdn.microsoft.com/en-us/library/ms229863.aspx##)

Alguns cenários, como implantar um aplicativo Web usando o Windows Installer de MSI, exigem o uso de nomes de arquivo consistente e conteúdo, bem como estruturas de diretório consistente para identificar os assemblies ou definições de configuração para atualizações. Nesses casos, você pode usar o **- fixednames** opção para especificar que a ferramenta de compilação do ASP.NET deve compilar um assembly para cada arquivo de origem em vez de usar onde várias páginas são compiladas em assemblies. Isso pode levar a um grande número de assemblies, portanto, se você estiver preocupado com a capacidade de expansão devem usar essa opção com cuidado.

### <a name="strong-name-compilationhttpsmsdnmicrosoftcomen-uslibraryms229863aspx"></a>[Compilação de nome forte](https://msdn.microsoft.com/en-us/library/ms229863.aspx##)

O **- aptca**, **- delaysign**, **- keycontainer** e **- keyfile** opções são fornecidas para que você possa usar Aspnet\_ Compiler.exe altamente criar conjuntos nomeados sem usar o [ferramenta de nome forte (Sn.exe)](https://msdn.microsoft.com/en-us/library/k5b5tt23.aspx) separadamente. Essas opções correspondem, respectivamente, para **AllowPartiallyTrustedCallersAttribute**, **AssemblyDelaySignAttribute**, **AssemblyKeyNameAttribute**e  **AssemblyKeyFileAttribute**.

Descrição desses atributos está fora do escopo deste curso.

## <a name="labs"></a>Laboratórios

Cada um dos seguintes laboratórios amplia os laboratórios anteriores. Você precisará executá-las em ordem.

## <a name="lab-1-using-the-configuration-api"></a>Laboratório 1: Usando a API de configuração

1. Criar um novo site chamado *mod9lab*.
2. Adicione um novo arquivo de configuração da Web para o site.
3. Adicione o seguinte ao arquivo Web. config:


[!code-xml[Main](configuration-and-instrumentation/samples/sample14.xml)]

Isso garantirá que você tem permissão para salvar as alterações no arquivo Web. config.

1. Adicionar um novo controle de rótulo default. aspx e alterar a ID para **lblDebugStatus**.
2. Adicione um novo controle de botão para Default.aspx.
3. Alterar a ID do controle Button para **btnToggleDebug** e o texto a ser **alternância depurar Status**.
4. Abra o modo de exibição de código para o arquivo code-behind de Default. aspx e adicione um **usando** instrução **System.Web.Configuration** da seguinte maneira:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample15.cs)]

1. Adicionar duas variáveis privadas para a classe e uma página\_método Init, conforme mostrado abaixo:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample16.cs)]

1. Adicione o seguinte código à página\_carga:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample17.cs)]

1. Salve e procurar default.aspx. Observe que o controle de rótulo exibe o status de depuração atual.
2. Clique duas vezes no controle de botão no designer e adicione o seguinte código ao evento de clique para o controle de botão:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample18.cs)]

1. Salvar e procurar default.aspx e clique no botão.
2. Abrir o arquivo Web. config após cada botão Clique e observar o **depurar** atributo o &lt;compilação&gt; seção.

## <a name="lab-2-logging-application-restarts"></a>Laboratório 2: Log reinicializações de aplicativo

Neste laboratório, você criará um código que permitirá que você alterne o log de encerrar os aplicativos, inicializações e recompilações no Visualizador de eventos.

1. Adicionar DropDownList para default.aspx e altere a ID para ddlLogAppEvents.
2. Definir o **AutoPostBack** propriedade DropDownList para **true**.
3. Adicione três itens à coleção de itens para DropDownList. Verifique o **texto** do primeiro item *Selecionar valor* e o valor -1. Verifique o **texto** e **valor** do segundo item **True** e **texto** e **valor** do terceiro item **False**.
4. Adicione um novo rótulo para default.aspx. Alterar a ID para **lblLogAppEvents**.
5. Abra o modo de exibição por trás do código default.aspx e adicionar uma nova declaração de uma variável do tipo HealthMonitoringSection, conforme mostrado abaixo:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample19.cs)]

1. Adicione o seguinte código para o código existente na página\_Init:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample20.cs)]

1. Clique duas vezes em DropDownList e adicione o seguinte código para o evento SelectedIndexChanged:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample21.cs)]

1. Procure default.aspx.
2. Definir a lista suspensa **False**.
3. Limpe o log de aplicativo no Visualizador de eventos.
4. Clique no botão para alterar o atributo de depuração para o aplicativo.
5. Atualize o log do aplicativo no Visualizador de eventos. 

    1. Todos os eventos foram registrados?
    2. Por que ou por que não?
6. Definir a lista suspensa **True.**
7. Clique no botão para alterar o atributo de depuração para o aplicativo.
8. Atualize o logon do aplicativo no Visualizador de eventos. 

    1. Todos os eventos foram registrados?
    2. Qual foi o motivo do desligamento do aplicativo?
9. Experimentar a ativar e desativar o registro em log e examinar as alterações feitas no arquivo Web. config.

## <a name="more-information"></a>Mais informações:

O ASP.NET do 2.0 modelo de provedor permite que você crie seus próprios provedores para não apenas a instrumentação de aplicativo, mas muitos outros usos, bem como associação, perfis, etc. Para obter informações detalhadas sobre como escrever um provedor personalizado para o log de eventos do aplicativo para um arquivo de texto, visite [este link](https://msdn.microsoft.com/library/default.asp?url=/library/en-us/dnaspp/html/ASPNETProvMod_Prt6.asp).
