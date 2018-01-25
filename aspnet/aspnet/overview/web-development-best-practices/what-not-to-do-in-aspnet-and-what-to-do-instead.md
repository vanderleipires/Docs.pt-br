---
uid: aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
title: "O que não fazer em ASP.NET e o que fazer em vez disso | Microsoft Docs"
author: tfitzmac
description: "Este tópico descreve os erros comuns de várias pessoas fazer dentro de projetos web ASP.NET. Ele fornece recomendações para que você deve fazer para evitar essas comu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/08/2014
ms.topic: article
ms.assetid: c39b9965-545c-4b04-8f55-21be7f28a9e5
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
msc.type: authoredcontent
ms.openlocfilehash: 829f3a024bc15bec8b60b91193ba9bca37b78009
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="what-not-to-do-in-aspnet-and-what-to-do-instead"></a>O que não fazer em ASP.NET e o que fazer em vez disso
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tópico descreve os erros comuns de várias pessoas fazer dentro de projetos web ASP.NET. Ele fornece recomendações para que você deve fazer para evitar esses erros comuns. Ela se baseia em uma [apresentação](http://vimeo.com/68390507) por **Damian Edwards** em norueguês conferência de desenvolvedores.


## <a name="disclaimer"></a>Aviso de isenção de responsabilidade

Este tópico não serve como um guia completo para garantir que seu aplicativo é seguro e eficiente. Você ainda precisa seguir as práticas recomendadas para segurança e desempenho que não são descritas neste tópico. Apenas sugere como evitar erros comuns relacionados a processos e classes do .NET.

## <a name="overview"></a>Visão geral

Esse tópico contém as seguintes seções:

- [Conformidade com padrões](#standards)

    - [Adaptadores de controle](#adapters)
    - [Propriedades de estilo para controles](#styleprop)
    - [Página e retornos de chamada de controle](#callback)
    - [Detecção de recursos do navegador](#browsercap)
- [Segurança](#security)

    - [Validação de solicitação](#validation)
    - [Sessão e autenticação de formulários cookieless](#cookieless)
    - [EnableViewStateMac](#viewstatemac)
    - [Confiança média](#medium)
    - [&lt;appSettings&gt;](#appsettings)
    - [UrlPathEncode](#urlpathencode)
- [Confiabilidade e desempenho](#performance)

    - [PreSendRequestHeaders e PreSendRequestContent](#presend)
    - [Eventos de página assíncrona com formulários da Web](#asyncevents)
    - [Disparar e esquecer de trabalho](#fire)
    - [Corpo da entidade de solicitação](#requestentity)
    - [Response. Redirect e Response](#redirect)
    - [EnableViewState e ViewStateMode](#viewstatemode)
    - [SqlMembershipProvider](#sqlprovider)
    - [Solicitações de execução longa (> 110 segundos)](#long)

<a id="standards"></a>

## <a name="standards-compliance"></a>Conformidade com padrões

<a id="adapters"></a>

### <a name="control-adapters"></a>Adaptadores de controle

Recomendação: Interromper o uso de adaptadores de controle para processamento adaptável e, em vez disso, use consultas de mídia CSS e HTML compatíveis com os padrões.

Adaptadores de controles foram introduzidas no .NET 2.0 para processar o código de apresentação que foi personalizado para ambientes e dispositivos diferentes. Agora, esse processamento adaptável pode ser feito com o CSS e HTML. Você deve parar de usar adaptadores de controle e converter todos os adaptadores existentes em CSS e HTML.

Para obter mais informações, consulte [consultas de mídia](http://www.w3.org/TR/css3-mediaqueries/) e [como: adicionar a páginas de celular para o ASP.NET Web Forms / aplicativo MVC](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).

<a id="styleprop"></a>

### <a name="style-properties-on-controls"></a>Propriedades de estilo para controles

Recomendação: Parar definindo valores de estilo na marcação do controle e, em vez disso, defina valores de formatação de folhas de estilo CSS.

Controles de servidor Web contêm dezenas de propriedades que podem ser usadas para definir propriedades de estilo de linha. Por exemplo, a propriedade ForeColor define a cor do texto para um controle. Você pode fazer o mesmo efeito com mais eficiência por meio de folhas de estilo CSS. Folhas de estilos permitem que você centralize o valores de estilo e evite definir esses valores ao longo de seu aplicativo.

O exemplo a seguir mostra uma classe CSS o texto de conjuntos para vermelho.

[!code-css[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample1.css)]

O exemplo a seguir mostra como aplicar dinamicamente a classe CSS.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample2.cs)]

<a id="callback"></a>

### <a name="page-and-control-callbacks"></a>Página e retornos de chamada de controle

Recomendação: Interromper o uso de retornos de chamada de página e controle e, em vez disso, use qualquer um dos seguintes: AJAX, UpdatePanel, métodos de ação MVC, API da Web ou SignalR.

Em versões anteriores do ASP.NET, métodos de retorno de chamada de página e controle habilitado atualizar parte da página da web sem atualizar uma página inteira. Agora você pode fazer atualizações parciais de página por meio de [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [API da Web](../../../web-api/index.md) ou [SignalR](../../../signalr/index.md). Você deve interromper usando métodos de retorno de chamada porque eles podem causar problemas com URLs amigáveis e roteamento. Por padrão, os controles não permitem que os métodos de retorno de chamada, mas se você habilitar esse recurso em um controle, você deverá desabilitá-la.

<a id="browsercap"></a>

### <a name="browser-capability-detection"></a>Detecção de recursos do navegador

Recomendação: Parar de usar a detecção de recursos do navegador estático e, em vez disso, usar a detecção de recurso dinâmico.

Em versões anteriores do ASP.NET, os recursos com suporte para cada navegador foram armazenados em um arquivo XML. Detectando suporte de recurso por meio de uma pesquisa estática não é a melhor abordagem. Agora, você pode detectar dinamicamente um navegador com suporte do recursos usando uma estrutura de detecção de recursos, tais como [Modernizr](http://modernizr.com/). Detecção de recurso determina o suporte ao tentar usar um método ou propriedade e, em seguida, verifique se o navegador produziu o resultado desejado. Por padrão, o Modernizr está incluído nos modelos de aplicativo Web.

<a id="security"></a>

## <a name="security"></a>Segurança

<a id="validation"></a>

### <a name="request-validation"></a>Validação de solicitação

Recomendação: Validar a entrada do usuário e codificar a saída dos usuários.

Validação de solicitação é um recurso do ASP.NET que inspeciona cada solicitação de e para a solicitação se for encontrada uma ameaça percebida. Não dependem de validação de solicitação para proteger seu aplicativo contra ataques de script entre sites. Em vez disso, valide todas as entradas de usuários e codificar a saída. Em alguns casos limitados, você pode usar expressões regulares para validar a entrada, mas em casos mais complicados, a que você deve validar entrada do usuário por meio de classes .NET que determinam se o valor corresponder valores permitidos.

O exemplo a seguir mostra como usar um método estático na classe Uri para determinar se o Uri fornecido por um usuário é válido.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample3.cs)]

No entanto, para verificar suficientemente Uri, você também deve verificar para certificar-se de que ele especifica `http` ou `https`. O exemplo a seguir usa métodos de instância para verificar se o Uri é válido.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample4.cs)]

Antes de processar a entrada do usuário como HTML ou como entrada do usuário em uma consulta SQL, codifica os valores para garantir um código mal-intencionado não está incluído.

Você pode HTML codificar o valor na marcação com o &lt;%: %&gt; sintaxe, conforme mostrado abaixo.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample5.aspx?highlight=1)]

Ou, na sintaxe do Razor, você pode HTML codificar com @, conforme mostrado abaixo.

[!code-cshtml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample6.cshtml?highlight=1)]

A exemplo a seguir mostra como HTML codifica um valor no code-behind.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample7.cs)]

Para codificar com segurança um valor para comandos SQL, use os parâmetros de comando, como o [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx). <a id="cookieless"></a>

### <a name="cookieless-forms-authentication-and-session"></a>Sessão e autenticação de formulários cookieless

Recomendação: Exigem cookies.

Passar informações de autenticação na cadeia de caracteres de consulta não é segura. Portanto, requerem cookies quando seu aplicativo inclui a autenticação. Se o cookie armazena informações confidenciais, considere a possibilidade de exigir SSL para o cookie.

O exemplo a seguir mostra como especificar o arquivo Web. config que a autenticação de formulários exige um cookie que é transmitido por SSL.

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample8.xml)]

<a id="viewstatemac"></a>

### <a name="enableviewstatemac"></a>EnableViewStateMac

Recomendação: Nunca definido como false.

Por padrão, EnbableViewStateMac é definido como true. Mesmo que seu aplicativo não estiver usando o estado de exibição, não defina EnableViewStateMac como false. Definir esse valor como false tornará seu aplicativo vulnerável a criação de scripts entre sites.

Começando com o ASP.NET 4.5.2, o tempo de execução impõe **EnableViewStateMac = true**. Mesmo se for definido como false, o tempo de execução ignora esse valor e prossegue com o valor definido como true. Para obter mais informações, consulte [ASP.NET 4.5.2 e EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).

O exemplo a seguir mostra como definir EnableViewStateMac como true. Você não precisa realmente definir esse valor como true porque ele é verdadeiro por padrão. No entanto, se você tiver defini-lo como false em qualquer página em seu aplicativo, você deve corrigir esse valor imediatamente.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample9.aspx)]

<a id="medium"></a>

### <a name="medium-trust"></a>Confiança média

Recomendação: Não dependem de confiança médio (ou qualquer outro nível de confiança) como um limite de segurança.

Confiança parcial não protegem adequadamente o aplicativo e não deve ser usada. Em vez disso, use a confiança total e isolar aplicativos não confiáveis em pools de aplicativos separados. Além disso, execute cada pool de aplicativos com uma identidade exclusiva. Para obter mais informações, consulte [ASP.NET de confiança parcial não garante o isolamento do aplicativo](https://support.microsoft.com/kb/2698981).

<a id="appsettings"></a>

### <a name="ltappsettingsgt"></a>&lt;appSettings&gt;

Recomendação: Não desative as configurações de segurança em &lt;appSettings&gt; elemento.

O elemento appSettings contém muitos valores que são necessários para atualizações de segurança. Você não deve alterar ou desativar esses valores. Se você precisar desativar esses valores ao implantar uma atualização, imediatamente habilite novamente depois de concluir a implantação.

Para obter detalhes, consulte [ASP.NET appSettings Element](https://msdn.microsoft.com/library/hh975440.aspx).

<a id="urlpathencode"></a>

### <a name="urlpathencode"></a>UrlPathEncode

Recomendação: Use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) em vez disso.

O método UrlPathEncode foi adicionado para o .NET Framework para resolver um problema de compatibilidade de navegador muito específicas. Ele não codifica adequadamente uma URL e não protege o aplicativo de script entre sites. Você nunca deve usá-lo em seu aplicativo. Em vez disso, use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).

O exemplo a seguir mostra como passar uma URL codificada como um parâmetro de cadeia de caracteres de consulta para um controle de hiperlink.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample10.cs)]

<a id="performance"></a>

## <a name="reliability-and-performance"></a>Confiabilidade e desempenho

<a id="presend"></a>

### <a name="presendrequestheaders-and-presendrequestcontent"></a>PreSendRequestHeaders e PreSendRequestContent

Recomendação: Não use esses eventos com os módulos gerenciados. Em vez disso, grave um módulo nativo do IIS para executar as tarefas necessárias. Consulte [criar módulos HTTP de código nativo](https://msdn.microsoft.com/library/ms693629.aspx).

Você pode usar o [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) e [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) eventos com os módulos nativos do IIS.
> [!WARNING]
> Não use `PreSendRequestHeaders` e `PreSendRequestContent` com módulos gerenciados que implementam `IHttpModule`. A configuração dessas propriedades pode causar problemas com solicitações assíncronas. A combinação de roteamento solicitado aplicativo (ARR) e websockets pode resultar em exceções de violação de acesso que podem causar w3wp falhar. Por exemplo, iiscore! W3_CONTEXT_BASE::GetIsLastNotification + 68 no iiscore.dll causou uma exceção de violação de acesso (0xC0000005).

<a id="asyncevents"></a>

### <a name="asynchronous-page-events-with-web-forms"></a>Eventos de página assíncrona com formulários da Web

Recomendação: Formulários da Web, evite escrever async void métodos para eventos de ciclo de vida da página e, em vez disso, use [RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) para código assíncrono.

Quando você marca um evento de página com **async** e **void**, você não pode determinar quando o código assíncrono foi concluída. Em vez disso, use RegisterAsyncTask para executar o código assíncrono de uma maneira que permite controlar a sua conclusão.

A exemplo a seguir mostra um botão Clique manipulador que contém código assíncrono. Este exemplo inclui ler um valor de cadeia de caracteres de forma assíncrona, que é fornecido apenas como um exemplo simplificado de uma tarefa assíncrona e não como uma prática recomendada.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample11.cs)]

Se você estiver usando tarefas assíncronas, defina a estrutura de destino de tempo de execução Http 4.5 no arquivo Web. config. Definir a estrutura de destino para 4.5 ativa no novo contexto de sincronização que foi adicionada no .NET 4.5. Esse valor é definido por padrão em novos projetos no Visual Studio 2012, mas não será definida se você estiver trabalhando com um projeto existente.

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample12.xml)]

<a id="fire"></a>

### <a name="fire-and-forget-work"></a>Disparar e esquecer de trabalho

Recomendação: Ao lidar com uma solicitação dentro do ASP.NET, evite Iniciando trabalho disparar e esquecer (chamando o método QueueUserWorkItem ou criando um temporizador que chama repetidamente um delegado).

Se seu aplicativo tiver disparar e esquecer de trabalho que é executado dentro do ASP.NET, seu aplicativo pode obter fora de sincronia. A qualquer momento, o domínio de aplicativo pode ser destruído que significa que o processo contínuo não pode coincidir com o estado atual do aplicativo.

Você deve mover esse tipo de trabalho fora do ASP.NET. Você pode usar um trabalhos da Web, o serviço do Windows ou uma função de trabalho no Azure para executar o trabalho em andamento e executar esse código de outro processo.

Se você deve executar este trabalho dentro do ASP.NET, você pode adicionar o pacote do Nuget chamado [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) para executar o código.

<a id="requestentity"></a>

### <a name="request-entity-body"></a>Corpo da entidade de solicitação

Recomendação: Evite ler Request. Form ou Request.InputStream antes do manipulador de evento de execução.

O mais recente que você deve ler de Request. Form ou Request.InputStream é durante o manipulador execute eventos. No MVC, o controlador é o manipulador e evento de execução é quando o método de ação é executada. Formulários da Web, a página é o manipulador e evento de execução é quando o evento Page.Init é acionado. Se você ler o corpo da entidade de solicitação anterior ao evento de execução, você interfere com o processamento da solicitação.

Se você precisar ler o corpo da entidade de solicitação antes do evento de execução, use [Request.GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) ou [Request.GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx). Quando você usa GetBufferlessInputStream, obter o fluxo bruto da solicitação e assuma a responsabilidade pelo processamento de toda a solicitação. Depois de chamar GetBufferlessInputStream, Request. Form e Request.InputStream não estão disponíveis porque eles não foram populados pelo ASP.NET. Quando você usa GetBufferedInputStream, você obter uma cópia do fluxo da solicitação. Request e Request.InputStream ainda estão disponíveis mais tarde na solicitação porque o ASP.NET preenche a outra cópia.

<a id="redirect"></a>

### <a name="responseredirect-and-responseend"></a>Response. Redirect e Response

Recomendação: Esteja ciente das diferenças em como o thread é manipulado depois de chamar [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).

O [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) método chama o método Response. Em um processo síncrono, chamar Request.Redirect faz com que o thread atual anular imediatamente. No entanto, em um processo assíncrono, chamar Response. Redirect não anular o thread atual, para a execução de código continua para a solicitação. Em um processo assíncrono, você deve retornar a tarefa do método para interromper a execução de código.

Em um projeto MVC, você não deve chamar Response. Redirect. Em vez disso, retorne um RedirectResult.

<a id="viewstatemode"></a>

### <a name="enableviewstate-and-viewstatemode"></a>EnableViewState e ViewStateMode

Recomendação: Use ViewStateMode, em vez de EnableViewState, para fornecer controle granular sobre quais controles usam o estado de exibição.

Quando você definir EnableViewState como false na diretiva de página, o estado de exibição está desabilitado para todos os controles dentro da página e não pode ser habilitado. Se você deseja habilitar o estado de exibição para somente certos controles na página, defina ViewStateMode como desabilitado para a página.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample13.aspx)]

Em seguida, defina ViewStateMode como habilitado nos controles que realmente precisam de estado de exibição.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample14.aspx)]

Habilitando o estado de exibição para o controle que necessite dele, é possível reduzir o tamanho do estado de exibição para páginas da web.

<a id="sqlprovider"></a>

### <a name="sqlmembershipprovider"></a>SqlMembershipProvider

Recomendação: Use provedores universais.

Os modelos de projeto atual, SqlMembershipProvider foi substituído por [ASP.NET Universal Providers](http://www.nuget.org/packages/Microsoft.AspNet.Providers), que está disponível como um pacote do NuGet. Se você estiver usando SqlMembershipProvider em um projeto que foi criado com uma versão anterior dos modelos, você deve alternar para provedores universais. Os provedores universais funciona com todos os bancos de dados que são suportados pelo Entity Framework.

Para obter mais informações, consulte [Introdução ao ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).

<a id="long"></a>

### <a name="long-running-requests-110-seconds"></a>Solicitações de execução longa (> 110 segundos)

Recomendação: Use [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) ou [SignalR](../../../signalr/index.md) para clientes conectados e usar operações de e/s assíncronas.

Solicitações de execução longa podem causar resultados imprevisíveis e baixo desempenho em seu aplicativo web. A configuração de tempo limite padrão para uma solicitação é 110 segundos. Se você estiver usando o estado de sessão com uma solicitação de longa execução, o ASP.NET irá liberar o bloqueio no objeto de sessão após segundos 110. No entanto, seu aplicativo pode estar no meio de uma operação no objeto de sessão quando o bloqueio for liberado, e a operação não pode ser concluída com êxito. Se uma segunda solicitação do usuário for bloqueada enquanto a primeira solicitação está em execução, a segunda solicitação pode acessar o objeto de sessão em um estado inconsistente.

Se seu aplicativo inclui operações de e/s de bloqueio (ou síncronas), o aplicativo terá resposto.

Para melhorar o desempenho, use as operações de e/s assíncronas do .NET Framework. Além disso, use WebSockets ou SignalR para conectar clientes ao servidor. Esses recursos são projetados para manipular com eficiência solicitações de longa execução.
