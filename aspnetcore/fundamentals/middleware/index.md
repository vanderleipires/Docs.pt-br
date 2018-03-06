---
title: Middleware do ASP.NET Core
author: rick-anderson
description: "Saiba mais sobre o middleware do ASP.NET Core e o pipeline de solicitação."
manager: wpickett
ms.author: riande
ms.date: 01/22/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/middleware/index
ms.openlocfilehash: 5d236c79120d79195c1970cc87d164002b56d0f1
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/02/2018
---
# <a name="aspnet-core-middleware"></a>Middleware do ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Steve Smith](https://ardalis.com/)

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/index/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-middleware"></a>O que é um middleware?

O middleware é um software montado em um pipeline de aplicativo para manipular solicitações e respostas. Cada componente:

* Escolhe se deseja passar a solicitação para o próximo componente no pipeline.
* Pode executar o trabalho antes e depois de o próximo componente no pipeline ser invocado. 

Os delegados de solicitação são usados para criar o pipeline de solicitação. Os delegados de solicitação manipulam cada solicitação HTTP.

Os delegados de solicitação são configurados usando os métodos de extensão [Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions), [Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) e [Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions). Um delegado de solicitação individual pode ser especificado em linha como um método anônimo (chamado do middleware em linha) ou pode ser definido em uma classe reutilizável. Essas classes reutilizáveis e os métodos anônimos em linha são o *middleware* ou *componentes do middleware*. Cada componente do middleware no pipeline de solicitação é responsável por invocar o próximo componente no pipeline ou por ligar a cadeia em curto-circuito, se apropriado.

A seção [Migrating HTTP Modules to Middleware](xref:migration/http-modules) (Migrando módulos HTTP para o Middleware) explica a diferença entre pipelines de solicitação no ASP.NET Core e no ASP.NET 4.x e fornece mais exemplos do middleware.

## <a name="creating-a-middleware-pipeline-with-iapplicationbuilder"></a>Criando um pipeline do middleware com o IApplicationBuilder

O pipeline de solicitação do ASP.NET Core consiste em uma sequência de delegados de solicitação, chamados um após o outro, como mostrado neste diagrama (o thread de execução segue as setas pretas):

![Padrão de processamento de solicitação mostrando a chegada de uma solicitação, processada por meio de três middlewares e a resposta que sai do aplicativo. Cada middleware executa sua lógica e transmite a solicitação para o próximo middleware na instrução next(). Depois que o terceiro middleware processa a solicitação, ela é transmitida por meio dos dois middlewares anteriores na ordem inversa para processamento adicional após suas instruções next(), antes de deixar o aplicativo como uma resposta ao cliente.](index/_static/request-delegate-pipeline.png)

Cada delegado pode executar operações antes e depois do próximo delegado. Um delegado também pode optar por não transmitir uma solicitação ao próximo delegado, o que também é chamado de ligar o pipeline de solicitação em curto-circuito. O curto-circuito geralmente é desejável porque ele evita trabalho desnecessário. Por exemplo, o middleware de arquivo estático pode retornar uma solicitação para um arquivo estático e ligar o restante do pipeline em curto-circuito. Os delegados de tratamento de exceção precisam ser chamados no início do pipeline, para que possam detectar exceções que ocorrem em etapas posteriores do pipeline.

O aplicativo ASP.NET Core mais simples possível define um delegado de solicitação única que controla todas as solicitações. Este caso não inclui um pipeline de solicitação real. Em vez disso, uma única função anônima é chamada em resposta a cada solicitação HTTP.

[!code-csharp[](index/sample/Middleware/Startup.cs)]

O primeiro delegado [app.Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) encerra o pipeline.

É possível encadear vários delegados de solicitação junto com o [app.Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions). O parâmetro `next` representa o próximo delegado no pipeline. (Lembre-se de que você pode ligar o pipeline em curto-circuito ao *não* chamar o *próximo* parâmetro.) Normalmente, você pode executar ações antes e depois do próximo delegado, conforme este exemplo demonstra:

[!code-csharp[](index/sample/Chain/Startup.cs?name=snippet1)]

>[!WARNING]
> Não chame `next.Invoke` depois que a resposta tiver sido enviada ao cliente. Altera para `HttpResponse` depois que a resposta foi iniciada e lançará uma exceção. Por exemplo, mudanças como a configuração de cabeçalhos, o código de status, etc., lançarão uma exceção. Gravar no corpo da resposta após a chamada `next`:
> - Pode causar uma violação do protocolo. Por exemplo, gravar mais do que o `content-length` indicado.
> - Pode corromper o formato do corpo. Por exemplo, gravar um rodapé HTML em um arquivo CSS.
>
> [HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) é uma dica útil para indicar se os cabeçalhos foram enviados e/ou o corpo foi gravado.

## <a name="ordering"></a>Ordenando

A ordem em que os componentes do middleware são adicionados ao método `Configure` define a ordem em que eles são invocados nas solicitações e a ordem inversa para a resposta. Essa ordem é crítica para a segurança, o desempenho e a funcionalidade.

O método Configure (mostrado abaixo) adiciona os seguintes componentes de middleware:

1. Exceção/tratamento de erro
2. Servidor de arquivos estático
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

No código acima, `UseExceptionHandler` é o primeiro componente de middleware adicionado ao pipeline—portanto, ele captura qualquer exceção que ocorra em chamadas posteriores.

O middleware de arquivo estático é chamado no início do pipeline para que possa controlar as solicitações e o curto-circuito sem passar pelos componentes restantes. O middleware de arquivo estático não fornece **nenhuma** verificação de autorização. Todos os arquivos atendidos, incluindo aqueles em *wwwroot*, estão disponíveis publicamente. Consulte [Trabalhando com arquivos estáticos](xref:fundamentals/static-files) para conhecer uma abordagem para proteger arquivos estáticos.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)


Se a solicitação não for controlada pelo middleware de arquivo estático, ela será transmitida para o middleware de identidade (`app.UseAuthentication`), que executa a autenticação. A identidade não liga as solicitações não autenticadas em curto-circuito. Embora a identidade autentique as solicitações, a autorização (e a rejeição) ocorre somente depois que o MVC seleciona uma Página Razor específica ou um controlador e uma ação.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Se a solicitação não for controlada pelo middleware de arquivo estático, ela será transmitida para o middleware de identidade (`app.UseIdentity`), que executa a autenticação. A identidade não liga as solicitações não autenticadas em curto-circuito. Embora a identidade autentique as solicitações, a autorização (e a rejeição) ocorre somente depois que o MVC seleciona um controlador específico e uma ação.

-----------

O exemplo a seguir demonstra uma solicitação de middleware na qual os pedidos de arquivos estáticos são tratados pelo middleware de arquivo estático antes do middleware de compactação de resposta. Os arquivos estáticos não são compactados com esse pedido de middleware. As respostas do MVC de [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) podem ser compactadas.

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

### <a name="use-run-and-map"></a>Use, Run e Map

Você pode configurar o pipeline de HTTP usando `Use`, `Run` e `Map`. O método `Use` pode ligar o pipeline em curto-circuito (ou seja, se ele não chamar um delegado de solicitação `next`). `Run` é uma convenção e alguns componentes de middleware podem expor os métodos `Run[Middleware]` que são executados no final do pipeline.

As extensões `Map*` são usadas como uma convenção de ramificação do pipeline. [Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) ramifica o pipeline de solicitação com base na correspondência do caminho da solicitação em questão. Se o caminho da solicitação iniciar com o caminho especificado, o branch será executado.

[!code-csharp[](index/sample/Chain/StartupMap.cs?name=snippet1)]

A tabela a seguir mostra as solicitações e as respostas de `http://localhost:1234` usando o código anterior:

| Solicitação | Resposta |
| --- | --- |
| localhost:1234 | Saudação do delegado diferente de Map.  |
| localhost:1234/map1 | Teste de Map 1 |
| localhost:1234/map2 | Teste de Map 2 |
| localhost:1234/map3 | Saudação do delegado diferente de Map.  |

Quando `Map` é usado, os segmentos de caminho correspondentes são removidos do `HttpRequest.Path` e anexados em `HttpRequest.PathBase` para cada solicitação.

[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) ramifica o pipeline de solicitação com base no resultado do predicado em questão. Qualquer predicado do tipo `Func<HttpContext, bool>` pode ser usado para mapear as solicitações para um novo branch do pipeline. No exemplo a seguir, um predicado é usado para detectar a presença de uma variável de cadeia de caracteres de consulta `branch`:

[!code-csharp[](index/sample/Chain/StartupMapWhen.cs?name=snippet1)]

A tabela a seguir mostra as solicitações e as respostas de `http://localhost:1234` usando o código anterior:

| Solicitação | Resposta |
| --- | --- |
| localhost:1234 | Saudação do delegado diferente de Map.  |
| localhost:1234/?branch=master | Branch usado = mestre|

`Map` é compatível com aninhamento, por exemplo:

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

`Map` também pode ser correspondido com vários segmentos de uma vez, por exemplo:

 ```csharp
app.Map("/level1/level2", HandleMultiSeg);
```

## <a name="built-in-middleware"></a>Middleware interno

O ASP.NET Core é fornecido com os componentes de middleware a seguir, bem como com uma descrição da ordem em que eles devem ser adicionados:

| Middleware | Descrição | Pedido |
| ---------- | ----------- | ----- |
| [Autenticação](xref:security/authentication/identity) | Fornece suporte à autenticação. | Antes de `HttpContext.User` ser necessário. Terminal para retornos de chamada OAuth. |
| [CORS](xref:security/cors) | Configura o Compartilhamento de Recursos entre Origens. | Antes de componentes que usam o CORS. |
| [Diagnóstico](xref:fundamentals/error-handling) | Configura o diagnóstico. | Antes dos componentes que geram erros. |
| [ForwardedHeaders/HttpOverrides](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | Encaminha cabeçalhos como proxy para a solicitação atual. | Antes dos componentes que consomem os campos atualizados (exemplos: Esquema, Host, ClientIP, Método). |
| [Cache de resposta](xref:performance/caching/middleware) | Fornece suporte para as respostas em cache. | Antes dos componentes que exigem armazenamento em cache. |
| [Compactação de resposta](xref:performance/response-compression) | Fornece suporte para a compactação de respostas. | Antes dos componentes que exigem compactação. |
| [RequestLocalization](xref:fundamentals/localization) | Fornece suporte à localização. | Antes dos componentes de localização importantes. |
| [Roteamento](xref:fundamentals/routing) | Define e restringe as rotas de solicitação. | Terminal de rotas correspondentes. |
| [Sessão](xref:fundamentals/app-state) | Fornece suporte para gerenciar sessões de usuário. | Antes de componentes que exigem a sessão. |
| [Arquivos estáticos](xref:fundamentals/static-files) | Fornece suporte para servir arquivos estáticos e pesquisa no diretório. | Terminal, se uma solicitação for correspondente aos arquivos. |
| [Regravação de URL ](xref:fundamentals/url-rewriting) | Fornece suporte para regravar URLs e redirecionar solicitações. | Antes dos componentes que consomem a URL. |
| [WebSockets](xref:fundamentals/websockets) | Habilita o protocolo WebSockets. | Antes dos componentes que são necessários para aceitar solicitações de WebSocket. |

<a name="middleware-writing-middleware"></a>

## <a name="writing-middleware"></a>Middleware de gravação

O middleware geralmente é encapsulado em uma classe e exposto com um método de extensão. Considere o middleware a seguir, que define a cultura para a solicitação atual da cadeia de caracteres de consulta:

[!code-csharp[](index/sample/Culture/StartupCulture.cs?name=snippet1)]

Observação: o código de exemplo acima é usado para demonstrar a criação de um componente de middleware. Consulte [ Globalização e localização](xref:fundamentals/localization) para obter suporte de localização interna do ASP.NET Core.

Você pode testar o middleware ao transmitir a cultura, por exemplo `http://localhost:7997/?culture=no`.

O código a seguir move o delegado de middleware para uma classe:

[!code-csharp[](index/sample/Culture/RequestCultureMiddleware.cs)]

O seguinte método de extensão expõe o middleware por meio do [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):

[!code-csharp[](index/sample/Culture/RequestCultureMiddlewareExtensions.cs)]

O código a seguir chama o middleware de `Configure`:

[!code-csharp[](index/sample/Culture/Startup.cs?name=snippet1&highlight=5)]

O middleware deve seguir o [princípio de dependências explícitas](http://deviq.com/explicit-dependencies-principle/) ao expor suas dependências em seu construtor. O middleware é construído uma vez por *tempo de vida do aplicativo*. Consulte a seção *Dependências por solicitação* abaixo se você precisar compartilhar serviços com middleware dentro de uma solicitação.

Os componentes de middleware podem resolver suas dependências, utilizando a injeção de dependência por meio de parâmetros do construtor. [`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary) também pode aceitar parâmetros adicionais diretamente.

### <a name="per-request-dependencies"></a>Dependências de pré-solicitação

Uma vez que o middleware é construído durante a inicialização do aplicativo, e não por solicitação, os serviços de tempo de vida *com escopo* usados pelos construtores do middleware não são compartilhados com outros tipos de dependência inseridos durante cada solicitação. Se você tiver que compartilhar um serviço *com escopo* entre seu serviço de middleware e serviços de outros tipos, adicione esses serviços à assinatura do método `Invoke`. O método `Invoke` pode aceitar parâmetros adicionais que são preenchidos pela injeção de dependência. Por exemplo:

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

## <a name="additional-resources"></a>Recursos adicionais

* [Migrando módulos HTTP para Middleware](xref:migration/http-modules)
* [Inicialização de aplicativos](xref:fundamentals/startup)
* [Recursos de solicitação](xref:fundamentals/request-features)
* [Ativação de middleware de fábrica](xref:fundamentals/middleware/extensibility)
* [Ativação de middleware com um contêiner de terceiros](xref:fundamentals/middleware/extensibility-third-party-container)
