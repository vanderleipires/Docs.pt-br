# <a name="aspnet-core-url-rewriting-sample-aspnet-core-2x"></a>Exemplo de regravação de URL de núcleo do ASP.NET (ASP.NET Core 2. x)

Este exemplo ilustra o uso do ASP.NET Core 2. x Middleware de regravação de URL. O aplicativo demonstra o redirecionamento de URL e opções de regravação de URL. Para o exemplo de 1. x do ASP.NET Core, consulte [exemplo de regravação de URL do ASP.NET Core (ASP.NET Core 1. x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/1.x).

Ao executar o exemplo, uma resposta será servida que mostra a URL redirecionada ou reconfigurada quando uma das regras é aplicada a uma URL de solicitação.

## <a name="examples-in-this-sample"></a>Exemplos neste exemplo

* `AddRedirect("redirect-rule/(.*)", "$1")`
  - Código de status de sucesso: 302 (não encontrado)
  - Exemplo (redirecionamento): **/redirect-rule / {capture_group}** para **/redirected/ {capture_group}**
* `AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", skipRemainingRules: true)`
  - Código de status de sucesso: 200 (Okey)
  - Exemplo (reconfiguração): **/rewrite-rule / {capture_group_1} / {capture_group_2}** para **/ reescrita? var1 = {capture_group_1} & var2 = {capture_group_2}**
* `AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")`
  - Código de status de sucesso: 302 (não encontrado)
  - Exemplo (redirecionamento): **/apache-mod-rules-redirect / {capture_group}** para **/ redirecionado? id = {capture_group}**
* `AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")`
  - Código de status de sucesso: 200 (Okey)
  - Exemplo (reconfiguração): **/iis-rules-rewrite / {capture_group}** para **/ reescrita? id = {capture_group}**
* `Add(RedirectXMLRequests)`
  - Código de status de sucesso: 301 (movido permanentemente)
  - Exemplo (redirecionamento): **/file.xml** para **/xmlfiles/file.xml**
* `Add(new RedirectPNGRequests(".png", "/png-images")))`<br>`Add(new RedirectPNGRequests(".jpg", "/jpg-images")))`
  - Código de status de sucesso: 301 (movido permanentemente)
  - Exemplo (redirecionamento): **/image.png** para **/png-images/image.png**
  - Exemplo (redirecionamento): **/image.jpg** para **/jpg-images/image.jpg**

## <a name="using-a-physicalfileprovider"></a>Usando um`PhysicalFileProvider`
Você também pode obter um `IFileProvider` criando um `PhysicalFileProvider` para passar para o `AddApacheModRewrite()` e `AddIISUrlRewrite()` métodos:
```csharp
using Microsoft.Extensions.FileProviders;
PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
```
## <a name="secure-redirection-extensions"></a>Extensões de redirecionamento
Este exemplo inclui `WebHostBuilder` configuração para o aplicativo usar URLs (**https://localhost:5001**, **https://localhost**) e um certificado de teste (**testCert.pfx**) para ajudar você na exploração esses redireciona métodos. Adicionar qualquer um para o `RewriteOptions()` na **Startup.cs** para estudar seu comportamento.

Método | Código de status | Porta
--- | :---: | :---:
`.AddRedirectToHttpsPermanent()` | 301 | NULL (465)
`.AddRedirectToHttps()` | 302 | NULL (465)
`.AddRedirectToHttps(301)` | 301 | NULL (465)
`.AddRedirectToHttps(301, 5001)` | 301 | 5001
