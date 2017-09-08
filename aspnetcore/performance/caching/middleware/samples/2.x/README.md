# <a name="aspnet-core-response-caching-sample-aspnet-core-2x"></a>Exemplo de cache de resposta de ASP.NET Core (ASP.NET Core 2. x)

Este exemplo ilustra o uso do ASP.NET Core [Middleware de cache de resposta](xref:performance/caching/middleware) com ASP.NET Core 2. x. Para o exemplo de 1. x do ASP.NET Core, consulte [exemplo de cache de resposta do ASP.NET Core (ASP.NET Core 1. x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/middleware/samples/1.x).

O aplicativo envia uma `Hello World!` mensagem e a hora atual juntamente com um `Cache-Control` cabeçalho para configurar o comportamento do cache. O aplicativo também envia uma `Vary` cabeçalho para configurar o cache para fornecer a resposta somente se o `Accept-Encoding` cabeçalho de solicitações subsequentes corresponde da solicitação original.

Ao executar o exemplo, uma resposta é servida do cache quando possível e armazenados por até 10 segundos.
