---
title: "Estado da sessão e do aplicativo no ASP.NET Core"
author: rick-anderson
description: "Abordagens para preservar o estado do aplicativo e do usuário (sessão) entre solicitações."
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 11/27/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/app-state
ms.openlocfilehash: 7aa200d3612f766ab633ccab807421b9c5393975
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-session-and-application-state-in-aspnet-core"></a>Introdução ao estado da sessão e do aplicativo no ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/) e [Diana LaRose](https://github.com/DianaLaRose)

O HTTP é um protocolo sem estado. Um servidor Web trata cada solicitação HTTP como uma solicitação independente e não mantém valores de usuários de solicitações anteriores. Este artigo aborda diferentes maneiras de preservar o estado de sessão e de aplicativo entre solicitações. 

## <a name="session-state"></a>Estado de sessão

O estado de sessão é um recurso do ASP.NET Core que você pode usar para salvar e armazenar dados de usuário enquanto o usuário navega seu aplicativo Web. Composto por um dicionário ou tabela de hash no servidor, o estado de sessão persiste dados entre solicitações de um navegador. É feito um back up dos dados da sessão em um cache.

O ASP.NET Core mantém o estado de sessão fornecendo ao cliente um cookie que contém a ID da sessão, que é enviada ao servidor com cada solicitação. O servidor usa a ID da sessão para buscar os dados da sessão. Como o cookie da sessão é específico ao navegador, não é possível compartilhar sessões entre navegadores. Cookies da sessão são excluídos somente quando a sessão do navegador termina. Se for recebido um cookie de uma sessão expirada, uma nova sessão que usa o mesmo cookie será criada. 

O servidor mantém uma sessão por um tempo limitado após a última solicitação. Defina o tempo limite da sessão ou use o valor padrão de 20 minutos. O estado de sessão é ideal para armazenar dados do usuário que são específicos a uma sessão específica, mas não precisam ser persistidos permanentemente. Dados são excluídos do repositório de backup ao chamar `Session.Clear` ou quando a sessão expira no armazenamento de dados. O servidor não sabe quando o navegador é fechado ou quando o cookie da sessão é excluído.

> [!WARNING]
> Não armazene dados confidenciais na sessão. O cliente pode não fechar o navegador e limpar o cookie da sessão (e alguns navegadores mantêm cookies de sessão ativos entre janelas diferentes). Além disso, uma sessão pode não ficar restrita a um único usuário; o usuário seguinte pode continuar com a mesma sessão.

O provedor da sessão na memória armazena dados da sessão no servidor local. Caso planeje executar seu aplicativo Web em um farm de servidores, você precisará usar sessões autoadesivas para vincular cada sessão a um servidor específico. A plataforma Sites do Microsoft Azure usa sessões autoadesivas como padrão (Application Request Routing ou ARR). Entretanto, sessões autoadesivas podem afetar a escalabilidade e complicar atualizações de aplicativos Web. Uma opção melhor é usar os caches distribuídos do Redis ou do SQL Server, que não requerem sessões autoadesivas. Para obter mais informações, consulte [Trabalhando com um cache distribuído](xref:performance/caching/distributed). Para obter detalhes sobre como configurar provedores de serviço, consulte [Configurando a sessão](#configuring-session) posteriormente neste artigo.

<a name="temp"></a>
## <a name="tempdata"></a>TempData

O ASP.NET Core MVC expõe a propriedade [TempData](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) em um [controlador](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller?view=aspnetcore-2.0). Essa propriedade armazena dados até eles serem lidos. Os métodos `Keep` e `Peek` podem ser usados para examinar os dados sem exclusão. `TempData` é particularmente útil para redirecionamento em casos em que os dados são necessários para mais de uma única solicitação. `TempData` é implementado por provedores de TempData, por exemplo, usando cookies ou estado de sessão.

<a name="tempdata-providers"></a>
### <a name="tempdata-providers"></a>Provedores de TempData

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

No ASP.NET Core 2.0 e posteriores, o provedor de TempData baseado em cookies é usado por padrão para armazenar TempData em cookies.

Os dados do cookie são codificados com o [Base64UrlTextEncoder](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder?view=aspnetcore-2.0). Como o cookie é criptografado e dividido em partes, o limite de tamanho de cookie único encontrado no ASP.NET Core 1. x não se aplica. Os dados do cookie não são compactados porque a compactação de dados criptografados pode levar a problemas de segurança, como os ataques [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) e [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)). Para obter mais informações sobre o provedor de TempData baseado em cookie, consulte [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

No ASP.NET Core 1.0 e 1.1, o provedor de TempData do estado de sessão é o padrão.

--------------

<a name="choose-temp"></a>
### <a name="choosing-a-tempdata-provider"></a>Escolhendo um provedor de TempData

Escolher um provedor de TempData envolve várias considerações, como:

1. O aplicativo já usa o estado de sessão para outras finalidades? Em caso afirmativo, usar o provedor de TempData do estado de sessão não tem custos adicionais para o aplicativo (além do tamanho dos dados).
2. O aplicativo usa TempData raramente, para quantidades relativamente pequenas de dados (até 500 bytes)? Em caso afirmativo, o provedor de TempData do cookie acrescentará um pequeno custo a cada solicitação que transportar TempData. Caso contrário, o provedor de TempData do estado de sessão pode ser útil para evitar fazer viagens de ida e volta para uma grande quantidade de dados a cada solicitação até que TempData seja consumido.
3. O aplicativo é executado em um web farm (vários servidores)? Nesse caso, nenhuma configuração adicional é necessária para usar o provedor de TempData do cookie.

> [!NOTE]
> A maioria dos clientes da Web (como navegadores da Web) impõem limites quanto ao tamanho máximo de cada cookie, o número total de cookies ou ambos. Portanto, ao usar o provedor de TempData do cookie, verifique se o aplicativo não ultrapassará esses limites. Considere o tamanho total dos dados, levando em consideração as sobrecargas de criptografia e divisão em partes.

<a name="config-temp"></a>
### <a name="configure-the-tempdata-provider"></a>Configurar o provedor de TempData

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

O provedor de TempData baseado em cookie é habilitado por padrão. O código da classe `Startup` a seguir configura o provedor de TempData baseado em sessão:

[!code-csharp[](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,6,11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

O código da classe `Startup` a seguir configura o provedor de TempData baseado em sessão:

[!code-csharp[](app-state/sample/src/WebAppSession/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,9)]

---

A ordenação é crítica para componentes de middleware. No exemplo anterior, uma exceção do tipo `InvalidOperationException` ocorre quando `UseSession` é invocado após `UseMvcWithDefaultRoute`. Consulte [Ordenação de Middleware](xref:fundamentals/middleware#ordering) para obter mais detalhes.

> [!IMPORTANT]
> Se o alvo for o .NET Framework e o provedor baseado em sessão for usado, adicione o pacote NuGet [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session) ao seu projeto.

## <a name="query-strings"></a>Cadeias de consulta

Você pode passar uma quantidade limitada de dados de uma solicitação para outra adicionando-os à cadeia de caracteres de consulta da nova solicitação. Isso é útil para capturar o estado de uma maneira persistente que permita que links com estado inserido sejam compartilhados por email ou por redes sociais. No entanto, por esse motivo, você nunca deve usar cadeias de consulta para dados confidenciais. Além de serem compartilhadas facilmente, incluir dados em cadeias de consulta pode criar oportunidades para ataques de [CSRF (solicitação intersite forjada)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)), que podem enganar os usuários para que eles visitem sites mal-intencionados enquanto estão autenticados. Invasores podem, então, roubar dados do usuário de seu aplicativo ou executar ações mal-intencionadas em nome do usuário. Qualquer estado de sessão ou aplicativo preservado deve proteger contra ataques CSRF. Para obter mais informações sobre ataques CSRF, consulte [Preventing Cross-Site Request Forgery (XSRF/CSRF) Attacks in ASP.NET Core](../security/anti-request-forgery.md) (Impedindo ataques XSRF/CSRF [solicitação intersite forjada] no ASP.NET Core).

## <a name="post-data-and-hidden-fields"></a>Dados de postagem e campos ocultos

Dados podem ser salvos em campos de formulário ocultos e postados novamente na solicitação seguinte. Isso é comum em formulários com várias páginas. No entanto, como o cliente pode adulterar os dados, o servidor sempre deve validá-lo novamente. 

## <a name="cookies"></a>Cookies

Cookies fornecem uma maneira de armazenar dados específicos do usuário em aplicativos Web. Como os cookies são enviados com cada solicitação, seu tamanho deve ser reduzido ao mínimo. Idealmente, somente um identificador deve ser armazenado em um cookie, com os dados reais armazenados no servidor. A maioria dos navegadores restringe os cookies a 4096 bytes. Além disso, somente um número limitado de cookies está disponível para cada domínio.  

Como cookies estão sujeitos à adulteração, eles devem ser validados no servidor. Embora a durabilidade do cookie em um cliente esteja sujeita à intervenção do usuário e à expiração, normalmente eles são a forma mais durável de persistência de dados no cliente.

Frequentemente, cookies são usados para personalização quando o conteúdo é personalizado para um usuário conhecido. Como na maioria dos casos o usuário é apenas identificado e não autenticado, normalmente você pode proteger um cookie armazenando o nome de usuário, o nome da conta ou uma ID de usuário único (como um GUID) no cookie. É possível, então, usar o cookie para acessar a infraestrutura de personalização de usuários de um site.

## <a name="httpcontextitems"></a>HttpContext.Items

A coleção `Items` é um bom local para armazenar dados que são necessários somente ao processar uma solicitação específica. O conteúdo da coleção é descartado após cada solicitação. A coleção `Items` é melhor usada como uma maneira de componentes ou middleware se comunicarem quando operam em momentos diferentes durante uma solicitação e não têm nenhuma maneira direta de passar parâmetros. Para obter mais informações, consulte [Trabalhando com HttpContext.Items](#working-with-httpcontextitems) posteriormente neste artigo.

## <a name="cache"></a>Cache

O cache é uma maneira eficiente de armazenar e recuperar dados. É possível controlar o tempo de vida dos itens em cache com base na hora e em outras considerações. Saiba mais sobre o [Caching](../performance/caching/index.md).

<a name="session"></a>
## <a name="working-with-session-state"></a>Trabalhando com o estado de sessão

### <a name="configuring-session"></a>Configurando a sessão

O pacote `Microsoft.AspNetCore.Session` fornece middleware para gerenciar o estado de sessão. Para habilitar o middleware da sessão, `Startup` deve conter:

- Qualquer um dos caches de memória [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache). A implementação `IDistributedCache` é usada como um repositório de backup para a sessão.
- Chamada [AddSession](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_), que requer o pacote NuGet "Microsoft.AspNetCore.Session".
- Chamada [UseSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_).

O código a seguir mostra como configurar o provedor de sessão na memória.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](app-state/sample/src/WebAppSessionDotNetCore2.0App/Startup.cs?highlight=11-19,24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](app-state/sample/src/WebAppSession/Startup.cs?highlight=11-19,24)]

---

É possível fazer referência à sessão de `HttpContext` após ele ser instalado e configurado.

Se você tentar acessar `Session` antes que `UseSession` tenha sido chamado, a exceção `InvalidOperationException: Session has not been configured for this application or request` será lançada.

Se você tentar criar um novo `Session` (ou seja, nenhum cookie de sessão foi criado) após já ter começado a gravar no fluxo `Response`, a exceção `InvalidOperationException: The session cannot be established after the response has started` será lançada. A exceção pode ser encontrada no log do servidor Web; ele não será exibido no navegador.

### <a name="loading-session-asynchronously"></a>Carregamento a sessão de forma assíncrona 

O provedor de sessão padrão no ASP.NET Core carrega o registro da sessão do repositório [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) subjacente de forma assíncrona somente se o método [ISession.LoadAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) for chamado explicitamente antes dos métodos `TryGetValue`, `Set` ou `Remove`. Se `LoadAsync` não for chamado primeiro, o registro da sessão subjacente é carregado de forma síncrona, o que poderia afetar a capacidade de dimensionamento do aplicativo.

Para que aplicativos imponham esse padrão, encapsule as implementações [DistributedSessionStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsessionstore) e [DistributedSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsession) com versões que geram uma exceção se o método `LoadAsync` não for chamado antes de `TryGetValue`, `Set` ou `Remove`. Registre as versões encapsuladas no contêiner de serviços.

### <a name="implementation-details"></a>Detalhes da implementação

A sessão usa um cookie para rastrear e identificar solicitações de um único navegador. Por padrão, esse cookie é denominado ". AspNet.Session" e usa um caminho de "/". Como o padrão do cookie não especifica um domínio, ele não fica disponível para o script do lado do cliente na página (porque `CookieHttpOnly` tem `true` como padrão).

Para substituir os padrões da sessão, use `SessionOptions`:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupCopy.cs?name=snippet1&highlight=8-12)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](app-state/sample/src/WebAppSession/StartupCopy.cs?name=snippet1&highlight=8-12)]

---

O servidor usa a propriedade `IdleTimeout` para determinar por quanto tempo uma sessão pode ficar ociosa antes que seu conteúdo seja abandonado. Essa propriedade é independente da expiração do cookie. Cada solicitação passada por meio do middleware de Sessão (lida ou gravada) redefine o tempo limite.

Como `Session` é *sem bloqueio*, se duas solicitações tentarem modificar o conteúdo da sessão, a última delas substituirá a primeira. `Session` é implementado como uma *sessão coerente*, o que significa que todo o conteúdo é armazenado junto. Duas solicitações que estão modificando partes diferentes da sessão (chaves diferentes) ainda podem afetar umas às outras.

### <a name="setting-and-getting-session-values"></a>Configurando e obtendo valores de sessão

A sessão é acessada por meio da propriedade `Session` em `HttpContext`. Esta propriedade é uma implementação de [ISession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession).

O exemplo a seguir mostra a configuração e a obtenção de um int e uma cadeia de caracteres:

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?range=8-27,49)]

Se adicionar os seguintes métodos de extensão, você poderá definir e obter objetos serializáveis da sessão:

[!code-csharp[Main](app-state/sample/src/WebAppSession/Extensions/SessionExtensions.cs)]

O exemplo a seguir mostra como definir e obter um objeto serializável:

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet2)]


## <a name="working-with-httpcontextitems"></a>Trabalhando com HttpContext.Items

A abstração `HttpContext` dá suporte a uma coleção de dicionário do tipo `IDictionary<object, object>`, chamada `Items`. Essa coleção está disponível desde o início de um *HttpRequest* e é descartada no final de cada solicitação. Você pode acessá-la atribuindo um valor a uma entrada com chave ou solicitando o valor de uma determinada chave.

No exemplo abaixo, [Middleware](middleware.md) adiciona `isVerified` à coleção `Items`.

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

Posteriormente no pipeline, outro middleware poderia acessá-la:

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync("Verified request? " + 
        context.Items["isVerified"]);
});
```

Para middleware que será usado apenas por um único aplicativo, chaves `string` são aceitáveis. No entanto, middleware que será compartilhado entre aplicativos deve usar chaves de objeto exclusivas para evitar qualquer possibilidade de colisões de chaves. Se estiver desenvolvendo middleware que precisa funcionar em vários aplicativos, use uma chave de objeto exclusiva definida na classe do middleware, como mostrado abaixo:

```csharp
public class SampleMiddleware
{
    public static readonly object SampleKey = new Object();

    public async Task Invoke(HttpContext httpContext)
    {
        httpContext.Items[SampleKey] = "some value";
        // additional code omitted
    }
}
```

Outros códigos podem acessar o valor armazenado em `HttpContext.Items` usando a chave exposta pela classe do middleware:

```csharp
public class HomeController : Controller
{
    public IActionResult Index()
    {
        string value = HttpContext.Items[SampleMiddleware.SampleKey];
    }
}
```

Essa abordagem também tem a vantagem de eliminar a repetição de "cadeias de caracteres mágicas" em vários locais no código.

<a name="appstate-errors"></a>

## <a name="application-state-data"></a>Dados de estado do aplicativo

Use a [Injeção de dependência](xref:fundamentals/dependency-injection) para disponibilizar dados para todos os usuários:

1. Defina um serviço que contém os dados (por exemplo, uma classe chamada `MyAppData`).

```csharp
public class MyAppData
{
    // Declare properties/methods/etc.
} 
```
2. Adicione a classe de serviço a `ConfigureServices` (por exemplo, `services.AddSingleton<MyAppData>();`).
3. Consuma a classe do serviço de dados em cada controlador:

```csharp
public class MyController : Controller
{
    public MyController(MyAppData myService)
    {
        // Do something with the service (read some data from it, 
        // store it in a private field/property, etc.)
    }
} 
```

## <a name="common-errors-when-working-with-session"></a>Erros comuns ao trabalhar com a sessão

* "Não é possível resolver o serviço para o tipo 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' ao tentar ativar 'Microsoft.AspNetCore.Session.DistributedSessionStore'."

  Geralmente, isso é causado quando não é configurada pelo menos uma implementação de `IDistributedCache`. Para obter mais informações, consulte [Trabalhando com um cache distribuído](xref:performance/caching/distributed) e [Cache de memória](xref:performance/caching/memory).

* Caso o middleware da sessão não consiga persistir uma sessão (por exemplo, se o banco de dados não estiver disponível), ele registrará a exceção e a ignorará. A solicitação continuará normalmente, o que leva a um comportamento muito imprevisível.

Um exemplo típico:

Alguém armazena um carrinho de compras na sessão. O usuário adiciona um item, mas a confirmação falha. O aplicativo não sabe sobre a falha e exibe a mensagem "O item foi adicionado", o que não é verdadeiro.

A maneira recomendada de verificar esses erros é chamar `await feature.Session.CommitAsync();` do código do aplicativo quando você terminar de gravar a sessão. Em seguida, você pode fazer o que quiser com o erro. Isso funciona da mesma forma ao chamar `LoadAsync`.

### <a name="additional-resources"></a>Recursos adicionais

* [ASP.NET Core 1. x: código de exemplo usado neste documento](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSession)
* [ASP.NET Core 2. x: código de exemplo usado neste documento](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSessionDotNetCore2.0App)
