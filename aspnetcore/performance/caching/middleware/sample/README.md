# <a name="aspnet-core-response-caching-sample"></a>Exemplo de cache de resposta do ASP.NET Core

Este exemplo ilustra o uso do ASP.NET Core [Middleware de cache de resposta](https://docs.microsoft.com/aspnet/core/performance/caching/middleware).

O aplicativo responde com sua página de índice, incluindo um `Cache-Control` cabeçalho para configurar o comportamento do cache. O aplicativo também define o `Vary` cabeçalho para configurar o cache para fornecer a resposta somente se o `Accept-Encoding` cabeçalho de solicitações subsequentes corresponde da solicitação original.

Ao executar o exemplo, a página de índice é servida do cache quando armazenada e armazenado em cache por até 10 segundos.
