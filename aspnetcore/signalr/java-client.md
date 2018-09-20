---
title: Cliente de Java do SignalR Core ASP.NET
author: mikaelm12
description: Saiba como usar o cliente de Java de SignalR do ASP.NET Core.
monikerRange: '>= aspnetcore-2.2'
ms.author: mimengis
ms.custom: mvc
ms.date: 09/06/2018
uid: signalr/java-client
ms.openlocfilehash: 0eba59a05ea6fd3fed46fcab86ac20caf40ebb65
ms.sourcegitcommit: 8bf4dff3069e62972c1b0839a93fb444e502afe7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/20/2018
ms.locfileid: "46482912"
---
# <a name="aspnet-core-signalr-java-client"></a><span data-ttu-id="899bf-103">Cliente de Java do SignalR do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="899bf-103">ASP.NET Core SignalR Java Client</span></span>

<span data-ttu-id="899bf-104">Por [Mikael Mengistu](https://twitter.com/MikaelM_12)</span><span class="sxs-lookup"><span data-stu-id="899bf-104">By [Mikael Mengistu](https://twitter.com/MikaelM_12)</span></span>

<span data-ttu-id="899bf-105">O cliente de Java permite se conectar a um servidor do SignalR do ASP.NET Core do código Java, incluindo aplicativos Android.</span><span class="sxs-lookup"><span data-stu-id="899bf-105">The Java client enables connecting to an ASP.NET Core SignalR server from Java code, including Android apps.</span></span> <span data-ttu-id="899bf-106">Como o [cliente JavaScript](xref:signalr/javascript-client) e o [cliente .NET](xref:signalr/dotnet-client), o cliente de Java permite que você receber e enviar mensagens a um hub em tempo real.</span><span class="sxs-lookup"><span data-stu-id="899bf-106">Like the [JavaScript client](xref:signalr/javascript-client) and the [.NET client](xref:signalr/dotnet-client), the Java client enables you to receive and send messages to a hub in real time.</span></span> <span data-ttu-id="899bf-107">O cliente de Java está disponível no ASP.NET Core 2.2 e posterior.</span><span class="sxs-lookup"><span data-stu-id="899bf-107">The Java client is available in ASP.NET Core 2.2 and later.</span></span>

<span data-ttu-id="899bf-108">O aplicativo de console do Java de exemplo referenciado neste artigo usa o cliente de Java do SignalR.</span><span class="sxs-lookup"><span data-stu-id="899bf-108">The sample Java console app referenced in this article uses the SignalR Java client.</span></span>

<span data-ttu-id="899bf-109">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="899bf-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-java-client-package"></a><span data-ttu-id="899bf-110">Instalar o pacote de cliente Java do SignalR</span><span class="sxs-lookup"><span data-stu-id="899bf-110">Install the SignalR Java client package</span></span>

<span data-ttu-id="899bf-111">O *signalr 0.1.0 – visualização 2 35174* arquivo JAR permite que os clientes se conectem aos hubs de SignalR.</span><span class="sxs-lookup"><span data-stu-id="899bf-111">The *signalr-0.1.0-preview2-35174* JAR file allows clients to connect to SignalR hubs.</span></span> <span data-ttu-id="899bf-112">Para localizar o número de versão de arquivo JAR mais recente, consulte o [resultados da pesquisa Maven](https://search.maven.org/search?q=g:com.microsoft.aspnet%20AND%20a:signalr&core=gav).</span><span class="sxs-lookup"><span data-stu-id="899bf-112">To find the latest JAR file version number, see the [Maven search results](https://search.maven.org/search?q=g:com.microsoft.aspnet%20AND%20a:signalr&core=gav).</span></span>

<span data-ttu-id="899bf-113">Se usando o Gradle, adicione a seguinte linha para o `dependencies` seção do seu *Build. gradle* arquivo:</span><span class="sxs-lookup"><span data-stu-id="899bf-113">If using Gradle, add the following line to the `dependencies` section of your *build.gradle* file:</span></span>

```gradle
implementation 'com.microsoft.aspnet:signalr:0.1.0-preview2-35174'
```

<span data-ttu-id="899bf-114">Se usando o Maven, adicione as seguintes linhas dentro de `<dependencies>` elemento da sua *POM. XML* arquivo:</span><span class="sxs-lookup"><span data-stu-id="899bf-114">If using Maven, add the following lines inside the `<dependencies>` element of your *pom.xml* file:</span></span>

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a><span data-ttu-id="899bf-115">Conectar a um hub</span><span class="sxs-lookup"><span data-stu-id="899bf-115">Connect to a hub</span></span>

<span data-ttu-id="899bf-116">Para estabelecer uma `HubConnection`, o `HubConnectionBuilder` deve ser usado.</span><span class="sxs-lookup"><span data-stu-id="899bf-116">To establish a `HubConnection`, the `HubConnectionBuilder` should be used.</span></span> <span data-ttu-id="899bf-117">O nível de log e a URL do hub pode ser configurado durante a criação de uma conexão.</span><span class="sxs-lookup"><span data-stu-id="899bf-117">The hub URL and log level can be configured while building a connection.</span></span> <span data-ttu-id="899bf-118">Configurar as opções necessárias chamando o `HubConnectionBuilder` métodos antes `build`.</span><span class="sxs-lookup"><span data-stu-id="899bf-118">Configure any required options by calling any of the `HubConnectionBuilder` methods before `build`.</span></span> <span data-ttu-id="899bf-119">Iniciar a conexão com `start`.</span><span class="sxs-lookup"><span data-stu-id="899bf-119">Start the connection with `start`.</span></span>

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=17-20)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="899bf-120">Chamar métodos de hub do cliente</span><span class="sxs-lookup"><span data-stu-id="899bf-120">Call hub methods from client</span></span>

<span data-ttu-id="899bf-121">Uma chamada para `send` invoca um método de hub.</span><span class="sxs-lookup"><span data-stu-id="899bf-121">A call to `send` invokes a hub method.</span></span> <span data-ttu-id="899bf-122">Passe o nome do método de hub e quaisquer argumentos definidos no método de hub para `send`.</span><span class="sxs-lookup"><span data-stu-id="899bf-122">Pass the hub method name and any arguments defined in the hub method to `send`.</span></span>

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=31)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="899bf-123">Chamar métodos de cliente do hub</span><span class="sxs-lookup"><span data-stu-id="899bf-123">Call client methods from hub</span></span>

<span data-ttu-id="899bf-124">Use `hubConnection.on` para definir métodos no cliente que o hub pode chamar.</span><span class="sxs-lookup"><span data-stu-id="899bf-124">Use `hubConnection.on` to define methods on the client that the hub can call.</span></span> <span data-ttu-id="899bf-125">Defina os métodos depois de criar, mas antes de iniciar a conexão.</span><span class="sxs-lookup"><span data-stu-id="899bf-125">Define the methods after building but before starting the connection.</span></span>

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=22-24)]

## <a name="known-limitations"></a><span data-ttu-id="899bf-126">Limitações conhecidas</span><span class="sxs-lookup"><span data-stu-id="899bf-126">Known limitations</span></span>

<span data-ttu-id="899bf-127">Isso é uma versão de visualização inicial do cliente Java.</span><span class="sxs-lookup"><span data-stu-id="899bf-127">This is an early preview release of the Java client.</span></span> <span data-ttu-id="899bf-128">Há muitos recursos que ainda não têm suporte.</span><span class="sxs-lookup"><span data-stu-id="899bf-128">There are many features that aren't supported yet.</span></span> <span data-ttu-id="899bf-129">Os intervalos a seguir estão sendo trabalhados para versões futuras:</span><span class="sxs-lookup"><span data-stu-id="899bf-129">The following gaps are being worked on for future releases:</span></span>

* <span data-ttu-id="899bf-130">Apenas tipos primitivos podem ser aceitos como parâmetros e tipos de retorno.</span><span class="sxs-lookup"><span data-stu-id="899bf-130">Only primitive types can be accepted as parameters and return types.</span></span>
* <span data-ttu-id="899bf-131">As APIs são síncronas.</span><span class="sxs-lookup"><span data-stu-id="899bf-131">The APIs are synchronous.</span></span>
* <span data-ttu-id="899bf-132">Somente o tipo de chamada de "Envio" é suportado neste momento.</span><span class="sxs-lookup"><span data-stu-id="899bf-132">Only the "Send" call type is supported at this time.</span></span> <span data-ttu-id="899bf-133">"Invocar" e transmissão de valores de retorno não são suportados.</span><span class="sxs-lookup"><span data-stu-id="899bf-133">"Invoke" and streaming of return values aren't supported.</span></span>
* <span data-ttu-id="899bf-134">Há suporte para apenas o protocolo JSON.</span><span class="sxs-lookup"><span data-stu-id="899bf-134">Only the JSON protocol is supported.</span></span>
* <span data-ttu-id="899bf-135">Há suporte para apenas o transporte de WebSockets.</span><span class="sxs-lookup"><span data-stu-id="899bf-135">Only the WebSockets transport is supported.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="899bf-136">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="899bf-136">Additional resources</span></span>

* [<span data-ttu-id="899bf-137">Referência da API Java</span><span class="sxs-lookup"><span data-stu-id="899bf-137">Java API reference</span></span>](/java/api/com.microsoft.aspnet.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
