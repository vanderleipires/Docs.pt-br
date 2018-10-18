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
# <a name="security-considerations-in-aspnet-core-signalr"></a>Considerações de segurança no SignalR do ASP.NET Core

Por [Andrew Stanton-Nurse](https://twitter.com/anurse)

## <a name="overview"></a>Visão geral

O SignalR fornece um número de proteções de segurança por padrão. É importante entender como configurar essas proteções.

### <a name="cross-origin-resource-sharing"></a>Compartilhamento de recursos entre origens

[Recursos entre origens (CORS) compartilhamento](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) pode ser usado para permitir conexões do SignalR entre origens no navegador. Se seu código JavaScript é hospedado em um nome de domínio diferente do seu aplicativo do SignalR, você precisa habilitar o [middleware do ASP.NET Core CORS](xref:security/cors) para permitir que a conexão. Em geral, permitir que solicitações entre origens somente de domínios controlados por você. Por exemplo, se seu site estiver hospedado em `http://www.example.com` e seu aplicativo SignalR está hospedado no `http://signalr.example.com`, você deve configurar o CORS em seu aplicativo do SignalR para permitir somente a origem `www.example.com`.

Para obter mais informações sobre como configurar o CORS, consulte [a documentação sobre o ASP.NET Core CORS](xref:security/cors). O SignalR requer as seguintes políticas CORS para operar corretamente:

* A política deve permitir as origens específicas que você espera ou permitir qualquer origem (não recomendada).
* Métodos HTTP `GET` e `POST` devem ser permitidos.
* Credenciais devem ser habilitadas, mesmo quando você não estiver usando a autenticação.

Por exemplo, a seguinte política CORS permite que um cliente de navegador do SignalR hospedado em `http://example.com` para acessar seu aplicativo do SignalR:

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
> O SignalR não é compatível com o recurso interno de CORS no serviço de aplicativo do Azure.

### <a name="websocket-origin-restriction"></a>Restrição de origem do WebSocket

As proteções fornecidas pelo CORS não se aplicam aos WebSockets. Navegadores não executam solicitações de simulação CORS, nem respeitam as restrições especificadas no `Access-Control` cabeçalhos ao fazer solicitações de WebSocket. No entanto, os navegadores enviam a `Origin` cabeçalho ao emitir solicitações de WebSocket. Você deve configurar seu aplicativo para validar esses cabeçalhos a fim de garantir que apenas WebSockets provenientes das origens, você espera que são permitidos.

No ASP.NET Core 2.1, isso pode ser feito usando um middleware personalizado, você pode colocar **acima `UseSignalR`e qualquer middleware de autenticação** em seu `Configure` método:

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
> O `Origin` cabeçalho é totalmente controlado pelo cliente e, como o `Referer` cabeçalho, podem ser falsificadas. Esses cabeçalhos nunca devem ser usados como um mecanismo de autenticação.

### <a name="access-token-logging"></a>Log de token de acesso

Ao usar WebSockets ou Server-Sent eventos, o cliente de navegador envia o token de acesso na cadeia de caracteres de consulta. Isso é geralmente tão seguro quanto usar o padrão `Authorization` cabeçalho, no entanto, a URL para cada solicitação de log de muitos servidores web, incluindo a cadeia de caracteres de consulta. Isso significa que o token de acesso pode ser incluído nos logs. Considere a possibilidade de revisar as configurações de registro em log do servidor web para evitar essas informações de registro em log.

### <a name="exceptions"></a>Exceções

Mensagens de exceção são geralmente consideradas dados confidenciais que não devem ser revelados para um cliente. Por padrão, o SignalR não envia os detalhes de uma exceção gerada por um método de hub para o cliente. Em vez disso, o cliente recebe uma mensagem genérica indicando que ocorreu um erro. Você pode substituir esse comportamento, definindo a [ `EnableDetailedErrors` ](xref:signalr/configuration#configure-server-options) configuração.

### <a name="buffer-management"></a>Gerenciamento de buffer

O SignalR usa buffers por conexão para gerenciar mensagens de entrada e saídas. Por padrão, o SignalR limita esses buffers para 32KB. Isso significa que a mensagem de possíveis maior que um cliente ou servidor pode enviar é 32KB. Isso também significa que a quantidade máxima de memória consumida por uma conexão de mensagens é 32KB. Se você souber que as mensagens são sempre menores do que esse limite, você pode reduzir esse tamanho a fim de impedir que um cliente que está sendo capaz de enviar uma mensagem maior e forçar o servidor para alocar memória para aceitá-lo. Da mesma forma, se você souber que as mensagens são maiores do que esse limite, você pode aumentá-lo. No entanto, lembre-se de que aumentar esse limite, significa que o cliente é capaz de fazer com que o servidor para alocar memória adicional e pode reduzir o número de conexões simultâneas que seu aplicativo pode manipular.

Há limites separados para mensagens de entrada e saídas, ambos podem ser configuradas na [ `HttpConnectionDispatcherOptions` ](xref:signalr/configuration#configure-server-options) objeto configurado no `MapHub`:

* `ApplicationMaxBufferSize` representa o número máximo de bytes do cliente que os buffers de servidor. Se o cliente tenta enviar uma mensagem maior que esse limite, a conexão poderá ser fechada.
* `TransportMaxBufferSize` representa o número máximo de bytes que o servidor pode enviar. Se o servidor tenta enviar uma mensagem (inclui valores de retorno de métodos de hub) maior do que esse limite, uma exceção será gerada.

Definir o limite como `0` desabilita o limite inteiramente. No entanto, isso deve ser feito com muito cuidado. Remover o limite permite que um cliente enviar uma mensagem de qualquer tamanho. Isso pode ser usado por um cliente mal-intencionado para fazer com que a memória em excesso ser alocado, que pode reduzir drasticamente o número de conexões simultâneas que pode dar suporte a seu aplicativo.
