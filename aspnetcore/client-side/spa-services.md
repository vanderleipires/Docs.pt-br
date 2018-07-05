---
title: Usar JavaScriptServices para criar aplicativos de única página no ASP.NET Core
author: scottaddie
description: Saiba mais sobre os benefícios de usar JavaScriptServices para criar um aplicativo de página única (SPA) feito pelo ASP.NET Core.
ms.author: scaddie
ms.custom: H1Hack27Feb2017
ms.date: 08/02/2017
uid: client-side/spa-services
ms.openlocfilehash: 6ac922d82e5c93343cd0e9df312719c6df121dcb
ms.sourcegitcommit: 18339e3cb5a891a3ca36d8146fa83cf91c32e707
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37433994"
---
# <a name="use-javascriptservices-to-create-single-page-applications-in-aspnet-core"></a><span data-ttu-id="5bdb8-103">Usar JavaScriptServices para criar aplicativos de única página no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5bdb8-103">Use JavaScriptServices to Create Single Page Applications in ASP.NET Core</span></span>

<span data-ttu-id="5bdb8-104">Por [Scott Addie](https://github.com/scottaddie) e [Fiyaz Hasan](http://fiyazhasan.me/)</span><span class="sxs-lookup"><span data-stu-id="5bdb8-104">By [Scott Addie](https://github.com/scottaddie) and [Fiyaz Hasan](http://fiyazhasan.me/)</span></span>

<span data-ttu-id="5bdb8-105">Um aplicativo de página única (SPA) é um tipo popular de aplicativo web devido à sua experiência de usuário avançada inerente.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-105">A Single Page Application (SPA) is a popular type of web application due to its inherent rich user experience.</span></span> <span data-ttu-id="5bdb8-106">A integração do lado do cliente estruturas de SPA ou bibliotecas, como [Angular](https://angular.io/) ou [reagir](https://facebook.github.io/react/), com estruturas do lado do servidor, como ASP.NET Core pode ser difícil.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-106">Integrating client-side SPA frameworks or libraries, such as [Angular](https://angular.io/) or [React](https://facebook.github.io/react/), with server-side frameworks like ASP.NET Core can be difficult.</span></span> <span data-ttu-id="5bdb8-107">[JavaScriptServices](https://github.com/aspnet/JavaScriptServices) foi desenvolvido para reduzir a fricção no processo de integração.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-107">[JavaScriptServices](https://github.com/aspnet/JavaScriptServices) was developed to reduce friction in the integration process.</span></span> <span data-ttu-id="5bdb8-108">Ele permite que a operação contínua entre o cliente diferentes e pilhas de tecnologia do servidor.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-108">It enables seamless operation between the different client and server technology stacks.</span></span>

<span data-ttu-id="5bdb8-109">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/spa-services/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5bdb8-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/spa-services/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<a name="what-is-js-services"></a>

## <a name="what-is-javascriptservices"></a><span data-ttu-id="5bdb8-110">O que é JavaScriptServices?</span><span class="sxs-lookup"><span data-stu-id="5bdb8-110">What is JavaScriptServices?</span></span>

<span data-ttu-id="5bdb8-111">JavaScriptServices é uma coleção de tecnologias do lado do cliente para o ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-111">JavaScriptServices is a collection of client-side technologies for ASP.NET Core.</span></span> <span data-ttu-id="5bdb8-112">Sua meta é posicionar o ASP.NET Core como plataforma de servidor preferencial dos desenvolvedores para a criação de SPAs.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-112">Its goal is to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span>

<span data-ttu-id="5bdb8-113">JavaScriptServices consiste em três pacotes do NuGet distintos:</span><span class="sxs-lookup"><span data-stu-id="5bdb8-113">JavaScriptServices consists of three distinct NuGet packages:</span></span>
* <span data-ttu-id="5bdb8-114">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span><span class="sxs-lookup"><span data-stu-id="5bdb8-114">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span></span>
* <span data-ttu-id="5bdb8-115">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span><span class="sxs-lookup"><span data-stu-id="5bdb8-115">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span></span>
* <span data-ttu-id="5bdb8-116">[Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)</span><span class="sxs-lookup"><span data-stu-id="5bdb8-116">[Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)</span></span>

<span data-ttu-id="5bdb8-117">Esses pacotes são úteis se você:</span><span class="sxs-lookup"><span data-stu-id="5bdb8-117">These packages are useful if you:</span></span>
* <span data-ttu-id="5bdb8-118">Executar o JavaScript no servidor</span><span class="sxs-lookup"><span data-stu-id="5bdb8-118">Run JavaScript on the server</span></span>
* <span data-ttu-id="5bdb8-119">Usar uma estrutura de SPA ou biblioteca</span><span class="sxs-lookup"><span data-stu-id="5bdb8-119">Use a SPA framework or library</span></span>
* <span data-ttu-id="5bdb8-120">Criar ativos do lado do cliente com o Webpack</span><span class="sxs-lookup"><span data-stu-id="5bdb8-120">Build client-side assets with Webpack</span></span>

<span data-ttu-id="5bdb8-121">O foco deste artigo é colocado sobre como usar o pacote SpaServices.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-121">Much of the focus in this article is placed on using the SpaServices package.</span></span>

<a name="what-is-spa-services"></a>

## <a name="what-is-spaservices"></a><span data-ttu-id="5bdb8-122">O que é SpaServices?</span><span class="sxs-lookup"><span data-stu-id="5bdb8-122">What is SpaServices?</span></span>

<span data-ttu-id="5bdb8-123">SpaServices foi criado para posicionar o ASP.NET Core como plataforma de servidor preferencial dos desenvolvedores para a criação de SPAs.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-123">SpaServices was created to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span> <span data-ttu-id="5bdb8-124">SpaServices não é necessário para desenvolver os SPAs com o ASP.NET Core, e ele não bloqueie você em uma estrutura de cliente específico.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-124">SpaServices isn't required to develop SPAs with ASP.NET Core, and it doesn't lock you into a particular client framework.</span></span>

<span data-ttu-id="5bdb8-125">SpaServices fornece infraestrutura úteis, como:</span><span class="sxs-lookup"><span data-stu-id="5bdb8-125">SpaServices provides useful infrastructure such as:</span></span>
* [<span data-ttu-id="5bdb8-126">Pré-processamento do lado do servidor</span><span class="sxs-lookup"><span data-stu-id="5bdb8-126">Server-side prerendering</span></span>](#server-prerendering)
* [<span data-ttu-id="5bdb8-127">Middleware de desenvolvimento webpack</span><span class="sxs-lookup"><span data-stu-id="5bdb8-127">Webpack Dev Middleware</span></span>](#webpack-dev-middleware)
* [<span data-ttu-id="5bdb8-128">Substituição do módulo quente</span><span class="sxs-lookup"><span data-stu-id="5bdb8-128">Hot Module Replacement</span></span>](#hot-module-replacement)
* [<span data-ttu-id="5bdb8-129">Auxiliares de roteamentos</span><span class="sxs-lookup"><span data-stu-id="5bdb8-129">Routing helpers</span></span>](#routing-helpers)

<span data-ttu-id="5bdb8-130">Coletivamente, esses componentes de infraestrutura aprimoram o fluxo de trabalho de desenvolvimento e a experiência de tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-130">Collectively, these infrastructure components enhance both the development workflow and the runtime experience.</span></span> <span data-ttu-id="5bdb8-131">Os componentes podem ser adotados individualmente.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-131">The components can be adopted individually.</span></span>

<a name="spa-services-prereqs"></a>

## <a name="prerequisites-for-using-spaservices"></a><span data-ttu-id="5bdb8-132">Pré-requisitos para usar SpaServices</span><span class="sxs-lookup"><span data-stu-id="5bdb8-132">Prerequisites for using SpaServices</span></span>

<span data-ttu-id="5bdb8-133">Para trabalhar com SpaServices, instale o seguinte:</span><span class="sxs-lookup"><span data-stu-id="5bdb8-133">To work with SpaServices, install the following:</span></span>
* <span data-ttu-id="5bdb8-134">[Node. js](https://nodejs.org/) (versão 6 ou posterior) com npm</span><span class="sxs-lookup"><span data-stu-id="5bdb8-134">[Node.js](https://nodejs.org/) (version 6 or later) with npm</span></span>
  * <span data-ttu-id="5bdb8-135">Para verificar se esses componentes estão instalados e podem ser encontrados, execute o seguinte na linha de comando:</span><span class="sxs-lookup"><span data-stu-id="5bdb8-135">To verify these components are installed and can be found, run the following from the command line:</span></span>

    ```console
    node -v && npm -v
    ```

<span data-ttu-id="5bdb8-136">Observação: Se você estiver implantando em um site do Azure, você não precisará fazer nada aqui &mdash; Node. js está instalado e disponível em ambientes de servidor.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-136">Note: If you're deploying to an Azure web site, you don't need to do anything here &mdash; Node.js is installed and available in the server environments.</span></span>

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]

  * <span data-ttu-id="5bdb8-137">Se você estiver no Windows usando o Visual Studio 2017, o SDK está instalado, selecionando o **desenvolvimento de plataforma cruzada do .NET Core** carga de trabalho.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-137">If you're on Windows using Visual Studio 2017, the SDK is installed by selecting the **.NET Core cross-platform development** workload.</span></span>

* <span data-ttu-id="5bdb8-138">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) pacote do NuGet</span><span class="sxs-lookup"><span data-stu-id="5bdb8-138">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet package</span></span>

<a name="server-prerendering"></a>

## <a name="server-side-prerendering"></a><span data-ttu-id="5bdb8-139">Pré-processamento do lado do servidor</span><span class="sxs-lookup"><span data-stu-id="5bdb8-139">Server-side prerendering</span></span>

<span data-ttu-id="5bdb8-140">Um aplicativo universal de (também conhecido como isomórficos) é um aplicativo de JavaScript pode ser executada tanto no servidor e cliente.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-140">A universal (also known as isomorphic) application is a JavaScript application capable of running both on the server and the client.</span></span> <span data-ttu-id="5bdb8-141">Angular, React e outras estruturas populares fornecem uma plataforma universal para esse estilo de desenvolvimento do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-141">Angular, React, and other popular frameworks provide a universal platform for this application development style.</span></span> <span data-ttu-id="5bdb8-142">A ideia é renderizado primeiro os componentes do framework no servidor por meio do Node. js e delegar ainda mais a execução para o cliente.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-142">The idea is to first render the framework components on the server via Node.js, and then delegate further execution to the client.</span></span>

<span data-ttu-id="5bdb8-143">ASP.NET Core [auxiliares de marca](xref:mvc/views/tag-helpers/intro) fornecidos pelo SpaServices simplificar a implementação de pré-processamento do lado do servidor, chamando as funções de JavaScript no servidor.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-143">ASP.NET Core [Tag Helpers](xref:mvc/views/tag-helpers/intro) provided by SpaServices simplify the implementation of server-side prerendering by invoking the JavaScript functions on the server.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="5bdb8-144">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="5bdb8-144">Prerequisites</span></span>

<span data-ttu-id="5bdb8-145">Instale o seguinte:</span><span class="sxs-lookup"><span data-stu-id="5bdb8-145">Install the following:</span></span>
* <span data-ttu-id="5bdb8-146">[ASPNET pré-processamento](https://www.npmjs.com/package/aspnet-prerendering) pacote npm:</span><span class="sxs-lookup"><span data-stu-id="5bdb8-146">[aspnet-prerendering](https://www.npmjs.com/package/aspnet-prerendering) npm package:</span></span>

    ```console
    npm i -S aspnet-prerendering
    ```

### <a name="configuration"></a><span data-ttu-id="5bdb8-147">Configuração</span><span class="sxs-lookup"><span data-stu-id="5bdb8-147">Configuration</span></span>

<span data-ttu-id="5bdb8-148">Os auxiliares de marca são feitos podem ser descobertos por meio do registro do namespace do projeto *viewimports. cshtml* arquivo:</span><span class="sxs-lookup"><span data-stu-id="5bdb8-148">The Tag Helpers are made discoverable via namespace registration in the project's *_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

<span data-ttu-id="5bdb8-149">Esses auxiliares de marcação abstraem as complexidades de se comunicar diretamente com as APIs de baixo nível, utilizando uma sintaxe semelhante ao HTML dentro a exibição do Razor:</span><span class="sxs-lookup"><span data-stu-id="5bdb8-149">These Tag Helpers abstract away the intricacies of communicating directly with low-level APIs by leveraging an HTML-like syntax inside the Razor view:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="the-asp-prerender-module-tag-helper"></a><span data-ttu-id="5bdb8-150">O `asp-prerender-module` auxiliar de marca</span><span class="sxs-lookup"><span data-stu-id="5bdb8-150">The `asp-prerender-module` Tag Helper</span></span>

<span data-ttu-id="5bdb8-151">O `asp-prerender-module` auxiliar de marca, usado no exemplo de código anterior executa *ClientApp/dist/main-server.js* no servidor por meio do Node. js.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-151">The `asp-prerender-module` Tag Helper, used in the preceding code example, executes *ClientApp/dist/main-server.js* on the server via Node.js.</span></span> <span data-ttu-id="5bdb8-152">Para clareza, *main-Server. js* arquivo é um artefato da tarefa em que a transcompilação TypeScript e JavaScript a [Webpack](http://webpack.github.io/) processo de compilação.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-152">For clarity's sake, *main-server.js* file is an artifact of the TypeScript-to-JavaScript transpilation task in the [Webpack](http://webpack.github.io/) build process.</span></span> <span data-ttu-id="5bdb8-153">Webpack define um alias de ponto de entrada de `main-server`; e, passagem de grafo de dependência para este alias começa em de *ClientApp/inicialização-server.ts* arquivo:</span><span class="sxs-lookup"><span data-stu-id="5bdb8-153">Webpack defines an entry point alias of `main-server`; and, traversal of the dependency graph for this alias begins at the *ClientApp/boot-server.ts* file:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

<span data-ttu-id="5bdb8-154">No exemplo a seguir Angular, o *ClientApp/inicialização-server.ts* arquivo utiliza a `createServerRenderer` função e `RenderResult` tipo do `aspnet-prerendering` pacote npm para configurar a renderização do servidor por meio do Node. js.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-154">In the following Angular example, the *ClientApp/boot-server.ts* file utilizes the `createServerRenderer` function and `RenderResult` type of the `aspnet-prerendering` npm package to configure server rendering via Node.js.</span></span> <span data-ttu-id="5bdb8-155">A marcação HTML destinada a renderização do lado do servidor é passada para uma chamada de função de resolução, que é encapsulada em um JavaScript fortemente tipada `Promise` objeto.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-155">The HTML markup destined for server-side rendering is passed to a resolve function call, which is wrapped in a strongly-typed JavaScript `Promise` object.</span></span> <span data-ttu-id="5bdb8-156">O `Promise` significância do objeto é que ele fornece assincronamente a marcação HTML para a página para injeção no elemento de espaço reservado do DOM.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-156">The `Promise` object's significance is that it asynchronously supplies the HTML markup to the page for injection in the DOM's placeholder element.</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="the-asp-prerender-data-tag-helper"></a><span data-ttu-id="5bdb8-157">O `asp-prerender-data` auxiliar de marca</span><span class="sxs-lookup"><span data-stu-id="5bdb8-157">The `asp-prerender-data` Tag Helper</span></span>

<span data-ttu-id="5bdb8-158">Quando combinado com o `asp-prerender-module` auxiliar de marca, o `asp-prerender-data` auxiliar de marca pode ser usado para passar informações contextuais da exibição do Razor para o JavaScript do lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-158">When coupled with the `asp-prerender-module` Tag Helper, the `asp-prerender-data` Tag Helper can be used to pass contextual information from the Razor view to the server-side JavaScript.</span></span> <span data-ttu-id="5bdb8-159">Por exemplo, a marcação a seguir passa os dados do usuário para o `main-server` módulo:</span><span class="sxs-lookup"><span data-stu-id="5bdb8-159">For example, the following markup passes user data to the `main-server` module:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

<span data-ttu-id="5bdb8-160">Recebido `UserName` argumento for serializado usando o serializador JSON interno e é armazenado no `params.data` objeto.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-160">The received `UserName` argument is serialized using the built-in JSON serializer and is stored in the `params.data` object.</span></span> <span data-ttu-id="5bdb8-161">No exemplo a seguir Angular, os dados são usados para construir uma saudação personalizada dentro de um `h1` elemento:</span><span class="sxs-lookup"><span data-stu-id="5bdb8-161">In the following Angular example, the data is used to construct a personalized greeting within an `h1` element:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

<span data-ttu-id="5bdb8-162">Observação: Os nomes de propriedade passados na auxiliares de marca são representados com **PascalCase** notação.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-162">Note: Property names passed in Tag Helpers are represented with **PascalCase** notation.</span></span> <span data-ttu-id="5bdb8-163">Compare isso com JavaScript, onde os mesmos nomes de propriedade são representados com **camelCase**.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-163">Contrast that to JavaScript, where the same property names are represented with **camelCase**.</span></span> <span data-ttu-id="5bdb8-164">A configuração de serialização JSON padrão é responsável por essa diferença.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-164">The default JSON serialization configuration is responsible for this difference.</span></span>

<span data-ttu-id="5bdb8-165">Para expandir o exemplo de código anterior, dados podem ser passados do servidor para o modo de exibição por hydrating a `globals` propriedade fornecida para o `resolve` função:</span><span class="sxs-lookup"><span data-stu-id="5bdb8-165">To expand upon the preceding code example, data can be passed from the server to the view by hydrating the `globals` property provided to the `resolve` function:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

<span data-ttu-id="5bdb8-166">O `postList` matriz definida dentro de `globals` objeto está anexado para o navegador global `window` objeto.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-166">The `postList` array defined inside the `globals` object is attached to the browser's global `window` object.</span></span> <span data-ttu-id="5bdb8-167">Essa elevação de variáveis em escopo global elimina a duplicação de esforço, particularmente, pois pertence ao carregar os mesmos dados, uma vez no servidor e novamente no cliente.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-167">This variable hoisting to global scope eliminates duplication of effort, particularly as it pertains to loading the same data once on the server and again on the client.</span></span>

![variável global de postList anexada ao objeto de janela](spa-services/_static/global_variable.png)

<a name="webpack-dev-middleware"></a>

## <a name="webpack-dev-middleware"></a><span data-ttu-id="5bdb8-169">Middleware de desenvolvimento webpack</span><span class="sxs-lookup"><span data-stu-id="5bdb8-169">Webpack Dev Middleware</span></span>

<span data-ttu-id="5bdb8-170">[Middleware de desenvolvimento webpack](https://webpack.github.io/docs/webpack-dev-middleware.html) introduz um fluxo de trabalho de desenvolvimento simplificado no qual o Webpack cria recursos sob demanda.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-170">[Webpack Dev Middleware](https://webpack.github.io/docs/webpack-dev-middleware.html) introduces a streamlined development workflow whereby Webpack builds resources on demand.</span></span> <span data-ttu-id="5bdb8-171">O middleware automaticamente compila e atende recursos do lado do cliente quando uma página é recarregada no navegador.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-171">The middleware automatically compiles and serves client-side resources when a page is reloaded in the browser.</span></span> <span data-ttu-id="5bdb8-172">A abordagem alternativa é invocar manualmente Webpack por meio do script de compilação do projeto npm quando uma dependência de terceiros ou o código personalizado é alterado.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-172">The alternate approach is to manually invoke Webpack via the project's npm build script when a third-party dependency or the custom code changes.</span></span> <span data-ttu-id="5bdb8-173">Script de construção de um npm *Package. JSON* arquivo é mostrado no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="5bdb8-173">An npm build script in the *package.json* file is shown in the following example:</span></span>

[!code-json[](../client-side/spa-services/sample/SpaServicesSampleApp/package.json?range=5)]

### <a name="prerequisites"></a><span data-ttu-id="5bdb8-174">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="5bdb8-174">Prerequisites</span></span>

<span data-ttu-id="5bdb8-175">Instale o seguinte:</span><span class="sxs-lookup"><span data-stu-id="5bdb8-175">Install the following:</span></span>
* <span data-ttu-id="5bdb8-176">[ASPNET webpack](https://www.npmjs.com/package/aspnet-webpack) pacote npm:</span><span class="sxs-lookup"><span data-stu-id="5bdb8-176">[aspnet-webpack](https://www.npmjs.com/package/aspnet-webpack) npm package:</span></span>

    ```console
    npm i -D aspnet-webpack
    ```

### <a name="configuration"></a><span data-ttu-id="5bdb8-177">Configuração</span><span class="sxs-lookup"><span data-stu-id="5bdb8-177">Configuration</span></span>

<span data-ttu-id="5bdb8-178">Webpack Dev Middleware é registrado no pipeline de solicitação HTTP por meio de código a seguir na *Startup.cs* do arquivo `Configure` método:</span><span class="sxs-lookup"><span data-stu-id="5bdb8-178">Webpack Dev Middleware is registered into the HTTP request pipeline via the following code in the *Startup.cs* file's `Configure` method:</span></span>

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=webpack-middleware-registration&highlight=4)]

<span data-ttu-id="5bdb8-179">O `UseWebpackDevMiddleware` método de extensão deve ser chamado antes [Registrando a hospedagem de arquivos estáticos](xref:fundamentals/static-files) por meio de `UseStaticFiles` método de extensão.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-179">The `UseWebpackDevMiddleware` extension method must be called before [registering static file hosting](xref:fundamentals/static-files) via the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="5bdb8-180">Por motivos de segurança, registre o middleware somente quando o aplicativo é executado no modo de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-180">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="5bdb8-181">O *webpack.config.js* do arquivo `output.publicPath` propriedade informa o middleware para observar o `dist` pasta para que as alterações:</span><span class="sxs-lookup"><span data-stu-id="5bdb8-181">The *webpack.config.js* file's `output.publicPath` property tells the middleware to watch the `dist` folder for changes:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

<a name="hot-module-replacement"></a>

## <a name="hot-module-replacement"></a><span data-ttu-id="5bdb8-182">Substituição do módulo quente</span><span class="sxs-lookup"><span data-stu-id="5bdb8-182">Hot Module Replacement</span></span>

<span data-ttu-id="5bdb8-183">Pense do Webpack [módulo de substituição a quente](https://webpack.js.org/concepts/hot-module-replacement/) recurso (HMR) como uma evolução do [Webpack Dev Middleware](#webpack-dev-middleware).</span><span class="sxs-lookup"><span data-stu-id="5bdb8-183">Think of Webpack's [Hot Module Replacement](https://webpack.js.org/concepts/hot-module-replacement/) (HMR) feature as an evolution of [Webpack Dev Middleware](#webpack-dev-middleware).</span></span> <span data-ttu-id="5bdb8-184">HMR apresenta todos os mesmos benefícios, mas ela simplifica ainda mais o fluxo de trabalho de desenvolvimento, atualizando automaticamente o conteúdo da página depois de compilar as alterações.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-184">HMR introduces all the same benefits, but it further streamlines the development workflow by automatically updating page content after compiling the changes.</span></span> <span data-ttu-id="5bdb8-185">Não confunda isso com uma atualização do navegador, que poderia interferir com o estado atual de na memória e a sessão de depuração do SPA.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-185">Don't confuse this with a refresh of the browser, which would interfere with the current in-memory state and debugging session of the SPA.</span></span> <span data-ttu-id="5bdb8-186">Há um link em tempo real entre o serviço de Middleware de desenvolvimento Webpack e o navegador, o que significa que alterações são enviadas para o navegador.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-186">There's a live link between the Webpack Dev Middleware service and the browser, which means changes are pushed to the browser.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="5bdb8-187">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="5bdb8-187">Prerequisites</span></span>

<span data-ttu-id="5bdb8-188">Instale o seguinte:</span><span class="sxs-lookup"><span data-stu-id="5bdb8-188">Install the following:</span></span>
* <span data-ttu-id="5bdb8-189">[middleware hot webpack](https://www.npmjs.com/package/webpack-hot-middleware) pacote npm:</span><span class="sxs-lookup"><span data-stu-id="5bdb8-189">[webpack-hot-middleware](https://www.npmjs.com/package/webpack-hot-middleware) npm package:</span></span>

    ```console
    npm i -D webpack-hot-middleware
    ```

### <a name="configuration"></a><span data-ttu-id="5bdb8-190">Configuração</span><span class="sxs-lookup"><span data-stu-id="5bdb8-190">Configuration</span></span>

<span data-ttu-id="5bdb8-191">O componente HMR deve ser registrado no pipeline de solicitação HTTP do MVC no `Configure` método:</span><span class="sxs-lookup"><span data-stu-id="5bdb8-191">The HMR component must be registered into MVC's HTTP request pipeline in the `Configure` method:</span></span>

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

<span data-ttu-id="5bdb8-192">Como ocorria com [Webpack Dev Middleware](#webpack-dev-middleware), o `UseWebpackDevMiddleware` método de extensão deve ser chamado antes do `UseStaticFiles` método de extensão.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-192">As was true with [Webpack Dev Middleware](#webpack-dev-middleware), the `UseWebpackDevMiddleware` extension method must be called before the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="5bdb8-193">Por motivos de segurança, registre o middleware somente quando o aplicativo é executado no modo de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-193">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="5bdb8-194">O *webpack.config.js* arquivo deve definir um `plugins` de matriz, mesmo se ela for deixada em branco:</span><span class="sxs-lookup"><span data-stu-id="5bdb8-194">The *webpack.config.js* file must define a `plugins` array, even if it's left empty:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

<span data-ttu-id="5bdb8-195">Depois de carregar o aplicativo no navegador, a guia Console de ferramentas de desenvolvedor fornece confirmação de ativação de HMR:</span><span class="sxs-lookup"><span data-stu-id="5bdb8-195">After loading the app in the browser, the developer tools' Console tab provides confirmation of HMR activation:</span></span>

![Hot mensagem conectada de substituição do módulo](spa-services/_static/hmr_connected.png)

<a name="routing-helpers"></a>

## <a name="routing-helpers"></a><span data-ttu-id="5bdb8-197">Auxiliares de roteamentos</span><span class="sxs-lookup"><span data-stu-id="5bdb8-197">Routing helpers</span></span>

<span data-ttu-id="5bdb8-198">A maioria dos SPAs com base no ASP.NET Core, você desejará roteamento do lado do cliente além do roteamento do lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-198">In most ASP.NET Core-based SPAs, you'll want client-side routing in addition to server-side routing.</span></span> <span data-ttu-id="5bdb8-199">Os sistemas de roteamento do SPA e o MVC podem trabalhar de forma independente, sem interferência.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-199">The SPA and MVC routing systems can work independently without interference.</span></span> <span data-ttu-id="5bdb8-200">Há, no entanto, os desafios de uma borda caso apresentando: identificando as respostas HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-200">There's, however, one edge case posing challenges: identifying 404 HTTP responses.</span></span>

<span data-ttu-id="5bdb8-201">Considere o cenário em que uma rota sem extensão de `/some/page` é usado.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-201">Consider the scenario in which an extensionless route of `/some/page` is used.</span></span> <span data-ttu-id="5bdb8-202">Suponha que a solicitação não-correspondência de padrão uma rota do lado do servidor, mas seu padrão corresponde a uma rota do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-202">Assume the request doesn't pattern-match a server-side route, but its pattern does match a client-side route.</span></span> <span data-ttu-id="5bdb8-203">Agora, considere uma solicitação de entrada para `/images/user-512.png`, que geralmente espera encontrar um arquivo de imagem no servidor.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-203">Now consider an incoming request for `/images/user-512.png`, which generally expects to find an image file on the server.</span></span> <span data-ttu-id="5bdb8-204">Se esse caminho de recurso solicitado não corresponder a qualquer rota do lado do servidor ou um arquivo estático, é improvável que o aplicativo do lado do cliente seria lidará com isso, você geralmente deseja retornar um código de status HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-204">If that requested resource path doesn't match any server-side route or static file, it's unlikely that the client-side application would handle it — you generally want to return a 404 HTTP status code.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="5bdb8-205">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="5bdb8-205">Prerequisites</span></span>

<span data-ttu-id="5bdb8-206">Instale o seguinte:</span><span class="sxs-lookup"><span data-stu-id="5bdb8-206">Install the following:</span></span>
* <span data-ttu-id="5bdb8-207">O pacote npm de roteamento do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-207">The client-side routing npm package.</span></span> <span data-ttu-id="5bdb8-208">Usando Angular como um exemplo:</span><span class="sxs-lookup"><span data-stu-id="5bdb8-208">Using Angular as an example:</span></span>

    ```console
    npm i -S @angular/router
    ```

### <a name="configuration"></a><span data-ttu-id="5bdb8-209">Configuração</span><span class="sxs-lookup"><span data-stu-id="5bdb8-209">Configuration</span></span>

<span data-ttu-id="5bdb8-210">Um método de extensão denominado `MapSpaFallbackRoute` é usado no `Configure` método:</span><span class="sxs-lookup"><span data-stu-id="5bdb8-210">An extension method named `MapSpaFallbackRoute` is used in the `Configure` method:</span></span>

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=mvc-routing-table&highlight=7-9)]

<span data-ttu-id="5bdb8-211">Dica: As rotas são avaliadas na ordem em que eles foram configurados.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-211">Tip: Routes are evaluated in the order in which they're configured.</span></span> <span data-ttu-id="5bdb8-212">Consequentemente, o `default` rota no exemplo de código anterior é usada primeiro para correspondência de padrões.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-212">Consequently, the `default` route in the preceding code example is used first for pattern matching.</span></span>

<a name="new-project-creation"></a>

## <a name="creating-a-new-project"></a><span data-ttu-id="5bdb8-213">Criar um novo projeto</span><span class="sxs-lookup"><span data-stu-id="5bdb8-213">Creating a new project</span></span>

<span data-ttu-id="5bdb8-214">JavaScriptServices fornece modelos de aplicativos pré-configurados.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-214">JavaScriptServices provides pre-configured application templates.</span></span> <span data-ttu-id="5bdb8-215">SpaServices é usado nesses modelos, em conjunto com diferentes estruturas e bibliotecas, como Angular, React e Redux.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-215">SpaServices is used in these templates, in conjunction with different frameworks and libraries such as Angular, React, and Redux.</span></span>

<span data-ttu-id="5bdb8-216">Esses modelos podem ser instalados por meio da CLI do .NET Core, executando o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="5bdb8-216">These templates can be installed via the .NET Core CLI by running the following command:</span></span>

```console
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

<span data-ttu-id="5bdb8-217">Uma lista de modelos SPA disponíveis é exibida:</span><span class="sxs-lookup"><span data-stu-id="5bdb8-217">A list of available SPA templates is displayed:</span></span>

| <span data-ttu-id="5bdb8-218">Modelos</span><span class="sxs-lookup"><span data-stu-id="5bdb8-218">Templates</span></span>                                 | <span data-ttu-id="5bdb8-219">Nome curto</span><span class="sxs-lookup"><span data-stu-id="5bdb8-219">Short Name</span></span> | <span data-ttu-id="5bdb8-220">Idioma</span><span class="sxs-lookup"><span data-stu-id="5bdb8-220">Language</span></span> | <span data-ttu-id="5bdb8-221">Marcas</span><span class="sxs-lookup"><span data-stu-id="5bdb8-221">Tags</span></span>        |
|:------------------------------------------|:-----------|:---------|:------------|
| <span data-ttu-id="5bdb8-222">MVC ASP.NET Core com Angular</span><span class="sxs-lookup"><span data-stu-id="5bdb8-222">MVC ASP.NET Core with Angular</span></span>             | <span data-ttu-id="5bdb8-223">angular</span><span class="sxs-lookup"><span data-stu-id="5bdb8-223">angular</span></span>    | <span data-ttu-id="5bdb8-224">[C#]</span><span class="sxs-lookup"><span data-stu-id="5bdb8-224">[C#]</span></span>     | <span data-ttu-id="5bdb8-225">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="5bdb8-225">Web/MVC/SPA</span></span> |
| <span data-ttu-id="5bdb8-226">MVC ASP.NET Core com React. js</span><span class="sxs-lookup"><span data-stu-id="5bdb8-226">MVC ASP.NET Core with React.js</span></span>            | <span data-ttu-id="5bdb8-227">react</span><span class="sxs-lookup"><span data-stu-id="5bdb8-227">react</span></span>      | <span data-ttu-id="5bdb8-228">[C#]</span><span class="sxs-lookup"><span data-stu-id="5bdb8-228">[C#]</span></span>     | <span data-ttu-id="5bdb8-229">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="5bdb8-229">Web/MVC/SPA</span></span> |
| <span data-ttu-id="5bdb8-230">MVC ASP.NET Core com React. js e Redux</span><span class="sxs-lookup"><span data-stu-id="5bdb8-230">MVC ASP.NET Core with React.js and Redux</span></span>  | <span data-ttu-id="5bdb8-231">reactredux</span><span class="sxs-lookup"><span data-stu-id="5bdb8-231">reactredux</span></span> | <span data-ttu-id="5bdb8-232">[C#]</span><span class="sxs-lookup"><span data-stu-id="5bdb8-232">[C#]</span></span>     | <span data-ttu-id="5bdb8-233">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="5bdb8-233">Web/MVC/SPA</span></span> |

<span data-ttu-id="5bdb8-234">Para criar um novo projeto usando um dos modelos do SPA, inclua o **nome curto** do modelo na [dotnet novo](/dotnet/core/tools/dotnet-new) comando.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-234">To create a new project using one of the SPA templates, include the **Short Name** of the template in the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="5bdb8-235">O comando a seguir cria um aplicativo Angular com ASP.NET Core MVC configurado para o lado do servidor:</span><span class="sxs-lookup"><span data-stu-id="5bdb8-235">The following command creates an Angular application with ASP.NET Core MVC configured for the server side:</span></span>

```console
dotnet new angular
```

<a name="runtime-config-mode"></a>

### <a name="set-the-runtime-configuration-mode"></a><span data-ttu-id="5bdb8-236">Definir o modo de configuração de tempo de execução</span><span class="sxs-lookup"><span data-stu-id="5bdb8-236">Set the runtime configuration mode</span></span>

<span data-ttu-id="5bdb8-237">Existem dois modos de configuração de tempo de execução principal:</span><span class="sxs-lookup"><span data-stu-id="5bdb8-237">Two primary runtime configuration modes exist:</span></span>
* <span data-ttu-id="5bdb8-238">**Desenvolvimento**:</span><span class="sxs-lookup"><span data-stu-id="5bdb8-238">**Development**:</span></span>
    * <span data-ttu-id="5bdb8-239">Inclui mapas de código-fonte para facilitar a depuração.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-239">Includes source maps to ease debugging.</span></span>
    * <span data-ttu-id="5bdb8-240">Não Otimize o código do lado do cliente para o desempenho.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-240">Doesn't optimize the client-side code for performance.</span></span>
* <span data-ttu-id="5bdb8-241">**Produção**:</span><span class="sxs-lookup"><span data-stu-id="5bdb8-241">**Production**:</span></span>
    * <span data-ttu-id="5bdb8-242">Exclui os mapas de origem.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-242">Excludes source maps.</span></span>
    * <span data-ttu-id="5bdb8-243">Otimiza o código do lado do cliente por meio do agrupamento e minificação.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-243">Optimizes the client-side code via bundling & minification.</span></span>

<span data-ttu-id="5bdb8-244">O ASP.NET Core usa uma variável de ambiente denominada `ASPNETCORE_ENVIRONMENT` para armazenar o modo de configuração.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-244">ASP.NET Core uses an environment variable named `ASPNETCORE_ENVIRONMENT` to store the configuration mode.</span></span> <span data-ttu-id="5bdb8-245">Ver **[defina o ambiente](xref:fundamentals/environments#set-the-environment)** para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-245">See **[Set the environment](xref:fundamentals/environments#set-the-environment)** for more information.</span></span>

### <a name="running-with-net-core-cli"></a><span data-ttu-id="5bdb8-246">Executando com a CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="5bdb8-246">Running with .NET Core CLI</span></span>

<span data-ttu-id="5bdb8-247">Restaure os pacotes de npm e NuGet necessários, executando o seguinte comando na raiz do projeto:</span><span class="sxs-lookup"><span data-stu-id="5bdb8-247">Restore the required NuGet and npm packages by running the following command at the project root:</span></span>

```console
dotnet restore && npm i
```

<span data-ttu-id="5bdb8-248">Compilar e executar o aplicativo:</span><span class="sxs-lookup"><span data-stu-id="5bdb8-248">Build and run the application:</span></span>

```console
dotnet run
```

<span data-ttu-id="5bdb8-249">O aplicativo é iniciado no localhost de acordo com o [modo de configuração de tempo de execução](#runtime-config-mode).</span><span class="sxs-lookup"><span data-stu-id="5bdb8-249">The application starts on localhost according to the [runtime configuration mode](#runtime-config-mode).</span></span> <span data-ttu-id="5bdb8-250">Navegando até `http://localhost:5000` no navegador exibe a página de aterrissagem.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-250">Navigating to `http://localhost:5000` in the browser displays the landing page.</span></span>

### <a name="running-with-visual-studio-2017"></a><span data-ttu-id="5bdb8-251">Em execução com o Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="5bdb8-251">Running with Visual Studio 2017</span></span>

<span data-ttu-id="5bdb8-252">Abra o *. csproj* arquivo gerado pelo [dotnet novo](/dotnet/core/tools/dotnet-new) comando.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-252">Open the *.csproj* file generated by the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="5bdb8-253">Os pacotes NuGet e npm necessários são restaurados automaticamente ao projeto aberto.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-253">The required NuGet and npm packages are restored automatically upon project open.</span></span> <span data-ttu-id="5bdb8-254">Esse processo de restauração pode demorar alguns minutos e o aplicativo está pronto para ser executado quando ele for concluído.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-254">This restoration process may take up to a few minutes, and the application is ready to run when it completes.</span></span> <span data-ttu-id="5bdb8-255">Clique no botão de execução verde ou pressione `Ctrl + F5`, e o navegador abre a página de aterrissagem do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-255">Click the green run button or press `Ctrl + F5`, and the browser opens to the application's landing page.</span></span> <span data-ttu-id="5bdb8-256">O aplicativo é executado no localhost de acordo com o [modo de configuração de tempo de execução](#runtime-config-mode).</span><span class="sxs-lookup"><span data-stu-id="5bdb8-256">The application runs on localhost according to the [runtime configuration mode](#runtime-config-mode).</span></span> 

<a name="app-testing"></a>

## <a name="testing-the-app"></a><span data-ttu-id="5bdb8-257">Testar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="5bdb8-257">Testing the app</span></span>

<span data-ttu-id="5bdb8-258">Modelos SpaServices são pré-configuradas para executar testes do lado do cliente usando [Karma](https://karma-runner.github.io/1.0/index.html) e [Jasmine](https://jasmine.github.io/).</span><span class="sxs-lookup"><span data-stu-id="5bdb8-258">SpaServices templates are pre-configured to run client-side tests using [Karma](https://karma-runner.github.io/1.0/index.html) and [Jasmine](https://jasmine.github.io/).</span></span> <span data-ttu-id="5bdb8-259">Jasmine é uma estrutura de teste para JavaScript, de unidade popular enquanto Karma é um executor de teste para que esses testes.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-259">Jasmine is a popular unit testing framework for JavaScript, whereas Karma is a test runner for those tests.</span></span> <span data-ttu-id="5bdb8-260">Karma está configurado para funcionar com o [Webpack Dev Middleware](#webpack-dev-middleware) , de modo que o desenvolvedor não é necessário parar e executar o teste sempre que forem feitas alterações.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-260">Karma is configured to work with the [Webpack Dev Middleware](#webpack-dev-middleware) such that the developer isn't required to stop and run the test every time changes are made.</span></span> <span data-ttu-id="5bdb8-261">Se ele é o código em execução no caso de teste ou caso de teste em si, o teste é executado automaticamente.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-261">Whether it's the code running against the test case or the test case itself, the test runs automatically.</span></span>

<span data-ttu-id="5bdb8-262">Usando o aplicativo Angular como exemplo, dois casos de teste Jasmine já são fornecidos para o `CounterComponent` no *counter.component.spec.ts* arquivo:</span><span class="sxs-lookup"><span data-stu-id="5bdb8-262">Using the Angular application as an example, two Jasmine test cases are already provided for the `CounterComponent` in the *counter.component.spec.ts* file:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

<span data-ttu-id="5bdb8-263">Abra o prompt de comando de *ClientApp* diretório.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-263">Open the command prompt in the *ClientApp* directory.</span></span> <span data-ttu-id="5bdb8-264">Execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="5bdb8-264">Run the following command:</span></span>

```console
npm test
```

<span data-ttu-id="5bdb8-265">O script inicia o executor de teste Karma, que lê as configurações definidas na *karma.conf.js* arquivo.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-265">The script launches the Karma test runner, which reads the settings defined in the *karma.conf.js* file.</span></span> <span data-ttu-id="5bdb8-266">Entre outras configurações, o *karma.conf.js* identifica os arquivos de teste para ser executada por meio do seu `files` matriz:</span><span class="sxs-lookup"><span data-stu-id="5bdb8-266">Among other settings, the *karma.conf.js* identifies the test files to be executed via its `files` array:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

<a name="app-publishing"></a>

## <a name="publishing-the-application"></a><span data-ttu-id="5bdb8-267">Publicando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="5bdb8-267">Publishing the application</span></span>

<span data-ttu-id="5bdb8-268">Combinando os ativos gerados do lado do cliente e os artefatos publicados do ASP.NET Core em um pacote pronto para implantar pode ser complicado.</span><span class="sxs-lookup"><span data-stu-id="5bdb8-268">Combining the generated client-side assets and the published ASP.NET Core artifacts into a ready-to-deploy package can be cumbersome.</span></span> <span data-ttu-id="5bdb8-269">Felizmente, SpaServices orquestra o processo de publicação inteira com um destino MSBuild personalizado chamado `RunWebpack`:</span><span class="sxs-lookup"><span data-stu-id="5bdb8-269">Thankfully, SpaServices orchestrates that entire publication process with a custom MSBuild target named `RunWebpack`:</span></span>

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

<span data-ttu-id="5bdb8-270">O destino do MSBuild tem as seguintes responsabilidades:</span><span class="sxs-lookup"><span data-stu-id="5bdb8-270">The MSBuild target has the following responsibilities:</span></span>
1. <span data-ttu-id="5bdb8-271">Restaure os pacotes de npm</span><span class="sxs-lookup"><span data-stu-id="5bdb8-271">Restore the npm packages</span></span>
1. <span data-ttu-id="5bdb8-272">Criar uma compilação de nível de produção dos ativos de terceiros, do lado do cliente</span><span class="sxs-lookup"><span data-stu-id="5bdb8-272">Create a production-grade build of the third-party, client-side assets</span></span>
1. <span data-ttu-id="5bdb8-273">Criar uma compilação de nível de produção dos ativos do cliente personalizados</span><span class="sxs-lookup"><span data-stu-id="5bdb8-273">Create a production-grade build of the custom client-side assets</span></span>
1. <span data-ttu-id="5bdb8-274">Copie os ativos gerados pelo Webpack para a pasta de publicação</span><span class="sxs-lookup"><span data-stu-id="5bdb8-274">Copy the Webpack-generated assets to the publish folder</span></span>

<span data-ttu-id="5bdb8-275">O destino do MSBuild é chamado durante a execução:</span><span class="sxs-lookup"><span data-stu-id="5bdb8-275">The MSBuild target is invoked when running:</span></span>

```console
dotnet publish -c Release
```

## <a name="additional-resources"></a><span data-ttu-id="5bdb8-276">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="5bdb8-276">Additional resources</span></span>

* [<span data-ttu-id="5bdb8-277">Docs angular</span><span class="sxs-lookup"><span data-stu-id="5bdb8-277">Angular Docs</span></span>](https://angular.io/docs)
