---
title: Cliente de Java do SignalR Core ASP.NET
author: mikaelm12
description: Saiba como usar o cliente de Java de SignalR do ASP.NET Core.
monikerRange: '>= aspnetcore-2.2'
ms.author: mimengis
ms.custom: mvc
ms.date: 10/18/2018
uid: signalr/java-client
ms.openlocfilehash: 77ea338f08b1986e69ba8ef1578c4cfe01a310de
ms.sourcegitcommit: ce6b6792c650708e92cdea051a5d166c0708c7c0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/23/2018
ms.locfileid: "49652300"
---
# <a name="aspnet-core-signalr-java-client"></a><span data-ttu-id="aadff-103">Cliente de Java do SignalR Core ASP.NET</span><span class="sxs-lookup"><span data-stu-id="aadff-103">ASP.NET Core SignalR Java client</span></span>

<span data-ttu-id="aadff-104">Por [Mikael Mengistu](https://twitter.com/MikaelM_12)</span><span class="sxs-lookup"><span data-stu-id="aadff-104">By [Mikael Mengistu](https://twitter.com/MikaelM_12)</span></span>

<span data-ttu-id="aadff-105">O cliente de Java permite se conectar a um servidor do SignalR do ASP.NET Core do código Java, incluindo aplicativos Android.</span><span class="sxs-lookup"><span data-stu-id="aadff-105">The Java client enables connecting to an ASP.NET Core SignalR server from Java code, including Android apps.</span></span> <span data-ttu-id="aadff-106">Como o [cliente JavaScript](xref:signalr/javascript-client) e o [cliente .NET](xref:signalr/dotnet-client), o cliente de Java permite que você receber e enviar mensagens a um hub em tempo real.</span><span class="sxs-lookup"><span data-stu-id="aadff-106">Like the [JavaScript client](xref:signalr/javascript-client) and the [.NET client](xref:signalr/dotnet-client), the Java client enables you to receive and send messages to a hub in real time.</span></span> <span data-ttu-id="aadff-107">O cliente de Java está disponível no ASP.NET Core 2.2 e posterior.</span><span class="sxs-lookup"><span data-stu-id="aadff-107">The Java client is available in ASP.NET Core 2.2 and later.</span></span>

<span data-ttu-id="aadff-108">O aplicativo de console do Java de exemplo referenciado neste artigo usa o cliente de Java do SignalR.</span><span class="sxs-lookup"><span data-stu-id="aadff-108">The sample Java console app referenced in this article uses the SignalR Java client.</span></span>

<span data-ttu-id="aadff-109">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="aadff-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-java-client-package"></a><span data-ttu-id="aadff-110">Instalar o pacote de cliente Java do SignalR</span><span class="sxs-lookup"><span data-stu-id="aadff-110">Install the SignalR Java client package</span></span>

<span data-ttu-id="aadff-111">O *signalr 1.0.0-preview3 35501* arquivo JAR permite que os clientes se conectem aos hubs de SignalR.</span><span class="sxs-lookup"><span data-stu-id="aadff-111">The *signalr-1.0.0-preview3-35501* JAR file allows clients to connect to SignalR hubs.</span></span> <span data-ttu-id="aadff-112">Para localizar o número de versão de arquivo JAR mais recente, consulte o [resultados da pesquisa Maven](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr).</span><span class="sxs-lookup"><span data-stu-id="aadff-112">To find the latest JAR file version number, see the [Maven search results](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr).</span></span>

<span data-ttu-id="aadff-113">Se usando o Gradle, adicione a seguinte linha para o `dependencies` seção do seu *Build. gradle* arquivo:</span><span class="sxs-lookup"><span data-stu-id="aadff-113">If using Gradle, add the following line to the `dependencies` section of your *build.gradle* file:</span></span>

```gradle
implementation 'com.microsoft.signalr:signalr:1.0.0-preview3-35501'
implementation 'io.reactivex.rxjava2:rxjava:2.2.2'
```

<span data-ttu-id="aadff-114">Se usando o Maven, adicione as seguintes linhas dentro de `<dependencies>` elemento da sua *POM. XML* arquivo:</span><span class="sxs-lookup"><span data-stu-id="aadff-114">If using Maven, add the following lines inside the `<dependencies>` element of your *pom.xml* file:</span></span>

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a><span data-ttu-id="aadff-115">Conectar a um hub</span><span class="sxs-lookup"><span data-stu-id="aadff-115">Connect to a hub</span></span>

<span data-ttu-id="aadff-116">Para estabelecer uma `HubConnection`, o `HubConnectionBuilder` deve ser usado.</span><span class="sxs-lookup"><span data-stu-id="aadff-116">To establish a `HubConnection`, the `HubConnectionBuilder` should be used.</span></span> <span data-ttu-id="aadff-117">O nível de log e a URL do hub pode ser configurado durante a criação de uma conexão.</span><span class="sxs-lookup"><span data-stu-id="aadff-117">The hub URL and log level can be configured while building a connection.</span></span> <span data-ttu-id="aadff-118">Configurar as opções necessárias chamando o `HubConnectionBuilder` métodos antes `build`.</span><span class="sxs-lookup"><span data-stu-id="aadff-118">Configure any required options by calling any of the `HubConnectionBuilder` methods before `build`.</span></span> <span data-ttu-id="aadff-119">Iniciar a conexão com `start`.</span><span class="sxs-lookup"><span data-stu-id="aadff-119">Start the connection with `start`.</span></span>

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=16-17)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="aadff-120">Chamar métodos de hub do cliente</span><span class="sxs-lookup"><span data-stu-id="aadff-120">Call hub methods from client</span></span>

<span data-ttu-id="aadff-121">Uma chamada para `send` invoca um método de hub.</span><span class="sxs-lookup"><span data-stu-id="aadff-121">A call to `send` invokes a hub method.</span></span> <span data-ttu-id="aadff-122">Passe o nome do método de hub e quaisquer argumentos definidos no método de hub para `send`.</span><span class="sxs-lookup"><span data-stu-id="aadff-122">Pass the hub method name and any arguments defined in the hub method to `send`.</span></span>

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=28)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="aadff-123">Chamar métodos de cliente do hub</span><span class="sxs-lookup"><span data-stu-id="aadff-123">Call client methods from hub</span></span>

<span data-ttu-id="aadff-124">Use `hubConnection.on` para definir métodos no cliente que o hub pode chamar.</span><span class="sxs-lookup"><span data-stu-id="aadff-124">Use `hubConnection.on` to define methods on the client that the hub can call.</span></span> <span data-ttu-id="aadff-125">Defina os métodos depois de criar, mas antes de iniciar a conexão.</span><span class="sxs-lookup"><span data-stu-id="aadff-125">Define the methods after building but before starting the connection.</span></span>

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=19-21)]

## <a name="add-logging"></a><span data-ttu-id="aadff-126">Adicionar registro em log</span><span class="sxs-lookup"><span data-stu-id="aadff-126">Add logging</span></span>

<span data-ttu-id="aadff-127">O cliente SignalR Java usa a [SLF4J](https://www.slf4j.org/) biblioteca para registro em log.</span><span class="sxs-lookup"><span data-stu-id="aadff-127">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="aadff-128">É uma API de alto nível de log que permite aos usuários da biblioteca de escolher sua própria implementação de log específico, colocando em uma dependência de log específico.</span><span class="sxs-lookup"><span data-stu-id="aadff-128">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="aadff-129">O trecho de código a seguir mostra como usar `java.util.logging` com o cliente de Java do SignalR.</span><span class="sxs-lookup"><span data-stu-id="aadff-129">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="aadff-130">Se você não configurar o registro em log em suas dependências, SLF4J carrega um agente de operação não padrão com a seguinte mensagem de aviso:</span><span class="sxs-lookup"><span data-stu-id="aadff-130">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="aadff-131">Isso pode ser ignorado.</span><span class="sxs-lookup"><span data-stu-id="aadff-131">This can safely be ignored.</span></span>

## <a name="known-limitations"></a><span data-ttu-id="aadff-132">Limitações conhecidas</span><span class="sxs-lookup"><span data-stu-id="aadff-132">Known limitations</span></span>

<span data-ttu-id="aadff-133">Esta é uma versão de visualização do cliente Java.</span><span class="sxs-lookup"><span data-stu-id="aadff-133">This is a preview release of the Java client.</span></span> <span data-ttu-id="aadff-134">Não há suporte para alguns recursos:</span><span class="sxs-lookup"><span data-stu-id="aadff-134">Some features aren't supported:</span></span>

* <span data-ttu-id="aadff-135">Há suporte para apenas o protocolo JSON.</span><span class="sxs-lookup"><span data-stu-id="aadff-135">Only the JSON protocol is supported.</span></span>
* <span data-ttu-id="aadff-136">Há suporte para apenas o transporte de WebSockets.</span><span class="sxs-lookup"><span data-stu-id="aadff-136">Only the WebSockets transport is supported.</span></span>
* <span data-ttu-id="aadff-137">Streaming ainda não tem suporte.</span><span class="sxs-lookup"><span data-stu-id="aadff-137">Streaming isn't supported yet.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="aadff-138">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="aadff-138">Additional resources</span></span>

* [<span data-ttu-id="aadff-139">Referência de API Java</span><span class="sxs-lookup"><span data-stu-id="aadff-139">Java API reference</span></span>](/java/api/com.microsoft.aspnet.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
