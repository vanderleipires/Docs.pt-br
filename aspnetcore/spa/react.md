---
title: Use o modelo de projeto reagir
author: SteveSandersonMS
description: "Saiba como começar a usar o modelo de projeto de aplicativo de página única (SPA) do ASP.NET Core para reagir e criar reagir-aplicativo."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/react
ms.openlocfilehash: e093a47159fd8278ff3705bb2c53571a8e27fab8
ms.sourcegitcommit: 49fb3b7669b504d35edad34db8285e56b958a9fc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/23/2018
---
# <a name="use-the-react-project-template"></a><span data-ttu-id="f06d1-103">Use o modelo de projeto reagir</span><span class="sxs-lookup"><span data-stu-id="f06d1-103">Use the React project template</span></span>

> [!NOTE]
> <span data-ttu-id="f06d1-104">Esta documentação não está sobre o modelo de projeto reagir incluída no ASP.NET 2.0 de núcleo.</span><span class="sxs-lookup"><span data-stu-id="f06d1-104">This documentation isn't about the React project template included in ASP.NET Core 2.0.</span></span> <span data-ttu-id="f06d1-105">Trata-se o modelo de reagir mais recente para o qual você pode atualizar manualmente.</span><span class="sxs-lookup"><span data-stu-id="f06d1-105">It's about the newer React template to which you can update manually.</span></span> <span data-ttu-id="f06d1-106">O modelo é incluído no ASP.NET Core 2.1 por padrão.</span><span class="sxs-lookup"><span data-stu-id="f06d1-106">The template is included in ASP.NET Core 2.1 by default.</span></span>

<span data-ttu-id="f06d1-107">O modelo de projeto reagir atualizado fornece um ponto inicial conveniente para ASP.NET Core aplicativos usando reagir e [criar reagir-aplicativo](https://github.com/facebookincubator/create-react-app) convenções (CRA) para implementar uma interface de usuário do lado do cliente avançado (IU).</span><span class="sxs-lookup"><span data-stu-id="f06d1-107">The updated React project template provides a convenient starting point for ASP.NET Core apps using React and [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) conventions to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="f06d1-108">O modelo é equivalente à criação de um projeto do ASP.NET Core para atuar como um back-end de API e um projeto padrão CRA reagir para atuar como uma interface do usuário, mas com a conveniência de hospedagem em um projeto de aplicativo único que pode ser criado e publicado como uma única unidade.</span><span class="sxs-lookup"><span data-stu-id="f06d1-108">The template is equivalent to creating both an ASP.NET Core project to act as an API backend, and a standard CRA React project to act as a UI, but with the convenience of hosting both in a single app project that can be built and published as a single unit.</span></span>

## <a name="create-a-new-app"></a><span data-ttu-id="f06d1-109">Criar um novo aplicativo</span><span class="sxs-lookup"><span data-stu-id="f06d1-109">Create a new app</span></span>

<span data-ttu-id="f06d1-110">Se usar o ASP.NET 2.0 de núcleo, certifique-se de que você [instalado o modelo de projeto reagir atualizado](xref:spa/index#installation).</span><span class="sxs-lookup"><span data-stu-id="f06d1-110">If using ASP.NET Core 2.0, ensure you've [installed the updated React project template](xref:spa/index#installation).</span></span> <span data-ttu-id="f06d1-111">Se você tiver o ASP.NET Core 2.1, não é necessário instalá-lo.</span><span class="sxs-lookup"><span data-stu-id="f06d1-111">If you have ASP.NET Core 2.1, there's no need to install it.</span></span>

<span data-ttu-id="f06d1-112">Criar um novo projeto a partir de um prompt de comando usando o comando `dotnet new react` em um diretório vazio.</span><span class="sxs-lookup"><span data-stu-id="f06d1-112">Create a new project from a command prompt using the command `dotnet new react` in an empty directory.</span></span> <span data-ttu-id="f06d1-113">Por exemplo, os seguintes comandos criam o aplicativo em um *aplicativo my-novo* diretório e alterne para o diretório:</span><span class="sxs-lookup"><span data-stu-id="f06d1-113">For example, the following commands create the app in a *my-new-app* directory and switch to that directory:</span></span>

```console
dotnet new react -o my-new-app
cd my-new-app
```

<span data-ttu-id="f06d1-114">Execute o aplicativo do Visual Studio ou o .NET Core CLI:</span><span class="sxs-lookup"><span data-stu-id="f06d1-114">Run the app from either Visual Studio or the .NET Core CLI:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f06d1-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f06d1-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f06d1-116">Abra o gerado *. csproj* de arquivo e executar o aplicativo como normal de lá.</span><span class="sxs-lookup"><span data-stu-id="f06d1-116">Open the generated *.csproj* file, and run the app as normal from there.</span></span>

<span data-ttu-id="f06d1-117">O processo de compilação restaura npm dependências na primeira execução, o que pode levar vários minutos.</span><span class="sxs-lookup"><span data-stu-id="f06d1-117">The build process restores npm dependencies on the first run, which can take several minutes.</span></span> <span data-ttu-id="f06d1-118">Compilações subsequentes são muito mais rápidas.</span><span class="sxs-lookup"><span data-stu-id="f06d1-118">Subsequent builds are much faster.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f06d1-119">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="f06d1-119">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="f06d1-120">Verifique se você tem uma variável de ambiente chamada `ASPNETCORE_Environment` com valor de `Development`.</span><span class="sxs-lookup"><span data-stu-id="f06d1-120">Ensure you have an environment variable called `ASPNETCORE_Environment` with value of `Development`.</span></span> <span data-ttu-id="f06d1-121">No Windows (no prompt do PowerShell não), execute `SET ASPNETCORE_Environment=Development`.</span><span class="sxs-lookup"><span data-stu-id="f06d1-121">On Windows (in non-PowerShell prompts), run `SET ASPNETCORE_Environment=Development`.</span></span> <span data-ttu-id="f06d1-122">No Linux ou macOS, execute `export ASPNETCORE_Environment=Development`.</span><span class="sxs-lookup"><span data-stu-id="f06d1-122">On Linux or macOS, run `export ASPNETCORE_Environment=Development`.</span></span>

<span data-ttu-id="f06d1-123">Execute `dotnet build` verificar se seu aplicativo cria corretamente.</span><span class="sxs-lookup"><span data-stu-id="f06d1-123">Run `dotnet build` to verify your app builds correctly.</span></span> <span data-ttu-id="f06d1-124">Na primeira execução, o processo de compilação restaura dependências npm, o que podem levar vários minutos.</span><span class="sxs-lookup"><span data-stu-id="f06d1-124">On the first run, the build process restores npm dependencies, which can take several minutes.</span></span> <span data-ttu-id="f06d1-125">Compilações subsequentes são muito mais rápidas.</span><span class="sxs-lookup"><span data-stu-id="f06d1-125">Subsequent builds are much faster.</span></span>

<span data-ttu-id="f06d1-126">Execute `dotnet run` para iniciar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="f06d1-126">Run `dotnet run` to start the app.</span></span>

---

<span data-ttu-id="f06d1-127">O modelo de projeto cria um aplicativo do ASP.NET Core e um aplicativo reagir.</span><span class="sxs-lookup"><span data-stu-id="f06d1-127">The project template creates an ASP.NET Core app and a React app.</span></span> <span data-ttu-id="f06d1-128">O aplicativo do ASP.NET Core destina-se a ser usado para acesso a dados, autorização e outras questões do lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="f06d1-128">The ASP.NET Core app is intended to be used for data access, authorization, and other server-side concerns.</span></span> <span data-ttu-id="f06d1-129">O aplicativo reagir, que residem no *ClientApp* subdiretório, destina-se a ser usado para todas as questões de interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="f06d1-129">The React app, residing in the *ClientApp* subdirectory, is intended to be used for all UI concerns.</span></span>

## <a name="add-pages-images-styles-modules-etc"></a><span data-ttu-id="f06d1-130">Adicione páginas, imagens, estilos, módulos, etc.</span><span class="sxs-lookup"><span data-stu-id="f06d1-130">Add pages, images, styles, modules, etc.</span></span>

<span data-ttu-id="f06d1-131">O *ClientApp* diretório é um aplicativo de reagir CRA padrão.</span><span class="sxs-lookup"><span data-stu-id="f06d1-131">The *ClientApp* directory is a standard CRA React app.</span></span> <span data-ttu-id="f06d1-132">Consulte o oficial [documentação CRA](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="f06d1-132">See the official [CRA documentation](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md) for more information.</span></span>

<span data-ttu-id="f06d1-133">Há pequenas diferenças entre o aplicativo reagir criado por este modelo e um criado por CRA em si; No entanto, os recursos do aplicativo são inalterados.</span><span class="sxs-lookup"><span data-stu-id="f06d1-133">There are slight differences between the React app created by this template and the one created by CRA itself; however, the app's capabilities are unchanged.</span></span> <span data-ttu-id="f06d1-134">O aplicativo criado pelo modelo contém um [Bootstrap](https://getbootstrap.com/)-com base em layout e um exemplo básico de roteamento.</span><span class="sxs-lookup"><span data-stu-id="f06d1-134">The app created by the template contains a [Bootstrap](https://getbootstrap.com/)-based layout and a basic routing example.</span></span>

## <a name="install-npm-packages"></a><span data-ttu-id="f06d1-135">Instalar pacotes de npm</span><span class="sxs-lookup"><span data-stu-id="f06d1-135">Install npm packages</span></span>

<span data-ttu-id="f06d1-136">Para instalar pacotes de terceiros npm, use um prompt de comando no *ClientApp* subdiretório.</span><span class="sxs-lookup"><span data-stu-id="f06d1-136">To install third-party npm packages, use a command prompt in the *ClientApp* subdirectory.</span></span> <span data-ttu-id="f06d1-137">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="f06d1-137">For example:</span></span>

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a><span data-ttu-id="f06d1-138">Publicar e implantar</span><span class="sxs-lookup"><span data-stu-id="f06d1-138">Publish and deploy</span></span>

<span data-ttu-id="f06d1-139">No desenvolvimento, o aplicativo é executado em um modo otimizado para conveniência do desenvolvedor.</span><span class="sxs-lookup"><span data-stu-id="f06d1-139">In development, the app runs in a mode optimized for developer convenience.</span></span> <span data-ttu-id="f06d1-140">Por exemplo, pacotes de JavaScript incluem mapas de origem (de modo que durante a depuração, você pode ver o código-fonte original).</span><span class="sxs-lookup"><span data-stu-id="f06d1-140">For example, JavaScript bundles include source maps (so that when debugging, you can see your original source code).</span></span> <span data-ttu-id="f06d1-141">O aplicativo observa JavaScript, HTML e CSS alterações de arquivo no disco e recompila automaticamente e recarrega quando ele vê esses arquivos alterar.</span><span class="sxs-lookup"><span data-stu-id="f06d1-141">The app watches JavaScript, HTML, and CSS file changes on disk and automatically recompiles and reloads when it sees those files change.</span></span>

<span data-ttu-id="f06d1-142">Em produção, atende a uma versão de seu aplicativo que é otimizado para desempenho.</span><span class="sxs-lookup"><span data-stu-id="f06d1-142">In production, serve a version of your app that's optimized for performance.</span></span> <span data-ttu-id="f06d1-143">Isso é configurado para ocorrer automaticamente.</span><span class="sxs-lookup"><span data-stu-id="f06d1-143">This is configured to happen automatically.</span></span> <span data-ttu-id="f06d1-144">Quando você publica, a configuração de build emite uma compilação transpiled minimizada, do seu código do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="f06d1-144">When you publish, the build configuration emits a minified, transpiled build of your client-side code.</span></span> <span data-ttu-id="f06d1-145">Diferentemente de compilação de desenvolvimento, a compilação de produção não requer Node. js ser instalado no servidor.</span><span class="sxs-lookup"><span data-stu-id="f06d1-145">Unlike the development build, the production build doesn't require Node.js to be installed on the server.</span></span>

<span data-ttu-id="f06d1-146">Você pode usar o padrão [métodos de implantação e hospedagem ASP.NET Core](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="f06d1-146">You can use standard [ASP.NET Core hosting and deployment methods](xref:host-and-deploy/index).</span></span>

## <a name="run-the-cra-server-independently"></a><span data-ttu-id="f06d1-147">Executar o servidor CRA independentemente</span><span class="sxs-lookup"><span data-stu-id="f06d1-147">Run the CRA server independently</span></span>

<span data-ttu-id="f06d1-148">O projeto está configurado para iniciar sua própria instância do servidor de desenvolvimento CRA em segundo plano quando o aplicativo ASP.NET Core é iniciado no modo de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="f06d1-148">The project is configured to start its own instance of the CRA development server in the background when the ASP.NET Core app starts in development mode.</span></span> <span data-ttu-id="f06d1-149">Isso é conveniente, porque isso significa que você não precisa executar um servidor separado manualmente.</span><span class="sxs-lookup"><span data-stu-id="f06d1-149">This is convenient because it means you don't have to run a separate server manually.</span></span>

<span data-ttu-id="f06d1-150">Há uma desvantagem para essa configuração padrão.</span><span class="sxs-lookup"><span data-stu-id="f06d1-150">There's a drawback to this default setup.</span></span> <span data-ttu-id="f06d1-151">Cada vez que você modificar seu código c# e o ASP.NET Core aplicativo precisa ser reiniciado, o servidor CRA for reiniciado.</span><span class="sxs-lookup"><span data-stu-id="f06d1-151">Each time you modify your C# code and your ASP.NET Core app needs to restart, the CRA server restarts.</span></span> <span data-ttu-id="f06d1-152">Alguns segundos são necessários para iniciar um backup.</span><span class="sxs-lookup"><span data-stu-id="f06d1-152">A few seconds are required to start back up.</span></span> <span data-ttu-id="f06d1-153">Se você estiver fazendo frequentes edições de código c# e não quiser esperar o servidor CRA reiniciar, execute o servidor CRA externamente, independentemente do processo do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f06d1-153">If you're making frequent C# code edits and don't want to wait for the CRA server to restart, run the CRA server externally, independently of the ASP.NET Core process.</span></span> <span data-ttu-id="f06d1-154">Para fazer isso:</span><span class="sxs-lookup"><span data-stu-id="f06d1-154">To do so:</span></span>

1. <span data-ttu-id="f06d1-155">No prompt de comando, alterne para o *ClientApp* subdiretório e iniciar o servidor de desenvolvimento CRA:</span><span class="sxs-lookup"><span data-stu-id="f06d1-155">In a command prompt, switch to the *ClientApp* subdirectory, and launch the CRA development server:</span></span>

    ```console
    cd ClientApp
    npm start
    ```

2. <span data-ttu-id="f06d1-156">Modifique seu aplicativo ASP.NET Core para usar a instância do servidor CRA externa em vez de iniciar um dos seus próprios.</span><span class="sxs-lookup"><span data-stu-id="f06d1-156">Modify your ASP.NET Core app to use the external CRA server instance instead of launching one of its own.</span></span> <span data-ttu-id="f06d1-157">No seu *inicialização* classe, substitua o `spa.UseReactDevelopmentServer` chamada com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="f06d1-157">In your *Startup* class, replace the `spa.UseReactDevelopmentServer` invocation with the following:</span></span>

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:3000");
    ```

<span data-ttu-id="f06d1-158">Quando você iniciar seu aplicativo ASP.NET Core, ele não é possível abrir um servidor CRA.</span><span class="sxs-lookup"><span data-stu-id="f06d1-158">When you start your ASP.NET Core app, it won't launch a CRA server.</span></span> <span data-ttu-id="f06d1-159">Em vez disso, é usada a instância que é iniciado manualmente.</span><span class="sxs-lookup"><span data-stu-id="f06d1-159">The instance you started manually is used instead.</span></span> <span data-ttu-id="f06d1-160">Isso permite iniciar e reiniciar mais rapidamente.</span><span class="sxs-lookup"><span data-stu-id="f06d1-160">This enables it to start and restart faster.</span></span> <span data-ttu-id="f06d1-161">Ele não esteja mais aguardando para seu aplicativo reagir recriar a cada vez.</span><span class="sxs-lookup"><span data-stu-id="f06d1-161">It's no longer waiting for your React app to rebuild each time.</span></span>
