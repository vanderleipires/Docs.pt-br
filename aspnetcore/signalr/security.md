---
title: Considerações de segurança no SignalR do ASP.NET Core
author: tdykstra
description: Saiba como usar a autenticação e autorização no SignalR do ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 06/29/2018
uid: signalr/security
ms.openlocfilehash: 98b5eb7be87920aacf7a941f76ff652ae7905303
ms.sourcegitcommit: f43f430a166a7ec137fcad12ded0372747227498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49391252"
---
# <a name="security-considerations-in-aspnet-core-signalr"></a><span data-ttu-id="8cbdf-103">Considerações de segurança no SignalR do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8cbdf-103">Security considerations in ASP.NET Core SignalR</span></span>

<span data-ttu-id="8cbdf-104">Por [Andrew Stanton-Nurse](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="8cbdf-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

## <a name="overview"></a><span data-ttu-id="8cbdf-105">Visão geral</span><span class="sxs-lookup"><span data-stu-id="8cbdf-105">Overview</span></span>

<span data-ttu-id="8cbdf-106">O SignalR fornece um número de proteções de segurança por padrão.</span><span class="sxs-lookup"><span data-stu-id="8cbdf-106">SignalR provides a number of security protections by default.</span></span> <span data-ttu-id="8cbdf-107">É importante entender como configurar essas proteções.</span><span class="sxs-lookup"><span data-stu-id="8cbdf-107">It's important to understand how to configure these protections.</span></span>

### <a name="cross-origin-resource-sharing"></a><span data-ttu-id="8cbdf-108">Compartilhamento de recursos entre origens</span><span class="sxs-lookup"><span data-stu-id="8cbdf-108">Cross-origin resource sharing</span></span>

<span data-ttu-id="8cbdf-109">[Recursos entre origens (CORS) compartilhamento](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) pode ser usado para permitir conexões do SignalR entre origens no navegador.</span><span class="sxs-lookup"><span data-stu-id="8cbdf-109">[Cross-origin resource sharing (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) can be used to allow cross-origin SignalR connections in the browser.</span></span> <span data-ttu-id="8cbdf-110">Se seu código JavaScript é hospedado em um nome de domínio diferente do seu aplicativo do SignalR, você precisa habilitar o [middleware do ASP.NET Core CORS](xref:security/cors) para permitir que a conexão.</span><span class="sxs-lookup"><span data-stu-id="8cbdf-110">If your JavaScript code is hosted on a different domain name from your SignalR app, you have to enable the [ASP.NET Core CORS middleware](xref:security/cors) in order to allow the connection.</span></span> <span data-ttu-id="8cbdf-111">Em geral, permitir que solicitações entre origens somente de domínios controlados por você.</span><span class="sxs-lookup"><span data-stu-id="8cbdf-111">In general, allow cross-origin requests only from domains you control.</span></span> <span data-ttu-id="8cbdf-112">Por exemplo, se seu site estiver hospedado em `http://www.example.com` e seu aplicativo SignalR está hospedado no `http://signalr.example.com`, você deve configurar o CORS em seu aplicativo do SignalR para permitir somente a origem `www.example.com`.</span><span class="sxs-lookup"><span data-stu-id="8cbdf-112">For example, if your site is hosted on `http://www.example.com` and your SignalR app is hosted on `http://signalr.example.com`, you should configure CORS in your SignalR app to only allow the origin `www.example.com`.</span></span>

<span data-ttu-id="8cbdf-113">Para obter mais informações sobre como configurar o CORS, consulte [a documentação sobre o ASP.NET Core CORS](xref:security/cors).</span><span class="sxs-lookup"><span data-stu-id="8cbdf-113">For more information on configuring CORS, see [the documentation on ASP.NET Core CORS](xref:security/cors).</span></span> <span data-ttu-id="8cbdf-114">O SignalR requer as seguintes políticas CORS para operar corretamente:</span><span class="sxs-lookup"><span data-stu-id="8cbdf-114">SignalR requires the following CORS policies in order to operate correctly:</span></span>

* <span data-ttu-id="8cbdf-115">A política deve permitir as origens específicas que você espera ou permitir qualquer origem (não recomendada).</span><span class="sxs-lookup"><span data-stu-id="8cbdf-115">The policy must allow the specific origins you expect, or allow any origin (not recommended).</span></span>
* <span data-ttu-id="8cbdf-116">Métodos HTTP `GET` e `POST` devem ser permitidos.</span><span class="sxs-lookup"><span data-stu-id="8cbdf-116">HTTP methods `GET` and `POST` must be allowed.</span></span>
* <span data-ttu-id="8cbdf-117">Credenciais devem ser habilitadas, mesmo quando você não estiver usando a autenticação.</span><span class="sxs-lookup"><span data-stu-id="8cbdf-117">Credentials must be enabled, even when you aren't using authentication.</span></span>

<span data-ttu-id="8cbdf-118">Por exemplo, a seguinte política CORS permite que um cliente de navegador do SignalR hospedado em `http://example.com` para acessar seu aplicativo do SignalR:</span><span class="sxs-lookup"><span data-stu-id="8cbdf-118">For example, the following CORS policy allows a SignalR browser client hosted on `http://example.com` to access your SignalR app:</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    // ... other middleware ...

    // Make sure the CORS middleware is ahead of SignalR.
    app.UseCors(builder => {
        builder.WithOrigins("http://example.com")
            .AllowAnyHeader()
            .WithMethods("GET", "POST")
            .AllowCredentials();
    });

    // ... other middleware ...

    app.UseSignalR();

    // ... other middleware ...
}
```

> [!NOTE]
> <span data-ttu-id="8cbdf-119">O SignalR não é compatível com o recurso interno de CORS no serviço de aplicativo do Azure.</span><span class="sxs-lookup"><span data-stu-id="8cbdf-119">SignalR is not compatible with the built-in CORS feature in Azure App Service.</span></span>

### <a name="websocket-origin-restriction"></a><span data-ttu-id="8cbdf-120">Restrição de origem do WebSocket</span><span class="sxs-lookup"><span data-stu-id="8cbdf-120">WebSocket Origin Restriction</span></span>

<span data-ttu-id="8cbdf-121">As proteções fornecidas pelo CORS não se aplicam aos WebSockets.</span><span class="sxs-lookup"><span data-stu-id="8cbdf-121">The protections provided by CORS do not apply to WebSockets.</span></span> <span data-ttu-id="8cbdf-122">Navegadores não executam solicitações de simulação CORS, nem respeitam as restrições especificadas no `Access-Control` cabeçalhos ao fazer solicitações de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="8cbdf-122">Browsers do not perform CORS pre-flight requests, nor do they respect the restrictions specified in `Access-Control` headers when making WebSocket requests.</span></span> <span data-ttu-id="8cbdf-123">No entanto, os navegadores enviam a `Origin` cabeçalho ao emitir solicitações de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="8cbdf-123">However, browsers do send the `Origin` header when issuing WebSocket requests.</span></span> <span data-ttu-id="8cbdf-124">Você deve configurar seu aplicativo para validar esses cabeçalhos a fim de garantir que apenas WebSockets provenientes das origens, você espera que são permitidos.</span><span class="sxs-lookup"><span data-stu-id="8cbdf-124">You should configure your application to validate these headers in order to ensure that only WebSockets coming from the origins you expect are allowed.</span></span>

<span data-ttu-id="8cbdf-125">No ASP.NET Core 2.1, isso pode ser feito usando um middleware personalizado, você pode colocar **acima `UseSignalR`e qualquer middleware de autenticação** em seu `Configure` método:</span><span class="sxs-lookup"><span data-stu-id="8cbdf-125">In ASP.NET Core 2.1, this can be achieved using a custom middleware you can place **above `UseSignalR`, and any authentication middleware** in your `Configure` method:</span></span>

```csharp
// In your Startup class, add a static field listing the allowed Origin values:
private static readonly HashSet<string> _allowedOrigins = new HashSet<string>()
{
    // Add allowed origins here. For example:
    "http://www.mysite.com",
    "http://mysite.com",
};

// In your Configure method:
public void Configure(IApplicationBuilder app)
{
    // ... other middleware ...

    // Validate Origin header on WebSocket requests to prevent unexpected cross-site WebSocket requests
    app.Use((context, next) =>
    {
        // Check for a WebSocket request.
        if(string.Equals(context.Request.Headers["Upgrade"], "websocket"))
        {
            var origin = context.Request.Headers["Origin"];

            // If there is no origin header, or if the origin header doesn't match an allowed value:
            if(string.IsNullOrEmpty(origin) && !_allowedOrigins.Contains(origin))
            {
                // The origin is not allowed, reject the request
                context.Response.StatusCode = StatusCodes.Status400BadRequest;
                return Task.CompletedTask;
            }
        }

        // The request is not a WebSocket request or is a valid Origin, so let it continue
        return next();
    });

    // ... other middleware ...

    app.UseSignalR();

    // ... other middleware ...
}
```

> [!NOTE]
> <span data-ttu-id="8cbdf-126">O `Origin` cabeçalho é totalmente controlado pelo cliente e, como o `Referer` cabeçalho, podem ser falsificadas.</span><span class="sxs-lookup"><span data-stu-id="8cbdf-126">The `Origin` header is completely controlled by the client and, like the `Referer` header, can be faked.</span></span> <span data-ttu-id="8cbdf-127">Esses cabeçalhos nunca devem ser usados como um mecanismo de autenticação.</span><span class="sxs-lookup"><span data-stu-id="8cbdf-127">These headers should never be used as an authentication mechanism.</span></span>

### <a name="access-token-logging"></a><span data-ttu-id="8cbdf-128">Log de token de acesso</span><span class="sxs-lookup"><span data-stu-id="8cbdf-128">Access token logging</span></span>

<span data-ttu-id="8cbdf-129">Ao usar WebSockets ou Server-Sent eventos, o cliente de navegador envia o token de acesso na cadeia de caracteres de consulta.</span><span class="sxs-lookup"><span data-stu-id="8cbdf-129">When using WebSockets or Server-Sent Events, the browser client sends the access token in the query string.</span></span> <span data-ttu-id="8cbdf-130">Isso é geralmente tão seguro quanto usar o padrão `Authorization` cabeçalho, no entanto, a URL para cada solicitação de log de muitos servidores web, incluindo a cadeia de caracteres de consulta.</span><span class="sxs-lookup"><span data-stu-id="8cbdf-130">This is generally as secure as using the standard `Authorization` header, however many web servers log the URL for each request, including the query string.</span></span> <span data-ttu-id="8cbdf-131">Isso significa que o token de acesso pode ser incluído nos logs.</span><span class="sxs-lookup"><span data-stu-id="8cbdf-131">This means the access token may be included in logs.</span></span> <span data-ttu-id="8cbdf-132">Considere a possibilidade de revisar as configurações de registro em log do servidor web para evitar essas informações de registro em log.</span><span class="sxs-lookup"><span data-stu-id="8cbdf-132">Consider reviewing the web server's logging settings to avoid logging this information.</span></span>

### <a name="exceptions"></a><span data-ttu-id="8cbdf-133">Exceções</span><span class="sxs-lookup"><span data-stu-id="8cbdf-133">Exceptions</span></span>

<span data-ttu-id="8cbdf-134">Mensagens de exceção são geralmente consideradas dados confidenciais que não devem ser revelados para um cliente.</span><span class="sxs-lookup"><span data-stu-id="8cbdf-134">Exception messages are generally considered sensitive data that shouldn't be revealed to a client.</span></span> <span data-ttu-id="8cbdf-135">Por padrão, o SignalR não envia os detalhes de uma exceção gerada por um método de hub para o cliente.</span><span class="sxs-lookup"><span data-stu-id="8cbdf-135">By default, SignalR doesn't send the details of an exception thrown by a hub method to the client.</span></span> <span data-ttu-id="8cbdf-136">Em vez disso, o cliente recebe uma mensagem genérica indicando que ocorreu um erro.</span><span class="sxs-lookup"><span data-stu-id="8cbdf-136">Instead, the client receives a generic message indicating an error occurred.</span></span> <span data-ttu-id="8cbdf-137">Você pode substituir esse comportamento, definindo a [ `EnableDetailedErrors` ](xref:signalr/configuration#configure-server-options) configuração.</span><span class="sxs-lookup"><span data-stu-id="8cbdf-137">You can override this behavior by setting the [`EnableDetailedErrors`](xref:signalr/configuration#configure-server-options) setting.</span></span>

### <a name="buffer-management"></a><span data-ttu-id="8cbdf-138">Gerenciamento de buffer</span><span class="sxs-lookup"><span data-stu-id="8cbdf-138">Buffer management</span></span>

<span data-ttu-id="8cbdf-139">O SignalR usa buffers por conexão para gerenciar mensagens de entrada e saídas.</span><span class="sxs-lookup"><span data-stu-id="8cbdf-139">SignalR uses per-connection buffers in order to manage incoming and outgoing messages.</span></span> <span data-ttu-id="8cbdf-140">Por padrão, o SignalR limita esses buffers para 32KB.</span><span class="sxs-lookup"><span data-stu-id="8cbdf-140">By default, SignalR limits these buffers to 32KB.</span></span> <span data-ttu-id="8cbdf-141">Isso significa que a mensagem de possíveis maior que um cliente ou servidor pode enviar é 32KB.</span><span class="sxs-lookup"><span data-stu-id="8cbdf-141">This means the largest possible message a client or server can send is 32KB.</span></span> <span data-ttu-id="8cbdf-142">Isso também significa que a quantidade máxima de memória consumida por uma conexão de mensagens é 32KB.</span><span class="sxs-lookup"><span data-stu-id="8cbdf-142">This also means the maximum amount of memory consumed by a connection for messages is 32KB.</span></span> <span data-ttu-id="8cbdf-143">Se você souber que as mensagens são sempre menores do que esse limite, você pode reduzir esse tamanho a fim de impedir que um cliente que está sendo capaz de enviar uma mensagem maior e forçar o servidor para alocar memória para aceitá-lo.</span><span class="sxs-lookup"><span data-stu-id="8cbdf-143">If you know your messages are always smaller than this limit, you can reduce this size to prevent a client from being able to send a larger message and force the server to allocate memory to accept it.</span></span> <span data-ttu-id="8cbdf-144">Da mesma forma, se você souber que as mensagens são maiores do que esse limite, você pode aumentá-lo.</span><span class="sxs-lookup"><span data-stu-id="8cbdf-144">Similarly, if you know your messages are larger than this limit, you can increase it.</span></span> <span data-ttu-id="8cbdf-145">No entanto, lembre-se de que aumentar esse limite, significa que o cliente é capaz de fazer com que o servidor para alocar memória adicional e pode reduzir o número de conexões simultâneas que seu aplicativo pode manipular.</span><span class="sxs-lookup"><span data-stu-id="8cbdf-145">However, be aware that increasing this limit means that the client is able to cause the server to allocate additional memory and may reduce the number of concurrent connections your app can handle.</span></span>

<span data-ttu-id="8cbdf-146">Há limites separados para mensagens de entrada e saídas, ambos podem ser configuradas na [ `HttpConnectionDispatcherOptions` ](xref:signalr/configuration#configure-server-options) objeto configurado no `MapHub`:</span><span class="sxs-lookup"><span data-stu-id="8cbdf-146">There are separate limits for incoming and outgoing messages, both can be configured on the [`HttpConnectionDispatcherOptions`](xref:signalr/configuration#configure-server-options) object configured in `MapHub`:</span></span>

* <span data-ttu-id="8cbdf-147">`ApplicationMaxBufferSize` representa o número máximo de bytes do cliente que os buffers de servidor.</span><span class="sxs-lookup"><span data-stu-id="8cbdf-147">`ApplicationMaxBufferSize` represents the maximum number of bytes from the client that the server buffers.</span></span> <span data-ttu-id="8cbdf-148">Se o cliente tenta enviar uma mensagem maior que esse limite, a conexão poderá ser fechada.</span><span class="sxs-lookup"><span data-stu-id="8cbdf-148">If the client attempts to send a message larger than this limit, the connection may be closed.</span></span>
* <span data-ttu-id="8cbdf-149">`TransportMaxBufferSize` representa o número máximo de bytes que o servidor pode enviar.</span><span class="sxs-lookup"><span data-stu-id="8cbdf-149">`TransportMaxBufferSize` represents the maximum number of bytes the server can send.</span></span> <span data-ttu-id="8cbdf-150">Se o servidor tenta enviar uma mensagem (inclui valores de retorno de métodos de hub) maior do que esse limite, uma exceção será gerada.</span><span class="sxs-lookup"><span data-stu-id="8cbdf-150">If the server attempts to send a message (includes return values from hub methods) larger than this limit, an exception will be thrown.</span></span>

<span data-ttu-id="8cbdf-151">Definir o limite como `0` desabilita o limite inteiramente.</span><span class="sxs-lookup"><span data-stu-id="8cbdf-151">Setting the limit to `0` disables the limit entirely.</span></span> <span data-ttu-id="8cbdf-152">No entanto, isso deve ser feito com muito cuidado.</span><span class="sxs-lookup"><span data-stu-id="8cbdf-152">However, this should be done with extreme caution.</span></span> <span data-ttu-id="8cbdf-153">Remover o limite permite que um cliente enviar uma mensagem de qualquer tamanho.</span><span class="sxs-lookup"><span data-stu-id="8cbdf-153">Removing the limit allows a client to send a message of any size.</span></span> <span data-ttu-id="8cbdf-154">Isso pode ser usado por um cliente mal-intencionado para fazer com que a memória em excesso ser alocado, que pode reduzir drasticamente o número de conexões simultâneas que pode dar suporte a seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8cbdf-154">This could be used by a malicious client to cause excess memory to be allocated, which could dramatically reduce the number of concurrent connections your app can support.</span></span>
