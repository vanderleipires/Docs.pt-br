---
title: Middleware ASP.NET Core
author: rick-anderson
description: "Saiba mais sobre o ASP.NET Core middleware e o pipeline de solicitação."
ms.author: riande
manager: wpickett
ms.date: 01/22/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/middleware
ms.openlocfilehash: ef130e736e2f32fa134156d979ce5bfbedcae828
ms.sourcegitcommit: 3f491f887074310fc0f145cd01a670aa63b969e3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/22/2018
---
# <a name="aspnet-core-middleware-fundamentals"></a>Conceitos básicos de Middleware do ASP.NET Core

<a name="fundamentals-middleware"></a>

Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Steve Smith](https://ardalis.com/)

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-middleware"></a>O que é middleware?

Middleware é um software que é gerado em um pipeline de aplicativo para lidar com solicitações e respostas. Cada componente:

* Escolhe se deseja transmitir a solicitação para o próximo componente no pipeline.
* Pode executar o trabalho antes e após o próximo componente de pipeline é invocado. 

Solicitação delegados são usados para criar o pipeline de solicitação. Os delegados de solicitação lidar com cada solicitação HTTP.

Solicitar delegados são configurados usando [executar](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions), [mapa](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions), e [Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) métodos de extensão. Um delegado de solicitação individual pode ser especificado em linha como um método anônimo (chamado de middleware em linha) ou ela pode ser definida em uma classe reutilizável. Essas classes reutilizáveis e métodos anônimos em linha são *middleware*, ou *componentes de middleware*. Cada componente de middleware no pipeline de solicitação é responsável por chamar o próximo componente no pipeline ou curto-circuito da cadeia, se apropriado.

[Migrando módulos HTTP Middleware](../migration/http-modules.md) explica a diferença entre pipelines de solicitação no núcleo do ASP.NET e as versões anteriores e fornece mais exemplos de middleware.

## <a name="creating-a-middleware-pipeline-with-iapplicationbuilder"></a>Criando um pipeline de middleware com IApplicationBuilder

O pipeline de solicitação do ASP.NET Core consiste em uma sequência de representantes de solicitação, chamado um após o outro, como este diagrama mostra (o thread de execução segue as setas pretas):

![Padrão de processamento de solicitação mostra uma solicitação que chegam, por meio de três middlewares e a resposta sair do aplicativo de processamento. Cada middleware executa sua lógica e transmite a solicitação para o próximo middleware na instrução Next (). Depois que o middleware terceiro processa a solicitação, ele é lado através de duas middlewares anteriores para processamento adicional após as instruções Next () por sua vez antes de sair do aplicativo como uma resposta ao cliente.](middleware/_static/request-delegate-pipeline.png)

Cada representante pode executar operações antes e após o próximo delegado. Um delegado também pode optar por não passar uma solicitação para o próximo delegado, que é chamado de curto-circuito do pipeline de solicitação. Curto-circuito geralmente é desejável, porque ela evita trabalho desnecessário. Por exemplo, o middleware de arquivo estático pode retornar uma solicitação para um arquivo estático e o restante do pipeline de curto-circuito. Delegados de tratamento de exceção precisam ser chamado no início do pipeline, para que podem detectar exceções que ocorrem em etapas posteriores do pipeline.

O aplicativo do ASP.NET Core possíveis mais simples define um delegado de solicitação única que manipula todas as solicitações. Neste caso, não inclui um pipeline de solicitação real. Em vez disso, uma única função anônima é chamada em resposta a cada solicitação HTTP.

[!code-csharp[Main](middleware/sample/Middleware/Startup.cs)]

A primeira [aplicativo. Executar](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) delegado encerra o pipeline.

É possível encadear vários representantes de solicitação junto com [aplicativo. Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions). O `next` parâmetro representa o delegado Avançar no pipeline. (Lembre-se de que você pode curto-circuito do pipeline por *não* chamando o *próximo* parâmetro.) Você normalmente pode executar ações antes e após o próximo representante, conforme este exemplo demonstra:

[!code-csharp[Main](middleware/sample/Chain/Startup.cs?name=snippet1)]

>[!WARNING]
> Não chame `next.Invoke` depois que a resposta foi enviada ao cliente. Altera para `HttpResponse` depois que a resposta foi iniciada lançará uma exceção. Por exemplo, as alterações, como definição de cabeçalhos, etc., o código de status lançará uma exceção. Gravar o corpo da resposta após a chamada `next`:
> - Pode causar uma violação do protocolo. Por exemplo, gravar mais do que o indicado `content-length`.
> - Pode corromper o formato do corpo. Por exemplo, gravando um rodapé HTML em um arquivo CSS.
>
> [HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) é uma dica útil para indicar se cabeçalhos terem sido enviados e/ou o corpo foi gravado.

## <a name="ordering"></a>Ordenando

A ordem em que foram adicionados a componentes de middleware de `Configure` método define a ordem na qual eles são chamados em solicitações e ordem inversa para a resposta. Essa ordem é crítico para segurança, desempenho e funcionalidade.

O método de configurar (mostrado abaixo) adiciona os seguintes componentes de middleware:

1. Tratamento de exceção/erros
2. Servidor de arquivos estáticos
3. Autenticação
4. MVC

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)


```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseAuthentication();               // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseIdentity();                     // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

-----------

No código acima, `UseExceptionHandler` é o primeiro componente de middleware adicionado ao pipeline, portanto, ele captura todas as exceções que ocorrem em chamadas posteriores.

O middleware de arquivo estático é chamado no início do pipeline para que possa manipular solicitações e sem passar através dos componentes restantes de curto-circuito. Fornece o middleware de arquivo estático **sem** verificações de autorização. Todos os arquivos servidas por ele, incluindo aqueles em *wwwroot*, publicamente disponíveis. Consulte [trabalhando com arquivos estáticos](xref:fundamentals/static-files) para uma abordagem proteger arquivos estáticos.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)


Se a solicitação não é manipulada pelo middleware de arquivo estático, ele é passado para o middleware de identidade (`app.UseAuthentication`), que executa a autenticação. Identidade não curto-circuito solicitações não autenticadas. Embora identidade autentica solicitações, autorização (e rejeição) ocorrem somente após MVC seleciona uma página Razor específico ou controlador e ação.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Se a solicitação não é manipulada pelo middleware de arquivo estático, ele é passado para o middleware de identidade (`app.UseIdentity`), que executa a autenticação. Identidade não curto-circuito solicitações não autenticadas. Embora identidade autentica solicitações, autorização (e rejeição) ocorrem somente após MVC seleciona um controlador específico e a ação.

-----------

O exemplo a seguir demonstra um middleware de ordem em que as solicitações para arquivos estáticos são manipuladas pelo middleware de arquivo estático antes do middleware de compactação de resposta. Arquivos estáticos não são compactados com essa ordenação do middleware. As respostas MVC de [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) podem ser compactados.

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseStaticFiles();         // Static files not compressed
                                  // by middleware.
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

<a name="middleware-run-map-use"></a>

### <a name="use-run-and-map"></a>Use, executar e mapa

Configurar o pipeline HTTP usando `Use`, `Run`, e `Map`. O `Use` método pode curto-circuito do pipeline (ou seja, se ele não chama um `next` delegado de solicitação). `Run`é uma convenção e alguns componentes de middleware podem expor `Run[Middleware]` métodos que são executados no final do pipeline.

`Map*`extensões são usadas como uma convenção de ramificação do pipeline. [Mapa](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) ramificações o pipeline de solicitação com base na correspondência do caminho da solicitação em questão. Se o caminho da solicitação inicia com o caminho especificado, a ramificação é executada.

[!code-csharp[Main](middleware/sample/Chain/StartupMap.cs?name=snippet1)]

A tabela a seguir mostra as solicitações e respostas de `http://localhost:1234` usando o código anterior:

| Solicitação | Resposta |
| --- | --- |
| localhost:1234 | Saudação de mapa não delegado.  |
| localhost:1234/map1 | Teste 1 de mapa |
| localhost:1234/map2 | Teste 2 de mapa |
| localhost:1234/map3 | Saudação de mapa não delegado.  |

Quando `Map` é usado, o segmento de caminho correspondente (s) serão removidos do `HttpRequest.Path` e anexado a `HttpRequest.PathBase` para cada solicitação.

[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) ramificações do pipeline de solicitação com base no resultado do predicado em questão. Nenhum predicado de tipo `Func<HttpContext, bool>` pode ser usado para mapear solicitações para uma nova ramificação do pipeline. No exemplo a seguir, um predicado é usado para detectar a presença de uma variável de cadeia de caracteres de consulta `branch`:

[!code-csharp[Main](middleware/sample/Chain/StartupMapWhen.cs?name=snippet1)]

A tabela a seguir mostra as solicitações e respostas de `http://localhost:1234` usando o código anterior:

| Solicitação | Resposta |
| --- | --- |
| localhost:1234 | Saudação de mapa não delegado.  |
| localhost:1234/?branch=master | Ramificação usada = mestre|

`Map`dá suporte a aninhamento, por exemplo:

```csharp
app.Map("/level1", level1App => {
       level1App.Map("/level2a", level2AApp => {
           // "/level1/level2a"
           //...
       });
       level1App.Map("/level2b", level2BApp => {
           // "/level1/level2b"
           //...
       });
   });
   ```

`Map`pode também corresponder a vários segmentos de uma vez, por exemplo:

 ```csharp
app.Map("/level1/level2", HandleMultiSeg);
```

## <a name="built-in-middleware"></a>Middleware interno

ASP.NET Core é fornecido com os seguintes componentes de middleware, bem como uma descrição da ordem em que eles devem ser adicionados:

| Middleware | Descrição | Pedido |
| ---------- | ----------- | ----- |
| [Autenticação](xref:security/authentication/identity) | Dá suporte à autenticação. | Antes de `HttpContext.User` é necessária. Terminal para retornos de chamada de OAuth. |
| [CORS](xref:security/cors) | Define o compartilhamento de recursos entre origens. | Antes de componentes que usam o CORS. |
| [Diagnóstico](xref:fundamentals/error-handling) | Configura o diagnóstico. | Antes de componentes que geram erros. |
| [ForwardedHeaders/HttpOverrides](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | Encaminha cabeçalhos como proxy para a solicitação atual. | Antes dos componentes que consomem os campos atualizados (exemplos: esquema, Host, ClientIP, método). |
| [Cache de resposta](xref:performance/caching/middleware) | Fornece suporte para as respostas em cache. | Antes de componentes que requerem armazenamento em cache. |
| [Compactação de resposta](xref:performance/response-compression) | Fornece suporte para a compactação de respostas. | Antes de componentes que requerem a compactação. |
| [RequestLocalization](xref:fundamentals/localization) | Fornece suporte de localização. | Antes de componentes importantes da localização. |
| [Roteamento](xref:fundamentals/routing) | Define e restringe as rotas de solicitação. | Terminal de rotas correspondentes. |
| [Sessão](xref:fundamentals/app-state) | Fornece suporte para gerenciar sessões de usuário. | Antes de componentes que requerem a sessão. |
| [Arquivos estáticos](xref:fundamentals/static-files) | Fornece suporte para servir arquivos estáticos e pesquisa no diretório. | Terminal se corresponder a uma solicitação de arquivos. |
| [Regravação de URL](xref:fundamentals/url-rewriting) | Fornece suporte para URLs de regravação e redirecionar solicitações. | Antes dos componentes que consomem a URL. |
| [WebSockets](xref:fundamentals/websockets) | Permite que o protocolo WebSocket. | Antes de componentes que são necessárias para aceitar solicitações de WebSocket. |

<a name="middleware-writing-middleware"></a>

## <a name="writing-middleware"></a>Middleware de gravação

Middleware geralmente é encapsulado em uma classe e exposto com um método de extensão. Considere o middleware a seguir, que define a cultura para a solicitação atual da cadeia de consulta:

[!code-csharp[Main](middleware/sample/Culture/StartupCulture.cs?name=snippet1)]

Observação: O código de exemplo acima é usado para demonstrar a criação de um componente de middleware. Consulte [ globalização e localização](xref:fundamentals/localization) para suporte de localização interna do ASP.NET Core.

Você pode testar o middleware passando na cultura, por exemplo `http://localhost:7997/?culture=no`.

O código a seguir move o delegado de middleware para uma classe:

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddleware.cs)]

O seguinte método de extensão expõe o middleware por meio de [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddlewareExtensions.cs)]

O código a seguir chama o middleware de `Configure`:

[!code-csharp[Main](middleware/sample/Culture/Startup.cs?name=snippet1&highlight=5)]

Middleware deve seguir o [princípio de dependências explícitas](http://deviq.com/explicit-dependencies-principle/) expondo suas dependências em seu construtor. Middleware é construído uma vez por *tempo de vida do aplicativo*. Consulte *dependências por solicitação* abaixo se você precisa compartilhar serviços com middleware dentro de uma solicitação.

Componentes de middleware podem resolver as dependências de injeção de dependência por meio de parâmetros do construtor. [`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary)também pode aceitar parâmetros adicionais diretamente.

### <a name="per-request-dependencies"></a>Dependências por solicitação

Porque o middleware é construído durante a inicialização do aplicativo, não por solicitação, *escopo* serviços de tempo de vida usados pelo middleware construtores não são compartilhados com outros tipos de dependência inseridos durante cada solicitação. Se você deve compartilhar uma *escopo* entre seu middleware e outros tipos de serviço, adicione esses serviços para o `Invoke` assinatura do método. O `Invoke` método pode aceitar parâmetros adicionais que são preenchidos, injeção de dependência. Por exemplo:

```c#
public class MyMiddleware
{
    private readonly RequestDelegate _next;

    public MyMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="resources"></a>Recursos

* [Código de exemplo usado deste documento](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample)
* [Migrando módulos HTTP para Middleware](../migration/http-modules.md)
* [Inicialização de aplicativos](startup.md)
* [Recursos de solicitação](request-features.md)
