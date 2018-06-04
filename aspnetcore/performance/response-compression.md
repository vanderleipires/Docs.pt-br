---
title: Middleware de compactação de resposta para o ASP.NET Core
author: guardrex
description: Saiba mais sobre a compactação de resposta e como usar o Middleware de compactação de resposta em aplicativos do ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/20/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: performance/response-compression
ms.openlocfilehash: 152799500577dd09247bcee8c87cde39ca20aa79
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34729568"
---
# <a name="response-compression-middleware-for-aspnet-core"></a>Middleware de compactação de resposta para o ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

Largura de banda de rede é um recurso limitado. Reduzir o tamanho da resposta geralmente aumenta a capacidade de resposta de um aplicativo com frequência. É uma maneira de reduzir os tamanhos de carga compactar respostas do aplicativo.

## <a name="when-to-use-response-compression-middleware"></a>Quando usar o Middleware de compactação de resposta

Use tecnologias de compactação de resposta com base em servidor no IIS, o Apache ou Nginx. O desempenho do middleware provavelmente não corresponde dos módulos de servidor. [Servidor de HTTP. sys](xref:fundamentals/servers/httpsys) e [Kestrel](xref:fundamentals/servers/kestrel) atualmente não oferecem suporte à compactação interna.

Use o Middleware de compactação de resposta quando estiver:

* Não é possível usar as seguintes tecnologias de compactação baseada em servidor:
  * [Módulo de compactação dinâmica do IIS](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [Módulo do Apache mod_deflate](http://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [Nginx compactação e descompactação](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* Hospedagem diretamente em:
  * [Servidor de HTTP. sys](xref:fundamentals/servers/httpsys) (anteriormente chamado [WebListener](xref:fundamentals/servers/weblistener))
  * [Kestrel](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a>Compactação de resposta

Em geral, qualquer resposta não nativamente compactada pode se beneficiar da compactação de resposta. As respostas que não foi compactadas normalmente incluem: CSS, JavaScript, HTML, XML e JSON. Você não deve compactar ativos nativamente compactados, como arquivos PNG. Se você tentar compactar ainda mais uma resposta compactada nativamente, qualquer redução pequena adicional em tempo de tamanho e a transmissão será provavelmente ser ofuscada pelo tempo necessário para processar a compactação. Não compacte arquivos menores que cerca de 150 e 1000 bytes (dependendo do conteúdo do arquivo e a eficiência da compactação). A sobrecarga de compactação de arquivos pequenos pode produzir um arquivo compactado maior do que o arquivo não compactado.

Quando um cliente pode processar conteúdo compactado, o cliente deve informar o servidor de seus recursos, enviando o `Accept-Encoding` cabeçalho com a solicitação. Quando um servidor envia conteúdo compactado, ele deve incluir informações de `Content-Encoding` cabeçalho em como a resposta compactada é codificada. Conteúdo designações de codificação com suporte pelo middleware são mostradas na tabela a seguir.

| `Accept-Encoding` valores de cabeçalho | Middleware com suporte | Descrição                                                 |
| ------------------------------- | :------------------: | ----------------------------------------------------------- |
| `br`                            | Não                   | Formato de dados compactados Brotli                               |
| `compress`                      | Não                   | Formato de dados de "compactar" UNIX                                 |
| `deflate`                       | Não                   | "deflate" dados compactados no formato de dados "zlib"     |
| `exi`                           | Não                   | W3C XML eficiente intercâmbio                               |
| `gzip`                          | Sim (padrão)        | formato de arquivo gzip                                            |
| `identity`                      | Sim                  | "Nenhuma codificação" identificador: A resposta não deve ser codificada. |
| `pack200-gzip`                  | Não                   | Formato de transferência de rede para arquivos mortos de Java                   |
| `*`                             | Sim                  | Nenhum conteúdo disponível não codificação explicitamente solicitada     |

Para obter mais informações, consulte o [IANA conteúdo codificação lista oficial](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).

O middleware permite adicionar provedores de compactação adicional para personalizado `Accept-Encoding` valores de cabeçalho. Para obter mais informações, consulte [provedores personalizados](#custom-providers) abaixo.

O middleware é capaz de reagir a valor de qualidade (qvalue, `q`) quando enviado pelo cliente para priorizar os esquemas de compactação de relevância. Para obter mais informações, consulte [RFC 7231: codificação aceita](https://tools.ietf.org/html/rfc7231#section-5.3.4).

Algoritmos de compactação estão sujeitos a um equilíbrio entre a velocidade de compactação e a eficiência da compactação de. *Eficácia* neste contexto refere-se ao tamanho da saída após a compactação. O menor tamanho é alcançado por mais *ideal* compactação.

Os cabeçalhos envolvidos na solicitação, enviar, cache e receber conteúdo compactado são descritos na tabela a seguir.

| Cabeçalho             | Função |
| ------------------ | ---- |
| `Accept-Encoding`  | Enviada do cliente para o servidor para indicar o conteúdo aceitável para o cliente de esquemas de codificação. |
| `Content-Encoding` | Enviados do servidor para o cliente para indicar a codificação do conteúdo na carga. |
| `Content-Length`   | Quando ocorre a compactação, o `Content-Length` cabeçalho for removido, pois as alterações de conteúdo do corpo quando a resposta é compactada. |
| `Content-MD5`      | Quando ocorre a compactação, o `Content-MD5` cabeçalho for removido, pois o conteúdo do corpo foi alterado e o hash não é mais válido. |
| `Content-Type`     | Especifica o tipo MIME do conteúdo. Cada resposta deve especificar seu `Content-Type`. O middleware verifica esse valor para determinar se a resposta deve ser compactada. O middleware Especifica um conjunto de [padrão tipos MIME](#mime-types) que ele pode codificar, mas você pode substituir ou adicionar tipos MIME. |
| `Vary`             | Quando enviadas pelo servidor com um valor de `Accept-Encoding` para clientes e proxies, o `Vary` cabeçalho indica ao cliente ou proxy que ele deve armazenar em cache (variar) respostas com base no valor da `Accept-Encoding` cabeçalho da solicitação. O resultado de retorno de conteúdo com o `Vary: Accept-Encoding` cabeçalho é que ambos compactado e descompactadas respostas são armazenadas em cache separadamente. |

Você pode explorar os recursos do Middleware de compactação de resposta com o [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples). O exemplo ilustra:

* A compactação de respostas de aplicativo usando o gzip e provedores personalizados de compactação.
* Como adicionar um tipo de MIME para a lista padrão de tipos de MIME para compactação.

## <a name="package"></a>Pacote

::: moniker range="< aspnetcore-2.0"

Para incluir o middleware em seu projeto, adicione uma referência para o [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) pacote. Esse recurso está disponível para aplicativos direcionados ao ASP.NET Core 1.1 ou posterior.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Para incluir o middleware em seu projeto, adicione uma referência para o [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) pacote ou use o [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).

::: moniker-end

::: moniker range="> aspnetcore-2.0"

Para incluir o middleware em seu projeto, adicione uma referência para o [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) pacote ou use o [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).

::: moniker-end

## <a name="configuration"></a>Configuração

O código a seguir mostra como habilitar o Middleware de compactação de resposta com a compactação gzip padrão e para tipos MIME padrão.

```csharp
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddResponseCompression();
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        app.UseResponseCompression();
    }
}
```

> [!NOTE]
> Usar uma ferramenta como [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), ou [carteiro](https://www.getpostman.com/) para definir o `Accept-Encoding` cabeçalho de solicitação e analise os cabeçalhos de resposta, o tamanho e o corpo.

Enviar uma solicitação para o aplicativo de exemplo sem o `Accept-Encoding` cabeçalho e observe que a resposta é descompactada. O `Content-Encoding` e `Vary` cabeçalhos não estão presentes na resposta.

![Janela do Fiddler mostrando o resultado de uma solicitação sem o cabeçalho Accept-Encoding. A resposta não é compactada.](response-compression/_static/request-uncompressed.png)

Enviar uma solicitação para o aplicativo de exemplo com o `Accept-Encoding: gzip` cabeçalho e observe que a resposta é compactada. O `Content-Encoding` e `Vary` cabeçalhos estão presentes na resposta.

![Janela do Fiddler mostrando o resultado de uma solicitação com o cabeçalho Accept-Encoding e um valor de gzip. Os cabeçalhos podem variar e codificação de conteúdo são adicionados à resposta. A resposta é compactada.](response-compression/_static/request-compressed.png)

## <a name="providers"></a>provedores

### <a name="gzipcompressionprovider"></a>GzipCompressionProvider

Use o [GzipCompressionProvider](/dotnet/api/microsoft.aspnetcore.responsecompression.gzipcompressionprovider) para compactar respostas com gzip. Este é o provedor de compactação padrão se nenhum for especificado. Você pode definir a compactação de nível com o [GzipCompressionProviderOptions](/dotnet/api/microsoft.aspnetcore.responsecompression.gzipcompressionprovideroptions).

O provedor de compactação gzip como padrão o nível de compactação mais rápido ([CompressionLevel.Fastest](/dotnet/api/system.io.compression.compressionlevel)), que não pode produzir a compactação mais eficiente. Se a compactação mais eficiente é desejada, você pode configurar o middleware para compactação ideal.

| Nível de compactação | Descrição |
| ----------------- | ----------- |
| [CompressionLevel.Fastest](/dotnet/api/system.io.compression.compressionlevel) | A compactação deve ser concluída assim que possível, mesmo se a saída resultante ideal não é compactada. |
| [CompressionLevel.NoCompression](/dotnet/api/system.io.compression.compressionlevel) | Não há nenhuma compactação deve ser executada. |
| [CompressionLevel.Optimal](/dotnet/api/system.io.compression.compressionlevel) | As respostas devem ser compactadas ideal, mesmo se a compactação leva mais tempo para concluir. |

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=5,12-15)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=5,12-15)]

---

## <a name="mime-types"></a>tipos MIME

O middleware Especifica um conjunto padrão de tipos de MIME para compactação:

* `text/plain`
* `text/css`
* `application/javascript`
* `text/html`
* `application/xml`
* `text/xml`
* `application/json`
* `text/json`

Você pode substituir ou acrescentar tipos MIME com as opções de Middleware de compactação de resposta. Observe que MIME curinga tipos, como `text/*` não são suportados. O aplicativo de exemplo adiciona um tipo MIME para `image/svg+xml` e compacta e serve o ASP.NET Core imagem da faixa (*banner.svg*).

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=7-9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=7-9)]

---

### <a name="custom-providers"></a>Provedores personalizados

Você pode criar implementações personalizadas de compactação com [ICompressionProvider](/dotnet/api/microsoft.aspnetcore.responsecompression.icompressionprovider). O [EncodingName](/dotnet/api/microsoft.aspnetcore.responsecompression.icompressionprovider.encodingname) representa o conteúdo de codificação que este `ICompressionProvider` produz. O middleware usa essas informações para escolher o fornecedor de acordo com a lista especificada no `Accept-Encoding` cabeçalho da solicitação.

Usando o aplicativo de exemplo, o cliente envia uma solicitação com o `Accept-Encoding: mycustomcompression` cabeçalho. O middleware usa a implementação da compactação personalizada e retorna a resposta com uma `Content-Encoding: mycustomcompression` cabeçalho. O cliente deve ser capaz de descompactar a codificação personalizada para que uma implementação personalizada de compactação trabalhar.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=5,12-15)]

[!code-csharp[](response-compression/samples/2.x/CustomCompressionProvider.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=5,12-15)]

[!code-csharp[](response-compression/samples/1.x/CustomCompressionProvider.cs?name=snippet1)]

---

Enviar uma solicitação para o aplicativo de exemplo com o `Accept-Encoding: mycustomcompression` cabeçalho e observar os cabeçalhos de resposta. O `Vary` e `Content-Encoding` cabeçalhos estão presentes na resposta. O corpo da resposta (não mostrado) não é compactado pelo exemplo. Não há uma implementação da compactação no `CustomCompressionProvider` classe do exemplo. No entanto, o exemplo mostra onde você implementaria tal um algoritmo de compactação.

![Janela do Fiddler mostrando o resultado de uma solicitação com o cabeçalho Accept-Encoding e um valor de mycustomcompression. Os cabeçalhos podem variar e codificação de conteúdo são adicionados à resposta.](response-compression/_static/request-custom-compression.png)

## <a name="compression-with-secure-protocol"></a>Compactação com protocolo seguro

As respostas compactadas através de conexões seguras podem ser controladas com o `EnableForHttps` opção, que é desabilitada por padrão. Usar a compactação com páginas geradas dinamicamente pode levar a problemas de segurança, como o [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) e [violação](https://wikipedia.org/wiki/BREACH_(security_exploit)) ataques.

## <a name="adding-the-vary-header"></a>Adicionar o cabeçalho Vary

::: moniker range=">= aspnetcore-2.0"

Ao compactar respostas com base no `Accept-Encoding` cabeçalho, há potencialmente várias versões compactadas da resposta e uma versão descompactada. Para instruir os caches de cliente e proxy que várias versões existem e devem ser armazenadas, o `Vary` cabeçalho é adicionado com uma `Accept-Encoding` valor. No ASP.NET Core 2.0 ou posterior, o middleware adiciona o `Vary` cabeçalho automaticamente quando a resposta é compactada.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Ao compactar respostas com base no `Accept-Encoding` cabeçalho, há potencialmente várias versões compactadas da resposta e uma versão descompactada. Para instruir os caches de cliente e proxy que várias versões existem e devem ser armazenadas, o `Vary` cabeçalho é adicionado com uma `Accept-Encoding` valor. No núcleo do ASP.NET 1. x, adicionando o `Vary` cabeçalho para a resposta é realizado manualmente:

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet1)]

::: moniker-end

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a>Problema de middleware quando atrás de um proxy reverso Nginx

Quando uma solicitação é delegada por Nginx, o `Accept-Encoding` cabeçalho é removido. Isso impede que o middleware da compactação de resposta. Para obter mais informações, consulte [NGINX: compactação e descompactação de](https://www.nginx.com/resources/admin-guide/compression-and-decompression/). Esse problema é acompanhado por [descobrir a compactação de passagem para Nginx (BasicMiddleware 123)](https://github.com/aspnet/BasicMiddleware/issues/123).

## <a name="working-with-iis-dynamic-compression"></a>Trabalhando com compactação dinâmica do IIS

Se você tiver um active IIS compactação módulo dinâmico configurado no nível do servidor que você deseja desabilitar para um aplicativo, você pode fazer isso com uma adição à sua *Web. config* arquivo. Para obter mais informações, consulte [Desabilitando módulos do IIS](xref:host-and-deploy/iis/modules#disabling-iis-modules).

## <a name="troubleshooting"></a>Solução de problemas

Usar uma ferramenta como [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), ou [carteiro](https://www.getpostman.com/), que permitem que você defina o `Accept-Encoding` cabeçalho de solicitação e analise os cabeçalhos de resposta, o tamanho e o corpo. O Middleware de compactação de resposta compacta respostas que atendem às seguintes condições:

* O `Accept-Encoding` cabeçalho estiver presente com um valor de `gzip`, `*`, ou codificação personalizada que corresponda a um provedor personalizado de compactação estabelecida por você. O valor não deve ser `identity` ou tem um valor de qualidade (qvalue, `q`) configuração de 0 (zero).
* O tipo MIME (`Content-Type`) deve ser definido e deve corresponder a um tipo MIME configurado no [ResponseCompressionOptions](/dotnet/api/microsoft.aspnetcore.responsecompression.responsecompressionoptions).
* A solicitação não deve incluir o `Content-Range` cabeçalho.
* A solicitação deve usar o protocolo seguro (http), a menos que o protocolo seguro (https) é configurado nas opções de Middleware de compactação de resposta. *Observe o perigo [descrito acima](#compression-with-secure-protocol) ao habilitar a compactação de conteúdo segura.*

## <a name="additional-resources"></a>Recursos adicionais

* [Inicialização de aplicativos](xref:fundamentals/startup)
* [Middleware](xref:fundamentals/middleware/index)
* [Rede Mozilla Developer: Codificação aceita](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [RFC 7231 seção 3.1.2.1: Codings conteúdos](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [RFC 7230 seção 4.2.3: Codificação de Gzip](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [Versão de especificação de formato de arquivo GZIP 4.3](http://www.ietf.org/rfc/rfc1952.txt)
