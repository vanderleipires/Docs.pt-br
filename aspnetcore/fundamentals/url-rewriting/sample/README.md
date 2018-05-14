# <a name="aspnet-core-url-rewriting-sample-aspnet-core-2x"></a>Amostra de reconfiguração de URL do ASP.NET Core (ASP.NET Core 2.x)

Esta amostra ilustra o uso do Middleware de Reconfiguração de URL do ASP.NET Core 2.x. O aplicativo demonstra as opções de redirecionamento e reconfiguração de URL. Para obter a amostra do ASP.NET Core 1.x, consulte [Amostra de reconfiguração de URL do ASP.NET Core (ASP.NET Core 1.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/1.x).

Durante a execução da amostra, uma resposta será fornecida, que mostra a URL reconfigurada ou redirecionada quando uma das regras é aplicada a uma URL de solicitação.

## <a name="examples-in-this-sample"></a>Exemplos desta amostra

* `AddRedirect("redirect-rule/(.*)", "redirected/$1")`
  - Código de status de êxito: 302 (Encontrado)
  - Exemplo (redirecionamento): **/redirect-rule/{capture_group}** para **/redirected/{capture_group}**
* `AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", skipRemainingRules: true)`
  - Código de status de êxito: 200 (OK)
  - Exemplo (reconfiguração): **/rewrite-rule/{capture_group_1}/{capture_group_2}** para **/rewritten?var1={capture_group_1}&var2={capture_group_2}**
* `AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")`
  - Código de status de êxito: 302 (Encontrado)
  - Exemplo (redirecionamento): **/apache-mod-rules-redirect/{capture_group}** para **/redirected?id={capture_group}**
* `AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")`
  - Código de status de êxito: 200 (OK)
  - Exemplo (reconfiguração): **/iis-rules-rewrite/{capture_group}** para **/rewritten?id={capture_group}**
* `Add(RedirectXMLRequests)`
  - Código de status de êxito: 301 (Movido Permanentemente)
  - Exemplo (redirecionamento): **/file.xml** para **/xmlfiles/file.xml**
* `Add(new RedirectPNGRequests(".png", "/png-images")))`<br>`Add(new RedirectPNGRequests(".jpg", "/jpg-images")))`
  - Código de status de êxito: 301 (Movido Permanentemente)
  - Exemplo (redirecionamento): **/image.png** para **/png-images/image.png**
  - Exemplo (redirecionamento): **/image.jpg** para **/jpg-images/image.jpg**

## <a name="using-a-physicalfileprovider"></a>Usando um `PhysicalFileProvider`
Também obtenha um `IFileProvider` criando um `PhysicalFileProvider` para passar para os métodos `AddApacheModRewrite()` e `AddIISUrlRewrite()`:
```csharp
using Microsoft.Extensions.FileProviders;
PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
```
## <a name="secure-redirection-extensions"></a>Proteger extensões de redirecionamento
Este exemplo inclui a configuração `WebHostBuilder` para o aplicativo usar URLs (**https://localhost:5001**, **https://localhost**) e um certificado de teste (**testCert.pfx**) para ajudá-lo a explorar esses métodos de redirecionamento. Adicione um deles à `RewriteOptions()` em **Startup.cs** para estudar seu comportamento.


|              Método              | Código de status |    Porta    |
|----------------------------------|:-----------:|:----------:|
| `.AddRedirectToHttpsPermanent()` |     301     | nulo (465) |
|     `.AddRedirectToHttps()`      |     302     | nulo (465) |
|    `.AddRedirectToHttps(301)`    |     301     | nulo (465) |
| `.AddRedirectToHttps(301, 5001)` |     301     |    5001    |

