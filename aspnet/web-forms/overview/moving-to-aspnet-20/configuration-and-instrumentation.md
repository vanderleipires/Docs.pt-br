---
uid: web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
title: Configuração e instrumentação | Microsoft Docs
author: microsoft
description: Há grandes alterações na configuração e instrumentação no ASP.NET 2.0. A nova API de configuração do ASP.NET permite que as alterações de configuração sejam feitas de pr...
ms.author: aspnetcontent
ms.date: 02/20/2005
ms.assetid: 21ebbaee-7ed8-45ae-b6c1-c27c88342e48
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
msc.type: authoredcontent
ms.openlocfilehash: 8437b13cae83208983f26e0a5042a5f6c19e516e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822895"
---
<a name="configuration-and-instrumentation"></a>Configuração e instrumentação
====================
por [Microsoft](https://github.com/microsoft)

> Há grandes alterações na configuração e instrumentação no ASP.NET 2.0. A nova API de configuração do ASP.NET permite que as alterações de configuração sejam feitas de forma programática. Além disso, existem a muitas novas definições de configuração Permitir novas configurações e instrumentação.


Há grandes alterações na configuração e instrumentação no ASP.NET 2.0. A nova API de configuração do ASP.NET permite que as alterações de configuração sejam feitas de forma programática. Além disso, existem a muitas novas definições de configuração Permitir novas configurações e instrumentação.

Este módulo, abordaremos a API de configuração do ASP.NET conforme ele se relaciona com lendo e gravando em arquivos de configuração do ASP.NET, e também abordaremos instrumentação ASP.NET. Também abordaremos os novos recursos disponíveis no rastreamento do ASP.NET.

## <a name="aspnet-configuration-api"></a>API de configuração do ASP.NET

A API de configuração do ASP.NET permite que você desenvolver, implantar e gerenciar dados de configuração de aplicativo por meio de uma única interface de programação. Você pode usar a API de configuração para desenvolver e modificar configurações completas do ASP.NET por meio de programação sem editar diretamente o XML nos arquivos de configuração. Além disso, você pode usar a API de configuração em aplicativos de console e scripts que você desenvolve, em ferramentas de gerenciamento baseado na Web e no snap-ins do Microsoft Management Console (MMC).

As duas ferramentas de gerenciamento de configuração a seguir usam a API de configuração e são incluídas com o .NET Framework versão 2.0:

- O snap-in do MMC do ASP.NET, que usa a API de configuração para simplificar tarefas administrativas, fornecendo uma exibição integrada dos dados de configuração local de todos os níveis da hierarquia de configuração.
- A ferramenta de administração de Site da Web, que permite que você gerencie definições de configuração para aplicativos locais e remotos, inclusive sites hospedados.

A API de configuração do ASP.NET inclui um conjunto de objetos de gerenciamento do ASP.NET que você pode usar para configurar sites e aplicativos Web de forma programática. Objetos de gerenciamento são implementados como uma biblioteca de classes do .NET Framework. O modelo de programação da API de configuração ajuda a garantir a consistência de código e confiabilidade por meio da imposição tipos de dados em tempo de compilação. Para tornar mais fácil de gerenciar as configurações do aplicativo, a API de configuração permite que você exiba os dados que são mesclados de todos os pontos na hierarquia de configuração como uma única coleção, em vez de exibir os dados como coleções separadas de diferentes arquivos de configuração. Além disso, a API de configuração permite que você manipule configurações de todo o aplicativo sem editar diretamente o XML nos arquivos de configuração. Por fim, a API simplifica as tarefas de configuração, oferecendo suporte a ferramentas administrativas, como a ferramenta de administração de Site da Web. A API de configuração simplifica a implantação dá suporte à criação de arquivos de configuração em um computador e executando scripts de configuração em vários computadores.

> [!NOTE]
> A API de configuração não suporta a criação de aplicativos do IIS.


## <a name="working-with-local-and-remote-configuration-settings"></a>Trabalhando com definições de configuração Local e remota

Um objeto de configuração representa a exibição mesclada das definições de configuração que se aplicam a uma entidade física específica, como um computador, ou a uma entidade lógica, como um aplicativo ou um site da Web. A entidade lógica especificada pode existir no computador local ou em um servidor remoto. Quando não existe nenhum arquivo de configuração para uma entidade especificada, o objeto de configuração representa as definições de configuração padrão, conforme definido pelo arquivo Machine. config.

Você pode obter um objeto de configuração usando um dos métodos abrir configuração das classes a seguir:

1. A classe ConfigurationManager, se sua entidade é um aplicativo cliente.
2. A classe WebConfigurationManager, se sua entidade é um aplicativo Web.

Esses métodos retornarão um objeto de configuração, que por sua vez, fornece os métodos e propriedades necessários para lidar com os arquivos de configuração subjacentes. Você pode acessar esses arquivos para leitura ou gravação.

### <a name="reading"></a>Leitura

Você pode usar o método GetSection ou GetSectionGroup para ler as informações de configuração. O usuário ou processo que lê deve ter permissões de leitura em todos os arquivos de configuração na hierarquia.

> [!NOTE]
> Se você usar um método GetSection estático que aceita um parâmetro de caminho, o parâmetro path deve se referir ao aplicativo no qual o código está em execução. Caso contrário, o parâmetro será ignorado e as informações de configuração para o aplicativo em execução no momento são retornadas.


### <a name="writing"></a>Gravação

Você pode usar um dos métodos Save para gravar informações de configuração. O usuário ou processo que grava deve ter permissões no arquivo de configuração e diretório no nível da hierarquia de configuração atual de gravação, bem como permissões de leitura em todos os arquivos de configuração na hierarquia.

Para gerar um arquivo de configuração que representa as definições de configuração herdado para uma entidade especificada, use um dos seguintes métodos de salvar configuração:

1. O método Save para criar um novo arquivo de configuração.
2. O método SaveAs para gerar um novo arquivo de configuração em outro local.

## <a name="configuration-classes-and-namespaces"></a>Namespaces e Classes de configuração

Muitas classes de configuração e métodos são semelhantes entre si. A tabela a seguir descreve as classes de configuração mais comumente usadas e namespaces.

| **Namespace ou classe de configuração** | **Descrição** |
| --- | --- |
| [System. Configuration](https://msdn.microsoft.com/library/system.configuration.aspx) namespace | Contém as classes de configuração principal para todos os aplicativos do .NET Framework. Classes de manipulador de seção são usadas para obter dados de configuração para uma seção de métodos, como GetSection e GetSectionGroup. Esses dois métodos são não estático. |
| Classe System.Configuration.Configuration | Representa um conjunto de dados de configuração para um computador, aplicativo, diretório da Web ou outro recurso. Essa classe contém métodos úteis, como GetSection e GetSectionGroup, para atualizar as definições de configuração e obter referências para seções e grupos de seções. Essa classe é usada como um tipo de retorno para métodos que obtêm dados de configuração de tempo de design, como os métodos das classes WebConfigurationManager e ConfigurationManager. |
| Namespace System | Contém as classes de manipulador de seção para as seções de configuração do ASP.NET definidas em [definições de configuração do ASP.NET](https://msdn.microsoft.com/library/b5ysx397.aspx). Classes de manipulador de seção são usadas para obter dados de configuração para uma seção de métodos, como GetSection e GetSectionGroup. |
| Classe System.Web.Configuration.WebConfigurationManager | Fornece métodos úteis para obter referências para definições de configuração de tempo de execução e tempo de design. Esses métodos usam a classe System.Configuration.Configuration como um tipo de retorno. Você pode usar o método GetSection estático dessa classe ou o método GetSection de não-estático da classe ConfigurationManager alternadamente. Para configurações de aplicativos da Web, a classe System.Web.Configuration.WebConfigurationManager é recomendada em vez da classe ConfigurationManager. |
| [System.Configuration.Provider](https://msdn.microsoft.com/library/system.configuration.provider.aspx) namespace | Fornece uma maneira de personalizar e estender o provedor de configuração. Isso é a classe base para todas as classes de provedor no sistema de configuração. |
| [System.Web.Management](https://msdn.microsoft.com/library/system.web.management.aspx) namespace | Contém classes e interfaces para gerenciar e monitorar a integridade dos aplicativos Web. Estritamente falando, esse namespace não é considerado parte da configuração de API. Por exemplo, rastreamento e o acionamento de evento é realizado pelas classes neste namespace. |
| [System.Management.Instrumentation](https://msdn.microsoft.com/library/system.management.instrumentation.aspx) namespace | Fornece as classes necessárias para a instrumentação de aplicativos exponham suas informações de gerenciamento e eventos por meio do Windows Management Instrumentation (WMI) a consumidores em potencial. Monitoramento de integridade do ASP.NET usa o WMI para entregar eventos. Estritamente falando, esse namespace não é considerado parte da configuração de API. |

## <a name="reading-from-aspnet-configuration-files"></a>Leitura de arquivos de configuração do ASP.NET

A classe WebConfigurationManager é a classe principal para leitura de arquivos de configuração do ASP.NET. Há basicamente três etapas para ler arquivos de configuração do ASP.NET:

1. Obtenha um objeto de configuração usando o método OpenWebConfiguration.
2. Obtenha uma referência para a seção desejada no arquivo de configuração.
3. Leia as informações desejadas do arquivo de configuração.

A configuração do objeto representa não representa um arquivo de configuração específico. Em vez disso, ele representa uma exibição mesclada da configuração de um computador, aplicativo ou site da Web. O exemplo de código a seguir cria uma instância de um objeto de configuração que representa a configuração de um aplicativo Web chamado *ProductInfo*.

[!code-csharp[Main](configuration-and-instrumentation/samples/sample1.cs)]

> [!NOTE]
> Observe que, se o caminho /ProductInfo não existir, o código acima retornará a configuração padrão, conforme especificado no arquivo Machine. config.


Depois que o objeto de configuração, você pode, em seguida, usar o método GetSection ou GetSectionGroup para detalhar as definições de configuração. O exemplo a seguir obtém uma referência para as configurações de representação para o aplicativo ProductInfo acima:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample2.cs)]

## <a name="writing-to-aspnet-configuration-files"></a>Gravando em arquivos de configuração do ASP.NET

Como na leitura de arquivos de configuração, a classe WebConfigurationManager é a principal para gravar em arquivos de configuração do Asp.NET. Também há três etapas para gravar em arquivos de configuração do ASP.NET.

1. Obtenha um objeto de configuração usando o método OpenWebConfiguration.
2. Obtenha uma referência para a seção desejada no arquivo de configuração.
3. Gravar as informações desejadas do arquivo de configuração usando a salvar ou salvar como método.

Alterações de código a seguir a **debug** atributo da &lt;compilação&gt; elemento como false:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample3.cs)]

Quando esse código é executado, o **debug** atributo da &lt;compilação&gt; elemento será definido como false para o *webApp* arquivo da Web. config do aplicativo.

## <a name="systemwebmanagement-namespace"></a>Namespace System.Web.Management

O namespace de System.Web.Management fornece as classes e interfaces para gerenciar e monitorar a integridade dos aplicativos ASP.NET.

Registro em log é feito definindo uma regra que associa a um provedor de eventos. A regra define o tipo de eventos que são enviados para o provedor. Os seguintes eventos base estão disponíveis para você fazer logon:

| **WebBaseEvent** | A classe de evento de base para todos os eventos. Contém as propriedades para todos os eventos, como o código de evento, código de detalhe do evento, data e hora que o evento foi acionado, detalhes do evento, a mensagem de evento e número de sequência. |
| --- | --- |
| **WebManagementEvent** | A classe de evento de base para eventos de gerenciamento, como tempo de vida do aplicativo, solicitação, erros e eventos de auditoria. |
| **WebHeartbeatEvent** | O evento gerado pelo aplicativo em intervalos regulares para capturar informações de estado de tempo de execução útil. |
| **WebAuditEvent** | A classe base para eventos de auditoria de segurança, que são usadas para marcar as condições, como falha de autorização, falha de descriptografia, *etc.* |
| **WebRequestEvent** | A classe base para todos os eventos de solicitação informativos. |
| **WebBaseErrorEvent** | A classe base para todos os eventos que indica as condições de erro. |

Os tipos de provedores disponíveis permitem que você enviar a saída de evento para o Visualizador de eventos, o SQL Server, o Windows Management Instrumentation (WMI) e o email. Os mapeamentos de eventos e provedores pré-configurados reduzem a quantidade de trabalho necessário para obter a saída de evento registrada em log.

O ASP.NET 2.0 usa o Log de eventos provedor out-of-the-box para registrar eventos com base em domínios de aplicativo, iniciando e parando, bem como registro em log qualquer exceção sem tratamento. Isso ajuda a abordar alguns dos cenários básicos. Por exemplo, digamos que seu aplicativo gera uma exceção, mas o usuário não salva o erro e não é possível reproduzi-lo. Com a regra de Log de eventos padrão, você poderá coletar as informações de exceção e a pilha para obter uma ideia melhor de que tipo de erro ocorreu. Aplica-se outro exemplo, se seu aplicativo está perdendo o estado de sessão. Nesse caso, você pode examinar o Log de eventos para determinar se a reciclagem de domínio do aplicativo, e por que o domínio do aplicativo parou em primeiro lugar.

Além disso, o sistema de monitoramento de integridade é extensível. Por exemplo, você pode definir eventos personalizados da Web, acioná-los dentro de seu aplicativo e, em seguida, definir uma regra para enviar as informações de evento para um provedor, como seu email. Isso permite que você ligue facilmente sua instrumentação para provedores de monitoramento de integridade. Como outro exemplo, você pode acionar um evento sempre que um pedido é processado e configurar uma regra que envia cada evento no banco de dados do SQL Server. Você também pode disparar um evento quando um usuário não conseguir fazer logon várias vezes em uma linha e configurar o evento para usar os provedores de email.

A configuração para os eventos e os provedores padrão é armazenada no arquivo Web. config global. O arquivo Web. config global armazena todas as baseado na Web configurações que foram armazenadas no arquivo Machine. config no ASP.NET 1 x. O arquivo Web. config global está localizado no seguinte diretório:

`%windir%\Microsoft.Net\Framework\v2.0.*\config\Web.config`

O &lt;healthMonitoring&gt; seção do arquivo Web. config global fornece definições de configuração de padrão. Você pode substituir essas configurações ou configurar suas próprias configurações implementando a &lt;healthMonitoring&gt; seção no arquivo Web. config para seu aplicativo.

O &lt;healthMonitoring&gt; seção do arquivo Web. config global contém os seguintes itens:

| **provedores** | Contém os provedores de configurar para o Visualizador de eventos, o WMI e o SQL Server. |
| --- | --- |
| **eventMappings** | Contém mapeamentos para as várias classes de WebBase. Você pode estender essa lista se você gerar sua própria classe de evento. Gerar sua própria classe de evento oferece granularidade mais fina sobre os provedores que você enviar informações para. Por exemplo, você poderia configurar exceções sem tratamento a ser enviada para o SQL Server, ao enviar seus próprios eventos personalizados para o email. |
| **regras** | Links eventMappings ao provedor. |
| **armazenamento em buffer** | Usado com provedores SQL Server e o email para determinar a frequência de liberar os eventos para o provedor. |

Abaixo está um exemplo de código do arquivo Web. config global.

[!code-xml[Main](configuration-and-instrumentation/samples/sample4.xml)]

## <a name="how-to-store-events-to-event-viewer"></a>Como armazenar eventos no Visualizador de eventos

Como mencionado anteriormente, o provedor de eventos de log de eventos Visualizador é configurado por você no arquivo Web. config global. Por padrão, todos os eventos com base em **WebBaseErrorEvent** e **WebFailureAuditEvent** são registradas. Você pode adicionar regras adicionais para registrar informações adicionais no Log de eventos. Por exemplo, se você quisesse registrar todos os eventos (*ou seja,*, todos os eventos com base em **WebBaseEvent**), você pode adicionar a regra a seguir ao seu arquivo Web. config:

[!code-xml[Main](configuration-and-instrumentation/samples/sample5.xml)]

Essa regra faria um vínculo a **todos os eventos** mapa de evento para o provedor de Log de eventos. EventMapping faz e o provedor estão incluídos no arquivo Web. config global.

## <a name="how-to-store-events-to-sql-server"></a>Como armazenar os eventos para o SQL Server

Esse método usa o **ASPNETDB** banco de dados, que é gerado pelo Aspnet\_regsql.exe ferramenta. O provedor padrão usa a cadeia de conexão do LocalSqlServer, que usa um arquivo com base no banco de dados no aplicativo\_pasta de dados ou instância SQLExpress local do SQL Server. A cadeia de caracteres de conexão do LocalSqlServer e o SqlProvider são configurados no arquivo Web. config global.

A cadeia de caracteres de conexão LocalSqlServer no arquivo Web. config global tem esta aparência:

[!code-xml[Main](configuration-and-instrumentation/samples/sample6.xml)]

Se você quiser usar outra instância do SQL Server, você precisará usar o Aspnet\_regsql.exe ferramenta, que pode ser encontrada em % windir%\Microsoft.Net\Framework\v2.0.\* pasta. Use o Aspnet\_regsql.exe ferramenta para gerar um personalizado **ASPNETDB** de banco de dados na instância do SQL Server, em seguida, adicione a cadeia de conexão para seu arquivo de configuração de aplicativos e, em seguida, adicionar um provedor usando o novo cadeia de caracteres de conexão. Uma vez que o **ASPNETDB** banco de dados criado, você precisará definir uma regra para vincular um EventMapping faz para o sqlProvider.

Se você usa o padrão SqlProvider ou configurar seu próprio provedor, você precisará adicionar uma regra que o provedor com um mapa de eventos de vinculação. A regra a seguir vincula o novo provedor que você criou acima para o **todos os eventos** mapa de evento. Essa regra registrará em log todos os eventos com base em **WebBaseEvent** e enviá-los para o MySqlWebEventProvider que usará a cadeia de caracteres de conexão MYASPNETDB. O código a seguir adiciona uma regra para vincular o provedor com um mapa de eventos:

[!code-xml[Main](configuration-and-instrumentation/samples/sample7.xml)]

Se você quisesse somente enviar erros para o SQL Server, você pode adicionar a regra a seguir:

[!code-xml[Main](configuration-and-instrumentation/samples/sample8.xml)]

## <a name="how-to-forward-events-to-wmi"></a>Como o encaminhamento de eventos WMI

Você também pode encaminhar os eventos WMI. O provedor WMI é configurado por você no arquivo Web. config global por padrão.

O exemplo de código a seguir adiciona uma regra para encaminhar os eventos de WMI:

[!code-xml[Main](configuration-and-instrumentation/samples/sample9.xml)]

Você precisará adicionar uma regra para associar um EventMapping faz para o provedor e um aplicativo de escuta do WMI para escutar eventos. O exemplo de código a seguir adiciona uma regra para vincular o provedor WMI para o **todos os eventos** mapa de evento:

[!code-xml[Main](configuration-and-instrumentation/samples/sample10.xml)]

## <a name="how-to-forward-events-to-email"></a>Como encaminhar eventos para email

Você também pode encaminhar eventos para email. Tenha cuidado sobre quais regras de evento que você mapear para seu provedor de email, como você pode, inadvertidamente, enviar por conta própria muitas informações que podem ser mais adequado para o SQL Server ou o Log de eventos. Há dois provedores de email; SimpleMailWebEventProvider e TemplatedMailWebEventProvider. Cada um tem os mesmos atributos de configuração, com exceção de atributos "template" e "detailedTemplateErrors", que só estão disponíveis no TemplatedMailWebEventProvider.

> [!NOTE]
> Nenhum desses provedores de email é configurado por você. Você precisará adicioná-los ao seu arquivo Web. config.


A principal diferença entre esses provedores de duas email é que SimpleMailWebEventProvider envia emails em um modelo genérico que não pode ser modificado. O arquivo Web. config de exemplo adiciona esse provedor de email à lista de provedores configurados usando a seguinte regra:

[!code-xml[Main](configuration-and-instrumentation/samples/sample11.xml)]

A regra a seguir também é adicionada para ligar o provedor de email para o **todos os eventos** mapa de evento:

[!code-xml[Main](configuration-and-instrumentation/samples/sample12.xml)]

## <a name="aspnet-20-tracing"></a>O ASP.NET 2.0 rastreamento

Há três principais aprimoramentos para o rastreamento do ASP.NET 2.0.

1. Funcionalidade de rastreamento integrado
2. Acesso programático a mensagens de rastreamento
3. Rastreamento de aplicativo aprimorado

## <a name="integrated-tracing-functionality"></a>Integrado a funcionalidade de rastreamento

Agora você pode rotear mensagens emitidas pela classe Trace para rastreamento de saída do ASP.NET e rotear mensagens emitidas pelo rastreamento do ASP.NET para Trace. Você também pode encaminhar eventos de instrumentação do ASP.NET para Trace. Essa funcionalidade é fornecida pelo novo **writeToDiagnosticsTrace** atributo da &lt;rastreamento&gt; elemento. Quando esse valor booliano for verdadeiro, as mensagens de rastreamento do ASP.NET são encaminhadas para a infra-estrutura de rastreamento de System. Diagnostics para uso por todos os ouvintes que estão registrados para exibir mensagens de rastreamento.

## <a name="programmatic-access-to-trace-messages"></a>Acesso programático às mensagens de rastreamento

O ASP.NET 2.0 permite acesso programático a todas as mensagens de rastreamento por meio de **TraceContextRecord** classe e o **TraceRecords** coleção. A maneira mais eficiente de acessar as mensagens de rastreamento é registrar um **TraceContextEventHandler** delegado (também novo no ASP.NET 2.0) para lidar com a nova **TraceFinished** eventos. Você pode, em seguida, executar um loop por meio de mensagens de rastreamento como desejar.

O exemplo de código a seguir ilustra isso:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample13.cs)]

No exemplo acima, posso executar um loop pela coleção TraceRecords e, em seguida, gravar cada mensagem no fluxo de resposta.

## <a name="improved-application-level-tracing"></a>Rastreamento de aplicativo aprimorado

Rastreamento de aplicativo é aprimorado por meio da introdução do novo **mostRecent** atributo da &lt;rastreamento&gt; elemento. Esse atributo especifica se a saída de rastreamento de aplicativo mais recente é exibida e os dados antigos de rastreamento além dos limites que são indicados pelo requestLimit são descartados. Se for falso, dados de rastreamento são exibidos para solicitações até que o atributo requestLimit seja atingido.

## <a name="aspnet-command-line-tools"></a>Ferramentas de linha de comando do ASP.NET

Há várias ferramentas de linha de comando para ajudar na configuração do ASP.NET. Os desenvolvedores de ASP.NET devem estar familiarizados com o aspnet\_regiis.exe ferramenta. O ASP.NET 2.0 oferece três outras ferramentas de linha de comando para ajudar na configuração.

As ferramentas de linha de comando a seguir estão disponíveis:

| **Ferramenta** | **Use** |
| --- | --- |
| **aspnet\_regiis.exe** | Permite que o registro do ASP.NET com o IIS. Há duas versões dessa ferramentas que acompanham o ASP.NET 2.0, um para sistemas de 32 bits (na pasta de Framework) e outra para sistemas de 64 bits (na pasta Framework64.) A versão de 64 bits não será instalada em um sistema operacional de 32 bits. |
| **aspnet\_regsql.exe** | A ferramenta de registro do SQL Server do ASP.NET é usada para criar um banco de dados do Microsoft SQL Server para uso pelos provedores do SQL Server no ASP.NET, ou para adicionar ou remover opções de banco de dados existente. O Aspnet\_regsql.exe arquivo está localizado na [drive:]\WINDOWS\Microsoft.NET\Framework\versionNumber pasta em seu servidor Web. |
| **aspnet\_regbrowsers.exe** | A ferramenta de registro do navegador ASP.NET analisa e compila todas as definições do navegador de todo o sistema em um assembly e instala o assembly no cache de assembly global. A ferramenta usa os arquivos de definição do navegador (. Arquivos do navegador) do subdiretório navegadores do .NET Framework. A ferramenta pode ser encontrada no diretório %SystemRoot%\Microsoft.NET\Framework\version\. |
| **aspnet\_compiler.exe** | A ferramenta de compilação do ASP.NET permite que você compilar um aplicativo Web ASP.NET, no local ou para implantação em um local de destino como um servidor de produção. Compilação no local ajuda no desempenho do aplicativo porque os usuários finais não encontrar um atraso na primeira solicitação para o aplicativo enquanto o aplicativo é compilado. |

Porque o aspnet\_regiis.exe ferramenta não é nova no ASP.NET 2.0, não discutiremos esse assunto aqui.

## <a name="aspnet-sql-server-registration-tool---aspnetregsqlexe"></a>Ferramenta de registro do servidor SQL do ASP.NET - aspnet\_regsql.exe

Você pode definir vários tipos de opções usando a ferramenta de registro do SQL Server do ASP.NET. Você pode especificar uma conexão de SQL, especifique quais serviços de aplicativo do ASP.NET usam o SQL Server para gerenciar as informações, indicar qual banco de dados ou tabela é usada para a dependência de cache SQL e adicionar ou remover suporte para o uso do SQL Server para armazenar o estado de sessão e procedimentos.

Vários serviços de aplicativos do ASP.NET dependem de um provedor para gerenciar, armazenar e recuperar dados de uma fonte de dados. Cada provedor é específico para a fonte de dados. O ASP.NET inclui um provedor do SQL Server para os seguintes recursos do ASP.NET:

- Associação (o [SqlMembershipProvider](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx) classe).
- Gerenciamento de função (a [SqlRoleProvider](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx) classe).
- Perfil (o [SqlProfileProvider](https://msdn.microsoft.com/library/system.web.profile.sqlprofileprovider.aspx) classe).
- Personalização de Web Parts (a [SqlPersonalizationProvider](https://msdn.microsoft.com/library/system.web.ui.webcontrols.webparts.sqlpersonalizationprovider.aspx) classe).
- Eventos da Web (o [SqlWebEventProvider](https://msdn.microsoft.com/library/system.web.management.sqlwebeventprovider.aspx) classe).

Quando você instala o ASP.NET, o arquivo Machine. config para seu servidor inclui elementos de configuração que especificam os provedores SQL Server para cada um dos recursos do ASP.NET que contam com um provedor. Esses provedores são configurados, por padrão, para se conectar a uma instância de usuário local do SQL Server 2005 Express. Se você alterar a cadeia de conexão padrão usada pelos provedores, em seguida, antes de usar qualquer um dos recursos do ASP.NET configurados na configuração da máquina, você deve instalar o banco de dados do SQL Server e os elementos de banco de dados para o recurso escolhido usando Aspnet\_regsql.exe. Se o banco de dados que você especificar com a ferramenta de registro SQL ainda não existir (aspnetdb será o banco de dados padrão se nenhum for especificado na linha de comando), em seguida, o usuário atual deve ter direitos para criar bancos de dados no SQL Server, bem como criar e de esquema lements dentro de um banco de dados.

### <a name="sql-cache-dependency"></a>SQL Cache Dependency

Um recurso avançado de cache de saída ASP.NET é a dependência de cache SQL. Dependência de cache SQL dá suporte a dois modos diferentes de operação: um que usa uma implementação do ASP.NET de sondagem de tabela e um segundo de modo que usa os recursos de notificação de consulta do SQL Server 2005. A ferramenta de registro do SQL pode ser usada para configurar o modo de tabela de sondagem da operação.

### <a name="session-state"></a>Estado de sessão

Por padrão, informações e valores de estado de sessão são armazenados na memória no processo do ASP.NET. Como alternativa, você pode armazenar dados de sessão em um banco de dados do SQL Server, onde ele pode ser compartilhado por vários servidores Web. Se o banco de dados que você especificar para o estado de sessão com a ferramenta de registro SQL ainda não existir, o usuário atual deve ter direitos para criar bancos de dados no SQL Server, bem como para criar elementos de esquema em um banco de dados. Se o banco de dados existir, o usuário atual deve ter direitos para criar elementos de esquema no banco de dados existente.

Para instalar o banco de dados de estado de sessão no SQL Server, execute o Aspnet\_regsql.exe ferramenta e forneça as seguintes informações com o comando:

- O nome do SQL Server da instância, usando o **-S** opção.
- As credenciais de logon para uma conta que tenha permissão para criar um banco de dados em um computador executando o SQL Server. Use o **-E** opção de usar o usuário conectado no momento, ou usar o **- U** opção para especificar uma ID de usuário juntamente com o **-P** opção para especificar uma senha.
- O **- ssadd** opção de linha de comando para adicionar o banco de dados de estado de sessão.

Por padrão, não é possível usar o Aspnet\_regsql.exe ferramenta para instalar o banco de dados de estado de sessão em um computador executando o SQL Server 2005 Express Edition.

### <a name="the-aspnet-browser-registration-tool---aspnetregbrowsersexe"></a>A ferramenta de registro do navegador ASP.NET - aspnet\_regbrowsers.exe

No ASP.NET versão 1.1, o arquivo Machine. config continha uma seção chamada &lt;browserCaps&gt;. Esta seção continha uma série de entradas XML que definiu as configurações para vários navegadores com base em uma expressão regular. Para o ASP.NET versão 2.0, um novo. Arquivo de navegador define os parâmetros de um navegador específico usando as entradas XML. Você pode adicionar informações em um novo navegador adicionando um novo. Arquivo de navegador para a pasta localizada em %SystemRoot%\Microsoft.NET\Framework\version\CONFIG\Browsers em seu sistema.

Como um aplicativo não está lendo um arquivo. config sempre que ele requer informações do navegador, você pode criar um novo. Arquivo de navegador e execução Aspnet\_regbrowsers.exe para adicionar as alterações necessárias ao assembly. Isso permite que o servidor acessar as informações do novo navegador imediatamente para que você não precise desligar a qualquer um dos seus aplicativos para obter as informações. Um aplicativo pode acessar os recursos do navegador por meio da propriedade do navegador de HttpRequest atual.

As seguintes opções estão disponíveis durante a execução aspnet\_regbrowser.exe:

| **Opção** | **Descrição** |
| --- | --- |
| **-?** | Exibe o Aspnet\_regbbrowsers.exe texto de ajuda na janela de comando. |
| **-i** | Cria o assembly de recursos do navegador de tempo de execução e o instala no cache de assembly global. |
| **-u** | Desinstala o assembly de recursos do navegador de tempo de execução do cache de assembly global. |

## <a name="the-aspnet-compilation-tool---aspnetcompilerexe"></a>A ferramenta de compilação do ASP.NET - aspnet\_compiler.exe

A ferramenta de compilação do ASP.NET pode ser usada de duas maneiras gerais: para compilação no local e a compilação para implantação, em que um diretório de saída de destino é especificado.

### <a name="compiling-an-application-in-placehttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[Compilar um aplicativo no local](https://msdn.microsoft.com/library/ms229863.aspx)

A ferramenta de compilação do ASP.NET pode compilar um aplicativo no local, ou seja, ele simula o comportamento de fazer várias solicitações para o aplicativo, fazendo com que a compilação regular. Os usuários de um site pré-compilado não haverá um atraso causado por compilar a página na primeira solicitação.

Quando você pré-compila um site em vigor, os itens a seguir se aplicam:

- O site mantém seus arquivos e a estrutura de diretório.
- Você deve ter os compiladores para todas as linguagens de programação usadas pelo site no servidor.
- Se qualquer arquivo falhar compilação, todo o site na compilação.

Você também pode recompilar um aplicativo em vigor após a adição de novos arquivos de origem para ele. A ferramenta compila somente os arquivos novos ou alterados, a menos que você inclua o **- c** opção.

> [!NOTE]
> Compilação de um aplicativo que contém um aplicativo aninhado não compilar o aplicativo de aninhados. O aplicativo aninhado deve ser compilado separadamente.


### <a name="compiling-an-application-for-deploymenthttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[Compilando um aplicativo para implantação](https://msdn.microsoft.com/library/ms229863.aspx)

Você pode compilar um aplicativo para implantação (compilação para um local de destino), especificando o parâmetro targetDir. O targetDir pode ser o local final para o aplicativo Web ou aplicativo compilado pode ser implantado. Usando o **-u** opção compila o aplicativo de tal forma que você pode fazer alterações a certos arquivos no aplicativo compilado sem recompilá-lo. ASPNET\_compiler.exe faz uma distinção entre tipos de arquivos estáticos e dinâmicos e trata-los de maneira diferente ao criar o aplicativo resultante.

- Tipos de arquivo estático são aqueles que não têm um compilador associado ou criar um provedor, como arquivos cuja nomeado têm extensões como. CSS,. gif,. htm,. HTML,. jpg,. js e assim por diante. Esses arquivos simplesmente são copiados para o local de destino, com suas casas relativas na estrutura de diretórios preservados.
- Tipos de arquivo dinâmico são aqueles que têm um compilador associado ou criar um provedor, incluindo arquivos com extensões de nome de arquivo específico do ASP.NET, como. asax,. ascx,. ashx,. aspx,. browser,. master e assim por diante. A ferramenta de compilação do ASP.NET gera assemblies desses arquivos. Se o **-u** opção for omitida, a ferramenta também cria arquivos com a extensão de nome de arquivo. COMPILED que mapeiam os arquivos de origem para seu assembly. Para garantir que a estrutura do diretório de origem do aplicativo seja preservada, a ferramenta gera arquivos de espaço reservado nos locais correspondentes no aplicativo de destino.

Você deve usar o **-u** opção para indicar que o conteúdo do aplicativo compilado pode ser modificado. Caso contrário, as modificações subsequentes serão ignoradas ou causam erros de tempo de execução.

A tabela a seguir descreve como o compilação do ASP.NET ferramenta lida com diferentes tipos de arquivos quando o **-u** opção é incluída.

| **Tipo de arquivo** | **Ação de compilador** |
| --- | --- |
| .ascx, .aspx, .master | Esses arquivos são divididos em marcação e código-fonte, que inclui arquivos code-behind e qualquer código que é colocado entre &lt;script runat = "server"&gt; elementos. Código-fonte é compilado em assemblies com nomes que são derivados de um algoritmo de hash e os assemblies são colocados no diretório Bin. Qualquer código embutido, ou seja, código colocado entre o **&lt; %** e **% &gt;** colchetes, está incluído com a marcação e não compilados. Novos arquivos com o mesmo nome que os arquivos de origem são criados para conter a marcação e colocados nos diretórios de saída correspondente. |
| .ashx, .asmx | Esses arquivos não são compilados e são movidos para os diretórios de saída como está e não compilados. Se você quiser ter o código de manipulador compilado, coloque o código em arquivos de código fonte no aplicativo\_diretório de código. |
| . cs,. vb,. jsl,. cpp (não incluindo os arquivos code-behind para os tipos de arquivo listados anteriormente) | Esses arquivos são compilados e incluídos como um recurso em assemblies que faça referência a eles. Arquivos de origem não são copiados para o diretório de saída. Se um arquivo de código não é referenciado, ele não é compilado. |
| Tipos de arquivo personalizados | Esses arquivos não são compilados. Esses arquivos são copiados para os diretórios de saída correspondentes. |
| Arquivos de código no aplicativo de origem\_subdiretório de código | Esses arquivos são compilados em assemblies e colocados no diretório Bin. |
| arquivos. resx e. Resource no aplicativo\_GlobalResources subdiretório | Esses arquivos são compilados em assemblies e colocados no diretório Bin. Nenhum aplicativo\_GlobalResources subdiretório é criado sob o diretório de saída principal, e nenhum arquivo. resx ou. Resources localizado no diretório de origem é copiado para os diretórios de saída. |
| arquivos. resx e. Resource no aplicativo\_LocalResources subdiretório | Esses arquivos não são compilados e são copiados para os diretórios de saída correspondentes. |
| arquivos. skin no aplicativo\_subdiretório de temas | Os arquivos. skin e tema estáticos não são compilados e são copiados para os diretórios de saída correspondentes. |
| Assemblies já está presentes no diretório Bin de tipos. browser arquivos estáticos Web. config | Esses arquivos são copiados como está para os diretórios de saída. |

A tabela a seguir descreve como o compilação do ASP.NET ferramenta lida com diferentes tipos de arquivos quando o **-u** opção for omitida.

| **Tipo de arquivo** | **Ação de compilador** |
| --- | --- |
| .aspx, .asmx, .ashx, .master | Esses arquivos são divididos em marcação e código-fonte, que inclui arquivos code-behind e qualquer código que é colocado entre &lt;script runat = "server"&gt; elementos. Código-fonte é compilado em assemblies com nomes que são derivados de um algoritmo de hash. Os assemblies resultantes são colocados no diretório Bin. Qualquer código embutido, ou seja, código colocado entre o **&lt; %** e **% &gt;** colchetes, está incluído com a marcação e não compilados. O compilador cria novos arquivos para conter a marcação com o mesmo nome que os arquivos de origem. Esses arquivos resultantes são colocados no diretório Bin. O compilador também cria arquivos com o mesmo nome que os arquivos de origem, mas com a extensão. COMPILED que contenham informações de mapeamento. A. Arquivos COMPILADOS são colocados nos diretórios de saída correspondente para o local original dos arquivos de origem. |
| .ascx | Esses arquivos são divididos em marcação e código-fonte. Código-fonte é compilado em assemblies e colocado no diretório Bin, com nomes que são derivados de um algoritmo de hash. Nenhum arquivo de marcação é gerado. |
| . cs,. vb,. jsl,. cpp (não incluindo os arquivos code-behind para os tipos de arquivo listados anteriormente) | Código-fonte que é referenciado pelos assemblies gerados a partir de arquivos. aspx,. ashx ou. ascx é compilado em assemblies e colocado no diretório Bin. Nenhum arquivo de origem é copiado. |
| Tipos de arquivo personalizados | Esses arquivos são compilados como arquivos dinâmicos. Dependendo do tipo de arquivo, que eles se baseiam no, o compilador pode colocar arquivos de mapeamento nos diretórios de saída. |
| Arquivos no aplicativo\_subdiretório de código | Arquivos de código fonte desse subdiretório são compilados em assemblies e colocados no diretório Bin. |
| Arquivos no aplicativo\_GlobalResources subdiretório | Esses arquivos são compilados em assemblies e colocados no diretório Bin. Nenhum aplicativo\_GlobalResources subdiretório é criado sob o diretório de saída principal. Se o arquivo de configuração especifica appliesTo = "All", arquivos. resx e. resources são copiados para os diretórios de saída. Eles não são copiados se eles são referenciados por uma [BuildProvider](https://msdn.microsoft.com/library/system.web.configuration.buildprovider.aspx). |
| arquivos. resx e. Resource no aplicativo\_LocalResources subdiretório | Esses arquivos são compilados em assemblies com nomes exclusivos e colocados no diretório Bin. Não há arquivos. resx ou. Resource são copiados para os diretórios de saída. |
| arquivos. skin no aplicativo\_subdiretório de temas | Temas são compilados em assemblies e colocados no diretório Bin. Arquivos stub são criados para arquivos. skin e colocados no diretório de saída correspondente. Arquivos estáticos (por exemplo,. CSS) são copiados para os diretórios de saída. |
| Assemblies já está presentes no diretório Bin de tipos. browser arquivos estáticos Web. config | Esses arquivos são copiados como é o diretório de saída. |

### <a name="fixed-assembly-nameshttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[Nomes de Assembly corrigido](https://msdn.microsoft.com/library/ms229863.aspx##)

Alguns cenários, como a implantação de um aplicativo Web usando o instalador MSI do Windows, exigem o uso de nomes de arquivo consistente e conteúdo, bem como estruturas de diretório consistente para identificar os assemblies ou definições de configuração para atualizações. Nesses casos, você pode usar o **- fixednames** opção para especificar que a ferramenta de compilação do ASP.NET deve compilar um assembly para cada arquivo de origem em vez de usar where várias páginas são compiladas em assemblies. Isso pode levar a um grande número de assemblies, portanto, se você estiver preocupado com escalabilidade devem usar essa opção com cuidado.

### <a name="strong-name-compilationhttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[Compilação de nome forte](https://msdn.microsoft.com/library/ms229863.aspx##)

O **- aptca**, **- delaysign**, **- keycontainer** e **- keyfile** opções são fornecidas para que você possa usar Aspnet\_ Compiler.exe Criar fortemente nomeado assemblies sem usar o [ferramenta de nome forte (Sn.exe)](https://msdn.microsoft.com/library/k5b5tt23.aspx) separadamente. Essas opções correspondem, respectivamente, para **AllowPartiallyTrustedCallersAttribute**, **AssemblyDelaySignAttribute**, **AssemblyKeyNameAttribute**e  **AssemblyKeyFileAttribute**.

Discussão desses atributos está fora do escopo deste curso.

## <a name="labs"></a>Laboratórios

Cada um dos seguintes laboratórios amplia os laboratórios anteriores. Você precisará fazê-las na ordem.

## <a name="lab-1-using-the-configuration-api"></a>Laboratório 1: Usando a API de configuração

1. Criar um novo site denominado *mod9lab*.
2. Adicione um novo arquivo de configuração da Web para o site.
3. Adicione o seguinte ao arquivo Web. config:


[!code-xml[Main](configuration-and-instrumentation/samples/sample14.xml)]

Isso garantirá que você tenha permissão para salvar as alterações no arquivo Web. config.

1. Adicionar um novo controle de rótulo para default. aspx e alterar a ID a ser **lblDebugStatus**.
2. Adicione um novo controle de botão para default. aspx.
3. Alterar a ID do controle Button para **btnToggleDebug** e o texto a ser **ativar/desativar Status de depuração**.
4. Abra o modo de exibição de código para o arquivo code-behind de Default. aspx e adicione uma **usando** instrução **System** da seguinte maneira:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample15.cs)]

1. Adicione duas variáveis particulares para uma página e a classe\_método Init, conforme mostrado abaixo:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample16.cs)]

1. Adicione o seguinte código à página\_carga:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample17.cs)]

1. Salve e procurar o default. aspx. Observe que o controle de rótulo exibe o status atual de depuração.
2. Clique duas vezes no controle de botão no designer e adicione o seguinte código ao evento de clique do controle de botão:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample18.cs)]

1. Salvar e default. aspx e clique no botão.
2. Abra o arquivo Web. config após cada botão Clique e observar o **debug** atributo na &lt;compilação&gt; seção.

## <a name="lab-2-logging-application-restarts"></a>Laboratório 2: Registro em log as reinicializações de aplicativo

Neste laboratório, você criará um código que permitirá que você alterne o registro em log de recompilações no Visualizador de eventos, startups e desligamentos de aplicativo.

1. Adicione uma DropDownList para default. aspx e mude o ID para ddlLogAppEvents.
2. Definir a **AutoPostBack** propriedade para a DropDownList **true**.
3. Adicione três itens à coleção de itens para DropDownList. Verifique as **texto** para o primeiro item *Select Value* e o valor -1. Verifique as **texto** e **valor** do segundo item **verdadeiro** e o **texto** e **valor** do terceiro item **Falsos**.
4. Adicione um novo rótulo para default. aspx. Alterar a ID para **lblLogAppEvents**.
5. Abra o modo de exibição de lógica para default. aspx e adicionar uma nova declaração de uma variável do tipo HealthMonitoringSection, conforme mostrado abaixo:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample19.cs)]

1. Adicione o seguinte código para o código existente no página\_Init:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample20.cs)]

1. Clique duas vezes na DropDownList e adicione o seguinte código ao evento SelectedIndexChanged:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample21.cs)]

1. Procure default. aspx.
2. Defina a lista suspensa **falsos**.
3. Limpe o log de aplicativo no Visualizador de eventos.
4. Clique no botão para alterar o atributo de depuração para o aplicativo.
5. Atualize o log de aplicativo no Visualizador de eventos. 

    1. Todos os eventos foram registrados?
    2. Por que ou não?
6. Defina a lista suspensa **True.**
7. Clique no botão para alterar o atributo de depuração para o aplicativo.
8. Atualize o Visualizador de eventos de logon do aplicativo. 

    1. Todos os eventos foram registrados?
    2. Qual foi o motivo do desligamento do aplicativo?
9. Experimentar a ativar e desativar o registro em log e examine as alterações feitas no arquivo Web. config.

## <a name="more-information"></a>Obter mais informações:

O ASP.NET do 2.0 modelo de provedor permite que você crie seus próprios provedores para instrumentação de aplicativos não apenas, mas para muitos outros usos, bem como associação, perfis, etc. Para obter informações detalhadas sobre como escrever um provedor personalizado para o log de eventos do aplicativo em um arquivo de texto, visite [esse link](https://msdn.microsoft.com/library/default.asp?url=/library/dnaspp/html/ASPNETProvMod_Prt6.asp).
