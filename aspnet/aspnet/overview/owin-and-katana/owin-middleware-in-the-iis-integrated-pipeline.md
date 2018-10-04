---
uid: aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
title: Pipeline integrado do Middleware do OWIN no IIS | Microsoft Docs
author: Praburaj
description: Este artigo mostra como executar componentes de middleware OWIN (OMCs) no pipeline integrado do IIS, e como definir o evento de pipeline um OMC executa em. Você deve...
ms.author: riande
ms.date: 11/07/2013
ms.assetid: d031c021-33c2-45a5-bf9f-98f8fa78c2ab
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
msc.type: authoredcontent
ms.openlocfilehash: 6124bcdaeeb0d4342cbde0d3ca52d55f76a953ab
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48576435"
---
<a name="owin-middleware-in-the-iis-integrated-pipeline"></a>Middleware do OWIN no pipeline integrado do IIS
====================
por [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Este artigo mostra como executar componentes de middleware OWIN (OMCs) no pipeline integrado do IIS, e como definir o evento de pipeline um OMC executa em. Você deve revisar [uma visão geral do projeto Katana](an-overview-of-project-katana.md) e [detecção de classe de inicialização OWIN](owin-startup-class-detection.md) antes de ler este tutorial. Este tutorial foi escrito por Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ), Howard Dierking, Praburaj Thiagarajan e Chris Ross ( [ @howard \_dierking](https://twitter.com/howard_dierking) ).


Embora [OWIN](an-overview-of-project-katana.md) componentes de middleware (OMCs) são projetados basicamente para ser executado em um pipeline de servidor independente, é possível executar um OMC no pipeline integrado do IIS também (**é o modo clássico *não* suporte**). Um OMC pode ser feita para funcionar no pipeline integrado do IIS ao instalar o pacote a seguir do Manager Console (PMC):

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample1.cmd)]

Isso significa que todas as estruturas de aplicativo, mesmo aqueles que não são capazes de executar fora do IIS e System. Web, podem se beneficiar de componentes de middleware do OWIN existentes. 

> [!NOTE]
> Todos os `Microsoft.Owin.Security.*` pacotes de envio com o novo sistema de identidade no Visual Studio 2013 (por exemplo: Cookies, Account da Microsoft, Google, Facebook, Twitter, [Token de portador](http://self-issued.info/docs/draft-ietf-oauth-v2-bearer.html), OAuth, servidor de autorização, JWT, Active Directory do Azure diretório e os serviços de Federação do Active Directory) são criados como OMCs e podem ser usado em cenários auto-hospedados e hospedados no IIS.

## <a name="how-owin-middleware-executes-in-the-iis-integrated-pipeline"></a>Como o Middleware do OWIN executa no Pipeline integrado do IIS

OWIN para aplicativos de console, o pipeline do aplicativo criado usando o [configuração de inicialização](owin-startup-class-detection.md) é definida pela ordem em que os componentes são adicionados usando o `IAppBuilder.Use` método. Ou seja, o pipeline do OWIN na [Katana](an-overview-of-project-katana.md) tempo de execução irá processar OMCs na ordem em que eles foram registrados usando `IAppBuilder.Use`. No pipeline integrado do IIS o pipeline de solicitação consiste [HttpModules](https://msdn.microsoft.com/library/ms178468(v=vs.85).aspx) inscreveu como um conjunto predefinido de eventos do pipeline [BeginRequest](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx), [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx), [AuthorizeRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx), etc.

Se compararmos um OMC ao de um [HttpModule](https://msdn.microsoft.com/library/zec9k340(v=vs.85).aspx) no mundo ASP.NET, um OMC deve ser registrado para o evento de pipeline predefinido correto. Por exemplo, o HttpModule `MyModule` será chamado quando uma solicitação é fornecido o [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx) estágio no pipeline:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample2.cs?highlight=10)]

Para que um OMC participar essa ordem de execução do mesma, baseado em evento, o [Katana](an-overview-of-project-katana.md) código de tempo de execução examina o [configuração de inicialização](owin-startup-class-detection.md) e assina cada um dos componentes de middleware para um eventos de pipeline integrado. Por exemplo, o código a seguir OMC e o registro permite que você veja o registro de evento padrão dos componentes de middleware. (Para obter instruções sobre como criar uma classe de inicialização OWIN mais detalhadas, consulte [detecção de classe de inicialização OWIN](owin-startup-class-detection.md).)

1. Crie um projeto de aplicativo web vazio e denomine **owin2**.
2. Do pacote Manager Console (PMC), execute o seguinte comando: 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample3.cmd)]
3. Adicionar um `OWIN Startup Class` e nomeie-o `Startup`. Substitua o código gerado pelo seguinte (as alterações são realçadas):  

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample4.cs?highlight=5-7,15-36)]
4. Pressione F5 para executar o aplicativo.

Define a configuração de inicialização de um pipeline com componentes de middleware de três, os dois primeiros exibindo informações de diagnóstico e o último deles respondendo a eventos (e também exibir informações de diagnóstico). O `PrintCurrentIntegratedPipelineStage` método exibe os eventos de pipeline integrado este middleware é invocado em e uma mensagem. As janelas de saída exibe o seguinte:

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample5.cmd)]

O tempo de execução do Katana mapeado em cada um dos componentes de middleware OWIN para [PreExecuteRequestHandler](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx) por padrão, que corresponde ao evento de pipeline do IIS [PreRequestHandlerExecute](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx).

## <a name="stage-markers"></a>Marcadores de estágio

Você pode marcar OMCs executar nos estágios específicos do pipeline usando o `IAppBuilder UseStageMarker()` método de extensão. Para executar um conjunto de componentes de middleware durante um determinado estágio, inserir um marcador de estágio logo após o último componente é o conjunto durante o registro. Há regras de em qual estágio do pipeline, você pode executar o middleware e os componentes de ordem devem executar (as regras são explicadas adiante no tutorial). Adicione a `UseStageMarker` método para o `Configuration` código conforme mostrado abaixo:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample6.cs?highlight=13,19)]

O `app.UseStageMarker(PipelineStage.Authenticate)` chamada configura todos os componentes de middleware previamente registrado (nesse caso, nossos dois componentes de diagnóstico) para ser executado no estágio de autenticação do pipeline. O último componente de middleware (que exibe o diagnóstico e responde às solicitações) serão executados `ResolveCache` estágio (o [ResolveRequestCache](https://msdn.microsoft.com/library/system.web.httpapplication.resolverequestcache.aspx) evento).

Pressione F5 para executar o aplicativo. A janela de saída mostra o seguinte:

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample7.cmd)]

## <a name="stage-marker-rules"></a>Regras de marcador de estágio

Componentes de middleware Owin (OMC) podem ser configurados para serem executados nos seguintes eventos de estágio de pipeline do OWIN:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample8.cs)]

1. Por padrão, OMCs executados no último evento (`PreHandlerExecute`). É por isso que nosso primeiro código de exemplo exibido "PreExecuteRequestHandler".
2. Você pode usar a um `app.UseStageMarker` método para registrar um OMC anteriormente, executar em qualquer estágio do pipeline do OWIN listado no `PipelineStage` enum.
3. O pipeline do OWIN e o pipeline do IIS for ordenado, portanto, chamadas para `app.UseStageMarker` devem estar na ordem. Você não pode definir o manipulador de eventos a um evento que precede o último evento registrado com `app.UseStageMarker`. Por exemplo, *após* chamando:

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample9.cmd)]

   chamadas para `app.UseStageMarker` passando `Authenticate` ou `PostAuthenticate` não será respeitada e nenhuma exceção será lançada. OMCs executados no estágio de mais recente, que, por padrão, é `PreHandlerExecute`. Os marcadores de estágio são usados para torná-los para ser executado anteriormente. Se você especificar os marcadores de estágio fora de ordem, podemos ida e volta para o marcador anterior. Em outras palavras, a adição de um marcador de estágio diz "Executar não mais tarde do que o estágio de X". Execução do OMC o marcador de estágio mais antigo adicionado depois-los no pipeline do OWIN.
4. O mais cedo de chamadas para `app.UseStageMarker` wins. Por exemplo, se você alternar a ordem das `app.UseStageMarker` chamadas do nosso exemplo anterior:

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample10.cs?highlight=13,19)]

   A janela de saída será exibida: 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample11.cmd)]

   Os OMCs todos executados nos `AuthenticateRequest` prepara, porque o último OMC registrado com o `Authenticate` evento e o `Authenticate` eventos precede todos os outros eventos.
