---
title: Middleware de Reconfiguração de URL no ASP.NET Core
author: guardrex
description: Saiba mais sobre a reconfiguração de URL e o redirecionamento com o Middleware de Reconfiguração de URL em aplicativos ASP.NET Core.
ms.author: riande
ms.date: 08/17/2017
uid: fundamentals/url-rewriting
ms.openlocfilehash: 5a1891c838436467fb49ff6288587fab08201179
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207180"
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a>Middleware de Reconfiguração de URL no ASP.NET Core

Por [Luke Latham](https://github.com/guardrex) e [Mikael Mengistu](https://github.com/mikaelm12)

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/) ([como baixar](xref:index#how-to-download-a-sample))

A reconfiguração de URL é o ato de modificar URLs de solicitação com base em uma ou mais regras predefinidas. A reconfiguração de URL cria uma abstração entre locais de recursos e seus endereços, de forma que os locais e endereços não estejam totalmente vinculados. Há vários cenários em que a reconfiguração de URL é útil:

* Mover ou substituir recursos de servidor temporária ou permanentemente mantendo localizadores estáveis para esses recursos.
* Dividir o processamento de solicitação entre diferentes aplicativos ou áreas de um aplicativo.
* Remover, adicionar ou reorganizar segmentos de URL em solicitações de entrada.
* Otimizar URLs públicas para SEO (Otimização do Mecanismo de Pesquisa).
* Permitir o uso de URLs públicas amigáveis para ajudar as pessoas a prever o conteúdo que encontrarão seguindo um link.
* Redirecionar solicitações não seguras para pontos de extremidade seguros.
* Impedir hotlinking de imagem.

Defina regras para alterar a URL de várias maneiras, incluindo Regex, regras do módulo mod_rewrite do Apache, regras do Módulo de Reconfiguração do IIS e uso de uma lógica de regra personalizada. Este documento apresenta a reconfiguração de URL com instruções sobre como usar o Middleware de Reconfiguração de URL em aplicativos ASP.NET Core.

> [!NOTE]
> A reconfiguração de URL pode reduzir o desempenho de um aplicativo. Sempre que possível, você deve limitar o número e a complexidade de regras.

## <a name="url-redirect-and-url-rewrite"></a>Redirecionamento e reconfiguração de URL

A diferença entre os termos *redirecionamento de URL* e *reconfiguração de URL* pode parecer sutil no primeiro momento, mas tem implicações importantes no fornecimento de recursos aos clientes. O Middleware de Reconfiguração de URL do ASP.NET Core pode atender às necessidades de ambos.

Um *redirecionamento de URL* é uma operação do lado do cliente, na qual o cliente é instruído a acessar um recurso em outro endereço. Isso exige a ida e vinda para o servidor. A URL de redirecionamento retornada para o cliente é exibida na barra de endereços do navegador quando o cliente faz uma nova solicitação para o recurso. 

Se `/resource` é *redirecionado* para `/different-resource`, o cliente solicita `/resource`. O servidor responde que o cliente deve obter o recurso em `/different-resource` com um código de status que indica que o redirecionamento é temporário ou permanente. O cliente executa uma nova solicitação para o recurso na URL de redirecionamento.

![Um ponto de extremidade de serviço WebAPI foi alterado temporariamente da versão 1 (v1) para a versão 2 (v2) no servidor. Um cliente faz uma solicitação para o serviço no caminho /v1/api da versão 1. O servidor envia novamente uma resposta 302 (Encontrado) com o novo caminho temporário para o serviço em /v2/api da versão 2. O cliente faz uma segunda solicitação para o serviço na URL de redirecionamento. O servidor responde com um código de status 200 (OK).](url-rewriting/_static/url_redirect.png)

Ao redirecionar solicitações para uma URL diferente, indique se o redirecionamento é permanente ou temporário. O código de status 301 (Movido Permanentemente) é usado quando o recurso tem uma nova URL permanente e você deseja instruir o cliente que todas as solicitações futuras para o recurso devem usar a nova URL. *O cliente pode armazenar a resposta em cache quando um código de status 301 é recebido.* O código de status 302 (Encontrado) é usado quando o redirecionamento é temporário ou geralmente sujeito à alteração, como aquele em que o cliente não deve armazenar e reutilizar a URL de redirecionamento no futuro. Para obter mais informações, consulte [RFC 2616: definições de código de status](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).

Uma *reconfiguração de URL* é uma operação do servidor usada para fornecer um recurso de outro endereço de recurso. A reconfiguração de uma URL não exige a ida e vinda para o servidor. A URL reconfigurada não é retornada para o cliente e não será exibida na barra de endereços do navegador. Quando `/resource` é *reconfigurado* para `/different-resource`, o cliente solicita `/resource` e o servidor busca *internamente* o recurso em `/different-resource`. Embora o cliente possa ter a capacidade de recuperar o recurso na URL reconfigurada, o cliente não será informado de que o recurso existe na URL reconfigurada quando fizer a solicitação e receber a resposta.

![Um ponto de extremidade de serviço WebAPI foi alterado da versão 1 (v1) para a versão 2 (v2) no servidor. Um cliente faz uma solicitação para o serviço no caminho /v1/api da versão 1. A URL de solicitação foi reconfigurada para acessar o serviço no caminho /v2/api da versão 2. O serviço responde ao cliente com um código de status 200 (OK).](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a>Aplicativo de exemplo de reconfiguração de URL

Explore os recursos do Middleware de Reconfiguração de URL com o [aplicativo de exemplo de reconfiguração de URL](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/). O aplicativo aplica as regras de reconfiguração e redirecionamento e mostra a URL reconfigurada ou redirecionada.

## <a name="when-to-use-url-rewriting-middleware"></a>Quando usar o Middleware de Reconfiguração de URL

Use o Middleware de Reconfiguração de URL quando não puder usar o [módulo de Reconfiguração de URL](https://www.iis.net/downloads/microsoft/url-rewrite) com o IIS no Windows Server, o [módulo mod_rewrite do Apache](https://httpd.apache.org/docs/2.4/rewrite/) no Apache Server, a [reconfiguração de URL no Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/) ou quando o aplicativo estiver hospedado no [servidor HTTP.sys](xref:fundamentals/servers/httpsys) (anteriormente chamado [WebListener](xref:fundamentals/servers/weblistener)). As principais razões para o uso das tecnologias de reconfiguração de URL baseadas em servidor no IIS, Apache ou Nginx são que o middleware não dá suporte aos recursos completos desses módulos e o desempenho do middleware provavelmente não corresponderá ao desempenho dos módulos. No entanto, há alguns recursos dos módulos de servidor que não funcionam com projetos do ASP.NET Core, como as restrições `IsFile` e `IsDirectory` do módulo de Reconfiguração de IIS. Nesses cenários, use o middleware.

## <a name="package"></a>Pacote

Para incluir o middleware em seu projeto, adicione uma referência ao pacote [`Microsoft.AspNetCore.Rewrite`](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/). Esse recurso está disponível para aplicativos direcionados ao ASP.NET Core 1.1 ou posterior.

## <a name="extension-and-options"></a>Extensão e opções

Estabeleça as regras de reconfiguração e redirecionamento de URL criando uma instância da classe [RewriteOptions](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptions) com métodos de extensão para cada uma das regras. Encadeie várias regras na ordem em que deseja que sejam processadas. As `RewriteOptions` são passadas para o Middleware de Reconfiguração de URL, conforme ele é adicionado ao pipeline de solicitação com `app.UseRewriter(options);`.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddRedirect("redirect-rule/(.*)", "redirected/$1")
        .AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", 
            skipRemainingRules: true)
        .AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")
        .AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")
        .Add(RedirectXMLRequests)
        .Add(new RedirectImageRequests(".png", "/png-images"))
        .Add(new RedirectImageRequests(".jpg", "/jpg-images"));

    app.UseRewriter(options);
}
```

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="redirect-non-www-to-www"></a>Redirecionar não www para www

Três opções permitem que o aplicativo redirecione solicitações não `www`para `www`:

* [AddRedirectToWwwPermanent(RewriteOptions)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowwwpermanent) &ndash; Redireciona permanentemente a solicitação para o subdomínio `www` se a solicitação é não `www`. Redireciona com um código de status [Status308PermanentRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status308permanentredirect).
* [AddRedirectToWww(RewriteOptions)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowww) &ndash; Redireciona a solicitação para o subdomínio `www` se a solicitação de entrada é não `www`. Redireciona com um código de status [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect).
* [AddRedirectToWww(RewriteOptions, Int32)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowww) &ndash; Redireciona a solicitação para o subdomínio `www` se a solicitação de entrada é não `www`. Permite que você forneça o código de status para a resposta. Use os campos da classe [StatusCodes](/dotnet/api/microsoft.aspnetcore.http.statuscodes) para atribuições para `AddRedirectToWww`.

::: moniker-end

### <a name="url-redirect"></a>Redirecionamento de URL

Use `AddRedirect` para redirecionar as solicitações. O primeiro parâmetro contém o regex para correspondência no caminho da URL de entrada. O segundo parâmetro é a cadeia de caracteres de substituição. O terceiro parâmetro, se presente, especifica o código de status. Se você não especificar o código de status, ele usará como padrão 302 (Encontrado), que indica que o recurso foi movido temporariamente ou substituído.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=9)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirect("redirect-rule/(.*)", "redirected/$1");

    app.UseRewriter(options);
}
```

::: moniker-end

Em um navegador com as ferramentas para desenvolvedores habilitadas, faça uma solicitação para o aplicativo de exemplo com o caminho `/redirect-rule/1234/5678`. O regex corresponde ao caminho de solicitação em `redirect-rule/(.*)` e o caminho é substituído por `/redirected/1234/5678`. A URL de redirecionamento é enviada novamente ao cliente com um código de status 302 (Encontrado). O navegador faz uma nova solicitação na URL de redirecionamento, que é exibida na barra de endereços do navegador. Como nenhuma regra no aplicativo de exemplo corresponde à URL de redirecionamento, a segunda solicitação recebe uma resposta 200 (OK) do aplicativo e o corpo da resposta mostra a URL de redirecionamento. Uma ida e vinda é feita para o servidor quando uma URL é *redirecionada*.

> [!WARNING]
> Tenha cuidado ao estabelecer as regras de redirecionamento. As regras de redirecionamento são avaliadas em cada solicitação para o aplicativo, incluindo após um redirecionamento. É fácil criar acidentalmente um loop de redirecionamentos infinitos.

Solicitação original: `/redirect-rule/1234/5678`

![Janela do navegador com as Ferramentas para Desenvolvedores acompanhando as solicitações e respostas](url-rewriting/_static/add_redirect.png)

A parte da expressão contida nos parênteses é chamada um *grupo de captura*. O ponto (`.`) da expressão significa *corresponder a qualquer caractere*. O asterisco (`*`) indica *corresponder ao caractere zero precedente ou mais vezes*. Portanto, os dois últimos segmentos de caminho da URL, `1234/5678`, são capturados pelo grupo de captura `(.*)`. Qualquer valor que você fornecer na URL de solicitação após `redirect-rule/` é capturado por esse único grupo de captura.

Na cadeia de caracteres de substituição, os grupos capturados são injetados na cadeia de caracteres com o cifrão (`$`) seguido do número de sequência da captura. O primeiro valor de grupo de captura é obtido com `$1`, o segundo com `$2` e eles continuam em sequência para os grupos de captura no regex. Há apenas um grupo capturado no regex da regra de redirecionamento no aplicativo de exemplo, para que haja apenas um grupo injetado na cadeia de caracteres de substituição, que é `$1`. Quando a regra é aplicada, a URL se torna `/redirected/1234/5678`.

### <a name="url-redirect-to-a-secure-endpoint"></a>Redirecionamento de URL para um ponto de extremidade seguro

Use `AddRedirectToHttps` para redirecionar solicitações HTTP para o mesmo host e caminho usando HTTPS (`https://`). Se o código de status não for fornecido, o middleware usará como padrão 302 (Encontrado). Se a porta não for fornecida, o middleware usará `null` como padrão, o que significa que o protocolo é alterado para `https://` e o cliente acessa o recurso na porta 443. O exemplo mostra como definir o código de status como 301 (Movido Permanentemente) e alterar a porta para 5001.

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttps(301, 5001);

    app.UseRewriter(options);
}
```

Use `AddRedirectToHttpsPermanent` para redirecionar solicitações não seguras para o mesmo host e caminho com o protocolo HTTPS seguro (`https://` na porta 443). O middleware define o código de status como 301 (Movido Permanentemente).

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttpsPermanent();

    app.UseRewriter(options);
}
```

> [!NOTE]
> Ao redirecionar para HTTPS sem a necessidade de regras de redirecionamento adicionais, é recomendável usar Middleware de Redirecionamento HTTPS. Para obter mais informações, veja o tópico [Impor HTTPS](xref:security/enforcing-ssl#require-https).

O aplicativo de exemplo pode demonstrar como usar `AddRedirectToHttps` ou `AddRedirectToHttpsPermanent`. Adicione o método de extensão às `RewriteOptions`. Faça uma solicitação não segura para o aplicativo em qualquer URL. Ignore o aviso de segurança do navegador de que o certificado autoassinado não é confiável ou crie uma exceção para confiar no certificado.

Solicitação original usando `AddRedirectToHttps(301, 5001)`: `http://localhost:5000/secure`

![Janela do navegador com as Ferramentas para Desenvolvedores acompanhando as solicitações e respostas](url-rewriting/_static/add_redirect_to_https.png)

Solicitação original usando `AddRedirectToHttpsPermanent`: `http://localhost:5000/secure`

![Janela do navegador com as Ferramentas para Desenvolvedores acompanhando as solicitações e respostas](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a>Reconfiguração de URL

Use `AddRewrite` para criar uma regra para a reconfiguração de URLs. O primeiro parâmetro contém o regex para correspondência no caminho da URL de entrada. O segundo parâmetro é a cadeia de caracteres de substituição. O terceiro parâmetro, `skipRemainingRules: {true|false}`, indica para o middleware se ele deve ou não ignorar regras de reconfiguração adicionais se a regra atual é aplicada.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=10-11)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", 
            skipRemainingRules: true);

    app.UseRewriter(options);
}
```

::: moniker-end

Solicitação original: `/rewrite-rule/1234/5678`

![Janela do navegador com as Ferramentas para Desenvolvedores acompanhando a solicitação e resposta](url-rewriting/_static/add_rewrite.png)

A primeira coisa que é observada no regex é o acento circunflexo (`^`) no início da expressão. Isso significa que a correspondência começa no início do caminho da URL.

No exemplo anterior com a regra de redirecionamento, `redirect-rule/(.*)`, não há nenhum acento circunflexo no início do regex; portanto, qualquer caractere pode preceder `redirect-rule/` no caminho para uma correspondência bem-sucedida.

| Caminho                               | Corresponder a |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | Sim   |
| `/my-cool-redirect-rule/1234/5678` | Sim   |
| `/anotherredirect-rule/1234/5678`  | Sim   |

A regra de reconfiguração, `^rewrite-rule/(\d+)/(\d+)`, corresponde apenas a caminhos se eles são iniciados com `rewrite-rule/`. Observe a diferença na correspondência entre a regra de reconfiguração abaixo e a regra de redirecionamento acima.

| Caminho                              | Corresponder a |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | Sim   |
| `/my-cool-rewrite-rule/1234/5678` | Não    |
| `/anotherrewrite-rule/1234/5678`  | Não    |

Após a parte `^rewrite-rule/` da expressão, há dois grupos de captura, `(\d+)/(\d+)`. O `\d` significa *corresponder a um dígito (número)*. O sinal de adição (`+`) significa *corresponder a um ou mais caracteres anteriores*. Portanto, a URL precisa conter um número seguido de uma barra "/" seguida de outro número. Esses grupos de captura são injetados na URL reconfigurada como `$1` e `$2`. A cadeia de caracteres de substituição da regra de reconfiguração coloca os grupos capturados na cadeia de consulta. O caminho solicitado de `/rewrite-rule/1234/5678` foi reconfigurado para obter o recurso em `/rewritten?var1=1234&var2=5678`. Se uma cadeia de consulta estiver presente na solicitação original, ela será preservada quando a URL for reconfigurada.

Não há nenhuma ida e vinda para o servidor para obtenção do recurso. Se o recurso existir, ele será buscado e retornado ao cliente com um código de status 200 (OK). Como o cliente não é redirecionado, a URL na barra de endereços do navegador não é alterada. Quanto ao cliente, a operação de reconfiguração de URL nunca ocorreu.

> [!NOTE]
> Use `skipRemainingRules: true` sempre que possível, porque as regras de correspondência são um processo caro e reduzem o tempo de resposta do aplicativo. Para a resposta mais rápida do aplicativo:
> * Ordene as regras de reconfiguração da regra com correspondência mais frequente para a regra com correspondência menos frequente.
> * Ignore o processamento das regras restantes quando ocorrer uma correspondência e nenhum processamento de regra adicional for necessário.

### <a name="apache-modrewrite"></a>mod_rewrite do Apache

Aplique as regras do mod_rewrite do Apache com `AddApacheModRewrite`. Verifique se o arquivo de regras foi implantado com o aplicativo. Para obter mais informações e exemplos de regras de mod_rewrite, consulte [mod_rewrite do Apache](https://httpd.apache.org/docs/2.4/rewrite/).

::: moniker range=">= aspnetcore-2.0"

Um `StreamReader` é usado para ler as regras do arquivo de regras *ApacheModRewrite.txt*.

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=3-4,12)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

O primeiro parâmetro usa um `IFileProvider`, que é fornecido por meio da [Injeção de Dependência](dependency-injection.md). O `IHostingEnvironment` é injetado para fornecer o `ContentRootFileProvider`. O segundo parâmetro é o caminho para o arquivo de regras, que é *ApacheModRewrite.txt* no aplicativo de exemplo.

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt");

    app.UseRewriter(options);
}
```

::: moniker-end

O aplicativo de exemplo redireciona solicitações de `/apache-mod-rules-redirect/(.\*)` para `/redirected?id=$1`. O código de status da resposta é 302 (Encontrado).

[!code[](url-rewriting/sample/ApacheModRewrite.txt)]

Solicitação original: `/apache-mod-rules-redirect/1234`

![Janela do navegador com as Ferramentas para Desenvolvedores acompanhando as solicitações e respostas](url-rewriting/_static/add_apache_mod_redirect.png)

O middleware dá suporte às seguintes variáveis de servidor do mod_rewrite do Apache:

* CONN_REMOTE_ADDR
* HTTP_ACCEPT
* HTTP_CONNECTION
* HTTP_COOKIE
* HTTP_FORWARDED
* HTTP_HOST
* HTTP_REFERER
* HTTP_USER_AGENT
* HTTPS
* IPV6
* QUERY_STRING
* REMOTE_ADDR
* REMOTE_PORT
* REQUEST_FILENAME
* REQUEST_METHOD
* REQUEST_SCHEME
* REQUEST_URI
* SCRIPT_FILENAME
* SERVER_ADDR
* SERVER_PORT
* SERVER_PROTOCOL
* TIME
* TIME_DAY
* TIME_HOUR
* TIME_MIN
* TIME_MON
* TIME_SEC
* TIME_WDAY
* TIME_YEAR

### <a name="iis-url-rewrite-module-rules"></a>Regras do Módulo de Reconfiguração de URL do IIS

Para usar regras que se aplicam ao Módulo de Reconfiguração de URL do IIS, use `AddIISUrlRewrite`. Verifique se o arquivo de regras foi implantado com o aplicativo. Não instrua o middleware a usar o arquivo *web.config* quando ele estiver em execução no IIS do Windows Server. Com o IIS, essas regras devem ser armazenadas fora do *web.config* para evitar conflitos com o módulo de Reconfiguração do IIS. Para obter mais informações e exemplos de regras do Módulo de Reconfiguração de URL do IIS, consulte [Usando o Módulo de Reconfiguração de URL 2.0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) e [Referência de configuração do Módulo de Reconfiguração de URL](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).

::: moniker range=">= aspnetcore-2.0"

Um `StreamReader` é usado para ler as regras do arquivo de regras *IISUrlRewrite.xml*.

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=5-6,13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

O primeiro parâmetro usa um `IFileProvider`, enquanto o segundo é o caminho para o arquivo de regras XML, que é *IISUrlRewrite.xml* no aplicativo de exemplo.

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml");

    app.UseRewriter(options);
}
```

::: moniker-end

O aplicativo de exemplo reconfigura as solicitações de `/iis-rules-rewrite/(.*)` para `/rewritten?id=$1`. A resposta é enviada ao cliente com um código de status 200 (OK).

[!code-xml[](url-rewriting/sample/IISUrlRewrite.xml)]

Solicitação original: `/iis-rules-rewrite/1234`

![Janela do navegador com as Ferramentas para Desenvolvedores acompanhando a solicitação e resposta](url-rewriting/_static/add_iis_url_rewrite.png)

Caso você tenha um Módulo de Reconfiguração do IIS ativo com regras no nível do servidor configuradas que poderiam afetar o aplicativo de maneiras indesejadas, desabilite o Módulo de Reconfiguração do IIS em um aplicativo. Para obter mais informações, consulte [Desabilitando módulos do IIS](xref:host-and-deploy/iis/modules#disabling-iis-modules).

#### <a name="unsupported-features"></a>Recursos sem suporte

::: moniker range=">= aspnetcore-2.0"

O middleware liberado com o ASP.NET Core 2.x não dá suporte aos seguintes recursos do Módulo de Reconfiguração de URL do IIS:

* Regras de saída
* Variáveis de servidor personalizadas
* Curingas
* LogRewrittenUrl

::: moniker-end

::: moniker range="< aspnetcore-2.0"

O middleware liberado com o ASP.NET Core 1.x não dá suporte aos seguintes recursos do Módulo de Reconfiguração de URL do IIS:

* Regras globais
* Regras de saída
* Mapas de Reconfiguração
* Ação de CustomResponse
* Variáveis de servidor personalizadas
* Curingas
* Action:CustomResponse
* LogRewrittenUrl

::: moniker-end

#### <a name="supported-server-variables"></a>Variáveis de servidor compatíveis

O middleware dá suporte às seguintes variáveis de servidor do Módulo de Reconfiguração de URL do IIS:

* CONTENT_LENGTH
* CONTENT_TYPE
* HTTP_ACCEPT
* HTTP_CONNECTION
* HTTP_COOKIE
* HTTP_HOST
* HTTP_REFERER
* HTTP_URL
* HTTP_USER_AGENT
* HTTPS
* LOCAL_ADDR
* QUERY_STRING
* REMOTE_ADDR
* REMOTE_PORT
* REQUEST_FILENAME
* REQUEST_URI

> [!NOTE]
> Também obtenha um `IFileProvider` por meio de um `PhysicalFileProvider`. Essa abordagem pode fornecer maior flexibilidade para o local dos arquivos de regras de reconfiguração. Verifique se os arquivos de regras de reconfiguração são implantados no servidor no caminho fornecido.
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a>Regra baseada em método

Use `Add(Action<RewriteContext> applyRule)` para implementar sua própria lógica de regra em um método. O `RewriteContext` expõe o `HttpContext` para uso no método. O `RewriteContext.Result` determina como o processamento de pipeline adicional é manipulado.

| `RewriteContext.Result`              | Ação                                                          |
| ------------------------------------ | --------------------------------------------------------------- |
| `RuleResult.ContinueRules` (padrão) | Continuar aplicando regras                                         |
| `RuleResult.EndResponse`             | Parar de aplicar regras e enviar a resposta                       |
| `RuleResult.SkipRemainingRules`      | Parar de aplicar regras e enviar o contexto para o próximo middleware |

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=14)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .Add(RedirectXMLRequests);

    app.UseRewriter(options);
}
```

::: moniker-end

O aplicativo de exemplo demonstra um método que redireciona as solicitações para caminhos que terminam com *.xml*. Se você faz uma solicitação para `/file.xml`, ela é redirecionada para `/xmlfiles/file.xml`. O código de status é definido como 301 (Movido Permanentemente). Para um redirecionamento, é necessário definir explicitamente o código de status da resposta; caso contrário, será retornado um código de status 200 (OK) e o redirecionamento não ocorrerá no cliente.

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet1)]

Solicitação original: `/file.xml`

![Janela do navegador com as Ferramentas para Desenvolvedores acompanhando as solicitações e respostas para file.xml](url-rewriting/_static/add_redirect_xml_requests.png)

### <a name="irule-based-rule"></a>Regra baseada em IRule

Use `Add(IRule)` para encapsular sua própria lógica de regra em uma classe que implementa a interface `IRule`. O uso de uma `IRule` fornece maior flexibilidade em relação ao uso da abordagem de regra baseada em método. A classe de implementação pode incluir um construtor, no qual você pode passar parâmetros para o método `ApplyRule`.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=15-16)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .Add(new RedirectImageRequests(".png", "/png-images"))
        .Add(new RedirectImageRequests(".jpg", "/jpg-images"));

    app.UseRewriter(options);
}
```

::: moniker-end

Os valores dos parâmetros no aplicativo de exemplo para a `extension` e o `newPath` são verificados para atender a várias condições. A `extension` precisa conter um valor que precisa ser *.png*, *.jpg* ou *.gif*. Se o `newPath` não é válido, uma `ArgumentException` é gerada. Se você faz uma solicitação para *image.png*, ela é redirecionada para `/png-images/image.png`. Se você faz uma solicitação para *image.jpg*, ela é redirecionada para `/jpg-images/image.jpg`. O código de status é definido como 301 (Movido Permanentemente) e o `context.Result` é definida para parar o processamento de regras e enviar a resposta.

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet2)]

Solicitação original: `/image.png`

![Janela do navegador com as Ferramentas para Desenvolvedores acompanhando as solicitações e respostas para image.png](url-rewriting/_static/add_redirect_png_requests.png)

Solicitação original: `/image.jpg`

![Janela do navegador com as Ferramentas para Desenvolvedores acompanhando as solicitações e respostas para image.jpg](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a>Exemplos do regex

| Goal | Cadeia de caracteres do regex &<br>Exemplo de correspondência | Cadeia de caracteres de substituição &<br>Exemplo de saída |
| ---- | :-----------------------------: | :------------------------------------: |
| Reconfigurar o caminho na cadeia de consulta | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| Barra "/" à direita da faixa | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| Impor barra "/" à direita | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| Evitar a reconfiguração de solicitações específicas | `^(.*)(?<!\.axd)$` ou `^(?!.*\.axd$)(.*)$`<br>Sim: `/resource.htm`<br>Não: `/resource.axd` | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| Reorganizar segmentos de URL | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| Substituir um segmento de URL | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

## <a name="additional-resources"></a>Recursos adicionais

* [Inicialização de aplicativos](startup.md)
* [Middleware](xref:fundamentals/middleware/index)
* [Expressões regulares no .NET](/dotnet/articles/standard/base-types/regular-expressions)
* [Linguagem de expressão regular – referência rápida](/dotnet/articles/standard/base-types/quick-ref)
* [mod_rewrite do Apache](https://httpd.apache.org/docs/2.4/rewrite/)
* [Usando o Módulo de Reconfiguração de URL 2.0 (para IIS)](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)
* [Referência de configuração do Módulo de Reconfiguração de URL](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)
* [Fórum do Módulo de Reconfiguração de URL do IIS](https://forums.iis.net/1152.aspx)
* [Manter uma estrutura de URL simples](https://support.google.com/webmasters/answer/76329?hl=en)
* [10 dicas e truques de reconfiguração de URL](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)
* [Inserir ou não inserir uma barra "/"](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)
