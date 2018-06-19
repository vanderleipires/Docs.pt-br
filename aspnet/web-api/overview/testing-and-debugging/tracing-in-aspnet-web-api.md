---
uid: web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
title: Rastreamento no ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: Mostra como habilitar o rastreamento de API da Web do ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 66a837e9-600b-4b72-97a9-19804231c64a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 7392ae5d9bc4c3aab45a9373099a0ee18e873a4f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28044202"
---
<a name="tracing-in-aspnet-web-api-2"></a>Rastreamento no ASP.NET Web API 2
====================
por [Mike Wasson](https://github.com/MikeWasson)

> Quando você está tentando depurar um aplicativo baseado na web, não há nenhum substituto para um bom conjunto de logs de rastreamento. Este tutorial mostra como habilitar o rastreamento de API da Web do ASP.NET. Você pode usar esse recurso para rastrear o que faz a estrutura de Web API antes e depois de invocar o controlador. Você também pode usar isso para rastrear o seu próprio código.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - [Visual Studio de 2017](https://www.visualstudio.com/downloads/) (também funciona com o Visual Studio 2015)
> - Web API 2
> - [Microsoft.AspNet.WebApi.Tracing](http://www.nuget.org/packages/Microsoft.AspNet.WebApi.Tracing)


## <a name="enable-systemdiagnostics-tracing-in-web-api"></a>Habilitar rastreamento de API da Web de System. Diagnostics

Primeiro, vamos criar um novo projeto de aplicativo Web ASP.NET. No Visual Studio, do **arquivo** menu, selecione **novo**, em seguida, **projeto**. Em **modelos**, **Web**, selecione **aplicativo Web ASP.NET**.

[![](tracing-in-aspnet-web-api/_static/image2.png)](tracing-in-aspnet-web-api/_static/image1.png)

Escolha o modelo de projeto de API da Web.

[![](tracing-in-aspnet-web-api/_static/image4.png)](tracing-in-aspnet-web-api/_static/image3.png)

Do **ferramentas** menu, selecione **Gerenciador de biblioteca de pacote**, em seguida, **Console de gerenciamento de pacote**.

Na janela do Console do Gerenciador de pacotes, digite os comandos a seguir.

[!code-console[Main](tracing-in-aspnet-web-api/samples/sample1.cmd)]

O primeiro comando instala o pacote de rastreamento de API da Web mais recente. Ele também atualiza os pacotes de API da Web principal. O segundo comando atualiza o pacote de WebApi.WebHost para a versão mais recente.

> [!NOTE]
> Se você deseja direcionar uma versão específica de API da Web, use-sinalizador de versão, quando você instala o pacote de rastreamento.


Abra o arquivo WebApiConfig.cs no aplicativo\_pasta inicial. Adicione o seguinte código para o **registrar** método.

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample2.cs?highlight=6)]

Esse código adiciona a [SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) classe para o pipeline de API da Web. O **SystemDiagnosticsTraceWriter** classe grava rastreamentos para [Trace](https://msdn.microsoft.com/library/system.diagnostics.trace).

Para ver os rastreamentos, execute o aplicativo no depurador. No navegador, navegue até `/api/values`.

![](tracing-in-aspnet-web-api/_static/image5.png)

As instruções de rastreamento são gravadas na janela de saída no Visual Studio. (Da **exibição** menu, selecione **saída**).

[![](tracing-in-aspnet-web-api/_static/image7.png)](tracing-in-aspnet-web-api/_static/image6.png)

Porque **SystemDiagnosticsTraceWriter** grava rastreamentos para **Trace**, você pode registrar os ouvintes de rastreamento adicionais; por exemplo gravar rastreamentos em um arquivo de log. Para obter mais informações sobre os gravadores de rastreamento, consulte o [ouvintes de rastreamento](https://msdn.microsoft.com/library/4y5y10s7.aspx) no MSDN.

### <a name="configuring-systemdiagnosticstracewriter"></a>Configurando o SystemDiagnosticsTraceWriter

O código a seguir mostra como configurar o gravador de rastreamento.

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample3.cs)]

Há duas configurações que você pode controlar:

- IsVerbose: Se for falso, cada rastreamento contém um mínimo de informações. Se verdadeiro, os rastreamentos incluem mais informações.
- MinimumLevel: Define o nível mínimo de rastreamento. Níveis de rastreamento, em ordem, são depuração, informações, advertência, erro e Fatal.

## <a name="adding-traces-to-your-web-api-application"></a>Adicionar rastreamentos ao seu aplicativo de API da Web

Adicionar um gravador de rastreamento oferece acesso imediato para rastreamentos criados pelo pipeline de API da Web. Você também pode usar o gravador de rastreamento para rastrear o seu próprio código:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample4.cs)]

Para obter o gravador de rastreamento, chame **HttpConfiguration.Services.GetTraceWriter**. De um controlador, esse método é acessível por meio de **ApiController.Configuration** propriedade.

Para gravar um rastreamento, você pode chamar o **ITraceWriter.Trace** método diretamente, mas o [ITraceWriterExtensions](https://msdn.microsoft.com/library/system.web.http.tracing.itracewriterextensions.aspx) classe define alguns métodos de extensão que são mais amigáveis. Por exemplo, o **informações** método mostrado acima cria um rastreamento com o nível de rastreamento **informações**.

## <a name="web-api-tracing-infrastructure"></a>Infraestrutura de rastreamento de API da Web

Esta seção descreve como escrever um gravador de rastreamento personalizada para a API da Web.

O pacote de Microsoft.AspNet.WebApi.Tracing é criado sobre uma infraestrutura de rastreamento mais geral na API da Web. Em vez de usar Microsoft.AspNet.WebApi.Tracing, você também pode usar alguma outra biblioteca de rastreamento/armazenamento de logs, como [NLog](http://nlog-project.org/) ou [log4net](http://logging.apache.org/log4net/).

Para coletar rastreamentos, implementar a **ITraceWriter** interface. Aqui está um exemplo simples:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample5.cs)]

O **ITraceWriter.Trace** método cria um rastreamento. O chamador Especifica um nível de categoria e rastreamento. A categoria pode ser qualquer cadeia de caracteres definida pelo usuário. A implementação do **rastreamento** deve fazer o seguinte:

1. Criar um novo **TraceRecord**. Inicializá-lo com a solicitação, a categoria e o nível de rastreamento, conforme mostrado. Esses valores são fornecidos pelo chamador.
2. Invocar o *traceAction* delegate. Dentro este delegado, o chamador deve preencha o restante do **TraceRecord**.
3. Gravar o **TraceRecord**, usando qualquer técnica de registro em log que você deseja. O exemplo mostrado aqui simplesmente chama **Trace**.

## <a name="setting-the-trace-writer"></a>Definindo o gravador de rastreamento

Para habilitar o rastreamento, você deve configurar a API da Web para usar o **ITraceWriter** implementação. Fazer isso por meio de **HttpConfiguration** do objeto, como mostrado no código a seguir:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample6.cs)]

Gravador de rastreamento somente uma pode estar ativo. Por padrão, a API da Web define um &quot;não-operacional&quot; rastreamento que não faz nada. (O &quot;não-operacional&quot; rastreamento existe para que o código de rastreamento não precisa verificar se o gravador de rastreamento é **nulo** antes de gravar um rastreamento.)

## <a name="how-web-api-tracing-works"></a>Como Web API Tracing funciona

Rastreamento no usa API da Web que usa uma API da Web em um *fachada* padrão: quando o rastreamento estiver habilitado, API da Web envolve várias partes do canal de solicitação com classes que executam chamadas de rastreamento.

Por exemplo, ao selecionar um controlador, o pipeline usa o **IHttpControllerSelector** interface. Com o rastreamento ativado, o pipleline insere uma classe que implementa **IHttpControllerSelector** mas chamadas para a implementação real:

![Rastreamento de API da Web usa o padrão de fachada.](tracing-in-aspnet-web-api/_static/image8.png)

Os benefícios desse projeto incluem:

- Se você não adicionar um gravador de rastreamento, os componentes de rastreamento não serão instanciados e sem afetar o desempenho.
- Se você substituir serviços padrão como **IHttpControllerSelector** com sua própria implementação personalizada, o rastreamento não seja afetado porque o rastreamento é feito pelo objeto de wrapper.

Você também pode substituir a estrutura inteira de rastreamento de API da Web com sua própria estrutura personalizada, substituindo o padrão **ITraceManager** serviço:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample7.cs)]

Implementar **ITraceManager.Initialize** para inicializar o sistema de rastreamento. Lembre-se de que ele substitui o *todo* framework de rastreamento, incluindo todo o código de rastreamento é criado na API da Web.
