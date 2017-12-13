---
title: "Compilação de projetos com Yeoman no núcleo do ASP.NET"
author: spboyer
description: "Este artigo o orienta na criação de uma ASP.NET Core aplicativo web usando o Yeoman gerador de macOS."
keywords: ASP.NET Core, Yeoman, plataforma cruzada, seu aspnet
ms.author: spboyer
manager: wpickett
ms.date: 07/05/2017
ms.topic: article
ms.assetid: fda0c2a8-1743-4505-be1a-7f8ceeef8647
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/yeoman
ms.openlocfilehash: d7411c1635e9fef2857f9a03e7310224ee8d7344
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="introduction-to-building-projects-with-yeoman-in-aspnet-core"></a><span data-ttu-id="11e2a-104">Introdução à criação de projetos com Yeoman no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="11e2a-104">Introduction to building projects with Yeoman in ASP.NET Core</span></span>

<span data-ttu-id="11e2a-105">[Yeoman](http://yeoman.io/) é um sistema de scaffolding de projeto para a criação de vários tipos de aplicativos.</span><span class="sxs-lookup"><span data-stu-id="11e2a-105">[Yeoman](http://yeoman.io/) is a project scaffolding system for creating many kinds of applications.</span></span> <span data-ttu-id="11e2a-106">O Yeoman gerador para o ASP.NET Core contém uma variedade de modelos de projeto para iniciar uma nova web, o MVC ou o aplicativo de console.</span><span class="sxs-lookup"><span data-stu-id="11e2a-106">The Yeoman generator for ASP.NET Core contains a variety of project templates for starting a new web, MVC, or console application.</span></span>

## <a name="install-nodejs-npm-and-yeoman"></a><span data-ttu-id="11e2a-107">Instale o Node. js, npm e Yeoman</span><span class="sxs-lookup"><span data-stu-id="11e2a-107">Install Node.js, npm, and Yeoman</span></span>

### <a name="prerequisites"></a><span data-ttu-id="11e2a-108">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="11e2a-108">Prerequisites</span></span>

<span data-ttu-id="11e2a-109">Node. js e npm são necessários para Yeoman.</span><span class="sxs-lookup"><span data-stu-id="11e2a-109">Node.js and npm are required for Yeoman.</span></span> <span data-ttu-id="11e2a-110">Baixar do [Node.js](https://nodejs.org/).</span><span class="sxs-lookup"><span data-stu-id="11e2a-110">Download from [Node.js](https://nodejs.org/).</span></span> <span data-ttu-id="11e2a-111">O instalador inclui [Node.js](https://nodejs.org/) e [npm](https://www.npmjs.com/).</span><span class="sxs-lookup"><span data-stu-id="11e2a-111">The installer includes [Node.js](https://nodejs.org/) and [npm](https://www.npmjs.com/).</span></span> <span data-ttu-id="11e2a-112">Bower também é necessário para instalar estruturas de interface do usuário como a inicialização.</span><span class="sxs-lookup"><span data-stu-id="11e2a-112">Bower is also required for installing UI frameworks like Bootstrap.</span></span>

<span data-ttu-id="11e2a-113">Para instalar Yeoman e Bower, execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="11e2a-113">To install Yeoman and Bower, run the following command:</span></span>

```console
npm install -g yo bower
```

>[!Note]
><span data-ttu-id="11e2a-114">Se você receber o erro `npm ERR! Please try running this command again as root/Administrator.` no macOS, execute o seguinte comando usando [sudo](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man8/sudo.8.html):`sudo npm install -g yo bower`</span><span class="sxs-lookup"><span data-stu-id="11e2a-114">If you get the error `npm ERR! Please try running this command again as root/Administrator.` on macOS, run the following command using [sudo](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man8/sudo.8.html): `sudo npm install -g yo bower`</span></span>

<span data-ttu-id="11e2a-115">Em um prompt de comando, instale o gerador do ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="11e2a-115">From a command prompt, install the ASP.NET generator:</span></span>

```console
npm install -g generator-aspnet
```

> [!NOTE]
> <span data-ttu-id="11e2a-116">Se você receber um erro de permissão, execute o comando em `sudo` conforme descrito acima.</span><span class="sxs-lookup"><span data-stu-id="11e2a-116">If you get a permission error, run the command under `sudo` as described above.</span></span>

<span data-ttu-id="11e2a-117">O `–g` sinalizador instala o gerador de globalmente, para que ele pode ser usado em qualquer caminho.</span><span class="sxs-lookup"><span data-stu-id="11e2a-117">The `–g` flag installs the generator globally, so that it can be used from any path.</span></span>

## <a name="create-an-aspnet-app"></a><span data-ttu-id="11e2a-118">Criar um aplicativo ASP.NET</span><span class="sxs-lookup"><span data-stu-id="11e2a-118">Create an ASP.NET app</span></span>

<span data-ttu-id="11e2a-119">Execute o gerador ASP.NET baseados em Yeoman:</span><span class="sxs-lookup"><span data-stu-id="11e2a-119">Run the Yeoman-based ASP.NET generator:</span></span>

```console
yo aspnet
```

<span data-ttu-id="11e2a-120">O gerador de exibe um menu.</span><span class="sxs-lookup"><span data-stu-id="11e2a-120">The generator displays a menu.</span></span> <span data-ttu-id="11e2a-121">Seta para baixo até o **Web aplicativo básico [sem associação e a autorização]** do projeto e toque em **Enter**:</span><span class="sxs-lookup"><span data-stu-id="11e2a-121">Arrow down to the **Web Application Basic [without Membership and Authorization]** project and tap **Enter**:</span></span>

![Janela de comando: O tipo de aplicativo você deseja criar?](yeoman/_static/yeoman-yo-aspnet.png)

<span data-ttu-id="11e2a-124">Selecione inicialização como a estrutura de interface do usuário e toque em **Enter**.</span><span class="sxs-lookup"><span data-stu-id="11e2a-124">Select Bootstrap as the UI Framework and tap **Enter**.</span></span>

<span data-ttu-id="11e2a-125">Use "**MyWebApp**" para o nome do aplicativo e, em seguida, toque **Enter**.</span><span class="sxs-lookup"><span data-stu-id="11e2a-125">Use "**MyWebApp**" for the app name and then tap **Enter**.</span></span>

<span data-ttu-id="11e2a-126">Yeoman será scaffold o projeto e seus arquivos de suporte.</span><span class="sxs-lookup"><span data-stu-id="11e2a-126">Yeoman will scaffold the project and its supporting files.</span></span> <span data-ttu-id="11e2a-127">Próximas etapas sugeridas também são fornecidas na forma de comandos.</span><span class="sxs-lookup"><span data-stu-id="11e2a-127">Suggested next steps are also provided in the form of commands.</span></span>

![Janela de comando: qual é o nome do seu aplicativo ASP.NET?](yeoman/_static/yeoman-yo-aspnet-created.png)

<span data-ttu-id="11e2a-130">O [ASP.NET gerador](https://www.npmjs.com/package/generator-aspnet) cria ASP.NET Core projetos que podem ser carregado no código do Visual Studio, Visual Studio, ou executado na linha de comando.</span><span class="sxs-lookup"><span data-stu-id="11e2a-130">The [ASP.NET generator](https://www.npmjs.com/package/generator-aspnet) creates ASP.NET Core projects that can be loaded into Visual Studio Code, Visual Studio, or run from the command line.</span></span>

## <a name="restore-build-and-run"></a><span data-ttu-id="11e2a-131">Restaurar, compilar e executar</span><span class="sxs-lookup"><span data-stu-id="11e2a-131">Restore, build, and run</span></span>

<span data-ttu-id="11e2a-132">Execute os comandos sugeridos alterando diretórios para o `MyWebApp` directory.</span><span class="sxs-lookup"><span data-stu-id="11e2a-132">Follow the suggested commands by changing directories to the `MyWebApp` directory.</span></span> <span data-ttu-id="11e2a-133">Em seguida, execute `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="11e2a-133">Then run `dotnet restore`.</span></span>

![Janela Comando](yeoman/_static/dotnet-restore.png)

<span data-ttu-id="11e2a-135">Compilar e executar o aplicativo usando `dotnet build` e `dotnet run`:</span><span class="sxs-lookup"><span data-stu-id="11e2a-135">Build and run the app using `dotnet build` and `dotnet run`:</span></span>

![Janela Comando](yeoman/_static/dotnet-build-run.png)

<span data-ttu-id="11e2a-137">Neste ponto, você pode navegar para a URL mostrada para testar o aplicativo do ASP.NET Core recém-criado.</span><span class="sxs-lookup"><span data-stu-id="11e2a-137">At this point, you can navigate to the URL shown to test the newly-created ASP.NET Core app.</span></span>

## <a name="client-side-packages"></a><span data-ttu-id="11e2a-138">Pacotes do lado do cliente</span><span class="sxs-lookup"><span data-stu-id="11e2a-138">Client-side packages</span></span>

<span data-ttu-id="11e2a-139">Os recursos de front-end são fornecidos pelos modelos a partir de Yeoman usando o gerador de [Bower](xref:client-side/bower) Gerenciador de pacotes do lado do cliente, adicionando *bower. JSON* e *bowerrc* arquivos para restaurar os pacotes do lado do cliente usando o Bower.</span><span class="sxs-lookup"><span data-stu-id="11e2a-139">The front-end resources are provided by the templates from the Yeoman generator using the [Bower](xref:client-side/bower) client-side package manager, adding *bower.json* and *.bowerrc* files to restore client-side packages using Bower.</span></span>

<span data-ttu-id="11e2a-140">O [BundlerMinifier](xref:client-side/bundling-and-minification) componente também é incluído por padrão para facilidade de concatenação (agrupamento) e minimização de CSS, JavaScript e HTML.</span><span class="sxs-lookup"><span data-stu-id="11e2a-140">The [BundlerMinifier](xref:client-side/bundling-and-minification) component is also included by default for ease of concatenation (bundling) and minification of CSS, JavaScript, and HTML.</span></span>

## <a name="building-and-running-from-visual-studio"></a><span data-ttu-id="11e2a-141">Criação e a execução do Visual Studio</span><span class="sxs-lookup"><span data-stu-id="11e2a-141">Building and running from Visual Studio</span></span>

<span data-ttu-id="11e2a-142">Você pode carregar seu projeto de web do ASP.NET Core gerado diretamente no Visual Studio, em seguida, compilar e executar o projeto a partir daí.</span><span class="sxs-lookup"><span data-stu-id="11e2a-142">You can load your generated ASP.NET Core web project directly into Visual Studio, then build and run your project from there.</span></span> <span data-ttu-id="11e2a-143">Siga as instruções acima Scaffold um novo aplicativo ASP.NET Core usando Yeoman.</span><span class="sxs-lookup"><span data-stu-id="11e2a-143">Follow the instructions above to scaffold a new ASP.NET Core app using Yeoman.</span></span> <span data-ttu-id="11e2a-144">Desta vez, escolha **aplicativo Web** no menu e o nome do aplicativo `MyWebApp`.</span><span class="sxs-lookup"><span data-stu-id="11e2a-144">This time, choose **Web Application** from the menu and name the app `MyWebApp`.</span></span>

<span data-ttu-id="11e2a-145">Abra o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="11e2a-145">Open Visual Studio.</span></span> <span data-ttu-id="11e2a-146">No menu Arquivo, selecione Abrir ‣ projeto/solução.</span><span class="sxs-lookup"><span data-stu-id="11e2a-146">From the File menu, select Open ‣ Project/Solution.</span></span>

<span data-ttu-id="11e2a-147">Na caixa de diálogo Abrir projeto, navegue até o *. csproj* de arquivo, selecione-o e clique no **abrir** botão.</span><span class="sxs-lookup"><span data-stu-id="11e2a-147">In the Open Project dialog, navigate to the *.csproj* file, select it, and click the **Open** button.</span></span> <span data-ttu-id="11e2a-148">No Gerenciador de soluções, o projeto deve ser semelhante a captura de tela abaixo.</span><span class="sxs-lookup"><span data-stu-id="11e2a-148">In the Solution Explorer, the project should look something like the screenshot below.</span></span>

![Arquivos e pastas de um novo projeto no Gerenciador de soluções](yeoman/_static/yeoman-solution.png)

<span data-ttu-id="11e2a-150">Yeoman scaffolds um aplicativo da web MVC, compilação do servidor e cliente com ambos suporte.</span><span class="sxs-lookup"><span data-stu-id="11e2a-150">Yeoman scaffolds a MVC web application, complete with both server- and client-side build support.</span></span> <span data-ttu-id="11e2a-151">Dependências do lado do servidor são listadas no **dependências para o NuGet** nó e dependências do lado do cliente no **dependências/Bower** nó do Gerenciador de soluções.</span><span class="sxs-lookup"><span data-stu-id="11e2a-151">Server-side dependencies are listed under the **Dependencies/NuGet** node, and client-side dependencies in the **Dependencies/Bower** node of Solution Explorer.</span></span> <span data-ttu-id="11e2a-152">As dependências são restauradas automaticamente quando o projeto é carregado.</span><span class="sxs-lookup"><span data-stu-id="11e2a-152">Dependencies are restored automatically when the project is loaded.</span></span>

![Sob o nó dependências na exibição de árvore do Gerenciador de soluções, a pasta Bower é abrir lista de suas dependências.](yeoman/_static/yeoman-loading-dependencies.png)

<span data-ttu-id="11e2a-154">Quando todas as dependências são restauradas, pressione **F5** para executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="11e2a-154">When all the dependencies are restored, press **F5** to run the project.</span></span> <span data-ttu-id="11e2a-155">A página inicial padrão exibe no navegador.</span><span class="sxs-lookup"><span data-stu-id="11e2a-155">The default home page displays in the browser.</span></span>

![Aplicativo Web aberto no Microsoft Edge](yeoman/_static/yeoman-home-page.png)

## <a name="restoring-building-and-hosting-from-a-command-line"></a><span data-ttu-id="11e2a-157">Restaurando, criação e hospedagem de uma linha de comando</span><span class="sxs-lookup"><span data-stu-id="11e2a-157">Restoring, building, and hosting from a command line</span></span>

<span data-ttu-id="11e2a-158">Você pode preparar e hospedar seu aplicativo web usando a CLI do .NET Core.</span><span class="sxs-lookup"><span data-stu-id="11e2a-158">You can prepare and host your web application using the .NET Core CLI.</span></span>

<span data-ttu-id="11e2a-159">Em um prompt de comando, altere o diretório atual para a pasta que contém o projeto (ou seja, a pasta que contém o *. csproj* arquivo):</span><span class="sxs-lookup"><span data-stu-id="11e2a-159">At a command prompt, change the current directory to the folder containing the project (that is, the folder containing the *.csproj* file):</span></span>

```console
cd src\MyWebApp
```

<span data-ttu-id="11e2a-160">Restaure as dependências de pacotes do NuGet do projeto:</span><span class="sxs-lookup"><span data-stu-id="11e2a-160">Restore the project's NuGet package dependencies:</span></span>

```console
dotnet restore
```

<span data-ttu-id="11e2a-161">Execute o aplicativo:</span><span class="sxs-lookup"><span data-stu-id="11e2a-161">Run the application:</span></span>

```console
dotnet run
```

<span data-ttu-id="11e2a-162">A plataforma cruzada [Kestrel](xref:fundamentals/servers/kestrel) servidor da web será iniciada a escuta na porta 5000.</span><span class="sxs-lookup"><span data-stu-id="11e2a-162">The cross-platform [Kestrel](xref:fundamentals/servers/kestrel) web server will begin listening on port 5000.</span></span>

<span data-ttu-id="11e2a-163">Abra um navegador da web e navegue até `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="11e2a-163">Open a web browser, and navigate to `http://localhost:5000`.</span></span>

![Aplicativo Web aberto no Microsoft Edge](yeoman/_static/yeoman-home-page_5000.png)

## <a name="adding-to-your-project-with-sub-generators"></a><span data-ttu-id="11e2a-165">Adicionar ao seu projeto com geradores sub</span><span class="sxs-lookup"><span data-stu-id="11e2a-165">Adding to your project with sub generators</span></span>

<span data-ttu-id="11e2a-166">Usando Yeoman [sub geradores](https://github.com/omnisharp/generator-aspnet), você pode adicionar uma `nuget.config` ou um `web.config` depois que o projeto é criado.</span><span class="sxs-lookup"><span data-stu-id="11e2a-166">Using Yeoman [sub generators](https://github.com/omnisharp/generator-aspnet), you can add either a `nuget.config` or a `web.config` after the project is created.</span></span> <span data-ttu-id="11e2a-167">Por exemplo, execute o seguinte comando do diretório no qual o arquivo deve ser criado:</span><span class="sxs-lookup"><span data-stu-id="11e2a-167">For example, execute the following command from the directory in which the file should be created:</span></span>

```console
yo aspnet:nugetconfig
```

<span data-ttu-id="11e2a-168">O resultado é um arquivo de configuração NuGet chamado `nuget.config` com o seguinte conteúdo:</span><span class="sxs-lookup"><span data-stu-id="11e2a-168">The result is a NuGet configuration file named `nuget.config` with the following content:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
 <packageSources>
    <!--To inherit the global NuGet package sources remove the <clear/> line below -->
    <clear />
 </packageSources>
</configuration>
```

## <a name="additional-resources"></a><span data-ttu-id="11e2a-169">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="11e2a-169">Additional resources</span></span>

* [<span data-ttu-id="11e2a-170">Servidores (Kestrel e WebListener)</span><span class="sxs-lookup"><span data-stu-id="11e2a-170">Servers (Kestrel and WebListener)</span></span>](xref:fundamentals/servers/index)
* [<span data-ttu-id="11e2a-171">Conceitos básicos</span><span class="sxs-lookup"><span data-stu-id="11e2a-171">Fundamentals</span></span>](xref:fundamentals/index)
