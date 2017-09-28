---
title: "Desenvolvendo aplicativos ASP.NET Core usando a inspeção dotnet"
author: rick-anderson
description: "Mostra como usar a inspeção dotnet."
keywords: "ASP.NET Core, usando a inspeção dotnet"
ms.author: riande
manager: wpickett
ms.date: 03/09/2017
ms.topic: article
ms.assetid: 563ffb3f-d369-4aa5-bf0a-7300b4e7832c
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/dotnet-watch
ms.openlocfilehash: 6a8943619e6174dbcd3d901b7bb736ba5d3af95d
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/22/2017
---
# <a name="developing-aspnet-core-apps-using-dotnet-watch"></a><span data-ttu-id="44d5d-104">Desenvolvendo aplicativos ASP.NET Core usando a inspeção dotnet</span><span class="sxs-lookup"><span data-stu-id="44d5d-104">Developing ASP.NET Core apps using dotnet watch</span></span>


<span data-ttu-id="44d5d-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span><span class="sxs-lookup"><span data-stu-id="44d5d-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span></span>

<span data-ttu-id="44d5d-106">`dotnet watch` é uma ferramenta que executa um comando `dotnet` quando os arquivos de origem são alterados.</span><span class="sxs-lookup"><span data-stu-id="44d5d-106">`dotnet watch` is a tool that runs a `dotnet` command when source files change.</span></span> <span data-ttu-id="44d5d-107">Por exemplo, uma alteração de arquivo pode disparar uma compilação, testes ou uma implantação.</span><span class="sxs-lookup"><span data-stu-id="44d5d-107">For example, a file change can trigger compilation, tests, or deployment.</span></span>

<span data-ttu-id="44d5d-108">Neste tutorial, usamos um aplicativo de API Web existente com dois pontos de extremidade: um que retorna uma soma e outro que retorna um produto.</span><span class="sxs-lookup"><span data-stu-id="44d5d-108">In this tutorial, we use an existing Web API app with two endpoints: one that returns a sum and one that returns a product.</span></span> <span data-ttu-id="44d5d-109">O método de produto contém um bug que corrigiremos como parte deste tutorial.</span><span class="sxs-lookup"><span data-stu-id="44d5d-109">The product method contains a bug that we'll fix as part of this tutorial.</span></span>

<span data-ttu-id="44d5d-110">Baixe o [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span><span class="sxs-lookup"><span data-stu-id="44d5d-110">Download the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span></span> <span data-ttu-id="44d5d-111">Ele contém dois projetos, `WebApp` (um aplicativo Web) e `WebAppTests` (testes de unidade para o aplicativo Web).</span><span class="sxs-lookup"><span data-stu-id="44d5d-111">It contains two projects, `WebApp` (a web app) and `WebAppTests` (unit tests for the web app).</span></span>

<span data-ttu-id="44d5d-112">Em um console, navegue para a pasta WebApp e execute os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="44d5d-112">In a console, navigate to the WebApp folder and run the following commands:</span></span>

- `dotnet restore`
- `dotnet run`

<span data-ttu-id="44d5d-113">O resultado do console mostrará mensagens semelhantes à seguinte (indicando que o aplicativo está em execução e aguarda solicitações):</span><span class="sxs-lookup"><span data-stu-id="44d5d-113">The console output will show messages similar to the following (indicating that the app is running and waiting for requests):</span></span>

```console
$ dotnet run
Hosting environment: Production
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="44d5d-114">Em um navegador da Web, navegue para `http://localhost:5000/api/math/sum?a=4&b=5`. Você deverá ver o resultado `9`.</span><span class="sxs-lookup"><span data-stu-id="44d5d-114">In a web browser, navigate to `http://localhost:5000/api/math/sum?a=4&b=5`, you should see the result `9`.</span></span>

<span data-ttu-id="44d5d-115">Navegue para a API do produto (`http://localhost:5000/api/math/product?a=4&b=5`); ela retorna `9`, não `20`, conforme esperado.</span><span class="sxs-lookup"><span data-stu-id="44d5d-115">Navigate to the product API (`http://localhost:5000/api/math/product?a=4&b=5`), it returns `9`, not `20` as you'd expect.</span></span> <span data-ttu-id="44d5d-116">Corrigiremos isso mais adiante no tutorial.</span><span class="sxs-lookup"><span data-stu-id="44d5d-116">We'll fix that later in the tutorial.</span></span>

## <a name="add-dotnet-watch-to-a-project"></a><span data-ttu-id="44d5d-117">Adicionar `dotnet watch` a um projeto</span><span class="sxs-lookup"><span data-stu-id="44d5d-117">Add `dotnet watch` to a project</span></span>

- <span data-ttu-id="44d5d-118">Adicione `Microsoft.DotNet.Watcher.Tools` ao arquivo *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="44d5d-118">Add `Microsoft.DotNet.Watcher.Tools` to the *.csproj* file:</span></span>
 ```xml
 <ItemGroup>
   <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
 </ItemGroup> 
 ```

- <span data-ttu-id="44d5d-119">Execute `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="44d5d-119">Run `dotnet restore`.</span></span>

## <a name="running-dotnet-commands-using-dotnet-watch"></a><span data-ttu-id="44d5d-120">Executando comandos `dotnet` usando `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="44d5d-120">Running `dotnet` commands using `dotnet watch`</span></span>

<span data-ttu-id="44d5d-121">Qualquer comando `dotnet` pode ser executado com `dotnet watch`, por exemplo:</span><span class="sxs-lookup"><span data-stu-id="44d5d-121">Any `dotnet` command can be run with `dotnet watch`, for example:</span></span>

| <span data-ttu-id="44d5d-122">Comando</span><span class="sxs-lookup"><span data-stu-id="44d5d-122">Command</span></span> | <span data-ttu-id="44d5d-123">Comando com inspeção</span><span class="sxs-lookup"><span data-stu-id="44d5d-123">Command with watch</span></span> |
| ---- | ----- |
| <span data-ttu-id="44d5d-124">dotnet run</span><span class="sxs-lookup"><span data-stu-id="44d5d-124">dotnet run</span></span> | <span data-ttu-id="44d5d-125">dotnet watch run</span><span class="sxs-lookup"><span data-stu-id="44d5d-125">dotnet watch run</span></span> |
| <span data-ttu-id="44d5d-126">dotnet run -f net451</span><span class="sxs-lookup"><span data-stu-id="44d5d-126">dotnet run -f net451</span></span> | <span data-ttu-id="44d5d-127">dotnet watch run -f net451</span><span class="sxs-lookup"><span data-stu-id="44d5d-127">dotnet watch run -f net451</span></span> |
| <span data-ttu-id="44d5d-128">dotnet run -f net451 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="44d5d-128">dotnet run -f net451 -- --arg1</span></span> | <span data-ttu-id="44d5d-129">dotnet watch run -f net451 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="44d5d-129">dotnet watch run -f net451 -- --arg1</span></span> |
| <span data-ttu-id="44d5d-130">dotnet test</span><span class="sxs-lookup"><span data-stu-id="44d5d-130">dotnet test</span></span> | <span data-ttu-id="44d5d-131">dotnet watch test</span><span class="sxs-lookup"><span data-stu-id="44d5d-131">dotnet watch test</span></span> |

<span data-ttu-id="44d5d-132">Execute `dotnet watch run` na pasta `WebApp`.</span><span class="sxs-lookup"><span data-stu-id="44d5d-132">Run `dotnet watch run` in the `WebApp` folder.</span></span> <span data-ttu-id="44d5d-133">O resultado do console indicará que `watch` foi iniciado.</span><span class="sxs-lookup"><span data-stu-id="44d5d-133">The console output will indicate `watch` has started.</span></span>

## <a name="making-changes-with-dotnet-watch"></a><span data-ttu-id="44d5d-134">Fazer alterações com `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="44d5d-134">Making changes with `dotnet watch`</span></span>

<span data-ttu-id="44d5d-135">Verifique se `dotnet watch` está em execução.</span><span class="sxs-lookup"><span data-stu-id="44d5d-135">Make sure `dotnet watch` is running.</span></span>

<span data-ttu-id="44d5d-136">Corrija o bug no método `Product` do `MathController` para que ele retorne o produto e não a soma.</span><span class="sxs-lookup"><span data-stu-id="44d5d-136">Fix the bug in the `Product` method of the `MathController` so it returns the product and not the sum.</span></span>

```csharp
public static int Product(int a, int b)
{
  return a * b;
} 
```

<span data-ttu-id="44d5d-137">Salve o arquivo.</span><span class="sxs-lookup"><span data-stu-id="44d5d-137">Save the file.</span></span> <span data-ttu-id="44d5d-138">O resultado do console mostrará mensagens indicando que `dotnet watch` detectou uma alteração de arquivo e reiniciou o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="44d5d-138">The console output will show messages indicating that `dotnet watch` detected a file change and restarted the app.</span></span>

<span data-ttu-id="44d5d-139">Verifique se `http://localhost:5000/api/math/product?a=4&b=5` retorna o resultado correto.</span><span class="sxs-lookup"><span data-stu-id="44d5d-139">Verify `http://localhost:5000/api/math/product?a=4&b=5` returns the correct result.</span></span>

## <a name="running-tests-using-dotnet-watch"></a><span data-ttu-id="44d5d-140">Executando testes usando `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="44d5d-140">Running tests using `dotnet watch`</span></span>

- <span data-ttu-id="44d5d-141">Altere o método `Product` do `MathController` novamente para retornar a soma e salve o arquivo.</span><span class="sxs-lookup"><span data-stu-id="44d5d-141">Change the `Product` method of the `MathController` back to returning the sum and save the file.</span></span>
- <span data-ttu-id="44d5d-142">Em uma janela de comando, navegue para a pasta `WebAppTests`.</span><span class="sxs-lookup"><span data-stu-id="44d5d-142">In a command window, naviagate to the `WebAppTests` folder.</span></span>
- <span data-ttu-id="44d5d-143">Execute `dotnet restore`</span><span class="sxs-lookup"><span data-stu-id="44d5d-143">Run `dotnet restore`</span></span>
- <span data-ttu-id="44d5d-144">Execute `dotnet watch test`.</span><span class="sxs-lookup"><span data-stu-id="44d5d-144">Run `dotnet watch test`.</span></span> <span data-ttu-id="44d5d-145">Veja o resultado indicando que um teste falhou e que o inspetor está aguardando as alterações de arquivo:</span><span class="sxs-lookup"><span data-stu-id="44d5d-145">You see output indicating that a test failed and that watcher is waiting for file changes:</span></span>

 ```console
 Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
 Test Run Failed.
  ```
- <span data-ttu-id="44d5d-146">Corrija o código do método `Product` para que ele retorne o produto.</span><span class="sxs-lookup"><span data-stu-id="44d5d-146">Fix the `Product` method code so it returns the product.</span></span> <span data-ttu-id="44d5d-147">Salve o arquivo.</span><span class="sxs-lookup"><span data-stu-id="44d5d-147">Save the file.</span></span>

<span data-ttu-id="44d5d-148">`dotnet watch` detecta a alteração de arquivo e executa os testes novamente.</span><span class="sxs-lookup"><span data-stu-id="44d5d-148">`dotnet watch` detects the file change and reruns the tests.</span></span> <span data-ttu-id="44d5d-149">O resultado do console mostrará a aprovação nos testes.</span><span class="sxs-lookup"><span data-stu-id="44d5d-149">The console output will show the tests passed.</span></span>

## <a name="dotnet-watch-in-github"></a><span data-ttu-id="44d5d-150">dotnet-watch no GitHub</span><span class="sxs-lookup"><span data-stu-id="44d5d-150">dotnet-watch in GitHub</span></span>

<span data-ttu-id="44d5d-151">dotnet-watch faz parte do [repositório DotNetTools](https://github.com/aspnet/DotNetTools/tree/dev/src/Microsoft.DotNet.Watcher.Tools) do GitHub.</span><span class="sxs-lookup"><span data-stu-id="44d5d-151">dotnet-watch is part of the GitHub [DotNetTools repository](https://github.com/aspnet/DotNetTools/tree/dev/src/Microsoft.DotNet.Watcher.Tools).</span></span>

<span data-ttu-id="44d5d-152">A [seção MSBuild](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md#msbuild) do [Leiame do dotnet-watch](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) explica como o dotnet-watch pode ser configurado por meio do arquivo de projeto MSBuild inspecionado.</span><span class="sxs-lookup"><span data-stu-id="44d5d-152">The [MSBuild section](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md#msbuild) of the [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) explains how dotnet-watch can be configured from the MSBuild project file being watched.</span></span> <span data-ttu-id="44d5d-153">O [Leiame do dotnet-watch](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) contém informações sobre o dotnet-watch não abordadas neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="44d5d-153">The [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) contains information on dotnet-watch not covered in this tutorial.</span></span>
