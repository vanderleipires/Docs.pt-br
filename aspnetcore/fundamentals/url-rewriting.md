---
title: "Middleware no núcleo do ASP.NET de regravação de URL"
author: guardrex
description: "Saiba mais sobre a URL de regravação e redirecionar com Middleware de regravação de URL em aplicativos do ASP.NET Core."
keywords: "ASP.NET Core reescrever, URL de regravação de URL, URL de redirecionamento, redirecionamento de URL, middleware, apache_mod"
ms.author: riande
manager: wpickett
ms.date: 08/17/2017
ms.topic: article
ms.assetid: e6130638-c410-4161-9921-b658ce988bd1
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/url-rewriting
ms.openlocfilehash: 0a4024edf13651e2ed7e0f87e554e8ba8d895619
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/01/2017
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a>Middleware no núcleo do ASP.NET de regravação de URL

Por [Luke Latham](https://github.com/guardrex) e [Mikael Mengistu](https://github.com/mikaelm12)

[Exibir ou baixar o código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

Regravação de URL é o ato de modificar as URLs com base em uma ou mais regras predefinidas de solicitação. Regravação de URL criará uma abstração entre locais de recursos e seus endereços de forma que os locais e os endereços não estão vinculados firmemente. Há várias situações em que a regravação de URL é útil:
* Movendo ou substituindo recursos de servidor temporariamente ou permanentemente mantendo localizadores estáveis para esses recursos
* Dividir em diferentes aplicativos ou áreas de um aplicativo de processamento de solicitação
* Removendo, adicionando ou reorganizando segmentos de URL em solicitações de entrada
* Otimizando URLs públicas para otimização de mecanismo de pesquisa (SEO)
* Permitir o uso de URLs amigáveis do públicos ajudam as pessoas a prever o conteúdo que eles encontrarão seguindo um link
* Redirecionar solicitações não seguras para proteger pontos de extremidade
* Impedindo hotlinking de imagem

Você pode definir regras para alterar a URL de várias maneiras, incluindo regex, regras de módulo mod_rewrite Apache, regras de módulo de reescrita de IIS e usando lógica de regra personalizada. Este documento apresenta a regravação de URL com instruções sobre como usar o Middleware de regravação de URL em aplicativos do ASP.NET Core.

> [!NOTE]
> Regravação de URL pode reduzir o desempenho de um aplicativo. Sempre que possível, você deve limitar o número e a complexidade de regras.

## <a name="url-redirect-and-url-rewrite"></a>Redirecionamento de URL e a URL de reconfiguração
A diferença na frase entre *de redirecionamento de URL* e *regravação de URL* pode parecer sutis no primeiro, mas tem implicações importantes para fornecer recursos aos clientes. Middleware de regravação de URL do ASP.NET Core é capaz de atender às necessidades de ambos.

Um *de redirecionamento de URL* é uma operação de cliente, em que o cliente é instruído para acessar um recurso em outro endereço. Isso requer uma viagem para o servidor e a URL de redirecionamento retornada ao cliente aparece na barra de endereços do navegador quando o cliente faz uma nova solicitação para o recurso. Se `/resource` é *redirecionado* para `/different-resource`, as solicitações do cliente `/resource`, e o servidor responde que o cliente deve obter o recurso no `/different-resource` com um código de status que indica que o redirecionamento é temporário ou permanente. O cliente executa uma nova solicitação para o recurso na URL de redirecionamento.

![Um ponto de extremidade de serviço WebAPI foi alterado temporariamente da versão 1 (v1) para a versão 2 (v2) no servidor. Um cliente faz uma solicitação para o serviço em /v1/api de caminho de versão 1. O servidor envia uma resposta 302 de (não encontrado) com o caminho novo e temporário para o serviço no /v2/api versão 2. O cliente faz uma segunda solicitação para o serviço na URL de redirecionamento. O servidor responde com um código de status 200 (Okey).](url-rewriting/_static/url_redirect.png)

Ao redirecionar solicitações para uma URL diferente, você indica se o redirecionamento é permanente ou temporária. O código de status (movido permanentemente) 301 é usado em que o recurso tem uma nova URL permanente e você desejar para instruir o cliente que todas as solicitações futuras para o recurso devem usar a nova URL. *O cliente pode armazenar em cache a resposta quando um código de 301 status é recebido.* O código de status 302 (não encontrado) é usado em que o redirecionamento é temporário ou geralmente assunto para alterar, de modo que o cliente não deve armazenar e reutilizar a URL de redirecionamento no futuro. Para obter mais informações, consulte [RFC 2616: definições de código de Status](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).

Um *regravação de URL* é uma operação do servidor para fornecer um recurso de um endereço de recurso diferente. Reconfiguração de uma URL não requer uma viagem para o servidor. A URL regravada não é retornada ao cliente e não aparecerá na barra de endereços do navegador. Quando `/resource` é *reescrita* para `/different-resource`, as solicitações do cliente `/resource`e o servidor *internamente* busca o recurso no `/different-resource`. Embora o cliente possa ser capaz de recuperar o recurso na URL regravada, o cliente não ser informado que o recurso existe na URL regravada quando ele faz a solicitação e recebe a resposta.

![Um ponto de extremidade de serviço WebAPI foi alterado da versão 1 (v1) para a versão 2 (v2) no servidor. Um cliente faz uma solicitação para o serviço em /v1/api de caminho de versão 1. A URL da solicitação foi reescrita para acessar o serviço em /v2/api de caminho a versão 2. O serviço responde ao cliente com um código de status 200 (Okey).](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a>Aplicativo de exemplo de regravação de URL
Você pode explorar os recursos de Middleware de regravação de URL com o [aplicativo de exemplo de regravação de URL](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/). O aplicativo se aplica a regravação e regras de redirecionamento e mostra a URL redirecionada ou reconfigurada.

## <a name="when-to-use-url-rewriting-middleware"></a>Quando usar o Middleware de regravação de URL
Usar o Middleware de regravação de URL quando não for possível usar o [módulo de reescrita de URL](https://www.iis.net/downloads/microsoft/url-rewrite) com o IIS no Windows Server, o [módulo do Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/) no servidor do Apache, [regravação de URL em Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/), ou seu aplicativo é hospedado em [servidor HTTP. sys](xref:fundamentals/servers/httpsys) (anteriormente chamado [WebListener](xref:fundamentals/servers/weblistener)). As principais razões para usar as tecnologias regravação de URL com base em servidor no IIS, o Apache ou Nginx são que o middleware não oferece suporte os recursos completos desses módulos e o desempenho do middleware provavelmente não corresponde dos módulos. No entanto, há alguns recursos dos módulos de servidor que não funcionam com projetos do ASP.NET Core, como o `IsFile` e `IsDirectory` restrições do módulo de reescrita de IIS. Nesses cenários, use o middleware.

## <a name="package"></a>Pacote
Para incluir o middleware em seu projeto, adicione uma referência para o [ `Microsoft.AspNetCore.Rewrite` ](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) pacote. Este recurso está disponível para aplicativos que se destinam a ASP.NET Core 1.1 ou posterior.

## <a name="extension-and-options"></a>Extensão e opções
Estabelecer a regravação de URL e regras de redirecionamento, criando uma instância do `RewriteOptions` classe com métodos de extensão para cada uma das suas regras. Várias regras na ordem em que você gostaria que elas processado da cadeia. O `RewriteOptions` são passados para o Middleware de regravação de URL, como ele é adicionado ao pipeline de solicitação com `app.UseRewriter(options);`.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1)]

---

### <a name="url-redirect"></a>Redirecionamento de URL
Use `AddRedirect` para redirecionar as solicitações. O primeiro parâmetro contém o regex para correspondência no caminho da URL de entrada. O segundo parâmetro é a cadeia de caracteres de substituição. O terceiro parâmetro, se presente, especifica o código de status. Se você não especificar o código de status, o padrão é 302 (não encontrado), que indica que o recurso é movido temporariamente ou substituído.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=2)]

---

Em um navegador com as ferramentas de desenvolvedor habilitado, fazer uma solicitação para o aplicativo de exemplo com o caminho `/redirect-rule/1234/5678`. A regex corresponda ao caminho de solicitação em `redirect-rule/(.*)`, e o caminho é substituído por `/redirected/1234/5678`. O redirecionamento de URL é enviada de volta ao cliente com um código de status 302 (não encontrado). O navegador faz uma solicitação de novo na URL de redirecionamento, que aparece na barra de endereços do navegador. Uma vez que nenhuma regra no aplicativo de exemplo corresponde a URL de redirecionamento, a segunda solicitação recebe uma resposta 200 do (Okey) do aplicativo e o corpo da resposta mostra a URL de redirecionamento. Uma ida e volta é feita para o servidor quando uma URL é *redirecionado*.

> [!WARNING]
> Tenha cuidado ao estabelecer suas regras de redirecionamento. As regras de redirecionamento são avaliadas em cada solicitação para o aplicativo, incluindo depois de um redirecionamento. É fácil acidentalmente criar um loop de redirecionamentos infinitos.

Solicitação original:`/redirect-rule/1234/5678`

![Janela do navegador com as ferramentas de desenvolvedor do controle as solicitações e respostas](url-rewriting/_static/add_redirect.png)

A parte da expressão contida dentro dos parênteses é chamada uma *grupo de captura*. O ponto final (`.`) da expressão significa *corresponder qualquer caractere*. O asterisco (`*`) indica *corresponde ao caractere precedente zero ou mais vezes*. Portanto, os segmentos de duas últimas caminho da URL, `1234/5678`, são capturados pelo grupo de captura `(.*)`. Qualquer valor que você fornecer a URL de solicitação após `redirect-rule/` é capturado por esse grupo de captura único.

Na cadeia de caracteres de substituição, grupos capturados são injetados na cadeia de caracteres com o símbolo de dólar (`$`) seguido pelo número de sequência da captura. O primeiro valor de grupo de captura é obtido com `$1`, a segunda com `$2`, e eles continuam em sequência para os grupos de captura em sua regex. Há apenas um grupo capturado no regex de regra de redirecionamento no aplicativo de exemplo, para que haja apenas um grupo inserido na cadeia de caracteres de substituição, que é `$1`. Quando a regra é aplicada, torna-se a URL `/redirected/1234/5678`.

<a name=url-redirect-to-secure-endpoint></a>
### <a name="url-redirect-to-a-secure-endpoint"></a>Redirecionamento de URL para um ponto de extremidade seguro
Use `AddRedirectToHttps` para redirecionar solicitações HTTP para o mesmo host e caminho usando HTTPS (`https://`). Se o código de status não for fornecido, o middleware padrão 302 (não encontrado). Se a porta não for fornecida, o middleware padrão `null`, que significa que o protocolo é alterado para `https://` e o cliente acessa o recurso na porta 443. O exemplo mostra como definir o código de status para 301 (movido permanentemente) e altere a porta para 5001.
```csharp
var options = new RewriteOptions()
    .AddRedirectToHttps(301, 5001);

app.UseRewriter(options);
```
Use `AddRedirectToHttpsPermanent` para redirecionar solicitações não seguras para o mesmo host e o caminho com o protocolo HTTPS seguro (`https://` na porta 443). O middleware define o código de status 301 (movido permanentemente).

O aplicativo de exemplo é capaz de demonstrar como usar `AddRedirectToHttps` ou `AddRedirectToHttpsPermanent`. Adicione o método de extensão para o `RewriteOptions`. Fazer uma solicitação não segura para o aplicativo em qualquer URL. Ignore a aviso que o certificado autoassinado não é confiável de segurança do navegador.

Solicitação original usando `AddRedirectToHttps(301, 5001)`:`/secure`

![Janela do navegador com as ferramentas de desenvolvedor do controle as solicitações e respostas](url-rewriting/_static/add_redirect_to_https.png)

Solicitação original usando `AddRedirectToHttpsPermanent`:`/secure`

![Janela do navegador com as ferramentas de desenvolvedor do controle as solicitações e respostas](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a>Regravar URL
Use `AddRewrite` para criar uma regras de regravação de URLs. O primeiro parâmetro contém o regex para correspondência no caminho de URL de entrada. O segundo parâmetro é a cadeia de caracteres de substituição. O terceiro parâmetro, `skipRemainingRules: {true|false}`, indica se deve ou não ignorar regras de regravação adicionais se a regra atual é aplicada para o middleware.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=6)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=3)]

---

Solicitação original:`/rewrite-rule/1234/5678`

![Janela do navegador com as ferramentas de desenvolvedor de rastreamento de solicitação e resposta](url-rewriting/_static/add_rewrite.png)

A primeira coisa que você observar no regex é o circunflexo (`^`) no início da expressão. Isso significa que a correspondência começa no início do caminho da URL.

No exemplo anterior com a regra de redirecionamento, `redirect-rule/(.*)`, não há nenhum circunflexo no início do regex; portanto, qualquer caractere pode preceder `redirect-rule/` no caminho para uma correspondência com êxito.

| Caminho                               | Corresponder a |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | Sim   |
| `/my-cool-redirect-rule/1234/5678` | Sim   |
| `/anotherredirect-rule/1234/5678`  | Sim   |

A regra de regravação `^rewrite-rule/(\d+)/(\d+)`, corresponde apenas a caminhos se forem iniciados com `rewrite-rule/`. Observe a diferença de correspondência entre a regra de regravação abaixo e a regra de redirecionamento acima.

| Caminho                              | Corresponder a |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | Sim   |
| `/my-cool-rewrite-rule/1234/5678` | Não    |
| `/anotherrewrite-rule/1234/5678`  | Não    |

Após o `^rewrite-rule/` parte da expressão, há dois grupos de captura, `(\d+)/(\d+)`. O `\d` significa *corresponder um dígito (número)*. O sinal de adição (`+`) significa *corresponde a um ou mais o caractere anterior*. Portanto, a URL deve conter um número seguido por uma barra seguida por outro número. Esses grupos são injetados para a URL regravada como de captura `$1` e `$2`. A cadeia de caracteres de substituição de regra de regravação coloca os grupos capturados em querystring. O caminho solicitado de `/rewrite-rule/1234/5678` foi reescrito para obter o recurso no `/rewritten?var1=1234&var2=5678`. Se uma querystring estiver presente na solicitação original, ele é preservado quando a URL é recriada.

Não há nenhum ida e volta ao servidor para obter o recurso. Se o recurso existir, ela tem buscadas e retornada ao cliente com um código de status (Okey) 200. Porque o cliente não for redirecionado, não altere a URL na barra de endereços do navegador. Quanto o cliente está interessado, a operação de regravação de URL nunca ocorreu.

> [!NOTE]
> Use `skipRemainingRules: true` sempre que possível, porque as regras de correspondência é um processo caro e reduz o tempo de resposta do aplicativo. Para a resposta mais rápida do aplicativo:
> * Ordene suas regras de reescrita da regra de correspondência com mais frequência para a regra de correspondência com menos frequência.
> * Ignore o processamento das regras restantes quando ocorre uma correspondência e nenhum processamento de regra adicional é necessário.

### <a name="apache-modrewrite"></a>Apache mod_rewrite
Aplicar regras de mod_rewrite Apache com `AddApacheModRewrite`. Certifique-se de que o arquivo de regras é implantado com o aplicativo. Para obter mais informações e exemplos de regras mod_rewrite, consulte [mod_rewrite Apache](https://httpd.apache.org/docs/2.4/rewrite/).

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Um `StreamReader` é usado para ler as regras do *ApacheModRewrite.txt* arquivo de regras.

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=1,7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

O primeiro parâmetro aceita um `IFileProvider`, que é fornecido por meio de [injeção de dependência](dependency-injection.md). O `IHostingEnvironment` é injetado para fornecer o `ContentRootFileProvider`. O segundo parâmetro é o caminho para o arquivo de regras, que é *ApacheModRewrite.txt* no aplicativo de exemplo.

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=4)]

---

O aplicativo de exemplo redireciona solicitações de `/apache-mod-rules-redirect/(.\*)` para `/redirected?id=$1`. O código de status de resposta é 302 (não encontrado).

[!code[Main](url-rewriting/samples/2.x/ApacheModRewrite.txt)]

Solicitação original:`/apache-mod-rules-redirect/1234`

![Janela do navegador com as ferramentas de desenvolvedor do controle as solicitações e respostas](url-rewriting/_static/add_apache_mod_redirect.png)

##### <a name="supported-server-variables"></a>Variáveis de servidor com suporte
O middleware suporta as seguintes variáveis de servidor Apache mod_rewrite:
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
* TEMPO
* TIME_DAY
* TIME_HOUR
* TIME_MIN
* TIME_MON
* TIME_SEC
* TIME_WDAY
* TIME_YEAR

### <a name="iis-url-rewrite-module-rules"></a>Regras do módulo de reescrita de URL do IIS
Para usar as regras que se aplicam para o módulo de reescrita de URL do IIS, use `AddIISUrlRewrite`. Certifique-se de que o arquivo de regras é implantado com o aplicativo. Não direta de middleware para usar o *Web. config* arquivo quando em execução no IIS do Windows Server. Com o IIS, essas regras devem ser armazenadas fora do seu *Web. config* para evitar conflitos com o módulo de reescrita de IIS. Para obter mais informações e exemplos de regras do módulo de reescrita de URL do IIS, consulte [usando o Url Rewrite Module 2.0](https://docs.microsoft.com/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) e [URL de referência de configuração de módulo reescreva](https://docs.microsoft.com/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Um `StreamReader` é usado para ler as regras do *IISUrlRewrite.xml* arquivo de regras.

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=2,8)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

O primeiro parâmetro aceita um `IFileProvider`, enquanto o segundo parâmetro é o caminho para o arquivo de regras XML, que é *IISUrlRewrite.xml* no aplicativo de exemplo.

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=5)]

---

O aplicativo de exemplo reconfigura as solicitações de `/iis-rules-rewrite/(.*)` para `/rewritten?id=$1`. A resposta será enviada ao cliente com um código de status 200 (Okey).

[!code-xml[Main](url-rewriting/samples/2.x/IISUrlRewrite.xml)]

Solicitação original:`/iis-rules-rewrite/1234`

![Janela do navegador com as ferramentas de desenvolvedor de rastreamento de solicitação e resposta](url-rewriting/_static/add_iis_url_rewrite.png)

Se você tiver um módulo de reescrita de IIS ativa com as regras de nível de servidor configuradas que poderia afetar seu aplicativo de maneira indesejada, você pode desabilitar o módulo de reescrita de IIS para um aplicativo. Para obter mais informações, consulte [módulos do IIS desabilitando](xref:hosting/iis-modules#disabling-iis-modules).

#### <a name="unsupported-features"></a>Recursos sem suporte

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

O middleware liberado com o ASP.NET Core 2. x não oferece suporte para os seguintes recursos do IIS URL Rewrite Module:
* Regras de saída
* Variáveis de servidor personalizado
* Curingas
* LogRewrittenUrl

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

O middleware liberado com o ASP.NET Core 1. x não oferece suporte para os seguintes recursos do IIS URL Rewrite Module:
* Regras globais
* Regras de saída
* Mapas de regravação
* Ação de CustomResponse
* Variáveis de servidor personalizado
* Curingas
* Ação: CustomResponse
* LogRewrittenUrl

---

#### <a name="supported-server-variables"></a>Variáveis de servidor com suporte
O middleware suporta as seguintes variáveis de servidor IIS URL Rewrite Module:
* CONTENT_LENGTH
* TIPO_DE_CONTEÚDO
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
> Você também pode obter um `IFileProvider` por meio de um `PhysicalFileProvider`. Essa abordagem pode fornecer maior flexibilidade para o local da sua reescrita arquivos de regras. Certifique-se de que os arquivos de regras de regravação são implantados no servidor no caminho que você fornecer.
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a>Regra com base em método:
Use `Add(Action<RewriteContext> applyRule)` para implementar sua própria lógica de regra em um método. O `RewriteContext` expõe o `HttpContext` para uso em seu método. O `context.Result` determina o pipeline adicional como o processamento é tratado.

| contexto. Resultado                       | Ação                                                          |
| ------------------------------------ | --------------------------------------------------------------- |
| `RuleResult.ContinueRules` (padrão) | Continue a aplicar regras                                         |
| `RuleResult.EndResponse`             | Parar de aplicar regras e enviar a resposta                       |
| `RuleResult.SkipRemainingRules`      | Parar de aplicar regras e enviar o contexto para o próximo middleware |

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=6)]

---

O aplicativo de exemplo demonstra um método que redirecione as solicitações para caminhos que terminam com *. XML*. Se você fizer uma solicitação `/file.xml`, ele é redirecionado para `/xmlfiles/file.xml`. O código de status é definido como 301 (movido permanentemente). Para um redirecionamento, você deve definir explicitamente o código de status da resposta; Caso contrário, será retornado um código de status 200 (Okey) e o redirecionamento não ocorrerá no cliente.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/RewriteRules.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet2)]

---

Solicitação original:`/file.xml`

![Janela do navegador com as ferramentas de desenvolvedor controle as solicitações e respostas para File. XML](url-rewriting/_static/add_redirect_xml_requests.png)

### <a name="irule-based-rule"></a>Regra com base em IRule
Use `Add(IRule)` para implementar sua própria lógica de regra em uma classe que deriva de `IRule`. Usando um `IRule` fornece maior flexibilidade em relação a usar a abordagem de regra com base em método. A classe derivada pode incluir um construtor, onde você pode passar parâmetros para o `ApplyRule` método.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=10-11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=7-8)]

---

Os valores dos parâmetros no aplicativo de exemplo para o `extension` e `newPath` são verificados para atender a várias condições. O `extension` deve conter um valor, e o valor deve ser *. PNG*, *. jpg*, ou *. gif*. Se o `newPath` não é válido, um `ArgumentException` é gerada. Se você fizer uma solicitação *image.png*, ele é redirecionado para `/png-images/image.png`. Se você fizer uma solicitação *image.jpg*, ele é redirecionado para `/jpg-images/image.jpg`. O código de status é definido como 301 (movido permanentemente) e o `context.Result` é definida para parar o processamento de regras e enviar a resposta.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/RewriteRules.cs?name=snippet2)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/RewriteRule.cs?name=snippet1)]

---

Solicitação original:`/image.png`

![Janela do navegador com as ferramentas de desenvolvedor do controle as solicitações e respostas para image.png](url-rewriting/_static/add_redirect_png_requests.png)

Solicitação original:`/image.jpg`

![Janela do navegador com as ferramentas de desenvolvedor do controle as solicitações e respostas para image.jpg](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a>Exemplos de regex

| Goal | Cadeia de caracteres regex &<br>Exemplo de correspondência | Cadeia de caracteres de substituição &<br>Exemplo de saída |
| ---- | :-----------------------------: | :------------------------------------: |
| Reescreva o caminho em querystring | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| Barra à direita da faixa | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| Impor a barra à direita | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| Evitar a reconfiguração de solicitações específicas | `(.*[^(\.axd)])$`<br>Sim:`/resource.htm`<br>Não:`/resource.axd` | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| Reorganizar os segmentos de URL | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| Substituir um segmento de URL | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

## <a name="additional-resources"></a>Recursos adicionais
* [Inicialização de aplicativos](startup.md)
* [Middleware](middleware.md)
* [Expressões regulares no .NET](/dotnet/articles/standard/base-types/regular-expressions)
* [Linguagem de expressão regular – referência rápida](/dotnet/articles/standard/base-types/quick-ref)
* [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/)
* [Usando o módulo de reescrita de Url 2.0 (para IIS)](https://docs.microsoft.com/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)
* [Referência de configuração do módulo de regravação de URL](https://docs.microsoft.com/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)
* [Fórum de módulo de regravação de URL de IIS](https://forums.iis.net/1152.aspx)
* [Manter uma estrutura simples de URL](https://support.google.com/webmasters/answer/76329?hl=en)
* [Regravação de URL 10 dicas e truques](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)
* [Barra ou não de barra](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)
