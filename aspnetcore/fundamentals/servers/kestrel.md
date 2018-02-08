---
title: "Implementação do servidor Web Kestrel no ASP.NET Core"
author: tdykstra
description: Apresenta o Kestrel, o servidor Web multiplataforma para o ASP.NET Core baseado no libuv.
manager: wpickett
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 08/02/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/kestrel
ms.openlocfilehash: bfe7644891296c7c3485c9a870d90951ba87e9e8
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-kestrel-web-server-implementation-in-aspnet-core"></a>Introdução à implementação do servidor Web Kestrel no ASP.NET Core

Por [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher) e [Stephen Halter](https://twitter.com/halter73)

O Kestrel é um [servidor Web multiplataforma para o ASP.NET Core](index.md) baseado no [libuv](https://github.com/libuv/libuv), uma biblioteca de E/S assíncrona multiplataforma. O Kestrel é o servidor Web que está incluído por padrão em modelos de projeto do ASP.NET Core. 

O Kestrel dá suporte aos seguintes recursos:

  * HTTPS
  * Atualização do Opaque usado para habilitar o [WebSockets](https://github.com/aspnet/websockets)
  * Soquetes do UNIX para alto desempenho protegidos pelo Nginx 

Há suporte para o Kestrel em todas as plataformas e versões compatíveis com o .NET Core.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[Exibir ou baixar um código de exemplo para o 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[Exibir ou baixar um código de exemplo para o 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

---

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a>Quando usar o Kestrel com um proxy reverso

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

Um proxy reverso é necessário para implantações de borda (expostas ao tráfego da Internet) por motivos de segurança. As versões 1.x do Kestrel não têm um conjunto completo de proteção contra ataques. Isso inclui, mas não se limita aos tempos limite, limites de tamanho e limites de conexões simultâneas apropriados.

---

Um cenário que exige um proxy reverso é quando você tem vários aplicativos que compartilham o mesmo IP e porta em execução em um único servidor. Isso não funciona com o Kestrel diretamente, pois o Kestrel não dá suporte ao compartilhamento do mesmo IP e porta entre vários processos. Quando você configura o Kestrel para escutar em uma porta, ele manipula todo o tráfego para essa porta, independentemente do cabeçalho de host. Um proxy reverso que pode compartilhar portas, em seguida, precisa encaminhá-las para o Kestrel em um IP e porta exclusivos.

Mesmo se um servidor proxy reverso não é necessário, o uso de um pode ser uma boa opção por outros motivos:

* Ele pode limitar a área da superfície exposta.
* Fornece uma camada de configuração e proteção adicional opcional.
* Pode ser integrado melhor à infraestrutura existente.
* Simplifica o balanceamento de carga e a configuração do SSL. Somente o servidor proxy reverso exige um certificado SSL e esse servidor pode se comunicar com os servidores de aplicativos na rede interna usando HTTP simples.

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a>Como usar o Kestrel em aplicativos ASP.NET Core

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

O pacote [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) está incluído no [metapacote Microsoft.AspNetCore.All](xref:fundamentals/metapackage).

Os modelos de projeto do ASP.NET Core usam o Kestrel por padrão. Em *Program.cs*, o código de modelo chama `CreateDefaultBuilder`, que chama [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) em segundo plano.

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

Caso precise configurar as opções do Kestrel, chame `UseKestrel` em *Program.cs*, conforme mostrado no seguinte exemplo:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Instale o pacote NuGet [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/).

Chame o método de extensão [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) em `WebHostBuilder` no método `Main`, especificando as [opções do Kestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) necessárias, conforme mostrado na próxima seção.

[!code-csharp[](kestrel/sample1/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a>Opções do Kestrel

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

O servidor Web do Kestrel tem opções de configuração de restrição especialmente úteis em implantações para a Internet. Estes são alguns dos limites que podem ser definidos:

- Número máximo de conexões de cliente
- Tamanho máximo do corpo da solicitação
- Taxa de dados mínima do corpo da solicitação

Defina essas restrições e outras na propriedade `Limits` da classe [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs). A propriedade `Limits` contém uma instância da classe [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs). 

**Número máximo de conexões de cliente**

O número máximo de conexões TCP abertas simultâneas pode ser definido para todo o aplicativo com o seguinte código:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=3-4)]

Há um limite separado para conexões que foram atualizadas do HTTP ou HTTPS para outro protocolo (por exemplo, em uma solicitação do WebSockets). Depois que uma conexão é atualizada, ela não é contada em relação ao limite de `MaxConcurrentConnections`. 

O número máximo de conexões é ilimitado (nulo) por padrão.

**Tamanho máximo do corpo da solicitação**

O tamanho máximo do corpo da solicitação padrão é de 30.000.000 bytes, que equivale aproximadamente a 28,6 MB. 

A maneira recomendada para substituir o limite em um aplicativo ASP.NET Core MVC é usar o atributo [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) em um método de ação:

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

Este é um exemplo que mostra como configurar a restrição para todo o aplicativo, em cada solicitação:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=5)]

Substitua a configuração em uma solicitação específica no middleware:

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=3-4)]
 
Uma exceção é gerada se você tenta configurar o limite de uma solicitação depois que o aplicativo iniciou a leitura da solicitação. Há uma propriedade `IsReadOnly` que indica se a propriedade `MaxRequestBodySize` está no estado somente leitura, o que significa que é tarde demais para configurar o limite.

**Taxa de dados mínima do corpo da solicitação**

O Kestrel verifica a cada segundo se os dados estão sendo recebidos na taxa especificada em bytes/segundo. Se a taxa cair abaixo do mínimo, a conexão atingirá o tempo limite. O período de cortesia é o tempo que o Kestrel fornece ao cliente para aumentar sua taxa de envio até o mínimo; a taxa não é verificada durante esse período. O período de cortesia ajuda a evitar a remoção de conexões que inicialmente enviam dados em uma taxa baixa devido ao início lento do TCP.

A taxa mínima padrão é de 240 bytes/segundo, com um período de cortesia de 5 segundos.

Uma taxa mínima também se aplica à resposta. O código para definir o limite de solicitação e o limite de resposta é o mesmo, exceto por ter `RequestBody` ou `Response` nos nomes da propriedade e da interface. 

Este é um exemplo que mostra como configurar as taxas mínima de dados em *Program.cs*:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=6-9)]

Configure as taxas por solicitação no middleware:

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=5-8)]

Para obter informações sobre outras opções do Kestrel, consulte as seguintes classes:

* [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)
* [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)
* [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Para obter informações sobre as opções do Kestrel, consulte [Classe KestrelServerOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).

---

### <a name="endpoint-configuration"></a>Configuração do ponto de extremidade

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Por padrão, o ASP.NET Core é associado a `http://localhost:5000`. Configure prefixos de URL e portas nas quais o Kestrel escuta chamando métodos `Listen` ou `ListenUnixSocket` em `KestrelServerOptions`. (`UseUrls`, o argumento de linha de comando `urls` e a variável de ambiente ASPNETCORE_URLS também funcionam, mas têm as limitações indicadas [mais adiante neste artigo](#useurls-limitations).)

**Associar a um soquete TCP**

O método `Listen` é associado a um soquete TCP e um lambda de opções permite que você configure um certificado SSL:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

Observe como esse exemplo configura o SSL de um ponto de extremidade específico usando [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs). Use a mesma API para definir outras configurações do Kestrel para pontos de extremidade específicos.

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

**Associar a um soquete do UNIX**

Escute em um soquete do UNIX para um melhor desempenho com o Nginx, conforme mostrado neste exemplo:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_UnixSocket)]

**Porta 0**

Se você especificar o número de porta 0, o Kestrel associa-o dinamicamente a uma porta disponível. O seguinte exemplo mostra como determinar à qual porta o Kestrel realmente está associado em tempo de execução:

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Configure&highlight=3,13,16-17)]

<a id="useurls-limitations"></a>

**Limitações de UseUrls**

Configure pontos de extremidade chamando o método `UseUrls` ou usando o argumento de linha de comando `urls` ou a variável de ambiente ASPNETCORE_URLS. Esses métodos são úteis se você deseja que o código funcione com servidores que não sejam o Kestrel. No entanto, esteja ciente dessas limitações:

* Não é possível usar o SSL com esses métodos.
* Se você usar o método `Listen` e `UseUrls`, os pontos de extremidade `Listen` substituirão os pontos de extremidade `UseUrls`.

**Configuração de ponto de extremidade para o IIS**

Se você usar o IIS, as associações de URL do IIS substituirão as associações definidas com uma chamada a `Listen` ou `UseUrls`. Para obter mais informações, consulte [Introdução ao Módulo do ASP.NET Core](aspnet-core-module.md).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Por padrão, o ASP.NET Core é associado a `http://localhost:5000`. Configure prefixos de URL e portas nas quais o Kestrel escuta usando o método de extensão `UseUrls`, o argumento de linha de comando `urls` ou o sistema de configuração do ASP.NET Core. Para obter mais informações sobre esses métodos, consulte [Hospedagem](../../fundamentals/hosting.md). Para obter informações sobre como funciona a associação de URL quando o IIS é usado como um proxy reverso, consulte [Módulo do ASP.NET Core](aspnet-core-module.md). 

---

### <a name="url-prefixes"></a>Prefixos de URL

Se você chamar `UseUrls` ou usar o argumento de linha de comando `urls` ou a variável de ambiente ASPNETCORE_URLS, os prefixos de URL poderão estar em um dos formatos a seguir. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Somente prefixos de URL HTTP são válidos. O Kestrel não dá suporte ao SSL quando associações de URL são configuradas com `UseUrls`.

* Endereço IPv4 com o número da porta

  ```
  http://65.55.39.10:80/
  ```

  0.0.0.0 é um caso especial que é associado a todos os endereços IPv4.


* Endereço IPv6 com número da porta

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  ```

  [::] é o equivalente de IPv6 do IPv4 0.0.0.0.


* Nome do host com o número da porta

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  Nomes de host, * e +, não são especiais. Tudo o que não é um endereço IP reconhecido ou o "localhost" será associado a todos os IPs do IPv4 e IPv6. Se você precisa associar diferentes nomes de host a diferentes aplicativos ASP.NET Core na mesma porta, use o [HTTP.sys](httpsys.md) ou um servidor proxy reverso, como o IIS, Nginx ou Apache.

* Nome do "localhost" com o número da porta ou IP de loopback com o número da porta

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  Quando `localhost` é especificado, o Kestrel tenta associar a interfaces de loopback IPv4 e IPv6. Se a porta solicitada está sendo usada por outro serviço em uma das interfaces de loopback, o Kestrel falha ao ser iniciado. Se uma das interfaces de loopback não estiver disponível por qualquer outro motivo (geralmente porque não há suporte para o IPv6), o Kestrel registra um aviso em log. 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* Endereço IPv4 com o número da porta

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  0.0.0.0 é um caso especial que é associado a todos os endereços IPv4.


* Endereço IPv6 com número da porta

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  https://[0:0:0:0:0:ffff:4137:270a]:443/ 
  ```

  [::] é o equivalente de IPv6 do IPv4 0.0.0.0.


* Nome do host com o número da porta

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  Nomes de host, \* e + não são especiais. Tudo o que não é um endereço IP reconhecido ou o "localhost" é associado a todos os IPs do IPv4 e IPv6. Se você precisa associar diferentes nomes de host a diferentes aplicativos ASP.NET Core na mesma porta, use o [WebListener](weblistener.md) ou um servidor proxy reverso, como o IIS, Nginx ou Apache.

* Nome do "localhost" com o número da porta ou IP de loopback com o número da porta

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  Quando `localhost` é especificado, o Kestrel tenta associar a interfaces de loopback IPv4 e IPv6. Se a porta solicitada está sendo usada por outro serviço em uma das interfaces de loopback, o Kestrel falha ao ser iniciado. Se uma das interfaces de loopback não estiver disponível por qualquer outro motivo (geralmente porque não há suporte para o IPv6), o Kestrel registra um aviso em log. 

* Soquete do UNIX

  ```
  http://unix:/run/dan-live.sock  
  ```

**Porta 0**

Se você especificar o número de porta 0, o Kestrel associa-o dinamicamente a uma porta disponível. A associação à porta 0 é permitida para qualquer nome do host ou IP, exceto o nome `localhost`.

O seguinte exemplo mostra como determinar à qual porta o Kestrel realmente está associado em tempo de execução:

[!code-csharp[](kestrel/sample1/Startup.cs?name=snippet_Configure)]

**Prefixos de URL para o SSL**

Lembre-se de incluir os prefixos de URL com `https:` se você chamar o método de extensão `UseHttps`, conforme mostrado abaixo.

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

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

---
## <a name="next-steps"></a>Próximas etapas

Para obter mais informações, consulte os seguintes recursos:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

* [Aplicativo de exemplo para o 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)
* [Código-fonte do kestrel](https://github.com/aspnet/KestrelHttpServer)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* [Aplicativo de exemplo para o 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)
* [Código-fonte do kestrel](https://github.com/aspnet/KestrelHttpServer)

---
