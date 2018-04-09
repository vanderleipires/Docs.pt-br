---
uid: aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
title: Middleware OWIN no IIS integrado pipeline | Microsoft Docs
author: Praburaj
description: Este artigo mostra como executar componentes de middleware OWIN (OMCs) no pipeline integrado do IIS, e como definir o evento de pipeline um OMC executa em. Você deve...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2013
ms.topic: article
ms.assetid: d031c021-33c2-45a5-bf9f-98f8fa78c2ab
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
msc.type: authoredcontent
ms.openlocfilehash: 5df70c80084a32c5f61ac9288c8cdbfaaa47f124
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="owin-middleware-in-the-iis-integrated-pipeline"></a>Middleware OWIN no pipeline integrado do IIS
====================
por [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)

> Este artigo mostra como executar componentes de middleware OWIN (OMCs) no pipeline integrado do IIS, e como definir o evento de pipeline um OMC executa em. Você deve examinar [uma visão geral de projeto Katana](an-overview-of-project-katana.md) e [detecção de classe de inicialização OWIN](owin-startup-class-detection.md) antes de ler este tutorial. Este tutorial foi escrito de Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ), Chris Ross Praburaj Thiagarajan e Howard Dierking ( [ @howard \_dierking](https://twitter.com/howard_dierking) ).


Embora [OWIN](an-overview-of-project-katana.md) componentes de middleware (OMCs) são projetados basicamente para executar em um pipeline de servidor independente, é possível executar um OMC no pipeline integrado do IIS também (**modo clássico é *não* suporte**). Um OMC pode ser feita para funcionar no pipeline integrado do IIS ao instalar o pacote a seguir do Console de Gerenciador de pacote (PMC):

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample1.cmd)]

Isso significa que todas as estruturas de aplicativo, mesmo aqueles que não são capazes de executar fora do IIS e System. Web, podem se beneficiar de componentes de middleware OWIN existentes. 

> [!NOTE]
> Todos os `Microsoft.Owin.Security.*` pacotes envio com o novo sistema de identidade no Visual Studio 2013 (por exemplo: Cookies, Account da Microsoft, Google, Facebook, Twitter, [Token de portador](http://self-issued.info/docs/draft-ietf-oauth-v2-bearer.html), OAuth, servidor de autorização, JWT, Azure Active diretório e os serviços de Federação do Active directory) são criados como OMCs e podem ser usado em cenários auto-hospedado e hospedado no IIS.

## <a name="how-owin-middleware-executes-in-the-iis-integrated-pipeline"></a>Como o Middleware OWIN executa no Pipeline integrado do IIS

OWIN para aplicativos de console, o pipeline do aplicativo criado usando o [configuração de inicialização](owin-startup-class-detection.md) é definida pela ordem em que os componentes são adicionados usando o `IAppBuilder.Use` método. Ou seja, o pipeline OWIN no [Katana](an-overview-of-project-katana.md) tempo de execução processará OMCs na ordem em que eles foram registrados usando `IAppBuilder.Use`. O pipeline integrado do IIS o pipeline de solicitação consiste em [HttpModules](https://msdn.microsoft.com/library/ms178468(v=vs.85).aspx) inscrito em um conjunto predefinido de eventos do pipeline como [BeginRequest](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx), [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx), [AuthorizeRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx), etc.

Se compararmos um OMC ao de um [HttpModule](https://msdn.microsoft.com/library/zec9k340(v=vs.85).aspx) no mundo ASP.NET, um OMC deve ser registrado para o evento correto pipeline predefinidos. Por exemplo, o HttpModule `MyModule` será invocado quando uma solicitação de volta o [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx) estágio no pipeline:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample2.cs?highlight=10)]

Para que um OMC participar essa ordem de execução do mesmo, baseado em evento, o [Katana](an-overview-of-project-katana.md) examina o código de tempo de execução por meio de [configuração de inicialização](owin-startup-class-detection.md) e assina todos os componentes de middleware para um evento de pipeline integrado. Por exemplo, o código a seguir OMC e o registro permite que você veja o registro de evento padrão dos componentes de middleware. (Para obter mais instruções sobre como criar uma classe de inicialização OWIN, consulte [detecção de classe de inicialização OWIN](owin-startup-class-detection.md).)

1. Crie um projeto de aplicativo web vazio e nomeie- **owin2**.
2. Do pacote Manager Console (PMC), execute o seguinte comando: 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample3.cmd)]
3. Adicionar uma `OWIN Startup Class` e nomeie-o `Startup`. Substitua o código gerado pelo seguinte (as alterações são realçadas):  

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample4.cs?highlight=5-7,15-36)]
4. Pressione F5 para executar o aplicativo.

A configuração de inicialização configura um pipeline com middleware três componentes, as duas primeiras exibindo informações de diagnóstico e a última respondendo a eventos (e também exibir informações de diagnóstico). O `PrintCurrentIntegratedPipelineStage` método exibe os eventos de pipeline integrado este middleware é invocado em e uma mensagem. As janelas de saída exibe o seguinte:

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample5.cmd)]

O tempo de execução Katana mapeado cada um dos componentes de middleware OWIN para [PreExecuteRequestHandler](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx) por padrão, que corresponde ao evento de pipeline IIS [PreRequestHandlerExecute](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx).

## <a name="stage-markers"></a>Marcadores de estágio

Você pode marcar OMCs para executar em estágios específicos do pipeline usando o `IAppBuilder UseStageMarker()` método de extensão. Para executar um conjunto de componentes de middleware durante um estágio específico, insira um marcador de estágio logo após o último componente é o conjunto durante o registro. Em qual estágio do pipeline, você pode executar o middleware de regras e os componentes de ordem devem ser executada (as regras são explicadas no tutorial posteriormente). Adicionar o `UseStageMarker` método para o `Configuration` código conforme mostrado abaixo:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample6.cs?highlight=13,19)]

O `app.UseStageMarker(PipelineStage.Authenticate)` chamada configura todos os componentes de middleware registrado anteriormente (nesse caso, os dois componentes de diagnósticos) para executar a fase de autenticação do pipeline. O último componente de middleware (que exibe o diagnóstico e responde a solicitações) será executado no `ResolveCache` estágio (o [ResolveRequestCache](https://msdn.microsoft.com/library/system.web.httpapplication.resolverequestcache.aspx) evento).

Pressione F5 para executar o aplicativo. A janela de saída mostra o seguinte:

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample7.cmd)]

## <a name="stage-marker-rules"></a>Regras de marcador de estágio

Componentes de middleware Owin (OMC) podem ser configurados para serem executados em eventos de estágio de pipeline OWIN a seguir:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample8.cs)]

1. Por padrão, OMCs executado no último evento (`PreHandlerExecute`). É por isso que nosso primeiro exemplo de código exibido "PreExecuteRequestHandler".
2. Você pode usar a um `app.UseStageMarker` método para registrar um OMC para ser executado em versões anteriores, em qualquer estágio do pipeline OWIN listado no `PipelineStage` enum.
3. O pipeline OWIN e o pipeline IIS for ordenado, portanto, chamadas para `app.UseStageMarker` devem estar na ordem. Você não pode definir o manipulador de eventos para um evento que precede o último evento registrado com `app.UseStageMarker`. Por exemplo, *depois* chamando:

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample9.cmd)]

   chamadas para `app.UseStageMarker` passando `Authenticate` ou `PostAuthenticate` não será respeitada e nenhuma exceção será lançada. OMCs executados no estágio mais recente, que, por padrão, é `PreHandlerExecute`. Os marcadores de estágio são usados para torná-los para executar anteriormente. Se você especificar os marcadores de estágio fora de ordem, é arredondado para o marcador anterior. Em outras palavras, a adição de um marcador de estágio diz "Executar não mais tarde do que o estágio X". Execução do OMC no marcador de estágio mais antigo adicionado depois no pipeline OWIN.
4. O estágio mais antigo de chamadas para `app.UseStageMarker` wins. Por exemplo, se você alternar a ordem de `app.UseStageMarker` chamadas do nosso exemplo anterior:

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample10.cs?highlight=13,19)]

   A janela de saída será exibida: 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample11.cmd)]

   Os OMCs executados no `AuthenticateRequest` estágio, porque o último OMC registrado com o `Authenticate` evento e o `Authenticate` evento precede todos os outros eventos.
