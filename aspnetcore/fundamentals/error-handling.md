---
title: Tratar erros no ASP.NET Core
author: ardalis
description: Descubra como tratar erros em aplicativos ASP.NET Core.
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 11/30/2016
uid: fundamentals/error-handling
ms.openlocfilehash: 2fe46ecc32d61a7fafb2ad6e2a35456476608251
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273703"
---
# <a name="handle-errors-in-aspnet-core"></a>Tratar erros no ASP.NET Core

Por [Steve Smith](https://ardalis.com/) e [Tom Dykstra](https://github.com/tdykstra/)

Este artigo apresenta abordagens comuns para o tratamento de erros em aplicativos ASP.NET Core.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="the-developer-exception-page"></a>A página de exceção do desenvolvedor

Para configurar um aplicativo para exibir uma página que mostra informações detalhadas sobre exceções, instale o pacote NuGet `Microsoft.AspNetCore.Diagnostics` e adicione uma linha ao [método Configure na classe Startup](xref:fundamentals/startup):

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

Coloque `UseDeveloperExceptionPage` antes de qualquer middleware no qual você deseja capturar exceções, como `app.UseMvc`.

>[!WARNING]
> Habilite a página de exceção do desenvolvedor **somente quando o aplicativo estiver em execução no ambiente de Desenvolvimento**. Não é recomendável compartilhar informações de exceção detalhadas publicamente quando o aplicativo é executado em produção. [Saiba mais sobre como configurar ambientes](xref:fundamentals/environments).

Para ver a página de exceção do desenvolvedor, execute o aplicativo de exemplo com o ambiente definido como `Development` e adicione `?throw=true` à URL base do aplicativo. A página inclui várias guias com informações sobre a exceção e a solicitação. A primeira guia inclui um rastreamento de pilha. 

![Rastreamento de pilha](error-handling/_static/developer-exception-page.png)

A próxima guia mostra os parâmetros da cadeia de caracteres de consulta, se houver.

![Parâmetros de cadeia de caracteres de consulta](error-handling/_static/developer-exception-page-query.png)

Essa solicitação não tinha nenhum cookie, mas se tivesse, eles seriam exibidos na guia **Cookies**. Veja os cabeçalhos que foram passados na última guia.

![Cabeçalhos](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a>Configurando uma página de tratamento de exceção personalizada

Configure uma página do manipulador de exceção para usar quando o aplicativo não estiver em execução no ambiente `Development`.

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

Em um aplicativo Razor Pages, o modelo [dotnet novo](/dotnet/core/tools/dotnet-new) do Razor Pages fornece uma de erro e a classe de modelo de página `ErrorModel` na pasta *Páginas*.

Em um aplicativo MVC, não decore o método de ação do manipulador de erro com atributos de método HTTP, como `HttpGet`. Verbos explícitos impedem algumas solicitações de chegar ao método. Permita acesso anônimo ao método para que os usuários não autenticados possam capazes receber a exibição de erro.

Por exemplo, o seguinte método de manipulador de erro é fornecido pelo modelo MVC [dotnet novo](/dotnet/core/tools/dotnet-new) e aparece no controlador Home:

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

## <a name="configuring-status-code-pages"></a>Configurando páginas de código de status

Por padrão, o aplicativo não fornece uma página de código de status detalhada para códigos de status HTTP, como *404 Não Encontrado*. Para fornecer páginas de código de status, configure o middleware de páginas de código de status, adicionando uma linha ao método `Startup.Configure`:

```csharp
app.UseStatusCodePages();
```

Por padrão, o middleware de páginas de código de status adiciona manipuladores simples, somente de texto, para os códigos de status comuns, como o 404:

![Página 404](error-handling/_static/default-404-status-code.png)

O middleware permite vários métodos de extensão. Um método usa uma expressão lambda:

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]

Outro método usa uma cadeia de caracteres de formato e de tipo de conteúdo:

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

Há também métodos de extensão de redirecionamento e de nova execução. O método de redirecionamento envia um código de status 302 para o cliente:

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

O método de nova execução retorna o código de status original para o cliente, mas também executa o manipulador para a URL de redirecionamento:

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

As páginas de código de status podem ser desabilitadas para solicitações específicas em um método de manipulador de Páginas Razor ou em um controlador do MVC. Para desabilitar as páginas de código de status, tente recuperar o [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) da coleção [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) da solicitação e desabilitar o recurso se ele estiver disponível:

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

Se você está usando uma sobrecarga `UseStatusCodePages*` que aponta para um ponto de extremidade dentro do aplicativo, crie um modo de exibição do MVC ou Razor Page para o ponto de extremidade. Por exemplo, o modelo [dotnet novo](/dotnet/core/tools/dotnet-new) para um aplicativo Razor Pages produz a seguinte página e classe de modelo de página:

*Error.cshtml*:

```cshtml
@page
@model ErrorModel
@{
    ViewData["Title"] = "Error";
}

<h1 class="text-danger">Error.</h1>
<h2 class="text-danger">An error occurred while processing your request.</h2>

@if (Model.ShowRequestId)
{
    <p>
        <strong>Request ID:</strong> <code>@Model.RequestId</code>
    </p>
}

<h3>Development Mode</h3>
<p>
    Swapping to <strong>Development</strong> environment will display more detailed information about the error that occurred.
</p>
<p>
    <strong>Development environment should not be enabled in deployed applications</strong>, as it can result in sensitive information from exceptions being displayed to end users. For local debugging, development environment can be enabled by setting the <strong>ASPNETCORE_ENVIRONMENT</strong> environment variable to <strong>Development</strong>, and restarting the application.
</p>
```

*Error.cshtml.cs*:

```csharp
public class ErrorModel : PageModel
{
    public string RequestId { get; set; }

    public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);

    [ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, NoStore = true)]
    public void OnGet()
    {
        RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier;
    }
}
```

## <a name="exception-handling-code"></a>Código de tratamento de exceção

Código em páginas de tratamento de exceção pode gerar exceções. Geralmente, é uma boa ideia que páginas de erro de produção sejam compostas por conteúdo puramente estático.

Além disso, esteja ciente de que, depois que os cabeçalhos de uma resposta forem enviados, você não poderá alterar o código de status da resposta e nenhuma página de exceção ou manipulador poderá ser executado. A resposta deve ser concluída ou a conexão será anulada.

## <a name="server-exception-handling"></a>Tratamento de exceções do servidor

Além da lógica de tratamento de exceção no aplicativo, o [servidor](xref:fundamentals/servers/index) que hospeda o aplicativo executa uma parte do tratamento de exceção. Se o servidor capturar uma exceção antes que os cabeçalhos sejam enviados, o servidor enviará uma resposta *500 Erro Interno do Servidor* sem corpo. Se o servidor capturar uma exceção depois que os cabeçalhos forem enviados, o servidor fechará a conexão. As solicitações que não são manipuladas pelo aplicativo são manipuladas pelo servidor. Qualquer exceção ocorrida é tratada pelo tratamento de exceção do servidor. As páginas de erro personalizadas, o middleware de tratamento de exceção ou os filtros configurados não afetam esse comportamento.

## <a name="startup-exception-handling"></a>Tratamento de exceção na inicialização

Apenas a camada de hospedagem pode tratar exceções que ocorrem durante a inicialização do aplicativo. Usando o [Host da Web](xref:fundamentals/host/web-host), você pode [configurar como o host se comporta em resposta a erros durante a inicialização](xref:fundamentals/host/web-host#detailed-errors) usando as chaves `captureStartupErrors` e `detailedErrors`.

A hospedagem apenas poderá mostrar uma página de erro para um erro de inicialização capturado se o erro ocorrer após a associação de endereço do host/porta. Se alguma associação falhar por algum motivo, a camada de hospedagem registrará uma exceção crítica em log, o processo do dotnet falhará e nenhuma página de erro será exibida quando o aplicativo estiver sendo executado no servidor [Kestrel](xref:fundamentals/servers/kestrel).

Quando executado no [IIS](/iis) ou no [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), um *502.5 Falha no Processo* será retornado pelo [Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) se o processo não puder ser iniciado. Siga os avisos de solução de problemas no tópico [Solucionar problemas do ASP.NET Core no IIS](xref:host-and-deploy/iis/troubleshoot).

## <a name="aspnet-mvc-error-handling"></a>Tratamento de erro do ASP.NET MVC

Os aplicativos [MVC](xref:mvc/overview) contêm algumas opções adicionais para o tratamento de erros, como configurar filtros de exceção e executar a validação do modelo.

### <a name="exception-filters"></a>Filtros de exceção

Os filtros de exceção podem ser configurados globalmente ou por controlador ou por ação em um aplicativo MVC. Esses filtros tratam qualquer exceção sem tratamento ocorrida durante a execução de uma ação do controlador ou de outro filtro e, caso contrário, não são chamadas. Saiba mais sobre filtros de exceção em [Filtros](xref:mvc/controllers/filters).

> [!TIP]
> Filtros de exceção são bons para interceptar exceções que ocorrem em ações do MVC, mas não são tão flexíveis quanto o middleware de tratamento de erro. Dê preferência ao uso de middleware para o caso geral e use filtros apenas quando precisar fazer o tratamento de erro de modo *diferente*, dependendo da ação do MVC escolhida.

### <a name="handling-model-state-errors"></a>Tratando erros do estado do modelo

A [validação do modelo](xref:mvc/models/validation) ocorre antes da invocação de cada ação do controlador e é responsabilidade do método de ação inspecionar `ModelState.IsValid` e responder de forma adequada.

Alguns aplicativos optarão por seguir uma convenção padrão para lidar com erros de validação do modelo, caso em que um [filtro](xref:mvc/controllers/filters) pode ser um local adequado para implementar uma política como essa. É necessário testar o comportamento das ações com estados de modelo inválidos. Saiba mais em [Testar a lógica do controlador](xref:mvc/controllers/testing).
