---
uid: web-api/overview/advanced/configuring-aspnet-web-api
title: Configurando o ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2014
ms.topic: article
ms.assetid: 9e10a700-8d91-4d2e-a31e-b8b569fe867c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/configuring-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: f9b471fe2afdce278869a2e4d9b693a78030324b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="configuring-aspnet-web-api-2"></a><span data-ttu-id="f820b-102">Configurando o ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="f820b-102">Configuring ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="f820b-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f820b-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="f820b-104">Este tópico descreve como configurar a API da Web do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f820b-104">This topic describes how to configure ASP.NET Web API.</span></span>

- [<span data-ttu-id="f820b-105">Definições de configuração</span><span class="sxs-lookup"><span data-stu-id="f820b-105">Configuration Settings</span></span>](#settings)
- [<span data-ttu-id="f820b-106">Configurando a API da Web com o ASP.NET de hospedagem</span><span class="sxs-lookup"><span data-stu-id="f820b-106">Configuring Web API with ASP.NET Hosting</span></span>](#webhost)
- [<span data-ttu-id="f820b-107">Configurando a API da Web com auto-hospedagem OWIN</span><span class="sxs-lookup"><span data-stu-id="f820b-107">Configuring Web API with OWIN Self-Hosting</span></span>](#selfhost)
- [<span data-ttu-id="f820b-108">Serviços de API da Web global</span><span class="sxs-lookup"><span data-stu-id="f820b-108">Global Web API Services</span></span>](#services)
- [<span data-ttu-id="f820b-109">Configuração por controlador</span><span class="sxs-lookup"><span data-stu-id="f820b-109">Per-Controller Configuration</span></span>](#percontrollerconfig)

<a id="settings"></a>
## <a name="configuration-settings"></a><span data-ttu-id="f820b-110">Definições de configuração</span><span class="sxs-lookup"><span data-stu-id="f820b-110">Configuration Settings</span></span>

<span data-ttu-id="f820b-111">Configurações de configuração da API da Web são definidas no [HttpConfiguration](https://msdn.microsoft.com/library/system.web.http.httpconfiguration.aspx) classe.</span><span class="sxs-lookup"><span data-stu-id="f820b-111">Web API configuration setttings are defined in the [HttpConfiguration](https://msdn.microsoft.com/library/system.web.http.httpconfiguration.aspx) class.</span></span>

| <span data-ttu-id="f820b-112">Membro</span><span class="sxs-lookup"><span data-stu-id="f820b-112">Member</span></span> | <span data-ttu-id="f820b-113">Descrição</span><span class="sxs-lookup"><span data-stu-id="f820b-113">Description</span></span> |
| --- | --- |
| <span data-ttu-id="f820b-114">**DependencyResolver**</span><span class="sxs-lookup"><span data-stu-id="f820b-114">**DependencyResolver**</span></span> | <span data-ttu-id="f820b-115">Habilita a injeção de dependência para controladores.</span><span class="sxs-lookup"><span data-stu-id="f820b-115">Enables dependency injection for controllers.</span></span> <span data-ttu-id="f820b-116">Consulte [usando o resolvedor de dependência de API da Web](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="f820b-116">See [Using the Web API Dependency Resolver](dependency-injection.md).</span></span> |
| <span data-ttu-id="f820b-117">**Filtros**</span><span class="sxs-lookup"><span data-stu-id="f820b-117">**Filters**</span></span> | <span data-ttu-id="f820b-118">Filtros de ação.</span><span class="sxs-lookup"><span data-stu-id="f820b-118">Action filters.</span></span> |
| <span data-ttu-id="f820b-119">**Formatadores**</span><span class="sxs-lookup"><span data-stu-id="f820b-119">**Formatters**</span></span> | <span data-ttu-id="f820b-120">[Formatadores de tipo de mídia](../formats-and-model-binding/media-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="f820b-120">[Media-type formatters](../formats-and-model-binding/media-formatters.md).</span></span> |
| <span data-ttu-id="f820b-121">**IncludeErrorDetailPolicy**</span><span class="sxs-lookup"><span data-stu-id="f820b-121">**IncludeErrorDetailPolicy**</span></span> | <span data-ttu-id="f820b-122">Especifica se o servidor deve incluir os detalhes do erro, como mensagens de exceção e rastreamentos de pilha, nas mensagens de resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="f820b-122">Specifies whether the server should include error details, such as exception messages and stack traces, in HTTP response messages.</span></span> <span data-ttu-id="f820b-123">Consulte [IncludeErrorDetailPolicy](https://msdn.microsoft.com/library/system.web.http.includeerrordetailpolicy(v=vs.108)).</span><span class="sxs-lookup"><span data-stu-id="f820b-123">See [IncludeErrorDetailPolicy](https://msdn.microsoft.com/library/system.web.http.includeerrordetailpolicy(v=vs.108)).</span></span> |
| <span data-ttu-id="f820b-124">**Inicializador**</span><span class="sxs-lookup"><span data-stu-id="f820b-124">**Initializer**</span></span> | <span data-ttu-id="f820b-125">Uma função que executa a inicialização final do **HttpConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="f820b-125">A function that performs final initialization of the **HttpConfiguration**.</span></span> |
| <span data-ttu-id="f820b-126">**MessageHandlers**</span><span class="sxs-lookup"><span data-stu-id="f820b-126">**MessageHandlers**</span></span> | <span data-ttu-id="f820b-127">[Manipuladores de mensagens HTTP](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="f820b-127">[HTTP message handlers](http-message-handlers.md).</span></span> |
| <span data-ttu-id="f820b-128">**ParameterBindingRules**</span><span class="sxs-lookup"><span data-stu-id="f820b-128">**ParameterBindingRules**</span></span> | <span data-ttu-id="f820b-129">Uma coleção de regras para parâmetros de associação em ações do controlador.</span><span class="sxs-lookup"><span data-stu-id="f820b-129">A collection of rules for binding parameters on controller actions.</span></span> |
| <span data-ttu-id="f820b-130">**Propriedades**</span><span class="sxs-lookup"><span data-stu-id="f820b-130">**Properties**</span></span> | <span data-ttu-id="f820b-131">Um recipiente de propriedades genéricas.</span><span class="sxs-lookup"><span data-stu-id="f820b-131">A generic property bag.</span></span> |
| <span data-ttu-id="f820b-132">**Rotas**</span><span class="sxs-lookup"><span data-stu-id="f820b-132">**Routes**</span></span> | <span data-ttu-id="f820b-133">A coleção de rotas.</span><span class="sxs-lookup"><span data-stu-id="f820b-133">The collection of routes.</span></span> <span data-ttu-id="f820b-134">Consulte [roteamento na API da Web ASP.NET](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="f820b-134">See [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span> |
| <span data-ttu-id="f820b-135">**Serviços**</span><span class="sxs-lookup"><span data-stu-id="f820b-135">**Services**</span></span> | <span data-ttu-id="f820b-136">A coleção de serviços.</span><span class="sxs-lookup"><span data-stu-id="f820b-136">The collection of services.</span></span> <span data-ttu-id="f820b-137">Consulte [serviços](#services).</span><span class="sxs-lookup"><span data-stu-id="f820b-137">See [Services](#services).</span></span> |


## <a name="prerequisites"></a><span data-ttu-id="f820b-138">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="f820b-138">Prerequisites</span></span>

<span data-ttu-id="f820b-139">[Visual Studio de 2017](https://www.visualstudio.com/vs/) Community, Professional ou Enterprise Edition.</span><span class="sxs-lookup"><span data-stu-id="f820b-139">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional, or Enterprise Edition.</span></span>

<a id="webhost"></a>
## <a name="configuring-web-api-with-aspnet-hosting"></a><span data-ttu-id="f820b-140">Configurando a API da Web com o ASP.NET de hospedagem</span><span class="sxs-lookup"><span data-stu-id="f820b-140">Configuring Web API with ASP.NET Hosting</span></span>

<span data-ttu-id="f820b-141">Em um aplicativo ASP.NET, configure a API Web chamando [GlobalConfiguration.Configure](https://msdn.microsoft.com/library/system.web.http.globalconfiguration.configure.aspx) no **aplicativo\_iniciar** método.</span><span class="sxs-lookup"><span data-stu-id="f820b-141">In an ASP.NET application, configure Web API by calling [GlobalConfiguration.Configure](https://msdn.microsoft.com/library/system.web.http.globalconfiguration.configure.aspx) in the **Application\_Start** method.</span></span> <span data-ttu-id="f820b-142">O **configurar** método usa um delegado com um único parâmetro do tipo **HttpConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="f820b-142">The **Configure** method takes a delegate with a single parameter of type **HttpConfiguration**.</span></span> <span data-ttu-id="f820b-143">Faz a configuração dentro do delegado.</span><span class="sxs-lookup"><span data-stu-id="f820b-143">Perform all of your configuation inside the delegate.</span></span>

<span data-ttu-id="f820b-144">Aqui está um exemplo que usa um delegado anônimo:</span><span class="sxs-lookup"><span data-stu-id="f820b-144">Here is an example using an anonymous delegate:</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="f820b-145">No Visual Studio de 2017, o modelo de projeto "Aplicativo Web do ASP.NET" define automaticamente o código de configuração, se você selecionar "API Web" no **novo projeto ASP.NET** caixa de diálogo.</span><span class="sxs-lookup"><span data-stu-id="f820b-145">In Visual Studio 2017, the "ASP.NET Web Application" project template automatically sets up the configuration code, if you select "Web API" in the **New ASP.NET Project** dialog.</span></span>

[![](configuring-aspnet-web-api/_static/image2.png)](configuring-aspnet-web-api/_static/image1.png)

<span data-ttu-id="f820b-146">O modelo de projeto cria um arquivo chamado WebApiConfig.cs dentro do aplicativo\_pasta inicial.</span><span class="sxs-lookup"><span data-stu-id="f820b-146">The project template creates a file named WebApiConfig.cs inside the App\_Start folder.</span></span> <span data-ttu-id="f820b-147">Arquivo de código define o delegado em que você deve colocar o seu código de configuração de API da Web.</span><span class="sxs-lookup"><span data-stu-id="f820b-147">This code file defines the delegate where you should put your Web API configuration code.</span></span>

![](configuring-aspnet-web-api/_static/image3.png)

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample2.cs?highlight=12)]

<span data-ttu-id="f820b-148">O modelo de projeto também adiciona o código que chama o representante de **aplicativo\_iniciar**.</span><span class="sxs-lookup"><span data-stu-id="f820b-148">The project template also adds the code that calls the delegate from **Application\_Start**.</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample3.cs?highlight=5)]

<a id="selfhost"></a>
## <a name="configuring-web-api-with-owin-self-hosting"></a><span data-ttu-id="f820b-149">Configurando a API da Web com auto-hospedagem OWIN</span><span class="sxs-lookup"><span data-stu-id="f820b-149">Configuring Web API with OWIN Self-Hosting</span></span>

<span data-ttu-id="f820b-150">Se você estiver hospedagem interna com OWIN, crie um novo **HttpConfiguration** instância.</span><span class="sxs-lookup"><span data-stu-id="f820b-150">If you are self-hosting with OWIN, create a new **HttpConfiguration** instance.</span></span> <span data-ttu-id="f820b-151">Executar todas as configurações nessa instância e, em seguida, passe a instância para o **Owin.UseWebApi** método de extensão.</span><span class="sxs-lookup"><span data-stu-id="f820b-151">Perform any configuration on this instance, and then pass the instance to the **Owin.UseWebApi** extension method.</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample4.cs)]

<span data-ttu-id="f820b-152">O tutorial [OWIN de uso para Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md) mostra as etapas completas.</span><span class="sxs-lookup"><span data-stu-id="f820b-152">The tutorial [Use OWIN to Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md) shows the complete steps.</span></span>

<a id="services"></a>
## <a name="global-web-api-services"></a><span data-ttu-id="f820b-153">Serviços de API da Web global</span><span class="sxs-lookup"><span data-stu-id="f820b-153">Global Web API Services</span></span>

<span data-ttu-id="f820b-154">O **HttpConfiguration.Services** coleção contém um conjunto de serviços globais API da Web usa para executar várias tarefas, como a negociação de conteúdo e seleção de controlador.</span><span class="sxs-lookup"><span data-stu-id="f820b-154">The **HttpConfiguration.Services** collection contains a set of global services that Web API uses to perform various tasks, such as controller selection and content negotiation.</span></span>

> [!NOTE]
> <span data-ttu-id="f820b-155">O **serviços** coleção não é um mecanismo para fins gerais para injeção de dependência ou de descoberta de serviço.</span><span class="sxs-lookup"><span data-stu-id="f820b-155">The **Services** collection is not a general-purpose mechanism for service discovery or dependency injection.</span></span> <span data-ttu-id="f820b-156">Ele armazena somente os tipos de serviço que são conhecidos para a estrutura da API Web.</span><span class="sxs-lookup"><span data-stu-id="f820b-156">It only stores service types that are known to the Web API framework.</span></span>


<span data-ttu-id="f820b-157">O **serviços** coleção foi inicializada com um conjunto padrão de serviços e pode fornecer suas próprias implementações personalizadas.</span><span class="sxs-lookup"><span data-stu-id="f820b-157">The **Services** collection is initialized with a default set of services, and you can provide your own custom implementations.</span></span> <span data-ttu-id="f820b-158">Alguns serviços oferecem suporte a várias instâncias, enquanto outros podem ter apenas uma instância.</span><span class="sxs-lookup"><span data-stu-id="f820b-158">Some services support multiple instances, while others can have only one instance.</span></span> <span data-ttu-id="f820b-159">(No entanto, você também pode fornecer serviços no nível do controlador, consulte [por controlador configuração](#percontrollerconfig).</span><span class="sxs-lookup"><span data-stu-id="f820b-159">(However, you can also provide services at the controller level; see [Per-Controller Configuration](#percontrollerconfig).</span></span>

<span data-ttu-id="f820b-160">Serviços de instância única</span><span class="sxs-lookup"><span data-stu-id="f820b-160">Single-Instance Services</span></span>


| <span data-ttu-id="f820b-161">Serviço</span><span class="sxs-lookup"><span data-stu-id="f820b-161">Service</span></span> | <span data-ttu-id="f820b-162">Descrição</span><span class="sxs-lookup"><span data-stu-id="f820b-162">Description</span></span> |
| --- | --- |
| <span data-ttu-id="f820b-163">**IActionValueBinder**</span><span class="sxs-lookup"><span data-stu-id="f820b-163">**IActionValueBinder**</span></span> | <span data-ttu-id="f820b-164">Obtém uma associação para um parâmetro.</span><span class="sxs-lookup"><span data-stu-id="f820b-164">Gets a binding for a parameter.</span></span> |
| <span data-ttu-id="f820b-165">**IApiExplorer**</span><span class="sxs-lookup"><span data-stu-id="f820b-165">**IApiExplorer**</span></span> | <span data-ttu-id="f820b-166">Obtém as descrições das APIs expostas pelo aplicativo.</span><span class="sxs-lookup"><span data-stu-id="f820b-166">Gets descriptions of the APIs exposed by the application.</span></span> <span data-ttu-id="f820b-167">Consulte [criando uma página de ajuda para uma API da Web](../getting-started-with-aspnet-web-api/creating-api-help-pages.md).</span><span class="sxs-lookup"><span data-stu-id="f820b-167">See [Creating a Help Page for a Web API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md).</span></span> |
| <span data-ttu-id="f820b-168">**IAssembliesResolver**</span><span class="sxs-lookup"><span data-stu-id="f820b-168">**IAssembliesResolver**</span></span> | <span data-ttu-id="f820b-169">Obtém uma lista de assemblies para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="f820b-169">Gets a list of the assemblies for the application.</span></span> <span data-ttu-id="f820b-170">Consulte [roteamento e seleção de ação](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="f820b-170">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="f820b-171">**IBodyModelValidator**</span><span class="sxs-lookup"><span data-stu-id="f820b-171">**IBodyModelValidator**</span></span> | <span data-ttu-id="f820b-172">Valida um modelo que ler o corpo da solicitação por um formatador de tipo de mídia.</span><span class="sxs-lookup"><span data-stu-id="f820b-172">Validates a model that is read from the request body by a media-type formatter.</span></span> |
| <span data-ttu-id="f820b-173">**IContentNegotiator**</span><span class="sxs-lookup"><span data-stu-id="f820b-173">**IContentNegotiator**</span></span> | <span data-ttu-id="f820b-174">Realiza a negociação de conteúdo.</span><span class="sxs-lookup"><span data-stu-id="f820b-174">Performs content negotiation.</span></span> |
| <span data-ttu-id="f820b-175">**IDocumentationProvider**</span><span class="sxs-lookup"><span data-stu-id="f820b-175">**IDocumentationProvider**</span></span> | <span data-ttu-id="f820b-176">Fornece documentação para APIs.</span><span class="sxs-lookup"><span data-stu-id="f820b-176">Provides documentation for APIs.</span></span> <span data-ttu-id="f820b-177">O padrão é **nulo**.</span><span class="sxs-lookup"><span data-stu-id="f820b-177">The default is **null**.</span></span> <span data-ttu-id="f820b-178">Consulte [criando uma página de ajuda para uma API da Web](../getting-started-with-aspnet-web-api/creating-api-help-pages.md).</span><span class="sxs-lookup"><span data-stu-id="f820b-178">See [Creating a Help Page for a Web API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md).</span></span> |
| <span data-ttu-id="f820b-179">**IHostBufferPolicySelector**</span><span class="sxs-lookup"><span data-stu-id="f820b-179">**IHostBufferPolicySelector**</span></span> | <span data-ttu-id="f820b-180">Indica se o host deve armazenar em buffer de corpo de entidade de mensagens HTTP.</span><span class="sxs-lookup"><span data-stu-id="f820b-180">Indicates whether the host should buffer HTTP message entity bodies.</span></span> |
| <span data-ttu-id="f820b-181">**IHttpActionInvoker**</span><span class="sxs-lookup"><span data-stu-id="f820b-181">**IHttpActionInvoker**</span></span> | <span data-ttu-id="f820b-182">Invoca uma ação do controlador.</span><span class="sxs-lookup"><span data-stu-id="f820b-182">Invokes a controller action.</span></span> <span data-ttu-id="f820b-183">Consulte [roteamento e seleção de ação](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="f820b-183">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="f820b-184">**IHttpActionSelector**</span><span class="sxs-lookup"><span data-stu-id="f820b-184">**IHttpActionSelector**</span></span> | <span data-ttu-id="f820b-185">Seleciona uma ação do controlador.</span><span class="sxs-lookup"><span data-stu-id="f820b-185">Selects a controller action.</span></span> <span data-ttu-id="f820b-186">Consulte [roteamento e seleção de ação](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="f820b-186">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="f820b-187">**IHttpControllerActivator**</span><span class="sxs-lookup"><span data-stu-id="f820b-187">**IHttpControllerActivator**</span></span> | <span data-ttu-id="f820b-188">Ativa um controlador.</span><span class="sxs-lookup"><span data-stu-id="f820b-188">Activates a controller.</span></span> <span data-ttu-id="f820b-189">Consulte [roteamento e seleção de ação](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="f820b-189">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="f820b-190">**IHttpControllerSelector**</span><span class="sxs-lookup"><span data-stu-id="f820b-190">**IHttpControllerSelector**</span></span> | <span data-ttu-id="f820b-191">Seleciona um controlador.</span><span class="sxs-lookup"><span data-stu-id="f820b-191">Selects a controller.</span></span> <span data-ttu-id="f820b-192">Consulte [roteamento e seleção de ação](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="f820b-192">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="f820b-193">**IHttpControllerTypeResolver**</span><span class="sxs-lookup"><span data-stu-id="f820b-193">**IHttpControllerTypeResolver**</span></span> | <span data-ttu-id="f820b-194">Fornece uma lista dos tipos de controlador de API da Web no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="f820b-194">Provides a list of the Web API controller types in the application.</span></span> <span data-ttu-id="f820b-195">Consulte [roteamento e seleção de ação](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="f820b-195">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="f820b-196">**ITraceManager**</span><span class="sxs-lookup"><span data-stu-id="f820b-196">**ITraceManager**</span></span> | <span data-ttu-id="f820b-197">Inicializa a estrutura de rastreamento.</span><span class="sxs-lookup"><span data-stu-id="f820b-197">Initializes the tracing framework.</span></span> <span data-ttu-id="f820b-198">Consulte [rastreamento no ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="f820b-198">See [Tracing in ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md).</span></span> |
| <span data-ttu-id="f820b-199">**ITraceWriter**</span><span class="sxs-lookup"><span data-stu-id="f820b-199">**ITraceWriter**</span></span> | <span data-ttu-id="f820b-200">Fornece um gravador de rastreamento.</span><span class="sxs-lookup"><span data-stu-id="f820b-200">Provides a trace writer.</span></span> <span data-ttu-id="f820b-201">O padrão é um gravador de rastreamento "no-op".</span><span class="sxs-lookup"><span data-stu-id="f820b-201">The default is a "no-op" trace writer.</span></span> <span data-ttu-id="f820b-202">Consulte [rastreamento no ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="f820b-202">See [Tracing in ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md).</span></span> |
| <span data-ttu-id="f820b-203">**IModelValidatorCache**</span><span class="sxs-lookup"><span data-stu-id="f820b-203">**IModelValidatorCache**</span></span> | <span data-ttu-id="f820b-204">Fornece um cache de validadores de modelo.</span><span class="sxs-lookup"><span data-stu-id="f820b-204">Provides a cache of model validators.</span></span> |

<span data-ttu-id="f820b-205">Serviços de várias instâncias</span><span class="sxs-lookup"><span data-stu-id="f820b-205">Multiple-Instance Services</span></span>


| <span data-ttu-id="f820b-206">Serviço</span><span class="sxs-lookup"><span data-stu-id="f820b-206">Service</span></span> | <span data-ttu-id="f820b-207">Descrição</span><span class="sxs-lookup"><span data-stu-id="f820b-207">Description</span></span> |
| --- | --- |
| <span data-ttu-id="f820b-208">**IFilterProvider**</span><span class="sxs-lookup"><span data-stu-id="f820b-208">**IFilterProvider**</span></span> | <span data-ttu-id="f820b-209">Retorna uma lista de filtros para uma ação do controlador.</span><span class="sxs-lookup"><span data-stu-id="f820b-209">Returns a list of filters for a controller action.</span></span> |
| <span data-ttu-id="f820b-210">**ModelBinderProvider**</span><span class="sxs-lookup"><span data-stu-id="f820b-210">**ModelBinderProvider**</span></span> | <span data-ttu-id="f820b-211">Retorna um associador de modelo para um determinado tipo.</span><span class="sxs-lookup"><span data-stu-id="f820b-211">Returns a model binder for a given type.</span></span> |
| <span data-ttu-id="f820b-212">**ModelMetadataProvider**</span><span class="sxs-lookup"><span data-stu-id="f820b-212">**ModelMetadataProvider**</span></span> | <span data-ttu-id="f820b-213">Fornece metadados para um modelo.</span><span class="sxs-lookup"><span data-stu-id="f820b-213">Provides metadata for a model.</span></span> |
| <span data-ttu-id="f820b-214">**ModelValidatorProvider**</span><span class="sxs-lookup"><span data-stu-id="f820b-214">**ModelValidatorProvider**</span></span> | <span data-ttu-id="f820b-215">Fornece um validador para um modelo.</span><span class="sxs-lookup"><span data-stu-id="f820b-215">Provides a validator for a model.</span></span> |
| <span data-ttu-id="f820b-216">**ValueProviderFactory**</span><span class="sxs-lookup"><span data-stu-id="f820b-216">**ValueProviderFactory**</span></span> | <span data-ttu-id="f820b-217">Cria um provedor de valor.</span><span class="sxs-lookup"><span data-stu-id="f820b-217">Creates a value provider.</span></span> <span data-ttu-id="f820b-218">Para obter mais informações, consulte Mike Stall postagem de blog [como criar um provedor de valor personalizado no WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)</span><span class="sxs-lookup"><span data-stu-id="f820b-218">For more information, see Mike Stall's blog post [How to create a custom value provider in WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)</span></span> |<span data-ttu-id="f820b-219">.</span><span class="sxs-lookup"><span data-stu-id="f820b-219">.</span></span>

<span data-ttu-id="f820b-220">Para adicionar uma implementação personalizada para um serviço de várias instâncias, chame **adicionar** ou **inserir** no **serviços** coleção:</span><span class="sxs-lookup"><span data-stu-id="f820b-220">To add a custom implementation to a multi-instance service, call **Add** or **Insert** on the **Services** collection:</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="f820b-221">Para substituir um serviço de instância única com uma implementação personalizada, chame **substituir** no **serviços** coleção:</span><span class="sxs-lookup"><span data-stu-id="f820b-221">To replace a single-instance service with a custom implementation, call **Replace** on the **Services** collection:</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample6.cs)]

<a id="percontrollerconfig"></a>
## <a name="per-controller-configuration"></a><span data-ttu-id="f820b-222">Configuração por controlador</span><span class="sxs-lookup"><span data-stu-id="f820b-222">Per-Controller Configuration</span></span>

<span data-ttu-id="f820b-223">Você pode substituir as configurações a seguir em uma base por controlador:</span><span class="sxs-lookup"><span data-stu-id="f820b-223">You can override the following settings on a per-controller basis:</span></span>

- <span data-ttu-id="f820b-224">Formatadores de tipo de mídia</span><span class="sxs-lookup"><span data-stu-id="f820b-224">Media-type formatters</span></span>
- <span data-ttu-id="f820b-225">Regras de associação de parâmetro</span><span class="sxs-lookup"><span data-stu-id="f820b-225">Parameter binding rules</span></span>
- <span data-ttu-id="f820b-226">Serviços</span><span class="sxs-lookup"><span data-stu-id="f820b-226">Services</span></span>

<span data-ttu-id="f820b-227">Para fazer isso, defina um atributo personalizado que implementa o **IControllerConfiguration** interface.</span><span class="sxs-lookup"><span data-stu-id="f820b-227">To do so, define a custom attribute that implements the **IControllerConfiguration** interface.</span></span> <span data-ttu-id="f820b-228">Em seguida, aplique o atributo para o controlador.</span><span class="sxs-lookup"><span data-stu-id="f820b-228">Then apply the attribute to the controller.</span></span>

<span data-ttu-id="f820b-229">O exemplo a seguir substitui os formatadores de tipo de mídia padrão por um formatador personalizado.</span><span class="sxs-lookup"><span data-stu-id="f820b-229">The following example replaces the default media-type formatters with a custom formatter.</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="f820b-230">O **IControllerConfiguration.Initialize** método aceita dois parâmetros:</span><span class="sxs-lookup"><span data-stu-id="f820b-230">The **IControllerConfiguration.Initialize** method takes two parameters:</span></span>

- <span data-ttu-id="f820b-231">Um **HttpControllerSettings** objeto</span><span class="sxs-lookup"><span data-stu-id="f820b-231">An **HttpControllerSettings** object</span></span>
- <span data-ttu-id="f820b-232">Um **HttpControllerDescriptor** objeto</span><span class="sxs-lookup"><span data-stu-id="f820b-232">An **HttpControllerDescriptor** object</span></span>

<span data-ttu-id="f820b-233">O **HttpControllerDescriptor** contém uma descrição do controlador, você pode examinar para fins informativos (digamos, para distinguir entre os dois controladores).</span><span class="sxs-lookup"><span data-stu-id="f820b-233">The **HttpControllerDescriptor** contains a description of the controller, which you can examine for informational purposes (say, to distinguish between two controllers).</span></span>

<span data-ttu-id="f820b-234">Use o **HttpControllerSettings** objeto para configurar o controlador.</span><span class="sxs-lookup"><span data-stu-id="f820b-234">Use the **HttpControllerSettings** object to configure the controller.</span></span> <span data-ttu-id="f820b-235">Este objeto contém o subconjunto de parâmetros de configuração que pode ser substituído em uma base por controlador.</span><span class="sxs-lookup"><span data-stu-id="f820b-235">This object contains the subset of configuration parameters that you can override on a per-controller basis.</span></span> <span data-ttu-id="f820b-236">Todas as configurações que você não altere o padrão global **HttpConfiguration** objeto.</span><span class="sxs-lookup"><span data-stu-id="f820b-236">Any settings that you don't change default to the global **HttpConfiguration** object.</span></span>
