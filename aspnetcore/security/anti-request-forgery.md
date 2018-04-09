---
title: Ataques de evitar intersite solicitar CSRF (falsificação XSRF /) no núcleo do ASP.NET
author: steve-smith
description: Saiba como evitar ataques contra aplicativos web em que um site mal-intencionado pode influenciar a interação entre o aplicativo e um navegador cliente.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/19/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/anti-request-forgery
ms.openlocfilehash: ad50f8b261447d40ccc24c0ee006239aa976bf20
ms.sourcegitcommit: 7d02ca5f5ddc2ca3eb0258fdd6996fbf538c129a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/03/2018
---
# <a name="prevent-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a>Ataques de evitar intersite solicitar CSRF (falsificação XSRF /) no núcleo do ASP.NET

Por [Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), e [Rick Anderson](https://twitter.com/RickAndMSFT)

Falsificação de solicitação entre sites (também conhecido como XSRF ou CSRF, pronunciado *consulte navegação*) é um ataque contra aplicativos web hospedados no qual um aplicativo web mal-intencionado pode influenciar a interação entre o navegador do cliente e um aplicativo web que confia que Navegador. Esses ataques são possíveis, como navegadores da web enviam alguns tipos de tokens de autenticação automaticamente com todas as solicitações para um site. Essa forma de exploração é também conhecido como um *ataque de um clique* ou *sessão riding* porque o ataque aproveita o usuário do autenticado anteriormente sessão.

Um exemplo de um ataque CSRF:

1. Um usuário entra no `www.good-banking-site.com` usando a autenticação de formulários. O servidor autentica o usuário e envia uma resposta que inclui um cookie de autenticação. O site é vulnerável a ataques, pois ele confia qualquer solicitação recebida com um cookie de autenticação válido.
1. O usuário acessa um site mal-intencionado, `www.bad-crook-site.com`.

   O site mal-intencionado, `www.bad-crook-site.com`, contém um formulário HTML semelhante à seguinte:

   ```html
   <h1>Congratulations! You're a Winner!</h1>
   <form action="http://good-banking-site.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw">
       <input type="hidden" name="Amount" value="1000000">
       <input type="submit" value="Click to collect your prize!">
   </form>
   ```

   Observe que o formulário `action` postagens para o site vulnerável, não para o site mal-intencionado. Esta é a parte de "sites" de CSRF.

1. O usuário seleciona o botão de envio. O navegador faz a solicitação e inclui automaticamente o cookie de autenticação para o domínio solicitado, `www.good-banking-site.com`.
1. A solicitação é executada `www.good-banking-site.com` server com o contexto de autenticação do usuário e podem executar qualquer ação que um usuário autenticado tem permissão para executar.

Quando o usuário seleciona o botão para enviar o formulário, o site mal-intencionado pode:

* Execute um script que envia automaticamente o formulário.
* Envia o envio de um formulário como uma solicitação AJAX. 
* Use um formulário oculto com o CSS. 

Usar o HTTPS não impede que um ataque CSRF. O site mal-intencionado pode enviar um `https://www.good-banking-site.com/` solicitação tão fácil quanto que pode enviar uma solicitação não segura.

Alguns ataques de destino pontos de extremidade que respondem às solicitações GET, caso em que uma marca de imagem pode ser usada para executar a ação. Essa forma de ataque é comum em sites de Fórum que permitem imagens mas bloqueiam JavaScript. Aplicativos que alteram o estado em solicitações GET, onde variáveis ou os recursos são alterados, são vulneráveis a ataques mal-intencionados. **Solicitações GET que mudam de estado são seguros. Uma prática recomendada é nunca alterar estado em uma solicitação GET.**

Ataques CSRF são possíveis em relação a aplicativos web que usam cookies de autenticação porque:

* Os navegadores armazenam os cookies emitidos por um aplicativo web.
* Armazenado cookies incluem cookies de sessão para usuários autenticados.
* Navegadores enviam que todos os cookies associados a um domínio ao aplicativo web cada solicitação, independentemente de como a solicitação ao aplicativo foi gerada dentro do navegador.

No entanto, os ataques CSRF não estão limitadas a exploração de cookies. Por exemplo, a autenticação básica e resumida também são vulneráveis. Depois que um usuário entra com autenticação básica ou Digest, o navegador envia automaticamente as credenciais até que a sessão&dagger; termina.

&dagger;Nesse contexto, *sessão* refere-se à sessão do lado do cliente durante o qual o usuário é autenticado. É não relacionados para sessões do lado do servidor ou [Middleware de sessão do ASP.NET Core](xref:fundamentals/app-state).

Os usuários podem proteger contra vulnerabilidades CSRF tomando precauções:

* Inscreva-se de aplicativos da web quando terminar de usá-los.
* Limpar os cookies do navegador periodicamente.

No entanto, as vulnerabilidades CSRF são basicamente um problema com o aplicativo web, não o usuário final.

## <a name="authentication-fundamentals"></a>Conceitos básicos de autenticação

Autenticação baseada em cookie é uma forma popular de autenticação. Sistemas de autenticação baseada em token estão crescendo popularidade, especialmente para aplicativos de página única (SPAs).

### <a name="cookie-based-authentication"></a>Autenticação baseada em cookie

Quando um usuário é autenticado usando o nome de usuário e senha, eles são emitidos um token, que contém um tíquete de autenticação que pode ser usado para autenticação e autorização. O token é armazenado como um cookie que acompanha cada solicitação de cliente. Gerar e validar esse cookie é executada pelo Middleware de autenticação de Cookie. O [middleware](xref:fundamentals/middleware/index) serializa um objeto de usuário em um cookie criptografado. Em solicitações subsequentes, o middleware valida o cookie, recria a entidade de segurança e atribui a entidade de segurança de [usuário](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) propriedade [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).

### <a name="token-based-authentication"></a>Autenticação baseada em token

Quando um usuário é autenticado, eles são emitiu um token (não é um token antiforgery). O token contém informações de usuário na forma de [declarações](/dotnet/framework/security/claims-based-identity-model) ou um token de referência que aponta o aplicativo para o estado do usuário mantido no aplicativo. Quando um usuário tenta acessar um recurso que requer autenticação, o token é enviado para o aplicativo com um cabeçalho de autorização adicionais na forma de token de portador. Isso torna o aplicativo sem monitoração de estado. Em cada solicitação subsequente, o token é passado na solicitação de validação do lado do servidor. Esse token não *criptografado*; ele tem *codificado*. No servidor, o token é decodificado para acessar suas informações. Para enviar o token em solicitações subsequentes, armazene o token no armazenamento local do navegador. Não se preocupar sobre vulnerabilidade CSRF se o token é armazenado no armazenamento local do navegador. CSRF é uma preocupação quando o token é armazenado em um cookie.

### <a name="multiple-apps-hosted-at-one-domain"></a>Vários aplicativos hospedados em um domínio

Ambientes de hospedagem compartilhados são vulneráveis a sequestro de sessão, logon CSRF e outros ataques.

Embora `example1.contoso.net` e `example2.contoso.net` são hosts diferentes, há uma relação de confiança implícita entre os hosts sob o `*.contoso.net` domínio. Essa relação de confiança implícita permite que os hosts potencialmente não confiáveis afetam uns dos outros cookies (as políticas de mesma origem que governam as solicitações do AJAX não necessariamente se aplicam a cookies HTTP).

Ataques que exploram cookies confiáveis entre aplicativos hospedados no mesmo domínio podem ser evitados por compartilhamento não domínios. Quando cada aplicativo está hospedado em seu próprio domínio, não há nenhuma relação de confiança implícita de cookie para explorar.

## <a name="aspnet-core-antiforgery-configuration"></a>Configuração de ASP.NET Core antiforgery

> [!WARNING]
> ASP.NET Core implementa usando antiforgery [proteção de dados do ASP.NET Core](xref:security/data-protection/introduction). A pilha de proteção de dados deve ser configurada para funcionar em um farm de servidores. Consulte [Configurando a proteção de dados](xref:security/data-protection/configuration/overview) para obter mais informações.

No ASP.NET Core 2.0 ou posterior, o [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injeta antiforgery tokens em elementos de formulário HTML. A seguinte marcação em um arquivo Razor gera automaticamente tokens antiforgery:

```cshtml
<form method="post">
    ...
</form>
```

Similarily, [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) gera tokens antiforgery por padrão, se o método do formulário não é GET.

A geração automática de tokens antiforgery para elementos de formulário HTML acontece quando o `<form>` marca contém o `method="post"` atributo e qualquer uma das opções a seguir forem verdadeira:

  * O atributo de ação está vazio (`action=""`).
  * O atributo de ação não for fornecido (`<form method="post">`).

Geração automática de tokens antiforgery para elementos de formulário HTML pode ser desabilitada:

* Desabilitar explicitamente tokens antiforgery com o `asp-antiforgery` atributo:

  ```cshtml
  <form method="post" asp-antiforgery="false">
      ...
  </form>
  ```

* O elemento de formulário é desativado-out de auxiliares de marcação usando o auxiliar de marca [! recusar símbolo](xref:mvc/views/tag-helpers/intro#opt-out):

  ```cshtml
  <!form method="post">
      ...
  </!form>
  ```

* Remover o `FormTagHelper` do modo de exibição. O `FormTagHelper` podem ser removidas de um modo de exibição, adicionando a seguinte diretiva para o modo de exibição do Razor:

  ```cshtml
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> [Páginas Razor](xref:mvc/razor-pages/index) são protegidos automaticamente contra XSRF/CSRF. Para obter mais informações, consulte [XSRF/CSRF e páginas Razor](xref:mvc/razor-pages/index#xsrf).

A abordagem mais comum para proteger contra ataques CSRF é usar o *padrão de Token sincronizador* (STP). STP é usado quando o usuário solicita uma página de dados do formulário:

1. O servidor envia um token associado com a identidade do usuário atual para o cliente.
1. O cliente envia o token para o servidor para verificação.
1. Se o servidor recebe um token que não corresponde a identidade do usuário autenticado, a solicitação será rejeitada.

O token é exclusivo e imprevisível. O token também pode ser usado para garantir o sequenciamento adequado de uma série de solicitações (por exemplo, garantindo a sequência de solicitação de: a página 1 &ndash; página 2 &ndash; página 3). Todos os formulários em modelos do MVC do ASP.NET Core e páginas Razor geram tokens antiforgery. O seguinte par de exemplos de exibição gera tokens antiforgery:

```cshtml
<form asp-controller="Manage" asp-action="ChangePassword" method="post">
    ...
</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    ...
}
```

Adicionar explicitamente um token antiforgery para um `<form>` elemento sem o uso de auxiliares de marcação com o auxiliar HTML [ @Html.AntiForgeryToken ](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):

```cshtml
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

Em cada um dos casos anteriores, o ASP.NET Core adiciona um campo de formulário oculto semelhante à seguinte:

```cshtml
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkS ... s2-m9Yw">
```

ASP.NET Core inclui três [filtros](xref:mvc/controllers/filters) para trabalhar com tokens antiforgery:

* [ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)
* [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)
* [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)

## <a name="antiforgery-options"></a>Opções de antiforgery

Personalizar [opções antiforgery](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) em `Startup.ConfigureServices`:

```csharp
services.AddAntiforgery(options => 
{
    options.CookieDomain = "contoso.com";
    options.CookieName = "X-CSRF-TOKEN-COOKIENAME";
    options.CookiePath = "Path";
    options.FormFieldName = "AntiforgeryFieldname";
    options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
    options.RequireSsl = false;
    options.SuppressXFrameOptionsHeader = false;
});
```

| Opção | Descrição |
| ------ | ----------- |
| [Cookie](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | Determina as configurações usadas para criar o cookie antiforgery. |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiedomain) | O domínio do cookie. Assume o padrão de `null`. Essa propriedade está obsoleta e será removida em uma versão futura. A alternativa recomendada é Cookie.Domain. |
| [CookieName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiename) | O nome do cookie. Se não estiver definida, o sistema gera um nome exclusivo que começa com o [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (". AspNetCore.Antiforgery."). Essa propriedade está obsoleta e será removida em uma versão futura. A alternativa recomendada é Cookie.Name. |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiepath) | O caminho definido no cookie. Essa propriedade está obsoleta e será removida em uma versão futura. A alternativa recomendada é Cookie.Path. |
| [FormFieldName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | O nome do campo de formulário oculto usado pelo sistema antiforgery para processar tokens antiforgery nos modos de exibição. |
| [HeaderName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | O nome do cabeçalho usado pelo sistema antiforgery. Se `null`, o sistema considerará apenas os dados de formulário. |
| [RequireSsl](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.requiressl) | Especifica se o SSL é exigido pelo sistema antiforgery. Se `true`, falham de solicitações de não SSL. Assume o padrão de `false`. Essa propriedade está obsoleta e será removida em uma versão futura. A alternativa recomendada é definir Cookie.SecurePolicy. |
| [SuppressXFrameOptionsHeader](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | Especifica se é suprimir a geração do `X-Frame-Options` cabeçalho. Por padrão, o cabeçalho é gerado com um valor de "SAMEORIGIN". Assume o padrão de `false`. |

Para obter mais informações, consulte [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).

## <a name="configure-antiforgery-features-with-iantiforgery"></a>Configurar recursos antiforgery com IAntiforgery

[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) fornece a API para configurar recursos antiforgery. `IAntiforgery` pode ser solicitados no `Configure` método o `Startup` classe. O exemplo a seguir usa o middleware da home page do aplicativo para gerar um token antiforgery e enviá-lo na resposta como um cookie (usando a convenção de nomenclatura Angular da padrão descrita mais adiante neste tópico):

```csharp
public void Configure(IApplicationBuilder app, IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;

        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // The request token can be sent as a JavaScript-readable cookie, 
            // and Angular uses it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
}
```

### <a name="require-antiforgery-validation"></a>Exigir validação antiforgery

[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) é um filtro de ação que pode ser aplicado a uma ação individual, um controlador ou globalmente. Solicitações feitas para ações com esse filtro aplicado são bloqueadas, a menos que a solicitação inclui um token antiforgery válido.

```csharp
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> RemoveLogin(RemoveLoginViewModel account)
{
    ManageMessageId? message = ManageMessageId.Error;
    var user = await GetCurrentUserAsync();

    if (user != null)
    {
        var result = 
            await _userManager.RemoveLoginAsync(
                user, account.LoginProvider, account.ProviderKey);

        if (result.Succeeded)
        {
            await _signInManager.SignInAsync(user, isPersistent: false);
            message = ManageMessageId.RemoveLoginSuccess;
        }
    }

    return RedirectToAction(nameof(ManageLogins), new { Message = message });
}
```

O `ValidateAntiForgeryToken` atributo requer um token para solicitações para os métodos de ação decora, incluindo solicitações HTTP GET. Se o `ValidateAntiForgeryToken` atributo é aplicado em controladores do aplicativo, ele pode ser substituído com o `IgnoreAntiforgeryToken` atributo.

> [!NOTE]
> ASP.NET Core não oferece suporte à inclusão automaticamente tokens antiforgery solicitações GET.

### <a name="automatically-validate-antiforgery-tokens-for-unsafe-http-methods-only"></a>Validar automaticamente tokens antiforgery para métodos HTTP não seguros somente

Aplicativos ASP.NET Core não geram tokens de antiforgery para métodos HTTP seguros (GET, HEAD, opções e rastreamento). Em vez de em larga escala aplicar o `ValidateAntiForgeryToken` atributo e, em seguida, substituindo-o com `IgnoreAntiforgeryToken` atributos, o [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) atributo pode ser usado. Esse atributo funciona de forma idêntica ao `ValidateAntiForgeryToken` de atributos, exceto pelo fato de que não exige tokens para solicitações feitas usando os seguintes métodos HTTP:

* OBTER
* HOME
* OPÇÕES
* TRACE

Recomendamos o uso de `AutoValidateAntiforgeryToken` em larga escala para cenários de não-API. Isso garante que as ações de POSTAGEM são protegidas por padrão. A alternativa é ignorar tokens antiforgery por padrão, a menos que `ValidateAntiForgeryToken` é aplicado a métodos de ação individual. Ele provavelmente neste cenário para um método de ação de POSTAGEM deve ser desprotegido por engano, deixando o aplicativo vulnerável a ataques CSRF. Todas as postagens devem enviar o token antiforgery.

APIs não tem um mecanismo automático para enviar a parte do cookie não do token. A implementação provavelmente depende a implementação de código do cliente. Alguns exemplos são mostrados abaixo:

Exemplo de nível de classe:

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

Exemplo global:

```csharp
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

### <a name="override-global-or-controller-antiforgery-attributes"></a>Substituição global ou atributos antiforgery controlador

O [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) filtro é usado para eliminar a necessidade de um token de antiforgery para uma determinada ação (ou controlador). Quando aplicado, esse filtro substitui `ValidateAntiForgeryToken` e `AutoValidateAntiforgeryToken` filtros especificados em um nível mais alto (global ou em um controlador).

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
    [HttpPost]
    [IgnoreAntiforgeryToken]
    public async Task<IActionResult> DoSomethingSafe(SomeViewModel model)
    {
        // no antiforgery token required
    }
}
```

## <a name="refresh-tokens-after-authentication"></a>Tokens de atualização após a autenticação

Tokens devem ser atualizados depois que o usuário é autenticado ao redirecionar o usuário para uma exibição ou página das páginas Razor.

## <a name="javascript-ajax-and-spas"></a>JavaScript e AJAX SPAs

Em aplicativos tradicionais baseados em HTML, antiforgery tokens são passados para o servidor usando campos ocultos do formulário. Em aplicativos modernos baseados em JavaScript e SPAs, muitas solicitações são feitas por meio de programação. Essas solicitações do AJAX podem usar outras técnicas (como cabeçalhos de solicitação ou cookies) para enviar o token.

Se os cookies são usados para armazenar os tokens de autenticação e para autenticar solicitações de API no servidor, CSRF é um problema potencial. Se o armazenamento local é usado para armazenar o token, vulnerabilidade CSRF pode ser reduzida porque os valores do armazenamento local não são enviados automaticamente para o servidor com cada solicitação. Portanto, usando o armazenamento local para armazenar o token antiforgery no cliente e enviar o token como um cabeçalho de solicitação é uma abordagem recomendada.

### <a name="javascript"></a>JavaScript

Usando JavaScript com modos de exibição, o token pode ser criado usando um serviço a partir do modo de exibição. Injetar o [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) serviço no modo de exibição e chamada [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):

[!code-csharp[](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,12-13,35-36)]

Essa abordagem elimina a necessidade de lidar diretamente com configuração cookies do servidor ou lê-los do cliente.

O exemplo anterior usa JavaScript para ler o valor do campo oculto para o cabeçalho de POSTAGEM de AJAX.

JavaScript também pode acessar tokens em cookies e usar o conteúdo do cookie para criar um cabeçalho com o valor do token.

```csharp
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
    new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

Supondo que o script solicitações para enviar o token em um cabeçalho chamado `X-CSRF-TOKEN`, configure o serviço antiforgery para procurar o `X-CSRF-TOKEN` cabeçalho:

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

O exemplo a seguir usa JavaScript para fazer uma solicitação AJAX com o cabeçalho apropriado:

```javascript
function getCookie(cname) {
    var name = cname + "=";
    var decodedCookie = decodeURIComponent(document.cookie);
    var ca = decodedCookie.split(';');
    for(var i = 0; i <ca.length; i++) {
        var c = ca[i];
        while (c.charAt(0) == ' ') {
            c = c.substring(1);
        }
        if (c.indexOf(name) == 0) {
            return c.substring(name.length, c.length);
        }
    }
    return "";
}

var csrfToken = getCookie("CSRF-TOKEN");

var xhttp = new XMLHttpRequest();
xhttp.onreadystatechange = function() {
    if (xhttp.readyState == XMLHttpRequest.DONE) {
        if (xhttp.status == 200) {
            alert(xhttp.responseText);
        } else {
            alert('There was an error processing the AJAX request.');
        }
    }
};
xhttp.open('POST', '/api/password/changepassword', true);
xhttp.setRequestHeader("Content-type", "application/json");
xhttp.setRequestHeader("X-CSRF-TOKEN", csrfToken);
xhttp.send(JSON.stringify({ "newPassword": "ReallySecurePassword999$$$" }));
```

### <a name="angularjs"></a>AngularJS

AngularJS usa uma convenção para endereço CSRF. Se o servidor envia um cookie com o nome `XSRF-TOKEN`, o AngularJS `$http` serviço adiciona o valor do cookie para um cabeçalho quando ele envia uma solicitação ao servidor. Esse processo é automático. O cabeçalho não precisa ser definidos explicitamente. O nome do cabeçalho é `X-XSRF-TOKEN`. O servidor deve detectar esse cabeçalho e validar seu conteúdo.

Para o ASP.NET Core API trabalhe com essa convenção:

* Configure seu aplicativo para fornecer um token em um cookie chamado `XSRF-TOKEN`.
* Configurar o serviço antiforgery para procurar um cabeçalho chamado `X-XSRF-TOKEN`.

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="extend-antiforgery"></a>Estender antiforgery

O [IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) tipo permite aos desenvolvedores estender o comportamento do sistema anti-CSRF por dados adicionais do ciclo em cada token. O [GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) método é chamado sempre que um token de campo é gerado e o valor de retorno é inserido no token gerado. Um implementador poderia retornar um carimbo de hora, um valor de uso único ou qualquer outro valor e, em seguida, chamar [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) para validar dados quando o token é validado. Nome de usuário do cliente já é inserido nos tokens gerados, portanto, não há necessidade de incluir essas informações. Se um token inclui dados complementares, mas não `IAntiForgeryAdditionalDataProvider` é configurado, os dados complementares não são validados.

## <a name="additional-resources"></a>Recursos adicionais

* [CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) na [Abrir projeto de segurança de aplicativo da Web](https://www.owasp.org/index.php/Main_Page) (OWASP).
