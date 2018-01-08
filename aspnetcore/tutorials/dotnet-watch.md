---
title: "Desenvolvendo aplicativos ASP.NET Core usando a inspeção dotnet"
author: rick-anderson
description: Este tutorial demonstra como instalar e usar a ferramenta de inspetor de arquivo (dotnet watch) da CLI do .NET Core em um aplicativo do ASP.NET Core.
keywords: "ASP.NET Core, usando a inspeção dotnet"
ms.author: riande
manager: wpickett
ms.date: 10/05/2017
ms.topic: article
ms.assetid: 563ffb3f-d369-4aa5-bf0a-7300b4e7832c
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/dotnet-watch
ms.openlocfilehash: 1b26beaa41f4b38e0cfd2c8300cb521a3dcce47d
ms.sourcegitcommit: 281f0c614543a6c3db565ea4655b70fe49b61d84
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/03/2018
---
# <a name="developing-aspnet-core-apps-using-dotnet-watch"></a><span data-ttu-id="a2f4f-104">Desenvolvendo aplicativos ASP.NET Core usando a inspeção dotnet</span><span class="sxs-lookup"><span data-stu-id="a2f4f-104">Developing ASP.NET Core apps using dotnet watch</span></span>

<span data-ttu-id="a2f4f-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span><span class="sxs-lookup"><span data-stu-id="a2f4f-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span></span>

<span data-ttu-id="a2f4f-106">`dotnet watch` é uma ferramenta que executa um comando do [CLI do .NET Core](/dotnet/core/tools) quando os arquivos de origem são alterados.</span><span class="sxs-lookup"><span data-stu-id="a2f4f-106">`dotnet watch` is a tool that runs a [.NET Core CLI](/dotnet/core/tools) command when source files change.</span></span> <span data-ttu-id="a2f4f-107">Por exemplo, uma alteração de arquivo pode disparar uma compilação, execução de teste ou uma implantação.</span><span class="sxs-lookup"><span data-stu-id="a2f4f-107">For example, a file change can trigger compilation, test execution, or deployment.</span></span>

<span data-ttu-id="a2f4f-108">Neste tutorial, usamos um aplicativo de API Web existente com dois pontos de extremidade: um que retorna uma soma e outro que retorna um produto.</span><span class="sxs-lookup"><span data-stu-id="a2f4f-108">In this tutorial, we use an existing Web API app with two endpoints: one that returns a sum and one that returns a product.</span></span> <span data-ttu-id="a2f4f-109">O método de produto contém um bug que corrigiremos como parte deste tutorial.</span><span class="sxs-lookup"><span data-stu-id="a2f4f-109">The product method contains a bug that we'll fix as part of this tutorial.</span></span>

<span data-ttu-id="a2f4f-110">Baixe o [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span><span class="sxs-lookup"><span data-stu-id="a2f4f-110">Download the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span></span> <span data-ttu-id="a2f4f-111">Ele contém dois projetos: *WebApp* (uma API do ASP.NET Core Web) e *WebAppTests* (testes de unidade para a API da Web).</span><span class="sxs-lookup"><span data-stu-id="a2f4f-111">It contains two projects: *WebApp* (an ASP.NET Core Web API) and *WebAppTests* (unit tests for the Web API).</span></span>

<span data-ttu-id="a2f4f-112">Em um shell de comando, navegue para a pasta *WebApp* e execute os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="a2f4f-112">In a command shell, navigate to the *WebApp* folder and run the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="a2f4f-113">O resultado do console mostra mensagens semelhantes à seguinte (indicando que o aplicativo está em execução e aguarda solicitações):</span><span class="sxs-lookup"><span data-stu-id="a2f4f-113">The console output shows messages similar to the following (indicating that the app is running and awaiting requests):</span></span>

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="a2f4f-114">Em um navegador da Web, navegue até `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span><span class="sxs-lookup"><span data-stu-id="a2f4f-114">In a web browser, navigate to `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span></span> <span data-ttu-id="a2f4f-115">Você deve ver o resultado de `9`.</span><span class="sxs-lookup"><span data-stu-id="a2f4f-115">You should see the result of `9`.</span></span>

<span data-ttu-id="a2f4f-116">Navegue para o API do produto (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span><span class="sxs-lookup"><span data-stu-id="a2f4f-116">Navigate to the product API (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span></span> <span data-ttu-id="a2f4f-117">Ele retorna `9`, não `20`, conforme o esperado.</span><span class="sxs-lookup"><span data-stu-id="a2f4f-117">It returns `9`, not `20` as you'd expect.</span></span> <span data-ttu-id="a2f4f-118">Corrigiremos isso mais adiante no tutorial.</span><span class="sxs-lookup"><span data-stu-id="a2f4f-118">We'll fix that later in the tutorial.</span></span>

## <a name="add-dotnet-watch-to-a-project"></a><span data-ttu-id="a2f4f-119">Adicionar `dotnet watch` a um projeto</span><span class="sxs-lookup"><span data-stu-id="a2f4f-119">Add `dotnet watch` to a project</span></span>

1. <span data-ttu-id="a2f4f-120">Adicionar uma referência ao pacote de `Microsoft.DotNet.Watcher.Tools` para o arquivo *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="a2f4f-120">Add a `Microsoft.DotNet.Watcher.Tools` package reference to the *.csproj* file:</span></span>

    ```xml
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
    </ItemGroup> 
    ```

1. <span data-ttu-id="a2f4f-121">Instale o pacote `Microsoft.DotNet.Watcher.Tools` executando o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="a2f4f-121">Install the `Microsoft.DotNet.Watcher.Tools` package by running the following command:</span></span>
    
    ```console
    dotnet restore
    ```

## <a name="running-net-core-cli-commands-using-dotnet-watch"></a><span data-ttu-id="a2f4f-122">Executando comandos da CLI do .NET Core usando `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="a2f4f-122">Running .NET Core CLI commands using `dotnet watch`</span></span>

<span data-ttu-id="a2f4f-123">Qualquer [comando da CLI do .NET Core](/dotnet/core/tools#cli-commands) pode ser executado com `dotnet watch`.</span><span class="sxs-lookup"><span data-stu-id="a2f4f-123">Any [.NET Core CLI command](/dotnet/core/tools#cli-commands) can be run with `dotnet watch`.</span></span> <span data-ttu-id="a2f4f-124">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="a2f4f-124">For example:</span></span>

| <span data-ttu-id="a2f4f-125">Comando</span><span class="sxs-lookup"><span data-stu-id="a2f4f-125">Command</span></span> | <span data-ttu-id="a2f4f-126">Comando com inspeção</span><span class="sxs-lookup"><span data-stu-id="a2f4f-126">Command with watch</span></span> |
| ---- | ----- |
| <span data-ttu-id="a2f4f-127">dotnet run</span><span class="sxs-lookup"><span data-stu-id="a2f4f-127">dotnet run</span></span> | <span data-ttu-id="a2f4f-128">dotnet watch run</span><span class="sxs-lookup"><span data-stu-id="a2f4f-128">dotnet watch run</span></span> |
| <span data-ttu-id="a2f4f-129">dotnet run -f netcoreapp2.0</span><span class="sxs-lookup"><span data-stu-id="a2f4f-129">dotnet run -f netcoreapp2.0</span></span> | <span data-ttu-id="a2f4f-130">dotnet watch run -f netcoreapp2.0</span><span class="sxs-lookup"><span data-stu-id="a2f4f-130">dotnet watch run -f netcoreapp2.0</span></span> |
| <span data-ttu-id="a2f4f-131">dotnet run -f netcoreapp2.0 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="a2f4f-131">dotnet run -f netcoreapp2.0 -- --arg1</span></span> | <span data-ttu-id="a2f4f-132">dotnet watch run -f netcoreapp2.0 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="a2f4f-132">dotnet watch run -f netcoreapp2.0 -- --arg1</span></span> |
| <span data-ttu-id="a2f4f-133">dotnet test</span><span class="sxs-lookup"><span data-stu-id="a2f4f-133">dotnet test</span></span> | <span data-ttu-id="a2f4f-134">dotnet watch test</span><span class="sxs-lookup"><span data-stu-id="a2f4f-134">dotnet watch test</span></span> |

<span data-ttu-id="a2f4f-135">Executar `dotnet watch run` na pasta *WebApp*.</span><span class="sxs-lookup"><span data-stu-id="a2f4f-135">Run `dotnet watch run` in the *WebApp* folder.</span></span> <span data-ttu-id="a2f4f-136">O resultado do console indica que `watch` foi iniciado.</span><span class="sxs-lookup"><span data-stu-id="a2f4f-136">The console output indicates `watch` has started.</span></span>

## <a name="making-changes-with-dotnet-watch"></a><span data-ttu-id="a2f4f-137">Fazer alterações com `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="a2f4f-137">Making changes with `dotnet watch`</span></span>

<span data-ttu-id="a2f4f-138">Verifique se `dotnet watch` está em execução.</span><span class="sxs-lookup"><span data-stu-id="a2f4f-138">Make sure `dotnet watch` is running.</span></span>

<span data-ttu-id="a2f4f-139">Corrija o bug no método `Product` do *MathController.cs* para que ele retorne o produto e não a soma:</span><span class="sxs-lookup"><span data-stu-id="a2f4f-139">Fix the bug in the `Product` method of *MathController.cs* so it returns the product and not the sum:</span></span>

```csharp
public static int Product(int a, int b)
{
  return a * b;
} 
```

<span data-ttu-id="a2f4f-140">Salve o arquivo.</span><span class="sxs-lookup"><span data-stu-id="a2f4f-140">Save the file.</span></span> <span data-ttu-id="a2f4f-141">O resultado do console indica que o `dotnet watch` detectou uma alteração de arquivo e reiniciou o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="a2f4f-141">The console output indicates that `dotnet watch` detected a file change and restarted the app.</span></span>

<span data-ttu-id="a2f4f-142">Verifique se `http://localhost:<port number>/api/math/product?a=4&b=5` retorna o resultado correto.</span><span class="sxs-lookup"><span data-stu-id="a2f4f-142">Verify `http://localhost:<port number>/api/math/product?a=4&b=5` returns the correct result.</span></span>

## <a name="running-tests-using-dotnet-watch"></a><span data-ttu-id="a2f4f-143">Executando testes usando `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="a2f4f-143">Running tests using `dotnet watch`</span></span>

1. <span data-ttu-id="a2f4f-144">Altere o método `Product` de *MathController.cs* novamente para retornar a soma e salve o arquivo.</span><span class="sxs-lookup"><span data-stu-id="a2f4f-144">Change the `Product` method of *MathController.cs* back to returning the sum and save the file.</span></span>
1. <span data-ttu-id="a2f4f-145">Em um shell de comando, navegue até a pasta *WebAppTests*.</span><span class="sxs-lookup"><span data-stu-id="a2f4f-145">In a command shell, navigate to the *WebAppTests* folder.</span></span>
1. <span data-ttu-id="a2f4f-146">Execute `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="a2f4f-146">Run `dotnet restore`.</span></span>
1. <span data-ttu-id="a2f4f-147">Execute `dotnet watch test`.</span><span class="sxs-lookup"><span data-stu-id="a2f4f-147">Run `dotnet watch test`.</span></span> <span data-ttu-id="a2f4f-148">Seu resultado indica que um teste falhou e que o inspetor está aguardando as alterações de arquivo:</span><span class="sxs-lookup"><span data-stu-id="a2f4f-148">Its output indicates that a test failed and that watcher is awaiting file changes:</span></span>

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. <span data-ttu-id="a2f4f-149">Corrija o código do método `Product` para que ele retorne o produto.</span><span class="sxs-lookup"><span data-stu-id="a2f4f-149">Fix the `Product` method code so it returns the product.</span></span> <span data-ttu-id="a2f4f-150">Salve o arquivo.</span><span class="sxs-lookup"><span data-stu-id="a2f4f-150">Save the file.</span></span>

<span data-ttu-id="a2f4f-151">`dotnet watch` detecta a alteração de arquivo e executa os testes novamente.</span><span class="sxs-lookup"><span data-stu-id="a2f4f-151">`dotnet watch` detects the file change and reruns the tests.</span></span> <span data-ttu-id="a2f4f-152">O resultado do console indica a aprovação nos testes.</span><span class="sxs-lookup"><span data-stu-id="a2f4f-152">The console output indicates the tests passed.</span></span>

## <a name="dotnet-watch-in-github"></a><span data-ttu-id="a2f4f-153">dotnet-watch no GitHub</span><span class="sxs-lookup"><span data-stu-id="a2f4f-153">dotnet-watch in GitHub</span></span>

<span data-ttu-id="a2f4f-154">dotnet-watch faz parte do [repositório DotNetTools](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch) do GitHub.</span><span class="sxs-lookup"><span data-stu-id="a2f4f-154">dotnet-watch is part of the GitHub [DotNetTools repository](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch).</span></span>

<span data-ttu-id="a2f4f-155">A [seção MSBuild](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch#msbuild) do [Leiame do dotnet-watch](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) explica como o dotnet-watch pode ser configurado por meio do arquivo de projeto MSBuild inspecionado.</span><span class="sxs-lookup"><span data-stu-id="a2f4f-155">The [MSBuild section](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch#msbuild) of the [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) explains how dotnet-watch can be configured from the MSBuild project file being watched.</span></span> <span data-ttu-id="a2f4f-156">O [Leiame do dotnet-watch](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) contém informações sobre o dotnet-watch não abordadas neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="a2f4f-156">The [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) contains information on dotnet-watch not covered in this tutorial.</span></span>
