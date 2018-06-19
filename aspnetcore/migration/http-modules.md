---
title: Migrar manipuladores HTTP e módulos para o ASP.NET Core middleware
author: rick-anderson
description: ''
manager: wpickett
ms.author: tdykstra
ms.date: 12/07/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/http-modules
ms.openlocfilehash: cbdef871ffc3269e3118d23ed20306a71b9df030
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/03/2018
ms.locfileid: "32741044"
---
# <a name="migrate-http-handlers-and-modules-to-aspnet-core-middleware"></a><span data-ttu-id="b1ee7-102">Migrar manipuladores HTTP e módulos para o ASP.NET Core middleware</span><span class="sxs-lookup"><span data-stu-id="b1ee7-102">Migrate HTTP handlers and modules to ASP.NET Core middleware</span></span>

<span data-ttu-id="b1ee7-103">Por [Matt Perdeck](https://www.linkedin.com/in/mattperdeck)</span><span class="sxs-lookup"><span data-stu-id="b1ee7-103">By [Matt Perdeck](https://www.linkedin.com/in/mattperdeck)</span></span>

<span data-ttu-id="b1ee7-104">Este artigo mostra como migrar ASP.NET existente [módulos HTTP e manipuladores de System. webServer](/iis/configuration/system.webserver/) para ASP.NET Core [middleware](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="b1ee7-104">This article shows how to migrate existing ASP.NET [HTTP modules and handlers from system.webserver](/iis/configuration/system.webserver/) to ASP.NET Core [middleware](xref:fundamentals/middleware/index).</span></span>

## <a name="modules-and-handlers-revisited"></a><span data-ttu-id="b1ee7-105">Manipuladores revisitados e módulos</span><span class="sxs-lookup"><span data-stu-id="b1ee7-105">Modules and handlers revisited</span></span>

<span data-ttu-id="b1ee7-106">Antes de prosseguir para o ASP.NET Core middleware, vejamos primeiro novamente como manipuladores e módulos HTTP funcionam:</span><span class="sxs-lookup"><span data-stu-id="b1ee7-106">Before proceeding to ASP.NET Core middleware, let's first recap how HTTP modules and handlers work:</span></span>

![Manipulador de módulos](http-modules/_static/moduleshandlers.png)

<span data-ttu-id="b1ee7-108">**Manipuladores são:**</span><span class="sxs-lookup"><span data-stu-id="b1ee7-108">**Handlers are:**</span></span>

   * <span data-ttu-id="b1ee7-109">As classes que implementam [IHttpHandler](/dotnet/api/system.web.ihttphandler)</span><span class="sxs-lookup"><span data-stu-id="b1ee7-109">Classes that implement [IHttpHandler](/dotnet/api/system.web.ihttphandler)</span></span>

   * <span data-ttu-id="b1ee7-110">Usado para manipular solicitações com uma extensão, ou o nome de arquivo fornecido como *relatório*</span><span class="sxs-lookup"><span data-stu-id="b1ee7-110">Used to handle requests with a given file name or extension, such as *.report*</span></span>

   * <span data-ttu-id="b1ee7-111">[Configurado](/iis/configuration/system.webserver/handlers/) em *Web. config*</span><span class="sxs-lookup"><span data-stu-id="b1ee7-111">[Configured](/iis/configuration/system.webserver/handlers/) in *Web.config*</span></span>

<span data-ttu-id="b1ee7-112">**Os módulos são:**</span><span class="sxs-lookup"><span data-stu-id="b1ee7-112">**Modules are:**</span></span>

   * <span data-ttu-id="b1ee7-113">As classes que implementam [IHttpModule](/dotnet/api/system.web.ihttpmodule)</span><span class="sxs-lookup"><span data-stu-id="b1ee7-113">Classes that implement [IHttpModule](/dotnet/api/system.web.ihttpmodule)</span></span>

   * <span data-ttu-id="b1ee7-114">Chamado para cada solicitação</span><span class="sxs-lookup"><span data-stu-id="b1ee7-114">Invoked for every request</span></span>

   * <span data-ttu-id="b1ee7-115">Capaz de curto-circuito (Interromper processamento adicional de uma solicitação)</span><span class="sxs-lookup"><span data-stu-id="b1ee7-115">Able to short-circuit (stop further processing of a request)</span></span>

   * <span data-ttu-id="b1ee7-116">Capaz de adicionar a resposta HTTP, ou criar seus próprios</span><span class="sxs-lookup"><span data-stu-id="b1ee7-116">Able to add to the HTTP response, or create their own</span></span>

   * <span data-ttu-id="b1ee7-117">[Configurado](/iis/configuration/system.webserver/modules/) em *Web. config*</span><span class="sxs-lookup"><span data-stu-id="b1ee7-117">[Configured](/iis/configuration/system.webserver/modules/) in *Web.config*</span></span>

<span data-ttu-id="b1ee7-118">**A ordem em que os módulos de processam solicitações de entrada é determinada por:**</span><span class="sxs-lookup"><span data-stu-id="b1ee7-118">**The order in which modules process incoming requests is determined by:**</span></span>

   1. <span data-ttu-id="b1ee7-119">O [ciclo de vida do aplicativo](https://msdn.microsoft.com/library/ms227673.aspx), que é um eventos série acionado pelo ASP.NET: [BeginRequest](/dotnet/api/system.web.httpapplication.beginrequest), [AuthenticateRequest](/dotnet/api/system.web.httpapplication.authenticaterequest), etc. Cada módulo pode criar um manipulador de eventos de um ou mais.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-119">The [application life cycle](https://msdn.microsoft.com/library/ms227673.aspx), which is a series events fired by ASP.NET: [BeginRequest](/dotnet/api/system.web.httpapplication.beginrequest), [AuthenticateRequest](/dotnet/api/system.web.httpapplication.authenticaterequest), etc. Each module can create a handler for one or more events.</span></span>

   2. <span data-ttu-id="b1ee7-120">Para o mesmo evento, a ordem na qual eles foram configurados no *Web. config*.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-120">For the same event, the order in which they're configured in *Web.config*.</span></span>

<span data-ttu-id="b1ee7-121">Além de módulos, você pode adicionar manipuladores para os eventos de ciclo de vida para o *Global.asax.cs* arquivo.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-121">In addition to modules, you can add handlers for the life cycle events to your *Global.asax.cs* file.</span></span> <span data-ttu-id="b1ee7-122">Esses manipuladores executar após os manipuladores nos módulos configurados.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-122">These handlers run after the handlers in the configured modules.</span></span>

## <a name="from-handlers-and-modules-to-middleware"></a><span data-ttu-id="b1ee7-123">Manipuladores e módulos de middleware</span><span class="sxs-lookup"><span data-stu-id="b1ee7-123">From handlers and modules to middleware</span></span>

<span data-ttu-id="b1ee7-124">**Middleware são mais simples do que manipuladores e módulos HTTP:**</span><span class="sxs-lookup"><span data-stu-id="b1ee7-124">**Middleware are simpler than HTTP modules and handlers:**</span></span>

   * <span data-ttu-id="b1ee7-125">Módulos, manipuladores, *Global.asax.cs*, *Web. config* (exceto para a configuração do IIS) e o ciclo de vida do aplicativo serão excluídos</span><span class="sxs-lookup"><span data-stu-id="b1ee7-125">Modules, handlers, *Global.asax.cs*, *Web.config* (except for IIS configuration) and the application life cycle are gone</span></span>

   * <span data-ttu-id="b1ee7-126">As funções dos módulos e manipuladores de tem sido controladas por middleware</span><span class="sxs-lookup"><span data-stu-id="b1ee7-126">The roles of both modules and handlers have been taken over by middleware</span></span>

   * <span data-ttu-id="b1ee7-127">Middleware são configurados usando o código em vez de em *Web. config*</span><span class="sxs-lookup"><span data-stu-id="b1ee7-127">Middleware are configured using code rather than in *Web.config*</span></span>

   * <span data-ttu-id="b1ee7-128">[Ramificação de pipeline](xref:fundamentals/middleware/index#middleware-run-map-use) permite que você envia solicitações ao middleware específico, com base na URL não só mas também em cabeçalhos de solicitação, cadeias de caracteres de consulta, etc.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-128">[Pipeline branching](xref:fundamentals/middleware/index#middleware-run-map-use) lets you send requests to specific middleware, based on not only the URL but also on request headers, query strings, etc.</span></span>

<span data-ttu-id="b1ee7-129">**Middleware são muito semelhantes aos módulos:**</span><span class="sxs-lookup"><span data-stu-id="b1ee7-129">**Middleware are very similar to modules:**</span></span>

   * <span data-ttu-id="b1ee7-130">Invocado em princípio para cada solicitação</span><span class="sxs-lookup"><span data-stu-id="b1ee7-130">Invoked in principle for every request</span></span>

   * <span data-ttu-id="b1ee7-131">Capaz de curto-circuito a uma solicitação por [não passar a solicitação para o próximo middleware](#http-modules-shortcircuiting-middleware)</span><span class="sxs-lookup"><span data-stu-id="b1ee7-131">Able to short-circuit a request, by [not passing the request to the next middleware](#http-modules-shortcircuiting-middleware)</span></span>

   * <span data-ttu-id="b1ee7-132">Capaz de criar sua própria resposta HTTP</span><span class="sxs-lookup"><span data-stu-id="b1ee7-132">Able to create their own HTTP response</span></span>

<span data-ttu-id="b1ee7-133">**Middleware e módulos são processados em uma ordem diferente:**</span><span class="sxs-lookup"><span data-stu-id="b1ee7-133">**Middleware and modules are processed in a different order:**</span></span>

   * <span data-ttu-id="b1ee7-134">Ordem de middleware é baseada na ordem em que inserção no pipeline de solicitação, enquanto a ordem dos módulos baseia-se principalmente em [ciclo de vida do aplicativo](https://msdn.microsoft.com/library/ms227673.aspx) eventos</span><span class="sxs-lookup"><span data-stu-id="b1ee7-134">Order of middleware is based on the order in which they're inserted into the request pipeline, while order of modules is mainly based on [application life cycle](https://msdn.microsoft.com/library/ms227673.aspx) events</span></span>

   * <span data-ttu-id="b1ee7-135">Ordem de middleware para respostas é o oposto do que para solicitações, enquanto a ordem dos módulos é o mesmo para solicitações e respostas</span><span class="sxs-lookup"><span data-stu-id="b1ee7-135">Order of middleware for responses is the reverse from that for requests, while order of modules is the same for requests and responses</span></span>

   * <span data-ttu-id="b1ee7-136">Consulte [criar um pipeline de middleware com IApplicationBuilder](xref:fundamentals/middleware/index#creating-a-middleware-pipeline-with-iapplicationbuilder)</span><span class="sxs-lookup"><span data-stu-id="b1ee7-136">See [Create a middleware pipeline with IApplicationBuilder](xref:fundamentals/middleware/index#creating-a-middleware-pipeline-with-iapplicationbuilder)</span></span>

![Middleware](http-modules/_static/middleware.png)

<span data-ttu-id="b1ee7-138">Observe como na imagem acima, o middleware de autenticação curto-circuito a solicitação.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-138">Note how in the image above, the authentication middleware short-circuited the request.</span></span>

## <a name="migrating-module-code-to-middleware"></a><span data-ttu-id="b1ee7-139">Migrando o código de módulo para middleware</span><span class="sxs-lookup"><span data-stu-id="b1ee7-139">Migrating module code to middleware</span></span>

<span data-ttu-id="b1ee7-140">Um módulo HTTP existente será semelhante a este:</span><span class="sxs-lookup"><span data-stu-id="b1ee7-140">An existing HTTP module will look similar to this:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyModule.cs?highlight=6,8,24,31)]

<span data-ttu-id="b1ee7-141">Conforme mostrado no [Middleware](xref:fundamentals/middleware/index) página, um middleware ASP.NET Core é uma classe que expõe um `Invoke` colocando método um `HttpContext` e retornar um `Task`.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-141">As shown in the [Middleware](xref:fundamentals/middleware/index) page, an ASP.NET Core middleware is a class that exposes an `Invoke` method taking an `HttpContext` and returning a `Task`.</span></span> <span data-ttu-id="b1ee7-142">Seu novo middleware será assim:</span><span class="sxs-lookup"><span data-stu-id="b1ee7-142">Your new middleware will look like this:</span></span>

<a name="http-modules-usemiddleware"></a>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddleware.cs?highlight=9,13,20,24,28,30,32)]

<span data-ttu-id="b1ee7-143">O modelo de middleware anterior foi obtido da seção de [gravar middleware](xref:fundamentals/middleware/index#middleware-writing-middleware).</span><span class="sxs-lookup"><span data-stu-id="b1ee7-143">The preceding middleware template was taken from the section on [writing middleware](xref:fundamentals/middleware/index#middleware-writing-middleware).</span></span>

<span data-ttu-id="b1ee7-144">O *MyMiddlewareExtensions* classe auxiliar torna mais fácil de configurar o middleware em seu `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-144">The *MyMiddlewareExtensions* helper class makes it easier to configure your middleware in your `Startup` class.</span></span> <span data-ttu-id="b1ee7-145">O `UseMyMiddleware` método adiciona sua classe de middleware no pipeline de solicitação.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-145">The `UseMyMiddleware` method adds your middleware class to the request pipeline.</span></span> <span data-ttu-id="b1ee7-146">Serviços necessários para o middleware são injetados no construtor do middleware.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-146">Services required by the middleware get injected in the middleware's constructor.</span></span>

<a name="http-modules-shortcircuiting-middleware"></a>

<span data-ttu-id="b1ee7-147">O módulo pode encerrar uma solicitação, por exemplo, se o usuário não autorizado:</span><span class="sxs-lookup"><span data-stu-id="b1ee7-147">Your module might terminate a request, for example if the user isn't authorized:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyTerminatingModule.cs?highlight=9,10,11,12,13&name=snippet_Terminate)]

<span data-ttu-id="b1ee7-148">Um middleware lida com isso chamando não `Invoke` no próximo middleware no pipeline.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-148">A middleware handles this by not calling `Invoke` on the next middleware in the pipeline.</span></span> <span data-ttu-id="b1ee7-149">Tenha em mente que isso não totalmente encerra a solicitação, pois middlewares anterior ainda será invocado quando a resposta torna sua maneira novamente por meio do pipeline.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-149">Keep in mind that this doesn't fully terminate the request, because previous middlewares will still be invoked when the response makes its way back through the pipeline.</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyTerminatingMiddleware.cs?highlight=7,8&name=snippet_Terminate)]

<span data-ttu-id="b1ee7-150">Ao migrar a funcionalidade do módulo para o seu middleware de novo, você pode achar que seu código não compilado porque o `HttpContext` classe tiverem sido alterados significativamente no núcleo do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-150">When you migrate your module's functionality to your new middleware, you may find that your code doesn't compile because the `HttpContext` class has significantly changed in ASP.NET Core.</span></span> <span data-ttu-id="b1ee7-151">[Mais tarde](#migrating-to-the-new-httpcontext), você verá como migrar para o ASP.NET Core HttpContext novo.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-151">[Later on](#migrating-to-the-new-httpcontext), you'll see how to migrate to the new ASP.NET Core HttpContext.</span></span>

## <a name="migrating-module-insertion-into-the-request-pipeline"></a><span data-ttu-id="b1ee7-152">Migrando inserção de módulo no pipeline de solicitação</span><span class="sxs-lookup"><span data-stu-id="b1ee7-152">Migrating module insertion into the request pipeline</span></span>

<span data-ttu-id="b1ee7-153">Módulos HTTP normalmente são adicionados ao pipeline de solicitação usando *Web. config*:</span><span class="sxs-lookup"><span data-stu-id="b1ee7-153">HTTP modules are typically added to the request pipeline using *Web.config*:</span></span>

[!code-xml[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32-33,36,43,50,101)]

<span data-ttu-id="b1ee7-154">Converter isso por [adicionando seu novo middleware](xref:fundamentals/middleware/index#creating-a-middleware-pipeline-with-iapplicationbuilder) para o pipeline de solicitação no seu `Startup` classe:</span><span class="sxs-lookup"><span data-stu-id="b1ee7-154">Convert this by [adding your new middleware](xref:fundamentals/middleware/index#creating-a-middleware-pipeline-with-iapplicationbuilder) to the request pipeline in your `Startup` class:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=16)]

<span data-ttu-id="b1ee7-155">Ponto no pipeline, em que você insere seu novo middleware exato depende do evento que ela tratada como um módulo (`BeginRequest`, `EndRequest`, etc.) e sua ordem na lista de módulos em *Web. config*.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-155">The exact spot in the pipeline where you insert your new middleware depends on the event that it handled as a module (`BeginRequest`, `EndRequest`, etc.) and its order in your list of modules in *Web.config*.</span></span>

<span data-ttu-id="b1ee7-156">Como anteriormente não mencionado, há mais nenhum ciclo de vida do aplicativo no núcleo do ASP.NET e a ordem na qual as respostas são processadas pelo middleware diferente daquela usada pelos módulos.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-156">As previously stated, there's no application life cycle in ASP.NET Core and the order in which responses are processed by middleware differs from the order used by modules.</span></span> <span data-ttu-id="b1ee7-157">Isso pode tornar sua decisão de ordenação mais difícil.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-157">This could make your ordering decision more challenging.</span></span>

<span data-ttu-id="b1ee7-158">Se ordenação se torna um problema, você pode dividir seu módulo em vários componentes de middleware que podem ser ordenados de forma independente.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-158">If ordering becomes a problem, you could split your module into multiple middleware components that can be ordered independently.</span></span>

## <a name="migrating-handler-code-to-middleware"></a><span data-ttu-id="b1ee7-159">Migrando o código de manipulador de middleware</span><span class="sxs-lookup"><span data-stu-id="b1ee7-159">Migrating handler code to middleware</span></span>

<span data-ttu-id="b1ee7-160">Um manipulador HTTP parecida com esta:</span><span class="sxs-lookup"><span data-stu-id="b1ee7-160">An HTTP handler looks something like this:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/HttpHandlers/ReportHandler.cs?highlight=5,7,13,14,15,16)]

<span data-ttu-id="b1ee7-161">No seu projeto do ASP.NET Core, isso pode ser traduzido para um middleware semelhante a este:</span><span class="sxs-lookup"><span data-stu-id="b1ee7-161">In your ASP.NET Core project, you would translate this to a middleware similar to this:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/ReportHandlerMiddleware.cs?highlight=7,9,13,20,21,22,23,40,42,44)]

<span data-ttu-id="b1ee7-162">Este middleware é muito semelhante para o middleware correspondente a módulos.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-162">This middleware is very similar to the middleware corresponding to modules.</span></span> <span data-ttu-id="b1ee7-163">A única diferença é que não há aqui nenhuma chamada para `_next.Invoke(context)`.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-163">The only real difference is that here there's no call to `_next.Invoke(context)`.</span></span> <span data-ttu-id="b1ee7-164">Isso faz sentido, porque o manipulador não é no final do pipeline de solicitação, que será a nenhum próximo middleware para invocar.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-164">That makes sense, because the handler is at the end of the request pipeline, so there will be no next middleware to invoke.</span></span>

## <a name="migrating-handler-insertion-into-the-request-pipeline"></a><span data-ttu-id="b1ee7-165">Migrando inserção manipulador no pipeline de solicitação</span><span class="sxs-lookup"><span data-stu-id="b1ee7-165">Migrating handler insertion into the request pipeline</span></span>

<span data-ttu-id="b1ee7-166">Configurar um manipulador HTTP é feita em *Web. config* e tem a seguinte aparência:</span><span class="sxs-lookup"><span data-stu-id="b1ee7-166">Configuring an HTTP handler is done in *Web.config* and looks something like this:</span></span>

[!code-xml[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32,46-48,50,101)]

<span data-ttu-id="b1ee7-167">Você pode convertê-lo adicionando seu novo middleware de manipulador para o pipeline de solicitação no seu `Startup` classe, semelhante ao middleware convertido de módulos.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-167">You could convert this by adding your new handler middleware to the request pipeline in your `Startup` class, similar to middleware converted from modules.</span></span> <span data-ttu-id="b1ee7-168">O problema com essa abordagem é que ele poderia enviar todas as solicitações para seu novo middleware de manipulador.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-168">The problem with that approach is that it would send all requests to your new handler middleware.</span></span> <span data-ttu-id="b1ee7-169">No entanto, você precisará apenas solicitações com uma determinada extensão para alcançar seu middleware.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-169">However, you only want requests with a given extension to reach your middleware.</span></span> <span data-ttu-id="b1ee7-170">Essa seria a mesma funcionalidade que tinha com o manipulador HTTP.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-170">That would give you the same functionality you had with your HTTP handler.</span></span>

<span data-ttu-id="b1ee7-171">Uma solução é ramificar o pipeline de solicitações com uma determinada extensão, usando o `MapWhen` método de extensão.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-171">One solution is to branch the pipeline for requests with a given extension, using the `MapWhen` extension method.</span></span> <span data-ttu-id="b1ee7-172">Você pode fazer isso no mesmo `Configure` método em que você adiciona o middleware outros:</span><span class="sxs-lookup"><span data-stu-id="b1ee7-172">You do this in the same `Configure` method where you add the other middleware:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=27-34)]

<span data-ttu-id="b1ee7-173">`MapWhen` usa esses parâmetros:</span><span class="sxs-lookup"><span data-stu-id="b1ee7-173">`MapWhen` takes these parameters:</span></span>

1. <span data-ttu-id="b1ee7-174">Uma expressão lambda que usa o `HttpContext` e retorna `true` se a solicitação deve ir para a ramificação.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-174">A lambda that takes the `HttpContext` and returns `true` if the request should go down the branch.</span></span> <span data-ttu-id="b1ee7-175">Isso significa que você pode se ramificar solicitações não apenas com base em sua extensão, mas também em cabeçalhos de solicitação, parâmetros de cadeia de caracteres de consulta, etc.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-175">This means you can branch requests not just based on their extension, but also on request headers, query string parameters, etc.</span></span>

2. <span data-ttu-id="b1ee7-176">Uma expressão lambda que leva um `IApplicationBuilder` e adiciona todos o middleware para a ramificação.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-176">A lambda that takes an `IApplicationBuilder` and adds all the middleware for the branch.</span></span> <span data-ttu-id="b1ee7-177">Isso significa que você pode adicionar middleware adicional para a ramificação na frente do seu manipulador middleware.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-177">This means you can add additional middleware to the branch in front of your handler middleware.</span></span>

<span data-ttu-id="b1ee7-178">Middleware adicionados ao pipeline antes da ramificação será invocada em todas as solicitações; a ramificação não terá impacto sobre eles.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-178">Middleware added to the pipeline before the branch will be invoked on all requests; the branch will have no impact on them.</span></span>

## <a name="loading-middleware-options-using-the-options-pattern"></a><span data-ttu-id="b1ee7-179">Opções de middleware usando o padrão de opções de carregamento</span><span class="sxs-lookup"><span data-stu-id="b1ee7-179">Loading middleware options using the options pattern</span></span>

<span data-ttu-id="b1ee7-180">Alguns módulos e manipuladores têm opções de configuração que são armazenadas em *Web. config*. No entanto, no ASP.NET Core um novo modelo de configuração é usado no lugar de *Web. config*.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-180">Some modules and handlers have configuration options that are stored in *Web.config*. However, in ASP.NET Core a new configuration model is used in place of *Web.config*.</span></span>

<span data-ttu-id="b1ee7-181">O novo [sistema de configuração](xref:fundamentals/configuration/index) oferece essas opções para resolver isso:</span><span class="sxs-lookup"><span data-stu-id="b1ee7-181">The new [configuration system](xref:fundamentals/configuration/index) gives you these options to solve this:</span></span>

* <span data-ttu-id="b1ee7-182">Injetar diretamente as opções para o middleware, conforme o [próxima seção](#loading-middleware-options-through-direct-injection).</span><span class="sxs-lookup"><span data-stu-id="b1ee7-182">Directly inject the options into the middleware, as shown in the [next section](#loading-middleware-options-through-direct-injection).</span></span>

* <span data-ttu-id="b1ee7-183">Use o [padrão de opções](xref:fundamentals/configuration/options):</span><span class="sxs-lookup"><span data-stu-id="b1ee7-183">Use the [options pattern](xref:fundamentals/configuration/options):</span></span>

1. <span data-ttu-id="b1ee7-184">Crie uma classe para manter suas opções de middleware, por exemplo:</span><span class="sxs-lookup"><span data-stu-id="b1ee7-184">Create a class to hold your middleware options, for example:</span></span>

   [!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Options)]

2. <span data-ttu-id="b1ee7-185">Armazenar os valores de opção</span><span class="sxs-lookup"><span data-stu-id="b1ee7-185">Store the option values</span></span>

   <span data-ttu-id="b1ee7-186">O sistema de configuração permite que você armazene valores de opção em qualquer local desejado.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-186">The configuration system allows you to store option values anywhere you want.</span></span> <span data-ttu-id="b1ee7-187">No entanto, a maioria dos locais use *appSettings. JSON*, portanto, vamos essa abordagem:</span><span class="sxs-lookup"><span data-stu-id="b1ee7-187">However, most sites use *appsettings.json*, so we'll take that approach:</span></span>

   [!code-json[](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,14-18)]

   <span data-ttu-id="b1ee7-188">*MyMiddlewareOptionsSection* aqui é um nome de seção.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-188">*MyMiddlewareOptionsSection* here is a section name.</span></span> <span data-ttu-id="b1ee7-189">Ele não precisa ser igual ao nome da sua classe de opções.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-189">It doesn't have to be the same as the name of your options class.</span></span>

3. <span data-ttu-id="b1ee7-190">Associe os valores de opção com a classe de opções</span><span class="sxs-lookup"><span data-stu-id="b1ee7-190">Associate the option values with the options class</span></span>

    <span data-ttu-id="b1ee7-191">O padrão de opções usa a estrutura de injeção de dependência do ASP.NET Core para associar o tipo de opções (como `MyMiddlewareOptions`) com um `MyMiddlewareOptions` objeto que tem as opções reais.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-191">The options pattern uses ASP.NET Core's dependency injection framework to associate the options type (such as `MyMiddlewareOptions`) with a `MyMiddlewareOptions` object that has the actual options.</span></span>

    <span data-ttu-id="b1ee7-192">Atualização de seu `Startup` classe:</span><span class="sxs-lookup"><span data-stu-id="b1ee7-192">Update your `Startup` class:</span></span>

   1. <span data-ttu-id="b1ee7-193">Se você estiver usando *appSettings. JSON*, adicioná-lo para o construtor de configuração no `Startup` construtor:</span><span class="sxs-lookup"><span data-stu-id="b1ee7-193">If you're using *appsettings.json*, add it to the configuration builder in the `Startup` constructor:</span></span>

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Ctor&highlight=5-6)]

   2. <span data-ttu-id="b1ee7-194">Configure o serviço de opções:</span><span class="sxs-lookup"><span data-stu-id="b1ee7-194">Configure the options service:</span></span>

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

   3. <span data-ttu-id="b1ee7-195">Associe as opções de sua classe de opções:</span><span class="sxs-lookup"><span data-stu-id="b1ee7-195">Associate your options with your options class:</span></span>

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=6-8)]

4. <span data-ttu-id="b1ee7-196">Inserir as opções para o construtor de middleware.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-196">Inject the options into your middleware constructor.</span></span> <span data-ttu-id="b1ee7-197">Isso é semelhante a injeção de opções em um controlador.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-197">This is similar to injecting options into a controller.</span></span>

   [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_MiddlewareWithParams&highlight=4,7,10,15-16)]

   <span data-ttu-id="b1ee7-198">O [UseMiddleware](#http-modules-usemiddleware) método de extensão que adiciona o middleware para o `IApplicationBuilder` cuida de injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-198">The [UseMiddleware](#http-modules-usemiddleware) extension method that adds your middleware to the `IApplicationBuilder` takes care of dependency injection.</span></span>

   <span data-ttu-id="b1ee7-199">Isso não é limitado a `IOptions` objetos.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-199">This isn't limited to `IOptions` objects.</span></span> <span data-ttu-id="b1ee7-200">Qualquer objeto que requer o middleware pode ser inserido dessa maneira.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-200">Any other object that your middleware requires can be injected this way.</span></span>

## <a name="loading-middleware-options-through-direct-injection"></a><span data-ttu-id="b1ee7-201">Carregando opções de middleware injeção direto</span><span class="sxs-lookup"><span data-stu-id="b1ee7-201">Loading middleware options through direct injection</span></span>

<span data-ttu-id="b1ee7-202">O padrão de opções tem a vantagem de que ele cria flexível acoplamento entre valores de opções e seus consumidores.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-202">The options pattern has the advantage that it creates loose coupling between options values and their consumers.</span></span> <span data-ttu-id="b1ee7-203">Depois que você associou a uma classe de opções com os valores reais de opções, qualquer outra classe pode obter acesso às opções por meio da estrutura de injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-203">Once you've associated an options class with the actual options values, any other class can get access to the options through the dependency injection framework.</span></span> <span data-ttu-id="b1ee7-204">Não é necessário para passar em torno de valores de opções.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-204">There's no need to pass around options values.</span></span>

<span data-ttu-id="b1ee7-205">Isso divide Embora se você quiser usar o mesmo middleware duas vezes, com opções diferentes.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-205">This breaks down though if you want to use the same middleware twice, with different options.</span></span> <span data-ttu-id="b1ee7-206">Por exemplo um autorização middleware usado em diferentes ramificações, permitindo que diferentes funções.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-206">For example an authorization middleware used in different branches allowing different roles.</span></span> <span data-ttu-id="b1ee7-207">Você não pode associar dois objetos diferentes opções com a classe de opções de um.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-207">You can't associate two different options objects with the one options class.</span></span>

<span data-ttu-id="b1ee7-208">A solução é obter os objetos de opções com os valores de opções reais no seu `Startup` classe e passe-os diretamente a cada instância de seu middleware.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-208">The solution is to get the options objects with the actual options values in your `Startup` class and pass those directly to each instance of your middleware.</span></span>

1. <span data-ttu-id="b1ee7-209">Adicionar uma segunda chave para *appSettings. JSON*</span><span class="sxs-lookup"><span data-stu-id="b1ee7-209">Add a second key to *appsettings.json*</span></span>

   <span data-ttu-id="b1ee7-210">Para adicionar um segundo conjunto de opções para o *appSettings. JSON* de arquivo, use uma nova chave para identificá-lo exclusivamente:</span><span class="sxs-lookup"><span data-stu-id="b1ee7-210">To add a second set of options to the *appsettings.json* file, use a new key to uniquely identify it:</span></span>

   [!code-json[](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,10-18&highlight=2-5)]

2. <span data-ttu-id="b1ee7-211">Recuperar valores de opções e os transmite para o middleware.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-211">Retrieve options values and pass them to middleware.</span></span> <span data-ttu-id="b1ee7-212">O `Use...` método de extensão (o que adiciona o middleware no pipeline) é um local lógico para transmitir os valores de opção:</span><span class="sxs-lookup"><span data-stu-id="b1ee7-212">The `Use...` extension method (which adds your middleware to the pipeline) is a logical place to pass in the option values:</span></span> 

   [!code-csharp[](http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=20-23)]

3. <span data-ttu-id="b1ee7-213">Habilite middleware para utilizar um parâmetro de opções.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-213">Enable middleware to take an options parameter.</span></span> <span data-ttu-id="b1ee7-214">Forneça uma sobrecarga do `Use...` método de extensão (que usa o parâmetro options e passá-lo para `UseMiddleware`).</span><span class="sxs-lookup"><span data-stu-id="b1ee7-214">Provide an overload of the `Use...` extension method (that takes the options parameter and passes it to `UseMiddleware`).</span></span> <span data-ttu-id="b1ee7-215">Quando `UseMiddleware` é chamado com parâmetros, ele passa os parâmetros para o construtor de middleware quando ele cria uma instância do objeto de middleware.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-215">When `UseMiddleware` is called with parameters, it passes the parameters to your middleware constructor when it instantiates the middleware object.</span></span>

   [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Extensions&highlight=9-14)]

   <span data-ttu-id="b1ee7-216">Observe como isso encapsula o objeto de opções em um `OptionsWrapper` objeto.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-216">Note how this wraps the options object in an `OptionsWrapper` object.</span></span> <span data-ttu-id="b1ee7-217">Isso implementa `IOptions`, conforme o esperado pelo construtor de middleware.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-217">This implements `IOptions`, as expected by the middleware constructor.</span></span>

## <a name="migrating-to-the-new-httpcontext"></a><span data-ttu-id="b1ee7-218">Migrando para o novo HttpContext</span><span class="sxs-lookup"><span data-stu-id="b1ee7-218">Migrating to the new HttpContext</span></span>

<span data-ttu-id="b1ee7-219">Anteriormente, você viu que o `Invoke` método no seu middleware usa um parâmetro de tipo `HttpContext`:</span><span class="sxs-lookup"><span data-stu-id="b1ee7-219">You saw earlier that the `Invoke` method in your middleware takes a parameter of type `HttpContext`:</span></span>

```csharp
public async Task Invoke(HttpContext context)
```

<span data-ttu-id="b1ee7-220">`HttpContext` foi alterado significativamente no núcleo do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-220">`HttpContext` has significantly changed in ASP.NET Core.</span></span> <span data-ttu-id="b1ee7-221">Esta seção mostra como converter as propriedades mais usadas de [System.Web.HttpContext](/dotnet/api/system.web.httpcontext) para o novo `Microsoft.AspNetCore.Http.HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-221">This section shows how to translate the most commonly used properties of [System.Web.HttpContext](/dotnet/api/system.web.httpcontext) to the new `Microsoft.AspNetCore.Http.HttpContext`.</span></span>

### <a name="httpcontext"></a><span data-ttu-id="b1ee7-222">HttpContext</span><span class="sxs-lookup"><span data-stu-id="b1ee7-222">HttpContext</span></span>

<span data-ttu-id="b1ee7-223">**HttpContext** se traduz em:</span><span class="sxs-lookup"><span data-stu-id="b1ee7-223">**HttpContext.Items** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Items)]

<span data-ttu-id="b1ee7-224">**ID da solicitação exclusiva (nenhum equivalente System.Web.HttpContext)**</span><span class="sxs-lookup"><span data-stu-id="b1ee7-224">**Unique request ID (no System.Web.HttpContext counterpart)**</span></span>

<span data-ttu-id="b1ee7-225">Fornece uma id exclusiva para cada solicitação.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-225">Gives you a unique id for each request.</span></span> <span data-ttu-id="b1ee7-226">Muito útil para incluir nos logs.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-226">Very useful to include in your logs.</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Trace)]

### <a name="httpcontextrequest"></a><span data-ttu-id="b1ee7-227">HttpContext.Request</span><span class="sxs-lookup"><span data-stu-id="b1ee7-227">HttpContext.Request</span></span>

<span data-ttu-id="b1ee7-228">**HttpContext.Request.HttpMethod** se traduz em:</span><span class="sxs-lookup"><span data-stu-id="b1ee7-228">**HttpContext.Request.HttpMethod** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Method)]

<span data-ttu-id="b1ee7-229">**HttpContext.Request.QueryString** se traduz em:</span><span class="sxs-lookup"><span data-stu-id="b1ee7-229">**HttpContext.Request.QueryString** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Query)]

<span data-ttu-id="b1ee7-230">**HttpContext.Request.Url** e **HttpContext.Request.RawUrl** traduzir para:</span><span class="sxs-lookup"><span data-stu-id="b1ee7-230">**HttpContext.Request.Url** and **HttpContext.Request.RawUrl** translate to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Url)]

<span data-ttu-id="b1ee7-231">**HttpContext.Request.IsSecureConnection** se traduz em:</span><span class="sxs-lookup"><span data-stu-id="b1ee7-231">**HttpContext.Request.IsSecureConnection** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Secure)]

<span data-ttu-id="b1ee7-232">**HttpContext.Request.UserHostAddress** se traduz em:</span><span class="sxs-lookup"><span data-stu-id="b1ee7-232">**HttpContext.Request.UserHostAddress** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Host)]

<span data-ttu-id="b1ee7-233">**HttpContext.Request.Cookies** se traduz em:</span><span class="sxs-lookup"><span data-stu-id="b1ee7-233">**HttpContext.Request.Cookies** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Cookies)]

<span data-ttu-id="b1ee7-234">**HttpContext.Request.RequestContext.RouteData** se traduz em:</span><span class="sxs-lookup"><span data-stu-id="b1ee7-234">**HttpContext.Request.RequestContext.RouteData** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Route)]

<span data-ttu-id="b1ee7-235">**HttpContext.Request.Headers** se traduz em:</span><span class="sxs-lookup"><span data-stu-id="b1ee7-235">**HttpContext.Request.Headers** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Headers)]

<span data-ttu-id="b1ee7-236">**HttpContext.Request.UserAgent** se traduz em:</span><span class="sxs-lookup"><span data-stu-id="b1ee7-236">**HttpContext.Request.UserAgent** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Agent)]

<span data-ttu-id="b1ee7-237">**HttpContext.Request.UrlReferrer** se traduz em:</span><span class="sxs-lookup"><span data-stu-id="b1ee7-237">**HttpContext.Request.UrlReferrer** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Referrer)]

<span data-ttu-id="b1ee7-238">**HttpContext.Request.ContentType** se traduz em:</span><span class="sxs-lookup"><span data-stu-id="b1ee7-238">**HttpContext.Request.ContentType** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Type)]

<span data-ttu-id="b1ee7-239">**HttpContext.Request.Form** se traduz em:</span><span class="sxs-lookup"><span data-stu-id="b1ee7-239">**HttpContext.Request.Form** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Form)]

> [!WARNING]
> <span data-ttu-id="b1ee7-240">Valores de formulário de leitura somente se o tipo de conteúdo sub é *x-www-form-urlencoded* ou *dados do formulário*.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-240">Read form values only if the content sub type is *x-www-form-urlencoded* or *form-data*.</span></span>

<span data-ttu-id="b1ee7-241">**HttpContext.Request.InputStream** se traduz em:</span><span class="sxs-lookup"><span data-stu-id="b1ee7-241">**HttpContext.Request.InputStream** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Input)]

> [!WARNING]
> <span data-ttu-id="b1ee7-242">Use esse código somente em um middleware de tipo de manipulador, no final de um pipeline.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-242">Use this code only in a handler type middleware, at the end of a pipeline.</span></span>
>
><span data-ttu-id="b1ee7-243">Você pode ler o corpo bruto conforme mostrado acima apenas uma vez por solicitação.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-243">You can read the raw body as shown above only once per request.</span></span> <span data-ttu-id="b1ee7-244">Middleware tentar ler o corpo após a primeira leitura lê um corpo vazio.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-244">Middleware trying to read the body after the first read will read an empty body.</span></span>
>
><span data-ttu-id="b1ee7-245">Isso não se aplica a leitura de um formulário como mostrado anteriormente, porque isso é feito de um buffer.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-245">This doesn't apply to reading a form as shown earlier, because that's done from a buffer.</span></span>

### <a name="httpcontextresponse"></a><span data-ttu-id="b1ee7-246">HttpContext.Response</span><span class="sxs-lookup"><span data-stu-id="b1ee7-246">HttpContext.Response</span></span>

<span data-ttu-id="b1ee7-247">**HttpContext.Response.Status** e **HttpContext.Response.StatusDescription** traduzir para:</span><span class="sxs-lookup"><span data-stu-id="b1ee7-247">**HttpContext.Response.Status** and **HttpContext.Response.StatusDescription** translate to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Status)]

<span data-ttu-id="b1ee7-248">**HttpContext.Response.ContentEncoding** e **HttpContext.Response.ContentType** traduzir para:</span><span class="sxs-lookup"><span data-stu-id="b1ee7-248">**HttpContext.Response.ContentEncoding** and **HttpContext.Response.ContentType** translate to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespType)]

<span data-ttu-id="b1ee7-249">**HttpContext.Response.ContentType** em seu próprio também se traduz em:</span><span class="sxs-lookup"><span data-stu-id="b1ee7-249">**HttpContext.Response.ContentType** on its own also translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespTypeOnly)]

<span data-ttu-id="b1ee7-250">**HttpContext.Response.Output** se traduz em:</span><span class="sxs-lookup"><span data-stu-id="b1ee7-250">**HttpContext.Response.Output** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Output)]

<span data-ttu-id="b1ee7-251">**HttpContext.Response.TransmitFile**</span><span class="sxs-lookup"><span data-stu-id="b1ee7-251">**HttpContext.Response.TransmitFile**</span></span>

<span data-ttu-id="b1ee7-252">Serviços de um arquivo é discutida [aqui](../fundamentals/request-features.md#middleware-and-request-features).</span><span class="sxs-lookup"><span data-stu-id="b1ee7-252">Serving up a file is discussed [here](../fundamentals/request-features.md#middleware-and-request-features).</span></span>

<span data-ttu-id="b1ee7-253">**HttpContext.Response.Headers**</span><span class="sxs-lookup"><span data-stu-id="b1ee7-253">**HttpContext.Response.Headers**</span></span>

<span data-ttu-id="b1ee7-254">Enviar cabeçalhos de resposta é complicada pelo fato de que se você defini-las depois que nada foi gravado no corpo da resposta, ele não serão enviados.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-254">Sending response headers is complicated by the fact that if you set them after anything has been written to the response body, they will not be sent.</span></span>

<span data-ttu-id="b1ee7-255">A solução é definir um método de retorno de chamada que será chamado direita antes da gravação do início da resposta.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-255">The solution is to set a callback method that will be called right before writing to the response starts.</span></span> <span data-ttu-id="b1ee7-256">Isso é feito melhor no início do `Invoke` método no seu middleware.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-256">This is best done at the start of the `Invoke` method in your middleware.</span></span> <span data-ttu-id="b1ee7-257">É desse método de retorno de chamada que define os cabeçalhos de resposta.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-257">It's this callback method that sets your response headers.</span></span>

<span data-ttu-id="b1ee7-258">O código a seguir define um método de retorno de chamada chamado `SetHeaders`:</span><span class="sxs-lookup"><span data-stu-id="b1ee7-258">The following code sets a callback method called `SetHeaders`:</span></span>

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

<span data-ttu-id="b1ee7-259">O `SetHeaders` método de retorno de chamada terá esta aparência:</span><span class="sxs-lookup"><span data-stu-id="b1ee7-259">The `SetHeaders` callback method would look like this:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetHeaders)]

<span data-ttu-id="b1ee7-260">**HttpContext.Response.Cookies**</span><span class="sxs-lookup"><span data-stu-id="b1ee7-260">**HttpContext.Response.Cookies**</span></span>

<span data-ttu-id="b1ee7-261">Cookies de viagem para o navegador em um *Set-Cookie* cabeçalho de resposta.</span><span class="sxs-lookup"><span data-stu-id="b1ee7-261">Cookies travel to the browser in a *Set-Cookie* response header.</span></span> <span data-ttu-id="b1ee7-262">Como resultado, envio de cookies requer o retorno de chamada mesmo usada para enviar cabeçalhos de resposta:</span><span class="sxs-lookup"><span data-stu-id="b1ee7-262">As a result, sending cookies requires the same callback as used for sending response headers:</span></span>

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetCookies, state: httpContext);
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

<span data-ttu-id="b1ee7-263">O `SetCookies` método de retorno de chamada deve ser semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="b1ee7-263">The `SetCookies` callback method would look like the following:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetCookies)]

## <a name="additional-resources"></a><span data-ttu-id="b1ee7-264">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="b1ee7-264">Additional resources</span></span>

* [<span data-ttu-id="b1ee7-265">Visão geral de módulos HTTP e de manipuladores HTTP</span><span class="sxs-lookup"><span data-stu-id="b1ee7-265">HTTP Handlers and HTTP Modules Overview</span></span>](/iis/configuration/system.webserver/)
* [<span data-ttu-id="b1ee7-266">Configuração</span><span class="sxs-lookup"><span data-stu-id="b1ee7-266">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="b1ee7-267">Inicialização de aplicativos</span><span class="sxs-lookup"><span data-stu-id="b1ee7-267">Application Startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="b1ee7-268">Middleware</span><span class="sxs-lookup"><span data-stu-id="b1ee7-268">Middleware</span></span>](xref:fundamentals/middleware/index)
