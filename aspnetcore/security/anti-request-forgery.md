---
title: "Impedindo ataques CSRF (falsificação XSRF /) de solicitação entre sites no núcleo do ASP.NET"
author: steve-smith
ms.author: riande
description: "Impedindo ataques CSRF (falsificação XSRF /) de solicitação entre sites no núcleo do ASP.NET"
manager: wpickett
ms.date: 7/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/anti-request-forgery
ms.openlocfilehash: d7df8f91e88290509c8751a4b69804b60138846e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="preventing-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a>Impedindo ataques CSRF (falsificação XSRF /) de solicitação entre sites no núcleo do ASP.NET

[Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), e [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="what-attack-does-anti-forgery-prevent"></a>O ataque de falsificação contra impedir que?

Falsificação de solicitação entre sites (também conhecido como XSRF ou CSRF, pronunciado *consulte navegação*) é um ataque contra aplicativos web hospedados no qual um site mal-intencionado pode influenciar a interação entre um navegador de cliente e um site da web que relações de confiança esse navegador. Esses ataques são possibilitados como navegadores da web enviam alguns tipos de tokens de autenticação automaticamente com todas as solicitações para um site da web. Essa forma de exploração é também conhecido como um *ataque de um clique* ou como *sessão riding*, pois aproveita o ataque do usuário do anteriormente autenticados sessão.

Um exemplo de um ataque CSRF:

1. Um usuário faz logon em `www.example.com`, usando a autenticação de formulários.
2. O servidor autentica o usuário e envia uma resposta que inclui um cookie de autenticação.
3. O usuário acessa um site mal-intencionado.

   O site mal-intencionado contém um formulário HTML, semelhante ao seguinte:

```html
   <h1>You Are a Winner!</h1>
     <form action="http://example.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw" />
       <input type="hidden" name="Amount" value="1000000" />
     <input type="submit" value="Click Me"/>
   </form>
```

Observe que a ação de formulário envia para o site vulnerável, não para o site mal-intencionado. Esta é a parte de "sites" de CSRF.

4. O usuário clica no botão Enviar. O navegador inclui automaticamente o cookie de autenticação para o domínio solicitado (o site vulnerável neste caso) com a solicitação.
5. A solicitação é executado no servidor com o contexto de autenticação do usuário e pode fazer tudo o que um usuário autenticado tem permissão para fazer.

Este exemplo requer que o usuário clicar no botão do formulário. A página mal-intencionado pode:

* Execute um script que envia automaticamente o formulário.
* Envia o envio de um formulário como uma solicitação AJAX. 
* Use um formulário oculto com o CSS. 

Usando o SSL não impede que um ataque CSRF, o site mal-intencionado pode enviar um `https://` solicitação. 

Pontos de extremidade de site que respondem a de destino a alguns ataques `GET` solicitações, nesse caso uma marca de imagem pode ser usado para executar a ação (essa forma de ataque comuns em sites de Fórum que permitem imagens mas bloqueiam JavaScript). Aplicativos que a alteração de estado `GET` solicitações são vulneráveis contra ataques mal-intencionados.

Ataques CSRF são possíveis nos sites da web que usam cookies para autenticação, como navegadores enviam todos os cookies relevantes para o site de destino. No entanto, ataques CSRF não estão limitados a exploração de cookies. Por exemplo, a autenticação básica e resumida também são vulneráveis. Depois que um usuário faz logon com a autenticação básica ou Digest, o navegador envia automaticamente as credenciais, até que a sessão termina.

Observação: nesse contexto, *sessão* refere-se à sessão do lado do cliente durante o qual o usuário é autenticado. Isso não está relacionado a sessões do lado do servidor ou [sessão middleware](xref:fundamentals/app-state).

Os usuários podem proteger contra vulnerabilidades CSRF por:
* Log de sites da web quando ele tiver terminado de usá-los.
* Limpar periodicamente os cookies do seu navegador.

No entanto, as vulnerabilidades CSRF são basicamente um problema com o aplicativo web, não o usuário final.

## <a name="how-does-aspnet-core-mvc-address-csrf"></a>Como o ASP.NET MVC de núcleo endereço CSRF?

> [!WARNING]
> ASP.NET Core implementa falsificação-solicitação usando o [pilha de proteção de dados do ASP.NET Core](xref:security/data-protection/introduction). Proteção de dados do ASP.NET Core deve ser configurada para funcionar em um farm de servidores. Consulte [Configurando a proteção de dados](xref:security/data-protection/configuration/overview) para obter mais informações.

Configuração de proteção de dados do ASP.NET Core falsificação-solicitação padrão 

No núcleo do ASP.NET MVC 2.0 a [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injeta tokens antifalsificação para elementos de formulário HTML. Por exemplo, a seguinte marcação em um arquivo Razor gerará automaticamente tokens antifalsificação:

```html
<form method="post">
  <!-- form markup -->
</form>
```

A geração automática de tokens antifalsificação para elementos de formulário HTML acontece quando:

* O `form` marca contém o `method="post"` atributo AND

  * O atributo de ação está vazio. ( `action=""`) OU
  * O atributo de ação não for fornecido. (`<form method="post">`)

Você pode desabilitar a geração automática de tokens antifalsificação para elementos de formulário HTML por:

* Desabilitando explicitamente `asp-antiforgery`. Por exemplo

 ```html
  <form method="post" asp-antiforgery="false">
  </form>
  ```

* Aceitar o elemento de formulário fora de auxiliares de marcação usando o auxiliar de marca [! recusar símbolo](xref:mvc/views/tag-helpers/intro#opt-out).

 ```html
  <!form method="post">
  </!form>
  ```

* Remover o `FormTagHelper` do modo de exibição. Você pode remover o `FormTagHelper` de um modo de exibição, adicionando a seguinte diretiva para o modo de exibição do Razor:

 ```html
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> [Páginas Razor](xref:mvc/razor-pages/index) são protegidos automaticamente contra XSRF/CSRF. Você não precisa escrever nenhum código adicional. Consulte [XSRF/CSRF e páginas Razor](xref:mvc/razor-pages/index#xsrf) para obter mais informações.

A abordagem mais comum para proteger contra ataques CSRF é o padrão de token sincronizador (STP). STP é uma técnica usada quando o usuário solicita uma página de dados do formulário. O servidor envia um token associado com a identidade do usuário atual para o cliente. O cliente envia o token para o servidor para verificação. Se o servidor recebe um token que não corresponde a identidade do usuário autenticado, a solicitação será rejeitada. O token é exclusivo e imprevisível. O token também pode ser usado para garantir o sequenciamento adequado de uma série de solicitações (página garantia 1 precede a página 2 que precede a página 3). Todos os formulários em modelos do MVC do ASP.NET Core geram tokens antiforgery. Os exemplos a seguir da lógica de exibição geram tokens antiforgery:

```html
<form asp-controller="Manage" asp-action="ChangePassword" method="post">

</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    
}
```

Você pode adicionar explicitamente um token antiforgery para um ``<form>`` elemento sem o uso de auxiliares de marcação com o auxiliar HTML ``@Html.AntiForgeryToken``:


```html
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

Em cada um dos casos anteriores, o ASP.NET Core adicionará um campo de formulário oculto semelhante à seguinte:
```html
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkSldwD9CpLRyOtm6FiJB1Jr_F3FQJQDvhlHoLNJJrLA6zaMUmhjMsisu2D2tFkAiYgyWQawJk9vNm36sYP1esHOtamBEPvSk1_x--Sg8Ey2a-d9CV2zHVWIN9MVhvKHOSyKqdZFlYDVd69XYx-rOWPw3ilHGLN6K0Km-1p83jZzF0E4WU5OGg5ns2-m9Yw" />
```

ASP.NET Core inclui três [filtros](xref:mvc/controllers/filters) para trabalhar com tokens antiforgery: ``ValidateAntiForgeryToken``, ``AutoValidateAntiforgeryToken``, e ``IgnoreAntiforgeryToken``.

<a name="vaft"></a>

### <a name="validateantiforgerytoken"></a>ValidateAntiForgeryToken

O ``ValidateAntiForgeryToken`` é um filtro de ação que pode ser aplicado a uma ação individual, um controlador ou globalmente. Solicitações feitas para ações com esse filtro aplicado serão bloqueadas, a menos que a solicitação inclui um token antiforgery válido.

```c#
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> RemoveLogin(RemoveLoginViewModel account)
{
    ManageMessageId? message = ManageMessageId.Error;
    var user = await GetCurrentUserAsync();
    if (user != null)
    {
        var result = await _userManager.RemoveLoginAsync(user, account.LoginProvider, account.ProviderKey);
        if (result.Succeeded)
        {
            await _signInManager.SignInAsync(user, isPersistent: false);
            message = ManageMessageId.RemoveLoginSuccess;
        }
    }
    return RedirectToAction(nameof(ManageLogins), new { Message = message });
}
```

O ``ValidateAntiForgeryToken`` atributo requer um token para solicitações para os métodos de ação decora, incluindo `HTTP GET` solicitações. Se você aplicá-lo em larga escala, você pode substituí-la com o ``IgnoreAntiforgeryToken`` atributo.

### <a name="autovalidateantiforgerytoken"></a>AutoValidateAntiforgeryToken

Aplicativos ASP.NET Core geralmente não gerar tokens de antiforgery para métodos HTTP seguros (GET, HEAD, opções e rastreamento). Em vez de em larga escala aplicar o ``ValidateAntiForgeryToken`` atributo e, em seguida, substituindo-o com ``IgnoreAntiforgeryToken`` atributos, você pode usar o ``AutoValidateAntiforgeryToken`` atributo. Esse atributo funciona de forma idêntica ao ``ValidateAntiForgeryToken`` de atributos, exceto pelo fato de que não exige tokens para solicitações feitas usando os seguintes métodos HTTP:

* OBTER
* HOME
* OPÇÕES
* TRACE

Recomendamos que você use ``AutoValidateAntiforgeryToken`` em larga escala para cenários de não-API. Isso garante que as ações de POSTAGEM são protegidas por padrão. A alternativa é ignorar tokens antiforgery por padrão, a menos que ``ValidateAntiForgeryToken`` é aplicado ao método de ação individuais. É mais provável que neste cenário para um método de ação de POST ser esquerda desprotegida, deixando o seu aplicativo vulnerável a ataques CSRF. POSTAGENS mesmo anônimas devem enviar o token antiforgery.

Observação: APIs não têm um mecanismo automático para enviar a parte do cookie não do token; sua implementação provavelmente depende de sua implementação de código do cliente. Alguns exemplos são mostrados abaixo.


Exemplo (nível de classe):

```c#
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

Exemplo (global):

```c#
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

<a name="iaft"></a>

### <a name="ignoreantiforgerytoken"></a>IgnoreAntiforgeryToken

O ``IgnoreAntiforgeryToken`` filtro é usado para eliminar a necessidade de um token antiforgery estar presente para uma determinada ação (ou controlador). Quando aplicado, substituirá este filtro ``ValidateAntiForgeryToken`` e/ou ``AutoValidateAntiforgeryToken`` filtros especificados em um nível mais alto (global ou em um controlador).

```c#
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

## <a name="javascript-ajax-and-spas"></a>JavaScript e AJAX SPAs

Em aplicativos tradicionais baseados em HTML, antiforgery tokens são passados para o servidor usando campos ocultos do formulário. Em aplicativos modernos baseados em JavaScript e aplicativos de única página (SPAs), muitas solicitações são feitas por meio de programação. Essas solicitações do AJAX podem usar outras técnicas (como cabeçalhos de solicitação ou cookies) para enviar o token. Se os cookies são usados para armazenar os tokens de autenticação e para autenticar solicitações de API no servidor, CSRF será um problema potencial. No entanto, se o armazenamento local é usado para armazenar o token, CSRF vulnerabilidade pode ser atenuada, desde que os valores do armazenamento local não são enviados automaticamente para o servidor com cada nova solicitação. Portanto, usando o armazenamento local para armazenar o token antiforgery no cliente e enviar o token como um cabeçalho de solicitação é uma abordagem recomendada.

### <a name="angularjs"></a>AngularJS

AngularJS usa uma convenção para endereço CSRF. Se o servidor envia um cookie com o nome ``XSRF-TOKEN``, o Angular ``$http`` serviço adicionará o valor desse cookie para um cabeçalho quando ele envia uma solicitação para este servidor. Esse processo é automático; Você não precisa definir o cabeçalho explicitamente. O nome do cabeçalho é ``X-XSRF-TOKEN``. O servidor deve detectar esse cabeçalho e validar seu conteúdo.

Para o ASP.NET Core API trabalhe com essa convenção:

* Configure seu aplicativo para fornecer um token em um cookie chamado``XSRF-TOKEN``
* Configurar o serviço antiforgery para procurar um cabeçalho chamado``X-XSRF-TOKEN``

```c#
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

[Exibir exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample).

### <a name="javascript"></a>JavaScript

Usando JavaScript com modos de exibição, você pode criar o token usando um serviço a partir do modo de exibição. Para fazer isso, você deve inserir o `Microsoft.AspNetCore.Antiforgery.IAntiforgery` serviço no modo de exibição e chamada `GetAndStoreTokens`, conforme mostrado:

[!code-csharp[Main](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,24)]

Essa abordagem elimina a necessidade de lidar diretamente com configuração cookies do servidor ou lê-los do cliente.

JavaScript pode também acessar tokens fornecidos em cookies e, em seguida, usar o conteúdo do cookie para criar um cabeçalho com o valor do token, conforme mostrado abaixo.

```c#
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
  new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

Em seguida, supondo que você construir seu script solicitações para enviar o token em um cabeçalho chamado ``X-CSRF-TOKEN``, configure o serviço antiforgery para procurar o ``X-CSRF-TOKEN`` cabeçalho:

```c#
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

O exemplo a seguir usa o jQuery para fazer uma solicitação AJAX com o cabeçalho apropriado:

```javascript
var csrfToken = $.cookie("CSRF-TOKEN");

$.ajax({
    url: "/api/password/changepassword",
    contentType: "application/json",
    data: JSON.stringify({ "newPassword": "ReallySecurePassword999$$$" }),
    type: "POST",
    headers: {
        "X-CSRF-TOKEN": csrfToken
    }
});
```

## <a name="configuring-antiforgery"></a>Configurando Antiforgery

`IAntiforgery`fornece a API para configurar o sistema antiforgery. Ele pode ser solicitado no `Configure` método o `Startup` classe. O exemplo a seguir usa o middleware da home page do aplicativo para gerar um token antiforgery e enviá-lo na resposta como um cookie (usando a convenção de nomenclatura Angular da padrão descrita acima):


```c#
public void Configure(IApplicationBuilder app, 
    IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;
        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // We can send the request token as a JavaScript-readable cookie, 
            // and Angular will use it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
    //
}
```

### <a name="options"></a>Opções

Você pode personalizar [opções antiforgery](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions#fields_summary) em `ConfigureServices`:

```c#
services.AddAntiforgery(options => 
{
  options.CookieDomain = "mydomain.com";
  options.CookieName = "X-CSRF-TOKEN-COOKIENAME";
  options.CookiePath = "Path";
  options.FormFieldName = "AntiforgeryFieldname";
  options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
  options.RequireSsl = false;
  options.SuppressXFrameOptionsHeader = false;
});
```

<!-- QAfix fix table -->

|Opção        | Descrição |
|------------- | ----------- |
|CookieDomain  | O domínio do cookie. Assume o padrão de `null`. |
|CookieName    | O nome do cookie. Se não estiver definida, o sistema irá gerar um nome exclusivo que começa com o `DefaultCookiePrefix` (". AspNetCore.Antiforgery."). |
|CookiePath    | O caminho definido no cookie. |
|FormFieldName | O nome do campo de formulário oculto usado pelo sistema antiforgery para processar tokens antiforgery nos modos de exibição. |
|HeaderName    | O nome do cabeçalho usado pelo sistema antiforgery. Se `null`, o sistema considerará apenas os dados de formulário. |
|RequireSsl    | Especifica se o SSL é exigido pelo sistema antiforgery. Assume o padrão de `false`. Se `true`, solicitações de não SSL falhará. |
|SuppressXFrameOptionsHeader  | Especifica se é suprimir a geração do `X-Frame-Options` cabeçalho. Por padrão, o cabeçalho é gerado com um valor de "SAMEORIGIN". Assume o padrão de `false`. |

Consulte https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions para obter mais informações.

### <a name="extending-antiforgery"></a>Estendendo Antiforgery

O [IAntiForgeryAdditionalDataProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) tipo permite aos desenvolvedores estender o comportamento do sistema anti-XSRF por dados adicionais do ciclo em cada token. O [GetAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_GetAdditionalData_Microsoft_AspNetCore_Http_HttpContext_) método é chamado sempre que um token de campo é gerado e o valor de retorno é inserido no token gerado. Um implementador poderia retornar um carimbo de hora, um valor de uso único ou qualquer outro valor e, em seguida, chamar [ValidateAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_ValidateAdditionalData_Microsoft_AspNetCore_Http_HttpContext_System_String_) para validar dados quando o token é validado. Nome de usuário do cliente já é inserido nos tokens gerados, portanto, não há necessidade de incluir essas informações. Se um token inclui dados complementares, mas não `IAntiForgeryAdditionalDataProvider` tiver sido configurado, os dados complementares não são validados.

## <a name="fundamentals"></a>Princípios básicos

Ataques CSRF contam com o comportamento do navegador padrão de envio de cookies associados a um domínio com todas as solicitações feitas a esse domínio. Esses cookies são armazenados dentro do navegador. Eles incluem frequentemente cookies de sessão para usuários autenticados. Autenticação baseada em cookie é uma forma popular de autenticação. Sistemas de autenticação baseada em token crescem popularidade, especialmente para SPAs e outros cenários "smart client".

### <a name="cookie-based-authentication"></a>Autenticação baseada em cookie

Depois que um usuário autenticado usando o nome de usuário e senha, eles são emitidos um token que pode ser usado para identificá-los e validar que eles foram autenticados. O token é armazenado como um cookie que acompanha cada solicitação de cliente. Gerar e validar esse cookie é feito pelo middleware de autenticação de cookie. ASP.NET Core fornece o cookie [middleware](../fundamentals/middleware.md) que serializa um objeto de usuário em um cookie criptografado e, em seguida, em solicitações subsequentes, valida o cookie recria a entidade de segurança e o atribui para a `User` propriedade no `HttpContext`.

Quando um cookie é usado, o cookie de autenticação é apenas um contêiner para o tíquete de autenticação de formulários. O tíquete é passado como o valor do cookie de autenticação de formulários com cada solicitação e é usado pela autenticação de formulários, no servidor, para identificar um usuário autenticado.

Quando um usuário está conectado a um sistema, uma sessão de usuário é criada no lado do servidor e é armazenada em um banco de dados ou algum outro repositório persistente. O sistema gera uma chave de sessão que aponta para a sessão atual no repositório de dados e ele é enviado como um cookie do lado do cliente. O servidor web verifica essa chave de sessão sempre que um usuário solicita um recurso que requer autorização. O sistema verifica se a sessão de usuário associado tem o privilégio para acessar o recurso solicitado. Nesse caso, a solicitação continuará. Caso contrário, a solicitação retorna como não autorizados. Nessa abordagem, os cookies são usados para fazer com que o aplicativo parece estar com monitoração de estado, pois ele é capaz de "lembrar" que o usuário foi autenticado anteriormente com o servidor.

### <a name="user-tokens"></a>Tokens de usuário

Autenticação baseada em token não armazena a sessão no servidor. Em vez disso, quando um usuário está conectado em eles são um token emitidos (não é um token antiforgery). Esse token contém todos os dados que é necessária para validar o token. Ele também contém informações de usuário, na forma de [declarações](https://docs.microsoft.com/dotnet/framework/security/claims-based-identity-model). Quando um usuário deseja acessar um recurso de servidor que requer autenticação, o token é enviado para o servidor com um cabeçalho de autorização adicionais na forma de portador {token}. Isso torna o aplicativo sem monitoração de estado como em cada solicitação subsequente o token é passado na solicitação de validação do lado do servidor. Esse token não é *criptografado*; em vez disso, ele é *codificado*. No lado do servidor, o token pode ser decodificado para acessar as informações não processadas dentro do token. Para enviar o token em solicitações subsequentes, você ou pode armazená-lo no armazenamento local do navegador ou em um cookie. Você não precisa se preocupar sobre vulnerabilidade XSRF se o token é armazenado no armazenamento local, mas é um problema se o token é armazenado em um cookie.

### <a name="multiple-applications-are-hosted-in-one-domain"></a>Vários aplicativos são hospedados em um domínio

Embora `example1.cloudapp.net` e `example2.cloudapp.net` são hosts diferentes, há uma relação de confiança implícita entre todos os hosts sob o `*.cloudapp.net` domínio. Essa relação de confiança implícita permite que os hosts potencialmente não confiáveis afetam uns dos outros cookies (as políticas de mesma origem que governam as solicitações do AJAX não necessariamente se aplicam a cookies HTTP). O tempo de execução do ASP.NET Core fornece alguma redução em que o nome de usuário é incorporada ao token de campo, assim mesmo se um subdomínio mal-intencionado é capaz de substituir um token de sessão, será possível gerar um token de campo válido para o usuário. No entanto, quando hospedados em um ambiente desse tipo as rotinas de anti-XSRF internas ainda não é possível proteger contra sequestro de sessão ou logon CSRF ataques. Ambientes de hospedagem compartilhados são vunerable sequestro de sessão, logon CSRF e outros ataques.


### <a name="additional-resources"></a>Recursos adicionais

* [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) na [Abrir projeto de segurança de aplicativo da Web](https://www.owasp.org/index.php/Main_Page) (OWASP).
