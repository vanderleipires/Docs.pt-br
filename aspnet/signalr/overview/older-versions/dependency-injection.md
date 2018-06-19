---
uid: signalr/overview/older-versions/dependency-injection
title: Injeção de dependência no SignalR 1. x | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/15/2013
ms.topic: article
ms.assetid: eaa206c4-edb3-487e-8fcb-54a3261fed36
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 6fd155adc9a0aa71b66db7a51062a51fb0c1feca
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26505485"
---
<a name="dependency-injection-in-signalr-1x"></a><span data-ttu-id="c717d-102">Injeção de dependência no SignalR 1. x</span><span class="sxs-lookup"><span data-stu-id="c717d-102">Dependency Injection in SignalR 1.x</span></span>
====================
<span data-ttu-id="c717d-103">por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="c717d-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="c717d-104">Injeção de dependência é uma maneira de remover embutido dependências entre objetos, tornando mais fácil para substituir as dependências de um objeto, para testar (usando objetos fictícios) ou para alterar o comportamento de tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="c717d-104">Dependency injection is a way to remove hard-coded dependencies between objects, making it easier to replace an object's dependencies, either for testing (using mock objects) or to change run-time behavior.</span></span> <span data-ttu-id="c717d-105">Este tutorial mostra como realizar injeção de dependência em hubs de SignalR.</span><span class="sxs-lookup"><span data-stu-id="c717d-105">This tutorial shows how to perform dependency injection on SignalR hubs.</span></span> <span data-ttu-id="c717d-106">Ele também mostra como usar contêineres IoC com SignalR.</span><span class="sxs-lookup"><span data-stu-id="c717d-106">It also shows how to use IoC containers with SignalR.</span></span> <span data-ttu-id="c717d-107">Um contêiner IoC é uma estrutura geral para a injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="c717d-107">An IoC container is a general framework for dependency injection.</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="c717d-108">O que é a injeção de dependência?</span><span class="sxs-lookup"><span data-stu-id="c717d-108">What is Dependency Injection?</span></span>

<span data-ttu-id="c717d-109">Se você já estiver familiarizado com a injeção de dependência, ignore esta seção.</span><span class="sxs-lookup"><span data-stu-id="c717d-109">Skip this section if you are already familiar with dependency injection.</span></span>

<span data-ttu-id="c717d-110">*Injeção de dependência* (DI) é um padrão onde os objetos não são responsáveis por criar suas próprias dependências.</span><span class="sxs-lookup"><span data-stu-id="c717d-110">*Dependency injection* (DI) is a pattern where objects are not responsible for creating their own dependencies.</span></span> <span data-ttu-id="c717d-111">Aqui está um exemplo simples motivar DI.</span><span class="sxs-lookup"><span data-stu-id="c717d-111">Here is a simple example to motivate DI.</span></span> <span data-ttu-id="c717d-112">Suponha que você tem um objeto que precisa para as mensagens de log.</span><span class="sxs-lookup"><span data-stu-id="c717d-112">Suppose you have an object that needs to log messages.</span></span> <span data-ttu-id="c717d-113">Você pode definir uma interface de log:</span><span class="sxs-lookup"><span data-stu-id="c717d-113">You might define a logging interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="c717d-114">Em seu objeto, você pode criar um `ILogger` para as mensagens de log:</span><span class="sxs-lookup"><span data-stu-id="c717d-114">In your object, you can create an `ILogger` to log messages:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="c717d-115">Isso funciona, mas não é o melhor design.</span><span class="sxs-lookup"><span data-stu-id="c717d-115">This works, but it's not the best design.</span></span> <span data-ttu-id="c717d-116">Se você quiser substituir `FileLogger` com outra `ILogger` implementação, você precisará modificar `SomeComponent`.</span><span class="sxs-lookup"><span data-stu-id="c717d-116">If you want to replace `FileLogger` with another `ILogger` implementation, you will have to modify `SomeComponent`.</span></span> <span data-ttu-id="c717d-117">Suponhamos que muitos outros objetos `FileLogger`, você precisará alterar todos eles.</span><span class="sxs-lookup"><span data-stu-id="c717d-117">Supposing that a lot of other objects use `FileLogger`, you will need to change all of them.</span></span> <span data-ttu-id="c717d-118">Ou se você decidir fazer `FileLogger` um singleton, você também precisará fazer alterações em todo o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c717d-118">Or if you decide to make `FileLogger` a singleton, you'll also need to make changes throughout the application.</span></span>

<span data-ttu-id="c717d-119">Uma abordagem melhor é "inserir" um `ILogger` no objeto — por exemplo, usando um argumento de construtor:</span><span class="sxs-lookup"><span data-stu-id="c717d-119">A better approach is to "inject" an `ILogger` into the object—for example, by using a constructor argument:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="c717d-120">Agora que o objeto não é responsável por selecionar qual `ILogger` para usar.</span><span class="sxs-lookup"><span data-stu-id="c717d-120">Now the object is not responsible for selecting which `ILogger` to use.</span></span> <span data-ttu-id="c717d-121">Você pode alternar `ILogger` implementações sem alterar os objetos que dependem dele.</span><span class="sxs-lookup"><span data-stu-id="c717d-121">You can swich `ILogger` implementations without changing the objects that depend on it.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="c717d-122">Esse padrão é chamado [injeção de construtor](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="c717d-122">This pattern is called [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="c717d-123">Outro padrão é a inclusão de setter, em que você definir a dependência por meio de uma propriedade ou método de setter.</span><span class="sxs-lookup"><span data-stu-id="c717d-123">Another pattern is setter injection, where you set the dependency through a setter method or property.</span></span>

## <a name="simple-dependency-injection-in-signalr"></a><span data-ttu-id="c717d-124">Injeção de dependência simples no SignalR</span><span class="sxs-lookup"><span data-stu-id="c717d-124">Simple Dependency Injection in SignalR</span></span>

<span data-ttu-id="c717d-125">Considere a aplicação de Chat do tutorial [guia de Introdução ao SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="c717d-125">Consider the Chat application from the tutorial [Getting Started with SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="c717d-126">Aqui está a classe de hub do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="c717d-126">Here is the hub class from that application:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="c717d-127">Suponha que você deseja armazenar mensagens de chat no servidor antes de enviá-los.</span><span class="sxs-lookup"><span data-stu-id="c717d-127">Suppose that you want to store chat messages on the server before sending them.</span></span> <span data-ttu-id="c717d-128">Você pode definir uma interface que abstrai essa funcionalidade e usar a DI injetar a interface para o `ChatHub` classe.</span><span class="sxs-lookup"><span data-stu-id="c717d-128">You might define an interface that abstracts this functionality, and use DI to inject the interface into the `ChatHub` class.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="c717d-129">O único problema é que um aplicativo SignalR não cria diretamente hubs; SignalR criará para você.</span><span class="sxs-lookup"><span data-stu-id="c717d-129">The only problem is that a SignalR application does not directly create hubs; SignalR creates them for you.</span></span> <span data-ttu-id="c717d-130">Por padrão, o SignalR espera uma classe de hub para ter um construtor sem parâmetros.</span><span class="sxs-lookup"><span data-stu-id="c717d-130">By default, SignalR expects a hub class to have a parameterless constructor.</span></span> <span data-ttu-id="c717d-131">No entanto, você pode facilmente registrar uma função para criar instâncias de hub e usar essa função para executar a injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="c717d-131">However, you can easily register a function to create hub instances, and use this function to perform DI.</span></span> <span data-ttu-id="c717d-132">Registre a função chamando **GlobalHost.DependencyResolver.Register**.</span><span class="sxs-lookup"><span data-stu-id="c717d-132">Register the function by calling **GlobalHost.DependencyResolver.Register**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

<span data-ttu-id="c717d-133">Agora o SignalR chamará essa função anônima sempre que precisar criar um `ChatHub` instância.</span><span class="sxs-lookup"><span data-stu-id="c717d-133">Now SignalR will invoke this anonymous function whenever it needs to create a `ChatHub` instance.</span></span>

## <a name="ioc-containers"></a><span data-ttu-id="c717d-134">Contêineres IoC</span><span class="sxs-lookup"><span data-stu-id="c717d-134">IoC Containers</span></span>

<span data-ttu-id="c717d-135">O código anterior é bom para casos simples.</span><span class="sxs-lookup"><span data-stu-id="c717d-135">The previous code is fine for simple cases.</span></span> <span data-ttu-id="c717d-136">Mas ainda era necessário escrever isso:</span><span class="sxs-lookup"><span data-stu-id="c717d-136">But you still had to write this:</span></span>

[!code-css[Main](dependency-injection/samples/sample8.css)]

<span data-ttu-id="c717d-137">Em um aplicativo complexo com várias dependências, você precisará escrever muito do código "fiação".</span><span class="sxs-lookup"><span data-stu-id="c717d-137">In a complex application with many dependencies, you might need to write a lot of this "wiring" code.</span></span> <span data-ttu-id="c717d-138">Esse código pode ser difícil de manter, especialmente se as dependências são aninhadas.</span><span class="sxs-lookup"><span data-stu-id="c717d-138">This code can be hard to maintain, especially if dependencies are nested.</span></span> <span data-ttu-id="c717d-139">Também é difícil de teste de unidade.</span><span class="sxs-lookup"><span data-stu-id="c717d-139">It is also hard to unit test.</span></span>

<span data-ttu-id="c717d-140">Uma solução é usar um contêiner IoC.</span><span class="sxs-lookup"><span data-stu-id="c717d-140">One solution is to use an IoC container.</span></span> <span data-ttu-id="c717d-141">Um contêiner IoC é um componente de software que é responsável por gerenciar as dependências. Você registrar tipos com o contêiner e, em seguida, usa o contêiner para criar objetos.</span><span class="sxs-lookup"><span data-stu-id="c717d-141">An IoC container is a software component that is responsible for managing dependencies.You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="c717d-142">O contêiner automaticamente descobre as relações de dependência.</span><span class="sxs-lookup"><span data-stu-id="c717d-142">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="c717d-143">Vários contêineres IoC também permitem que você controle como escopo e tempo de vida do objeto.</span><span class="sxs-lookup"><span data-stu-id="c717d-143">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="c717d-144">"IoC" significa "inversão de controle", que é um padrão geral em que uma estrutura chama o código do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c717d-144">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="c717d-145">Um contêiner IoC constrói os objetos para você, o que "inverte" o fluxo normal de controle.</span><span class="sxs-lookup"><span data-stu-id="c717d-145">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


## <a name="using-ioc-containers-in-signalr"></a><span data-ttu-id="c717d-146">Usando contêineres IoC no SignalR</span><span class="sxs-lookup"><span data-stu-id="c717d-146">Using IoC Containers in SignalR</span></span>

<span data-ttu-id="c717d-147">O aplicativo de Chat provavelmente é muito simple para se beneficiar de um contêiner IoC.</span><span class="sxs-lookup"><span data-stu-id="c717d-147">The Chat application is probably too simple to benefit from an IoC container.</span></span> <span data-ttu-id="c717d-148">Em vez disso, vamos analisar o [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) exemplo.</span><span class="sxs-lookup"><span data-stu-id="c717d-148">Instead, let's look at the [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) sample.</span></span>

<span data-ttu-id="c717d-149">O exemplo StockTicker define duas classes principais:</span><span class="sxs-lookup"><span data-stu-id="c717d-149">The StockTicker sample defines two main classes:</span></span>

- <span data-ttu-id="c717d-150">`StockTickerHub`: A classe de hub, que gerencia as conexões de cliente.</span><span class="sxs-lookup"><span data-stu-id="c717d-150">`StockTickerHub`: The hub class, which manages client connections.</span></span>
- <span data-ttu-id="c717d-151">`StockTicker`: Um singleton que contém os preços de estoque e os atualiza periodicamente.</span><span class="sxs-lookup"><span data-stu-id="c717d-151">`StockTicker`: A singleton that holds stock prices and periodically updates them.</span></span>

<span data-ttu-id="c717d-152">`StockTickerHub`contém uma referência para o `StockTicker` singleton, enquanto `StockTicker` contém uma referência para o **IHubConnectionContext** para o `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="c717d-152">`StockTickerHub` holds a reference to the `StockTicker` singleton, while `StockTicker` holds a reference to the **IHubConnectionContext** for the `StockTickerHub`.</span></span> <span data-ttu-id="c717d-153">Ele usa essa interface para se comunicar com `StockTickerHub` instâncias.</span><span class="sxs-lookup"><span data-stu-id="c717d-153">It uses this interface to communicate with `StockTickerHub` instances.</span></span> <span data-ttu-id="c717d-154">(Para obter mais informações, consulte [de difusão de servidor com ASP.NET SignalR](index.md).)</span><span class="sxs-lookup"><span data-stu-id="c717d-154">(For more information, see [Server Broadcast with ASP.NET SignalR](index.md).)</span></span>

<span data-ttu-id="c717d-155">Podemos usar um contêiner IoC para simplificar os essas dependências um pouco.</span><span class="sxs-lookup"><span data-stu-id="c717d-155">We can use an IoC container to untangle these dependencies a bit.</span></span> <span data-ttu-id="c717d-156">Primeiro, vamos simplificar o `StockTickerHub` e `StockTicker` classes.</span><span class="sxs-lookup"><span data-stu-id="c717d-156">First, let's simplify the `StockTickerHub` and `StockTicker` classes.</span></span> <span data-ttu-id="c717d-157">No código a seguir, eu já comentadas as partes que não é necessário.</span><span class="sxs-lookup"><span data-stu-id="c717d-157">In the following code, I've commented out the parts that we don't need.</span></span>

<span data-ttu-id="c717d-158">Remover o construtor sem parâmetros de `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="c717d-158">Remove the parameterless constructor from `StockTicker`.</span></span> <span data-ttu-id="c717d-159">Em vez disso, nós usaremos sempre DI para criar o hub.</span><span class="sxs-lookup"><span data-stu-id="c717d-159">Instead, we will always use DI to create the hub.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

<span data-ttu-id="c717d-160">Para StockTicker, remova a instância singleton.</span><span class="sxs-lookup"><span data-stu-id="c717d-160">For StockTicker, remove the singleton instance.</span></span> <span data-ttu-id="c717d-161">Posteriormente, usaremos o contêiner IoC para controlar o tempo de vida StockTicker.</span><span class="sxs-lookup"><span data-stu-id="c717d-161">Later, we'll use the IoC container to control the StockTicker lifetime.</span></span> <span data-ttu-id="c717d-162">Além disso, verifique o construtor público.</span><span class="sxs-lookup"><span data-stu-id="c717d-162">Also, make the constructor public.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

<span data-ttu-id="c717d-163">Em seguida, é possível refatorar o código, criando uma interface para `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="c717d-163">Next, we can refactor the code by creating an interface for `StockTicker`.</span></span> <span data-ttu-id="c717d-164">Vamos usar essa interface para desacoplar a `StockTickerHub` do `StockTicker` classe.</span><span class="sxs-lookup"><span data-stu-id="c717d-164">We'll use this interface to decouple the `StockTickerHub` from the `StockTicker` class.</span></span>

<span data-ttu-id="c717d-165">O Visual Studio faz esse tipo de refatoração fácil.</span><span class="sxs-lookup"><span data-stu-id="c717d-165">Visual Studio makes this kind of refactoring easy.</span></span> <span data-ttu-id="c717d-166">Abra o arquivo StockTicker.cs, clique duas vezes no `StockTicker` declaração de classe e, em seguida, selecione **refatorar** ... **Extrair Interface**.</span><span class="sxs-lookup"><span data-stu-id="c717d-166">Open the file StockTicker.cs, right-click on the `StockTicker` class declaration, and select **Refactor** ... **Extract Interface**.</span></span>

![](dependency-injection/_static/image1.png)

<span data-ttu-id="c717d-167">No **extrair Interface** caixa de diálogo, clique em **Selecionar tudo**.</span><span class="sxs-lookup"><span data-stu-id="c717d-167">In the **Extract Interface** dialog, click **Select All**.</span></span> <span data-ttu-id="c717d-168">Deixe os outros padrões.</span><span class="sxs-lookup"><span data-stu-id="c717d-168">Leave the other defaults.</span></span> <span data-ttu-id="c717d-169">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="c717d-169">Click **OK**.</span></span>

![](dependency-injection/_static/image2.png)

<span data-ttu-id="c717d-170">O Visual Studio cria uma nova interface chamada `IStockTicker`e também o altera `StockTicker` derivar `IStockTicker`.</span><span class="sxs-lookup"><span data-stu-id="c717d-170">Visual Studio creates a new interface named `IStockTicker`, and also changes `StockTicker` to derive from `IStockTicker`.</span></span>

<span data-ttu-id="c717d-171">Abra o arquivo IStockTicker.cs e alterar a interface para **público**.</span><span class="sxs-lookup"><span data-stu-id="c717d-171">Open the file IStockTicker.cs and change the interface to **public**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

<span data-ttu-id="c717d-172">No `StockTickerHub` classe, altere as duas instâncias de `StockTicker` para `IStockTicker`:</span><span class="sxs-lookup"><span data-stu-id="c717d-172">In the `StockTickerHub` class, change the two instances of `StockTicker` to `IStockTicker`:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

<span data-ttu-id="c717d-173">Criando um `IStockTicker` interface não é estritamente necessária, mas gostaria de mostrar como a injeção de dependência pode ajudar a reduzir o acoplamento flexível entre os componentes de seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c717d-173">Creating an `IStockTicker` interface isn't strictly necessary, but I wanted to show how DI can help to reduce coupling between components in your application.</span></span>

## <a name="add-the-ninject-library"></a><span data-ttu-id="c717d-174">Adicionar a biblioteca de Ninject</span><span class="sxs-lookup"><span data-stu-id="c717d-174">Add the Ninject Library</span></span>

<span data-ttu-id="c717d-175">Há vários contêineres IoC de código-fonte aberto para .NET.</span><span class="sxs-lookup"><span data-stu-id="c717d-175">There are many open-source IoC containers for .NET.</span></span> <span data-ttu-id="c717d-176">Para este tutorial, usarei [Ninject](http://www.ninject.org/).</span><span class="sxs-lookup"><span data-stu-id="c717d-176">For this tutorial, I'll use [Ninject](http://www.ninject.org/).</span></span> <span data-ttu-id="c717d-177">(Outras bibliotecas populares incluem [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), e [StructureMap ](http://docs.structuremap.net).)</span><span class="sxs-lookup"><span data-stu-id="c717d-177">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), and [StructureMap](http://docs.structuremap.net).)</span></span>

<span data-ttu-id="c717d-178">Use o Gerenciador de pacote de NuGet para instalar o [Ninject biblioteca](https://nuget.org/packages/Ninject/3.0.1.10).</span><span class="sxs-lookup"><span data-stu-id="c717d-178">Use NuGet Package Manager to install the [Ninject library](https://nuget.org/packages/Ninject/3.0.1.10).</span></span> <span data-ttu-id="c717d-179">No Visual Studio, do **ferramentas** menu, selecione **Gerenciador de biblioteca de pacote** | **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="c717d-179">In Visual Studio, from the **Tools** menu select **Library Package Manager** | **Package Manager Console**.</span></span> <span data-ttu-id="c717d-180">Na janela do Console do Gerenciador de pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="c717d-180">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a><span data-ttu-id="c717d-181">Substitua o resolvedor de dependência de SignalR</span><span class="sxs-lookup"><span data-stu-id="c717d-181">Replace the SignalR Dependency Resolver</span></span>

<span data-ttu-id="c717d-182">Para usar Ninject no SignalR, crie uma classe que deriva de **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="c717d-182">To use Ninject within SignalR, create a class that derives from **DefaultDependencyResolver**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

<span data-ttu-id="c717d-183">Esta classe substitui o **GetService** e **GetServices** métodos de **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="c717d-183">This class overrides the **GetService** and **GetServices** methods of **DefaultDependencyResolver**.</span></span> <span data-ttu-id="c717d-184">SignalR chama esses métodos para criar vários objetos em tempo de execução, incluindo instâncias do hub, bem como vários serviços usados internamente pelo SignalR.</span><span class="sxs-lookup"><span data-stu-id="c717d-184">SignalR calls these methods to create various objects at runtime, including hub instances, as well as various services used internally by SignalR.</span></span>

- <span data-ttu-id="c717d-185">O **GetService** método cria uma única instância de um tipo.</span><span class="sxs-lookup"><span data-stu-id="c717d-185">The **GetService** method creates a single instance of a type.</span></span> <span data-ttu-id="c717d-186">Substitua este método para chamar o kernel de Ninject **TryGet** método.</span><span class="sxs-lookup"><span data-stu-id="c717d-186">Override this method to call the Ninject kernel's **TryGet** method.</span></span> <span data-ttu-id="c717d-187">Se esse método retornará nulo, voltar para o resolvedor padrão.</span><span class="sxs-lookup"><span data-stu-id="c717d-187">If that method returns null, fall back to the default resolver.</span></span>
- <span data-ttu-id="c717d-188">O **GetServices** método cria uma coleção de objetos de um tipo especificado.</span><span class="sxs-lookup"><span data-stu-id="c717d-188">The **GetServices** method creates a collection of objects of a specified type.</span></span> <span data-ttu-id="c717d-189">Substitua este método para concatenar os resultados de Ninject com os resultados do resolvedor padrão.</span><span class="sxs-lookup"><span data-stu-id="c717d-189">Override this method to concatenate the results from Ninject with the results from the default resolver.</span></span>

## <a name="configure-ninject-bindings"></a><span data-ttu-id="c717d-190">Configurar as associações de Ninject</span><span class="sxs-lookup"><span data-stu-id="c717d-190">Configure Ninject Bindings</span></span>

<span data-ttu-id="c717d-191">Agora vamos usar Ninject para declarar associações de tipo.</span><span class="sxs-lookup"><span data-stu-id="c717d-191">Now we'll use Ninject to declare type bindings.</span></span>

<span data-ttu-id="c717d-192">Abra o arquivo RegisterHubs.cs.</span><span class="sxs-lookup"><span data-stu-id="c717d-192">Open the file RegisterHubs.cs.</span></span> <span data-ttu-id="c717d-193">No `RegisterHubs.Start` método, criar o contêiner Ninject que Ninject chama o *kernel*.</span><span class="sxs-lookup"><span data-stu-id="c717d-193">In the `RegisterHubs.Start` method, create the Ninject container, which Ninject calls the *kernel*.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

<span data-ttu-id="c717d-194">Crie uma instância do nosso resolvedor de dependência personalizada:</span><span class="sxs-lookup"><span data-stu-id="c717d-194">Create an instance of our custom dependency resolver:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

<span data-ttu-id="c717d-195">Crie uma associação para `IStockTicker` da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="c717d-195">Create a binding for `IStockTicker` as follows:</span></span>

[!code-html[Main](dependency-injection/samples/sample17.html)]

<span data-ttu-id="c717d-196">Esse código está dizendo duas coisas.</span><span class="sxs-lookup"><span data-stu-id="c717d-196">This code is saying two things.</span></span> <span data-ttu-id="c717d-197">Primeiro, sempre que o aplicativo precisa de um `IStockTicker`, o kernel deve criar uma instância de `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="c717d-197">First, whenever the application needs an `IStockTicker`, the kernel should create an instance of `StockTicker`.</span></span> <span data-ttu-id="c717d-198">Segundo, a `StockTicker` classe deve ser criado como um objeto de singleton.</span><span class="sxs-lookup"><span data-stu-id="c717d-198">Second, the `StockTicker` class should be a created as a singleton object.</span></span> <span data-ttu-id="c717d-199">Ninject criará uma instância do objeto e retornar a mesma instância para cada solicitação.</span><span class="sxs-lookup"><span data-stu-id="c717d-199">Ninject will create one instance of the object, and return the same instance for each request.</span></span>

<span data-ttu-id="c717d-200">Crie uma associação para **IHubConnectionContext** da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="c717d-200">Create a binding for **IHubConnectionContext** as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

<span data-ttu-id="c717d-201">Esse código creatres uma função anônima que retorna um **IHubConnection**.</span><span class="sxs-lookup"><span data-stu-id="c717d-201">This code creatres an anonymous function that returns an **IHubConnection**.</span></span> <span data-ttu-id="c717d-202">O **WhenInjectedInto** Ninject para usar essa função somente durante a criação do método informa `IStockTicker` instâncias.</span><span class="sxs-lookup"><span data-stu-id="c717d-202">The **WhenInjectedInto** method tells Ninject to use this function only when creating `IStockTicker` instances.</span></span> <span data-ttu-id="c717d-203">O motivo é que o SignalR cria **IHubConnectionContext** instâncias internamente, e não queremos substituir como SignalR criá-los.</span><span class="sxs-lookup"><span data-stu-id="c717d-203">The reason is that SignalR creates **IHubConnectionContext** instances internally, and we don't want to override how SignalR creates them.</span></span> <span data-ttu-id="c717d-204">Essa função só se aplica ao nosso `StockTicker` classe.</span><span class="sxs-lookup"><span data-stu-id="c717d-204">This function only applies to our `StockTicker` class.</span></span>

<span data-ttu-id="c717d-205">Passe o resolvedor de dependência para o **MapHubs** método:</span><span class="sxs-lookup"><span data-stu-id="c717d-205">Pass the dependency resolver into the **MapHubs** method:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

<span data-ttu-id="c717d-206">Agora o SignalR usará o resolvedor especificado em **MapHubs**, em vez do resolvedor padrão.</span><span class="sxs-lookup"><span data-stu-id="c717d-206">Now SignalR will use the resolver specified in **MapHubs**, instead of the default resolver.</span></span>

<span data-ttu-id="c717d-207">Aqui está o código completo lista para `RegisterHubs.Start`.</span><span class="sxs-lookup"><span data-stu-id="c717d-207">Here is the complete code listing for `RegisterHubs.Start`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

<span data-ttu-id="c717d-208">Para executar o aplicativo StockTicker no Visual Studio, pressione F5.</span><span class="sxs-lookup"><span data-stu-id="c717d-208">To run the StockTicker application in Visual Studio, press F5.</span></span> <span data-ttu-id="c717d-209">Na janela do navegador, navegue até `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span><span class="sxs-lookup"><span data-stu-id="c717d-209">In the browser window, navigate to `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span></span>

![](dependency-injection/_static/image3.png)

<span data-ttu-id="c717d-210">O aplicativo tem a mesma funcionalidade.</span><span class="sxs-lookup"><span data-stu-id="c717d-210">The application has exactly the same functionality as before.</span></span> <span data-ttu-id="c717d-211">(Para obter uma descrição, consulte [de difusão de servidor com ASP.NET SignalR](index.md).) Nós não alterou o comportamento; acabou de criar o código mais fácil de testar, manter e evoluir.</span><span class="sxs-lookup"><span data-stu-id="c717d-211">(For a description, see [Server Broadcast with ASP.NET SignalR](index.md).) We haven't changed the behavior; just made the code easier to test, maintain, and evolve.</span></span>
