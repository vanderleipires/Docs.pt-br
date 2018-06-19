---
uid: signalr/overview/advanced/dependency-injection
title: Injeção de dependência no SignalR | Microsoft Docs
author: MikeWasson
description: Versões de software usados neste tópico Visual Studio 2013 .NET 4.5 SignalR versões anteriores de versão 2 deste tópico para obter informações sobre versões anteriores do...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: a14121ae-02cf-4024-8af0-9dd0dc810690
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 3732b5d0ea6de841a6c402bfd5ef4dfb7b7a9162
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26504175"
---
<a name="dependency-injection-in-signalr"></a><span data-ttu-id="5a9a1-103">Injeção de dependência no SignalR</span><span class="sxs-lookup"><span data-stu-id="5a9a1-103">Dependency Injection in SignalR</span></span>
====================
<span data-ttu-id="5a9a1-104">por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="5a9a1-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="5a9a1-105">Versões de software usadas neste tópico</span><span class="sxs-lookup"><span data-stu-id="5a9a1-105">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="5a9a1-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="5a9a1-106">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="5a9a1-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="5a9a1-107">.NET 4.5</span></span>
> - <span data-ttu-id="5a9a1-108">SignalR versão 2</span><span class="sxs-lookup"><span data-stu-id="5a9a1-108">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="5a9a1-109">Versões anteriores deste tópico</span><span class="sxs-lookup"><span data-stu-id="5a9a1-109">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="5a9a1-110">Para obter informações sobre versões anteriores do SignalR, consulte [versões mais antigas do SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="5a9a1-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="5a9a1-111">Perguntas e comentários</span><span class="sxs-lookup"><span data-stu-id="5a9a1-111">Questions and comments</span></span>
> 
> <span data-ttu-id="5a9a1-112">Deixe comentários em como você gostou neste tutorial e o que podemos melhorar nos comentários na parte inferior da página.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="5a9a1-113">Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, você poderá postá-los para o [ASP.NET SignalR fórum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="5a9a1-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="5a9a1-114">Injeção de dependência é uma maneira de remover embutido dependências entre objetos, tornando mais fácil para substituir as dependências de um objeto, para testar (usando objetos fictícios) ou para alterar o comportamento de tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-114">Dependency injection is a way to remove hard-coded dependencies between objects, making it easier to replace an object's dependencies, either for testing (using mock objects) or to change run-time behavior.</span></span> <span data-ttu-id="5a9a1-115">Este tutorial mostra como realizar injeção de dependência em hubs de SignalR.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-115">This tutorial shows how to perform dependency injection on SignalR hubs.</span></span> <span data-ttu-id="5a9a1-116">Ele também mostra como usar contêineres IoC com SignalR.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-116">It also shows how to use IoC containers with SignalR.</span></span> <span data-ttu-id="5a9a1-117">Um contêiner IoC é uma estrutura geral para a injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-117">An IoC container is a general framework for dependency injection.</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="5a9a1-118">O que é a injeção de dependência?</span><span class="sxs-lookup"><span data-stu-id="5a9a1-118">What is Dependency Injection?</span></span>

<span data-ttu-id="5a9a1-119">Se você já estiver familiarizado com a injeção de dependência, ignore esta seção.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-119">Skip this section if you are already familiar with dependency injection.</span></span>

<span data-ttu-id="5a9a1-120">*Injeção de dependência* (DI) é um padrão onde os objetos não são responsáveis por criar suas próprias dependências.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-120">*Dependency injection* (DI) is a pattern where objects are not responsible for creating their own dependencies.</span></span> <span data-ttu-id="5a9a1-121">Aqui está um exemplo simples motivar DI.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-121">Here is a simple example to motivate DI.</span></span> <span data-ttu-id="5a9a1-122">Suponha que você tem um objeto que precisa para as mensagens de log.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-122">Suppose you have an object that needs to log messages.</span></span> <span data-ttu-id="5a9a1-123">Você pode definir uma interface de log:</span><span class="sxs-lookup"><span data-stu-id="5a9a1-123">You might define a logging interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="5a9a1-124">Em seu objeto, você pode criar um `ILogger` para as mensagens de log:</span><span class="sxs-lookup"><span data-stu-id="5a9a1-124">In your object, you can create an `ILogger` to log messages:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="5a9a1-125">Isso funciona, mas não é o melhor design.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-125">This works, but it's not the best design.</span></span> <span data-ttu-id="5a9a1-126">Se você quiser substituir `FileLogger` com outra `ILogger` implementação, você precisará modificar `SomeComponent`.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-126">If you want to replace `FileLogger` with another `ILogger` implementation, you will have to modify `SomeComponent`.</span></span> <span data-ttu-id="5a9a1-127">Suponhamos que muitos outros objetos `FileLogger`, você precisará alterar todos eles.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-127">Supposing that a lot of other objects use `FileLogger`, you will need to change all of them.</span></span> <span data-ttu-id="5a9a1-128">Ou se você decidir fazer `FileLogger` um singleton, você também precisará fazer alterações em todo o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-128">Or if you decide to make `FileLogger` a singleton, you'll also need to make changes throughout the application.</span></span>

<span data-ttu-id="5a9a1-129">Uma abordagem melhor é "inserir" um `ILogger` no objeto — por exemplo, usando um argumento de construtor:</span><span class="sxs-lookup"><span data-stu-id="5a9a1-129">A better approach is to "inject" an `ILogger` into the object—for example, by using a constructor argument:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="5a9a1-130">Agora que o objeto não é responsável por selecionar qual `ILogger` para usar.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-130">Now the object is not responsible for selecting which `ILogger` to use.</span></span> <span data-ttu-id="5a9a1-131">Você pode alternar `ILogger` implementações sem alterar os objetos que dependem dele.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-131">You can swich `ILogger` implementations without changing the objects that depend on it.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="5a9a1-132">Esse padrão é chamado [injeção de construtor](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="5a9a1-132">This pattern is called [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="5a9a1-133">Outro padrão é a inclusão de setter, em que você definir a dependência por meio de uma propriedade ou método de setter.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-133">Another pattern is setter injection, where you set the dependency through a setter method or property.</span></span>

## <a name="simple-dependency-injection-in-signalr"></a><span data-ttu-id="5a9a1-134">Injeção de dependência simples no SignalR</span><span class="sxs-lookup"><span data-stu-id="5a9a1-134">Simple Dependency Injection in SignalR</span></span>

<span data-ttu-id="5a9a1-135">Considere a aplicação de Chat do tutorial [guia de Introdução ao SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="5a9a1-135">Consider the Chat application from the tutorial [Getting Started with SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="5a9a1-136">Aqui está a classe de hub do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="5a9a1-136">Here is the hub class from that application:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="5a9a1-137">Suponha que você deseja armazenar mensagens de chat no servidor antes de enviá-los.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-137">Suppose that you want to store chat messages on the server before sending them.</span></span> <span data-ttu-id="5a9a1-138">Você pode definir uma interface que abstrai essa funcionalidade e usar a DI injetar a interface para o `ChatHub` classe.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-138">You might define an interface that abstracts this functionality, and use DI to inject the interface into the `ChatHub` class.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="5a9a1-139">O único problema é que um aplicativo SignalR não cria diretamente hubs; SignalR criará para você.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-139">The only problem is that a SignalR application does not directly create hubs; SignalR creates them for you.</span></span> <span data-ttu-id="5a9a1-140">Por padrão, o SignalR espera uma classe de hub para ter um construtor sem parâmetros.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-140">By default, SignalR expects a hub class to have a parameterless constructor.</span></span> <span data-ttu-id="5a9a1-141">No entanto, você pode facilmente registrar uma função para criar instâncias de hub e usar essa função para executar a injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-141">However, you can easily register a function to create hub instances, and use this function to perform DI.</span></span> <span data-ttu-id="5a9a1-142">Registre a função chamando **GlobalHost.DependencyResolver.Register**.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-142">Register the function by calling **GlobalHost.DependencyResolver.Register**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

<span data-ttu-id="5a9a1-143">Agora o SignalR chamará essa função anônima sempre que precisar criar um `ChatHub` instância.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-143">Now SignalR will invoke this anonymous function whenever it needs to create a `ChatHub` instance.</span></span>

## <a name="ioc-containers"></a><span data-ttu-id="5a9a1-144">Contêineres IoC</span><span class="sxs-lookup"><span data-stu-id="5a9a1-144">IoC Containers</span></span>

<span data-ttu-id="5a9a1-145">O código anterior é bom para casos simples.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-145">The previous code is fine for simple cases.</span></span> <span data-ttu-id="5a9a1-146">Mas ainda era necessário escrever isso:</span><span class="sxs-lookup"><span data-stu-id="5a9a1-146">But you still had to write this:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

<span data-ttu-id="5a9a1-147">Em um aplicativo complexo com várias dependências, você precisará escrever muito do código "fiação".</span><span class="sxs-lookup"><span data-stu-id="5a9a1-147">In a complex application with many dependencies, you might need to write a lot of this "wiring" code.</span></span> <span data-ttu-id="5a9a1-148">Esse código pode ser difícil de manter, especialmente se as dependências são aninhadas.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-148">This code can be hard to maintain, especially if dependencies are nested.</span></span> <span data-ttu-id="5a9a1-149">Também é difícil de teste de unidade.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-149">It is also hard to unit test.</span></span>

<span data-ttu-id="5a9a1-150">Uma solução é usar um contêiner IoC.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-150">One solution is to use an IoC container.</span></span> <span data-ttu-id="5a9a1-151">Um contêiner IoC é um componente de software que é responsável por gerenciar as dependências. Você registrar tipos com o contêiner e, em seguida, usa o contêiner para criar objetos.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-151">An IoC container is a software component that is responsible for managing dependencies.You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="5a9a1-152">O contêiner automaticamente descobre as relações de dependência.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-152">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="5a9a1-153">Vários contêineres IoC também permitem que você controle como escopo e tempo de vida do objeto.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-153">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="5a9a1-154">"IoC" significa "inversão de controle", que é um padrão geral em que uma estrutura chama o código do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-154">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="5a9a1-155">Um contêiner IoC constrói os objetos para você, o que "inverte" o fluxo normal de controle.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-155">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


## <a name="using-ioc-containers-in-signalr"></a><span data-ttu-id="5a9a1-156">Usando contêineres IoC no SignalR</span><span class="sxs-lookup"><span data-stu-id="5a9a1-156">Using IoC Containers in SignalR</span></span>

<span data-ttu-id="5a9a1-157">O aplicativo de Chat provavelmente é muito simple para se beneficiar de um contêiner IoC.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-157">The Chat application is probably too simple to benefit from an IoC container.</span></span> <span data-ttu-id="5a9a1-158">Em vez disso, vamos analisar o [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) exemplo.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-158">Instead, let's look at the [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) sample.</span></span>

<span data-ttu-id="5a9a1-159">O exemplo StockTicker define duas classes principais:</span><span class="sxs-lookup"><span data-stu-id="5a9a1-159">The StockTicker sample defines two main classes:</span></span>

- <span data-ttu-id="5a9a1-160">`StockTickerHub`: A classe de hub, que gerencia as conexões de cliente.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-160">`StockTickerHub`: The hub class, which manages client connections.</span></span>
- <span data-ttu-id="5a9a1-161">`StockTicker`: Um singleton que contém os preços de estoque e os atualiza periodicamente.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-161">`StockTicker`: A singleton that holds stock prices and periodically updates them.</span></span>

<span data-ttu-id="5a9a1-162">`StockTickerHub`contém uma referência para o `StockTicker` singleton, enquanto `StockTicker` contém uma referência para o **IHubConnectionContext** para o `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-162">`StockTickerHub` holds a reference to the `StockTicker` singleton, while `StockTicker` holds a reference to the **IHubConnectionContext** for the `StockTickerHub`.</span></span> <span data-ttu-id="5a9a1-163">Ele usa essa interface para se comunicar com `StockTickerHub` instâncias.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-163">It uses this interface to communicate with `StockTickerHub` instances.</span></span> <span data-ttu-id="5a9a1-164">(Para obter mais informações, consulte [de difusão de servidor com ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).)</span><span class="sxs-lookup"><span data-stu-id="5a9a1-164">(For more information, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).)</span></span>

<span data-ttu-id="5a9a1-165">Podemos usar um contêiner IoC para simplificar os essas dependências um pouco.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-165">We can use an IoC container to untangle these dependencies a bit.</span></span> <span data-ttu-id="5a9a1-166">Primeiro, vamos simplificar o `StockTickerHub` e `StockTicker` classes.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-166">First, let's simplify the `StockTickerHub` and `StockTicker` classes.</span></span> <span data-ttu-id="5a9a1-167">No código a seguir, eu já comentadas as partes que não é necessário.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-167">In the following code, I've commented out the parts that we don't need.</span></span>

<span data-ttu-id="5a9a1-168">Remover o construtor sem parâmetros de `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-168">Remove the parameterless constructor from `StockTickerHub`.</span></span> <span data-ttu-id="5a9a1-169">Em vez disso, nós usaremos sempre DI para criar o hub.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-169">Instead, we will always use DI to create the hub.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

<span data-ttu-id="5a9a1-170">Para StockTicker, remova a instância singleton.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-170">For StockTicker, remove the singleton instance.</span></span> <span data-ttu-id="5a9a1-171">Posteriormente, usaremos o contêiner IoC para controlar o tempo de vida StockTicker.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-171">Later, we'll use the IoC container to control the StockTicker lifetime.</span></span> <span data-ttu-id="5a9a1-172">Além disso, verifique o construtor público.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-172">Also, make the constructor public.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

<span data-ttu-id="5a9a1-173">Em seguida, é possível refatorar o código, criando uma interface para `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-173">Next, we can refactor the code by creating an interface for `StockTicker`.</span></span> <span data-ttu-id="5a9a1-174">Vamos usar essa interface para desacoplar a `StockTickerHub` do `StockTicker` classe.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-174">We'll use this interface to decouple the `StockTickerHub` from the `StockTicker` class.</span></span>

<span data-ttu-id="5a9a1-175">O Visual Studio faz esse tipo de refatoração fácil.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-175">Visual Studio makes this kind of refactoring easy.</span></span> <span data-ttu-id="5a9a1-176">Abra o arquivo StockTicker.cs, clique duas vezes no `StockTicker` declaração de classe e, em seguida, selecione **refatorar** ... **Extrair Interface**.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-176">Open the file StockTicker.cs, right-click on the `StockTicker` class declaration, and select **Refactor** ... **Extract Interface**.</span></span>

![](dependency-injection/_static/image1.png)

<span data-ttu-id="5a9a1-177">No **extrair Interface** caixa de diálogo, clique em **Selecionar tudo**.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-177">In the **Extract Interface** dialog, click **Select All**.</span></span> <span data-ttu-id="5a9a1-178">Deixe os outros padrões.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-178">Leave the other defaults.</span></span> <span data-ttu-id="5a9a1-179">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-179">Click **OK**.</span></span>

![](dependency-injection/_static/image2.png)

<span data-ttu-id="5a9a1-180">O Visual Studio cria uma nova interface chamada `IStockTicker`e também o altera `StockTicker` derivar `IStockTicker`.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-180">Visual Studio creates a new interface named `IStockTicker`, and also changes `StockTicker` to derive from `IStockTicker`.</span></span>

<span data-ttu-id="5a9a1-181">Abra o arquivo IStockTicker.cs e alterar a interface para **público**.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-181">Open the file IStockTicker.cs and change the interface to **public**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

<span data-ttu-id="5a9a1-182">No `StockTickerHub` classe, altere as duas instâncias de `StockTicker` para `IStockTicker`:</span><span class="sxs-lookup"><span data-stu-id="5a9a1-182">In the `StockTickerHub` class, change the two instances of `StockTicker` to `IStockTicker`:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

<span data-ttu-id="5a9a1-183">Criando um `IStockTicker` interface não é estritamente necessária, mas gostaria de mostrar como a injeção de dependência pode ajudar a reduzir o acoplamento flexível entre os componentes de seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-183">Creating an `IStockTicker` interface isn't strictly necessary, but I wanted to show how DI can help to reduce coupling between components in your application.</span></span>

## <a name="add-the-ninject-library"></a><span data-ttu-id="5a9a1-184">Adicionar a biblioteca de Ninject</span><span class="sxs-lookup"><span data-stu-id="5a9a1-184">Add the Ninject Library</span></span>

<span data-ttu-id="5a9a1-185">Há vários contêineres IoC de código-fonte aberto para .NET.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-185">There are many open-source IoC containers for .NET.</span></span> <span data-ttu-id="5a9a1-186">Para este tutorial, usarei [Ninject](http://www.ninject.org/).</span><span class="sxs-lookup"><span data-stu-id="5a9a1-186">For this tutorial, I'll use [Ninject](http://www.ninject.org/).</span></span> <span data-ttu-id="5a9a1-187">(Outras bibliotecas populares incluem [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), e [StructureMap ](http://docs.structuremap.net).)</span><span class="sxs-lookup"><span data-stu-id="5a9a1-187">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), and [StructureMap](http://docs.structuremap.net).)</span></span>

<span data-ttu-id="5a9a1-188">Use o Gerenciador de pacote de NuGet para instalar o [Ninject biblioteca](https://nuget.org/packages/Ninject/3.0.1.10).</span><span class="sxs-lookup"><span data-stu-id="5a9a1-188">Use NuGet Package Manager to install the [Ninject library](https://nuget.org/packages/Ninject/3.0.1.10).</span></span> <span data-ttu-id="5a9a1-189">No Visual Studio, do **ferramentas** menu, selecione **Gerenciador de biblioteca de pacote** | **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-189">In Visual Studio, from the **Tools** menu select **Library Package Manager** | **Package Manager Console**.</span></span> <span data-ttu-id="5a9a1-190">Na janela do Console do Gerenciador de pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="5a9a1-190">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a><span data-ttu-id="5a9a1-191">Substitua o resolvedor de dependência de SignalR</span><span class="sxs-lookup"><span data-stu-id="5a9a1-191">Replace the SignalR Dependency Resolver</span></span>

<span data-ttu-id="5a9a1-192">Para usar Ninject no SignalR, crie uma classe que deriva de **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-192">To use Ninject within SignalR, create a class that derives from **DefaultDependencyResolver**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

<span data-ttu-id="5a9a1-193">Esta classe substitui o **GetService** e **GetServices** métodos de **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-193">This class overrides the **GetService** and **GetServices** methods of **DefaultDependencyResolver**.</span></span> <span data-ttu-id="5a9a1-194">SignalR chama esses métodos para criar vários objetos em tempo de execução, incluindo instâncias do hub, bem como vários serviços usados internamente pelo SignalR.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-194">SignalR calls these methods to create various objects at runtime, including hub instances, as well as various services used internally by SignalR.</span></span>

- <span data-ttu-id="5a9a1-195">O **GetService** método cria uma única instância de um tipo.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-195">The **GetService** method creates a single instance of a type.</span></span> <span data-ttu-id="5a9a1-196">Substitua este método para chamar o kernel de Ninject **TryGet** método.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-196">Override this method to call the Ninject kernel's **TryGet** method.</span></span> <span data-ttu-id="5a9a1-197">Se esse método retornará nulo, voltar para o resolvedor padrão.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-197">If that method returns null, fall back to the default resolver.</span></span>
- <span data-ttu-id="5a9a1-198">O **GetServices** método cria uma coleção de objetos de um tipo especificado.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-198">The **GetServices** method creates a collection of objects of a specified type.</span></span> <span data-ttu-id="5a9a1-199">Substitua este método para concatenar os resultados de Ninject com os resultados do resolvedor padrão.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-199">Override this method to concatenate the results from Ninject with the results from the default resolver.</span></span>

## <a name="configure-ninject-bindings"></a><span data-ttu-id="5a9a1-200">Configurar as associações de Ninject</span><span class="sxs-lookup"><span data-stu-id="5a9a1-200">Configure Ninject Bindings</span></span>

<span data-ttu-id="5a9a1-201">Agora vamos usar Ninject para declarar associações de tipo.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-201">Now we'll use Ninject to declare type bindings.</span></span>

<span data-ttu-id="5a9a1-202">Abra a classe de Startup.cs do aplicativo (que é criada manualmente de acordo com as instruções do pacote de `readme.txt`, ou que foi criado pela adição de autenticação ao seu projeto).</span><span class="sxs-lookup"><span data-stu-id="5a9a1-202">Open your application's Startup.cs class (that you either created manually as per the package instructions in `readme.txt`, or that was created by adding authentication to your project).</span></span> <span data-ttu-id="5a9a1-203">No `Startup.Configuration` método, criar o contêiner Ninject que Ninject chama o *kernel*.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-203">In the `Startup.Configuration` method, create the Ninject container, which Ninject calls the *kernel*.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

<span data-ttu-id="5a9a1-204">Crie uma instância do nosso resolvedor de dependência personalizada:</span><span class="sxs-lookup"><span data-stu-id="5a9a1-204">Create an instance of our custom dependency resolver:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

<span data-ttu-id="5a9a1-205">Crie uma associação para `IStockTicker` da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="5a9a1-205">Create a binding for `IStockTicker` as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample17.cs)]

<span data-ttu-id="5a9a1-206">Esse código está dizendo duas coisas.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-206">This code is saying two things.</span></span> <span data-ttu-id="5a9a1-207">Primeiro, sempre que o aplicativo precisa de um `IStockTicker`, o kernel deve criar uma instância de `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-207">First, whenever the application needs an `IStockTicker`, the kernel should create an instance of `StockTicker`.</span></span> <span data-ttu-id="5a9a1-208">Segundo, a `StockTicker` classe deve ser criado como um objeto de singleton.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-208">Second, the `StockTicker` class should be a created as a singleton object.</span></span> <span data-ttu-id="5a9a1-209">Ninject criará uma instância do objeto e retornar a mesma instância para cada solicitação.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-209">Ninject will create one instance of the object, and return the same instance for each request.</span></span>

<span data-ttu-id="5a9a1-210">Crie uma associação para **IHubConnectionContext** da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="5a9a1-210">Create a binding for **IHubConnectionContext** as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

<span data-ttu-id="5a9a1-211">Esse código creatres uma função anônima que retorna um **IHubConnection**.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-211">This code creatres an anonymous function that returns an **IHubConnection**.</span></span> <span data-ttu-id="5a9a1-212">O **WhenInjectedInto** Ninject para usar essa função somente durante a criação do método informa `IStockTicker` instâncias.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-212">The **WhenInjectedInto** method tells Ninject to use this function only when creating `IStockTicker` instances.</span></span> <span data-ttu-id="5a9a1-213">O motivo é que o SignalR cria **IHubConnectionContext** instâncias internamente, e não queremos substituir como SignalR criá-los.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-213">The reason is that SignalR creates **IHubConnectionContext** instances internally, and we don't want to override how SignalR creates them.</span></span> <span data-ttu-id="5a9a1-214">Essa função só se aplica ao nosso `StockTicker` classe.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-214">This function only applies to our `StockTicker` class.</span></span>

<span data-ttu-id="5a9a1-215">Passe o resolvedor de dependência para o **MapSignalR** método adicionando uma configuração de hub:</span><span class="sxs-lookup"><span data-stu-id="5a9a1-215">Pass the dependency resolver into the **MapSignalR** method by adding a hub configuration:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

<span data-ttu-id="5a9a1-216">Atualize o método Startup.ConfigureSignalR na classe de inicialização do exemplo com o novo parâmetro:</span><span class="sxs-lookup"><span data-stu-id="5a9a1-216">Update the Startup.ConfigureSignalR method in the sample's Startup class with the new parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

<span data-ttu-id="5a9a1-217">Agora o SignalR usará o resolvedor especificado em **MapSignalR**, em vez do resolvedor padrão.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-217">Now SignalR will use the resolver specified in **MapSignalR**, instead of the default resolver.</span></span>

<span data-ttu-id="5a9a1-218">Aqui está o código completo lista para `Startup.Configuration`.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-218">Here is the complete code listing for `Startup.Configuration`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample21.cs)]

<span data-ttu-id="5a9a1-219">Para executar o aplicativo StockTicker no Visual Studio, pressione F5.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-219">To run the StockTicker application in Visual Studio, press F5.</span></span> <span data-ttu-id="5a9a1-220">Na janela do navegador, navegue até `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-220">In the browser window, navigate to `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span></span>

![](dependency-injection/_static/image3.png)

<span data-ttu-id="5a9a1-221">O aplicativo tem a mesma funcionalidade.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-221">The application has exactly the same functionality as before.</span></span> <span data-ttu-id="5a9a1-222">(Para obter uma descrição, consulte [de difusão de servidor com ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).) Nós não alterou o comportamento; acabou de criar o código mais fácil de testar, manter e evoluir.</span><span class="sxs-lookup"><span data-stu-id="5a9a1-222">(For a description, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).) We haven't changed the behavior; just made the code easier to test, maintain, and evolve.</span></span>
