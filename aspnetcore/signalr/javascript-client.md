---
title: Cliente do ASP.NET Core SignalR JavaScript
author: rachelappel
description: Visão geral do cliente do ASP.NET Core SignalR JavaScript.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/29/2018
uid: signalr/javascript-client
ms.openlocfilehash: 9ea12dc21abfef6d2e6f3fcc455d8aa58ecaa011
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36271938"
---
# <a name="aspnet-core-signalr-javascript-client"></a><span data-ttu-id="e6df6-103">Cliente do ASP.NET Core SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="e6df6-103">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="e6df6-104">Por [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="e6df6-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="e6df6-105">A biblioteca de cliente ASP.NET Core SignalR JavaScript permite que os desenvolvedores chamar o código de hub do lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="e6df6-105">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="e6df6-106">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e6df6-106">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-client-package"></a><span data-ttu-id="e6df6-107">Instale o pacote de cliente SignalR</span><span class="sxs-lookup"><span data-stu-id="e6df6-107">Install the SignalR client package</span></span>

<span data-ttu-id="e6df6-108">A biblioteca de cliente SignalR JavaScript é entregue como um [npm](https://www.npmjs.com/) pacote.</span><span class="sxs-lookup"><span data-stu-id="e6df6-108">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="e6df6-109">Se você estiver usando o Visual Studio, execute `npm install` do **Package Manager Console** enquanto estiver na pasta raiz.</span><span class="sxs-lookup"><span data-stu-id="e6df6-109">If you're using Visual Studio, run `npm install` from the **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="e6df6-110">Para o código do Visual Studio, execute o comando a partir de **Terminal integrada**.</span><span class="sxs-lookup"><span data-stu-id="e6df6-110">For Visual Studio Code, run the command from the **Integrated Terminal**.</span></span>

  ```console
   npm init -y
   npm install @aspnet/signalr
  ```

<span data-ttu-id="e6df6-111">NPM instala o conteúdo do pacote no *node_modules\\ @aspnet\signalr\dist\browser*  pasta.</span><span class="sxs-lookup"><span data-stu-id="e6df6-111">Npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="e6df6-112">Criar uma nova pasta chamada *signalr* sob o *wwwroot\\lib* pasta.</span><span class="sxs-lookup"><span data-stu-id="e6df6-112">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="e6df6-113">Copie o *signalr.js* o arquivo para o *wwwroot\lib\signalr* pasta.</span><span class="sxs-lookup"><span data-stu-id="e6df6-113">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

## <a name="use-the-signalr-javascript-client"></a><span data-ttu-id="e6df6-114">Usar o cliente SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="e6df6-114">Use the SignalR JavaScript client</span></span>

<span data-ttu-id="e6df6-115">Referência do cliente SignalR JavaScript a `<script>` elemento.</span><span class="sxs-lookup"><span data-stu-id="e6df6-115">Reference the SignalR JavaScript client in the `<script>` element.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="e6df6-116">Conectar a um hub</span><span class="sxs-lookup"><span data-stu-id="e6df6-116">Connect to a hub</span></span>

<span data-ttu-id="e6df6-117">O código a seguir cria e inicia uma conexão.</span><span class="sxs-lookup"><span data-stu-id="e6df6-117">The following code creates and starts a connection.</span></span> <span data-ttu-id="e6df6-118">Nome do hub diferencia maiusculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="e6df6-118">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-12,28)]

### <a name="cross-origin-connections"></a><span data-ttu-id="e6df6-119">Conexões entre origens</span><span class="sxs-lookup"><span data-stu-id="e6df6-119">Cross-origin connections</span></span>

<span data-ttu-id="e6df6-120">Normalmente, navegadores carregar conexões do mesmo domínio que a página solicitada.</span><span class="sxs-lookup"><span data-stu-id="e6df6-120">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="e6df6-121">No entanto, há ocasiões em que é necessária uma conexão para outro domínio.</span><span class="sxs-lookup"><span data-stu-id="e6df6-121">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="e6df6-122">Para impedir que um site mal-intencionado lendo dados confidenciais de outro site, [conexões entre origens](xref:security/cors) estão desabilitados por padrão.</span><span class="sxs-lookup"><span data-stu-id="e6df6-122">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="e6df6-123">Para permitir que uma solicitação entre origens, habilitá-lo na `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="e6df6-123">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="e6df6-124">Chamar métodos de hub do cliente</span><span class="sxs-lookup"><span data-stu-id="e6df6-124">Call hub methods from client</span></span>

<span data-ttu-id="e6df6-125">Os clientes JavaScript chamar métodos públicos em hubs por usando `connection.invoke`.</span><span class="sxs-lookup"><span data-stu-id="e6df6-125">JavaScript clients call public methods on hubs by using `connection.invoke`.</span></span> <span data-ttu-id="e6df6-126">O `invoke` método aceita dois argumentos:</span><span class="sxs-lookup"><span data-stu-id="e6df6-126">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="e6df6-127">O nome do método de hub.</span><span class="sxs-lookup"><span data-stu-id="e6df6-127">The name of the hub method.</span></span> <span data-ttu-id="e6df6-128">O exemplo a seguir, o nome de hub é `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="e6df6-128">In the following example, the hub name is `SendMessage`.</span></span>
* <span data-ttu-id="e6df6-129">Nenhum argumento definido no método de hub.</span><span class="sxs-lookup"><span data-stu-id="e6df6-129">Any arguments defined in the hub method.</span></span> <span data-ttu-id="e6df6-130">No exemplo a seguir, é o nome do argumento `message`.</span><span class="sxs-lookup"><span data-stu-id="e6df6-130">In the following example, the argument name is `message`.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="e6df6-131">Chamar métodos de cliente do hub</span><span class="sxs-lookup"><span data-stu-id="e6df6-131">Call client methods from hub</span></span>

<span data-ttu-id="e6df6-132">Para receber mensagens do hub, definir um método usando o `connection.on` método.</span><span class="sxs-lookup"><span data-stu-id="e6df6-132">To receive messages from the hub, define a method using the `connection.on` method.</span></span>

* <span data-ttu-id="e6df6-133">O nome do método de cliente JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e6df6-133">The name of the JavaScript client method.</span></span> <span data-ttu-id="e6df6-134">No exemplo a seguir, é o nome do método `ReceiveMessage`.</span><span class="sxs-lookup"><span data-stu-id="e6df6-134">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="e6df6-135">O hub passa para o método de argumentos.</span><span class="sxs-lookup"><span data-stu-id="e6df6-135">Arguments the hub passes to the method.</span></span> <span data-ttu-id="e6df6-136">No exemplo a seguir, o valor do argumento é `message`.</span><span class="sxs-lookup"><span data-stu-id="e6df6-136">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

<span data-ttu-id="e6df6-137">O código anterior no `connection.on` é executado quando o código do lado do servidor chamá-lo usando o `SendAsync` método.</span><span class="sxs-lookup"><span data-stu-id="e6df6-137">The preceding code in `connection.on` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

<span data-ttu-id="e6df6-138">SignalR determina qual método de cliente para chamar correspondendo o nome do método e argumentos definidos no `SendAsync` e `connection.on`.</span><span class="sxs-lookup"><span data-stu-id="e6df6-138">SignalR determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="e6df6-139">Como prática recomendada, chame `connection.start` depois `connection.on` para seus manipuladores são registrados antes de todas as mensagens são recebidas.</span><span class="sxs-lookup"><span data-stu-id="e6df6-139">As a best practice, call `connection.start` after `connection.on` so your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="e6df6-140">Registro em log e tratamento de erros</span><span class="sxs-lookup"><span data-stu-id="e6df6-140">Error handling and logging</span></span>

<span data-ttu-id="e6df6-141">Cadeia de um `catch` método a fim do `connection.start` método para tratar erros de cliente.</span><span class="sxs-lookup"><span data-stu-id="e6df6-141">Chain a `catch` method to the end of the `connection.start` method to handle client-side errors.</span></span> <span data-ttu-id="e6df6-142">Use `console.error` para erros de saída para o console do navegador.</span><span class="sxs-lookup"><span data-stu-id="e6df6-142">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=28)]

<span data-ttu-id="e6df6-143">Configure o rastreamento de log do lado do cliente, passando um agente de log e o tipo de evento em log quando a conexão é feita.</span><span class="sxs-lookup"><span data-stu-id="e6df6-143">Setup client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="e6df6-144">Mensagens são registradas com o nível de log especificado e superior.</span><span class="sxs-lookup"><span data-stu-id="e6df6-144">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="e6df6-145">Níveis de log disponíveis são os seguintes:</span><span class="sxs-lookup"><span data-stu-id="e6df6-145">Available log levels are as follows:</span></span>

* <span data-ttu-id="e6df6-146">`signalR.LogLevel.Error` : Mensagens de erro.</span><span class="sxs-lookup"><span data-stu-id="e6df6-146">`signalR.LogLevel.Error` : Error messages.</span></span> <span data-ttu-id="e6df6-147">Logs de `Error` mensagens somente.</span><span class="sxs-lookup"><span data-stu-id="e6df6-147">Logs `Error` messages only.</span></span>
* <span data-ttu-id="e6df6-148">`signalR.LogLevel.Warning` : Mensagens de aviso sobre erros em potencial.</span><span class="sxs-lookup"><span data-stu-id="e6df6-148">`signalR.LogLevel.Warning` : Warning messages about potential errors.</span></span> <span data-ttu-id="e6df6-149">Logs de `Warning`, e `Error` mensagens.</span><span class="sxs-lookup"><span data-stu-id="e6df6-149">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="e6df6-150">`signalR.LogLevel.Information` : Mensagens de status sem erros.</span><span class="sxs-lookup"><span data-stu-id="e6df6-150">`signalR.LogLevel.Information` : Status messages without errors.</span></span> <span data-ttu-id="e6df6-151">Logs de `Information`, `Warning`, e `Error` mensagens.</span><span class="sxs-lookup"><span data-stu-id="e6df6-151">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="e6df6-152">`signalR.LogLevel.Trace` : Mensagens de rastreamento.</span><span class="sxs-lookup"><span data-stu-id="e6df6-152">`signalR.LogLevel.Trace` : Trace messages.</span></span> <span data-ttu-id="e6df6-153">Registra tudo, incluindo dados transportados entre cliente e hub.</span><span class="sxs-lookup"><span data-stu-id="e6df6-153">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="e6df6-154">Use o `configureLogging` método `HubConnectionBuilder` para configurar o nível de log.</span><span class="sxs-lookup"><span data-stu-id="e6df6-154">Use the `configureLogging` method on `HubConnectionBuilder` to configure the log level.</span></span> <span data-ttu-id="e6df6-155">Mensagens são registradas para o console do navegador.</span><span class="sxs-lookup"><span data-stu-id="e6df6-155">Messages are logged to the browser console.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="related-resources"></a><span data-ttu-id="e6df6-156">Recursos relacionados</span><span class="sxs-lookup"><span data-stu-id="e6df6-156">Related resources</span></span>

* [<span data-ttu-id="e6df6-157">Hubs</span><span class="sxs-lookup"><span data-stu-id="e6df6-157">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="e6df6-158">Cliente .NET</span><span class="sxs-lookup"><span data-stu-id="e6df6-158">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="e6df6-159">Publicar no Azure</span><span class="sxs-lookup"><span data-stu-id="e6df6-159">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="e6df6-160">Habilitar solicitações entre origens (CORS) no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e6df6-160">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>](xref:security/cors)