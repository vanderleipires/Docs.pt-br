---
title: "Usando JavaScriptServices para criar aplicativos de única página"
author: scottaddie
description: "Saiba mais sobre os benefícios de usar JavaScriptServices para criar um aplicativo de página única (SPA) com o apoio de ASP.NET Core."
keywords: "Núcleo do ASP.NET, Angular, SPA, JavaScriptServices, SpaServices"
ms.author: scaddie
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.assetid: 4b30576b-2718-4c39-9253-a59966747893
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/spa-services
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f3c38a1730e43586f37cd773bb8daa418736952f
ms.sourcegitcommit: b3d46df910fb679edb8dd47234db6b4da604eedb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/10/2017
---
# <a name="using-javascriptservices-for-creating-single-page-applications-with-aspnet-core"></a><span data-ttu-id="1c805-104">Usando JavaScriptServices para criar aplicativos de única página com o ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1c805-104">Using JavaScriptServices for Creating Single Page Applications with ASP.NET Core</span></span>

<span data-ttu-id="1c805-105">Por [Scott Addie](https://github.com/scottaddie) e [Fiyaz Hasan](http://fiyazhasan.me/)</span><span class="sxs-lookup"><span data-stu-id="1c805-105">By [Scott Addie](https://github.com/scottaddie) and [Fiyaz Hasan](http://fiyazhasan.me/)</span></span>

<span data-ttu-id="1c805-106">Um aplicativo de página única (SPA) é um tipo popular de aplicativo da web devido a sua experiência de usuário avançada inerente.</span><span class="sxs-lookup"><span data-stu-id="1c805-106">A Single Page Application (SPA) is a popular type of web application due to its inherent rich user experience.</span></span> <span data-ttu-id="1c805-107">Integração de estruturas SPA ou bibliotecas de cliente, como [Angular](https://angular.io/) ou [reagir](https://facebook.github.io/react/), com as estruturas do lado do servidor como o ASP.NET Core pode ser difícil.</span><span class="sxs-lookup"><span data-stu-id="1c805-107">Integrating client-side SPA frameworks or libraries, such as [Angular](https://angular.io/) or [React](https://facebook.github.io/react/), with server-side frameworks like ASP.NET Core can be difficult.</span></span> <span data-ttu-id="1c805-108">[JavaScriptServices](https://github.com/aspnet/JavaScriptServices) foi desenvolvido para reduzir a fricção no processo de integração.</span><span class="sxs-lookup"><span data-stu-id="1c805-108">[JavaScriptServices](https://github.com/aspnet/JavaScriptServices) was developed to reduce friction in the integration process.</span></span> <span data-ttu-id="1c805-109">Ele permite que uma operação contínua entre o cliente diferentes e pilhas de tecnologia do servidor.</span><span class="sxs-lookup"><span data-stu-id="1c805-109">It enables seamless operation between the different client and server technology stacks.</span></span>

<span data-ttu-id="1c805-110">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/spa-services/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1c805-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/spa-services/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<a name="what-is-js-services"></a>

## <a name="what-is-javascriptservices"></a><span data-ttu-id="1c805-111">O que é JavaScriptServices?</span><span class="sxs-lookup"><span data-stu-id="1c805-111">What is JavaScriptServices?</span></span>

<span data-ttu-id="1c805-112">JavaScriptServices é uma coleção de tecnologias de cliente para o ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1c805-112">JavaScriptServices is a collection of client-side technologies for ASP.NET Core.</span></span> <span data-ttu-id="1c805-113">Sua meta é posicionar o ASP.NET Core como plataforma de lado do servidor preferencial dos desenvolvedores para a criação de SPAs.</span><span class="sxs-lookup"><span data-stu-id="1c805-113">Its goal is to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span>

<span data-ttu-id="1c805-114">JavaScriptServices consiste em três pacotes do NuGet distintos:</span><span class="sxs-lookup"><span data-stu-id="1c805-114">JavaScriptServices consists of three distinct NuGet packages:</span></span>
* <span data-ttu-id="1c805-115">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span><span class="sxs-lookup"><span data-stu-id="1c805-115">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span></span>
* <span data-ttu-id="1c805-116">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span><span class="sxs-lookup"><span data-stu-id="1c805-116">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span></span>
* <span data-ttu-id="1c805-117">[Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)</span><span class="sxs-lookup"><span data-stu-id="1c805-117">[Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)</span></span>

<span data-ttu-id="1c805-118">Esses pacotes são úteis se você:</span><span class="sxs-lookup"><span data-stu-id="1c805-118">These packages are useful if you:</span></span>
* <span data-ttu-id="1c805-119">Executar o JavaScript no servidor</span><span class="sxs-lookup"><span data-stu-id="1c805-119">Run JavaScript on the server</span></span>
* <span data-ttu-id="1c805-120">Use uma estrutura SPA ou biblioteca</span><span class="sxs-lookup"><span data-stu-id="1c805-120">Use a SPA framework or library</span></span>
* <span data-ttu-id="1c805-121">Criar ativos do lado do cliente com Webpack</span><span class="sxs-lookup"><span data-stu-id="1c805-121">Build client-side assets with Webpack</span></span>

<span data-ttu-id="1c805-122">O foco deste artigo é colocado sobre como usar o pacote de SpaServices.</span><span class="sxs-lookup"><span data-stu-id="1c805-122">Much of the focus in this article is placed on using the SpaServices package.</span></span>

<a name="what-is-spa-services"></a>

## <a name="what-is-spaservices"></a><span data-ttu-id="1c805-123">O que é SpaServices?</span><span class="sxs-lookup"><span data-stu-id="1c805-123">What is SpaServices?</span></span>

<span data-ttu-id="1c805-124">SpaServices foi criado para posicionar o ASP.NET Core como plataforma de lado do servidor preferencial dos desenvolvedores para a criação de SPAs.</span><span class="sxs-lookup"><span data-stu-id="1c805-124">SpaServices was created to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span> <span data-ttu-id="1c805-125">SpaServices não é necessário para desenvolver SPAs com ASP.NET Core, e ele não bloqueie você em uma estrutura de cliente específico.</span><span class="sxs-lookup"><span data-stu-id="1c805-125">SpaServices is not required to develop SPAs with ASP.NET Core, and it doesn't lock you into a particular client framework.</span></span>

<span data-ttu-id="1c805-126">SpaServices fornece a infraestrutura úteis, como:</span><span class="sxs-lookup"><span data-stu-id="1c805-126">SpaServices provides useful infrastructure such as:</span></span>
* [<span data-ttu-id="1c805-127">Pré-processamento do lado do servidor</span><span class="sxs-lookup"><span data-stu-id="1c805-127">Server-side prerendering</span></span>](#server-prerendering)
* [<span data-ttu-id="1c805-128">Middleware de desenvolvimento webpack</span><span class="sxs-lookup"><span data-stu-id="1c805-128">Webpack Dev Middleware</span></span>](#webpack-dev-middleware)
* [<span data-ttu-id="1c805-129">Substituição do módulo ativa</span><span class="sxs-lookup"><span data-stu-id="1c805-129">Hot Module Replacement</span></span>](#hot-module-replacement)
* [<span data-ttu-id="1c805-130">Roteamentos auxiliares</span><span class="sxs-lookup"><span data-stu-id="1c805-130">Routing helpers</span></span>](#routing-helpers)

<span data-ttu-id="1c805-131">Coletivamente, esses componentes de infraestrutura melhoram o fluxo de trabalho de desenvolvimento e a experiência de tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="1c805-131">Collectively, these infrastructure components enhance both the development workflow and the runtime experience.</span></span> <span data-ttu-id="1c805-132">Os componentes podem ser adotados individualmente.</span><span class="sxs-lookup"><span data-stu-id="1c805-132">The components can be adopted individually.</span></span>

<a name="spa-services-prereqs"></a>

## <a name="prerequisites-for-using-spaservices"></a><span data-ttu-id="1c805-133">Pré-requisitos para usar SpaServices</span><span class="sxs-lookup"><span data-stu-id="1c805-133">Prerequisites for using SpaServices</span></span>

<span data-ttu-id="1c805-134">Para trabalhar com SpaServices, instale o seguinte:</span><span class="sxs-lookup"><span data-stu-id="1c805-134">To work with SpaServices, install the following:</span></span>
* <span data-ttu-id="1c805-135">[Node. js](https://nodejs.org/) (versão 6 ou posterior) com npm</span><span class="sxs-lookup"><span data-stu-id="1c805-135">[Node.js](https://nodejs.org/) (version 6 or later) with npm</span></span>
    * <span data-ttu-id="1c805-136">Para verificar se esses componentes estão instalados e podem ser encontrados, execute o seguinte na linha de comando:</span><span class="sxs-lookup"><span data-stu-id="1c805-136">To verify these components are installed and can be found, run the following from the command line:</span></span>

    ```console
    node -v && npm -v
    ```

<span data-ttu-id="1c805-137">Observação: Se você estiver implantando em um site do Azure, você não precisa fazer nada aqui &mdash; Node. js está instalado e disponível em ambientes de servidor.</span><span class="sxs-lookup"><span data-stu-id="1c805-137">Note: If you're deploying to an Azure web site, you don't need to do anything here &mdash; Node.js is installed and available in the server environments.</span></span>

* <span data-ttu-id="1c805-138">[SDK do .NET core](https://www.microsoft.com/net/download/core) 1.0 (ou posterior)</span><span class="sxs-lookup"><span data-stu-id="1c805-138">[.NET Core SDK](https://www.microsoft.com/net/download/core) 1.0 (or later)</span></span>
    * <span data-ttu-id="1c805-139">Se você estiver no Windows, isso pode ser instalado, selecionando o Visual Studio 2017 **desenvolvimento de plataforma cruzada do .NET Core** carga de trabalho.</span><span class="sxs-lookup"><span data-stu-id="1c805-139">If you're on Windows, this can be installed by selecting Visual Studio 2017's **.NET Core cross-platform development** workload.</span></span>

* <span data-ttu-id="1c805-140">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) pacote NuGet</span><span class="sxs-lookup"><span data-stu-id="1c805-140">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet package</span></span>

<a name="server-prerendering"></a>

## <a name="server-side-prerendering"></a><span data-ttu-id="1c805-141">Pré-processamento do lado do servidor</span><span class="sxs-lookup"><span data-stu-id="1c805-141">Server-side prerendering</span></span>

<span data-ttu-id="1c805-142">Um universal (também conhecido como isomórficos) é um aplicativo JavaScript capaz de executar tanto no servidor e o cliente.</span><span class="sxs-lookup"><span data-stu-id="1c805-142">A universal (also known as isomorphic) application is a JavaScript application capable of running both on the server and the client.</span></span> <span data-ttu-id="1c805-143">Angular, reagir e outras estruturas populares fornecem uma plataforma universal para esse estilo de desenvolvimento de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1c805-143">Angular, React, and other popular frameworks provide a universal platform for this application development style.</span></span> <span data-ttu-id="1c805-144">A ideia é primeiro renderizar os componentes do framework no servidor por meio do Node. js e, em seguida, delegado ainda mais a execução para o cliente.</span><span class="sxs-lookup"><span data-stu-id="1c805-144">The idea is to first render the framework components on the server via Node.js, and then delegate further execution to the client.</span></span>

<span data-ttu-id="1c805-145">ASP.NET Core [auxiliares de marcação](xref:mvc/views/tag-helpers/intro) fornecida pelo SpaServices simplificar a implementação de pré-processamento do lado do servidor ao chamar as funções JavaScript no servidor.</span><span class="sxs-lookup"><span data-stu-id="1c805-145">ASP.NET Core [Tag Helpers](xref:mvc/views/tag-helpers/intro) provided by SpaServices simplify the implementation of server-side prerendering by invoking the JavaScript functions on the server.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="1c805-146">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="1c805-146">Prerequisites</span></span>

<span data-ttu-id="1c805-147">Instale o seguinte:</span><span class="sxs-lookup"><span data-stu-id="1c805-147">Install the following:</span></span>
* <span data-ttu-id="1c805-148">[pré-processamento ASPNET](https://www.npmjs.com/package/aspnet-prerendering) pacote npm:</span><span class="sxs-lookup"><span data-stu-id="1c805-148">[aspnet-prerendering](https://www.npmjs.com/package/aspnet-prerendering) npm package:</span></span>

    ```console
    npm i -S aspnet-prerendering
    ```

### <a name="configuration"></a><span data-ttu-id="1c805-149">Configuração</span><span class="sxs-lookup"><span data-stu-id="1c805-149">Configuration</span></span>

<span data-ttu-id="1c805-150">Os auxiliares de marca são feitos detectáveis por meio de registro de namespace do projeto *viewimports. cshtml* arquivo:</span><span class="sxs-lookup"><span data-stu-id="1c805-150">The Tag Helpers are made discoverable via namespace registration in the project's *_ViewImports.cshtml* file:</span></span>

[!code-csharp[Main](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

<span data-ttu-id="1c805-151">Esses auxiliares de marcação abstrair a complexidade de se comunicar diretamente com as APIs de baixo nível utilizando uma sintaxe semelhante do HTML no modo de exibição Razor:</span><span class="sxs-lookup"><span data-stu-id="1c805-151">These Tag Helpers abstract away the intricacies of communicating directly with low-level APIs by leveraging an HTML-like syntax inside the Razor view:</span></span>

[!code-html[Main](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="the-asp-prerender-module-tag-helper"></a><span data-ttu-id="1c805-152">O `asp-prerender-module` auxiliar de marca</span><span class="sxs-lookup"><span data-stu-id="1c805-152">The `asp-prerender-module` Tag Helper</span></span>

<span data-ttu-id="1c805-153">O `asp-prerender-module` auxiliar de marca, usados no exemplo de código anterior, executa *ClientApp/dist/main-server.js* no servidor por meio do Node. js.</span><span class="sxs-lookup"><span data-stu-id="1c805-153">The `asp-prerender-module` Tag Helper, used in the preceding code example, executes *ClientApp/dist/main-server.js* on the server via Node.js.</span></span> <span data-ttu-id="1c805-154">Para clareza, *principal server.js* arquivo é um artefato da tarefa transpilation TypeScript e JavaScript no [Webpack](http://webpack.github.io/) do processo de compilação.</span><span class="sxs-lookup"><span data-stu-id="1c805-154">For clarity's sake, *main-server.js* file is an artifact of the TypeScript-to-JavaScript transpilation task in the [Webpack](http://webpack.github.io/) build process.</span></span> <span data-ttu-id="1c805-155">Webpack define um alias de ponto de entrada de `main-server`; e, passagem do gráfico de dependência para este alias começa a *inicialização/ClientApp-server.ts* arquivo:</span><span class="sxs-lookup"><span data-stu-id="1c805-155">Webpack defines an entry point alias of `main-server`; and, traversal of the dependency graph for this alias begins at the *ClientApp/boot-server.ts* file:</span></span>

[!code-javascript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

<span data-ttu-id="1c805-156">No exemplo a seguir Angular, o *inicialização/ClientApp-server.ts* arquivo utiliza o `createServerRenderer` função e `RenderResult` tipo do `aspnet-prerendering` pacote npm para configurar o processamento de servidor por meio do Node. js.</span><span class="sxs-lookup"><span data-stu-id="1c805-156">In the following Angular example, the *ClientApp/boot-server.ts* file utilizes the `createServerRenderer` function and `RenderResult` type of the `aspnet-prerendering` npm package to configure server rendering via Node.js.</span></span> <span data-ttu-id="1c805-157">A marcação HTML destinada a renderização do lado do servidor é passada para uma chamada de função de resolução, que é encapsulada em um JavaScript fortemente tipada `Promise` objeto.</span><span class="sxs-lookup"><span data-stu-id="1c805-157">The HTML markup destined for server-side rendering is passed to a resolve function call, which is wrapped in a strongly-typed JavaScript `Promise` object.</span></span> <span data-ttu-id="1c805-158">O `Promise` significância do objeto é que ele fornece assincronamente a marcação HTML para a página para inclusão no elemento de espaço reservado do DOM.</span><span class="sxs-lookup"><span data-stu-id="1c805-158">The `Promise` object's significance is that it asynchronously supplies the HTML markup to the page for injection in the DOM's placeholder element.</span></span>

[!code-typescript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="the-asp-prerender-data-tag-helper"></a><span data-ttu-id="1c805-159">O `asp-prerender-data` auxiliar de marca</span><span class="sxs-lookup"><span data-stu-id="1c805-159">The `asp-prerender-data` Tag Helper</span></span>

<span data-ttu-id="1c805-160">Quando combinado com o `asp-prerender-module` auxiliar de marca, o `asp-prerender-data` auxiliar de marca pode ser usado para passar informações contextuais da exibição do Razor para o JavaScript do lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="1c805-160">When coupled with the `asp-prerender-module` Tag Helper, the `asp-prerender-data` Tag Helper can be used to pass contextual information from the Razor view to the server-side JavaScript.</span></span> <span data-ttu-id="1c805-161">Por exemplo, a seguinte marcação transmite dados de usuário para o `main-server` módulo:</span><span class="sxs-lookup"><span data-stu-id="1c805-161">For example, the following markup passes user data to the `main-server` module:</span></span>

[!code-html[Main](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

<span data-ttu-id="1c805-162">Recebido `UserName` argumento é serializado usando o serializador JSON interno e é armazenado no `params.data` objeto.</span><span class="sxs-lookup"><span data-stu-id="1c805-162">The received `UserName` argument is serialized using the built-in JSON serializer and is stored in the `params.data` object.</span></span> <span data-ttu-id="1c805-163">No exemplo a seguir Angular, os dados são usados para construir uma saudação personalizada em um `h1` elemento:</span><span class="sxs-lookup"><span data-stu-id="1c805-163">In the following Angular example, the data is used to construct a personalized greeting within an `h1` element:</span></span>

[!code-typescript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

<span data-ttu-id="1c805-164">Observação: Os nomes de propriedade passados auxiliares de marca são representados com **PascalCase** notação.</span><span class="sxs-lookup"><span data-stu-id="1c805-164">Note: Property names passed in Tag Helpers are represented with **PascalCase** notation.</span></span> <span data-ttu-id="1c805-165">Compare isso com a do JavaScript, onde os mesmos nomes de propriedade são representados com **camelCase**.</span><span class="sxs-lookup"><span data-stu-id="1c805-165">Contrast that to JavaScript, where the same property names are represented with **camelCase**.</span></span> <span data-ttu-id="1c805-166">A configuração de serialização JSON padrão é responsável por essa diferença.</span><span class="sxs-lookup"><span data-stu-id="1c805-166">The default JSON serialization configuration is responsible for this difference.</span></span>

<span data-ttu-id="1c805-167">Para expandir o exemplo de código anterior, dados podem ser transmitidos do servidor para o modo de exibição por hydrating o `globals` propriedade fornecida para o `resolve` função:</span><span class="sxs-lookup"><span data-stu-id="1c805-167">To expand upon the preceding code example, data can be passed from the server to the view by hydrating the `globals` property provided to the `resolve` function:</span></span>

[!code-typescript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

<span data-ttu-id="1c805-168">O `postList` matriz definida dentro do `globals` objeto está anexado no navegador global `window` objeto.</span><span class="sxs-lookup"><span data-stu-id="1c805-168">The `postList` array defined inside the `globals` object is attached to the browser's global `window` object.</span></span> <span data-ttu-id="1c805-169">Essa variável suspensão em escopo global elimina a duplicação de esforço, particularmente pois ela pertence ao carregar os mesmos dados uma vez no servidor e novamente no cliente.</span><span class="sxs-lookup"><span data-stu-id="1c805-169">This variable hoisting to global scope eliminates duplication of effort, particularly as it pertains to loading the same data once on the server and again on the client.</span></span>

![variável global de postList anexado ao objeto de janela](spa-services/_static/global_variable.png)

<a name="webpack-dev-middleware"></a>

## <a name="webpack-dev-middleware"></a><span data-ttu-id="1c805-171">Middleware de desenvolvimento webpack</span><span class="sxs-lookup"><span data-stu-id="1c805-171">Webpack Dev Middleware</span></span>

<span data-ttu-id="1c805-172">[Middleware de desenvolvimento webpack](https://webpack.github.io/docs/webpack-dev-middleware.html) introduz um fluxo de trabalho de desenvolvimento simplificado por meio do qual Webpack cria recursos sob demanda.</span><span class="sxs-lookup"><span data-stu-id="1c805-172">[Webpack Dev Middleware](https://webpack.github.io/docs/webpack-dev-middleware.html) introduces a streamlined development workflow whereby Webpack builds resources on demand.</span></span> <span data-ttu-id="1c805-173">O middleware automaticamente compila e serve de recursos do cliente quando uma página é recarregada no navegador.</span><span class="sxs-lookup"><span data-stu-id="1c805-173">The middleware automatically compiles and serves client-side resources when a page is reloaded in the browser.</span></span> <span data-ttu-id="1c805-174">A abordagem alternativa é invocar Webpack manualmente por meio do script de compilação do projeto npm quando uma dependência de terceiros ou o código personalizado é alterado.</span><span class="sxs-lookup"><span data-stu-id="1c805-174">The alternate approach is to manually invoke Webpack via the project's npm build script when a third-party dependency or the custom code changes.</span></span> <span data-ttu-id="1c805-175">Script de compilação de um npm *Package. JSON* arquivo é mostrado no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="1c805-175">An npm build script in the *package.json* file is shown in the following example:</span></span>

[!code-json[Main](../client-side/spa-services/sample/SpaServicesSampleApp/package.json?range=5)]

### <a name="prerequisites"></a><span data-ttu-id="1c805-176">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="1c805-176">Prerequisites</span></span>

<span data-ttu-id="1c805-177">Instale o seguinte:</span><span class="sxs-lookup"><span data-stu-id="1c805-177">Install the following:</span></span>
* <span data-ttu-id="1c805-178">[ASPNET webpack](https://www.npmjs.com/package/aspnet-webpack) pacote npm:</span><span class="sxs-lookup"><span data-stu-id="1c805-178">[aspnet-webpack](https://www.npmjs.com/package/aspnet-webpack) npm package:</span></span>

    ```console
    npm i -D aspnet-webpack
    ```

### <a name="configuration"></a><span data-ttu-id="1c805-179">Configuração</span><span class="sxs-lookup"><span data-stu-id="1c805-179">Configuration</span></span>

<span data-ttu-id="1c805-180">Middleware de desenvolvimento webpack está registrado no pipeline de solicitação HTTP por meio de código a seguir no *Startup.cs* do arquivo `Configure` método:</span><span class="sxs-lookup"><span data-stu-id="1c805-180">Webpack Dev Middleware is registered into the HTTP request pipeline via the following code in the *Startup.cs* file's `Configure` method:</span></span>

[!code-csharp[Main](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=webpack-middleware-registration&highlight=4)]

<span data-ttu-id="1c805-181">O `UseWebpackDevMiddleware` método de extensão deve ser chamado antes de [Registrando a hospedagem de arquivo estático](xref:fundamentals/static-files) por meio de `UseStaticFiles` método de extensão.</span><span class="sxs-lookup"><span data-stu-id="1c805-181">The `UseWebpackDevMiddleware` extension method must be called before [registering static file hosting](xref:fundamentals/static-files) via the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="1c805-182">Por motivos de segurança, registre o middleware somente quando o aplicativo é executado no modo de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="1c805-182">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="1c805-183">O *webpack.config.js* do arquivo `output.publicPath` propriedade informa o middleware para observar o `dist` pasta alterações:</span><span class="sxs-lookup"><span data-stu-id="1c805-183">The *webpack.config.js* file's `output.publicPath` property tells the middleware to watch the `dist` folder for changes:</span></span>

[!code-javascript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

<a name="hot-module-replacement"></a>

## <a name="hot-module-replacement"></a><span data-ttu-id="1c805-184">Substituição do módulo ativa</span><span class="sxs-lookup"><span data-stu-id="1c805-184">Hot Module Replacement</span></span>

<span data-ttu-id="1c805-185">Pense do Webpack [módulo substituições](https://webpack.github.io/docs/hot-module-replacement-with-webpack.html) recurso (HMR) como uma evolução do [Webpack desenvolvimento Middleware](#webpack-dev-middleware).</span><span class="sxs-lookup"><span data-stu-id="1c805-185">Think of Webpack's [Hot Module Replacement](https://webpack.github.io/docs/hot-module-replacement-with-webpack.html) (HMR) feature as an evolution of [Webpack Dev Middleware](#webpack-dev-middleware).</span></span> <span data-ttu-id="1c805-186">HMR apresenta os mesmos benefícios, mas ele simplifica ainda mais o fluxo de trabalho de desenvolvimento por atualizar automaticamente o conteúdo da página depois de compilar as alterações.</span><span class="sxs-lookup"><span data-stu-id="1c805-186">HMR introduces all the same benefits, but it further streamlines the development workflow by automatically updating page content after compiling the changes.</span></span> <span data-ttu-id="1c805-187">Não confunda isso com uma atualização do navegador, o que poderia interferir com o estado de memória atual e a sessão de depuração do SPA.</span><span class="sxs-lookup"><span data-stu-id="1c805-187">Don't confuse this with a refresh of the browser, which would interfere with the current in-memory state and debugging session of the SPA.</span></span> <span data-ttu-id="1c805-188">Há um vínculo dinâmico entre o serviço de Middleware de desenvolvimento Webpack e o navegador, o que significa que as alterações são enviados por push para o navegador.</span><span class="sxs-lookup"><span data-stu-id="1c805-188">There is a live link between the Webpack Dev Middleware service and the browser, which means changes are pushed to the browser.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="1c805-189">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="1c805-189">Prerequisites</span></span>

<span data-ttu-id="1c805-190">Instale o seguinte:</span><span class="sxs-lookup"><span data-stu-id="1c805-190">Install the following:</span></span>
* <span data-ttu-id="1c805-191">[middleware hot webpack](https://www.npmjs.com/package/webpack-hot-middleware) pacote npm:</span><span class="sxs-lookup"><span data-stu-id="1c805-191">[webpack-hot-middleware](https://www.npmjs.com/package/webpack-hot-middleware) npm package:</span></span>

    ```console
    npm i -D webpack-hot-middleware
    ```

### <a name="configuration"></a><span data-ttu-id="1c805-192">Configuração</span><span class="sxs-lookup"><span data-stu-id="1c805-192">Configuration</span></span>

<span data-ttu-id="1c805-193">O componente HMR deve ser registrado no pipeline de solicitação HTTP do MVC no `Configure` método:</span><span class="sxs-lookup"><span data-stu-id="1c805-193">The HMR component must be registered into MVC's HTTP request pipeline in the `Configure` method:</span></span>

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

<span data-ttu-id="1c805-194">Como ocorria no [Webpack desenvolvimento Middleware](#webpack-dev-middleware), o `UseWebpackDevMiddleware` método de extensão deve ser chamado antes do `UseStaticFiles` método de extensão.</span><span class="sxs-lookup"><span data-stu-id="1c805-194">As was true with [Webpack Dev Middleware](#webpack-dev-middleware), the `UseWebpackDevMiddleware` extension method must be called before the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="1c805-195">Por motivos de segurança, registre o middleware somente quando o aplicativo é executado no modo de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="1c805-195">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="1c805-196">O *webpack.config.js* arquivo deve definir um `plugins` de matriz, mesmo se ele for deixado em branco:</span><span class="sxs-lookup"><span data-stu-id="1c805-196">The *webpack.config.js* file must define a `plugins` array, even if it's left empty:</span></span>

[!code-javascript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

<span data-ttu-id="1c805-197">Depois de carregar o aplicativo no navegador, a guia Console de ferramentas de desenvolvedor fornece confirmação de ativação de HMR:</span><span class="sxs-lookup"><span data-stu-id="1c805-197">After loading the app in the browser, the developer tools' Console tab provides confirmation of HMR activation:</span></span>

![Hot mensagem conectada de substituição do módulo](spa-services/_static/hmr_connected.png)

<a name="routing-helpers"></a>

## <a name="routing-helpers"></a><span data-ttu-id="1c805-199">Roteamentos auxiliares</span><span class="sxs-lookup"><span data-stu-id="1c805-199">Routing helpers</span></span>

<span data-ttu-id="1c805-200">Na maioria dos SPAs baseado em núcleo do ASP.NET, você vai querer roteamento do lado do cliente além do roteamento do lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="1c805-200">In most ASP.NET Core-based SPAs, you'll want client-side routing in addition to server-side routing.</span></span> <span data-ttu-id="1c805-201">Os sistemas de roteamento SPA e MVC podem trabalhar de forma independente, sem interferência.</span><span class="sxs-lookup"><span data-stu-id="1c805-201">The SPA and MVC routing systems can work independently without interference.</span></span> <span data-ttu-id="1c805-202">No entanto, há um caso de borda apresentando desafios: identificar as respostas HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="1c805-202">There is, however, one edge case posing challenges: identifying 404 HTTP responses.</span></span>

<span data-ttu-id="1c805-203">Considere o cenário no qual uma rota sem extensão de `/some/page` é usado.</span><span class="sxs-lookup"><span data-stu-id="1c805-203">Consider the scenario in which an extensionless route of `/some/page` is used.</span></span> <span data-ttu-id="1c805-204">Suponha que a solicitação não-correspondência de padrão uma rota do lado do servidor, mas o padrão corresponde a uma rota de cliente.</span><span class="sxs-lookup"><span data-stu-id="1c805-204">Assume the request doesn't pattern-match a server-side route, but its pattern does match a client-side route.</span></span> <span data-ttu-id="1c805-205">Agora, considere uma solicitação de entrada para `/images/user-512.png`, que geralmente espera encontrar um arquivo de imagem no servidor.</span><span class="sxs-lookup"><span data-stu-id="1c805-205">Now consider an incoming request for `/images/user-512.png`, which generally expects to find an image file on the server.</span></span> <span data-ttu-id="1c805-206">Se esse caminho de recurso solicitado não corresponde a qualquer rota do lado do servidor ou um arquivo estático, é improvável que o aplicativo cliente deve tratá-la, você geralmente deseja retornar um código de status HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="1c805-206">If that requested resource path doesn't match any server-side route or static file, it's unlikely that the client-side application would handle it — you generally want to return a 404 HTTP status code.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="1c805-207">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="1c805-207">Prerequisites</span></span>

<span data-ttu-id="1c805-208">Instale o seguinte:</span><span class="sxs-lookup"><span data-stu-id="1c805-208">Install the following:</span></span>
* <span data-ttu-id="1c805-209">O pacote npm de roteamento do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="1c805-209">The client-side routing npm package.</span></span> <span data-ttu-id="1c805-210">Usando Angular como um exemplo:</span><span class="sxs-lookup"><span data-stu-id="1c805-210">Using Angular as an example:</span></span>

    ```console
    npm i -S @angular/router
    ```

### <a name="configuration"></a><span data-ttu-id="1c805-211">Configuração</span><span class="sxs-lookup"><span data-stu-id="1c805-211">Configuration</span></span>

<span data-ttu-id="1c805-212">Um método de extensão denominado `MapSpaFallbackRoute` é usado no `Configure` método:</span><span class="sxs-lookup"><span data-stu-id="1c805-212">An extension method named `MapSpaFallbackRoute` is used in the `Configure` method:</span></span>

[!code-csharp[Main](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=mvc-routing-table&highlight=7-9)]

<span data-ttu-id="1c805-213">Dica: As rotas são avaliadas na ordem em que eles foram configurados.</span><span class="sxs-lookup"><span data-stu-id="1c805-213">Tip: Routes are evaluated in the order in which they're configured.</span></span> <span data-ttu-id="1c805-214">Consequentemente, o `default` rota no exemplo de código anterior é usada pela primeira vez para correspondência de padrões.</span><span class="sxs-lookup"><span data-stu-id="1c805-214">Consequently, the `default` route in the preceding code example is used first for pattern matching.</span></span>

<a name="new-project-creation"></a>

## <a name="creating-a-new-project"></a><span data-ttu-id="1c805-215">Criar um novo projeto</span><span class="sxs-lookup"><span data-stu-id="1c805-215">Creating a new project</span></span>

<span data-ttu-id="1c805-216">JavaScriptServices fornece modelos de aplicativo pré-configurado.</span><span class="sxs-lookup"><span data-stu-id="1c805-216">JavaScriptServices provides pre-configured application templates.</span></span> <span data-ttu-id="1c805-217">SpaServices é usada nesses modelos, junto com diferentes estruturas e bibliotecas como Angular, Aurelia, Knockout, reagir e Vue.</span><span class="sxs-lookup"><span data-stu-id="1c805-217">SpaServices is used in these templates, in conjunction with different frameworks and libraries such as Angular, Aurelia, Knockout, React, and Vue.</span></span>

<span data-ttu-id="1c805-218">Esses modelos podem ser instalados por meio da CLI do .NET Core executando o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="1c805-218">These templates can be installed via the .NET Core CLI by running the following command:</span></span>

```console
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

<span data-ttu-id="1c805-219">Uma lista de modelos disponíveis do SPA é exibida:</span><span class="sxs-lookup"><span data-stu-id="1c805-219">A list of available SPA templates is displayed:</span></span>

| <span data-ttu-id="1c805-220">Modelos</span><span class="sxs-lookup"><span data-stu-id="1c805-220">Templates</span></span>                                 | <span data-ttu-id="1c805-221">Nome curto</span><span class="sxs-lookup"><span data-stu-id="1c805-221">Short Name</span></span> | <span data-ttu-id="1c805-222">Linguagem</span><span class="sxs-lookup"><span data-stu-id="1c805-222">Language</span></span> | <span data-ttu-id="1c805-223">Marcas</span><span class="sxs-lookup"><span data-stu-id="1c805-223">Tags</span></span>        |
|:------------------------------------------|:-----------|:---------|:------------|
| <span data-ttu-id="1c805-224">Núcleo do ASP.NET MVC com Angular</span><span class="sxs-lookup"><span data-stu-id="1c805-224">MVC ASP.NET Core with Angular</span></span>             | <span data-ttu-id="1c805-225">angular</span><span class="sxs-lookup"><span data-stu-id="1c805-225">angular</span></span>    | <span data-ttu-id="1c805-226">[C#]</span><span class="sxs-lookup"><span data-stu-id="1c805-226">[C#]</span></span>     | <span data-ttu-id="1c805-227">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="1c805-227">Web/MVC/SPA</span></span> |
| <span data-ttu-id="1c805-228">Núcleo do ASP.NET MVC com Aurelia</span><span class="sxs-lookup"><span data-stu-id="1c805-228">MVC ASP.NET Core with Aurelia</span></span>             | <span data-ttu-id="1c805-229">aurelia</span><span class="sxs-lookup"><span data-stu-id="1c805-229">aurelia</span></span>    | <span data-ttu-id="1c805-230">[C#]</span><span class="sxs-lookup"><span data-stu-id="1c805-230">[C#]</span></span>     | <span data-ttu-id="1c805-231">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="1c805-231">Web/MVC/SPA</span></span> |
| <span data-ttu-id="1c805-232">Núcleo do ASP.NET MVC com Knockout. js</span><span class="sxs-lookup"><span data-stu-id="1c805-232">MVC ASP.NET Core with Knockout.js</span></span>         | <span data-ttu-id="1c805-233">separação</span><span class="sxs-lookup"><span data-stu-id="1c805-233">knockout</span></span>   | <span data-ttu-id="1c805-234">[C#]</span><span class="sxs-lookup"><span data-stu-id="1c805-234">[C#]</span></span>     | <span data-ttu-id="1c805-235">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="1c805-235">Web/MVC/SPA</span></span> |
| <span data-ttu-id="1c805-236">Núcleo do ASP.NET MVC com React.js</span><span class="sxs-lookup"><span data-stu-id="1c805-236">MVC ASP.NET Core with React.js</span></span>            | <span data-ttu-id="1c805-237">react</span><span class="sxs-lookup"><span data-stu-id="1c805-237">react</span></span>      | <span data-ttu-id="1c805-238">[C#]</span><span class="sxs-lookup"><span data-stu-id="1c805-238">[C#]</span></span>     | <span data-ttu-id="1c805-239">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="1c805-239">Web/MVC/SPA</span></span> |
| <span data-ttu-id="1c805-240">Núcleo do ASP.NET MVC com React.js e retorno</span><span class="sxs-lookup"><span data-stu-id="1c805-240">MVC ASP.NET Core with React.js and Redux</span></span>  | <span data-ttu-id="1c805-241">reactredux</span><span class="sxs-lookup"><span data-stu-id="1c805-241">reactredux</span></span> | <span data-ttu-id="1c805-242">[C#]</span><span class="sxs-lookup"><span data-stu-id="1c805-242">[C#]</span></span>     | <span data-ttu-id="1c805-243">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="1c805-243">Web/MVC/SPA</span></span> |
| <span data-ttu-id="1c805-244">Núcleo do ASP.NET MVC com Vue.js</span><span class="sxs-lookup"><span data-stu-id="1c805-244">MVC ASP.NET Core with Vue.js</span></span>              | <span data-ttu-id="1c805-245">VUE</span><span class="sxs-lookup"><span data-stu-id="1c805-245">vue</span></span>        | <span data-ttu-id="1c805-246">[C#]</span><span class="sxs-lookup"><span data-stu-id="1c805-246">[C#]</span></span>     | <span data-ttu-id="1c805-247">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="1c805-247">Web/MVC/SPA</span></span> | 

<span data-ttu-id="1c805-248">Para criar um novo projeto usando um dos modelos de SPA, inclua o **nome curto** do modelo no `dotnet new` comando.</span><span class="sxs-lookup"><span data-stu-id="1c805-248">To create a new project using one of the SPA templates, include the **Short Name** of the template in the `dotnet new` command.</span></span> <span data-ttu-id="1c805-249">O comando a seguir cria um aplicativo Angular com ASP.NET MVC de núcleo configurado para o lado do servidor:</span><span class="sxs-lookup"><span data-stu-id="1c805-249">The following command creates an Angular application with ASP.NET Core MVC configured for the server side:</span></span>

```console
dotnet new angular
```

<a name="runtime-config-mode"></a>

### <a name="set-the-runtime-configuration-mode"></a><span data-ttu-id="1c805-250">Definir o modo de configuração de tempo de execução</span><span class="sxs-lookup"><span data-stu-id="1c805-250">Set the runtime configuration mode</span></span>

<span data-ttu-id="1c805-251">Existem dois modos de configuração de tempo de execução principal:</span><span class="sxs-lookup"><span data-stu-id="1c805-251">Two primary runtime configuration modes exist:</span></span>
* <span data-ttu-id="1c805-252">**Desenvolvimento**:</span><span class="sxs-lookup"><span data-stu-id="1c805-252">**Development**:</span></span>
    * <span data-ttu-id="1c805-253">Inclui mapas de origem para facilitar a depuração.</span><span class="sxs-lookup"><span data-stu-id="1c805-253">Includes source maps to ease debugging.</span></span>
    * <span data-ttu-id="1c805-254">Não Otimize o código do lado do cliente para o desempenho.</span><span class="sxs-lookup"><span data-stu-id="1c805-254">Doesn't optimize the client-side code for performance.</span></span>
* <span data-ttu-id="1c805-255">**Produção**:</span><span class="sxs-lookup"><span data-stu-id="1c805-255">**Production**:</span></span>
    * <span data-ttu-id="1c805-256">Exclui os mapas de origem.</span><span class="sxs-lookup"><span data-stu-id="1c805-256">Excludes source maps.</span></span>
    * <span data-ttu-id="1c805-257">Otimiza o código do lado do cliente por meio de empacotamento e minimização.</span><span class="sxs-lookup"><span data-stu-id="1c805-257">Optimizes the client-side code via bundling & minification.</span></span>

<span data-ttu-id="1c805-258">ASP.NET Core usa uma variável de ambiente denominada `ASPNETCORE_ENVIRONMENT` para armazenar o modo de configuração.</span><span class="sxs-lookup"><span data-stu-id="1c805-258">ASP.NET Core uses an environment variable named `ASPNETCORE_ENVIRONMENT` to store the configuration mode.</span></span> <span data-ttu-id="1c805-259">Consulte  **[Configurando o ambiente](xref:fundamentals/environments#setting-the-environment)**  para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="1c805-259">See **[Setting the environment](xref:fundamentals/environments#setting-the-environment)** for more information.</span></span>

### <a name="running-with-net-core-cli"></a><span data-ttu-id="1c805-260">Executando com .NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="1c805-260">Running with .NET Core CLI</span></span>

<span data-ttu-id="1c805-261">Restaure o NuGet necessário e os pacotes de npm executando o seguinte comando na raiz do projeto:</span><span class="sxs-lookup"><span data-stu-id="1c805-261">Restore the required NuGet and npm packages by running the following command at the project root:</span></span>

```console
dotnet restore && npm i
```

<span data-ttu-id="1c805-262">Compilar e executar o aplicativo:</span><span class="sxs-lookup"><span data-stu-id="1c805-262">Build and run the application:</span></span>

```console
dotnet run
```

<span data-ttu-id="1c805-263">O aplicativo for iniciado no localhost de acordo com o [modo de configuração de tempo de execução](#runtime-config-mode).</span><span class="sxs-lookup"><span data-stu-id="1c805-263">The application starts on localhost according to the [runtime configuration mode](#runtime-config-mode).</span></span> <span data-ttu-id="1c805-264">Navegar para `http://localhost:5000` no navegador exibe a página inicial.</span><span class="sxs-lookup"><span data-stu-id="1c805-264">Navigating to `http://localhost:5000` in the browser displays the landing page.</span></span>

### <a name="running-with-visual-studio-2017"></a><span data-ttu-id="1c805-265">Executando o Visual Studio de 2017</span><span class="sxs-lookup"><span data-stu-id="1c805-265">Running with Visual Studio 2017</span></span>

<span data-ttu-id="1c805-266">Abra o *. csproj* arquivo gerado pelo `dotnet new` comando.</span><span class="sxs-lookup"><span data-stu-id="1c805-266">Open the *.csproj* file generated by the `dotnet new` command.</span></span> <span data-ttu-id="1c805-267">Os pacotes NuGet e npm necessários são restaurados automaticamente após a abertura do projeto.</span><span class="sxs-lookup"><span data-stu-id="1c805-267">The required NuGet and npm packages are restored automatically upon project open.</span></span> <span data-ttu-id="1c805-268">Esse processo de restauração pode demorar alguns minutos e o aplicativo está pronto para ser executado quando ele for concluído.</span><span class="sxs-lookup"><span data-stu-id="1c805-268">This restoration process may take up to a few minutes, and the application is ready to run when it completes.</span></span> <span data-ttu-id="1c805-269">Clique no botão verde de execução ou pressione `Ctrl + F5`, e o navegador abre a página de aterrissagem do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1c805-269">Click the green run button or press `Ctrl + F5`, and the browser opens to the application's landing page.</span></span> <span data-ttu-id="1c805-270">O aplicativo é executado no localhost de acordo com o [modo de configuração de tempo de execução](#runtime-config-mode).</span><span class="sxs-lookup"><span data-stu-id="1c805-270">The application runs on localhost according to the [runtime configuration mode](#runtime-config-mode).</span></span> 

<a name="app-testing"></a>

## <a name="testing-the-app"></a><span data-ttu-id="1c805-271">O aplicativo de teste</span><span class="sxs-lookup"><span data-stu-id="1c805-271">Testing the app</span></span>

<span data-ttu-id="1c805-272">Modelos de SpaServices são pré-configurados para executar testes de cliente usando [Karma](https://karma-runner.github.io/1.0/index.html) e [Jasmine](https://jasmine.github.io/).</span><span class="sxs-lookup"><span data-stu-id="1c805-272">SpaServices templates are pre-configured to run client-side tests using [Karma](https://karma-runner.github.io/1.0/index.html) and [Jasmine](https://jasmine.github.io/).</span></span> <span data-ttu-id="1c805-273">Jasmine é uma unidade popular de estrutura de teste para JavaScript, enquanto Karma é um executor de teste para os testes.</span><span class="sxs-lookup"><span data-stu-id="1c805-273">Jasmine is a popular unit testing framework for JavaScript, whereas Karma is a test runner for those tests.</span></span> <span data-ttu-id="1c805-274">Karma está configurado para funcionar com o [Webpack desenvolvimento Middleware](#webpack-dev-middleware) , de modo que você não precisa parar e o teste seja executado sempre que forem feitas alterações.</span><span class="sxs-lookup"><span data-stu-id="1c805-274">Karma is configured to work with the [Webpack Dev Middleware](#webpack-dev-middleware) such that you don’t have to stop and run the test every time changes are made.</span></span> <span data-ttu-id="1c805-275">Se o código em execução no caso de teste ou o caso de teste, o teste será executado automaticamente.</span><span class="sxs-lookup"><span data-stu-id="1c805-275">Whether it's the code running against the test case or the test case itself, the test runs automatically.</span></span>

<span data-ttu-id="1c805-276">Usando o aplicativo Angular como exemplo, dois casos de teste Jasmine já são fornecidos para o `CounterComponent` no *counter.component.spec.ts* arquivo:</span><span class="sxs-lookup"><span data-stu-id="1c805-276">Using the Angular application as an example, two Jasmine test cases are already provided for the `CounterComponent` in the *counter.component.spec.ts* file:</span></span>

[!code-typescript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

<span data-ttu-id="1c805-277">Abra o prompt de comando na raiz do projeto e execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="1c805-277">Open the command prompt at the project root, and run the following command:</span></span>

```console
npm test
```

<span data-ttu-id="1c805-278">O script inicia o executor de testes Karma, que lê as configurações definidas de *karma.conf.js* arquivo.</span><span class="sxs-lookup"><span data-stu-id="1c805-278">The script launches the Karma test runner, which reads the settings defined in the *karma.conf.js* file.</span></span> <span data-ttu-id="1c805-279">Entre outras configurações, o *karma.conf.js* identifica os arquivos de teste para ser executado por meio de seu `files` matriz:</span><span class="sxs-lookup"><span data-stu-id="1c805-279">Among other settings, the *karma.conf.js* identifies the test files to be executed via its `files` array:</span></span>

[!code-javascript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

<a name="app-publishing"></a>

## <a name="publishing-the-application"></a><span data-ttu-id="1c805-280">Publicando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="1c805-280">Publishing the application</span></span>

<span data-ttu-id="1c805-281">Combinando os ativos do lado do cliente gerados e os artefatos do ASP.NET Core publicados em um pacote pronto para implantar pode ser trabalhoso.</span><span class="sxs-lookup"><span data-stu-id="1c805-281">Combining the generated client-side assets and the published ASP.NET Core artifacts into a ready-to-deploy package can be cumbersome.</span></span> <span data-ttu-id="1c805-282">Felizmente, SpaServices orquestra esse processo de publicação inteira com um destino do MSBuild personalizado chamado `RunWebpack`:</span><span class="sxs-lookup"><span data-stu-id="1c805-282">Thankfully, SpaServices orchestrates that entire publication process with a custom MSBuild target named `RunWebpack`:</span></span>

[!code-xml[Main](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

<span data-ttu-id="1c805-283">O destino do MSBuild tem as seguintes responsabilidades:</span><span class="sxs-lookup"><span data-stu-id="1c805-283">The MSBuild target has the following responsibilities:</span></span>
1. <span data-ttu-id="1c805-284">Restaure os pacotes de npm</span><span class="sxs-lookup"><span data-stu-id="1c805-284">Restore the npm packages</span></span>
1. <span data-ttu-id="1c805-285">Crie uma compilação de nível de produção dos ativos de terceiros, do lado do cliente</span><span class="sxs-lookup"><span data-stu-id="1c805-285">Create a production-grade build of the third-party, client-side assets</span></span>
1. <span data-ttu-id="1c805-286">Crie uma compilação de nível de produção dos ativos do lado do cliente personalizados</span><span class="sxs-lookup"><span data-stu-id="1c805-286">Create a production-grade build of the custom client-side assets</span></span>
1. <span data-ttu-id="1c805-287">Copie os ativos Webpack gerado para a pasta de publicação</span><span class="sxs-lookup"><span data-stu-id="1c805-287">Copy the Webpack-generated assets to the publish folder</span></span>

<span data-ttu-id="1c805-288">O destino do MSBuild é chamado durante a execução:</span><span class="sxs-lookup"><span data-stu-id="1c805-288">The MSBuild target is invoked when running:</span></span>

```console
dotnet publish -c Release
```

## <a name="additional-resources"></a><span data-ttu-id="1c805-289">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="1c805-289">Additional resources</span></span>

* [<span data-ttu-id="1c805-290">Documentos angulares</span><span class="sxs-lookup"><span data-stu-id="1c805-290">Angular Docs</span></span>](https://angular.io/docs)
