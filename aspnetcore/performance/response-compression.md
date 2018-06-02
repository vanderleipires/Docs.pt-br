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
ms.sourcegitcommit: a0b6319c36f41cdce76ea334372f6e14fc66507e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34729568"
---
# <a name="response-compression-middleware-for-aspnet-core"></a><span data-ttu-id="21b21-103">Middleware de compactação de resposta para o ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="21b21-103">Response Compression Middleware for ASP.NET Core</span></span>

<span data-ttu-id="21b21-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="21b21-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="21b21-105">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="21b21-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="21b21-106">Largura de banda de rede é um recurso limitado.</span><span class="sxs-lookup"><span data-stu-id="21b21-106">Network bandwidth is a limited resource.</span></span> <span data-ttu-id="21b21-107">Reduzir o tamanho da resposta geralmente aumenta a capacidade de resposta de um aplicativo com frequência.</span><span class="sxs-lookup"><span data-stu-id="21b21-107">Reducing the size of the response usually increases the responsiveness of an app, often dramatically.</span></span> <span data-ttu-id="21b21-108">É uma maneira de reduzir os tamanhos de carga compactar respostas do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="21b21-108">One way to reduce payload sizes is to compress an app's responses.</span></span>

## <a name="when-to-use-response-compression-middleware"></a><span data-ttu-id="21b21-109">Quando usar o Middleware de compactação de resposta</span><span class="sxs-lookup"><span data-stu-id="21b21-109">When to use Response Compression Middleware</span></span>

<span data-ttu-id="21b21-110">Use tecnologias de compactação de resposta com base em servidor no IIS, o Apache ou Nginx.</span><span class="sxs-lookup"><span data-stu-id="21b21-110">Use server-based response compression technologies in IIS, Apache, or Nginx.</span></span> <span data-ttu-id="21b21-111">O desempenho do middleware provavelmente não corresponde dos módulos de servidor.</span><span class="sxs-lookup"><span data-stu-id="21b21-111">The performance of the middleware probably won't match that of the server modules.</span></span> <span data-ttu-id="21b21-112">[Servidor de HTTP. sys](xref:fundamentals/servers/httpsys) e [Kestrel](xref:fundamentals/servers/kestrel) atualmente não oferecem suporte à compactação interna.</span><span class="sxs-lookup"><span data-stu-id="21b21-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) and [Kestrel](xref:fundamentals/servers/kestrel) don't currently offer built-in compression support.</span></span>

<span data-ttu-id="21b21-113">Use o Middleware de compactação de resposta quando estiver:</span><span class="sxs-lookup"><span data-stu-id="21b21-113">Use Response Compression Middleware when you're:</span></span>

* <span data-ttu-id="21b21-114">Não é possível usar as seguintes tecnologias de compactação baseada em servidor:</span><span class="sxs-lookup"><span data-stu-id="21b21-114">Unable to use the following server-based compression technologies:</span></span>
  * [<span data-ttu-id="21b21-115">Módulo de compactação dinâmica do IIS</span><span class="sxs-lookup"><span data-stu-id="21b21-115">IIS Dynamic Compression module</span></span>](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [<span data-ttu-id="21b21-116">Módulo do Apache mod_deflate</span><span class="sxs-lookup"><span data-stu-id="21b21-116">Apache mod_deflate module</span></span>](http://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [<span data-ttu-id="21b21-117">Nginx compactação e descompactação</span><span class="sxs-lookup"><span data-stu-id="21b21-117">Nginx Compression and Decompression</span></span>](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* <span data-ttu-id="21b21-118">Hospedagem diretamente em:</span><span class="sxs-lookup"><span data-stu-id="21b21-118">Hosting directly on:</span></span>
  * <span data-ttu-id="21b21-119">[Servidor de HTTP. sys](xref:fundamentals/servers/httpsys) (anteriormente chamado [WebListener](xref:fundamentals/servers/weblistener))</span><span class="sxs-lookup"><span data-stu-id="21b21-119">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener))</span></span>
  * [<span data-ttu-id="21b21-120">Kestrel</span><span class="sxs-lookup"><span data-stu-id="21b21-120">Kestrel</span></span>](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a><span data-ttu-id="21b21-121">Compactação de resposta</span><span class="sxs-lookup"><span data-stu-id="21b21-121">Response compression</span></span>

<span data-ttu-id="21b21-122">Em geral, qualquer resposta não nativamente compactada pode se beneficiar da compactação de resposta.</span><span class="sxs-lookup"><span data-stu-id="21b21-122">Usually, any response not natively compressed can benefit from response compression.</span></span> <span data-ttu-id="21b21-123">As respostas que não foi compactadas normalmente incluem: CSS, JavaScript, HTML, XML e JSON.</span><span class="sxs-lookup"><span data-stu-id="21b21-123">Responses not natively compressed typically include: CSS, JavaScript, HTML, XML, and JSON.</span></span> <span data-ttu-id="21b21-124">Você não deve compactar ativos nativamente compactados, como arquivos PNG.</span><span class="sxs-lookup"><span data-stu-id="21b21-124">You shouldn't compress natively compressed assets, such as PNG files.</span></span> <span data-ttu-id="21b21-125">Se você tentar compactar ainda mais uma resposta compactada nativamente, qualquer redução pequena adicional em tempo de tamanho e a transmissão será provavelmente ser ofuscada pelo tempo necessário para processar a compactação.</span><span class="sxs-lookup"><span data-stu-id="21b21-125">If you attempt to further compress a natively compressed response, any small additional reduction in size and transmission time will likely be overshadowed by the time it took to process the compression.</span></span> <span data-ttu-id="21b21-126">Não compacte arquivos menores que cerca de 150 e 1000 bytes (dependendo do conteúdo do arquivo e a eficiência da compactação).</span><span class="sxs-lookup"><span data-stu-id="21b21-126">Don't compress files smaller than about 150-1000 bytes (depending on the file's content and the efficiency of compression).</span></span> <span data-ttu-id="21b21-127">A sobrecarga de compactação de arquivos pequenos pode produzir um arquivo compactado maior do que o arquivo não compactado.</span><span class="sxs-lookup"><span data-stu-id="21b21-127">The overhead of compressing small files may produce a compressed file larger than the uncompressed file.</span></span>

<span data-ttu-id="21b21-128">Quando um cliente pode processar conteúdo compactado, o cliente deve informar o servidor de seus recursos, enviando o `Accept-Encoding` cabeçalho com a solicitação.</span><span class="sxs-lookup"><span data-stu-id="21b21-128">When a client can process compressed content, the client must inform the server of its capabilities by sending the `Accept-Encoding` header with the request.</span></span> <span data-ttu-id="21b21-129">Quando um servidor envia conteúdo compactado, ele deve incluir informações de `Content-Encoding` cabeçalho em como a resposta compactada é codificada.</span><span class="sxs-lookup"><span data-stu-id="21b21-129">When a server sends compressed content, it must include information in the `Content-Encoding` header on how the compressed response is encoded.</span></span> <span data-ttu-id="21b21-130">Conteúdo designações de codificação com suporte pelo middleware são mostradas na tabela a seguir.</span><span class="sxs-lookup"><span data-stu-id="21b21-130">Content encoding designations supported by the middleware are shown in the following table.</span></span>

| <span data-ttu-id="21b21-131">`Accept-Encoding` valores de cabeçalho</span><span class="sxs-lookup"><span data-stu-id="21b21-131">`Accept-Encoding` header values</span></span> | <span data-ttu-id="21b21-132">Middleware com suporte</span><span class="sxs-lookup"><span data-stu-id="21b21-132">Middleware Supported</span></span> | <span data-ttu-id="21b21-133">Descrição</span><span class="sxs-lookup"><span data-stu-id="21b21-133">Description</span></span>                                                 |
| ------------------------------- | :------------------: | ----------------------------------------------------------- |
| `br`                            | <span data-ttu-id="21b21-134">Não</span><span class="sxs-lookup"><span data-stu-id="21b21-134">No</span></span>                   | <span data-ttu-id="21b21-135">Formato de dados compactados Brotli</span><span class="sxs-lookup"><span data-stu-id="21b21-135">Brotli Compressed Data Format</span></span>                               |
| `compress`                      | <span data-ttu-id="21b21-136">Não</span><span class="sxs-lookup"><span data-stu-id="21b21-136">No</span></span>                   | <span data-ttu-id="21b21-137">Formato de dados de "compactar" UNIX</span><span class="sxs-lookup"><span data-stu-id="21b21-137">UNIX "compress" data format</span></span>                                 |
| `deflate`                       | <span data-ttu-id="21b21-138">Não</span><span class="sxs-lookup"><span data-stu-id="21b21-138">No</span></span>                   | <span data-ttu-id="21b21-139">"deflate" dados compactados no formato de dados "zlib"</span><span class="sxs-lookup"><span data-stu-id="21b21-139">"deflate" compressed data inside the "zlib" data format</span></span>     |
| `exi`                           | <span data-ttu-id="21b21-140">Não</span><span class="sxs-lookup"><span data-stu-id="21b21-140">No</span></span>                   | <span data-ttu-id="21b21-141">W3C XML eficiente intercâmbio</span><span class="sxs-lookup"><span data-stu-id="21b21-141">W3C Efficient XML Interchange</span></span>                               |
| `gzip`                          | <span data-ttu-id="21b21-142">Sim (padrão)</span><span class="sxs-lookup"><span data-stu-id="21b21-142">Yes (default)</span></span>        | <span data-ttu-id="21b21-143">formato de arquivo gzip</span><span class="sxs-lookup"><span data-stu-id="21b21-143">gzip file format</span></span>                                            |
| `identity`                      | <span data-ttu-id="21b21-144">Sim</span><span class="sxs-lookup"><span data-stu-id="21b21-144">Yes</span></span>                  | <span data-ttu-id="21b21-145">"Nenhuma codificação" identificador: A resposta não deve ser codificada.</span><span class="sxs-lookup"><span data-stu-id="21b21-145">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="21b21-146">Não</span><span class="sxs-lookup"><span data-stu-id="21b21-146">No</span></span>                   | <span data-ttu-id="21b21-147">Formato de transferência de rede para arquivos mortos de Java</span><span class="sxs-lookup"><span data-stu-id="21b21-147">Network Transfer Format for Java Archives</span></span>                   |
| `*`                             | <span data-ttu-id="21b21-148">Sim</span><span class="sxs-lookup"><span data-stu-id="21b21-148">Yes</span></span>                  | <span data-ttu-id="21b21-149">Nenhum conteúdo disponível não codificação explicitamente solicitada</span><span class="sxs-lookup"><span data-stu-id="21b21-149">Any available content encoding not explicitly requested</span></span>     |

<span data-ttu-id="21b21-150">Para obter mais informações, consulte o [IANA conteúdo codificação lista oficial](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span><span class="sxs-lookup"><span data-stu-id="21b21-150">For more information, see the [IANA Official Content Coding List](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span></span>

<span data-ttu-id="21b21-151">O middleware permite adicionar provedores de compactação adicional para personalizado `Accept-Encoding` valores de cabeçalho.</span><span class="sxs-lookup"><span data-stu-id="21b21-151">The middleware allows you to add additional compression providers for custom `Accept-Encoding` header values.</span></span> <span data-ttu-id="21b21-152">Para obter mais informações, consulte [provedores personalizados](#custom-providers) abaixo.</span><span class="sxs-lookup"><span data-stu-id="21b21-152">For more information, see [Custom Providers](#custom-providers) below.</span></span>

<span data-ttu-id="21b21-153">O middleware é capaz de reagir a valor de qualidade (qvalue, `q`) quando enviado pelo cliente para priorizar os esquemas de compactação de relevância.</span><span class="sxs-lookup"><span data-stu-id="21b21-153">The middleware is capable of reacting to quality value (qvalue, `q`) weighting when sent by the client to prioritize compression schemes.</span></span> <span data-ttu-id="21b21-154">Para obter mais informações, consulte [RFC 7231: codificação aceita](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span><span class="sxs-lookup"><span data-stu-id="21b21-154">For more information, see [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span></span>

<span data-ttu-id="21b21-155">Algoritmos de compactação estão sujeitos a um equilíbrio entre a velocidade de compactação e a eficiência da compactação de.</span><span class="sxs-lookup"><span data-stu-id="21b21-155">Compression algorithms are subject to a tradeoff between compression speed and the effectiveness of the compression.</span></span> <span data-ttu-id="21b21-156">*Eficácia* neste contexto refere-se ao tamanho da saída após a compactação.</span><span class="sxs-lookup"><span data-stu-id="21b21-156">*Effectiveness* in this context refers to the size of the output after compression.</span></span> <span data-ttu-id="21b21-157">O menor tamanho é alcançado por mais *ideal* compactação.</span><span class="sxs-lookup"><span data-stu-id="21b21-157">The smallest size is achieved by the most *optimal* compression.</span></span>

<span data-ttu-id="21b21-158">Os cabeçalhos envolvidos na solicitação, enviar, cache e receber conteúdo compactado são descritos na tabela a seguir.</span><span class="sxs-lookup"><span data-stu-id="21b21-158">The headers involved in requesting, sending, caching, and receiving compressed content are described in the table below.</span></span>

| <span data-ttu-id="21b21-159">Cabeçalho</span><span class="sxs-lookup"><span data-stu-id="21b21-159">Header</span></span>             | <span data-ttu-id="21b21-160">Função</span><span class="sxs-lookup"><span data-stu-id="21b21-160">Role</span></span> |
| ------------------ | ---- |
| `Accept-Encoding`  | <span data-ttu-id="21b21-161">Enviada do cliente para o servidor para indicar o conteúdo aceitável para o cliente de esquemas de codificação.</span><span class="sxs-lookup"><span data-stu-id="21b21-161">Sent from the client to the server to indicate the content encoding schemes acceptable to the client.</span></span> |
| `Content-Encoding` | <span data-ttu-id="21b21-162">Enviados do servidor para o cliente para indicar a codificação do conteúdo na carga.</span><span class="sxs-lookup"><span data-stu-id="21b21-162">Sent from the server to the client to indicate the encoding of the content in the payload.</span></span> |
| `Content-Length`   | <span data-ttu-id="21b21-163">Quando ocorre a compactação, o `Content-Length` cabeçalho for removido, pois as alterações de conteúdo do corpo quando a resposta é compactada.</span><span class="sxs-lookup"><span data-stu-id="21b21-163">When compression occurs, the `Content-Length` header is removed, since the body content changes when the response is compressed.</span></span> |
| `Content-MD5`      | <span data-ttu-id="21b21-164">Quando ocorre a compactação, o `Content-MD5` cabeçalho for removido, pois o conteúdo do corpo foi alterado e o hash não é mais válido.</span><span class="sxs-lookup"><span data-stu-id="21b21-164">When compression occurs, the `Content-MD5` header is removed, since the body content has changed and the hash is no longer valid.</span></span> |
| `Content-Type`     | <span data-ttu-id="21b21-165">Especifica o tipo MIME do conteúdo.</span><span class="sxs-lookup"><span data-stu-id="21b21-165">Specifies the MIME type of the content.</span></span> <span data-ttu-id="21b21-166">Cada resposta deve especificar seu `Content-Type`.</span><span class="sxs-lookup"><span data-stu-id="21b21-166">Every response should specify its `Content-Type`.</span></span> <span data-ttu-id="21b21-167">O middleware verifica esse valor para determinar se a resposta deve ser compactada.</span><span class="sxs-lookup"><span data-stu-id="21b21-167">The middleware checks this value to determine if the response should be compressed.</span></span> <span data-ttu-id="21b21-168">O middleware Especifica um conjunto de [padrão tipos MIME](#mime-types) que ele pode codificar, mas você pode substituir ou adicionar tipos MIME.</span><span class="sxs-lookup"><span data-stu-id="21b21-168">The middleware specifies a set of [default MIME types](#mime-types) that it can encode, but you can replace or add MIME types.</span></span> |
| `Vary`             | <span data-ttu-id="21b21-169">Quando enviadas pelo servidor com um valor de `Accept-Encoding` para clientes e proxies, o `Vary` cabeçalho indica ao cliente ou proxy que ele deve armazenar em cache (variar) respostas com base no valor da `Accept-Encoding` cabeçalho da solicitação.</span><span class="sxs-lookup"><span data-stu-id="21b21-169">When sent by the server with a value of `Accept-Encoding` to clients and proxies, the `Vary` header indicates to the client or proxy that it should cache (vary) responses based on the value of the `Accept-Encoding` header of the request.</span></span> <span data-ttu-id="21b21-170">O resultado de retorno de conteúdo com o `Vary: Accept-Encoding` cabeçalho é que ambos compactado e descompactadas respostas são armazenadas em cache separadamente.</span><span class="sxs-lookup"><span data-stu-id="21b21-170">The result of returning content with the `Vary: Accept-Encoding` header is that both compressed and uncompressed responses are cached separately.</span></span> |

<span data-ttu-id="21b21-171">Você pode explorar os recursos do Middleware de compactação de resposta com o [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples).</span><span class="sxs-lookup"><span data-stu-id="21b21-171">You can explore the features of the Response Compression Middleware with the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples).</span></span> <span data-ttu-id="21b21-172">O exemplo ilustra:</span><span class="sxs-lookup"><span data-stu-id="21b21-172">The sample illustrates:</span></span>

* <span data-ttu-id="21b21-173">A compactação de respostas de aplicativo usando o gzip e provedores personalizados de compactação.</span><span class="sxs-lookup"><span data-stu-id="21b21-173">The compression of app responses using gzip and custom compression providers.</span></span>
* <span data-ttu-id="21b21-174">Como adicionar um tipo de MIME para a lista padrão de tipos de MIME para compactação.</span><span class="sxs-lookup"><span data-stu-id="21b21-174">How to add a MIME type to the default list of MIME types for compression.</span></span>

## <a name="package"></a><span data-ttu-id="21b21-175">Pacote</span><span class="sxs-lookup"><span data-stu-id="21b21-175">Package</span></span>

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="21b21-176">Para incluir o middleware em seu projeto, adicione uma referência para o [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) pacote.</span><span class="sxs-lookup"><span data-stu-id="21b21-176">To include the middleware in your project, add a reference to the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span> <span data-ttu-id="21b21-177">Esse recurso está disponível para aplicativos direcionados ao ASP.NET Core 1.1 ou posterior.</span><span class="sxs-lookup"><span data-stu-id="21b21-177">This feature is available for apps that target ASP.NET Core 1.1 or later.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="21b21-178">Para incluir o middleware em seu projeto, adicione uma referência para o [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) pacote ou use o [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="21b21-178">To include the middleware in your project, add a reference to the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package or use the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="21b21-179">Para incluir o middleware em seu projeto, adicione uma referência para o [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) pacote ou use o [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="21b21-179">To include the middleware in your project, add a reference to the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package or use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

## <a name="configuration"></a><span data-ttu-id="21b21-180">Configuração</span><span class="sxs-lookup"><span data-stu-id="21b21-180">Configuration</span></span>

<span data-ttu-id="21b21-181">O código a seguir mostra como habilitar o Middleware de compactação de resposta com a compactação gzip padrão e para tipos MIME padrão.</span><span class="sxs-lookup"><span data-stu-id="21b21-181">The following code shows how to enable the Response Compression Middleware with the default gzip compression and for default MIME types.</span></span>

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
> <span data-ttu-id="21b21-182">Usar uma ferramenta como [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), ou [carteiro](https://www.getpostman.com/) para definir o `Accept-Encoding` cabeçalho de solicitação e analise os cabeçalhos de resposta, o tamanho e o corpo.</span><span class="sxs-lookup"><span data-stu-id="21b21-182">Use a tool like [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), or [Postman](https://www.getpostman.com/) to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span>

<span data-ttu-id="21b21-183">Enviar uma solicitação para o aplicativo de exemplo sem o `Accept-Encoding` cabeçalho e observe que a resposta é descompactada.</span><span class="sxs-lookup"><span data-stu-id="21b21-183">Submit a request to the sample app without the `Accept-Encoding` header and observe that the response is uncompressed.</span></span> <span data-ttu-id="21b21-184">O `Content-Encoding` e `Vary` cabeçalhos não estão presentes na resposta.</span><span class="sxs-lookup"><span data-stu-id="21b21-184">The `Content-Encoding` and `Vary` headers aren't present on the response.</span></span>

![Janela do Fiddler mostrando o resultado de uma solicitação sem o cabeçalho Accept-Encoding.](response-compression/_static/request-uncompressed.png)

<span data-ttu-id="21b21-187">Enviar uma solicitação para o aplicativo de exemplo com o `Accept-Encoding: gzip` cabeçalho e observe que a resposta é compactada.</span><span class="sxs-lookup"><span data-stu-id="21b21-187">Submit a request to the sample app with the `Accept-Encoding: gzip` header and observe that the response is compressed.</span></span> <span data-ttu-id="21b21-188">O `Content-Encoding` e `Vary` cabeçalhos estão presentes na resposta.</span><span class="sxs-lookup"><span data-stu-id="21b21-188">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Janela do Fiddler mostrando o resultado de uma solicitação com o cabeçalho Accept-Encoding e um valor de gzip.](response-compression/_static/request-compressed.png)

## <a name="providers"></a><span data-ttu-id="21b21-192">provedores</span><span class="sxs-lookup"><span data-stu-id="21b21-192">Providers</span></span>

### <a name="gzipcompressionprovider"></a><span data-ttu-id="21b21-193">GzipCompressionProvider</span><span class="sxs-lookup"><span data-stu-id="21b21-193">GzipCompressionProvider</span></span>

<span data-ttu-id="21b21-194">Use o [GzipCompressionProvider](/dotnet/api/microsoft.aspnetcore.responsecompression.gzipcompressionprovider) para compactar respostas com gzip.</span><span class="sxs-lookup"><span data-stu-id="21b21-194">Use the [GzipCompressionProvider](/dotnet/api/microsoft.aspnetcore.responsecompression.gzipcompressionprovider) to compress responses with gzip.</span></span> <span data-ttu-id="21b21-195">Este é o provedor de compactação padrão se nenhum for especificado.</span><span class="sxs-lookup"><span data-stu-id="21b21-195">This is the default compression provider if none are specified.</span></span> <span data-ttu-id="21b21-196">Você pode definir a compactação de nível com o [GzipCompressionProviderOptions](/dotnet/api/microsoft.aspnetcore.responsecompression.gzipcompressionprovideroptions).</span><span class="sxs-lookup"><span data-stu-id="21b21-196">You can set the compression level with the [GzipCompressionProviderOptions](/dotnet/api/microsoft.aspnetcore.responsecompression.gzipcompressionprovideroptions).</span></span>

<span data-ttu-id="21b21-197">O provedor de compactação gzip como padrão o nível de compactação mais rápido ([CompressionLevel.Fastest](/dotnet/api/system.io.compression.compressionlevel)), que não pode produzir a compactação mais eficiente.</span><span class="sxs-lookup"><span data-stu-id="21b21-197">The gzip compression provider defaults to the fastest compression level ([CompressionLevel.Fastest](/dotnet/api/system.io.compression.compressionlevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="21b21-198">Se a compactação mais eficiente é desejada, você pode configurar o middleware para compactação ideal.</span><span class="sxs-lookup"><span data-stu-id="21b21-198">If the most efficient compression is desired, you can configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="21b21-199">Nível de compactação</span><span class="sxs-lookup"><span data-stu-id="21b21-199">Compression Level</span></span> | <span data-ttu-id="21b21-200">Descrição</span><span class="sxs-lookup"><span data-stu-id="21b21-200">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="21b21-201">CompressionLevel.Fastest</span><span class="sxs-lookup"><span data-stu-id="21b21-201">CompressionLevel.Fastest</span></span>](/dotnet/api/system.io.compression.compressionlevel) | <span data-ttu-id="21b21-202">A compactação deve ser concluída assim que possível, mesmo se a saída resultante ideal não é compactada.</span><span class="sxs-lookup"><span data-stu-id="21b21-202">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="21b21-203">CompressionLevel.NoCompression</span><span class="sxs-lookup"><span data-stu-id="21b21-203">CompressionLevel.NoCompression</span></span>](/dotnet/api/system.io.compression.compressionlevel) | <span data-ttu-id="21b21-204">Não há nenhuma compactação deve ser executada.</span><span class="sxs-lookup"><span data-stu-id="21b21-204">No compression should be performed.</span></span> |
| [<span data-ttu-id="21b21-205">CompressionLevel.Optimal</span><span class="sxs-lookup"><span data-stu-id="21b21-205">CompressionLevel.Optimal</span></span>](/dotnet/api/system.io.compression.compressionlevel) | <span data-ttu-id="21b21-206">As respostas devem ser compactadas ideal, mesmo se a compactação leva mais tempo para concluir.</span><span class="sxs-lookup"><span data-stu-id="21b21-206">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="21b21-207">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="21b21-207">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=5,12-15)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="21b21-208">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="21b21-208">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=5,12-15)]

---

## <a name="mime-types"></a><span data-ttu-id="21b21-209">tipos MIME</span><span class="sxs-lookup"><span data-stu-id="21b21-209">MIME types</span></span>

<span data-ttu-id="21b21-210">O middleware Especifica um conjunto padrão de tipos de MIME para compactação:</span><span class="sxs-lookup"><span data-stu-id="21b21-210">The middleware specifies a default set of MIME types for compression:</span></span>

* `text/plain`
* `text/css`
* `application/javascript`
* `text/html`
* `application/xml`
* `text/xml`
* `application/json`
* `text/json`

<span data-ttu-id="21b21-211">Você pode substituir ou acrescentar tipos MIME com as opções de Middleware de compactação de resposta.</span><span class="sxs-lookup"><span data-stu-id="21b21-211">You can replace or append MIME types with the Response Compression Middleware options.</span></span> <span data-ttu-id="21b21-212">Observe que MIME curinga tipos, como `text/*` não são suportados.</span><span class="sxs-lookup"><span data-stu-id="21b21-212">Note that wildcard MIME types, such as `text/*` aren't supported.</span></span> <span data-ttu-id="21b21-213">O aplicativo de exemplo adiciona um tipo MIME para `image/svg+xml` e compacta e serve o ASP.NET Core imagem da faixa (*banner.svg*).</span><span class="sxs-lookup"><span data-stu-id="21b21-213">The sample app adds a MIME type for `image/svg+xml` and compresses and serves the ASP.NET Core banner image (*banner.svg*).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="21b21-214">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="21b21-214">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=7-9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="21b21-215">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="21b21-215">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=7-9)]

---

### <a name="custom-providers"></a><span data-ttu-id="21b21-216">Provedores personalizados</span><span class="sxs-lookup"><span data-stu-id="21b21-216">Custom providers</span></span>

<span data-ttu-id="21b21-217">Você pode criar implementações personalizadas de compactação com [ICompressionProvider](/dotnet/api/microsoft.aspnetcore.responsecompression.icompressionprovider).</span><span class="sxs-lookup"><span data-stu-id="21b21-217">You can create custom compression implementations with [ICompressionProvider](/dotnet/api/microsoft.aspnetcore.responsecompression.icompressionprovider).</span></span> <span data-ttu-id="21b21-218">O [EncodingName](/dotnet/api/microsoft.aspnetcore.responsecompression.icompressionprovider.encodingname) representa o conteúdo de codificação que este `ICompressionProvider` produz.</span><span class="sxs-lookup"><span data-stu-id="21b21-218">The [EncodingName](/dotnet/api/microsoft.aspnetcore.responsecompression.icompressionprovider.encodingname) represents the content encoding that this `ICompressionProvider` produces.</span></span> <span data-ttu-id="21b21-219">O middleware usa essas informações para escolher o fornecedor de acordo com a lista especificada no `Accept-Encoding` cabeçalho da solicitação.</span><span class="sxs-lookup"><span data-stu-id="21b21-219">The middleware uses this information to choose the provider based on the list specified in the `Accept-Encoding` header of the request.</span></span>

<span data-ttu-id="21b21-220">Usando o aplicativo de exemplo, o cliente envia uma solicitação com o `Accept-Encoding: mycustomcompression` cabeçalho.</span><span class="sxs-lookup"><span data-stu-id="21b21-220">Using the sample app, the client submits a request with the `Accept-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="21b21-221">O middleware usa a implementação da compactação personalizada e retorna a resposta com uma `Content-Encoding: mycustomcompression` cabeçalho.</span><span class="sxs-lookup"><span data-stu-id="21b21-221">The middleware uses the custom compression implementation and returns the response with a `Content-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="21b21-222">O cliente deve ser capaz de descompactar a codificação personalizada para que uma implementação personalizada de compactação trabalhar.</span><span class="sxs-lookup"><span data-stu-id="21b21-222">The client must be able to decompress the custom encoding in order for a custom compression implementation to work.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="21b21-223">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="21b21-223">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=5,12-15)]

[!code-csharp[](response-compression/samples/2.x/CustomCompressionProvider.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="21b21-224">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="21b21-224">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=5,12-15)]

[!code-csharp[](response-compression/samples/1.x/CustomCompressionProvider.cs?name=snippet1)]

---

<span data-ttu-id="21b21-225">Enviar uma solicitação para o aplicativo de exemplo com o `Accept-Encoding: mycustomcompression` cabeçalho e observar os cabeçalhos de resposta.</span><span class="sxs-lookup"><span data-stu-id="21b21-225">Submit a request to the sample app with the `Accept-Encoding: mycustomcompression` header and observe the response headers.</span></span> <span data-ttu-id="21b21-226">O `Vary` e `Content-Encoding` cabeçalhos estão presentes na resposta.</span><span class="sxs-lookup"><span data-stu-id="21b21-226">The `Vary` and `Content-Encoding` headers are present on the response.</span></span> <span data-ttu-id="21b21-227">O corpo da resposta (não mostrado) não é compactado pelo exemplo.</span><span class="sxs-lookup"><span data-stu-id="21b21-227">The response body (not shown) isn't compressed by the sample.</span></span> <span data-ttu-id="21b21-228">Não há uma implementação da compactação no `CustomCompressionProvider` classe do exemplo.</span><span class="sxs-lookup"><span data-stu-id="21b21-228">There isn't a compression implementation in the `CustomCompressionProvider` class of the sample.</span></span> <span data-ttu-id="21b21-229">No entanto, o exemplo mostra onde você implementaria tal um algoritmo de compactação.</span><span class="sxs-lookup"><span data-stu-id="21b21-229">However, the sample shows where you would implement such a compression algorithm.</span></span>

![Janela do Fiddler mostrando o resultado de uma solicitação com o cabeçalho Accept-Encoding e um valor de mycustomcompression.](response-compression/_static/request-custom-compression.png)

## <a name="compression-with-secure-protocol"></a><span data-ttu-id="21b21-232">Compactação com protocolo seguro</span><span class="sxs-lookup"><span data-stu-id="21b21-232">Compression with secure protocol</span></span>

<span data-ttu-id="21b21-233">As respostas compactadas através de conexões seguras podem ser controladas com o `EnableForHttps` opção, que é desabilitada por padrão.</span><span class="sxs-lookup"><span data-stu-id="21b21-233">Compressed responses over secure connections can be controlled with the `EnableForHttps` option, which is disabled by default.</span></span> <span data-ttu-id="21b21-234">Usar a compactação com páginas geradas dinamicamente pode levar a problemas de segurança, como o [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) e [violação](https://wikipedia.org/wiki/BREACH_(security_exploit)) ataques.</span><span class="sxs-lookup"><span data-stu-id="21b21-234">Using compression with dynamically generated pages can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span>

## <a name="adding-the-vary-header"></a><span data-ttu-id="21b21-235">Adicionar o cabeçalho Vary</span><span class="sxs-lookup"><span data-stu-id="21b21-235">Adding the Vary header</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="21b21-236">Ao compactar respostas com base no `Accept-Encoding` cabeçalho, há potencialmente várias versões compactadas da resposta e uma versão descompactada.</span><span class="sxs-lookup"><span data-stu-id="21b21-236">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="21b21-237">Para instruir os caches de cliente e proxy que várias versões existem e devem ser armazenadas, o `Vary` cabeçalho é adicionado com uma `Accept-Encoding` valor.</span><span class="sxs-lookup"><span data-stu-id="21b21-237">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="21b21-238">No ASP.NET Core 2.0 ou posterior, o middleware adiciona o `Vary` cabeçalho automaticamente quando a resposta é compactada.</span><span class="sxs-lookup"><span data-stu-id="21b21-238">In ASP.NET Core 2.0 or later, the middleware adds the `Vary` header automatically when the response is compressed.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="21b21-239">Ao compactar respostas com base no `Accept-Encoding` cabeçalho, há potencialmente várias versões compactadas da resposta e uma versão descompactada.</span><span class="sxs-lookup"><span data-stu-id="21b21-239">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="21b21-240">Para instruir os caches de cliente e proxy que várias versões existem e devem ser armazenadas, o `Vary` cabeçalho é adicionado com uma `Accept-Encoding` valor.</span><span class="sxs-lookup"><span data-stu-id="21b21-240">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="21b21-241">No núcleo do ASP.NET 1. x, adicionando o `Vary` cabeçalho para a resposta é realizado manualmente:</span><span class="sxs-lookup"><span data-stu-id="21b21-241">In ASP.NET Core 1.x, adding the `Vary` header to the response is accomplished manually:</span></span>

<span data-ttu-id="21b21-242">[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="21b21-242">[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet1)]</span></span>

::: moniker-end

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a><span data-ttu-id="21b21-243">Problema de middleware quando atrás de um proxy reverso Nginx</span><span class="sxs-lookup"><span data-stu-id="21b21-243">Middleware issue when behind an Nginx reverse proxy</span></span>

<span data-ttu-id="21b21-244">Quando uma solicitação é delegada por Nginx, o `Accept-Encoding` cabeçalho é removido.</span><span class="sxs-lookup"><span data-stu-id="21b21-244">When a request is proxied by Nginx, the `Accept-Encoding` header is removed.</span></span> <span data-ttu-id="21b21-245">Isso impede que o middleware da compactação de resposta.</span><span class="sxs-lookup"><span data-stu-id="21b21-245">This prevents the middleware from compressing the response.</span></span> <span data-ttu-id="21b21-246">Para obter mais informações, consulte [NGINX: compactação e descompactação de](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span><span class="sxs-lookup"><span data-stu-id="21b21-246">For more information, see [NGINX: Compression and Decompression](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span></span> <span data-ttu-id="21b21-247">Esse problema é acompanhado por [descobrir a compactação de passagem para Nginx (BasicMiddleware 123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span><span class="sxs-lookup"><span data-stu-id="21b21-247">This issue is tracked by [Figure out pass-through compression for Nginx (BasicMiddleware #123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span></span>

## <a name="working-with-iis-dynamic-compression"></a><span data-ttu-id="21b21-248">Trabalhando com compactação dinâmica do IIS</span><span class="sxs-lookup"><span data-stu-id="21b21-248">Working with IIS dynamic compression</span></span>

<span data-ttu-id="21b21-249">Se você tiver um active IIS compactação módulo dinâmico configurado no nível do servidor que você deseja desabilitar para um aplicativo, você pode fazer isso com uma adição à sua *Web. config* arquivo.</span><span class="sxs-lookup"><span data-stu-id="21b21-249">If you have an active IIS Dynamic Compression Module configured at the server level that you would like to disable for an app, you can do so with an addition to your *web.config* file.</span></span> <span data-ttu-id="21b21-250">Para obter mais informações, consulte [Desabilitando módulos do IIS](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span><span class="sxs-lookup"><span data-stu-id="21b21-250">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="21b21-251">Solução de problemas</span><span class="sxs-lookup"><span data-stu-id="21b21-251">Troubleshooting</span></span>

<span data-ttu-id="21b21-252">Usar uma ferramenta como [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), ou [carteiro](https://www.getpostman.com/), que permitem que você defina o `Accept-Encoding` cabeçalho de solicitação e analise os cabeçalhos de resposta, o tamanho e o corpo.</span><span class="sxs-lookup"><span data-stu-id="21b21-252">Use a tool like [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), or [Postman](https://www.getpostman.com/), which allow you to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span> <span data-ttu-id="21b21-253">O Middleware de compactação de resposta compacta respostas que atendem às seguintes condições:</span><span class="sxs-lookup"><span data-stu-id="21b21-253">The Response Compression Middleware compresses responses that meet the following conditions:</span></span>

* <span data-ttu-id="21b21-254">O `Accept-Encoding` cabeçalho estiver presente com um valor de `gzip`, `*`, ou codificação personalizada que corresponda a um provedor personalizado de compactação estabelecida por você.</span><span class="sxs-lookup"><span data-stu-id="21b21-254">The `Accept-Encoding` header is present with a value of `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="21b21-255">O valor não deve ser `identity` ou tem um valor de qualidade (qvalue, `q`) configuração de 0 (zero).</span><span class="sxs-lookup"><span data-stu-id="21b21-255">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="21b21-256">O tipo MIME (`Content-Type`) deve ser definido e deve corresponder a um tipo MIME configurado no [ResponseCompressionOptions](/dotnet/api/microsoft.aspnetcore.responsecompression.responsecompressionoptions).</span><span class="sxs-lookup"><span data-stu-id="21b21-256">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the [ResponseCompressionOptions](/dotnet/api/microsoft.aspnetcore.responsecompression.responsecompressionoptions).</span></span>
* <span data-ttu-id="21b21-257">A solicitação não deve incluir o `Content-Range` cabeçalho.</span><span class="sxs-lookup"><span data-stu-id="21b21-257">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="21b21-258">A solicitação deve usar o protocolo seguro (http), a menos que o protocolo seguro (https) é configurado nas opções de Middleware de compactação de resposta.</span><span class="sxs-lookup"><span data-stu-id="21b21-258">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="21b21-259">*Observe o perigo [descrito acima](#compression-with-secure-protocol) ao habilitar a compactação de conteúdo segura.*</span><span class="sxs-lookup"><span data-stu-id="21b21-259">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

## <a name="additional-resources"></a><span data-ttu-id="21b21-260">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="21b21-260">Additional resources</span></span>

* [<span data-ttu-id="21b21-261">Inicialização de aplicativos</span><span class="sxs-lookup"><span data-stu-id="21b21-261">Application Startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="21b21-262">Middleware</span><span class="sxs-lookup"><span data-stu-id="21b21-262">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="21b21-263">Rede Mozilla Developer: Codificação aceita</span><span class="sxs-lookup"><span data-stu-id="21b21-263">Mozilla Developer Network: Accept-Encoding</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [<span data-ttu-id="21b21-264">RFC 7231 seção 3.1.2.1: Codings conteúdos</span><span class="sxs-lookup"><span data-stu-id="21b21-264">RFC 7231 Section 3.1.2.1: Content Codings</span></span>](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [<span data-ttu-id="21b21-265">RFC 7230 seção 4.2.3: Codificação de Gzip</span><span class="sxs-lookup"><span data-stu-id="21b21-265">RFC 7230 Section 4.2.3: Gzip Coding</span></span>](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [<span data-ttu-id="21b21-266">Versão de especificação de formato de arquivo GZIP 4.3</span><span class="sxs-lookup"><span data-stu-id="21b21-266">GZIP file format specification version 4.3</span></span>](http://www.ietf.org/rfc/rfc1952.txt)
