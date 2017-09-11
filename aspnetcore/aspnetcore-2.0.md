---
title: Novidades do ASP.NET Core 2.0
author: rick-anderson
description: Novidades do ASP.NET Core 2.0
keywords: "ASP.NET Core, notas de versão, novidades"
ms.author: riande
manager: wpickett
ms.date: 07/10/2017
ms.topic: article
ms.assetid: 08c9f457-9c24-40f9-a08b-47dc251e4cec
ms.technology: aspnet
ms.prod: aspnet-core
uid: aspnetcore-2.0
ms.openlocfilehash: 2efb1ef63c378d84461da4f2d2f890423a670dda
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/25/2017
---
# <a name="whats-new-in-aspnet-core-20"></a><span data-ttu-id="56793-104">Novidades do ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="56793-104">What's new in ASP.NET Core 2.0</span></span>

<span data-ttu-id="56793-105">Este artigo destaca as alterações mais significativas no ASP.NET Core 2.0, com links para a documentação relevante.</span><span class="sxs-lookup"><span data-stu-id="56793-105">This article highlights the most significant changes in ASP.NET Core 2.0, with links to relevant documentation.</span></span>

## <a name="razor-pages"></a><span data-ttu-id="56793-106">Páginas do Razor</span><span class="sxs-lookup"><span data-stu-id="56793-106">Razor Pages</span></span>

<span data-ttu-id="56793-107">Páginas do Razor é um novo recurso do ASP.NET Core MVC que torna a codificação de cenários focados em página mais fácil e produtiva.</span><span class="sxs-lookup"><span data-stu-id="56793-107">Razor Pages is a new feature of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="56793-108">Para obter mais informações, consulte a introdução e o tutorial:</span><span class="sxs-lookup"><span data-stu-id="56793-108">For more information, see the introduction and tutorial:</span></span>

* [<span data-ttu-id="56793-109">Introdução a Páginas do Razor</span><span class="sxs-lookup"><span data-stu-id="56793-109">Introduction to Razor Pages</span></span>](xref:mvc/razor-pages/index)
* [<span data-ttu-id="56793-110">Introdução a Páginas do Razor</span><span class="sxs-lookup"><span data-stu-id="56793-110">Getting started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)

## <a name="aspnet-core-metapackage"></a><span data-ttu-id="56793-111">Metapacote do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="56793-111">ASP.NET Core metapackage</span></span>

<span data-ttu-id="56793-112">Um novo metapacote do ASP.NET Core inclui todos os pacotes feitos e com suporte pelas equipes do ASP.NET Core e do Entity Framework Core, juntamente com as respectivas dependências internas e de terceiros.</span><span class="sxs-lookup"><span data-stu-id="56793-112">A new ASP.NET Core metapackage includes all of the packages made and supported by the ASP.NET Core and Entity Framework Core teams, along with their internal and 3rd-party dependencies.</span></span> <span data-ttu-id="56793-113">Você não precisa mais escolher recursos individuais do ASP.NET Core por pacote.</span><span class="sxs-lookup"><span data-stu-id="56793-113">You no longer need to choose individual ASP.NET Core features by package.</span></span> <span data-ttu-id="56793-114">Todos os recursos estão incluídos no pacote [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All).</span><span class="sxs-lookup"><span data-stu-id="56793-114">All features are included in the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) package.</span></span> <span data-ttu-id="56793-115">Os modelos padrão usam este pacote.</span><span class="sxs-lookup"><span data-stu-id="56793-115">The default templates use this package.</span></span>

<span data-ttu-id="56793-116">Para obter mais informações, consulte [Metapacote do Microsoft.AspNetCore.All para ASP.NET Core 2.0](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="56793-116">For more information, see [Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.0](xref:fundamentals/metapackage).</span></span>

## <a name="runtime-store"></a><span data-ttu-id="56793-117">Repositório de tempo de execução</span><span class="sxs-lookup"><span data-stu-id="56793-117">Runtime Store</span></span>

<span data-ttu-id="56793-118">Aplicativos que usam o metapacote `Microsoft.AspNetCore.All` aproveitam automaticamente o novo repositório de tempo de execução do .NET Core.</span><span class="sxs-lookup"><span data-stu-id="56793-118">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the new .NET Core Runtime Store.</span></span> <span data-ttu-id="56793-119">O repositório contém todos os ativos de tempo de execução necessários para executar aplicativos ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="56793-119">The Store contains all the runtime assets needed to run ASP.NET Core 2.0 applications.</span></span> <span data-ttu-id="56793-120">Quando você usa o metapacote `Microsoft.AspNetCore.All`, nenhum ativo dos pacotes NuGet do ASP.NET Core referenciados são implantados com o aplicativo porque eles já estão no sistema de destino.</span><span class="sxs-lookup"><span data-stu-id="56793-120">When you use the `Microsoft.AspNetCore.All` metapackage, no assets from the referenced ASP.NET Core NuGet packages are deployed with the application because they already reside on the target system.</span></span> <span data-ttu-id="56793-121">Os ativos no repositório de tempo de execução também são pré-compilados para melhorar o tempo de inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="56793-121">The assets in the Runtime Store are also precompiled to improve application startup time.</span></span>

<span data-ttu-id="56793-122">Para obter mais informações, consulte [Repositório de tempo de execução](https://docs.microsoft.com/dotnet/core/deploying/runtime-store)</span><span class="sxs-lookup"><span data-stu-id="56793-122">For more information, see [Runtime store](https://docs.microsoft.com/dotnet/core/deploying/runtime-store)</span></span>

## <a name="net-standard-20"></a><span data-ttu-id="56793-123">.NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="56793-123">.NET Standard 2.0</span></span>

<span data-ttu-id="56793-124">Os pacotes do ASP.NET Core 2.0 são direcionados ao .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="56793-124">The ASP.NET Core 2.0 packages target .NET Standard 2.0.</span></span> <span data-ttu-id="56793-125">Os pacotes podem ser referenciados por outras bibliotecas do .NET Standard 2.0 e podem ser executados em implementações em conformidade com o .NET Standard 2.0, incluindo o .NET Core 2.0 e o .NET Framework 4.6.1.</span><span class="sxs-lookup"><span data-stu-id="56793-125">The packages can be referenced by other .NET Standard 2.0 libraries, and they can run on .NET Standard 2.0-compliant implementations of .NET, including .NET Core 2.0 and .NET Framework 4.6.1.</span></span> 

<span data-ttu-id="56793-126">O metapacote `Microsoft.AspNetCore.All` tem como destino apenas o .NET Core 2.0, porque ele se destina a ser usado com o repositório de tempo de execução do .NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="56793-126">The `Microsoft.AspNetCore.All` metapackage targets .NET Core 2.0 only, because it is intended to be used with the .NET Core 2.0 Runtime Store.</span></span>

## <a name="configuration-update"></a><span data-ttu-id="56793-127">Atualização da configuração</span><span class="sxs-lookup"><span data-stu-id="56793-127">Configuration update</span></span>

<span data-ttu-id="56793-128">Uma instância de `IConfiguration` é adicionada ao contêiner de serviços por padrão no ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="56793-128">An `IConfiguration` instance is added to the services container by default in ASP.NET Core 2.0.</span></span> <span data-ttu-id="56793-129">`IConfiguration` no contêiner de serviços torna mais fácil para aplicativos recuperarem valores de configuração do contêiner.</span><span class="sxs-lookup"><span data-stu-id="56793-129">`IConfiguration` in the services container makes it easier for applications to retrieve configuration values from the container.</span></span>

<span data-ttu-id="56793-130">Para obter informações sobre o status da documentação planejada, consulte o [problema do GitHub](https://github.com/aspnet/Docs/issues/3387).</span><span class="sxs-lookup"><span data-stu-id="56793-130">For information about the status of planned documentation, see the [GitHub issue](https://github.com/aspnet/Docs/issues/3387).</span></span>

## <a name="logging-update"></a><span data-ttu-id="56793-131">Atualização de registro em log</span><span class="sxs-lookup"><span data-stu-id="56793-131">Logging update</span></span>

<span data-ttu-id="56793-132">No ASP.NET Core 2.0, o log será incorporado no sistema de DI (injeção de dependência) por padrão.</span><span class="sxs-lookup"><span data-stu-id="56793-132">In ASP.NET Core 2.0, logging is incorporated into the dependency injection (DI) system by default.</span></span> <span data-ttu-id="56793-133">Você adiciona provedores e configura a filtragem no arquivo *Program.cs* em vez de usar o arquivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="56793-133">You add providers and configure filtering in the *Program.cs* file instead of in the *Startup.cs* file.</span></span> <span data-ttu-id="56793-134">E o `ILoggerFactory` padrão dá suporte à filtragem de forma que lhe permite usar uma abordagem flexível para filtragem entre provedores e filtragem específica do provedor.</span><span class="sxs-lookup"><span data-stu-id="56793-134">And the default `ILoggerFactory` supports filtering in a way that lets you use one flexible approach for both cross-provider filtering and specific-provider filtering.</span></span>

<span data-ttu-id="56793-135">Para obter mais informações, consulte [Introdução ao registro em log](xref:fundamentals/logging).</span><span class="sxs-lookup"><span data-stu-id="56793-135">For more information, see [Introduction to Logging](xref:fundamentals/logging).</span></span>

## <a name="authentication-update"></a><span data-ttu-id="56793-136">Atualização de autenticação</span><span class="sxs-lookup"><span data-stu-id="56793-136">Authentication update</span></span>

<span data-ttu-id="56793-137">Um novo modelo de autenticação torna mais fácil configurar a autenticação para um aplicativo usando a DI.</span><span class="sxs-lookup"><span data-stu-id="56793-137">A new authentication model makes it easier to configure authentication for an application using DI.</span></span>

<span data-ttu-id="56793-138">Novos modelos estão disponíveis para configurar a autenticação para aplicativos Web e APIs Web usando [Azure AD B2C] (https://azure.microsoft.com/services/active-directory-b2c/).</span><span class="sxs-lookup"><span data-stu-id="56793-138">New templates are available for configuring authentication for web apps and web APIs using [Azure AD B2C] (https://azure.microsoft.com/services/active-directory-b2c/).</span></span>

<span data-ttu-id="56793-139">Para obter informações sobre o status da documentação planejada, consulte o [problema do GitHub](https://github.com/aspnet/Docs/issues/3054).</span><span class="sxs-lookup"><span data-stu-id="56793-139">For information about the status of planned documentation, see the [GitHub issue](https://github.com/aspnet/Docs/issues/3054).</span></span>

## <a name="identity-update"></a><span data-ttu-id="56793-140">Atualização de identidade</span><span class="sxs-lookup"><span data-stu-id="56793-140">Identity update</span></span>

<span data-ttu-id="56793-141">Tornamos mais fácil criar APIs Web seguras usando a identidade do ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="56793-141">We've made it easier to build secure web APIs using Identity in ASP.NET Core 2.0.</span></span> <span data-ttu-id="56793-142">Você pode adquirir tokens de acesso para acessar suas APIs Web usando a [MSAL (Biblioteca de Autenticação da Microsoft)](https://www.nuget.org/packages/Microsoft.Identity.Client).</span><span class="sxs-lookup"><span data-stu-id="56793-142">You can acquire access tokens for accessing your web APIs using the [Microsoft Authentication Library (MSAL)](https://www.nuget.org/packages/Microsoft.Identity.Client).</span></span>

<span data-ttu-id="56793-143">Para obter mais informações sobre alterações de autenticação no 2.0, consulte os seguintes recursos:</span><span class="sxs-lookup"><span data-stu-id="56793-143">For more information on authentication changes in 2.0, see the following resources:</span></span>

* [<span data-ttu-id="56793-144">Confirmação de conta e de recuperação de senha no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="56793-144">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="56793-145">Habilitar a geração de código QR para aplicativos de autenticador no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="56793-145">Enabling QR Code generation for authenticator apps in ASP.NET Core</span></span>](xref:security/authentication/identity-enable-qrcodes)
* [<span data-ttu-id="56793-146">Migrando Autenticação e Identidade para o ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="56793-146">Migrating Authentication and Identity to ASP.NET Core 2.0</span></span>](xref:migration/1x-to-2x/identity-2x)

## <a name="spa-templates"></a><span data-ttu-id="56793-147">Modelos do SPA</span><span class="sxs-lookup"><span data-stu-id="56793-147">SPA templates</span></span>

<span data-ttu-id="56793-148">Modelos de projeto de SPA (aplicativo de página único) para Angular, Aurelia, Knockout.js, React.js e React.js com Redux estão disponíveis.</span><span class="sxs-lookup"><span data-stu-id="56793-148">Single Page Application (SPA) project templates for Angular, Aurelia, Knockout.js, React.js, and React.js with Redux are available.</span></span> <span data-ttu-id="56793-149">O modelo Angular foi atualizado para Angular 4.</span><span class="sxs-lookup"><span data-stu-id="56793-149">The Angular template has been updated to Angular 4.</span></span> <span data-ttu-id="56793-150">Os modelos Angular e React estão disponíveis por padrão. Para obter informações sobre como obter os outros modelos, consulte [Criar um novo projeto de SPA](xref:client-side/spa-services#creating-a-new-project).</span><span class="sxs-lookup"><span data-stu-id="56793-150">The Angular and React templates are available by default; for information about how to get the other templates, see [Creating a new SPA project](xref:client-side/spa-services#creating-a-new-project).</span></span> <span data-ttu-id="56793-151">Para obter informações sobre como criar um SPA no ASP.NET Core, consulte [Usando JavaScriptServices para criar aplicativos de página única](xref:client-side/spa-services).</span><span class="sxs-lookup"><span data-stu-id="56793-151">For information about how to build a SPA in ASP.NET Core, see [Using JavaScriptServices for Creating Single Page Applications](xref:client-side/spa-services).</span></span>

## <a name="kestrel-improvements"></a><span data-ttu-id="56793-152">Melhorias do Kestrel</span><span class="sxs-lookup"><span data-stu-id="56793-152">Kestrel improvements</span></span>

<span data-ttu-id="56793-153">O servidor Web Kestrel tem novos recursos que o tornam mais adequado como um servidor voltado para a Internet.</span><span class="sxs-lookup"><span data-stu-id="56793-153">The Kestrel web server has new features that make it more suitable as an Internet-facing server.</span></span> <span data-ttu-id="56793-154">Adicionamos um número de opções de configuração de restrição de servidor na nova propriedade `Limits` da classe `KestrelServerOptions`.</span><span class="sxs-lookup"><span data-stu-id="56793-154">We’ve added a number of server constraint configuration options in the `KestrelServerOptions` class’s new `Limits` property.</span></span> <span data-ttu-id="56793-155">Agora você pode adicionar limites para o seguinte:</span><span class="sxs-lookup"><span data-stu-id="56793-155">You can now add limits for the following:</span></span>

- <span data-ttu-id="56793-156">Número máximo de conexões de cliente</span><span class="sxs-lookup"><span data-stu-id="56793-156">Maximum client connections</span></span>
- <span data-ttu-id="56793-157">Tamanho máximo do corpo da solicitação</span><span class="sxs-lookup"><span data-stu-id="56793-157">Maximum request body size</span></span>
- <span data-ttu-id="56793-158">Taxa de dados mínima do corpo da solicitação</span><span class="sxs-lookup"><span data-stu-id="56793-158">Minimum request body data rate</span></span>

<span data-ttu-id="56793-159">Para obter mais informações, consulte [Implementação do servidor Web Kestrel no ASP.NET Core](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="56793-159">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel).</span></span>

## <a name="weblistener-renamed-to-httpsys"></a><span data-ttu-id="56793-160">WebListener renomeado para HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="56793-160">WebListener renamed to HTTP.sys</span></span>

<span data-ttu-id="56793-161">Os pacotes `Microsoft.AspNetCore.Server.WebListener` e `Microsoft.Net.Http.Server` foram mesclados em um novo pacote `Microsoft.AspNetCore.Server.HttpSys`.</span><span class="sxs-lookup"><span data-stu-id="56793-161">The packages `Microsoft.AspNetCore.Server.WebListener` and `Microsoft.Net.Http.Server` have been merged into a new package `Microsoft.AspNetCore.Server.HttpSys`.</span></span> <span data-ttu-id="56793-162">Os namespaces foram atualizados para corresponderem.</span><span class="sxs-lookup"><span data-stu-id="56793-162">The namespaces have been updated to match.</span></span>

<span data-ttu-id="56793-163">Para obter mais informações, consulte [Implementação do servidor Web HTTP.sys no ASP.NET Core](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="56793-163">For more information, see [HTTP.sys web server implementation in ASP.NET Core](xref:fundamentals/servers/httpsys).</span></span>

## <a name="enhanced-http-header-support"></a><span data-ttu-id="56793-164">Suporte aprimorado a cabeçalho HTTP</span><span class="sxs-lookup"><span data-stu-id="56793-164">Enhanced HTTP header support</span></span>

<span data-ttu-id="56793-165">Ao usar o MVC para transmitir um `FileStreamResult` ou um `FileContentResult`, agora você tem a opção de definir uma `ETag` ou uma data `LastModified` no conteúdo que você transmitir.</span><span class="sxs-lookup"><span data-stu-id="56793-165">When using MVC to transmit a `FileStreamResult` or a `FileContentResult`, you now have the option to set an `ETag` or a `LastModified` date on the content you transmit.</span></span> <span data-ttu-id="56793-166">Você pode definir esses valores no conteúdo retornado com código semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="56793-166">You can set these values on the returned content with code similar to the following:</span></span>

```csharp
var data = Encoding.UTF8.GetBytes("This is a sample text from a binary array");
var entityTag = new EntityTagHeaderValue("\"MyCalculatedEtagValue\"");
return File(data, "text/plain", "downloadName.txt", lastModified: DateTime.UtcNow.AddSeconds(-5), entityTag: entityTag);
```

<span data-ttu-id="56793-167">O arquivo retornado para os visitantes será decorado com os cabeçalhos HTTP apropriados para os valores `ETag` e `LastModified`.</span><span class="sxs-lookup"><span data-stu-id="56793-167">The file returned to your visitors will be decorated with the appropriate HTTP headers for the `ETag` and `LastModified` values.</span></span>

<span data-ttu-id="56793-168">Se um visitante de aplicativo solicita o conteúdo com um cabeçalho de solicitação de intervalo, o ASP.NET reconhece isso e manipula esse cabeçalho.</span><span class="sxs-lookup"><span data-stu-id="56793-168">If an application visitor requests content with a Range Request header, ASP.NET will recognize that and handle that header.</span></span> <span data-ttu-id="56793-169">Se o conteúdo solicitado puder ser parcialmente entregue, o ASP.NET ignorará adequadamente e retornará apenas o conjunto de bytes solicitado.</span><span class="sxs-lookup"><span data-stu-id="56793-169">If the requested content can be partially delivered, ASP.NET will appropriately skip and return just the requested set of bytes.</span></span>  <span data-ttu-id="56793-170">Você não precisa escrever nenhum manipulador especial em seus métodos para adaptar ou manipular esse recurso; ele é manipulado automaticamente para você.</span><span class="sxs-lookup"><span data-stu-id="56793-170">You do not need to write any special handlers into your methods to adapt or handle this feature; it is automatically handled for you.</span></span>

## <a name="hosting-startup-and-application-insights"></a><span data-ttu-id="56793-171">Inicialização de hospedagem e o Application Insights</span><span class="sxs-lookup"><span data-stu-id="56793-171">Hosting startup and Application Insights</span></span>

<span data-ttu-id="56793-172">Ambientes de hospedagem podem injetar dependências de pacote extras e executar código durante a inicialização do aplicativo, sem que o aplicativo precise tomar uma dependência explicitamente ou chamar algum método.</span><span class="sxs-lookup"><span data-stu-id="56793-172">Hosting environments can now inject extra package dependencies and execute code during application startup, without the application needing to explicitly take a dependency or call any methods.</span></span> <span data-ttu-id="56793-173">Esse recurso pode ser usado para habilitar determinados ambientes para recursos de "esclarecimento" exclusivos para esse ambiente sem que o aplicativo precise saber antecipadamente.</span><span class="sxs-lookup"><span data-stu-id="56793-173">This feature can be used to enable certain environments to "light-up" features unique to that environment without the application needing to know ahead of time.</span></span> 

<span data-ttu-id="56793-174">No ASP.NET Core 2.0, esse recurso é usado para habilitar o diagnóstico do Application Insights automaticamente durante a depuração no Visual Studio e (depois de optar por isto) quando em execução nos Serviços de Aplicativos do Azure.</span><span class="sxs-lookup"><span data-stu-id="56793-174">In ASP.NET Core 2.0, this feature is used to automatically enable Application Insights diagnostics when debugging in Visual Studio and (after opting in) when running in Azure App Services.</span></span> <span data-ttu-id="56793-175">Como resultado, os modelos de projeto não adicionam mais código e pacotes do Application Insights por padrão.</span><span class="sxs-lookup"><span data-stu-id="56793-175">As a result, the project templates no longer add Application Insights packages and code by default.</span></span>

<span data-ttu-id="56793-176">Para obter informações sobre o status da documentação planejada, consulte o [problema do GitHub](https://github.com/aspnet/Docs/issues/3389).</span><span class="sxs-lookup"><span data-stu-id="56793-176">For information about the status of planned documentation, see the [GitHub issue](https://github.com/aspnet/Docs/issues/3389).</span></span>

## <a name="automatic-use-of-anti-forgery-tokens"></a><span data-ttu-id="56793-177">Uso automático de tokens antifalsificação</span><span class="sxs-lookup"><span data-stu-id="56793-177">Automatic use of anti-forgery tokens</span></span>

<span data-ttu-id="56793-178">O ASP.NET Core sempre ajudou a fazer a codificação HTML de seu conteúdo por padrão, mas com a nova versão, estamos dando uma passo adicional para ajudar a impedir ataques de XSRF (falsificação de solicitação entre sites).</span><span class="sxs-lookup"><span data-stu-id="56793-178">ASP.NET Core has always helped HTML-encode your content by default, but with the new version we’re taking an extra step to help prevent cross-site request forgery (XSRF) attacks.</span></span> <span data-ttu-id="56793-179">O ASP.NET Core agora emitirá tokens antifalsificação por padrão e os validará em ações de POST de formulário e em páginas sem configuração adicional.</span><span class="sxs-lookup"><span data-stu-id="56793-179">ASP.NET Core will now emit anti-forgery tokens by default and validate them on form POST actions and pages without extra configuration.</span></span>

<span data-ttu-id="56793-180">Para obter mais informações, consulte [Impedindo ataques de falsificação de solicitação entre sites (CSRF/XSRF) no ASP.NET Core](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="56793-180">For more information, see [Preventing Cross-Site Request Forgery (XSRF/CSRF) Attacks in ASP.NET Core](xref:security/anti-request-forgery).</span></span>

## <a name="automatic-precompilation"></a><span data-ttu-id="56793-181">Pré-compilação automática</span><span class="sxs-lookup"><span data-stu-id="56793-181">Automatic precompilation</span></span>

<span data-ttu-id="56793-182">A pré-compilação da exibição do Razor é habilitada durante a publicação por padrão, reduzindo o tamanho da saída de publicação e o tempo de inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="56793-182">Razor view pre-compilation is enabled during publish by default, reducing the publish output size and application startup time.</span></span>

## <a name="razor-support-for-c-71"></a><span data-ttu-id="56793-183">Suporte ao Razor para C# 7.1</span><span class="sxs-lookup"><span data-stu-id="56793-183">Razor support for C# 7.1</span></span>

<span data-ttu-id="56793-184">O mecanismo de exibição Razor foi atualizado para funcionar com o novo compilador Roslyn.</span><span class="sxs-lookup"><span data-stu-id="56793-184">The Razor view engine has been updated to work with the new Roslyn compiler.</span></span> <span data-ttu-id="56793-185">Isso inclui suporte para recursos do C# 7.1 como expressões padrão, nomes de tupla inferidos e correspondência de padrões com genéricos.</span><span class="sxs-lookup"><span data-stu-id="56793-185">That includes support for C# 7.1 features like Default Expressions, Inferred Tuple Names, and Pattern-Matching with Generics.</span></span> <span data-ttu-id="56793-186">Para usar o C# 7.1 em seu projeto, adicione a seguinte propriedade no arquivo de projeto e, em seguida, recarregue a solução:</span><span class="sxs-lookup"><span data-stu-id="56793-186">To use C# 7.1 in your project, add the following property in your project file and then reload the solution:</span></span>

```xml
<LangVersion>latest</LangVersion>
```

<span data-ttu-id="56793-187">Para obter informações sobre o status dos recursos do C# 7.1, consulte [o repositório GitHub do Roslyn](https://github.com/dotnet/roslyn/blob/master/docs/Language%20Feature%20Status.md).</span><span class="sxs-lookup"><span data-stu-id="56793-187">For information about the status of C# 7.1 features, see [the Roslyn GitHub repository](https://github.com/dotnet/roslyn/blob/master/docs/Language%20Feature%20Status.md).</span></span>

## <a name="other-documentation-updates-for-20"></a><span data-ttu-id="56793-188">Outras atualizações de documentação para 2.0</span><span class="sxs-lookup"><span data-stu-id="56793-188">Other documentation updates for 2.0</span></span>

* [<span data-ttu-id="56793-189">Criar perfis de publicação para o Visual Studio e o MSBuild para implantar aplicativos ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="56793-189">Create publish profiles for Visual Studio and MSBuild, to deploy ASP.NET Core apps</span></span>](xref:publishing/web-publishing-vs)
* [<span data-ttu-id="56793-190">Gerenciamento de chaves</span><span class="sxs-lookup"><span data-stu-id="56793-190">Key Management</span></span>](xref:security/data-protection/implementation/key-management)
* [<span data-ttu-id="56793-191">Configurando a autenticação do Facebook</span><span class="sxs-lookup"><span data-stu-id="56793-191">Configuring Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="56793-192">Configurando a autenticação do Twitter</span><span class="sxs-lookup"><span data-stu-id="56793-192">Configuring Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="56793-193">Configurando a autenticação do Google</span><span class="sxs-lookup"><span data-stu-id="56793-193">Configuring Google authentication</span></span>](xref:security/authentication/google-logins)
* [<span data-ttu-id="56793-194">Configurando a autenticação da Conta da Microsoft</span><span class="sxs-lookup"><span data-stu-id="56793-194">Configuring Microsoft Account authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="56793-195">Configurando HTTPS para desenvolvimento no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="56793-195">Setting up HTTPS for development in ASP.NET Core</span></span>](xref:security/https)

## <a name="migration-guidance"></a><span data-ttu-id="56793-196">Diretrizes de migração</span><span class="sxs-lookup"><span data-stu-id="56793-196">Migration guidance</span></span>

<span data-ttu-id="56793-197">Para obter diretrizes sobre como migrar aplicativos ASP.NET Core 1.x para o ASP.NET Core 2.0, consulte os seguintes recursos:</span><span class="sxs-lookup"><span data-stu-id="56793-197">For guidance on how to migrate ASP.NET Core 1.x applications to ASP.NET Core 2.0, see the following resources:</span></span>

* [<span data-ttu-id="56793-198">Migrando do ASP.NET Core 1.x para o ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="56793-198">Migrating from ASP.NET Core 1.x to ASP.NET Core 2.0</span></span>](xref:migration/1x-to-2x/index)
* [<span data-ttu-id="56793-199">Migrando Autenticação e Identidade para o ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="56793-199">Migrating Authentication and Identity to ASP.NET Core 2.0</span></span>](xref:migration/1x-to-2x/identity-2x)

## <a name="additional-information"></a><span data-ttu-id="56793-200">Informações adicionais</span><span class="sxs-lookup"><span data-stu-id="56793-200">Additional Information</span></span>

<span data-ttu-id="56793-201">Para obter uma lista de alterações, consulte as [Notas de versão do ASP.NET Core 2.0](https://github.com/aspnet/Home/releases/tag/2.0.0).</span><span class="sxs-lookup"><span data-stu-id="56793-201">For the complete list of changes, see the [ASP.NET Core 2.0 Release Notes](https://github.com/aspnet/Home/releases/tag/2.0.0).</span></span>

<span data-ttu-id="56793-202">Se você deseja ficar conectado com o progresso e os planos da equipe de desenvolvimento do ASP.NET Core, prepare-se para a [palestra semanal da comunidade do ASP.NET](https://live.asp.net/).</span><span class="sxs-lookup"><span data-stu-id="56793-202">If you’d like to connect with the ASP.NET Core development team’s progress and plans, tune in to the weekly [ASP.NET Community Standup](https://live.asp.net/).</span></span>
