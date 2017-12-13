---
title: "Recursos de solicitação do ASP.NET Core"
author: ardalis
description: "Saiba mais sobre os detalhes de implementação do servidor web relacionados a solicitações HTTP e respostas que são definidas em interfaces para ASP.NET Core."
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: d1fbd23c-2ff9-4216-b908-0201ff3afb7c
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/request-features
ms.openlocfilehash: b689d82d16c6ef55485691b3474a070765c8144b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="request-features-in-aspnet-core"></a><span data-ttu-id="ee3db-104">Recursos de solicitação do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ee3db-104">Request Features in ASP.NET Core</span></span>

<span data-ttu-id="ee3db-105">Por [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="ee3db-105">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="ee3db-106">Os detalhes de implementação de servidor Web relacionados a solicitações HTTP e respostas são definidos em interfaces.</span><span class="sxs-lookup"><span data-stu-id="ee3db-106">Web server implementation details related to HTTP requests and responses are defined in interfaces.</span></span> <span data-ttu-id="ee3db-107">Essas interfaces são usadas por implementações de servidor e middleware para criar e modificar o pipeline de hospedagem do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ee3db-107">These interfaces are used by server implementations and middleware to create and modify the application's hosting pipeline.</span></span>

## <a name="feature-interfaces"></a><span data-ttu-id="ee3db-108">Interfaces de recurso</span><span class="sxs-lookup"><span data-stu-id="ee3db-108">Feature interfaces</span></span>

<span data-ttu-id="ee3db-109">ASP.NET Core define um número de interfaces de recurso HTTP em `Microsoft.AspNetCore.Http.Features` que são usados pelos servidores para identificar os recursos que oferecem suporte.</span><span class="sxs-lookup"><span data-stu-id="ee3db-109">ASP.NET Core defines a number of HTTP feature interfaces in `Microsoft.AspNetCore.Http.Features` which are used by servers to identify the features they support.</span></span> <span data-ttu-id="ee3db-110">As seguintes interfaces de recurso lidar com solicitações e respostas de retorno:</span><span class="sxs-lookup"><span data-stu-id="ee3db-110">The following feature interfaces handle requests and return responses:</span></span>

<span data-ttu-id="ee3db-111">`IHttpRequestFeature`Define a estrutura de uma solicitação HTTP, incluindo protocolo, caminho, cadeia de caracteres de consulta, cabeçalhos e corpo.</span><span class="sxs-lookup"><span data-stu-id="ee3db-111">`IHttpRequestFeature` Defines the structure of an HTTP request, including the protocol, path, query string, headers, and body.</span></span>

<span data-ttu-id="ee3db-112">`IHttpResponseFeature`Define a estrutura de uma resposta HTTP, incluindo o código de status, cabeçalhos e corpo da resposta.</span><span class="sxs-lookup"><span data-stu-id="ee3db-112">`IHttpResponseFeature` Defines the structure of an HTTP response, including the status code, headers, and body of the response.</span></span>

<span data-ttu-id="ee3db-113">`IHttpAuthenticationFeature`Define o suporte para identificar os usuários com base em um `ClaimsPrincipal` e especificando um manipulador de autenticação.</span><span class="sxs-lookup"><span data-stu-id="ee3db-113">`IHttpAuthenticationFeature` Defines support for identifying users based on a `ClaimsPrincipal` and specifying an authentication handler.</span></span>

<span data-ttu-id="ee3db-114">`IHttpUpgradeFeature`Define o suporte para [HTTP atualizações](https://tools.ietf.org/html/rfc2616.html#section-14.42), que permitem que o cliente especifique quais adicionais protocolos que ele gostaria de usar se o servidor que deseja alternar protocolos.</span><span class="sxs-lookup"><span data-stu-id="ee3db-114">`IHttpUpgradeFeature` Defines support for [HTTP Upgrades](https://tools.ietf.org/html/rfc2616.html#section-14.42), which allow the client to specify which additional protocols it would like to use if the server wishes to switch protocols.</span></span>

<span data-ttu-id="ee3db-115">`IHttpBufferingFeature`Define métodos para desabilitar o buffer de solicitações de e/ou respostas.</span><span class="sxs-lookup"><span data-stu-id="ee3db-115">`IHttpBufferingFeature` Defines methods for disabling buffering of requests and/or responses.</span></span>

<span data-ttu-id="ee3db-116">`IHttpConnectionFeature`Define propriedades para portas e endereços locais e remotos.</span><span class="sxs-lookup"><span data-stu-id="ee3db-116">`IHttpConnectionFeature` Defines properties for local and remote addresses and ports.</span></span>

<span data-ttu-id="ee3db-117">`IHttpRequestLifetimeFeature`Define o suporte para anular conexões ou detectar se uma solicitação foi encerrada prematuramente, tais como por uma desconexão do cliente.</span><span class="sxs-lookup"><span data-stu-id="ee3db-117">`IHttpRequestLifetimeFeature` Defines support for aborting connections, or detecting if a request has been terminated prematurely, such as by a client disconnect.</span></span>

<span data-ttu-id="ee3db-118">`IHttpSendFileFeature`Define um método para enviar arquivos de forma assíncrona.</span><span class="sxs-lookup"><span data-stu-id="ee3db-118">`IHttpSendFileFeature` Defines a method for sending files asynchronously.</span></span>

<span data-ttu-id="ee3db-119">`IHttpWebSocketFeature`Define uma API para soquetes da web de suporte.</span><span class="sxs-lookup"><span data-stu-id="ee3db-119">`IHttpWebSocketFeature` Defines an API for supporting web sockets.</span></span>

<span data-ttu-id="ee3db-120">`IHttpRequestIdentifierFeature`Adiciona uma propriedade que pode ser implementada para identificar exclusivamente as solicitações.</span><span class="sxs-lookup"><span data-stu-id="ee3db-120">`IHttpRequestIdentifierFeature` Adds a property that can be implemented to uniquely identify requests.</span></span>

<span data-ttu-id="ee3db-121">`ISessionFeature`Define `ISessionFactory` e `ISession` abstrações para dar suporte a sessões de usuário.</span><span class="sxs-lookup"><span data-stu-id="ee3db-121">`ISessionFeature` Defines `ISessionFactory` and `ISession` abstractions for supporting user sessions.</span></span>

<span data-ttu-id="ee3db-122">`ITlsConnectionFeature`Define uma API para recuperar os certificados de cliente.</span><span class="sxs-lookup"><span data-stu-id="ee3db-122">`ITlsConnectionFeature` Defines an API for retrieving client certificates.</span></span>

<span data-ttu-id="ee3db-123">`ITlsTokenBindingFeature`Define métodos para trabalhar com parâmetros de token de ligação de TLS.</span><span class="sxs-lookup"><span data-stu-id="ee3db-123">`ITlsTokenBindingFeature` Defines methods for working with TLS token binding parameters.</span></span>

> [!NOTE]
> <span data-ttu-id="ee3db-124">`ISessionFeature`não é um recurso de servidor, mas é implementado pelo `SessionMiddleware` (consulte [Gerenciando o estado do aplicativo](app-state.md)).</span><span class="sxs-lookup"><span data-stu-id="ee3db-124">`ISessionFeature` is not a server feature, but is implemented by the `SessionMiddleware` (see [Managing Application State](app-state.md)).</span></span>

## <a name="feature-collections"></a><span data-ttu-id="ee3db-125">Coleções de recurso</span><span class="sxs-lookup"><span data-stu-id="ee3db-125">Feature collections</span></span>

<span data-ttu-id="ee3db-126">O `Features` propriedade `HttpContext` fornece uma interface para obter e definir os recursos disponíveis de HTTP para a solicitação atual.</span><span class="sxs-lookup"><span data-stu-id="ee3db-126">The `Features` property of `HttpContext` provides an interface for getting and setting the available HTTP features for the current request.</span></span> <span data-ttu-id="ee3db-127">Como a coleção de recurso é mutável, mesmo dentro do contexto de uma solicitação, o middleware pode ser usado para modificar a coleção e adicionar suporte para recursos adicionais.</span><span class="sxs-lookup"><span data-stu-id="ee3db-127">Since the feature collection is mutable even within the context of a request, middleware can be used to modify the collection and add support for additional features.</span></span>

## <a name="middleware-and-request-features"></a><span data-ttu-id="ee3db-128">Recursos de solicitação e de middleware</span><span class="sxs-lookup"><span data-stu-id="ee3db-128">Middleware and request features</span></span>

<span data-ttu-id="ee3db-129">Enquanto os servidores são responsáveis por criar a coleção de recursos, middleware pode adicionar a esta coleção e consomem recursos da coleção.</span><span class="sxs-lookup"><span data-stu-id="ee3db-129">While servers are responsible for creating the feature collection, middleware can both add to this collection and consume features from the collection.</span></span> <span data-ttu-id="ee3db-130">Por exemplo, o `StaticFileMiddleware` acessa o `IHttpSendFileFeature` recurso.</span><span class="sxs-lookup"><span data-stu-id="ee3db-130">For example, the `StaticFileMiddleware` accesses the `IHttpSendFileFeature` feature.</span></span> <span data-ttu-id="ee3db-131">Se o recurso existir, ele é usado para enviar o arquivo estático solicitado de seu caminho físico.</span><span class="sxs-lookup"><span data-stu-id="ee3db-131">If the feature exists, it is used to send the requested static file from its physical path.</span></span> <span data-ttu-id="ee3db-132">Caso contrário, um método alternativo mais lento será usado para enviar o arquivo.</span><span class="sxs-lookup"><span data-stu-id="ee3db-132">Otherwise, a slower alternative method is used to send the file.</span></span> <span data-ttu-id="ee3db-133">Quando disponível, o `IHttpSendFileFeature` permite que o sistema operacional abrir o arquivo e executar uma cópia de modo kernel direto para a placa de rede.</span><span class="sxs-lookup"><span data-stu-id="ee3db-133">When available, the `IHttpSendFileFeature` allows the operating system to open the file and perform a direct kernel mode copy to the network card.</span></span>

<span data-ttu-id="ee3db-134">Além disso, o middleware pode adicionar à coleção de recurso estabelecida pelo servidor.</span><span class="sxs-lookup"><span data-stu-id="ee3db-134">Additionally, middleware can add to the feature collection established by the server.</span></span> <span data-ttu-id="ee3db-135">Recursos existentes ainda podem ser substituídos pelo middleware, permitindo que o middleware ampliar a funcionalidade do servidor.</span><span class="sxs-lookup"><span data-stu-id="ee3db-135">Existing features can even be replaced by middleware, allowing the middleware to augment the functionality of the server.</span></span> <span data-ttu-id="ee3db-136">Recursos adicionados à coleção estão disponíveis imediatamente para o outro middleware ou o aplicativo em si subjacente posteriormente no pipeline de solicitação.</span><span class="sxs-lookup"><span data-stu-id="ee3db-136">Features added to the collection are available immediately to other middleware or the underlying application itself later in the request pipeline.</span></span>

<span data-ttu-id="ee3db-137">Combinando implementações personalizadas de servidor e aprimoramentos de middleware específico, o conjunto exato de recursos que requer que um aplicativo pode ser construído.</span><span class="sxs-lookup"><span data-stu-id="ee3db-137">By combining custom server implementations and specific middleware enhancements, the precise set of features an application requires can be constructed.</span></span> <span data-ttu-id="ee3db-138">Isso permite que não tem recursos a serem adicionados sem a necessidade de uma alteração no servidor e garante que somente a quantidade mínima de recursos são expostas, limitando assim a ataque de superfície área e melhorar o desempenho.</span><span class="sxs-lookup"><span data-stu-id="ee3db-138">This allows missing features to be added without requiring a change in server, and ensures only the minimal amount of features are exposed, thus limiting attack surface area and improving performance.</span></span>

## <a name="summary"></a><span data-ttu-id="ee3db-139">Resumo</span><span class="sxs-lookup"><span data-stu-id="ee3db-139">Summary</span></span>

<span data-ttu-id="ee3db-140">Interfaces de recurso Definir recursos específicos de HTTP que pode dar suporte a uma determinada solicitação.</span><span class="sxs-lookup"><span data-stu-id="ee3db-140">Feature interfaces define specific HTTP features that a given request may support.</span></span> <span data-ttu-id="ee3db-141">Servidores definem coleções de recursos e o conjunto inicial de recursos com suporte pelo servidor, mas middleware pode ser usado para aprimorar a esses recursos.</span><span class="sxs-lookup"><span data-stu-id="ee3db-141">Servers define collections of features, and the initial set of features supported by that server, but middleware can be used to enhance these features.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ee3db-142">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="ee3db-142">Additional Resources</span></span>

* [<span data-ttu-id="ee3db-143">Servidores</span><span class="sxs-lookup"><span data-stu-id="ee3db-143">Servers</span></span>](servers/index.md)

* [<span data-ttu-id="ee3db-144">Middleware</span><span class="sxs-lookup"><span data-stu-id="ee3db-144">Middleware</span></span>](middleware.md)

* [<span data-ttu-id="ee3db-145">OWIN (Open Web Interface para .NET)</span><span class="sxs-lookup"><span data-stu-id="ee3db-145">Open Web Interface for .NET (OWIN)</span></span>](owin.md)
