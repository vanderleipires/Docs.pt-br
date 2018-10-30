---
title: Implementação do servidor Web Kestrel no ASP.NET Core
author: rick-anderson
description: Saiba mais sobre o Kestrel, o servidor Web multiplataforma do ASP.NET Core.
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/13/2018
uid: fundamentals/servers/kestrel
ms.openlocfilehash: 7e47bc3df90a72f15a6fdde080aae6fbf7494384
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207453"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a>Implementação do servidor Web Kestrel no ASP.NET Core

Por [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher) e [Stephen Halter](https://twitter.com/halter73)

O Kestrel é um [servidor Web multiplataforma para o ASP.NET Core](xref:fundamentals/servers/index). O Kestrel é o servidor Web que está incluído por padrão em modelos de projeto do ASP.NET Core.

O Kestrel dá suporte aos seguintes recursos:

::: moniker range=">= aspnetcore-2.2"

* HTTPS
* Atualização do Opaque usado para habilitar o [WebSockets](https://github.com/aspnet/websockets)
* Soquetes do UNIX para alto desempenho protegidos pelo Nginx
* HTTP/2 (exceto em macOS&dagger;)

&dagger;O HTTP/2 será compatível com macOS em uma versão futura.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* HTTPS
* Atualização do Opaque usado para habilitar o [WebSockets](https://github.com/aspnet/websockets)
* Soquetes do UNIX para alto desempenho protegidos pelo Nginx

::: moniker-end

Há suporte para o Kestrel em todas as plataformas e versões compatíveis com o .NET Core.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([como baixar](xref:index#how-to-download-a-sample))

::: moniker range=">= aspnetcore-2.2"

## <a name="http2-support"></a>Compatibilidade com HTTP/2

O [HTTP/2](https://httpwg.org/specs/rfc7540.html) estará disponível para aplicativos ASP.NET Core se os seguintes requisitos básicos forem atendidos:

* Sistema operacional&dagger;
  * Windows Server 2012 R2/Windows 8.1 ou posterior
  * Linux com OpenSSL 1.0.2 ou posterior (por exemplo, Ubuntu 16.04 ou posterior)
* Estrutura de destino: .NET Core 2.2 ou posterior
* Conexão [ALPN (Negociação de protocolo de camada de aplicativo)](https://tools.ietf.org/html/rfc7301#section-3)
* Conexão TLS 1.2 ou posterior

&dagger;O HTTP/2 será compatível com macOS em uma versão futura.

Se uma conexão HTTP/2 for estabelecida, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) relatará `HTTP/2`.

HTTP/2 está desabilitado por padrão. Para obter mais informações sobre a configuração, consulte as seções [Opções do Kestrel](#kestrel-options) e [Configuração de ponto de extremidade](#endpoint-configuration).

::: moniker-end

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a>Quando usar o Kestrel com um proxy reverso

::: moniker range=">= aspnetcore-2.0"

Você pode usar o Kestrel sozinho ou com um *servidor proxy reverso* como IIS, Nginx ou Apache. Um servidor proxy reverso recebe solicitações HTTP da Internet e as encaminha para o Kestrel após algum tratamento preliminar.

![O Kestrel se comunica diretamente com a Internet, sem um servidor proxy reverso](kestrel/_static/kestrel-to-internet2.png)

![O Kestrel se comunica indiretamente com a Internet através de um servidor proxy reverso, tal como o IIS, o Nginx ou o Apache](kestrel/_static/kestrel-to-internet.png)

Qualquer configuração &mdash;com ou sem um servidor proxy reverso&mdash; é uma configuração de hospedagem válida e compatível com o ASP.NET Core 2.0 ou aplicativos posteriores.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Se um aplicativo aceitar solicitações somente de uma rede interna, o Kestrel poderá ser usado diretamente como o servidor do aplicativo.

![O Kestrel se comunica diretamente com a rede interna](kestrel/_static/kestrel-to-internal.png)

Se você expõe o aplicativo à Internet, use o IIS, o Nginx ou o Apache como um *servidor proxy reverso*. Um servidor proxy reverso recebe solicitações HTTP da Internet e as encaminha para o Kestrel após algum tratamento preliminar.

![O Kestrel se comunica indiretamente com a Internet através de um servidor proxy reverso, tal como o IIS, o Nginx ou o Apache](kestrel/_static/kestrel-to-internet.png)

Um proxy reverso é necessário para implantações de servidor de borda voltado para o público (expostas ao tráfego da Internet) por motivos de segurança. As versões 1.x do Kestrel não têm um conjunto completo de defesas contra ataques, como tempos limite, limites de tamanho e limites de conexões simultâneas apropriados.

::: moniker-end

Um cenário de proxy reverso existe quando há vários aplicativos que compartilham o mesmo IP e a mesma porta em execução em um único servidor. O Kestrel não é compatível com esse cenário porque ele não permite compartilhar o mesmo IP e a mesma porta entre vários processos. Quando o Kestrel é configurado para escutar em uma porta, ele lida com todo o tráfego dessa porta, independentemente do cabeçalho do host das solicitações. Um proxy reverso que pode compartilhar portas tem a capacidade de encaminhar solicitações ao Kestrel em um IP e em uma porta exclusivos.

Mesmo se um servidor proxy reverso não for necessário, usar um servidor proxy reverso poderá ser uma boa opção:

* Ele pode limitar a área da superfície pública exposta dos aplicativos que hospeda.
* Ele fornece uma camada adicional de defesa e de configuração.
* Pode ser integrado melhor à infraestrutura existente.
* Ele simplifica a configuração de SSL e o balanceamento de carga. Somente o servidor proxy reverso exige um certificado SSL e esse servidor pode se comunicar com os servidores de aplicativos na rede interna usando HTTP simples.

> [!WARNING]
> Se você não estiver usando um proxy reverso com a filtragem de host habilitada, a [filtragem de host](#host-filtering) deverá ser habilitada.

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a>Como usar o Kestrel em aplicativos ASP.NET Core

::: moniker range=">= aspnetcore-2.0"

O pacote [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) está incluído no [metapacote Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 ou posterior).

Os modelos de projeto do ASP.NET Core usam o Kestrel por padrão. Em *Program.cs*, o código do modelo chama [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), que chama [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) em segundo plano.

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

Para fornecer configuração adicional após chamar `CreateDefaultBuilder`, use `ConfigureKestrel`:

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            // Set properties and call methods on options
        });
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

Para fornecer configuração adicional após chamar `CreateDefaultBuilder`, chame [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel):

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            // Set properties and call methods on options
        })
        .Build();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Instale o pacote NuGet [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/).

Chame o método de extensão [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) no [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder?view=aspnetcore-1.1) no método `Main`, especificando as [opções do Kestrel](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1) necessárias, conforme é mostrado na próxima seção.

[!code-csharp[](kestrel/samples/1.x/KestrelSample/Program.cs?name=snippet_Main&highlight=13-19)]

::: moniker-end

## <a name="kestrel-options"></a>Opções do Kestrel

::: moniker range=">= aspnetcore-2.0"

O servidor Web do Kestrel tem opções de configuração de restrição especialmente úteis em implantações para a Internet. Alguns limites importantes que podem ser personalizados:

* Número máximo de conexões de cliente
* Tamanho máximo do corpo da solicitação
* Taxa de dados mínima do corpo da solicitação

Defina essas e outras restrições na propriedade [Limites](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) da classe [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions). A propriedade `Limits` contém uma instância da classe [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits).

### <a name="maximum-client-connections"></a>Número máximo de conexões de cliente

[MaxConcurrentConnections](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentconnections)  
[MaxConcurrentUpgradedConnections](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentupgradedconnections)

::: moniker-end

O número máximo de conexões TCP abertas simultâneas pode ser definido para o aplicativo inteiro com o código a seguir:

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MaxConcurrentConnections = 100;
        });
```

::: moniker-end

Há um limite separado para conexões que foram atualizadas do HTTP ou HTTPS para outro protocolo (por exemplo, em uma solicitação do WebSockets). Depois que uma conexão é atualizada, ela não é contada em relação ao limite de `MaxConcurrentConnections`.

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MaxConcurrentUpgradedConnections = 100;
        });
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

O número máximo de conexões é ilimitado (nulo) por padrão.

### <a name="maximum-request-body-size"></a>Tamanho máximo do corpo da solicitação

[MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize)

O tamanho máximo do corpo da solicitação padrão é de 30.000.000 bytes, que equivale aproximadamente a 28,6 MB.

A abordagem recomendada para substituir o limite em um aplicativo ASP.NET Core MVC é usar o atributo [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) em um método de ação:

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

::: moniker-end

Aqui está um exemplo que mostra como configurar a restrição para o aplicativo em cada solicitação:

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 10 * 1024;
        });
```

Substitua a configuração em uma solicitação específica no middleware:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

Uma exceção será gerada se você tentar configurar o limite em uma solicitação depois que o aplicativo começar a ler a solicitação. Há uma propriedade `IsReadOnly` que indica se a propriedade `MaxRequestBodySize` está no estado somente leitura, o que significa que é tarde demais para configurar o limite.

### <a name="minimum-request-body-data-rate"></a>Taxa de dados mínima do corpo da solicitação

[MinRequestBodyDataRate](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minrequestbodydatarate)  
[MinResponseDataRate](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minresponsedatarate)

O Kestrel verifica a cada segundo se os dados estão sendo recebidos na taxa especificada em bytes/segundo. Se a taxa cair abaixo do mínimo, a conexão atingirá o tempo limite. O período de cortesia é o tempo que o Kestrel fornece ao cliente para aumentar sua taxa de envio até o mínimo; a taxa não é verificada durante esse período. O período de cortesia ajuda a evitar a remoção de conexões que inicialmente enviam dados em uma taxa baixa devido ao início lento do TCP.

A taxa mínima de padrão é de 240 bytes/segundo com um período de cortesia de 5 segundos.

Uma taxa mínima também se aplica à resposta. O código para definir o limite de solicitação e o limite de resposta é o mesmo, exceto por ter `RequestBody` ou `Response` nos nomes da propriedade e da interface.

Este é um exemplo que mostra como configurar as taxas mínima de dados em *Program.cs*:

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-9)]

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MinRequestBodyDataRate =
                new MinDataRate(bytesPerSecond: 100, gracePeriod: TimeSpan.FromSeconds(10));
            options.Limits.MinResponseDataRate =
                new MinDataRate(bytesPerSecond: 100, gracePeriod: TimeSpan.FromSeconds(10));
        });
```

Configure as taxas por solicitação no middleware:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=5-8)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="maximum-streams-per-connection"></a>Fluxos máximos por conexão

O `Http2.MaxStreamsPerConnection` limita o número de fluxos de solicitações simultâneas por conexão HTTP/2. Os fluxos em excesso são recusados.

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.MaxStreamsPerConnection = 100;
        });
```

O valor padrão é 100.

### <a name="header-table-size"></a>Tamanho da tabela de cabeçalho

O decodificador HPACK descompacta os cabeçalhos HTTP para conexões HTTP/2. O `Http2.HeaderTableSize` limita o tamanho da tabela de compactação de cabeçalho usada pelo decodificador HPACK. O valor é fornecido em octetos e deve ser maior do que zero (0).

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.HeaderTableSize = 4096;
        });
```

O valor padrão é 4096.

### <a name="maximum-frame-size"></a>Tamanho máximo do quadro

O `Http2.MaxFrameSize` indica o tamanho máximo do payload do quadro da conexão HTTP/2 a ser recebido. O valor é fornecido em octetos e deve estar entre 2^14 (16.384) e 2^24-1 (16.777.215).

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.MaxFrameSize = 16384;
        });
```

O valor padrão é 2^14 (16.384).

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

Para obter informações sobre outras opções e limites do Kestrel, confira:

* [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions)
* [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits)
* [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Para obter informações sobre as opções e os limites do Kestrel, confira:

* [Classe KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1)
* [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserverlimits?view=aspnetcore-1.1)

::: moniker-end

## <a name="endpoint-configuration"></a>Configuração do ponto de extremidade

::: moniker range="= aspnetcore-2.0"

Por padrão, o ASP.NET Core é associado a `http://localhost:5000`. Chame os métodos [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) ou [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) em [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) para configurar portas e prefixos de URL para o Kestrel. `UseUrls`, o argumento de linha de comando `--urls`, a chave de configuração de host `urls` e a variável de ambiente `ASPNETCORE_URLS` também funcionam mas têm limitações que serão indicadas mais adiante nesta seção.

A chave de configuração de host `urls` precisa vir da configuração do host, não da configuração do aplicativo. Adicionar uma chave e um valor de `urls` para *appsettings.json* não afeta a configuração do host porque o host está completamente inicializado quando a configuração é lida no arquivo de configuração. No entanto, uma chave `urls` em *appsettings.json* pode ser usada com [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) no construtor de host para configurar o host:

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddJsonFile("appSettings.json", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseKestrel()
    .UseConfiguration(config)
    .UseContentRoot(Directory.GetCurrentDirectory())
    .UseStartup<Startup>()
    .Build();
```

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

Por padrão, o ASP.NET Core associa a:

* `http://localhost:5000`
* `https://localhost:5001` (quando um certificado de desenvolvimento local está presente)

Um certificado de desenvolvimento é criado:

* Quando o [SDK do .NET Core](/dotnet/core/sdk) está instalado.
* A [ferramenta dev-certs](xref:aspnetcore-2.1#https) é usada para criar um certificado.

Alguns navegadores exigem que você conceda permissão explícita para o navegador para confiar no certificado de desenvolvimento local.

Os modelos de projeto do ASP.NET Core 2.1 e posteriores configuram os aplicativos para serem executados em HTTPS, por padrão, e incluem o [Redirecionamento de HTTPS e o suporte a HSTS](xref:security/enforcing-ssl).

Chame os métodos [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) ou [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) em [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) para configurar portas e prefixos de URL para o Kestrel.

`UseUrls`, o argumento de linha de comando `--urls`, a chave de configuração de host `urls` e a variável de ambiente `ASPNETCORE_URLS` também funcionam mas têm limitações que serão indicadas mais adiante nesta seção (um certificado padrão precisa estar disponível para a configuração do ponto de extremidade HTTPS).

Configuração do ASP.NET Core 2.1 `KestrelServerOptions`:

### <a name="configureendpointdefaultsactionltlistenoptionsgt"></a>ConfigureEndpointDefaults(Action&lt;ListenOptions&gt;)

Especifica uma `Action` de configuração a ser executada para cada ponto de extremidade especificado. Chamar `ConfigureEndpointDefaults` várias vezes substitui as `Action`s pela última `Action` especificada.

### <a name="configurehttpsdefaultsactionlthttpsconnectionadapteroptionsgt"></a>ConfigureHttpsDefaults(Action&lt;HttpsConnectionAdapterOptions&gt;)

Especifica uma `Action` de configuração a ser executada para cada ponto de extremidade HTTPS. Chamar `ConfigureHttpsDefaults` várias vezes substitui as `Action`s pela última `Action` especificada.

### <a name="configureiconfiguration"></a>Configure(IConfiguration)

Cria um carregador de configuração para configurar o Kestrel que usa uma [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) como entrada. A configuração precisa estar no escopo da seção de configuração do Kestrel.

### <a name="listenoptionsusehttps"></a>ListenOptions.UseHttps

Configure o Kestrel para usar HTTPS.

Extensões `ListenOptions.UseHttps`:

* `UseHttps` &ndash; Configure o Kestrel para usar HTTPS com o certificado padrão. Gera uma exceção quando não há nenhum certificado padrão configurado.
* `UseHttps(string fileName)`
* `UseHttps(string fileName, string password)`
* `UseHttps(string fileName, string password, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(StoreName storeName, string subject)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(X509Certificate2 serverCertificate)`
* `UseHttps(X509Certificate2 serverCertificate, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(Action<HttpsConnectionAdapterOptions> configureOptions)`

Parâmetros de `ListenOptions.UseHttps`:

* `filename` é o caminho e o nome do arquivo de um arquivo de certificado, relativo ao diretório que contém os arquivos de conteúdo do aplicativo.
* `password` é a senha necessária para acessar os dados do certificado X.509 .
* `configureOptions` é uma `Action` para configurar as `HttpsConnectionAdapterOptions`. Retorna o `ListenOptions`.
* `storeName` é o repositório de certificados do qual o certificado deve ser carregado.
* `subject` é o nome da entidade do certificado.
* `allowInvalid` indica se certificados inválidos devem ser considerados, como os certificados autoassinados.
* `location` é o local do repositório do qual o certificado deve ser carregado.
* `serverCertificate` é o certificado X.509.

Em produção, HTTPS precisa ser configurado explicitamente. No mínimo, um certificado padrão precisa ser fornecido.

Configurações com suporte descritas a seguir:

* Nenhuma configuração
* Substituir o certificado padrão da configuração
* Alterar os padrões no código

*Nenhuma configuração*

O Kestrel escuta em `http://localhost:5000` e em `https://localhost:5001` (se houver um certificado padrão disponível).

Especificar URLs usando:

* A variável de ambiente `ASPNETCORE_URLS`.
* O argumento de linha de comando `--urls`.
* A chave de configuração do host `urls`.
* O método de extensão `UseUrls`.

Para obter mais informações, confira [URLs de servidor](xref:fundamentals/host/web-host#server-urls) e [Substituir configuração](xref:fundamentals/host/web-host#override-configuration).

O valor fornecido usando essas abordagens pode ser um ou mais pontos de extremidade HTTP e HTTPS (HTTPS se houver um certificado padrão). Configure o valor como uma lista separada por ponto e vírgula (por exemplo, `"Urls": "http://localhost:8000; http://localhost:8001"`).

*Substituir o certificado padrão da configuração*

[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) chama `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` por padrão ao carregar a configuração do Kestrel. Há um esquema de definições de configurações de aplicativo HTTPS padrão disponível para o Kestrel. Configure vários pontos de extremidade, incluindo URLs e os certificados a serem usados, por meio de um arquivo no disco ou de um repositório de certificados.

No exemplo de *appsettings.json* a seguir:

* Defina **AllowInvalid** como `true` para permitir o uso de certificados inválidos (por exemplo, os certificados autoassinados).
* Todo ponto de extremidade HTTPS que não especificar um certificado (**HttpsDefaultCert** no exemplo a seguir) será revertido para o certificado definido em **Certificados** > **Padrão** ou para o certificado de desenvolvimento.

```json
{
"Kestrel": {
  "EndPoints": {
    "Http": {
      "Url": "http://localhost:5000"
    },

    "HttpsInlineCertFile": {
      "Url": "https://localhost:5001",
      "Certificate": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    },

    "HttpsInlineCertStore": {
      "Url": "https://localhost:5002",
      "Certificate": {
        "Subject": "<subject; required>",
        "Store": "<certificate store; defaults to My>",
        "Location": "<location; defaults to CurrentUser>",
        "AllowInvalid": "<true or false; defaults to false>"
      }
    },

    "HttpsDefaultCert": {
      "Url": "https://localhost:5003"
    },

    "Https": {
      "Url": "https://*:5004",
      "Certificate": {
      "Path": "<path to .pfx file>",
      "Password": "<certificate password>"
      }
    }
    },
    "Certificates": {
      "Default": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    }
  }
}
```

Uma alternativa ao uso de **Caminho** e **Senha** para qualquer nó de certificado é especificar o certificado usando campos de repositório de certificados. Por exemplo, o certificado **Certificados** > **Padrão** pode ser especificado como:

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; defaults to My>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

Observações do esquema:

* Os nomes de pontos de extremidade diferenciam maiúsculas de minúsculas. Por exemplo, `HTTPS` e `Https` são válidos.
* O parâmetro `Url` é necessário para cada ponto de extremidade. O formato desse parâmetro é o mesmo que o do parâmetro de configuração de `Urls` de nível superior, exceto que ele é limitado a um único valor.
* Esses pontos de extremidade substituem aqueles definidos na configuração de `Urls` de nível superior em vez de serem adicionados a eles. Os pontos de extremidade definidos no código por meio de `Listen` são acumulados com os pontos de extremidade definidos na seção de configuração.
* A seção `Certificate` é opcional. Se a seção `Certificate` não for especificada, os padrões definidos nos cenários anteriores serão usados. Se não houver nenhum padrão disponível, o servidor gerará uma exceção e não poderá ser iniciado.
* A seção `Certificate` é compatível com os certificados de **Caminho**&ndash;**Senha** e **Entidade**&ndash;**Repositório**.
* Qualquer número de pontos de extremidade pode ser definido dessa forma, contanto que eles não causem conflitos de porta.
* `options.Configure(context.Configuration.GetSection("Kestrel"))` retorna um `KestrelConfigurationLoader` com um método `.Endpoint(string name, options => { })` que pode ser usado para complementar as definições de um ponto de extremidade configurado:

  ```csharp
  options.Configure(context.Configuration.GetSection("Kestrel"))
      .Endpoint("HTTPS", opt =>
      {
          opt.HttpsOptions.SslProtocols = SslProtocols.Tls12;
      });
  ```

  Você também pode acessar o `KestrelServerOptions.ConfigurationLoader` diretamente para continuar a iteração no carregador existente, como aquele fornecido pelo [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).

* A seção de configuração de cada ponto de extremidade está disponível nas opções no método `Endpoint` para que as configurações personalizadas possam ser lidas.
* Várias configurações podem ser carregadas chamando `options.Configure(context.Configuration.GetSection("Kestrel"))` novamente com outra seção. Somente a última configuração será usada, a menos que `Load` seja chamado explicitamente nas instâncias anteriores. O metapacote não chama `Load`, portanto, sua seção de configuração padrão pode ser substituída.
* O `KestrelConfigurationLoader` espelha a família de APIs `Listen` de `KestrelServerOptions` como sobrecargas de `Endpoint`, portanto, os pontos de extremidade de código e de configuração podem ser configurados no mesmo local. Essas sobrecargas não usam nomes e consomem somente as definições padrão da configuração.

*Alterar os padrões no código*

`ConfigureEndpointDefaults` e `ConfigureHttpsDefaults` podem ser usados para alterar as configurações padrão de `ListenOptions` e `HttpsConnectionAdapterOptions`, incluindo a substituição do certificado padrão especificado no cenário anterior. `ConfigureEndpointDefaults` e `ConfigureHttpsDefaults` devem ser chamados antes que qualquer ponto de extremidade seja configurado.

```csharp
options.ConfigureEndpointDefaults(opt =>
{
    opt.NoDelay = true;
});

options.ConfigureHttpsDefaults(httpsOptions =>
{
    httpsOptions.SslProtocols = SslProtocols.Tls12;
});
```

*Suporte do Kestrel para SNI*

A [SNI (Indicação de Nome de Servidor)](https://tools.ietf.org/html/rfc6066#section-3) pode ser usada para hospedar vários domínios no mesmo endereço IP e na mesma porta. Para que a SNI funcione, o cliente envia o nome do host da sessão segura para o servidor durante o handshake TLS para que o servidor possa fornecer o certificado correto. O cliente usa o certificado fornecido para a comunicação criptografada com o servidor durante a sessão segura que segue o handshake TLS.

O Kestrel permite a SNI por meio do retorno de chamada do `ServerCertificateSelector`. O retorno de chamada é invocado uma vez por conexão para permitir que o aplicativo inspecione o nome do host e selecione o certificado apropriado.

O suporte para SNI requer:

* Execução na estrutura de destino `netcoreapp2.1`. No `netcoreapp2.0` e no `net461`, o retorno de chamada é invocado, mas o `name` é sempre `null`. O `name` também será `null` se o cliente não fornecer o parâmetro de nome do host no handshake TLS.
* Todos os sites executados na mesma instância do Kestrel. Kestrel não é compatível com o compartilhamento de endereço IP e porta entre várias instâncias sem um proxy reverso.

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.ListenAnyIP(5005, listenOptions =>
            {
                listenOptions.UseHttps(httpsOptions =>
                {
                    var localhostCert = CertificateLoader.LoadFromStoreCert(
                        "localhost", "My", StoreLocation.CurrentUser, 
                        allowInvalid: true);
                    var exampleCert = CertificateLoader.LoadFromStoreCert(
                        "example.com", "My", StoreLocation.CurrentUser, 
                        allowInvalid: true);
                    var subExampleCert = CertificateLoader.LoadFromStoreCert(
                        "sub.example.com", "My", StoreLocation.CurrentUser, 
                        allowInvalid: true);
                    var certs = new Dictionary<string, X509Certificate2>(
                        StringComparer.OrdinalIgnoreCase);
                    certs["localhost"] = localhostCert;
                    certs["example.com"] = exampleCert;
                    certs["sub.example.com"] = subExampleCert;

                    httpsOptions.ServerCertificateSelector = (connectionContext, name) =>
                    {
                        if (name != null && certs.TryGetValue(name, out var cert))
                        {
                            return cert;
                        }

                        return exampleCert;
                    };
                });
            });
        });
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, options) =>
        {
            options.ListenAnyIP(5005, listenOptions =>
            {
                listenOptions.UseHttps(httpsOptions =>
                {
                    var localhostCert = CertificateLoader.LoadFromStoreCert(
                        "localhost", "My", StoreLocation.CurrentUser, 
                        allowInvalid: true);
                    var exampleCert = CertificateLoader.LoadFromStoreCert(
                        "example.com", "My", StoreLocation.CurrentUser, 
                        allowInvalid: true);
                    var subExampleCert = CertificateLoader.LoadFromStoreCert(
                        "sub.example.com", "My", StoreLocation.CurrentUser, 
                        allowInvalid: true);
                    var certs = new Dictionary<string, X509Certificate2>(
                        StringComparer.OrdinalIgnoreCase);
                    certs["localhost"] = localhostCert;
                    certs["example.com"] = exampleCert;
                    certs["sub.example.com"] = subExampleCert;

                    httpsOptions.ServerCertificateSelector = (connectionContext, name) =>
                    {
                        if (name != null && certs.TryGetValue(name, out var cert))
                        {
                            return cert;
                        }

                        return exampleCert;
                    };
                });
            });
        })
        .Build();
```

::: moniker-end

### <a name="bind-to-a-tcp-socket"></a>Associar a um soquete TCP

O método [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) é associado a um soquete TCP e um lambda de opções permite a configuração do certificado SSL:

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

```csharp
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Listen(IPAddress.Loopback, 5000);
            options.Listen(IPAddress.Loopback, 5001, listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testPassword");
            });
        });
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

O exemplo configura o SSL para um ponto de extremidade com [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions). Use a mesma API para definir outras configurações do Kestrel para pontos de extremidade específicos.

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a>Associar a um soquete do UNIX

Escute em um soquete Unix com [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) para melhorar o desempenho com o Nginx, como é mostrado neste exemplo:

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.ListenUnixSocket("/tmp/kestrel-test.sock");
            options.ListenUnixSocket("/tmp/kestrel-test.sock", listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testpassword");
            });
        });
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="port-0"></a>Porta 0

Quando o número da porta `0` for especificado, o Kestrel se associará dinamicamente a uma porta disponível. O exemplo a seguir mostra como determinar a qual porta o Kestrel realmente se associou no tempo de execução:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

Quando o aplicativo é executado, a saída da janela do console indica a porta dinâmica na qual o aplicativo pode ser acessado:

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a>Limitações

Configure pontos de extremidade com as seguintes abordagens:

* [UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls)
* O argumento de linha de comando `--urls`
* A chave de configuração do host `urls`
* A variável de ambiente `ASPNETCORE_URLS`

Esses métodos são úteis para fazer com que o código funcione com servidores que não sejam o Kestrel. No entanto, esteja ciente das seguintes limitações:

* O protocolo SSL não pode ser usado com essas abordagens, a menos que um certificado padrão seja fornecido na configuração do ponto de extremidade HTTPS (por exemplo, usando a configuração `KestrelServerOptions` ou um arquivo de configuração, como já foi mostrado neste tópico).
* Quando ambas as abordagens, `Listen` e `UseUrls`, são usadas ao mesmo tempo, os pontos de extremidade de `Listen` substituem os pontos de extremidade de `UseUrls`.

### <a name="iis-endpoint-configuration"></a>Configuração de ponto de extremidade do IIS

Ao usar o IIS, as associações de URL para IIS substituem as associações definidas por `Listen` ou `UseUrls`. Para obter mais informações, confira o tópico [Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Por padrão, o ASP.NET Core é associado a `http://localhost:5000`. Configure prefixos de URL e portas para o Kestrel usando:

* O método de extensão [UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls?view=aspnetcore-1.1)
* O argumento de linha de comando `--urls`
* A chave de configuração do host `urls`
* O sistema de configuração do ASP.NET Core, incluindo a variável de ambiente `ASPNETCORE_URLS`

Para obter mais informações sobre esses métodos, confira [Hospedagem](xref:fundamentals/host/index).

### <a name="iis-endpoint-configuration"></a>Configuração de ponto de extremidade do IIS

Ao usar o IIS, as associações de URL para o IIS substituam as associações definidas por `UseUrls`. Para obter mais informações, confira o tópico [Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="listenoptionsprotocols"></a>ListenOptions.Protocols

A propriedade `Protocols` estabelece os protocolos HTTP (`HttpProtocols`) habilitados em um ponto de extremidade de conexão ou para o servidor. Atribua um valor à propriedade `Protocols` com base na enumeração `HttpProtocols`.

| Valor de enumeração `HttpProtocols` | Protocolo de conexão permitido |
| -------------------------- | ----------------------------- |
| `Http1`                    | HTTP/1.1 apenas. Pode ser usado com ou sem TLS. |
| `Http2`                    | HTTP/2 apenas. Usado principalmente com TLS. Poderá ser usado sem TLS apenas se o cliente for compatível com um [Modo de conhecimento prévio](https://tools.ietf.org/html/rfc7540#section-3.4). |
| `Http1AndHttp2`            | HTTP/1.1 e HTTP/2. Requer uma conexão TLS e [ALPN (Negociação de protocolo de camada de aplicativo)](https://tools.ietf.org/html/rfc7301#section-3) para negociar o HTTP/2; caso contrário, o padrão da conexão será HTTP/1.1. |

O protocolo padrão é HTTP/1.1.

Restrições TLS para HTTP/2:

* Versão TLS 1.2 ou posterior
* Renegociação desabilitada
* Compactação desabilitada
* Tamanhos mínimos de troca de chaves efêmera:
  * ECDHE (Diffie-Hellman de curva elíptica) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; mínimo de 224 bits
  * DHE (Diffie-Hellman de campo finito) &lbrack;`TLS12`&rbrack; &ndash; mínimo de 2048 bits
* Pacote de criptografia não autorizado

Há suporte para `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; com a curva elíptica P-256 &lbrack;`FIPS186`&rbrack; por padrão.

O exemplo a seguir permite conexões HTTP/1.1 e HTTP/2 na porta 8000. As conexões são protegidas pela TLS com um certificado fornecido:

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Listen(IPAddress.Any, 8000, listenOptions =>
            {
                listenOptions.Protocols = HttpProtocols.Http1AndHttp2;
                listenOptions.UseHttps("testCert.pfx", "testPassword");
            });
        }
```

Crie opcionalmente uma implementação do `IConnectionAdapter` para filtrar os handshakes TLS por conexão para codificações específicas:

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Listen(IPAddress.Any, 8000, listenOptions =>
            {
                listenOptions.Protocols = HttpProtocols.Http1AndHttp2;
                listenOptions.UseHttps("testCert.pfx", "testPassword");
                listenOptions.ConnectionAdapters.Add(new TlsFilterAdapter());
            });
        }
```

```csharp
private class TlsFilterAdapter : IConnectionAdapter
{
    public bool IsHttps => false;

    public Task<IAdaptedConnection> OnConnectionAsync(ConnectionAdapterContext context)
    {
        var tlsFeature = context.Features.Get<ITlsHandshakeFeature>();

        // Throw NotSupportedException for any cipher algorithm that you don't 
        // wish to support. Alternatively, define and compare 
        // ITlsHandshakeFeature.CipherAlgorithm to a list of acceptable cipher 
        // suites.
        //
        // A ITlsHandshakeFeature.CipherAlgorithm of CipherAlgorithmType.Null 
        // indicates that no cipher algorithm supported by Kestrel matches the 
        // requested algorithm(s).
        if (tlsFeature.CipherAlgorithm == CipherAlgorithmType.Null)
        {
            throw new NotSupportedException("Prohibited cipher: " + tlsFeature.CipherAlgorithm);
        }

        return Task.FromResult<IAdaptedConnection>(new AdaptedConnection(context.ConnectionStream));
    }

    private class AdaptedConnection : IAdaptedConnection
    {
        public AdaptedConnection(Stream adaptedStream)
        {
            ConnectionStream = adaptedStream;
        }

        public Stream ConnectionStream { get; }

        public void Dispose()
        {
        }
    }
}
```

*Definir o protocolo com base na configuração*

[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) chama `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` por padrão ao carregar a configuração do Kestrel.

No exemplo *appsettings.json* a seguir, um protocolo de conexão padrão (HTTP/1.1 e HTTP/2) é estabelecido para todos os pontos de extremidade do Kestrel:

```json
{
  "Kestrel": {
    "EndPointDefaults": {
      "Protocols": "Http1AndHttp2"
    }
  }
}
```

O arquivo de configuração de exemplo a seguir estabelece um protocolo de conexão para um ponto de extremidade específico:

```json
{
  "Kestrel": {
    "EndPoints": {
      "HttpsDefaultCert": {
        "Url": "https://localhost:5001",
        "Protocols": "Http1AndHttp2"
      }
    }
  }
}
```

Os protocolos especificados no código substituem os valores definidos pela configuração.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="transport-configuration"></a>Configuração de transporte

Com a liberação do ASP.NET Core 2.1, o transporte padrão do Kestrel deixa de ser baseado no Libuv, baseando-se agora em soquetes gerenciados. Essa é uma alteração da falha para aplicativos ASP.NET Core 2.0 que atualizam para o 2.1 que chamam [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv) e dependem de um dos seguintes pacotes:

* [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (referência direta de pacote)
* [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

Para projetos do ASP.NET Core 2.1 ou posteriores que usam o [metapacote Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) e exigem o uso de Libuv:

* Adicione uma dependência para o pacote [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) ao arquivo de projeto do aplicativo:

    ```xml
    <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv" 
                      Version="<LATEST_VERSION>" />
    ```

* Chame [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv):

    ```csharp
    public class Program
    {
        public static void Main(string[] args)
        {
            CreateWebHostBuilder(args).Build().Run();
        }

        public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .UseLibuv()
                .UseStartup<Startup>();
    }
    ```

::: moniker-end

### <a name="url-prefixes"></a>Prefixos de URL

Ao usar `UseUrls`, o argumento de linha de comando `--urls`, a chave de configuração de host `urls` ou a variável de ambiente `ASPNETCORE_URLS`, os prefixos de URL podem estar em um dos formatos a seguir.

::: moniker range=">= aspnetcore-2.0"

Somente os prefixos de URL HTTP são válidos. O Kestrel não é compatível com SSL ao configurar associações de URL usando `UseUrls`.

* Endereço IPv4 com o número da porta

  ```
  http://65.55.39.10:80/
  ```

  `0.0.0.0` é um caso especial que associa a todos os endereços IPv4.

* Endereço IPv6 com número da porta

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  `[::]` é o equivalente do IPv6 ao IPv4 `0.0.0.0`.

* Nome do host com o número da porta

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  Os nomes de host `*` e `+` não são especiais. Tudo o que não é reconhecido como um endereço IP ou um `localhost` válido é associado a todos os IPs IPv6 e IPv4. Para associar nomes de host diferentes a diferentes aplicativos ASP.NET Core na mesma porta, use o [HTTP.sys](xref:fundamentals/servers/httpsys) ou um servidor proxy reverso, como o IIS, o Nginx ou o Apache.

  > [!WARNING]
  > Se você não está usando um proxy reverso com a filtragem de host habilitada, habilite a [filtragem de host](#host-filtering).

* Nome do `localhost` do host com o número da porta ou o IP de loopback com o número da porta

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  Quando o `localhost` for especificado, o Kestrel tentará se associar às interfaces de loopback IPv4 e IPv6. Se a porta solicitada está sendo usada por outro serviço em uma das interfaces de loopback, o Kestrel falha ao ser iniciado. Se uma das interfaces de loopback não estiver disponível por qualquer outro motivo (geralmente porque não há suporte para o IPv6), o Kestrel registra um aviso em log.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* Endereço IPv4 com o número da porta

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  `0.0.0.0` é um caso especial que associa a todos os endereços IPv4.

* Endereço IPv6 com número da porta

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  https://[0:0:0:0:0:ffff:4137:270a]:443/
  ```

  `[::]` é o equivalente do IPv6 ao IPv4 `0.0.0.0`.

* Nome do host com o número da porta

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  Nomes de host, `*` e `+` não são especiais. Tudo o que não é um endereço IP ou um `localhost` reconhecido é associado a todos os IPs IPv6 e IPv4. Para associar nomes de host diferentes a diferentes aplicativos ASP.NET Core na mesma porta, use [WebListener](xref:fundamentals/servers/weblistener) ou um servidor proxy reverso, como o IIS, o Nginx ou o Apache.

* Nome do `localhost` do host com o número da porta ou o IP de loopback com o número da porta

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  Quando o `localhost` for especificado, o Kestrel tentará se associar às interfaces de loopback IPv4 e IPv6. Se a porta solicitada está sendo usada por outro serviço em uma das interfaces de loopback, o Kestrel falha ao ser iniciado. Se uma das interfaces de loopback não estiver disponível por qualquer outro motivo (geralmente porque não há suporte para o IPv6), o Kestrel registra um aviso em log.

* Soquete do UNIX

  ```
  http://unix:/run/dan-live.sock
  ```

**Porta 0**

Quando o número da porta `0` for especificado, o Kestrel se associará dinamicamente a uma porta disponível. A associação à porta `0` é permitida para qualquer nome do host ou IP, exceto `localhost`.

Quando o aplicativo é executado, a saída da janela do console indica a porta dinâmica na qual o aplicativo pode ser acessado:

```console
Now listening on: http://127.0.0.1:48508
```

**Prefixos de URL para o SSL**

Se você estiver chamando o método de extensão `UseHttps`, lembre-se de incluir os prefixos de URL com `https:`:

```csharp
var host = new WebHostBuilder()
    .UseKestrel(options =>
    {
        options.UseHttps("testCert.pfx", "testPassword");
    })
   .UseUrls("http://localhost:5000", "https://localhost:5001")
   .UseContentRoot(Directory.GetCurrentDirectory())
   .UseStartup<Startup>()
   .Build();
```

> [!NOTE]
> HTTPS e HTTP não podem ser hospedados na mesma porta.

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

::: moniker-end

## <a name="host-filtering"></a>Filtragem de host

Embora o Kestrel permita a configuração com base em prefixos como `http://example.com:5000`, ele geralmente ignora o nome do host. O host `localhost` é um caso especial usado para a associação a endereços de loopback. Todo host que não for um endereço IP explícito será associado a todos os endereços IP públicos. Nenhuma dessas informações é usada para validar os cabeçalhos do `Host` da solicitação.

::: moniker range="< aspnetcore-2.0"

Como uma solução alternativa, o host por trás de um proxy reverso com filtragem de cabeçalho do host. Esse é o único cenário compatível com Kestrel no ASP.NET Core 1.x.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Como uma solução alternativa, use middleware para filtrar as solicitações pelo cabeçalho `Host`:

```csharp
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Logging;
using Microsoft.Extensions.Primitives;
using Microsoft.Net.Http.Headers;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

// A normal middleware would provide an options type, config binding, extension methods, etc..
// This intentionally does all of the work inside of the middleware so it can be
// easily copy-pasted into docs and other projects.
public class HostFilteringMiddleware
{
    private readonly RequestDelegate _next;
    private readonly IList<string> _hosts;
    private readonly ILogger<HostFilteringMiddleware> _logger;

    public HostFilteringMiddleware(RequestDelegate next, IConfiguration config, ILogger<HostFilteringMiddleware> logger)
    {
        if (config == null)
        {
            throw new ArgumentNullException(nameof(config));
        }

        _next = next ?? throw new ArgumentNullException(nameof(next));
        _logger = logger ?? throw new ArgumentNullException(nameof(logger));

        // A semicolon separated list of host names without the port numbers.
        // IPv6 addresses must use the bounding brackets and be in their normalized form.
        _hosts = config["AllowedHosts"]?.Split(new[] { ';' }, StringSplitOptions.RemoveEmptyEntries);
        if (_hosts == null || _hosts.Count == 0)
        {
            throw new InvalidOperationException("No configuration entry found for AllowedHosts.");
        }
    }

    public Task Invoke(HttpContext context)
    {
        if (!ValidateHost(context))
        {
            context.Response.StatusCode = 400;
            _logger.LogDebug("Request rejected due to incorrect Host header.");
            return Task.CompletedTask;
        }

        return _next(context);
    }

    // This does not duplicate format validations that are expected to be performed by the host.
    private bool ValidateHost(HttpContext context)
    {
        StringSegment host = context.Request.Headers[HeaderNames.Host].ToString().Trim();

        if (StringSegment.IsNullOrEmpty(host))
        {
            // Http/1.0 does not require the Host header.
            // Http/1.1 requires the header but the value may be empty.
            return true;
        }

        // Drop the port

        var colonIndex = host.LastIndexOf(':');

        // IPv6 special case
        if (host.StartsWith("[", StringComparison.Ordinal))
        {
            var endBracketIndex = host.IndexOf(']');
            if (endBracketIndex < 0)
            {
                // Invalid format
                return false;
            }
            if (colonIndex < endBracketIndex)
            {
                // No port, just the IPv6 Host
                colonIndex = -1;
            }
        }

        if (colonIndex > 0)
        {
            host = host.Subsegment(0, colonIndex);
        }

        foreach (var allowedHost in _hosts)
        {
            if (StringSegment.Equals(allowedHost, host, StringComparison.OrdinalIgnoreCase))
            {
                return true;
            }

            // Sub-domain wildcards: *.example.com
            if (allowedHost.StartsWith("*.", StringComparison.Ordinal) && host.Length >= allowedHost.Length)
            {
                // .example.com
                var allowedRoot = new StringSegment(allowedHost, 1, allowedHost.Length - 1);

                var hostRoot = host.Subsegment(host.Length - allowedRoot.Length, allowedRoot.Length);
                if (hostRoot.Equals(allowedRoot, StringComparison.OrdinalIgnoreCase))
                {
                    return true;
                }
            }
        }

        return false;
    }
}
```

Registre o `HostFilteringMiddleware` anterior em `Startup.Configure`. Observe que a [ordem do registro de middleware](xref:fundamentals/middleware/index#order) é importante. O registro deve ocorrer imediatamente após o registro do middleware de diagnóstico (por exemplo, `app.UseExceptionHandler`).

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
        app.UseBrowserLink();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
    }

    app.UseMiddleware<HostFilteringMiddleware>();

    app.UseMvcWithDefaultRoute();
}
```

O middleware espera uma chave `AllowedHosts` em *appsettings.json*/*appsettings.\<EnvironmentName>.json*. O valor dessa chave é uma lista separada por ponto e vírgula de nomes de host sem números de porta:

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

Como uma solução alternativa, use o Middleware de Filtragem de Host. Middleware de Filtragem de Host é fornecido pelo pacote [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering), que está incluído no [metapacote Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 ou posterior). O middleware é adicionado por [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), que chama [AddHostFiltering](/dotnet/api/microsoft.aspnetcore.builder.hostfilteringservicesextensions.addhostfiltering):

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

Middleware de Filtragem de Host está desabilitado por padrão. Para habilitar o middleware, defina uma chave `AllowedHosts` em *appSettings. JSON*/*appsettings.\<EnvironmentName>.json*. O valor dessa chave é uma lista separada por ponto e vírgula de nomes de host sem números de porta:

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

*appsettings.json*:

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> [Middleware de Cabeçalhos Encaminhados](xref:host-and-deploy/proxy-load-balancer) também tem uma opção [ForwardedHeadersOptions.AllowedHosts](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.allowedhosts). Middleware de Cabeçalhos Encaminhados e Middleware de filtragem de Host têm funcionalidades semelhantes para cenários diferentes. Definir `AllowedHosts` com Middleware de Cabeçalhos Encaminhados é apropriado quando o cabeçalho do Host não é preservado ao encaminhar solicitações com um servidor proxy reverso ou balanceador de carga. Definir `AllowedHosts` com Middleware de Filtragem de Host é apropriado quando Kestrel é usado como um servidor de borda voltado para o público ou quando o cabeçalho de Host é encaminhado diretamente.
>
> Para obter mais informações sobre Middleware de Cabeçalhos Encaminhados, veja [Configurar o ASP.NET Core para funcionar com servidores proxy e balanceadores de carga](xref:host-and-deploy/proxy-load-balancer).

::: moniker-end

## <a name="additional-resources"></a>Recursos adicionais

* [Impor o HTTPS](xref:security/enforcing-ssl)
* [Código-fonte do kestrel](https://github.com/aspnet/KestrelHttpServer)
* [RFC 7230: Sintaxe e roteamento da mensagem (Seção 5.4: Host)](https://tools.ietf.org/html/rfc7230#section-5.4)
* [Configurar o ASP.NET Core para trabalhar com servidores proxy e balanceadores de carga](xref:host-and-deploy/proxy-load-balancer)
