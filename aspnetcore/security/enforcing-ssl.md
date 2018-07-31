---
title: Impor HTTPS no ASP.NET Core
author: rick-anderson
description: Mostra como exigir HTTPS/TLS em um ASP.NET Core em aplicativo web.
ms.author: riande
ms.date: 2/9/2018
uid: security/enforcing-ssl
ms.openlocfilehash: a4ab91ef23a798c919a23a44f5a050bd3c09d56a
ms.sourcegitcommit: d99a8554c91f626cf5e466911cf504dcbff0e02e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/31/2018
ms.locfileid: "39356682"
---
# <a name="enforce-https-in-aspnet-core"></a>Impor HTTPS no ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Este documento demonstra como:

* Exigir HTTPS para todas as solicitações.
* Redirecione todas as solicitações HTTP para HTTPS.

> [!WARNING]
> Fazer **não** usar [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) em APIs da Web que recebe informações confidenciais. `RequireHttpsAttribute` usa códigos de status HTTP para redirecionar navegadores de HTTP para HTTPS. Os clientes da API não podem compreender ou obedecem redirecionamentos de HTTP para HTTPS. Esses clientes podem enviar informações por meio de HTTP. As APIs da Web deverá:
>
> * Não realizar a escuta em HTTP.
> * Feche a conexão com o código de status 400 (solicitação incorreta) e não atender à solicitação.

<a name="require"></a>
## <a name="require-https"></a>Exigir HTTPS

::: moniker range=">= aspnetcore-2.1"

É recomendável que todos os aplicativos do ASP.NET Core web chamar Middleware de redirecionamento de HTTPS ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) para redirecionar todas as solicitações HTTP para HTTPS.

O código a seguir chama `UseHttpsRedirection` no `Startup` classe:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

O código realçado anterior:

* Usa o padrão [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`). Aplicativos de produção devem chamar [UseHsts](#hsts).
* Usa o padrão [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (443).

O código a seguir chama [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) para configurar as opções de middleware:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

O código realçado anterior:

* Conjuntos [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) para `Status307TemporaryRedirect`, que é o valor padrão. Aplicativos de produção devem chamar [UseHsts](#hsts).
* Define a porta HTTPS para 5001. O valor padrão é 443.

Os seguintes mecanismos de definir a porta automaticamente:

* O middleware pode descobrir as portas por meio [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) quando as condições a seguir se aplicam:
  - O kestrel ou HTTP. sys é usada diretamente com pontos de extremidade HTTPS (também se aplica a execução do aplicativo com o depurador do Visual Studio Code).
  - Somente **uma porta HTTPS** é usado pelo aplicativo.
* O Visual Studio é usado:
  - O IIS Express tem habilitadas para HTTPS.
  - *launchsettings. JSON* define o `sslPort` para o IIS Express.

> [!NOTE]
> Quando um aplicativo é executado atrás de um proxy reverso (por exemplo, IIS, IIS Express), `IServerAddressesFeature` não está disponível. A porta deve ser configurada manualmente. Quando a porta não for definida, as solicitações não são redirecionadas.

A porta pode ser configurada definindo a [definição de configuração do Host da Web https_port](xref:fundamentals/host/web-host#https-port):

**Chave**: https_port **tipo**: *cadeia de caracteres*
**padrão**: um valor padrão não está definido.
**Definido usando**: `UseSetting` 
 **variável de ambiente**: `<PREFIX_>HTTPS_PORT` (o prefixo é `ASPNETCORE_` ao usar o Host da Web.)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

> [!NOTE]
> A porta pode ser configurada indiretamente, definindo a URL com o `ASPNETCORE_URLS` variável de ambiente. A variável de ambiente configura o servidor e, em seguida, o middleware indiretamente descobre a porta HTTPS por meio de `IServerAddressesFeature`.

Se nenhuma porta for definida:

* As solicitações não são redirecionadas.
* O middleware registra um aviso.

> [!NOTE]
> Uma alternativa ao uso de Middleware de redirecionamento de HTTPS (`UseHttpsRedirection`) é usar o Middleware de reconfiguração de URL (`AddRedirectToHttps`). `AddRedirectToHttps` também pode definir o código de status e a porta quando o redirecionamento é executado. Para obter mais informações, consulte [Middleware de reconfiguração de URL](xref:fundamentals/url-rewriting).
>
> Ao redirecionar para HTTPS sem a necessidade de regras de redirecionamento adicional, é recomendável usar o Middleware de redirecionamento de HTTPS (`UseHttpsRedirection`) descrito neste tópico.

::: moniker-end

::: moniker range="< aspnetcore-2.1"

O [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) é usada para exigir HTTPS. `[RequireHttpsAttribute]` pode decorar controladores ou métodos, ou podem ser aplicadas globalmente. Para aplicar o atributo globalmente, adicione o seguinte código ao `ConfigureServices` em `Startup`:

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

O código realçado anterior requer que todas as solicitações usem `HTTPS`; portanto, as solicitações HTTP são ignoradas. O seguinte código realçado redireciona todas as solicitações HTTP para HTTPS:

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

Para obter mais informações, consulte [Middleware de reconfiguração de URL](xref:fundamentals/url-rewriting). O middleware também permite que o aplicativo para definir o código de status ou o código de status e a porta quando o redirecionamento é executado.

Exigir HTTPS globalmente (`options.Filters.Add(new RequireHttpsAttribute());`) é uma prática recomendada de segurança. Aplicando o `[RequireHttps]` atributo a todas as páginas do Razor/controladores não é considerado tão seguro quanto exigir HTTPS globalmente. Você não pode garantir o `[RequireHttps]` atributo é aplicado quando novos controladores e páginas Razor são adicionadas.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a>Protocolo de segurança de transporte estrito HTTP (HSTS)

Por [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [segurança de transporte estrito HTTP (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) é um aprimoramento de segurança opcional que é especificado por um aplicativo web com o uso de um cabeçalho de resposta especial. Depois que um navegador com suporte recebe esse cabeçalho nesse navegador impedirá todas as comunicações sejam enviadas pelo HTTP para o domínio especificado e envia todas as comunicações via HTTPS. Ele também evita via HTTPS em prompts em navegadores.

ASP.NET Core 2.1 ou posterior implementa HSTS com o `UseHsts` método de extensão. O código a seguir chama `UseHsts` quando o aplicativo não está no [modo de desenvolvimento](xref:fundamentals/environments):

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

`UseHsts` não é recomendado em desenvolvimento porque o cabeçalho HSTS é altamente armazenável em cache por navegadores. Por padrão, `UseHsts` exclui o endereço de loopback local.

O código a seguir:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* Define o parâmetro de pré-carregamento do cabeçalho de segurança de transporte estrito. Preload não é parte do [especificação RFC HSTS](https://tools.ietf.org/html/rfc6797), mas é compatível com navegadores da web para pré-carregar sites HSTS na nova instalação. Veja [https://hstspreload.org/](https://hstspreload.org/) para obter mais informações.
* Habilita [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), que se aplica a política HSTS para hospedar subdomínios. 
* Define explicitamente o parâmetro de idade máxima do cabeçalho de segurança de transporte estrito para 60 dias. Se não for definido, o padrão é 30 dias. Consulte a [max-age diretiva](https://tools.ietf.org/html/rfc6797#section-6.1.1) para obter mais informações.
* Adiciona `example.com` à lista de hosts a serem excluídos.

`UseHsts` Exclui os seguintes hosts de loopback:

* `localhost` : O endereço de loopback do IPv4.
* `127.0.0.1` : O endereço de loopback do IPv4.
* `[::1]` : O endereço de loopback do IPv6.

O exemplo anterior mostra como adicionar outros hosts.
::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a>Recusa de HTTPS na criação do projeto

Os modelos de aplicativos do ASP.NET Core web 2.1 ou posterior (do Visual Studio ou a linha de comando do dotnet) habilitam [redirecionamento a HTTPS](#require) e [HSTS](#hsts). Para implantações que não exijam HTTPS, você pode recusar de HTTPS. Por exemplo, alguns serviços de back-end em que HTTPS está sendo manipulado externamente na borda, usando HTTPS em cada nó não é necessária.

A recusa de HTTPS:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Desmarque a **configurar para HTTPS** caixa de seleção.

![Diagrama de entidade](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[CLI do .NET Core](#tab/netcore-cli) 

Use a opção `--no-https`. Por exemplo

```console
dotnet new webapp --no-https
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-setup-a-developer-certificate-for-docker"></a>Como configurar um certificado de desenvolvedor para o Docker

Ver [esse problema de GitHub](https://github.com/aspnet/Docs/issues/6199).

::: moniker-end
