---
title: Considerações de segurança no SignalR do ASP.NET Core
author: tdykstra
description: Saiba como usar a autenticação e autorização no SignalR do ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 06/29/2018
uid: signalr/security
ms.openlocfilehash: b66c7fbfbaee4c70a68f3132875fbc81018c3e20
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095126"
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
