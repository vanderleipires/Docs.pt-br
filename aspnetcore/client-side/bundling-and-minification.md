---
title: "Empacotamento e minimização no núcleo do ASP.NET"
author: spboyer
description: 
keywords: "ASP.NET Core, empacotamento e minimização, CSS, JavaScript, Minificada, BuildBundlerMinifier"
ms.author: riande
manager: wpickett
ms.date: 02/28/2017
ms.topic: article
ms.assetid: d54230f9-8e5f-4861-a29c-1d3a14e0b0d9
ms.technology: aspnet
ms.prod: aspnet-core
uid: client-side/bundling-and-minification
ms.openlocfilehash: 11528cb2067ced79a09845f9ff78d897da033438
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/22/2017
---
# <a name="bundling-and-minification-in-aspnet-core"></a><span data-ttu-id="3e96f-103">Empacotamento e minimização no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="3e96f-103">Bundling and minification in ASP.NET Core</span></span>

<span data-ttu-id="3e96f-104">Empacotamento e minimização são duas técnicas você pode usar para melhorar o desempenho de carregamento de página para seu aplicativo da web em ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3e96f-104">Bundling and minification are two techniques you can use in ASP.NET to improve page load performance for your web application.</span></span> <span data-ttu-id="3e96f-105">Agrupando combina vários arquivos em um único arquivo.</span><span class="sxs-lookup"><span data-stu-id="3e96f-105">Bundling combines multiple files into a single file.</span></span> <span data-ttu-id="3e96f-106">Minimização executa várias otimizações de código diferente para scripts e CSS, o que resulta em cargas menores.</span><span class="sxs-lookup"><span data-stu-id="3e96f-106">Minification performs a variety of different code optimizations to scripts and CSS, which results in smaller payloads.</span></span> <span data-ttu-id="3e96f-107">Usados juntos, empacotamento e minimização melhora o desempenho de tempo de carregamento, reduzindo o número de solicitações para o servidor e reduzindo o tamanho dos ativos solicitados (como arquivos CSS e JavaScript).</span><span class="sxs-lookup"><span data-stu-id="3e96f-107">Used together, bundling and minification improves load time performance by reducing the number of requests to the server and reducing the size of the requested assets (such as CSS and JavaScript files).</span></span>

<span data-ttu-id="3e96f-108">Este artigo explica os benefícios de usar o empacotamento e minimização, incluindo como esses recursos podem ser usados com aplicativos do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3e96f-108">This article explains the benefits of using bundling and minification, including how these features can be used with ASP.NET Core applications.</span></span>

## <a name="overview"></a><span data-ttu-id="3e96f-109">Visão Geral</span><span class="sxs-lookup"><span data-stu-id="3e96f-109">Overview</span></span>

<span data-ttu-id="3e96f-110">Em aplicativos do ASP.NET Core, há várias opções para Empacotando e minimizando os recursos do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="3e96f-110">In ASP.NET Core apps, there are multiple options for bundling and minifying client-side resources.</span></span> <span data-ttu-id="3e96f-111">Os modelos de núcleo para MVC fornecem uma solução de fora da caixa usando um arquivo de configuração e o pacote BuildBundlerMinifier NuGet.</span><span class="sxs-lookup"><span data-stu-id="3e96f-111">The core templates for MVC provide an out-of-the-box solution using a configuration file and BuildBundlerMinifier NuGet package.</span></span> <span data-ttu-id="3e96f-112">Ferramentas de terceiros, como [Gulp](using-gulp.md) e [Grunt](using-grunt.md) também estão disponíveis para executar as mesmas tarefas devem exigir seus processos de fluxo de trabalho adicional ou complexidades.</span><span class="sxs-lookup"><span data-stu-id="3e96f-112">Third party tools, such as [Gulp](using-gulp.md) and [Grunt](using-grunt.md) are also available to accomplish the same tasks should your processes require additional workflow or complexities.</span></span> <span data-ttu-id="3e96f-113">Usando o empacotamento e minimização tempo de design, os arquivos minimizados são criados antes da implantação do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="3e96f-113">By using design-time bundling and minification, the minified files are created prior to the application’s deployment.</span></span> <span data-ttu-id="3e96f-114">Empacotando e minimizando antes da implantação tem a vantagem de carga do servidor reduzido.</span><span class="sxs-lookup"><span data-stu-id="3e96f-114">Bundling and minifying before deployment provides the advantage of reduced server load.</span></span> <span data-ttu-id="3e96f-115">No entanto, é importante reconhecer que o agrupamento de tempo de design e minimização aumenta a complexidade de compilação e só funciona com arquivos estáticos.</span><span class="sxs-lookup"><span data-stu-id="3e96f-115">However, it’s important to recognize that design-time bundling and minification increases build complexity and only works with static files.</span></span>

<span data-ttu-id="3e96f-116">Empacotamento e minimização principalmente melhoram o tempo de carregamento de solicitação de página primeiro.</span><span class="sxs-lookup"><span data-stu-id="3e96f-116">Bundling and minification primarily improve the first page request load time.</span></span> <span data-ttu-id="3e96f-117">Depois que uma página da web foi solicitada, o navegador armazena em cache os ativos (JavaScript, CSS e imagens) para o empacotamento e minimização não fornecerá qualquer aumento de desempenho ao solicitar a mesma página ou páginas no mesmo site solicitando os mesmo ativos.</span><span class="sxs-lookup"><span data-stu-id="3e96f-117">Once a web page has been requested, the browser caches the assets (JavaScript, CSS and images) so bundling and minification won’t provide any performance boost when requesting the same page, or pages on the same site requesting the same assets.</span></span> <span data-ttu-id="3e96f-118">Se você não definir o expira cabeçalho corretamente em seus ativos e você não usar o empacotamento e minimização, heurística de atualização do navegador marcará os ativos obsoletos depois de alguns dias e o navegador exigirá uma solicitação de validação para cada ativo.</span><span class="sxs-lookup"><span data-stu-id="3e96f-118">If you don’t set the expires header correctly on your assets, and you don’t use bundling and minification, the browser's freshness heuristics will mark the assets stale after a few days and the browser will require a validation request for each asset.</span></span> <span data-ttu-id="3e96f-119">Nesse caso, empacotamento e minimização fornecem um aumento de desempenho mesmo após a primeira solicitação de página.</span><span class="sxs-lookup"><span data-stu-id="3e96f-119">In this case, bundling and minification provide a performance increase even after the first page request.</span></span>

### <a name="bundling"></a><span data-ttu-id="3e96f-120">Agrupamento</span><span class="sxs-lookup"><span data-stu-id="3e96f-120">Bundling</span></span>

<span data-ttu-id="3e96f-121">Agrupamento é um recurso que torna mais fácil combinar ou agrupar vários arquivos em um único arquivo.</span><span class="sxs-lookup"><span data-stu-id="3e96f-121">Bundling is a feature that makes it easy to combine or bundle multiple files into a single file.</span></span> <span data-ttu-id="3e96f-122">Porque o agrupamento combina vários arquivos em um único arquivo, ele reduz o número de solicitações para o servidor que são necessárias para recuperar e exibir um ativo de web, como uma página da web.</span><span class="sxs-lookup"><span data-stu-id="3e96f-122">Because bundling combines multiple files into a single file, it reduces the number of requests to the server that are required to retrieve and display a web asset, such as a web page.</span></span> <span data-ttu-id="3e96f-123">Você pode criar outros pacotes, JavaScript e CSS.</span><span class="sxs-lookup"><span data-stu-id="3e96f-123">You can create CSS, JavaScript and other bundles.</span></span> <span data-ttu-id="3e96f-124">Menos arquivos significa menos solicitações HTTP do seu navegador para o servidor ou do serviço que fornece seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="3e96f-124">Fewer files means fewer HTTP requests from your browser to the server or from the service providing your application.</span></span> <span data-ttu-id="3e96f-125">Isso resulta em melhor desempenho de carregamento de página primeiro.</span><span class="sxs-lookup"><span data-stu-id="3e96f-125">This results in improved first page load performance.</span></span>

### <a name="minification"></a><span data-ttu-id="3e96f-126">Minimização</span><span class="sxs-lookup"><span data-stu-id="3e96f-126">Minification</span></span>

<span data-ttu-id="3e96f-127">Minimização executa várias otimizações de código diferente para reduzir o tamanho dos ativos solicitados (como CSS, imagens, arquivos JavaScript).</span><span class="sxs-lookup"><span data-stu-id="3e96f-127">Minification performs a variety of different code optimizations to reduce the size of requested assets (such as CSS, images, JavaScript files).</span></span> <span data-ttu-id="3e96f-128">Resultados comuns de minimização incluem removendo comentários e espaços em branco desnecessários e encurtar nomes de variável para um caractere.</span><span class="sxs-lookup"><span data-stu-id="3e96f-128">Common results of minification include removing unnecessary white space and comments, and shortening variable names to one character.</span></span>

<span data-ttu-id="3e96f-129">Considere a função JavaScript a seguir:</span><span class="sxs-lookup"><span data-stu-id="3e96f-129">Consider the following JavaScript function:</span></span>

```javascript
AddAltToImg = function (imageTagAndImageID, imageContext) {
  ///<signature>
  ///<summary> Adds an alt tab to the image
  // </summary>
  //<param name="imgElement" type="String">The image selector.</param>
  //<param name="ContextForImage" type="String">The image context.</param>
  ///</signature>
  var imageElement = $(imageTagAndImageID, imageContext);
  imageElement.attr('alt', imageElement.attr('id').replace(/ID/, ''));
}
```

<span data-ttu-id="3e96f-130">Depois de minimização, a função será reduzida para o seguinte:</span><span class="sxs-lookup"><span data-stu-id="3e96f-130">After minification, the function is reduced to the following:</span></span>

```javascript
AddAltToImg=function(t,a){var r=$(t,a);r.attr("alt",r.attr("id").replace(/ID/,""))};
```

<span data-ttu-id="3e96f-131">Além de remover os comentários e espaços em branco desnecessários, os nomes de variáveis e parâmetros a seguir foram renomeados (reduzido) da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="3e96f-131">In addition to removing the comments and unnecessary whitespace, the following parameters and variable names were renamed (shortened) as follows:</span></span>

<span data-ttu-id="3e96f-132">Original</span><span class="sxs-lookup"><span data-stu-id="3e96f-132">Original</span></span> | <span data-ttu-id="3e96f-133">Renomeado</span><span class="sxs-lookup"><span data-stu-id="3e96f-133">Renamed</span></span>
--- | :---:
<span data-ttu-id="3e96f-134">imageTagAndImageID</span><span class="sxs-lookup"><span data-stu-id="3e96f-134">imageTagAndImageID</span></span> | <span data-ttu-id="3e96f-135">t</span><span class="sxs-lookup"><span data-stu-id="3e96f-135">t</span></span>
<span data-ttu-id="3e96f-136">imageContext</span><span class="sxs-lookup"><span data-stu-id="3e96f-136">imageContext</span></span> | <span data-ttu-id="3e96f-137">a</span><span class="sxs-lookup"><span data-stu-id="3e96f-137">a</span></span>
<span data-ttu-id="3e96f-138">imageElement</span><span class="sxs-lookup"><span data-stu-id="3e96f-138">imageElement</span></span> | <span data-ttu-id="3e96f-139">R</span><span class="sxs-lookup"><span data-stu-id="3e96f-139">r</span></span>

## <a name="impact-of-bundling-and-minification"></a><span data-ttu-id="3e96f-140">Impacto de empacotamento e minimização</span><span class="sxs-lookup"><span data-stu-id="3e96f-140">Impact of bundling and minification</span></span>

<span data-ttu-id="3e96f-141">A tabela a seguir mostra várias diferenças importantes entre listando todos os ativos individualmente e usar o empacotamento e minimização em uma página da web simples:</span><span class="sxs-lookup"><span data-stu-id="3e96f-141">The following table shows several important differences between listing all the assets individually and using bundling and minification on a simple web page:</span></span>

<span data-ttu-id="3e96f-142">Ação</span><span class="sxs-lookup"><span data-stu-id="3e96f-142">Action</span></span> | <span data-ttu-id="3e96f-143">Com B/M</span><span class="sxs-lookup"><span data-stu-id="3e96f-143">With B/M</span></span> | <span data-ttu-id="3e96f-144">Sem B/M</span><span class="sxs-lookup"><span data-stu-id="3e96f-144">Without B/M</span></span> | <span data-ttu-id="3e96f-145">Alteração</span><span class="sxs-lookup"><span data-stu-id="3e96f-145">Change</span></span>
--- | :---: | :---: | :---:
<span data-ttu-id="3e96f-146">Solicitações de arquivo</span><span class="sxs-lookup"><span data-stu-id="3e96f-146">File Requests</span></span> |<span data-ttu-id="3e96f-147">7</span><span class="sxs-lookup"><span data-stu-id="3e96f-147">7</span></span> | <span data-ttu-id="3e96f-148">18</span><span class="sxs-lookup"><span data-stu-id="3e96f-148">18</span></span> | <span data-ttu-id="3e96f-149">157%</span><span class="sxs-lookup"><span data-stu-id="3e96f-149">157%</span></span>
<span data-ttu-id="3e96f-150">KB transferido</span><span class="sxs-lookup"><span data-stu-id="3e96f-150">KB Transferred</span></span> | <span data-ttu-id="3e96f-151">156</span><span class="sxs-lookup"><span data-stu-id="3e96f-151">156</span></span> | <span data-ttu-id="3e96f-152">264.68</span><span class="sxs-lookup"><span data-stu-id="3e96f-152">264.68</span></span> | <span data-ttu-id="3e96f-153">70%</span><span class="sxs-lookup"><span data-stu-id="3e96f-153">70%</span></span>
<span data-ttu-id="3e96f-154">Tempo de carregamento (MS)</span><span class="sxs-lookup"><span data-stu-id="3e96f-154">Load Time (MS)</span></span> | <span data-ttu-id="3e96f-155">885</span><span class="sxs-lookup"><span data-stu-id="3e96f-155">885</span></span> | <span data-ttu-id="3e96f-156">2360</span><span class="sxs-lookup"><span data-stu-id="3e96f-156">2360</span></span> | <span data-ttu-id="3e96f-157">167%</span><span class="sxs-lookup"><span data-stu-id="3e96f-157">167%</span></span>

<span data-ttu-id="3e96f-158">Os bytes enviados tinham uma redução significativa com agrupamento como navegadores são bastante detalhados com os cabeçalhos HTTP que se aplicam em solicitações.</span><span class="sxs-lookup"><span data-stu-id="3e96f-158">The bytes sent had a significant reduction with bundling as browsers are fairly verbose with the HTTP headers that they apply on requests.</span></span> <span data-ttu-id="3e96f-159">O tempo de carregamento mostra uma grande melhoria, no entanto, esse exemplo foi executado localmente.</span><span class="sxs-lookup"><span data-stu-id="3e96f-159">The load time shows a big improvement, however this example was run locally.</span></span> <span data-ttu-id="3e96f-160">Você obterá maiores ganhos de desempenho ao usar o empacotamento e minimização com ativos transferido em uma rede.</span><span class="sxs-lookup"><span data-stu-id="3e96f-160">You will get greater gains in performance when using bundling and minification with assets transferred over a network.</span></span>

## <a name="using-bundling-and-minification-in-a-project"></a><span data-ttu-id="3e96f-161">Usando o empacotamento e minimização em um projeto</span><span class="sxs-lookup"><span data-stu-id="3e96f-161">Using bundling and minification in a project</span></span>

<span data-ttu-id="3e96f-162">O modelo de projeto MVC fornece uma `bundleconfig.json` arquivo de configuração que define as opções para cada pacote.</span><span class="sxs-lookup"><span data-stu-id="3e96f-162">The MVC project template provides a `bundleconfig.json` configuration file which defines the options for each bundle.</span></span> <span data-ttu-id="3e96f-163">Por padrão, uma configuração de pacote único é definida para o JavaScript personalizado (`wwwroot/js/site.js`) e a folha de estilos (`wwwroot/css/site.css`) arquivos.</span><span class="sxs-lookup"><span data-stu-id="3e96f-163">By default, a single bundle configuration is defined for the custom JavaScript (`wwwroot/js/site.js`) and Stylesheet (`wwwroot/css/site.css`) files.</span></span>

[!code-json[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/bundleconfig.json)]

<span data-ttu-id="3e96f-164">Opções do pacote incluem:</span><span class="sxs-lookup"><span data-stu-id="3e96f-164">Bundle options include:</span></span>

* <span data-ttu-id="3e96f-165">outputFileName - nome do arquivo de pacote de saída.</span><span class="sxs-lookup"><span data-stu-id="3e96f-165">outputFileName - name of the bundle file to output.</span></span> <span data-ttu-id="3e96f-166">Pode conter um caminho relativo do `bundleconfig.json` arquivo.</span><span class="sxs-lookup"><span data-stu-id="3e96f-166">Can contain a relative path from the `bundleconfig.json` file.</span></span> <span data-ttu-id="3e96f-167">**Necessário**</span><span class="sxs-lookup"><span data-stu-id="3e96f-167">**required**</span></span>
* <span data-ttu-id="3e96f-168">inputFiles - a matriz de arquivos para agrupar em conjunto.</span><span class="sxs-lookup"><span data-stu-id="3e96f-168">inputFiles - array of files to bundle together.</span></span> <span data-ttu-id="3e96f-169">Esses são os caminhos relativos ao arquivo de configuração.</span><span class="sxs-lookup"><span data-stu-id="3e96f-169">These are relative paths to the configuration file.</span></span> <span data-ttu-id="3e96f-170">**opcional**, * um valor vazio resulta em um arquivo de saída vazia.</span><span class="sxs-lookup"><span data-stu-id="3e96f-170">**optional**, *an empty value results in an empty output file.</span></span> <span data-ttu-id="3e96f-171">[Globalização](http://www.tldp.org/LDP/abs/html/globbingref.html) padrões são suportados.</span><span class="sxs-lookup"><span data-stu-id="3e96f-171">[globbing](http://www.tldp.org/LDP/abs/html/globbingref.html) patterns are supported.</span></span>
* <span data-ttu-id="3e96f-172">minificada - opções de minimização para a saída de tipo.</span><span class="sxs-lookup"><span data-stu-id="3e96f-172">minify - minification options for the output type.</span></span> <span data-ttu-id="3e96f-173">**opcional**, *padrão:`minify: { enabled: true }`*</span><span class="sxs-lookup"><span data-stu-id="3e96f-173">**optional**, *default - `minify: { enabled: true }`*</span></span>
  * <span data-ttu-id="3e96f-174">Opções de configuração estão disponíveis por tipo de arquivo de saída.</span><span class="sxs-lookup"><span data-stu-id="3e96f-174">Configuration options are available per output file type.</span></span>
    * [<span data-ttu-id="3e96f-175">Minificador CSS</span><span class="sxs-lookup"><span data-stu-id="3e96f-175">CSS Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [<span data-ttu-id="3e96f-176">Minificador de JavaScript</span><span class="sxs-lookup"><span data-stu-id="3e96f-176">JavaScript Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki)
    * [<span data-ttu-id="3e96f-177">Minificador de HTML</span><span class="sxs-lookup"><span data-stu-id="3e96f-177">HTML Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki)
* <span data-ttu-id="3e96f-178">Incluirnoproj - adicionar arquivos gerados ao arquivo de projeto.</span><span class="sxs-lookup"><span data-stu-id="3e96f-178">includeInProject - add generated files to project file.</span></span> <span data-ttu-id="3e96f-179">**opcional**, *default - false*</span><span class="sxs-lookup"><span data-stu-id="3e96f-179">**optional**, *default - false*</span></span>
* <span data-ttu-id="3e96f-180">sourceMaps - gerar mapas de origem para o arquivo de pacote.</span><span class="sxs-lookup"><span data-stu-id="3e96f-180">sourceMaps - generate source maps for the bundled file.</span></span> <span data-ttu-id="3e96f-181">**opcional**, *default - false*</span><span class="sxs-lookup"><span data-stu-id="3e96f-181">**optional**, *default - false*</span></span>

### <a name="visual-studio-2015--2017"></a><span data-ttu-id="3e96f-182">Visual Studio 2015 / 2017</span><span class="sxs-lookup"><span data-stu-id="3e96f-182">Visual Studio 2015 / 2017</span></span>

<span data-ttu-id="3e96f-183">Abra `bundleconfig.json` no Visual Studio, se seu ambiente não tem a extensão instalada; um prompt é apresentado sugerindo que haja um que pode ajudá-lo a esse tipo de arquivo.</span><span class="sxs-lookup"><span data-stu-id="3e96f-183">Open `bundleconfig.json` in Visual Studio, if your environment does not have the extension installed; a prompt is presented suggesting that there is one that could assist with this file type.</span></span>

![Sugestão de extensão BuildBundlerMinifier](../client-side/bundling-and-minification/_static/bundler-extension-suggestion.png)

<span data-ttu-id="3e96f-185">Exibir extensões de selecionar e instalar o **empacotador & Minificador** extensão (reiniciar requer o Visual Studio).</span><span class="sxs-lookup"><span data-stu-id="3e96f-185">Select View Extensions, and install the **Bundler & Minifier** extension (Requires Visual Studio restart).</span></span>

![Sugestão de extensão BuildBundlerMinifier](../client-side/bundling-and-minification/_static/view-extension.png)

<span data-ttu-id="3e96f-187">Quando a reinicialização estiver concluída, você precisa configurar a compilação para executar os processos de minimizar e agrupando os ativos do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="3e96f-187">When the restart is complete, you need to configure the build to run the processes of minifying and bundling the client-side assets.</span></span> <span data-ttu-id="3e96f-188">Clique com botão direito do `bundleconfig.json` de arquivo e selecione *habilitar pacote na compilação... *.</span><span class="sxs-lookup"><span data-stu-id="3e96f-188">Right-click the `bundleconfig.json` file and select *Enable bundle on build...*.</span></span>

<span data-ttu-id="3e96f-189">Compilar o projeto e o `bundleconfig.json` está incluído no processo de compilação para produzir os arquivos de saída com base na configuração.</span><span class="sxs-lookup"><span data-stu-id="3e96f-189">Build the project, and the `bundleconfig.json` is included in the build process to produce the output files based on the configuration.</span></span>

```console
1>------ Build started: Project: BuildBundlerMinifierExample, Configuration: Debug Any CPU ------
1>
1>Bundler: Begin processing bundleconfig.json
1>Bundler: Done processing bundleconfig.json
1>BuildBundlerMinifierExample -> C:\BuildBundlerMinifierExample\bin\Debug\netcoreapp1.1\BuildBundlerMinifierExample.dll
========== Build: 1 succeeded or up-to-date, 0 failed, 0 skipped ==========
```

### <a name="visual-studio-code-or-command-line"></a><span data-ttu-id="3e96f-190">Código do Visual Studio ou a linha de comando</span><span class="sxs-lookup"><span data-stu-id="3e96f-190">Visual Studio Code or Command Line</span></span>

<span data-ttu-id="3e96f-191">O processo de empacotamento e minimização usando gestos de GUI; de unidade do Visual Studio e a extensão No entanto, os mesmos recursos estão disponíveis com o `dotnet` pacote CLI e BuildBundlerMinifier NuGet.</span><span class="sxs-lookup"><span data-stu-id="3e96f-191">Visual Studio and the extension drive the bundling and minification process using GUI gestures; however, the same capabilities are available with the `dotnet` CLI and BuildBundlerMinifier NuGet package.</span></span>

<span data-ttu-id="3e96f-192">Adicione o pacote NuGet ao seu projeto:</span><span class="sxs-lookup"><span data-stu-id="3e96f-192">Add the NuGet package to your project:</span></span>

```console
dotnet add package BuildBundlerMinifier
```

<span data-ttu-id="3e96f-193">Restaure as dependências:</span><span class="sxs-lookup"><span data-stu-id="3e96f-193">Restore the dependencies:</span></span>

```console
dotnet restore
```

<span data-ttu-id="3e96f-194">Compile o aplicativo:</span><span class="sxs-lookup"><span data-stu-id="3e96f-194">Build the app:</span></span>

```console
dotnet build
```

<span data-ttu-id="3e96f-195">A saída do comando de compilação mostra os resultados da minimização e/ou agrupamento de acordo com o que está configurado.</span><span class="sxs-lookup"><span data-stu-id="3e96f-195">The output from the build command shows the results of the minification and/or bundling according to what is configured.</span></span>

```console
Microsoft (R) Build Engine version 15.1.545.13942
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Begin processing bundleconfig.json
     Minified wwwroot/css/site.min.css
  Bundler: Done processing bundleconfig.json
  BuildBundlerMinifierExample -> /BuildBundlerMinifierExample/bin/Debug/netcoreapp1.0/BuildBundlerMinifierExample.dll
```

## <a name="adding-files"></a><span data-ttu-id="3e96f-196">Adicionando arquivos</span><span class="sxs-lookup"><span data-stu-id="3e96f-196">Adding files</span></span>

<span data-ttu-id="3e96f-197">Neste exemplo, um arquivo CSS adicional é adicionado chamado `custom.css` e configurado para o empacotamento e minimização com `site.css`, resultando em um único `site.min.css`.</span><span class="sxs-lookup"><span data-stu-id="3e96f-197">In this example, an additional CSS file is added called `custom.css` and configured for bundling and minification with `site.css`, resulting in a single `site.min.css`.</span></span>

<span data-ttu-id="3e96f-198">Custom.CSS</span><span class="sxs-lookup"><span data-stu-id="3e96f-198">custom.css</span></span>

```css
.about, [role=main], [role=complementary]
{
    margin-top: 60px;
}

footer
{
    margin-top: 10px;
}
```

<span data-ttu-id="3e96f-199">Adicionar o caminho relativo para `bundleconfig.json`.</span><span class="sxs-lookup"><span data-stu-id="3e96f-199">Add the relative path to `bundleconfig.json`.</span></span>

[!code-json[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/bundleconfig2.json)]

> [!NOTE]
> <span data-ttu-id="3e96f-200">Como alternativa, pode ser usado o padrão de globalização - `"inputFiles": ["wwwroot/**/*(*.css|!(*.min.css)"]` que obtém CSS todos os arquivos e exclui o padrão de arquivo minimizada.</span><span class="sxs-lookup"><span data-stu-id="3e96f-200">Alternatively, the globbing pattern could be used - `"inputFiles": ["wwwroot/**/*(*.css|!(*.min.css)"]` which gets all CSS files and excludes the minified file pattern.</span></span>

<span data-ttu-id="3e96f-201">Compilar o aplicativo e se você abrir `site.min.css`, agora você vai notar que o conteúdo de `custom.css` foi acrescentado ao final do arquivo.</span><span class="sxs-lookup"><span data-stu-id="3e96f-201">Build the application and if you open `site.min.css`, you'll now notice that contents of `custom.css` has been appended to the end of the file.</span></span>

## <a name="controlling-bundling-and-minification"></a><span data-ttu-id="3e96f-202">Controlando o empacotamento e minimização</span><span class="sxs-lookup"><span data-stu-id="3e96f-202">Controlling bundling and minification</span></span>

<span data-ttu-id="3e96f-203">Em geral, você deseja usar os arquivos de pacotes e minimizados do aplicativo somente em um ambiente de produção.</span><span class="sxs-lookup"><span data-stu-id="3e96f-203">In general, you want to use the bundled and minified files of your app only in a production environment.</span></span> <span data-ttu-id="3e96f-204">Durante o desenvolvimento, você deseja usar os arquivos originais, seu aplicativo mais fácil de depurar.</span><span class="sxs-lookup"><span data-stu-id="3e96f-204">During development, you want to use your original files so your app is easier to debug.</span></span>

<span data-ttu-id="3e96f-205">Você pode especificar quais scripts e arquivos CSS para incluir em suas páginas usando o auxiliar de marca de ambiente em suas páginas de layout (consulte [auxiliares de marcação](../mvc/views/tag-helpers/index.md)).</span><span class="sxs-lookup"><span data-stu-id="3e96f-205">You can specify which scripts and CSS files to include in your pages using the environment tag helper in your layout pages (see [Tag Helpers](../mvc/views/tag-helpers/index.md)).</span></span> <span data-ttu-id="3e96f-206">O auxiliar de marca de ambiente renderizará somente seu conteúdo durante a execução em ambientes específicos.</span><span class="sxs-lookup"><span data-stu-id="3e96f-206">The environment tag helper will only render its contents when running in specific environments.</span></span> <span data-ttu-id="3e96f-207">Consulte [trabalhando com vários ambientes](../fundamentals/environments.md) para obter detalhes sobre como especificar o ambiente atual.</span><span class="sxs-lookup"><span data-stu-id="3e96f-207">See [Working with Multiple Environments](../fundamentals/environments.md) for details on specifying the current environment.</span></span>

<span data-ttu-id="3e96f-208">A seguinte marca de ambiente processará os arquivos CSS não processados durante a execução no `Development` ambiente:</span><span class="sxs-lookup"><span data-stu-id="3e96f-208">The following environment tag will render the unprocessed CSS files when running in the `Development` environment:</span></span>

[!code-html[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/Views/Shared/_Layout.cshtml?highlight=3&range=9-12)]

<span data-ttu-id="3e96f-209">Esta marca de ambiente processará os arquivos CSS agrupados e minimizados somente durante a execução no `Production` ou `Staging`:</span><span class="sxs-lookup"><span data-stu-id="3e96f-209">This environment tag will render the bundled and minified CSS files only when running in `Production` or `Staging`:</span></span>

[!code-html[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/Views/Shared/_Layout.cshtml?highlight=5&range=13-18)]

## <a name="consuming-bundleconfigjson-from-gulp"></a><span data-ttu-id="3e96f-210">Consumindo bundleconfig.json de Gulp</span><span class="sxs-lookup"><span data-stu-id="3e96f-210">Consuming bundleconfig.json from Gulp</span></span>

<span data-ttu-id="3e96f-211">Se o fluxo de trabalho de empacotamento e minimização de aplicativo exige processos adicionais, como o processamento de imagem, a eliminação de cache, processamento de assest CDN, etc., você pode converter o processo de pacote e Minify em Gulp.</span><span class="sxs-lookup"><span data-stu-id="3e96f-211">If your app bundling and minification workflow requires additional processes such as image processing, cache busting, CDN assest processing, etc., then you can convert the Bundle and Minify process to Gulp.</span></span>

> [!NOTE]
> <span data-ttu-id="3e96f-212">Opção de conversão disponível apenas no Visual Studio 2015 e 2017.</span><span class="sxs-lookup"><span data-stu-id="3e96f-212">Conversion option only available in Visual Studio 2015 and 2017.</span></span>

<span data-ttu-id="3e96f-213">Clique com botão direito do `bundleconfig.json` e selecione **converter Gulp... **. Isso irá gerar o `gulpfile.js` e instalar os pacotes necessários npm.</span><span class="sxs-lookup"><span data-stu-id="3e96f-213">Right-click the `bundleconfig.json` and select **Convert to Gulp...**. This will generate the `gulpfile.js` and install the necessary npm packages.</span></span>

![Converter em Gulp](../client-side/bundling-and-minification/_static/convert-togulp.png)

<span data-ttu-id="3e96f-215">O `gulpfile.js` produzido leituras de `bundleconfig.json` arquivo para a configuração, portanto, ele pode continuar a ser usado para as entradas/saídas e as configurações.</span><span class="sxs-lookup"><span data-stu-id="3e96f-215">The `gulpfile.js` produced reads the `bundleconfig.json` file for the configuration, therefore it can continue to be used for the inputs/outputs and settings.</span></span>

[!code-javascript[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/gulpfile.js)]

<span data-ttu-id="3e96f-216">Para habilitar o Gulp quando o projeto é compilado no Visual Studio de 2017, adicione o seguinte para o arquivo *. csproj:</span><span class="sxs-lookup"><span data-stu-id="3e96f-216">To enable Gulp when the project builds in Visual Studio 2017, add the following to the *.csproj file:</span></span>

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
    <Exec Command="gulp min" />
</Target>
```

<span data-ttu-id="3e96f-217">Para habilitar o Gulp quando o projeto é compilado no Visual Studio 2015, adicione o seguinte para o `project.json` arquivo:</span><span class="sxs-lookup"><span data-stu-id="3e96f-217">To enable Gulp when the project builds in Visual Studio 2015, add the following to the `project.json` file:</span></span>

```json
"scripts": {
    "precompile": "gulp min"
}
```

## <a name="additional-resources"></a><span data-ttu-id="3e96f-218">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="3e96f-218">Additional resources</span></span>

* [<span data-ttu-id="3e96f-219">Usando o Gulp</span><span class="sxs-lookup"><span data-stu-id="3e96f-219">Using Gulp</span></span>](using-gulp.md)
* [<span data-ttu-id="3e96f-220">Usando o Grunt</span><span class="sxs-lookup"><span data-stu-id="3e96f-220">Using Grunt</span></span>](using-grunt.md)
* [<span data-ttu-id="3e96f-221">Trabalhando com vários ambientes</span><span class="sxs-lookup"><span data-stu-id="3e96f-221">Working with Multiple Environments</span></span>](../fundamentals/environments.md)
* [<span data-ttu-id="3e96f-222">Auxiliares de marcação</span><span class="sxs-lookup"><span data-stu-id="3e96f-222">Tag Helpers</span></span>](../mvc/views/tag-helpers/index.md)
