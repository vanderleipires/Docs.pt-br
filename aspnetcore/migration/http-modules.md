---
title: "Migrando manipuladores HTTP e módulos ASP.NET Core middleware"
author: rick-anderson
description: 
ms.author: tdykstra
manager: wpickett
ms.date: 12/07/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/http-modules
ms.openlocfilehash: 44b2b38c284e678344432d4473162404b4bb75a5
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="migrating-http-handlers-and-modules-to-aspnet-core-middleware"></a>Migrando manipuladores HTTP e módulos ASP.NET Core middleware 

Por [Matt Perdeck](https://www.linkedin.com/in/mattperdeck)

Este artigo mostra como migrar ASP.NET existente [módulos HTTP e manipuladores de System. webServer](https://docs.microsoft.com/iis/configuration/system.webserver/) para ASP.NET Core [middleware](../fundamentals/middleware.md).

## <a name="modules-and-handlers-revisited"></a>Manipuladores revisitados e módulos

Antes de prosseguir para o ASP.NET Core middleware, vejamos primeiro novamente como manipuladores e módulos HTTP funcionam:

![Manipulador de módulos](http-modules/_static/moduleshandlers.png)

**Manipuladores são:**

   * As classes que implementam [IHttpHandler](https://docs.microsoft.com/dotnet/api/system.web.ihttphandler)

   * Usado para manipular solicitações com uma extensão, ou o nome de arquivo fornecido como *relatório*

   * [Configurado](https://docs.microsoft.com//iis/configuration/system.webserver/handlers/) em *Web. config*

**Os módulos são:**

   * As classes que implementam [IHttpModule](https://docs.microsoft.com/dotnet/api/system.web.ihttpmodule)

   * Chamado para cada solicitação

   * Capaz de curto-circuito (Interromper processamento adicional de uma solicitação)

   * Capaz de adicionar a resposta HTTP, ou criar seus próprios

   * [Configurado](https://docs.microsoft.com//iis/configuration/system.webserver/modules/) em *Web. config*

**A ordem em que os módulos de processam solicitações de entrada é determinada por:**

   1. O [ciclo de vida do aplicativo](https://msdn.microsoft.com/library/ms227673.aspx), que é um eventos série acionado pelo ASP.NET: [BeginRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.beginrequest), [AuthenticateRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.authenticaterequest), etc. Cada módulo pode criar um manipulador de eventos de um ou mais.

   2. Para o mesmo evento, a ordem na qual eles são configurados em *Web. config*.

Além de módulos, você pode adicionar manipuladores para os eventos de ciclo de vida para o *Global.asax.cs* arquivo. Esses manipuladores executar após os manipuladores nos módulos configurados.

## <a name="from-handlers-and-modules-to-middleware"></a>Manipuladores e módulos de middleware

**Middleware são mais simples do que manipuladores e módulos HTTP:**

   * Módulos, manipuladores, *Global.asax.cs*, *Web. config* (exceto para a configuração do IIS) e o ciclo de vida do aplicativo serão excluídos

   * As funções dos módulos e manipuladores de tem sido controladas por middleware

   * Middleware são configurados usando o código em vez de em *Web. config*

   * [Ramificação de pipeline](../fundamentals/middleware.md#middleware-run-map-use) permite que você envia solicitações ao middleware específico, com base na URL não só mas também em cabeçalhos de solicitação, cadeias de caracteres de consulta, etc.

**Middleware são muito semelhantes aos módulos:**

   * Invocado em princípio para cada solicitação

   * Capaz de curto-circuito a uma solicitação por [não passar a solicitação para o próximo middleware](#http-modules-shortcircuiting-middleware)

   * Capaz de criar sua própria resposta HTTP

**Middleware e módulos são processados em uma ordem diferente:**

   * Ordem de middleware é baseada na ordem em que são inseridos no pipeline de solicitação, enquanto a ordem dos módulos baseia-se principalmente em [ciclo de vida do aplicativo](https://msdn.microsoft.com/library/ms227673.aspx) eventos

   * Ordem de middleware para respostas é o oposto do que para solicitações, enquanto a ordem dos módulos é o mesmo para solicitações e respostas

   * Consulte [criando um pipeline de middleware com IApplicationBuilder](../fundamentals/middleware.md#creating-a-middleware-pipeline-with-iapplicationbuilder)

![Middleware](http-modules/_static/middleware.png)

Observe como na imagem acima, o middleware de autenticação curto-circuito a solicitação.

## <a name="migrating-module-code-to-middleware"></a>Migrando o código de módulo para middleware

Um módulo HTTP existente será semelhante a este:

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyModule.cs?highlight=6,8,24,31)]

Conforme mostrado no [Middleware](../fundamentals/middleware.md) página, um middleware ASP.NET Core é uma classe que expõe um `Invoke` colocando método um `HttpContext` e retornar um `Task`. Seu novo middleware será assim:

<a name="http-modules-usemiddleware"></a>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddleware.cs?highlight=9,13,20,24,28,30,32)]

O modelo de middleware acima foi obtido da seção de [gravar middleware](../fundamentals/middleware.md#middleware-writing-middleware).

O *MyMiddlewareExtensions* classe auxiliar torna mais fácil de configurar o middleware em seu `Startup` classe. O `UseMyMiddleware` método adiciona sua classe de middleware no pipeline de solicitação. Serviços necessários para o middleware são injetados no construtor do middleware.

<a name="http-modules-shortcircuiting-middleware"></a>

O módulo pode encerrar uma solicitação, por exemplo, se o usuário não está autorizado:

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyTerminatingModule.cs?highlight=9,10,11,12,13&name=snippet_Terminate)]

Um middleware lida com isso chamando não `Invoke` no próximo middleware no pipeline. Tenha em mente que isso não totalmente encerra a solicitação, pois middlewares anterior ainda será invocado quando a resposta torna sua maneira novamente por meio do pipeline.

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyTerminatingMiddleware.cs?highlight=7,8&name=snippet_Terminate)]

Ao migrar a funcionalidade do módulo para o seu middleware de novo, você pode achar que seu código não compilado porque o `HttpContext` classe tiverem sido alterados significativamente no núcleo do ASP.NET. [Mais tarde](#migrating-to-the-new-httpcontext), você verá como migrar para o ASP.NET Core HttpContext novo.

## <a name="migrating-module-insertion-into-the-request-pipeline"></a>Migrando inserção de módulo no pipeline de solicitação

Módulos HTTP normalmente são adicionados ao pipeline de solicitação usando *Web. config*:

[!code-xml[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32-33,36,43,50,101)]

Converter isso por [adicionando seu novo middleware](../fundamentals/middleware.md#creating-a-middleware-pipeline-with-iapplicationbuilder) para o pipeline de solicitação no seu `Startup` classe:

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=16)]

Ponto no pipeline, em que você insere seu novo middleware exato depende do evento que ela tratada como um módulo (`BeginRequest`, `EndRequest`, etc.) e sua ordem na lista de módulos em *Web. config*.

Como anteriormente não mencionado, há mais nenhum ciclo de vida do aplicativo no núcleo do ASP.NET e a ordem na qual as respostas são processadas pelo middleware diferente daquela usada pelos módulos. Isso pode tornar sua decisão de ordenação mais difícil.

Se ordenação se torna um problema, você pode dividir seu módulo em vários componentes de middleware que podem ser ordenados de forma independente.

## <a name="migrating-handler-code-to-middleware"></a>Migrando o código de manipulador de middleware

Um manipulador HTTP parecida com esta:

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/HttpHandlers/ReportHandler.cs?highlight=5,7,13,14,15,16)]

No seu projeto do ASP.NET Core, isso pode ser traduzido para um middleware semelhante a este:

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/ReportHandlerMiddleware.cs?highlight=7,9,13,20,21,22,23,40,42,44)]

Este middleware é muito semelhante para o middleware correspondente a módulos. A única diferença é que não há aqui nenhuma chamada para `_next.Invoke(context)`. Isso faz sentido, porque o manipulador não é no final do pipeline de solicitação, que será a nenhum próximo middleware para invocar.

## <a name="migrating-handler-insertion-into-the-request-pipeline"></a>Migrando inserção manipulador no pipeline de solicitação

Configurar um manipulador HTTP é feita em *Web. config* e tem a seguinte aparência:

[!code-xml[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32,46-48,50,101)]

Você pode convertê-lo adicionando seu novo middleware de manipulador para o pipeline de solicitação no seu `Startup` classe, semelhante ao middleware convertido de módulos. O problema com essa abordagem é que ele poderia enviar todas as solicitações para seu novo middleware de manipulador. No entanto, você precisará apenas solicitações com uma determinada extensão para alcançar seu middleware. Essa seria a mesma funcionalidade que tinha com o manipulador HTTP.

Uma solução é ramificar o pipeline de solicitações com uma determinada extensão, usando o `MapWhen` método de extensão. Você pode fazer isso no mesmo `Configure` método em que você adiciona o middleware outros:

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=27-34)]

`MapWhen`usa esses parâmetros:

1. Uma expressão lambda que usa o `HttpContext` e retorna `true` se a solicitação deve ir para a ramificação. Isso significa que você pode se ramificar solicitações não apenas com base em sua extensão, mas também em cabeçalhos de solicitação, parâmetros de cadeia de caracteres de consulta, etc.

2. Uma expressão lambda que leva um `IApplicationBuilder` e adiciona todos o middleware para a ramificação. Isso significa que você pode adicionar middleware adicional para a ramificação na frente do seu manipulador middleware.

Middleware adicionados ao pipeline antes da ramificação será invocada em todas as solicitações; a ramificação não terá impacto sobre eles.

## <a name="loading-middleware-options-using-the-options-pattern"></a>Opções de middleware usando o padrão de opções de carregamento

Alguns módulos e manipuladores têm opções de configuração que são armazenadas em *Web. config*. No entanto, no ASP.NET Core um novo modelo de configuração é usado no lugar de *Web. config*.

O novo [sistema de configuração](xref:fundamentals/configuration/index) oferece essas opções para resolver isso:

* Injetar diretamente as opções para o middleware, conforme o [próxima seção](#loading-middleware-options-through-direct-injection).

* Use o [padrão de opções](xref:fundamentals/configuration/options):

1.  Crie uma classe para manter suas opções de middleware, por exemplo:

    [!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Options)]

2.  Armazenar os valores de opção

    O sistema de configuração permite que você armazene valores de opção em qualquer local desejado. No entanto, a maioria dos locais use *appSettings. JSON*, portanto, vamos essa abordagem:

    [!code-json[Main](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,14-18)]

    *MyMiddlewareOptionsSection* aqui é um nome de seção. Ele não precisa ser igual ao nome da sua classe de opções.

3. Associe os valores de opção com a classe de opções

    O padrão de opções usa a estrutura de injeção de dependência do ASP.NET Core para associar o tipo de opções (como `MyMiddlewareOptions`) com um `MyMiddlewareOptions` objeto que tem as opções reais.

    Atualização de seu `Startup` classe:

    1.  Se você estiver usando *appSettings. JSON*, adicioná-lo para o construtor de configuração no `Startup` construtor:

      [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Ctor&highlight=5-6)]

    2.  Configure o serviço de opções:

      [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

    3.  Associe as opções de sua classe de opções:

      [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=6-8)]

4.  Inserir as opções para o construtor de middleware. Isso é semelhante a injeção de opções em um controlador.

  [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_MiddlewareWithParams&highlight=4,7,10,15-16)]

  O [UseMiddleware](#http-modules-usemiddleware) método de extensão que adiciona o middleware para o `IApplicationBuilder` cuida de injeção de dependência.

  Isso não é limitado a `IOptions` objetos. Qualquer objeto que requer o middleware pode ser inserido dessa maneira.

## <a name="loading-middleware-options-through-direct-injection"></a>Carregando opções de middleware injeção direto

O padrão de opções tem a vantagem de que ele cria flexível acoplamento entre valores de opções e seus consumidores. Depois que você associou a uma classe de opções com os valores reais de opções, qualquer outra classe pode obter acesso às opções por meio da estrutura de injeção de dependência. Não é necessário para passar em torno de valores de opções.

Isso divide Embora se você quiser usar o mesmo middleware duas vezes, com opções diferentes. Por exemplo um autorização middleware usado em diferentes ramificações, permitindo que diferentes funções. Você não pode associar dois objetos diferentes opções com a classe de opções de um.

A solução é obter os objetos de opções com os valores de opções reais no seu `Startup` classe e passe-os diretamente a cada instância de seu middleware.

1.  Adicionar uma segunda chave para *appSettings. JSON*

    Para adicionar um segundo conjunto de opções para o *appSettings. JSON* de arquivo, use uma nova chave para identificá-lo exclusivamente:

    [!code-json[Main](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,10-18&highlight=2-5)]

2.  Recuperar valores de opções e os transmite para o middleware. O `Use...` método de extensão (o que adiciona o middleware no pipeline) é um local lógico para transmitir os valores de opção: 

    [!code-csharp[Main](http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=20-23)]

4.  Habilite middleware para utilizar um parâmetro de opções. Forneça uma sobrecarga do `Use...` método de extensão (que usa o parâmetro options e passá-lo para `UseMiddleware`). Quando `UseMiddleware` é chamado com parâmetros, ele passa os parâmetros para o construtor de middleware quando ele cria uma instância do objeto de middleware.

    [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Extensions&highlight=9-14)]

    Observe como isso encapsula o objeto de opções em um `OptionsWrapper` objeto. Isso implementa `IOptions`, conforme o esperado pelo construtor de middleware.

## <a name="migrating-to-the-new-httpcontext"></a>Migrando para o novo HttpContext

Anteriormente, você viu que o `Invoke` método no seu middleware usa um parâmetro de tipo `HttpContext`:

```csharp
public async Task Invoke(HttpContext context)
```

`HttpContext`foi alterado significativamente no núcleo do ASP.NET. Esta seção mostra como converter as propriedades mais usadas de [System.Web.HttpContext](https://docs.microsoft.com/dotnet/api/system.web.httpcontext) para o novo `Microsoft.AspNetCore.Http.HttpContext`.

### <a name="httpcontext"></a>HttpContext

**HttpContext** se traduz em:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Items)]

**ID da solicitação exclusiva (nenhum equivalente System.Web.HttpContext)**

Fornece uma id exclusiva para cada solicitação. Muito útil para incluir nos logs.

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Trace)]

### <a name="httpcontextrequest"></a>HttpContext.Request

**HttpContext.Request.HttpMethod** se traduz em:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Method)]

**HttpContext.Request.QueryString** se traduz em:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Query)]

**HttpContext.Request.Url** e **HttpContext.Request.RawUrl** traduzir para:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Url)]

**HttpContext.Request.IsSecureConnection** se traduz em:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Secure)]

**HttpContext.Request.UserHostAddress** se traduz em:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Host)]

**HttpContext.Request.Cookies** se traduz em:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Cookies)]

**HttpContext.Request.RequestContext.RouteData** se traduz em:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Route)]

**HttpContext.Request.Headers** se traduz em:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Headers)]

**HttpContext.Request.UserAgent** se traduz em:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Agent)]

**HttpContext.Request.UrlReferrer** se traduz em:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Referrer)]

**HttpContext.Request.ContentType** se traduz em:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Type)]

**HttpContext.Request.Form** se traduz em:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Form)]

> [!WARNING]
> Valores de formulário de leitura somente se o tipo de conteúdo sub é *x-www-form-urlencoded* ou *dados do formulário*.

**HttpContext.Request.InputStream** se traduz em:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Input)]

> [!WARNING]
> Use esse código somente em um middleware de tipo de manipulador, no final de um pipeline.
>
>Você pode ler o corpo bruto conforme mostrado acima apenas uma vez por solicitação. Middleware tentar ler o corpo após a primeira leitura lê um corpo vazio.
>
>Isso não se aplica a leitura de um formulário como mostrado anteriormente, porque isso é feito de um buffer.

### <a name="httpcontextresponse"></a>HttpContext.Response

**HttpContext.Response.Status** e **HttpContext.Response.StatusDescription** traduzir para:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Status)]

**HttpContext.Response.ContentEncoding** e **HttpContext.Response.ContentType** traduzir para:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespType)]

**HttpContext.Response.ContentType** em seu próprio também se traduz em:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespTypeOnly)]

**HttpContext.Response.Output** se traduz em:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Output)]

**HttpContext.Response.TransmitFile**

Serviços de um arquivo é discutida [aqui](../fundamentals/request-features.md#middleware-and-request-features).

**HttpContext.Response.Headers**

Enviar cabeçalhos de resposta é complicada pelo fato de que se você defini-las depois que nada foi gravado no corpo da resposta, ele não serão enviados.

A solução é definir um método de retorno de chamada que será chamado direita antes da gravação do início da resposta. Isso é feito melhor no início do `Invoke` método no seu middleware. É desse método de retorno de chamada que define os cabeçalhos de resposta.

O código a seguir define um método de retorno de chamada chamado `SetHeaders`:

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

O `SetHeaders` método de retorno de chamada terá esta aparência:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetHeaders)]

**HttpContext.Response.Cookies**

Cookies de viagem para o navegador em um *Set-Cookie* cabeçalho de resposta. Como resultado, envio de cookies requer o retorno de chamada mesmo usada para enviar cabeçalhos de resposta:

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetCookies, state: httpContext);
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

O `SetCookies` método de retorno de chamada deve ser semelhante ao seguinte:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetCookies)]

## <a name="additional-resources"></a>Recursos adicionais

* [Visão geral de módulos HTTP e de manipuladores HTTP](https://docs.microsoft.com/iis/configuration/system.webserver/)

* [Configuração](xref:fundamentals/configuration/index)

* [Inicialização de aplicativos](../fundamentals/startup.md)

* [Middleware](../fundamentals/middleware.md)
