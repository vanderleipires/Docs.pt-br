---
title: "Implementação do servidor web kestrel no núcleo do ASP.NET"
author: tdykstra
description: Apresenta Kestrel, o servidor web de plataforma cruzada para o ASP.NET Core com base em libuv.
ms.author: tdykstra
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/kestrel
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3695a6a127f77bd90538d72af6112ccf507f3482
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-kestrel-web-server-implementation-in-aspnet-core"></a>Introdução a implementação do servidor web Kestrel no núcleo do ASP.NET

Por [Tom Dykstra](https://github.com/tdykstra), [Ross Carlos](https://github.com/Tratcher), e [Stephen Halter](https://twitter.com/halter73)

Kestrel é uma plataforma cruzada [servidor web para o ASP.NET Core](index.md) com base em [libuv](https://github.com/libuv/libuv), uma biblioteca de e/s assíncrona de plataforma cruzada. Kestrel é o servidor web que está incluído por padrão nos modelos de projeto do ASP.NET Core. 

Kestrel suporta os seguintes recursos:

  * HTTPS
  * Atualização opaca usada para habilitar [WebSockets](https://github.com/aspnet/websockets)
  * Soquetes de UNIX de alto desempenho atrás Nginx 

Kestrel tem suporte em todas as plataformas e versões que dá suporte ao .NET Core.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[Exibir ou baixar o código de exemplo para 2. x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[Exibir ou baixar o exemplo de código 1. x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

---

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a>Quando usar Kestrel com um proxy reverso

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Você pode usar o Kestrel sozinho ou com um *servidor proxy reverso* como IIS, Nginx ou Apache. Um servidor proxy reverso recebe solicitações HTTP da Internet e as encaminha para o Kestrel após algum tratamento preliminar.

![O Kestrel se comunica diretamente com a Internet, sem um servidor proxy reverso](kestrel/_static/kestrel-to-internet2.png)

![O Kestrel se comunica indiretamente com a Internet através de um servidor proxy reverso, tal como o IIS, o Nginx ou o Apache](kestrel/_static/kestrel-to-internet.png)

Qualquer configuração &mdash; com ou sem um servidor proxy reverso &mdash; também pode ser usada se o Kestrel é exposto somente a uma rede interna.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Se seu aplicativo aceita solicitações somente de uma rede interna, você pode usar o Kestrel sozinho.

![O Kestrel se comunica diretamente com a rede interna](kestrel/_static/kestrel-to-internal.png)

Se você expuser seu aplicativo à Internet, você deverá usar o IIS, o Nginx ou o Apache como um *servidor proxy reverso*. Um servidor proxy reverso recebe solicitações HTTP da Internet e as encaminha para o Kestrel após algum tratamento preliminar.

![O Kestrel se comunica indiretamente com a Internet através de um servidor proxy reverso, tal como o IIS, o Nginx ou o Apache](kestrel/_static/kestrel-to-internet.png)

Um proxy reverso é necessário para implantações de borda (expostas ao tráfego da Internet) por motivos de segurança. As versões 1.x do Kestrel não têm um conjunto completo de proteção contra ataques. Isso inclui, mas não está limitado a tempos limite apropriado, limites de tamanho e os limites de conexão simultâneas.

---

Um cenário que requer um proxy reverso é quando você tiver vários aplicativos que compartilham o mesmo IP e porta em execução em um único servidor. Isso não funcionar com Kestrel diretamente como Kestrel não dá suporte a compartilhar o mesmo IP e porta entre vários processos. Quando você configura Kestrel para escutar em uma porta, ele trata todo o tráfego para essa porta, independentemente de cabeçalho de host. Um proxy reverso que pode compartilhar portas, em seguida, encaminhe para Kestrel em um único IP e porta.

Mesmo se um servidor proxy reverso não é necessário, usando um pode ser uma boa opção por outros motivos:

* Ele pode limitar a área da superfície exposta.
* Ele fornece uma camada de configuração e proteção adicional opcional.
* Ele pode integrar melhor com a infraestrutura existente.
* Ele simplifica a configuração SSL e balanceamento de carga. Somente o servidor de proxy reverso requer um certificado SSL, e esse servidor pode se comunicar com seus servidores de aplicativos na rede interna usando HTTP simples.

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a>Como usar Kestrel em aplicativos do ASP.NET Core

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

O [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) pacote está incluído no [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).

Modelos de projeto do ASP.NET Core usam Kestrel por padrão. Em *Program.cs*, as chamadas de código de modelo `CreateDefaultBuilder`, que chama [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) em segundo plano.

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

Se você precisar configurar as opções de Kestrel, chame `UseKestrel` na *Program.cs* conforme mostrado no exemplo a seguir:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Instalar o [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) pacote NuGet.

Chamar o [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) método de extensão no `WebHostBuilder` no seu `Main` método, especificando um [opções Kestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) que você precisa, como mostrado na próxima seção.

[!code-csharp[](kestrel/sample1/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a>Opções de kestrel

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

O servidor de web Kestrel tem opções de configuração de restrição são especialmente úteis em implantações para a Internet. Aqui estão alguns dos limites que você pode definir:

- Número máximo de conexões de cliente
- Tamanho máximo do corpo da solicitação
- Taxa de dados mínima do corpo da solicitação

Você define essas restrições e outras o `Limits` propriedade o [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) classe. O `Limits` propriedade contém uma instância do [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) classe. 

**Conexões de cliente máximo**

O número máximo de conexões TCP abertas simultâneas pode ser definido para o aplicativo inteiro com o código a seguir:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=3-4)]

Há um limite separado para conexões que foram atualizados de HTTP ou HTTPS para outro protocolo (por exemplo, em uma solicitação WebSocket). Depois que uma conexão é atualizado, ele não é contado em relação a `MaxConcurrentConnections` limite. 

O número máximo de conexões é ilimitado (null) por padrão.

**Tamanho do corpo da solicitação máxima**

O tamanho do corpo da solicitação máximo padrão é 30.000.000 bytes, que é aproximadamente 28.6 MB. 

A maneira recomendada para substituir o limite em um aplicativo ASP.NET MVC de núcleo é usar o [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) atributo em um método de ação:

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

Aqui está um exemplo que mostra como configurar o limite para o aplicativo inteiro, cada solicitação:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=5)]

Você pode substituir a configuração em uma solicitação específica no middleware:

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=3-4)]
 
Uma exceção é gerada se você tentar configurar o limite de uma solicitação depois que o aplicativo começou a ler a solicitação. Há um `IsReadOnly` propriedade que indica se o `MaxRequestBodySize` propriedade está no estado somente leitura, que significa que é muito tarde para configurar o limite.

**Taxa de dados de corpo de solicitação mínimo**

Kestrel verifica a cada segundo se dados estão chegando a taxa especificada em bytes/segundo. Se a taxa de cair abaixo do mínimo, a conexão é atingiu o tempo limite. O período de cortesia é a quantidade de tempo que Kestrel dá ao cliente para aumentar sua taxa de envio até o mínimo; a taxa não é verificada durante esse período. O período de cortesia ajuda a evitar a remoção de conexões que inicialmente estão enviando dados em uma taxa baixa devido a partida lenta do TCP.

A taxa mínima de padrão é 240 bytes/segundo, com um período de cortesia de 5 segundos.

Uma taxa mínima também se aplica à resposta. O código para definir o limite de solicitação e o limite de resposta é o mesmo, exceto tendo `RequestBody` ou `Response` nos nomes de propriedade e a interface. 

Aqui está um exemplo que mostra como configurar as taxas de dados mínimo no *Program.cs*:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=6-9)]

Você pode configurar as taxas por solicitação no middleware:

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=5-8)]

Para obter informações sobre outras opções de Kestrel, consulte as seguintes classes:

* [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)
* [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)
* [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Para obter informações sobre as opções de Kestrel, consulte [KestrelServerOptions classe](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).

---

### <a name="endpoint-configuration"></a>Configuração de ponto de extremidade

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Por padrão o ASP.NET Core associa a `http://localhost:5000`. Configurar portas para Kestrel escutar em chamando e prefixos de URL `Listen` ou `ListenUnixSocket` métodos em `KestrelServerOptions`. (`UseUrls`, o `urls` argumento de linha de comando e a variável de ambiente ASPNETCORE_URLS também funcionam mas têm limitações observadas [posteriormente neste artigo](#useurls-limitations).)

**Vincular a um soquete TCP**

O `Listen` método vincula a um soquete TCP, e um lambda de opções permite que você configure um certificado SSL:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

Observe como este exemplo configura o SSL para um ponto de extremidade específico usando [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs). Você pode usar a mesma API para definir outras configurações de Kestrel para pontos de extremidade específicos.

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

**Vincular a um soquete do Unix**

Você pode escutar em um soquete de Unix para melhorar o desempenho com Nginx, conforme mostrado neste exemplo:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_UnixSocket)]

**Porta 0**

Se você especificar o número de porta 0, Kestrel dinamicamente associa a uma porta disponível. O exemplo a seguir mostra como determinar qual porta Kestrel realmente associado em tempo de execução:

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Configure&highlight=3,13,16-17)]

<a id="useurls-limitations"></a>

**Limitações de UseUrls**

Você pode configurar pontos de extremidade chamando o `UseUrls` método ou usando o `urls` argumento de linha de comando ou a variável de ambiente ASPNETCORE_URLS. Esses métodos são úteis se você quiser que seu código para trabalhar com servidores que não sejam Kestrel. No entanto, você deve estar atento essas limitações:

* Você não pode usar o SSL com esses métodos.
* Se você usar o `Listen` método e `UseUrls`, o `Listen` pontos de extremidade de substituem o `UseUrls` pontos de extremidade.

**Configuração de ponto de extremidade para o IIS**

Se você usar o IIS, as associações de URL para o IIS substituirão quaisquer associações que você definir chamando `Listen` ou `UseUrls`. Para obter mais informações, consulte [Introdução ao ASP.NET Core módulo](aspnet-core-module.md).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Por padrão o ASP.NET Core associa a `http://localhost:5000`. Você pode configurar os prefixos de URL e portas para Kestrel escutar usando o `UseUrls` método de extensão, o `urls` argumento de linha de comando ou o sistema de configuração do ASP.NET Core. Para obter mais informações sobre esses métodos, consulte [hospedagem](../../fundamentals/hosting.md). Para obter informações sobre como funciona a associação de URL quando você usar o IIS como um proxy reverso, consulte [ASP.NET Core módulo](aspnet-core-module.md). 

---

### <a name="url-prefixes"></a>Prefixos de URL

Se você chamar `UseUrls` ou use o `urls` argumento de linha de comando ou variável de ambiente ASPNETCORE_URLS, os prefixos de URL podem estar em qualquer um dos formatos a seguir. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Somente prefixos de URL HTTP são válidos. Kestrel não dá suporte a SSL quando você configurar as ligações de URL usando `UseUrls`.

* Endereço IPv4 com o número da porta

  ```
  http://65.55.39.10:80/
  ```

  0.0.0.0 é um caso especial que associa a todos os endereços IPv4.


* Endereço IPv6 com número de porta

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  ```

  [:] é o equivalente de IPv6 do IPv4 0.0.0.0.


* Nome de host com o número da porta

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  Nomes de host *, e +, não são especiais. Tudo o que não é um endereço IP ou o "localhost" reconhecido associará a todos os IPs de IPv6 e IPv4. Se você precisa associar nomes de host diferentes para diferentes aplicativos ASP.NET Core na mesma porta, use [HTTP.sys](httpsys.md) ou um servidor proxy reverso, como IIS, Nginx ou Apache.

* Nome do "Localhost" com o IP de loopback ou de número de porta com o número da porta

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  Quando `localhost` for especificado, Kestrel tenta associar a interfaces de loopback de IPv4 e IPv6. Se a porta solicitada está em uso por outro serviço em cada interface de loopback, Kestrel falhar ao iniciar. Se qualquer uma das interfaces de loopback não estiver disponível por qualquer outro motivo (mais comumente porque não há suporte para IPv6), Kestrel registra um aviso. 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* Endereço IPv4 com o número da porta

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  0.0.0.0 é um caso especial que associa a todos os endereços IPv4.


* Endereço IPv6 com número de porta

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  https://[0:0:0:0:0:ffff:4137:270a]:443/ 
  ```

  [:] é o equivalente de IPv6 do IPv4 0.0.0.0.


* Nome de host com o número da porta

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  Nomes de host \*, e + não são especiais. Tudo o que não é um endereço IP ou o "localhost" reconhecido associa a todos os IPs de IPv6 e IPv4. Se você precisa associar nomes de host diferentes para diferentes aplicativos ASP.NET Core na mesma porta, use [WebListener](weblistener.md) ou um servidor proxy reverso, como IIS, Nginx ou Apache.

* Nome do "Localhost" com o IP de loopback ou de número de porta com o número da porta

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  Quando `localhost` for especificado, Kestrel tenta associar a interfaces de loopback de IPv4 e IPv6. Se a porta solicitada está em uso por outro serviço em cada interface de loopback, Kestrel falhar ao iniciar. Se qualquer uma das interfaces de loopback não estiver disponível por qualquer outro motivo (mais comumente porque não há suporte para IPv6), Kestrel registra um aviso. 

* Soquete de UNIX

  ```
  http://unix:/run/dan-live.sock  
  ```

**Porta 0**

Se você especificar o número de porta 0, Kestrel dinamicamente associa a uma porta disponível. Associando a porta 0 é permitido para qualquer nome de host ou IP exceto `localhost` nome.

O exemplo a seguir mostra como determinar qual porta Kestrel realmente associado em tempo de execução:

[!code-csharp[](kestrel/sample1/Startup.cs?name=snippet_Configure)]

**Prefixos de URL para SSL**

Certifique-se de incluir os prefixos de URL com `https:` se você chamar o `UseHttps` método de extensão, conforme mostrado abaixo.

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
> HTTPS e HTTP não podem ser hospedado na mesma porta.

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

---
## <a name="next-steps"></a>Próximas etapas

Para obter mais informações, consulte os seguintes recursos:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

* [Aplicativo de exemplo para 2. x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)
* [Código-fonte kestrel](https://github.com/aspnet/KestrelHttpServer)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* [Aplicativo de exemplo para 1. x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)
* [Código-fonte kestrel](https://github.com/aspnet/KestrelHttpServer)

---
