---
title: "Desenvolvendo aplicativos ASP.NET Core usando a inspeção dotnet"
author: rick-anderson
description: Este tutorial demonstra como instalar e usar a ferramenta de inspetor de arquivo (dotnet watch) da CLI do .NET Core em um aplicativo do ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/05/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/dotnet-watch
ms.openlocfilehash: cb15e28cb98ea82091cf5ddeed12df8926079e52
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="developing-aspnet-core-apps-using-dotnet-watch"></a><span data-ttu-id="072fb-103">Desenvolvendo aplicativos ASP.NET Core usando a inspeção dotnet</span><span class="sxs-lookup"><span data-stu-id="072fb-103">Developing ASP.NET Core apps using dotnet watch</span></span>

<span data-ttu-id="072fb-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span><span class="sxs-lookup"><span data-stu-id="072fb-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span></span>

<span data-ttu-id="072fb-105">`dotnet watch` é uma ferramenta que executa um comando do [CLI do .NET Core](/dotnet/core/tools) quando os arquivos de origem são alterados.</span><span class="sxs-lookup"><span data-stu-id="072fb-105">`dotnet watch` is a tool that runs a [.NET Core CLI](/dotnet/core/tools) command when source files change.</span></span> <span data-ttu-id="072fb-106">Por exemplo, uma alteração de arquivo pode disparar uma compilação, execução de teste ou uma implantação.</span><span class="sxs-lookup"><span data-stu-id="072fb-106">For example, a file change can trigger compilation, test execution, or deployment.</span></span>

<span data-ttu-id="072fb-107">Neste tutorial, usamos um aplicativo de API Web existente com dois pontos de extremidade: um que retorna uma soma e outro que retorna um produto.</span><span class="sxs-lookup"><span data-stu-id="072fb-107">In this tutorial, we use an existing Web API app with two endpoints: one that returns a sum and one that returns a product.</span></span> <span data-ttu-id="072fb-108">O método de produto contém um bug que corrigiremos como parte deste tutorial.</span><span class="sxs-lookup"><span data-stu-id="072fb-108">The product method contains a bug that we'll fix as part of this tutorial.</span></span>

<span data-ttu-id="072fb-109">Baixe o [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span><span class="sxs-lookup"><span data-stu-id="072fb-109">Download the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span></span> <span data-ttu-id="072fb-110">Ele contém dois projetos: *WebApp* (uma API do ASP.NET Core Web) e *WebAppTests* (testes de unidade para a API da Web).</span><span class="sxs-lookup"><span data-stu-id="072fb-110">It contains two projects: *WebApp* (an ASP.NET Core Web API) and *WebAppTests* (unit tests for the Web API).</span></span>

<span data-ttu-id="072fb-111">Em um shell de comando, navegue para a pasta *WebApp* e execute os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="072fb-111">In a command shell, navigate to the *WebApp* folder and run the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="072fb-112">O resultado do console mostra mensagens semelhantes à seguinte (indicando que o aplicativo está em execução e aguarda solicitações):</span><span class="sxs-lookup"><span data-stu-id="072fb-112">The console output shows messages similar to the following (indicating that the app is running and awaiting requests):</span></span>

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="072fb-113">Em um navegador da Web, navegue até `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span><span class="sxs-lookup"><span data-stu-id="072fb-113">In a web browser, navigate to `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span></span> <span data-ttu-id="072fb-114">Você deve ver o resultado de `9`.</span><span class="sxs-lookup"><span data-stu-id="072fb-114">You should see the result of `9`.</span></span>

<span data-ttu-id="072fb-115">Navegue para o API do produto (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span><span class="sxs-lookup"><span data-stu-id="072fb-115">Navigate to the product API (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span></span> <span data-ttu-id="072fb-116">Ele retorna `9`, não `20`, conforme o esperado.</span><span class="sxs-lookup"><span data-stu-id="072fb-116">It returns `9`, not `20` as you'd expect.</span></span> <span data-ttu-id="072fb-117">Corrigiremos isso mais adiante no tutorial.</span><span class="sxs-lookup"><span data-stu-id="072fb-117">We'll fix that later in the tutorial.</span></span>

## <a name="add-dotnet-watch-to-a-project"></a><span data-ttu-id="072fb-118">Adicionar `dotnet watch` a um projeto</span><span class="sxs-lookup"><span data-stu-id="072fb-118">Add `dotnet watch` to a project</span></span>

1. <span data-ttu-id="072fb-119">Adicionar uma referência ao pacote de `Microsoft.DotNet.Watcher.Tools` para o arquivo *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="072fb-119">Add a `Microsoft.DotNet.Watcher.Tools` package reference to the *.csproj* file:</span></span>

    ```xml
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
    </ItemGroup> 
    ```

1. <span data-ttu-id="072fb-120">Instale o pacote `Microsoft.DotNet.Watcher.Tools` executando o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="072fb-120">Install the `Microsoft.DotNet.Watcher.Tools` package by running the following command:</span></span>
    
    ```console
    dotnet restore
    ```

## <a name="running-net-core-cli-commands-using-dotnet-watch"></a><span data-ttu-id="072fb-121">Executando comandos da CLI do .NET Core usando `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="072fb-121">Running .NET Core CLI commands using `dotnet watch`</span></span>

<span data-ttu-id="072fb-122">Qualquer [comando da CLI do .NET Core](/dotnet/core/tools#cli-commands) pode ser executado com `dotnet watch`.</span><span class="sxs-lookup"><span data-stu-id="072fb-122">Any [.NET Core CLI command](/dotnet/core/tools#cli-commands) can be run with `dotnet watch`.</span></span> <span data-ttu-id="072fb-123">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="072fb-123">For example:</span></span>

| <span data-ttu-id="072fb-124">Comando</span><span class="sxs-lookup"><span data-stu-id="072fb-124">Command</span></span> | <span data-ttu-id="072fb-125">Comando com inspeção</span><span class="sxs-lookup"><span data-stu-id="072fb-125">Command with watch</span></span> |
| ---- | ----- |
| <span data-ttu-id="072fb-126">dotnet run</span><span class="sxs-lookup"><span data-stu-id="072fb-126">dotnet run</span></span> | <span data-ttu-id="072fb-127">dotnet watch run</span><span class="sxs-lookup"><span data-stu-id="072fb-127">dotnet watch run</span></span> |
| <span data-ttu-id="072fb-128">dotnet run -f netcoreapp2.0</span><span class="sxs-lookup"><span data-stu-id="072fb-128">dotnet run -f netcoreapp2.0</span></span> | <span data-ttu-id="072fb-129">dotnet watch run -f netcoreapp2.0</span><span class="sxs-lookup"><span data-stu-id="072fb-129">dotnet watch run -f netcoreapp2.0</span></span> |
| <span data-ttu-id="072fb-130">dotnet run -f netcoreapp2.0 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="072fb-130">dotnet run -f netcoreapp2.0 -- --arg1</span></span> | <span data-ttu-id="072fb-131">dotnet watch run -f netcoreapp2.0 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="072fb-131">dotnet watch run -f netcoreapp2.0 -- --arg1</span></span> |
| <span data-ttu-id="072fb-132">dotnet test</span><span class="sxs-lookup"><span data-stu-id="072fb-132">dotnet test</span></span> | <span data-ttu-id="072fb-133">dotnet watch test</span><span class="sxs-lookup"><span data-stu-id="072fb-133">dotnet watch test</span></span> |

<span data-ttu-id="072fb-134">Executar `dotnet watch run` na pasta *WebApp*.</span><span class="sxs-lookup"><span data-stu-id="072fb-134">Run `dotnet watch run` in the *WebApp* folder.</span></span> <span data-ttu-id="072fb-135">O resultado do console indica que `watch` foi iniciado.</span><span class="sxs-lookup"><span data-stu-id="072fb-135">The console output indicates `watch` has started.</span></span>

## <a name="making-changes-with-dotnet-watch"></a><span data-ttu-id="072fb-136">Fazer alterações com `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="072fb-136">Making changes with `dotnet watch`</span></span>

<span data-ttu-id="072fb-137">Verifique se `dotnet watch` está em execução.</span><span class="sxs-lookup"><span data-stu-id="072fb-137">Make sure `dotnet watch` is running.</span></span>

<span data-ttu-id="072fb-138">Corrija o bug no método `Product` do *MathController.cs* para que ele retorne o produto e não a soma:</span><span class="sxs-lookup"><span data-stu-id="072fb-138">Fix the bug in the `Product` method of *MathController.cs* so it returns the product and not the sum:</span></span>

```csharp
public static int Product(int a, int b)
{
  return a * b;
} 
```

<span data-ttu-id="072fb-139">Salve o arquivo.</span><span class="sxs-lookup"><span data-stu-id="072fb-139">Save the file.</span></span> <span data-ttu-id="072fb-140">O resultado do console indica que o `dotnet watch` detectou uma alteração de arquivo e reiniciou o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="072fb-140">The console output indicates that `dotnet watch` detected a file change and restarted the app.</span></span>

<span data-ttu-id="072fb-141">Verifique se `http://localhost:<port number>/api/math/product?a=4&b=5` retorna o resultado correto.</span><span class="sxs-lookup"><span data-stu-id="072fb-141">Verify `http://localhost:<port number>/api/math/product?a=4&b=5` returns the correct result.</span></span>

## <a name="running-tests-using-dotnet-watch"></a><span data-ttu-id="072fb-142">Executando testes usando `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="072fb-142">Running tests using `dotnet watch`</span></span>

1. <span data-ttu-id="072fb-143">Altere o método `Product` de *MathController.cs* novamente para retornar a soma e salve o arquivo.</span><span class="sxs-lookup"><span data-stu-id="072fb-143">Change the `Product` method of *MathController.cs* back to returning the sum and save the file.</span></span>
1. <span data-ttu-id="072fb-144">Em um shell de comando, navegue até a pasta *WebAppTests*.</span><span class="sxs-lookup"><span data-stu-id="072fb-144">In a command shell, navigate to the *WebAppTests* folder.</span></span>
1. <span data-ttu-id="072fb-145">Execute `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="072fb-145">Run `dotnet restore`.</span></span>
1. <span data-ttu-id="072fb-146">Execute `dotnet watch test`.</span><span class="sxs-lookup"><span data-stu-id="072fb-146">Run `dotnet watch test`.</span></span> <span data-ttu-id="072fb-147">Seu resultado indica que um teste falhou e que o inspetor está aguardando as alterações de arquivo:</span><span class="sxs-lookup"><span data-stu-id="072fb-147">Its output indicates that a test failed and that watcher is awaiting file changes:</span></span>

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. <span data-ttu-id="072fb-148">Corrija o código do método `Product` para que ele retorne o produto.</span><span class="sxs-lookup"><span data-stu-id="072fb-148">Fix the `Product` method code so it returns the product.</span></span> <span data-ttu-id="072fb-149">Salve o arquivo.</span><span class="sxs-lookup"><span data-stu-id="072fb-149">Save the file.</span></span>

<span data-ttu-id="072fb-150">`dotnet watch` detecta a alteração de arquivo e executa os testes novamente.</span><span class="sxs-lookup"><span data-stu-id="072fb-150">`dotnet watch` detects the file change and reruns the tests.</span></span> <span data-ttu-id="072fb-151">O resultado do console indica a aprovação nos testes.</span><span class="sxs-lookup"><span data-stu-id="072fb-151">The console output indicates the tests passed.</span></span>

## <a name="dotnet-watch-in-github"></a><span data-ttu-id="072fb-152">dotnet-watch no GitHub</span><span class="sxs-lookup"><span data-stu-id="072fb-152">dotnet-watch in GitHub</span></span>

<span data-ttu-id="072fb-153">dotnet-watch faz parte do [repositório DotNetTools](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch) do GitHub.</span><span class="sxs-lookup"><span data-stu-id="072fb-153">dotnet-watch is part of the GitHub [DotNetTools repository](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch).</span></span>

<span data-ttu-id="072fb-154">A [seção MSBuild](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch#msbuild) do [Leiame do dotnet-watch](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) explica como o dotnet-watch pode ser configurado por meio do arquivo de projeto MSBuild inspecionado.</span><span class="sxs-lookup"><span data-stu-id="072fb-154">The [MSBuild section](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch#msbuild) of the [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) explains how dotnet-watch can be configured from the MSBuild project file being watched.</span></span> <span data-ttu-id="072fb-155">O [Leiame do dotnet-watch](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) contém informações sobre o dotnet-watch não abordadas neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="072fb-155">The [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) contains information on dotnet-watch not covered in this tutorial.</span></span>
