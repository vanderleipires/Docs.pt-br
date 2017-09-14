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
ms.openlocfilehash: 30e0d07bdfbd16a475e03c1a21cdd10220bd1630
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
# <a name="developing-aspnet-core-apps-using-dotnet-watch"></a><span data-ttu-id="1336f-104">Desenvolvendo aplicativos ASP.NET Core usando a inspeção dotnet</span><span class="sxs-lookup"><span data-stu-id="1336f-104">Developing ASP.NET Core apps using dotnet watch</span></span>


<span data-ttu-id="1336f-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span><span class="sxs-lookup"><span data-stu-id="1336f-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span></span>

<span data-ttu-id="1336f-106">`dotnet watch` é uma ferramenta que executa um comando `dotnet` quando os arquivos de origem são alterados.</span><span class="sxs-lookup"><span data-stu-id="1336f-106">`dotnet watch` is a tool that runs a `dotnet` command when source files change.</span></span> <span data-ttu-id="1336f-107">Por exemplo, uma alteração de arquivo pode disparar uma compilação, testes ou uma implantação.</span><span class="sxs-lookup"><span data-stu-id="1336f-107">For example, a file change can trigger compilation, tests, or deployment.</span></span>

<span data-ttu-id="1336f-108">Neste tutorial, usamos um aplicativo de API Web existente com dois pontos de extremidade: um que retorna uma soma e outro que retorna um produto.</span><span class="sxs-lookup"><span data-stu-id="1336f-108">In this tutorial we use an existing Web API app with two endpoints: one that returns a sum and one that returns a product.</span></span> <span data-ttu-id="1336f-109">O método de produto contém um bug que corrigiremos como parte deste tutorial.</span><span class="sxs-lookup"><span data-stu-id="1336f-109">The product method contains a bug that we'll fix as part of this tutorial.</span></span>

<span data-ttu-id="1336f-110">Baixe o [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span><span class="sxs-lookup"><span data-stu-id="1336f-110">Download the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span></span> <span data-ttu-id="1336f-111">Ele contém dois projetos, `WebApp` (um aplicativo Web) e `WebAppTests` (testes de unidade para o aplicativo Web).</span><span class="sxs-lookup"><span data-stu-id="1336f-111">It contains two projects, `WebApp` (a web app) and `WebAppTests` (unit tests for the web app).</span></span>

<span data-ttu-id="1336f-112">Em um console, navegue para a pasta WebApp e execute os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="1336f-112">In a console, navigate to the WebApp folder and run the following commands:</span></span>

- `dotnet restore`
- `dotnet run`

<span data-ttu-id="1336f-113">O resultado do console mostrará mensagens semelhantes à seguinte (indicando que o aplicativo está em execução e aguarda solicitações):</span><span class="sxs-lookup"><span data-stu-id="1336f-113">The console output will show messages similar to the following (indicating that the app is running and waiting for requests):</span></span>

```console
$ dotnet run
Hosting environment: Production
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="1336f-114">Em um navegador da Web, navegue para `http://localhost:5000/api/math/sum?a=4&b=5`. Você deverá ver o resultado `9`.</span><span class="sxs-lookup"><span data-stu-id="1336f-114">In a web browser, navigate to `http://localhost:5000/api/math/sum?a=4&b=5`, you should see the result `9`.</span></span>

<span data-ttu-id="1336f-115">Navegue para a API do produto (`http://localhost:5000/api/math/product?a=4&b=5`); ela retorna `9`, não `20`, conforme esperado.</span><span class="sxs-lookup"><span data-stu-id="1336f-115">Navigate to the product API (`http://localhost:5000/api/math/product?a=4&b=5`), it returns `9`, not `20` as you'd expect.</span></span> <span data-ttu-id="1336f-116">Corrigiremos isso mais adiante no tutorial.</span><span class="sxs-lookup"><span data-stu-id="1336f-116">We'll fix that later in the tutorial.</span></span>

## <a name="add-dotnet-watch-to-a-project"></a><span data-ttu-id="1336f-117">Adicionar `dotnet watch` a um projeto</span><span class="sxs-lookup"><span data-stu-id="1336f-117">Add `dotnet watch` to a project</span></span>

- <span data-ttu-id="1336f-118">Adicione `Microsoft.DotNet.Watcher.Tools` ao arquivo *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="1336f-118">Add `Microsoft.DotNet.Watcher.Tools` to the *.csproj* file:</span></span>
 ```xml
 <ItemGroup>
   <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="1.0.0" />
 </ItemGroup> 
 ```

- <span data-ttu-id="1336f-119">Execute `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="1336f-119">Run `dotnet restore`.</span></span>

## <a name="running-dotnet-commands-using-dotnet-watch"></a><span data-ttu-id="1336f-120">Executando comandos `dotnet` usando `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="1336f-120">Running `dotnet` commands using `dotnet watch`</span></span>

<span data-ttu-id="1336f-121">Qualquer comando `dotnet` pode ser executado com `dotnet watch`, por exemplo:</span><span class="sxs-lookup"><span data-stu-id="1336f-121">Any `dotnet` command can be run with `dotnet watch`, for example:</span></span>

| <span data-ttu-id="1336f-122">Comando</span><span class="sxs-lookup"><span data-stu-id="1336f-122">Command</span></span> | <span data-ttu-id="1336f-123">Comando com inspeção</span><span class="sxs-lookup"><span data-stu-id="1336f-123">Command with watch</span></span> |
| ---- | ----- |
| <span data-ttu-id="1336f-124">dotnet run</span><span class="sxs-lookup"><span data-stu-id="1336f-124">dotnet run</span></span> | <span data-ttu-id="1336f-125">dotnet watch run</span><span class="sxs-lookup"><span data-stu-id="1336f-125">dotnet watch run</span></span> |
| <span data-ttu-id="1336f-126">dotnet run -f net451</span><span class="sxs-lookup"><span data-stu-id="1336f-126">dotnet run -f net451</span></span> | <span data-ttu-id="1336f-127">dotnet watch run -f net451</span><span class="sxs-lookup"><span data-stu-id="1336f-127">dotnet watch run -f net451</span></span> |
| <span data-ttu-id="1336f-128">dotnet run -f net451 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="1336f-128">dotnet run -f net451 -- --arg1</span></span> | <span data-ttu-id="1336f-129">dotnet watch run -f net451 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="1336f-129">dotnet watch run -f net451 -- --arg1</span></span> |
| <span data-ttu-id="1336f-130">dotnet test</span><span class="sxs-lookup"><span data-stu-id="1336f-130">dotnet test</span></span> | <span data-ttu-id="1336f-131">dotnet watch test</span><span class="sxs-lookup"><span data-stu-id="1336f-131">dotnet watch test</span></span> |

<span data-ttu-id="1336f-132">Execute `dotnet watch run` na pasta `WebApp`.</span><span class="sxs-lookup"><span data-stu-id="1336f-132">Run `dotnet watch run` in the `WebApp` folder.</span></span> <span data-ttu-id="1336f-133">O resultado do console indicará que `watch` foi iniciado.</span><span class="sxs-lookup"><span data-stu-id="1336f-133">The console output will indicate `watch` has started.</span></span>

## <a name="making-changes-with-dotnet-watch"></a><span data-ttu-id="1336f-134">Fazer alterações com `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="1336f-134">Making changes with `dotnet watch`</span></span>

<span data-ttu-id="1336f-135">Verifique se `dotnet watch` está em execução.</span><span class="sxs-lookup"><span data-stu-id="1336f-135">Make sure `dotnet watch` is running.</span></span>

<span data-ttu-id="1336f-136">Corrija o bug no método `Product` do `MathController` para que ele retorne o produto e não a soma.</span><span class="sxs-lookup"><span data-stu-id="1336f-136">Fix the bug in the `Product` method of the `MathController` so it returns the product and not the sum.</span></span>

```csharp
public static int Product(int a, int b)
{
  return a * b;
} 
```

<span data-ttu-id="1336f-137">Salve o arquivo.</span><span class="sxs-lookup"><span data-stu-id="1336f-137">Save the file.</span></span> <span data-ttu-id="1336f-138">O resultado do console mostrará mensagens indicando que `dotnet watch` detectou uma alteração de arquivo e reiniciou o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1336f-138">The console output will show messages indicating that `dotnet watch` detected a file change and restarted the app.</span></span>

<span data-ttu-id="1336f-139">Verifique se `http://localhost:5000/api/math/product?a=4&b=5` retorna o resultado correto.</span><span class="sxs-lookup"><span data-stu-id="1336f-139">Verify `http://localhost:5000/api/math/product?a=4&b=5` returns the correct result.</span></span>

## <a name="running-tests-using-dotnet-watch"></a><span data-ttu-id="1336f-140">Executando testes usando `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="1336f-140">Running tests using `dotnet watch`</span></span>

- <span data-ttu-id="1336f-141">Altere o método `Product` do `MathController` novamente para retornar a soma e salve o arquivo.</span><span class="sxs-lookup"><span data-stu-id="1336f-141">Change the `Product` method of the `MathController` back to returning the sum and save the file.</span></span>
- <span data-ttu-id="1336f-142">Em uma janela de comando, navegue para a pasta `WebAppTests`.</span><span class="sxs-lookup"><span data-stu-id="1336f-142">In a command window, naviagate to the `WebAppTests` folder.</span></span>
- <span data-ttu-id="1336f-143">Execute `dotnet restore`</span><span class="sxs-lookup"><span data-stu-id="1336f-143">Run `dotnet restore`</span></span>
- <span data-ttu-id="1336f-144">Execute `dotnet watch test`.</span><span class="sxs-lookup"><span data-stu-id="1336f-144">Run `dotnet watch test`.</span></span> <span data-ttu-id="1336f-145">Veja o resultado indicando que um teste falhou e que o inspetor está aguardando as alterações de arquivo:</span><span class="sxs-lookup"><span data-stu-id="1336f-145">You see output indicating that a test failed and that watcher is waiting for file changes:</span></span>

 ```console
 Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
 Test Run Failed.
  ```
- <span data-ttu-id="1336f-146">Corrija o código do método `Product` para que ele retorne o produto.</span><span class="sxs-lookup"><span data-stu-id="1336f-146">Fix the `Product` method code so it returns the product.</span></span> <span data-ttu-id="1336f-147">Salve o arquivo.</span><span class="sxs-lookup"><span data-stu-id="1336f-147">Save the file.</span></span>

<span data-ttu-id="1336f-148">`dotnet watch` detecta a alteração de arquivo e executa os testes novamente.</span><span class="sxs-lookup"><span data-stu-id="1336f-148">`dotnet watch` detects the file change and reruns the tests.</span></span> <span data-ttu-id="1336f-149">O resultado do console mostrará a aprovação nos testes.</span><span class="sxs-lookup"><span data-stu-id="1336f-149">The console output will show the tests passed.</span></span>

## <a name="dotnet-watch-in-github"></a><span data-ttu-id="1336f-150">dotnet-watch no GitHub</span><span class="sxs-lookup"><span data-stu-id="1336f-150">dotnet-watch in GitHub</span></span>

<span data-ttu-id="1336f-151">dotnet-watch faz parte do [repositório DotNetTools](https://github.com/aspnet/DotNetTools/tree/dev/src/Microsoft.DotNet.Watcher.Tools) do GitHub.</span><span class="sxs-lookup"><span data-stu-id="1336f-151">dotnet-watch is part of the GitHub [DotNetTools repository](https://github.com/aspnet/DotNetTools/tree/dev/src/Microsoft.DotNet.Watcher.Tools).</span></span>

<span data-ttu-id="1336f-152">A [seção MSBuild](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md#msbuild) do [Leiame do dotnet-watch](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) explica como o dotnet-watch pode ser configurado por meio do arquivo de projeto MSBuild inspecionado.</span><span class="sxs-lookup"><span data-stu-id="1336f-152">The [MSBuild section](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md#msbuild) of the [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) explains how dotnet-watch can be configured from the MSBuild project file being watched.</span></span> <span data-ttu-id="1336f-153">O [Leiame do dotnet-watch](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) contém informações sobre o dotnet-watch não abordadas neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="1336f-153">The [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) contains information on dotnet-watch not covered in this tutorial.</span></span>
