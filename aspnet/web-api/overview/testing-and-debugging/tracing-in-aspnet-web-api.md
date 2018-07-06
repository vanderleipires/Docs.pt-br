---
uid: web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
title: Rastreamento na API Web ASP.NET 2 | Microsoft Docs
author: MikeWasson
description: Mostra como habilitar o rastreamento na API Web ASP.NET.
ms.author: aspnetcontent
ms.date: 02/25/2014
ms.assetid: 66a837e9-600b-4b72-97a9-19804231c64a
msc.legacyurl: /web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 0fabb9bcd0293ba88a41ad9d070958dbbb0c4749
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838103"
---
<a name="tracing-in-aspnet-web-api-2"></a>Rastreamento na API Web ASP.NET 2
====================
por [Mike Wasson](https://github.com/MikeWasson)

> Quando você está tentando depurar um aplicativo baseado na web, não há nenhum substituto para um bom conjunto de logs de rastreamento. Este tutorial mostra como habilitar o rastreamento na API Web ASP.NET. Você pode usar esse recurso para rastrear o que faz a estrutura da API da Web antes e depois que ela invoca seu controlador. Você também pode usá-lo para rastrear o seu próprio código.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - [Visual Studio 2017](https://www.visualstudio.com/downloads/) (também funciona com o Visual Studio 2015)
> - API Web 2
> - [Microsoft.AspNet.WebApi.Tracing](http://www.nuget.org/packages/Microsoft.AspNet.WebApi.Tracing)


## <a name="enable-systemdiagnostics-tracing-in-web-api"></a>Habilitar o rastreamento na API Web de System. Diagnostics

Primeiro, vamos criar um novo projeto de aplicativo Web ASP.NET. No Visual Studio, do **arquivo** menu, selecione **New**, em seguida, **projeto**. Sob **modelos**, **Web**, selecione **aplicativo Web ASP.NET**.

[![](tracing-in-aspnet-web-api/_static/image2.png)](tracing-in-aspnet-web-api/_static/image1.png)

Escolha o modelo de projeto de API da Web.

[![](tracing-in-aspnet-web-api/_static/image4.png)](tracing-in-aspnet-web-api/_static/image3.png)

Do **ferramentas** menu, selecione **Library Package Manager**, em seguida, **Console de gerenciamento de pacote**.

Na janela do Console do Gerenciador de pacotes, digite os comandos a seguir.

[!code-console[Main](tracing-in-aspnet-web-api/samples/sample1.cmd)]

O primeiro comando instala o pacote mais recente de rastreamento de API da Web. Ele também atualiza os pacotes de API da Web principal. O segundo comando atualiza o pacote de WebApi.WebHost para a versão mais recente.

> [!NOTE]
> Se você quiser direcionar uma versão específica de API da Web, use o - sinalizador de versão quando você instala o pacote de rastreamento.


Abra o arquivo WebApiConfig.cs no aplicativo\_pasta inicial. Adicione o seguinte código para o **registrar** método.

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample2.cs?highlight=6)]

Esse código adiciona o [SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) classe para o pipeline da API da Web. O **SystemDiagnosticsTraceWriter** classe grava rastreamentos [Trace](https://msdn.microsoft.com/library/system.diagnostics.trace).

Para ver os rastreamentos, execute o aplicativo no depurador. No navegador, navegue até `/api/values`.

![](tracing-in-aspnet-web-api/_static/image5.png)

As instruções de rastreamento são gravadas na janela de saída no Visual Studio. (Da **modo de exibição** menu, selecione **saída**).

[![](tracing-in-aspnet-web-api/_static/image7.png)](tracing-in-aspnet-web-api/_static/image6.png)

Porque **SystemDiagnosticsTraceWriter** grava rastreamentos **Trace**, você pode registrar ouvintes de rastreamento adicionais; por exemplo, para gravar rastreamentos em um arquivo de log. Para obter mais informações sobre os gravadores de rastreamento, consulte o [ouvintes de rastreamento](https://msdn.microsoft.com/library/4y5y10s7.aspx) tópico no MSDN.

### <a name="configuring-systemdiagnosticstracewriter"></a>Configurando o SystemDiagnosticsTraceWriter

O código a seguir mostra como configurar o gravador de rastreamento.

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample3.cs)]

Há duas configurações que você pode controlar:

- IsVerbose: Se for falso, cada rastreamento contém informações mínimas. Se for true, os rastreamentos incluem mais informações.
- MinimumLevel: Define o nível mínimo de rastreamento. Níveis de rastreamento, em ordem, são Debug, Info, Warn, erro e Fatal.

## <a name="adding-traces-to-your-web-api-application"></a>Adicionar rastreamentos ao seu aplicativo de API da Web

Adicionar um gravador de rastreamento fornece acesso imediato aos rastreamentos criados pelo pipeline da API Web. Você também pode usar o gravador de rastreamento para seu próprio código de rastreamento:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample4.cs)]

Para obter o gravador de rastreamento, chame **HttpConfiguration.Services.GetTraceWriter**. De um controlador, esse método é acessível por meio de **ApiController.Configuration** propriedade.

Para gravar um rastreamento, você pode chamar o **ITraceWriter.Trace** método diretamente, mas o [ITraceWriterExtensions](https://msdn.microsoft.com/library/system.web.http.tracing.itracewriterextensions.aspx) classe define alguns métodos de extensão que são mais amigáveis. Por exemplo, o **Info** método mostrado acima cria um rastreamento com nível de rastreamento **informações**.

## <a name="web-api-tracing-infrastructure"></a>Infraestrutura de rastreamento de API da Web

Esta seção descreve como escrever um gravador de rastreamento personalizado para a API da Web.

O pacote Microsoft.AspNet.WebApi.Tracing baseia-se em uma infraestrutura de rastreamento mais geral na API da Web. Em vez de usar Microsoft.AspNet.WebApi.Tracing, você também pode usar alguma outra biblioteca de rastreamento/armazenamento, tal como [NLog](http://nlog-project.org/) ou [log4net](http://logging.apache.org/log4net/).

Para coletar rastreamentos, implementar o **ITraceWriter** interface. Aqui está um exemplo simples:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample5.cs)]

O **ITraceWriter.Trace** método cria um rastreamento. O chamador Especifica um nível de categoria e de rastreamento. A categoria pode ser qualquer cadeia de caracteres definida pelo usuário. Sua implementação de **rastreamento** deve fazer o seguinte:

1. Criar um novo **TraceRecord**. Inicializá-lo com a solicitação, a categoria e o nível de rastreamento, conforme mostrado. Esses valores são fornecidos pelo chamador.
2. Invocar o *traceAction* delegar. Dentro desse delegado, o chamador deve preencher o restante dos **TraceRecord**.
3. Gravar o **TraceRecord**, usando qualquer técnica de registro em log que você deseja. O exemplo mostrado aqui chama simplesmente **Trace**.

## <a name="setting-the-trace-writer"></a>Definindo o gravador de rastreamento

Para habilitar o rastreamento, você deve configurar a API da Web para usar sua **ITraceWriter** implementação. Você fazer isso por meio de **HttpConfiguration** do objeto, conforme mostrado no código a seguir:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample6.cs)]

Gravador de rastreamento apenas uma pode estar ativo. Por padrão, a API Web define um &quot;inoperante&quot; rastreamento que não faz nada. (O &quot;inoperante&quot; rastreamento existe para que o código de rastreamento não precisa verificar se o gravador de rastreamento é **nulo** antes de gravar um rastreamento.)

## <a name="how-web-api-tracing-works"></a>Como funciona o rastreamento de API de Web

Rastreamento em usos de API da Web usa uma API da Web em um *fachada* padrão: quando o rastreamento está habilitado, a API da Web envolve diversas partes do pipeline de solicitação com classes que executam chamadas de rastreamento.

Por exemplo, ao selecionar um controlador, o pipeline usa o **IHttpControllerSelector** interface. Com o rastreamento habilitado, o pipleline insere uma classe que implementa **IHttpControllerSelector** mas chamadas por meio de para a implementação real:

![Rastreamento de API da Web usa o padrão de fachada.](tracing-in-aspnet-web-api/_static/image8.png)

Os benefícios desse design incluem:

- Se você não adicionar um gravador de rastreamento, os componentes de rastreamento não serão instanciados e não tem nenhum impacto no desempenho.
- Se você substituir os serviços padrão como **IHttpControllerSelector** com sua própria implementação personalizada, os rastreamento não é afetado, pois o rastreamento é feito pelo objeto de wrapper.

Você também pode substituir a estrutura de rastreamento de API da Web inteira com sua própria estrutura personalizada, substituindo o padrão **ITraceManager** serviço:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample7.cs)]

Implemente **ITraceManager.Initialize** para inicializar o sistema de rastreamento. Lembre-se de que ele substitui o *inteira* framework de rastreamento, incluindo todo o código de rastreamento que está embutido no API da Web.
