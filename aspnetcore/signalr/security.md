---
title: Considerações de segurança no SignalR do ASP.NET Core
author: tdykstra
description: Saiba como usar a autenticação e autorização no SignalR do ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 10/17/2018
uid: signalr/security
ms.openlocfilehash: 1adf762cd6de4f0cf62e31c0ec6e595a32ed56f8
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477534"
---
# <a name="security-considerations-in-aspnet-core-signalr"></a>Considerações de segurança no SignalR do ASP.NET Core

Por [Andrew Stanton-Nurse](https://twitter.com/anurse)

Este artigo fornece informações sobre como proteger o SignalR.

## <a name="cross-origin-resource-sharing"></a>Compartilhamento de recursos entre origens

[Recursos entre origens (CORS) compartilhamento](https://www.w3.org/TR/cors/) pode ser usado para permitir conexões do SignalR entre origens no navegador. Se o código JavaScript é hospedado em um domínio diferente do aplicativo do SignalR, [middleware CORS](xref:security/cors) deve estar habilitado para permitir que o JavaScript para se conectar ao aplicativo SignalR. Permitir solicitações entre origens de domínios que confiáveis ou controle. Por exemplo:

* O site é hospedado em `http://www.example.com`
* Seu aplicativo SignalR é hospedado em `http://signalr.example.com`

CORS devem ser configurado no aplicativo do SignalR para permitir somente a origem `www.example.com`.

Para obter mais informações sobre como configurar o CORS, consulte [habilitar solicitações de entre origens (CORS)](xref:security/cors). O SignalR **requer** as seguintes políticas CORS:

* Permitir que as origens esperadas específicas. Permitir qualquer origem é possível, mas está **não** segura ou recomendada.
* Métodos HTTP `GET` e `POST` devem ser permitidos.
* Credenciais devem ser habilitadas, mesmo quando a autenticação não é usada.

Por exemplo, a seguinte política CORS permite que um cliente de navegador do SignalR hospedado no `http://example.com` para acessar o aplicativo de SignalR hospedado em `http://signalr.example.com`:

[!code-csharp[Main](security/sample/Startup.cs?name=snippet1)]

> [!NOTE]
> O SignalR não é compatível com o recurso interno de CORS no serviço de aplicativo do Azure.

## <a name="websocket-origin-restriction"></a>Restrição de origem do WebSocket

As proteções fornecidas pelo CORS não se aplicam ao WebSockets. Navegadores fazem **não**:

* Execute solicitações de simulação CORS.
* Respeitar as restrições especificadas no `Access-Control` cabeçalhos ao fazer solicitações de WebSocket.

No entanto, os navegadores enviam a `Origin` cabeçalho ao emitir solicitações de WebSocket. Aplicativos devem ser configurados para validar esses cabeçalhos para garantir que apenas WebSockets provenientes de origens esperadas são permitidos.

No ASP.NET Core 2.1 e posterior, validação de cabeçalho pode ser obtida usando um middleware personalizado colocado **antes de `UseSignalR`e o middleware de autenticação** em `Configure`:

[!code-csharp[Main](security/sample/Startup.cs?name=snippet2)]

> [!NOTE]
> O `Origin` cabeçalho é controlado pelo cliente e, como o `Referer` cabeçalho, podem ser falsificadas. Esses cabeçalhos devem **não** ser usado como um mecanismo de autenticação.

## <a name="access-token-logging"></a>Log de token de acesso

Ao usar WebSockets ou Server-Sent eventos, o cliente de navegador envia o token de acesso na cadeia de caracteres de consulta. Receber o token de acesso por meio da cadeia de caracteres de consulta é geralmente tão seguro quanto usar o padrão `Authorization` cabeçalho. No entanto, muitos servidores web registrar a URL para cada solicitação, incluindo a cadeia de caracteres de consulta. Registro em log as URLs pode registrar o token de acesso. Uma prática recomendada é definir configurações de registro em log do servidor para impedir que tokens de acesso de registro em log de web.

## <a name="exceptions"></a>Exceções

Mensagens de exceção são geralmente consideradas dados confidenciais que não devem ser revelados para um cliente. Por padrão, o SignalR não envia os detalhes de uma exceção gerada por um método de hub para o cliente. Em vez disso, o cliente recebe uma mensagem genérica indicando que ocorreu um erro. Entrega de mensagem de exceção para o cliente pode ser substituída (por exemplo, no desenvolvimento ou teste) com [ `EnableDetailedErrors` ](xref:signalr/configuration#configure-server-options). Mensagens de exceção não devem ser expostas ao cliente em aplicativos de produção.

## <a name="buffer-management"></a>Gerenciamento de buffer

O SignalR usa buffers por conexão para gerenciar mensagens de entrada e saídas. Por padrão, o SignalR limita esses buffers para 32 KB. A mensagem maior que um cliente ou servidor pode enviar é 32 KB. O máximo de memória consumido por uma conexão de mensagens é 32 KB. Se as mensagens são sempre menores que 32 KB, você pode reduzir o limite, que:

* Impede que um cliente que está sendo capaz de enviar uma mensagem maior.
* O servidor nunca será necessário alocar buffers grandes para aceitar mensagens.

Se suas mensagens forem maiores que 32 KB, você pode aumentar o limite. Aumentar esse limite significa:

* O cliente pode fazer com que o servidor para alocar buffers de memória grandes.
* Alocação de servidor de buffers grandes pode reduzir o número de conexões simultâneas.

Há limites para mensagens de entrada e saídas, ambos podem ser configuradas sobre o [ `HttpConnectionDispatcherOptions` ](xref:signalr/configuration#configure-server-options) objeto configurado no `MapHub`:

* `ApplicationMaxBufferSize` representa o número máximo de bytes do cliente que os buffers de servidor. Se o cliente tenta enviar uma mensagem maior que esse limite, a conexão poderá ser fechada.
* `TransportMaxBufferSize` representa o número máximo de bytes que o servidor pode enviar. Se o servidor tenta enviar uma mensagem (incluindo valores de retorno de métodos de hub) maiores que esse limite, uma exceção será lançada.

Definir o limite como `0` desabilita o limite. Remover o limite permite que um cliente enviar uma mensagem de qualquer tamanho. Os clientes mal-intencionados enviando mensagens extensas podem provocar alocação de memória em excesso. Uso de memória em excesso pode reduzir significativamente o número de conexões simultâneas.
