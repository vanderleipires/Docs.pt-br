# <a name="response-compression-sample-application-aspnet-core-1x"></a>Aplicativo de exemplo de compactação de resposta (ASP.NET Core 1. x)

Este exemplo ilustra o uso do ASP.NET Core 1. x resposta Middleware de compactação para compactar respostas HTTP para. O exemplo demonstra o Gzip e provedores personalizados de compactação de respostas de texto e imagem e mostra como adicionar um tipo MIME para compactação. Para o exemplo do ASP.NET Core 2. x, consulte [aplicativo de exemplo de compactação de resposta (ASP.NET Core 2. x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples/2.x).

## <a name="examples-in-this-sample"></a>Exemplos neste exemplo
* `GzipCompressionProvider`
  * `text/plain`
    * **/**-Resposta de arquivo de texto Lorem Ipsum em bytes 2,044 que será compactada para 927 bytes
    * **/testfile1kb.txt** -resposta do arquivo de texto em bytes 1,033 que será compactada para bytes 47
    * **/ surjam** -resposta emitida como caracteres únicos intervalos de 1 segundo 
  * `image/svg+xml`
    * **/banner.SVG** -resposta de imagem de um escalonável vetor Graphics (SVG) em bytes 9,707 que será compactada para 4,459 bytes
* `CustomCompressionProvider`<br>Mostra como implementar um provedor personalizado de compactação para uso com o middleware

Quando a solicitação inclui o `Accept-Encoding` cabeçalho, o exemplo adiciona um `Vary: Accept-Encoding` cabeçalho para a resposta. O `Vary` cabeçalho instrui caches para manter várias cópias de resposta com base nos valores alternativos de `Accept-Encoding`, portanto, uma versão descompactada e compactado (gzip) são armazenados em caches para sistemas que podem aceitam o compactado ou o resposta não compactada.

## <a name="using-the-sample"></a>Usando o exemplo
1. Fazer uma solicitação usando [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), ou [carteiro](https://www.getpostman.com/) para o aplicativo sem um `Accept-Encoding` cabeçalho e observe a carga de resposta, tamanho da resposta, e cabeçalhos de resposta.
2. Adicionar um `Accept-Encoding: gzip` cabeçalho e observe o tamanho da resposta compactado e cabeçalhos de resposta. Consulte o tamanho da resposta drop e o `Content-Encoding: gzip` cabeçalho de resposta é incluído, o aplicativo de exemplo. Quando você examinar o corpo da resposta para Lorem Ipsum ou **testfile1kb.txt** resposta, consulte o texto é compactado e ilegível.
3. Adicionar um `Accept-Encoding: mycustomcompression` cabeçalho e observe os cabeçalhos de resposta. O `CustomCompressionProvider` é uma implementação vazia que realmente não compactar a resposta, mas você pode criar um wrapper de fluxo de compactação personalizada para o `CreateStream()` método.
