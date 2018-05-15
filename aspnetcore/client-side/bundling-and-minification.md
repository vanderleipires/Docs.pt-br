---
title: Pacote e minifiy ativos estáticos no núcleo do ASP.NET
author: scottaddie
description: Saiba como otimizar recursos estáticos em um aplicativo ASP.NET Core aplicando técnicas de empacotamento e minimização.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 01/10/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: client-side/bundling-and-minification
ms.openlocfilehash: 381025d1c4a9951349e242482ca9b780c461e588
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/07/2018
---
# <a name="bundle-and-minifiy-static-assets-in-aspnet-core"></a><span data-ttu-id="7d670-103">Pacote e minifiy ativos estáticos no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="7d670-103">Bundle and minifiy static assets in ASP.NET Core</span></span>

<span data-ttu-id="7d670-104">Por [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="7d670-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="7d670-105">Este artigo explica os benefícios da aplicação de empacotamento e minimização, incluindo como esses recursos podem ser usados com aplicativos web do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7d670-105">This article explains the benefits of applying bundling and minification, including how these features can be used with ASP.NET Core web apps.</span></span>

## <a name="what-is-bundling-and-minification"></a><span data-ttu-id="7d670-106">O que é o empacotamento e minimização?</span><span class="sxs-lookup"><span data-stu-id="7d670-106">What is bundling and minification?</span></span>

<span data-ttu-id="7d670-107">Empacotamento e minimização são duas otimizações de desempenho distintos que você pode aplicar em um aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="7d670-107">Bundling and minification are two distinct performance optimizations you can apply in a web app.</span></span> <span data-ttu-id="7d670-108">Usados juntos, empacotamento e minimização melhoram o desempenho reduzindo o número de solicitações do servidor e reduzindo o tamanho dos ativos estáticos solicitados.</span><span class="sxs-lookup"><span data-stu-id="7d670-108">Used together, bundling and minification improve performance by reducing the number of server requests and reducing the size of the requested static assets.</span></span>

<span data-ttu-id="7d670-109">Empacotamento e minimização principalmente melhoram o tempo de carregamento de solicitação de página primeiro.</span><span class="sxs-lookup"><span data-stu-id="7d670-109">Bundling and minification primarily improve the first page request load time.</span></span> <span data-ttu-id="7d670-110">Depois que uma página da web foi solicitada, o navegador armazena em cache os ativos estáticos (JavaScript, CSS e imagens).</span><span class="sxs-lookup"><span data-stu-id="7d670-110">Once a web page has been requested, the browser caches the static assets (JavaScript, CSS, and images).</span></span> <span data-ttu-id="7d670-111">Consequentemente, empacotamento e minimização não melhoram o desempenho ao solicitar a mesma página ou páginas, no mesmo site que está solicitando os mesmos ativos.</span><span class="sxs-lookup"><span data-stu-id="7d670-111">Consequently, bundling and minification don't improve performance when requesting the same page, or pages, on the same site requesting the same assets.</span></span> <span data-ttu-id="7d670-112">Se o vencimento cabeçalho não está definido corretamente nos ativos e se não for usado o empacotamento e minimização, heurística de atualização do navegador marca os ativos obsoletos depois de alguns dias.</span><span class="sxs-lookup"><span data-stu-id="7d670-112">If the expires header isn't set correctly on the assets and if bundling and minification isn't used, the browser's freshness heuristics mark the assets stale after a few days.</span></span> <span data-ttu-id="7d670-113">Além disso, o navegador requer uma solicitação de validação para cada ativo.</span><span class="sxs-lookup"><span data-stu-id="7d670-113">Additionally, the browser requires a validation request for each asset.</span></span> <span data-ttu-id="7d670-114">Nesse caso, empacotamento e minimização fornecem uma melhoria de desempenho mesmo após a primeira solicitação de página.</span><span class="sxs-lookup"><span data-stu-id="7d670-114">In this case, bundling and minification provide a performance improvement even after the first page request.</span></span>

### <a name="bundling"></a><span data-ttu-id="7d670-115">Agrupamento</span><span class="sxs-lookup"><span data-stu-id="7d670-115">Bundling</span></span>

<span data-ttu-id="7d670-116">Agrupando combina vários arquivos em um único arquivo.</span><span class="sxs-lookup"><span data-stu-id="7d670-116">Bundling combines multiple files into a single file.</span></span> <span data-ttu-id="7d670-117">Agrupando reduz o número de solicitações de servidor que são necessários para renderizar um ativo de web, como uma página da web.</span><span class="sxs-lookup"><span data-stu-id="7d670-117">Bundling reduces the number of server requests which are necessary to render a web asset, such as a web page.</span></span> <span data-ttu-id="7d670-118">Você pode criar qualquer número de pacotes individuais especificamente para CSS, JavaScript, etc. Menos arquivos significa menos solicitações HTTP do navegador para o servidor ou do serviço fornecendo o seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7d670-118">You can create any number of individual bundles specifically for CSS, JavaScript, etc. Fewer files means fewer HTTP requests from the browser to the server or from the service providing your application.</span></span> <span data-ttu-id="7d670-119">Isso resulta em melhor desempenho de carregamento de página primeiro.</span><span class="sxs-lookup"><span data-stu-id="7d670-119">This results in improved first page load performance.</span></span>

### <a name="minification"></a><span data-ttu-id="7d670-120">Minimização</span><span class="sxs-lookup"><span data-stu-id="7d670-120">Minification</span></span>

<span data-ttu-id="7d670-121">Minimização remove caracteres desnecessárias de código sem alterar a funcionalidade.</span><span class="sxs-lookup"><span data-stu-id="7d670-121">Minification removes unnecessary characters from code without altering functionality.</span></span> <span data-ttu-id="7d670-122">O resultado é uma redução de tamanho significativo nos ativos solicitados (como CSS, imagens e arquivos JavaScript).</span><span class="sxs-lookup"><span data-stu-id="7d670-122">The result is a significant size reduction in requested assets (such as CSS, images, and JavaScript files).</span></span> <span data-ttu-id="7d670-123">Os efeitos colaterais de minimização incluem encurtar nomes de variável para um caractere e remover comentários e espaços em branco desnecessários.</span><span class="sxs-lookup"><span data-stu-id="7d670-123">Common side effects of minification include shortening variable names to one character and removing comments and unnecessary whitespace.</span></span>

<span data-ttu-id="7d670-124">Considere a função JavaScript a seguir:</span><span class="sxs-lookup"><span data-stu-id="7d670-124">Consider the following JavaScript function:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

<span data-ttu-id="7d670-125">Minimização reduz a função para o seguinte:</span><span class="sxs-lookup"><span data-stu-id="7d670-125">Minification reduces the function to the following:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

<span data-ttu-id="7d670-126">Além de remover os comentários e espaços em branco desnecessários, os seguintes nomes de parâmetro e variável foram renomeados da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="7d670-126">In addition to removing the comments and unnecessary whitespace, the following parameter and variable names were renamed as follows:</span></span>

<span data-ttu-id="7d670-127">Original</span><span class="sxs-lookup"><span data-stu-id="7d670-127">Original</span></span> | <span data-ttu-id="7d670-128">Renomeado</span><span class="sxs-lookup"><span data-stu-id="7d670-128">Renamed</span></span>
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a><span data-ttu-id="7d670-129">Impacto de empacotamento e minimização</span><span class="sxs-lookup"><span data-stu-id="7d670-129">Impact of bundling and minification</span></span>

<span data-ttu-id="7d670-130">A tabela a seguir descreve as diferenças entre carregamento ativos individualmente e usar o empacotamento e minimização:</span><span class="sxs-lookup"><span data-stu-id="7d670-130">The following table outlines differences between individually loading assets and using bundling and minification:</span></span>

<span data-ttu-id="7d670-131">Ação</span><span class="sxs-lookup"><span data-stu-id="7d670-131">Action</span></span> | <span data-ttu-id="7d670-132">Com B/M</span><span class="sxs-lookup"><span data-stu-id="7d670-132">With B/M</span></span> | <span data-ttu-id="7d670-133">Sem B/M</span><span class="sxs-lookup"><span data-stu-id="7d670-133">Without B/M</span></span> | <span data-ttu-id="7d670-134">Alteração</span><span class="sxs-lookup"><span data-stu-id="7d670-134">Change</span></span>
--- | :---: | :---: | :---:
<span data-ttu-id="7d670-135">Solicitações de arquivo</span><span class="sxs-lookup"><span data-stu-id="7d670-135">File Requests</span></span>  | <span data-ttu-id="7d670-136">7</span><span class="sxs-lookup"><span data-stu-id="7d670-136">7</span></span>   | <span data-ttu-id="7d670-137">18</span><span class="sxs-lookup"><span data-stu-id="7d670-137">18</span></span>     | <span data-ttu-id="7d670-138">157%</span><span class="sxs-lookup"><span data-stu-id="7d670-138">157%</span></span>
<span data-ttu-id="7d670-139">KB transferido</span><span class="sxs-lookup"><span data-stu-id="7d670-139">KB Transferred</span></span> | <span data-ttu-id="7d670-140">156</span><span class="sxs-lookup"><span data-stu-id="7d670-140">156</span></span> | <span data-ttu-id="7d670-141">264.68</span><span class="sxs-lookup"><span data-stu-id="7d670-141">264.68</span></span> | <span data-ttu-id="7d670-142">70%</span><span class="sxs-lookup"><span data-stu-id="7d670-142">70%</span></span>
<span data-ttu-id="7d670-143">Tempo de carregamento (ms)</span><span class="sxs-lookup"><span data-stu-id="7d670-143">Load Time (ms)</span></span> | <span data-ttu-id="7d670-144">885</span><span class="sxs-lookup"><span data-stu-id="7d670-144">885</span></span> | <span data-ttu-id="7d670-145">2360</span><span class="sxs-lookup"><span data-stu-id="7d670-145">2360</span></span>   | <span data-ttu-id="7d670-146">167%</span><span class="sxs-lookup"><span data-stu-id="7d670-146">167%</span></span>

<span data-ttu-id="7d670-147">Navegadores são bastante detalhados em relação a cabeçalhos de solicitação HTTP.</span><span class="sxs-lookup"><span data-stu-id="7d670-147">Browsers are fairly verbose with regard to HTTP request headers.</span></span> <span data-ttu-id="7d670-148">O total de bytes enviados métrica viu uma redução significativa ao agrupamento.</span><span class="sxs-lookup"><span data-stu-id="7d670-148">The total bytes sent metric saw a significant reduction when bundling.</span></span> <span data-ttu-id="7d670-149">O tempo de carregamento mostra uma melhoria significativa, no entanto, esse exemplo foi executado localmente.</span><span class="sxs-lookup"><span data-stu-id="7d670-149">The load time shows a significant improvement, however this example ran locally.</span></span> <span data-ttu-id="7d670-150">Maiores ganhos de desempenho são obtidos quando usar o empacotamento e minimização com ativos transferido em uma rede.</span><span class="sxs-lookup"><span data-stu-id="7d670-150">Greater performance gains are realized when using bundling and minification with assets transferred over a network.</span></span>

## <a name="choose-a-bundling-and-minification-strategy"></a><span data-ttu-id="7d670-151">Escolha uma estratégia de empacotamento e minimização</span><span class="sxs-lookup"><span data-stu-id="7d670-151">Choose a bundling and minification strategy</span></span>

<span data-ttu-id="7d670-152">Os modelos de projeto MVC e páginas Razor fornecem uma solução para empacotamento e minimização consiste em um arquivo de configuração JSON.</span><span class="sxs-lookup"><span data-stu-id="7d670-152">The MVC and Razor Pages project templates provide an out-of-the-box solution for bundling and minification consisting of a JSON configuration file.</span></span> <span data-ttu-id="7d670-153">Ferram de terceiros, como o [Gulp](xref:client-side/using-gulp) e [Grunt](xref:client-side/using-grunt) executores de tarefas, realizar as mesmas tarefas com um pouco mais complexidade.</span><span class="sxs-lookup"><span data-stu-id="7d670-153">Third-party tools, such as the [Gulp](xref:client-side/using-gulp) and [Grunt](xref:client-side/using-grunt) task runners, accomplish the same tasks with a bit more complexity.</span></span> <span data-ttu-id="7d670-154">Uma ferramenta de terceiros é uma excelente opção quando o fluxo de trabalho de desenvolvimento requer processamento além de empacotamento e minimização&mdash;como otimização linting e imagem.</span><span class="sxs-lookup"><span data-stu-id="7d670-154">A third-party tool is a great fit when your development workflow requires processing beyond bundling and minification&mdash;such as linting and image optimization.</span></span> <span data-ttu-id="7d670-155">Usando o empacotamento e minimização tempo de design, os arquivos minimizados são criados antes da implantação do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7d670-155">By using design-time bundling and minification, the minified files are created prior to the app's deployment.</span></span> <span data-ttu-id="7d670-156">Empacotando e minimizando antes da implantação tem a vantagem de carga do servidor reduzido.</span><span class="sxs-lookup"><span data-stu-id="7d670-156">Bundling and minifying before deployment provides the advantage of reduced server load.</span></span> <span data-ttu-id="7d670-157">No entanto, é importante reconhecer que o agrupamento de tempo de design e minimização aumenta a complexidade de compilação e só funciona com arquivos estáticos.</span><span class="sxs-lookup"><span data-stu-id="7d670-157">However, it's important to recognize that design-time bundling and minification increases build complexity and only works with static files.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="7d670-158">Configurar o empacotamento e minimização</span><span class="sxs-lookup"><span data-stu-id="7d670-158">Configure bundling and minification</span></span>

<span data-ttu-id="7d670-159">Os modelos de projeto MVC e páginas Razor fornecem uma *bundleconfig.json* arquivo de configuração que define as opções para cada pacote.</span><span class="sxs-lookup"><span data-stu-id="7d670-159">The MVC and Razor Pages project templates provide a *bundleconfig.json* configuration file which defines the options for each bundle.</span></span> <span data-ttu-id="7d670-160">Por padrão, uma configuração de pacote único é definida para o JavaScript personalizado (*wwwroot/js/site.js*) e a folha de estilos (*wwwroot/css/site.css*) arquivos:</span><span class="sxs-lookup"><span data-stu-id="7d670-160">By default, a single bundle configuration is defined for the custom JavaScript (*wwwroot/js/site.js*) and stylesheet (*wwwroot/css/site.css*) files:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

<span data-ttu-id="7d670-161">Opções de configuração incluem:</span><span class="sxs-lookup"><span data-stu-id="7d670-161">Configuration options include:</span></span>

* <span data-ttu-id="7d670-162">`outputFileName`: O nome do arquivo de pacote de saída.</span><span class="sxs-lookup"><span data-stu-id="7d670-162">`outputFileName`: The name of the bundle file to output.</span></span> <span data-ttu-id="7d670-163">Pode conter um caminho relativo do *bundleconfig.json* arquivo.</span><span class="sxs-lookup"><span data-stu-id="7d670-163">Can contain a relative path from the *bundleconfig.json* file.</span></span> <span data-ttu-id="7d670-164">**Necessário**</span><span class="sxs-lookup"><span data-stu-id="7d670-164">**required**</span></span>
* <span data-ttu-id="7d670-165">`inputFiles`: Uma matriz de arquivos para agrupar em conjunto.</span><span class="sxs-lookup"><span data-stu-id="7d670-165">`inputFiles`: An array of files to bundle together.</span></span> <span data-ttu-id="7d670-166">Esses são os caminhos relativos ao arquivo de configuração.</span><span class="sxs-lookup"><span data-stu-id="7d670-166">These are relative paths to the configuration file.</span></span> <span data-ttu-id="7d670-167">**opcional**, \* um valor vazio resulta em um arquivo de saída vazia.</span><span class="sxs-lookup"><span data-stu-id="7d670-167">**optional**, \*an empty value results in an empty output file.</span></span> <span data-ttu-id="7d670-168">[Globalização](http://www.tldp.org/LDP/abs/html/globbingref.html) padrões são suportados.</span><span class="sxs-lookup"><span data-stu-id="7d670-168">[globbing](http://www.tldp.org/LDP/abs/html/globbingref.html) patterns are supported.</span></span>
* <span data-ttu-id="7d670-169">`minify`: As opções de minimização para o tipo de saída.</span><span class="sxs-lookup"><span data-stu-id="7d670-169">`minify`: The minification options for the output type.</span></span> <span data-ttu-id="7d670-170">**opcional**, *padrão: `minify: { enabled: true }`*</span><span class="sxs-lookup"><span data-stu-id="7d670-170">**optional**, *default - `minify: { enabled: true }`*</span></span>
  * <span data-ttu-id="7d670-171">Opções de configuração estão disponíveis por tipo de arquivo de saída.</span><span class="sxs-lookup"><span data-stu-id="7d670-171">Configuration options are available per output file type.</span></span>
    * [<span data-ttu-id="7d670-172">Minificador CSS</span><span class="sxs-lookup"><span data-stu-id="7d670-172">CSS Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [<span data-ttu-id="7d670-173">Minificador de JavaScript</span><span class="sxs-lookup"><span data-stu-id="7d670-173">JavaScript Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [<span data-ttu-id="7d670-174">Minificador de HTML</span><span class="sxs-lookup"><span data-stu-id="7d670-174">HTML Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki)
* <span data-ttu-id="7d670-175">`includeInProject`: O sinalizador que indica se é para adicionar arquivos gerados ao arquivo de projeto.</span><span class="sxs-lookup"><span data-stu-id="7d670-175">`includeInProject`: Flag indicating whether to add generated files to project file.</span></span> <span data-ttu-id="7d670-176">**opcional**, *default - false*</span><span class="sxs-lookup"><span data-stu-id="7d670-176">**optional**, *default - false*</span></span>
* <span data-ttu-id="7d670-177">`sourceMap`: O sinalizador que indica se deve gerar um mapa de origem para o arquivo de pacote.</span><span class="sxs-lookup"><span data-stu-id="7d670-177">`sourceMap`: Flag indicating whether to generate a source map for the bundled file.</span></span> <span data-ttu-id="7d670-178">**opcional**, *default - false*</span><span class="sxs-lookup"><span data-stu-id="7d670-178">**optional**, *default - false*</span></span>
* <span data-ttu-id="7d670-179">`sourceMapRootPath`: O caminho raiz para armazenar o arquivo de mapa de código-fonte gerado.</span><span class="sxs-lookup"><span data-stu-id="7d670-179">`sourceMapRootPath`: The root path for storing the generated source map file.</span></span>

## <a name="build-time-execution-of-bundling-and-minification"></a><span data-ttu-id="7d670-180">Execução de tempo de compilação de empacotamento e minimização</span><span class="sxs-lookup"><span data-stu-id="7d670-180">Build-time execution of bundling and minification</span></span>

<span data-ttu-id="7d670-181">O [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) pacote NuGet permite que a execução de empacotamento e minimização no momento da compilação.</span><span class="sxs-lookup"><span data-stu-id="7d670-181">The [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) NuGet package enables the execution of bundling and minification at build time.</span></span> <span data-ttu-id="7d670-182">O pacote injeta [destinos do MSBuild](/visualstudio/msbuild/msbuild-targets) quais executar compilação e tempo limpo.</span><span class="sxs-lookup"><span data-stu-id="7d670-182">The package injects [MSBuild Targets](/visualstudio/msbuild/msbuild-targets) which run at build and clean time.</span></span> <span data-ttu-id="7d670-183">O *bundleconfig.json* arquivo é analisado pelo processo de compilação para produzir os arquivos de saída com base na configuração de definidos.</span><span class="sxs-lookup"><span data-stu-id="7d670-183">The *bundleconfig.json* file is analyzed by the build process to produce the output files based on the defined configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="7d670-184">BuildBundlerMinifier pertence a um projeto voltado para a comunidade no GitHub para o qual a Microsoft fornece sem suporte.</span><span class="sxs-lookup"><span data-stu-id="7d670-184">BuildBundlerMinifier belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="7d670-185">Problemas que devem ser arquivados [aqui](https://github.com/madskristensen/BundlerMinifier/issues).</span><span class="sxs-lookup"><span data-stu-id="7d670-185">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7d670-186">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7d670-186">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="7d670-187">Adicionar o *BuildBundlerMinifier* pacote ao seu projeto.</span><span class="sxs-lookup"><span data-stu-id="7d670-187">Add the *BuildBundlerMinifier* package to your project.</span></span>

<span data-ttu-id="7d670-188">Compile o projeto.</span><span class="sxs-lookup"><span data-stu-id="7d670-188">Build the project.</span></span> <span data-ttu-id="7d670-189">A seguir é exibido na janela de saída:</span><span class="sxs-lookup"><span data-stu-id="7d670-189">The following appears in the Output window:</span></span>

```console
1>------ Build started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Begin processing bundleconfig.json
1>  Minified wwwroot/css/site.min.css
1>  Minified wwwroot/js/site.min.js
1>Bundler: Done processing bundleconfig.json
1>BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
```

<span data-ttu-id="7d670-190">Limpe o projeto.</span><span class="sxs-lookup"><span data-stu-id="7d670-190">Clean the project.</span></span> <span data-ttu-id="7d670-191">A seguir é exibido na janela de saída:</span><span class="sxs-lookup"><span data-stu-id="7d670-191">The following appears in the Output window:</span></span>

```console
1>------ Clean started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Cleaning output from bundleconfig.json
1>Bundler: Done cleaning output file from bundleconfig.json
========== Clean: 1 succeeded, 0 failed, 0 skipped ==========
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="7d670-192">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="7d670-192">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="7d670-193">Adicionar o *BuildBundlerMinifier* pacote ao seu projeto:</span><span class="sxs-lookup"><span data-stu-id="7d670-193">Add the *BuildBundlerMinifier* package to your project:</span></span>

```console
dotnet add package BuildBundlerMinifier
```

<span data-ttu-id="7d670-194">Se usar o ASP.NET Core 1. x, restaurar o pacote adicionado recentemente:</span><span class="sxs-lookup"><span data-stu-id="7d670-194">If using ASP.NET Core 1.x, restore the newly added package:</span></span>

```console
dotnet restore
```

<span data-ttu-id="7d670-195">Compile o projeto:</span><span class="sxs-lookup"><span data-stu-id="7d670-195">Build the project:</span></span>

```console
dotnet build
```

<span data-ttu-id="7d670-196">Será exibido o seguinte:</span><span class="sxs-lookup"><span data-stu-id="7d670-196">The following appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


    Bundler: Begin processing bundleconfig.json
    Bundler: Done processing bundleconfig.json
    BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
```

<span data-ttu-id="7d670-197">Limpe o projeto:</span><span class="sxs-lookup"><span data-stu-id="7d670-197">Clean the project:</span></span>

```console
dotnet clean
```

<span data-ttu-id="7d670-198">A seguinte saída é exibida:</span><span class="sxs-lookup"><span data-stu-id="7d670-198">The following output appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Cleaning output from bundleconfig.json
  Bundler: Done cleaning output file from bundleconfig.json
```

---

## <a name="ad-hoc-execution-of-bundling-and-minification"></a><span data-ttu-id="7d670-199">Execução ad hoc de empacotamento e minimização</span><span class="sxs-lookup"><span data-stu-id="7d670-199">Ad hoc execution of bundling and minification</span></span>

<span data-ttu-id="7d670-200">É possível executar as tarefas de empacotamento e minimização em uma base ad hoc, sem criar o projeto.</span><span class="sxs-lookup"><span data-stu-id="7d670-200">It's possible to run the bundling and minification tasks on an ad hoc basis, without building the project.</span></span> <span data-ttu-id="7d670-201">Adicionar o [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) pacote NuGet ao seu projeto:</span><span class="sxs-lookup"><span data-stu-id="7d670-201">Add the [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) NuGet package to your project:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

> [!NOTE]
> <span data-ttu-id="7d670-202">BundlerMinifier.Core pertence a um projeto voltado para a comunidade no GitHub para o qual a Microsoft fornece sem suporte.</span><span class="sxs-lookup"><span data-stu-id="7d670-202">BundlerMinifier.Core belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="7d670-203">Problemas que devem ser arquivados [aqui](https://github.com/madskristensen/BundlerMinifier/issues).</span><span class="sxs-lookup"><span data-stu-id="7d670-203">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

<span data-ttu-id="7d670-204">Este pacote estende a CLI do núcleo do .NET para incluir o *dotnet pacote* ferramenta.</span><span class="sxs-lookup"><span data-stu-id="7d670-204">This package extends the .NET Core CLI to include the *dotnet-bundle* tool.</span></span> <span data-ttu-id="7d670-205">O comando a seguir pode ser executado na janela do Console de Gerenciador de pacote (PMC) ou em um shell de comando:</span><span class="sxs-lookup"><span data-stu-id="7d670-205">The following command can be executed in the Package Manager Console (PMC) window or in a command shell:</span></span>

```console
dotnet bundle
```

> [!IMPORTANT]
> <span data-ttu-id="7d670-206">O NuGet Package Manager adiciona as dependências para o arquivo \*. csproj como `<PackageReference />` nós.</span><span class="sxs-lookup"><span data-stu-id="7d670-206">NuGet Package Manager adds dependencies to the \*.csproj file as `<PackageReference />` nodes.</span></span> <span data-ttu-id="7d670-207">O `dotnet bundle` comando está registrado com o .NET Core CLI somente quando um `<DotNetCliToolReference />` nó é usado.</span><span class="sxs-lookup"><span data-stu-id="7d670-207">The `dotnet bundle` command is registered with the .NET Core CLI only when a `<DotNetCliToolReference />` node is used.</span></span> <span data-ttu-id="7d670-208">Modifique o arquivo \*. csproj adequadamente.</span><span class="sxs-lookup"><span data-stu-id="7d670-208">Modify the \*.csproj file accordingly.</span></span>

## <a name="add-files-to-workflow"></a><span data-ttu-id="7d670-209">Adicionar arquivos ao fluxo de trabalho</span><span class="sxs-lookup"><span data-stu-id="7d670-209">Add files to workflow</span></span>

<span data-ttu-id="7d670-210">Considere um exemplo no qual adicional *custom.css* arquivo é adicionado a seguir:</span><span class="sxs-lookup"><span data-stu-id="7d670-210">Consider an example in which an additional *custom.css* file is added resembling the following:</span></span>

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

<span data-ttu-id="7d670-211">Ser minificada *custom.css* e agrupar com *site.css* em uma *site.min.css* de arquivo, adicione o caminho relativo para *bundleconfig.json*:</span><span class="sxs-lookup"><span data-stu-id="7d670-211">To minify *custom.css* and bundle it with *site.css* into a *site.min.css* file, add the relative path to *bundleconfig.json*:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> <span data-ttu-id="7d670-212">Como alternativa, é possível usar o seguinte padrão de globalização:</span><span class="sxs-lookup"><span data-stu-id="7d670-212">Alternatively, the following globbing pattern could be used:</span></span>
>
> ```json
> "inputFiles": ["wwwroot/**/*(*.css|!(*.min.css)"]
> ```
>
> <span data-ttu-id="7d670-213">Esse padrão de globalização corresponde a todos os arquivos CSS e exclui o padrão de arquivo minimizada.</span><span class="sxs-lookup"><span data-stu-id="7d670-213">This globbing pattern matches all CSS files and excludes the minified file pattern.</span></span>

<span data-ttu-id="7d670-214">Compile o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7d670-214">Build the application.</span></span> <span data-ttu-id="7d670-215">Abra *site.min.css* e observe o conteúdo de *custom.css* é acrescentado ao final do arquivo.</span><span class="sxs-lookup"><span data-stu-id="7d670-215">Open *site.min.css* and notice the content of *custom.css* is appended to the end of the file.</span></span>

## <a name="environment-based-bundling-and-minification"></a><span data-ttu-id="7d670-216">Empacotamento e minimização baseado no ambiente</span><span class="sxs-lookup"><span data-stu-id="7d670-216">Environment-based bundling and minification</span></span>

<span data-ttu-id="7d670-217">Como prática recomendada, os arquivos de pacotes e minimizados do seu aplicativo devem ser usados em um ambiente de produção.</span><span class="sxs-lookup"><span data-stu-id="7d670-217">As a best practice, the bundled and minified files of your app should be used in a production environment.</span></span> <span data-ttu-id="7d670-218">Durante o desenvolvimento, os arquivos originais fazer para facilitar a depuração do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7d670-218">During development, the original files make for easier debugging of the app.</span></span>

<span data-ttu-id="7d670-219">Especificar quais arquivos a serem incluídos nas suas páginas usando o [auxiliar de marca de ambiente](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) em exibições.</span><span class="sxs-lookup"><span data-stu-id="7d670-219">Specify which files to include in your pages by using the [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) in your views.</span></span> <span data-ttu-id="7d670-220">O auxiliar de marca de ambiente processa apenas seu conteúdo durante a execução no específico [ambientes](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="7d670-220">The Environment Tag Helper only renders its contents when running in specific [environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="7d670-221">O seguinte `environment` marca processa os arquivos CSS não processados durante a execução no `Development` ambiente:</span><span class="sxs-lookup"><span data-stu-id="7d670-221">The following `environment` tag renders the unprocessed CSS files when running in the `Development` environment:</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7d670-222">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7d670-222">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7d670-223">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7d670-223">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

* * *
<span data-ttu-id="7d670-224">O seguinte `environment` marca processa os arquivos CSS agrupados e minimizados quando executado em um ambiente diferente de `Development`.</span><span class="sxs-lookup"><span data-stu-id="7d670-224">The following `environment` tag renders the bundled and minified CSS files when running in an environment other than `Development`.</span></span> <span data-ttu-id="7d670-225">Por exemplo, em execução em `Production` ou `Staging` dispara o processamento dessas folhas de estilo:</span><span class="sxs-lookup"><span data-stu-id="7d670-225">For example, running in `Production` or `Staging` triggers the rendering of these stylesheets:</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7d670-226">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7d670-226">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7d670-227">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7d670-227">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

* * *
## <a name="consume-bundleconfigjson-from-gulp"></a><span data-ttu-id="7d670-228">Consumir bundleconfig.json de Gulp</span><span class="sxs-lookup"><span data-stu-id="7d670-228">Consume bundleconfig.json from Gulp</span></span>

<span data-ttu-id="7d670-229">Há casos em que o fluxo de trabalho empacotamento e minimização do aplicativo requer processamento adicional.</span><span class="sxs-lookup"><span data-stu-id="7d670-229">There are cases in which an app's bundling and minification workflow requires additional processing.</span></span> <span data-ttu-id="7d670-230">Exemplos incluem a otimização da imagem, a eliminação de cache e processamento de ativos CDN.</span><span class="sxs-lookup"><span data-stu-id="7d670-230">Examples include image optimization, cache busting, and CDN asset processing.</span></span> <span data-ttu-id="7d670-231">Para atender a esses requisitos, que você pode converter o fluxo de trabalho de empacotamento e minimização para usar o Gulp.</span><span class="sxs-lookup"><span data-stu-id="7d670-231">To satisfy these requirements, you can convert the bundling and minification workflow to use Gulp.</span></span>

### <a name="use-the-bundler--minifier-extension"></a><span data-ttu-id="7d670-232">Usar a extensão de empacotador & Minificador</span><span class="sxs-lookup"><span data-stu-id="7d670-232">Use the Bundler & Minifier extension</span></span>

<span data-ttu-id="7d670-233">O Visual Studio [empacotador & Minificador](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) extensão processa a conversão para Gulp.</span><span class="sxs-lookup"><span data-stu-id="7d670-233">The Visual Studio [Bundler & Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) extension handles the conversion to Gulp.</span></span>

> [!NOTE]
> <span data-ttu-id="7d670-234">A extensão de empacotador & Minificador pertence a um projeto voltado para a comunidade no GitHub para o qual a Microsoft fornece sem suporte.</span><span class="sxs-lookup"><span data-stu-id="7d670-234">The Bundler & Minifier extension belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="7d670-235">Problemas que devem ser arquivados [aqui](https://github.com/madskristensen/BundlerMinifier/issues).</span><span class="sxs-lookup"><span data-stu-id="7d670-235">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

<span data-ttu-id="7d670-236">Clique com botão direito do *bundleconfig.json* no Gerenciador de soluções e selecione **empacotador & Minificador** > **converter Gulp...** :</span><span class="sxs-lookup"><span data-stu-id="7d670-236">Right-click the *bundleconfig.json* file in Solution Explorer and select **Bundler & Minifier** > **Convert To Gulp...**:</span></span>

![Converter para Gulp item de menu de contexto](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

<span data-ttu-id="7d670-238">O *gulpfile.js* e *Package. JSON* arquivos são adicionados ao projeto.</span><span class="sxs-lookup"><span data-stu-id="7d670-238">The *gulpfile.js* and *package.json* files are added to the project.</span></span> <span data-ttu-id="7d670-239">O suporte [npm](https://www.npmjs.com/) os pacotes listados no *Package. JSON* do arquivo `devDependencies` seção estão instalados.</span><span class="sxs-lookup"><span data-stu-id="7d670-239">The supporting [npm](https://www.npmjs.com/) packages listed in the *package.json* file's `devDependencies` section are installed.</span></span>

<span data-ttu-id="7d670-240">Execute o seguinte comando na janela de PMC para instalar a CLI Gulp como uma dependência global:</span><span class="sxs-lookup"><span data-stu-id="7d670-240">Run the following command in the PMC window to install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="7d670-241">O *gulpfile.js* leituras de arquivo do *bundleconfig.json* arquivo para as entradas, saídas e as configurações.</span><span class="sxs-lookup"><span data-stu-id="7d670-241">The *gulpfile.js* file reads the *bundleconfig.json* file for the inputs, outputs, and settings.</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a><span data-ttu-id="7d670-242">Converter manualmente</span><span class="sxs-lookup"><span data-stu-id="7d670-242">Convert manually</span></span>

<span data-ttu-id="7d670-243">Se o Visual Studio e/ou a extensão de empacotador & Minificador não estiverem disponível, converta manualmente.</span><span class="sxs-lookup"><span data-stu-id="7d670-243">If Visual Studio and/or the Bundler & Minifier extension aren't available, convert manually.</span></span>

<span data-ttu-id="7d670-244">Adicionar um *Package. JSON* arquivo, com as seguintes `devDependencies`, para a raiz do projeto:</span><span class="sxs-lookup"><span data-stu-id="7d670-244">Add a *package.json* file, with the following `devDependencies`, to the project root:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

<span data-ttu-id="7d670-245">Instalar as dependências, executando o seguinte comando no mesmo nível como *Package. JSON*:</span><span class="sxs-lookup"><span data-stu-id="7d670-245">Install the dependencies by running the following command at the same level as *package.json*:</span></span>

```console
npm i
```

<span data-ttu-id="7d670-246">Instale a CLI Gulp como uma dependência global:</span><span class="sxs-lookup"><span data-stu-id="7d670-246">Install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="7d670-247">Copie o *gulpfile.js* arquivo abaixo da raiz do projeto:</span><span class="sxs-lookup"><span data-stu-id="7d670-247">Copy the *gulpfile.js* file below to the project root:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a><span data-ttu-id="7d670-248">Executar tarefas de Gulp</span><span class="sxs-lookup"><span data-stu-id="7d670-248">Run Gulp tasks</span></span>

<span data-ttu-id="7d670-249">Para disparar a tarefa de minimização Gulp antes que o projeto é compilado no Visual Studio, adicione o seguinte [destino do MSBuild](/visualstudio/msbuild/msbuild-targets) para o arquivo \*. csproj:</span><span class="sxs-lookup"><span data-stu-id="7d670-249">To trigger the Gulp minification task before the project builds in Visual Studio, add the following [MSBuild Target](/visualstudio/msbuild/msbuild-targets) to the \*.csproj file:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

<span data-ttu-id="7d670-250">Neste exemplo, todas as tarefas definidas dentro de `MyPreCompileTarget` destino executar antes de predefinida `Build` destino.</span><span class="sxs-lookup"><span data-stu-id="7d670-250">In this example, any tasks defined within the `MyPreCompileTarget` target run before the predefined `Build` target.</span></span> <span data-ttu-id="7d670-251">Saída semelhante à seguinte é exibida na janela de saída do Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="7d670-251">Output similar to the following appears in Visual Studio's Output window:</span></span>

```console
1>------ Build started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
1>[14:17:49] Using gulpfile C:\BuildBundlerMinifierApp\gulpfile.js
1>[14:17:49] Starting 'min:js'...
1>[14:17:49] Starting 'min:css'...
1>[14:17:49] Starting 'min:html'...
1>[14:17:49] Finished 'min:js' after 83 ms
1>[14:17:49] Finished 'min:css' after 88 ms
========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
```

<span data-ttu-id="7d670-252">Como alternativa, o Explorador do Executador de tarefas do Visual Studio pode ser usado para associar o Gulp tarefas a eventos específicos do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7d670-252">Alternatively, Visual Studio's Task Runner Explorer may be used to bind Gulp tasks to specific Visual Studio events.</span></span> <span data-ttu-id="7d670-253">Consulte [executando tarefas padrão](xref:client-side/using-gulp#running-default-tasks) para obter instruções sobre como fazer isso.</span><span class="sxs-lookup"><span data-stu-id="7d670-253">See [Running default tasks](xref:client-side/using-gulp#running-default-tasks) for instructions on doing that.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7d670-254">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="7d670-254">Additional resources</span></span>

* [<span data-ttu-id="7d670-255">Usar o Gulp</span><span class="sxs-lookup"><span data-stu-id="7d670-255">Use Gulp</span></span>](xref:client-side/using-gulp)
* [<span data-ttu-id="7d670-256">Usar o Grunt</span><span class="sxs-lookup"><span data-stu-id="7d670-256">Use Grunt</span></span>](xref:client-side/using-grunt)
* [<span data-ttu-id="7d670-257">Usar vários ambientes</span><span class="sxs-lookup"><span data-stu-id="7d670-257">Use multiple environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="7d670-258">Auxiliares de marcação</span><span class="sxs-lookup"><span data-stu-id="7d670-258">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
