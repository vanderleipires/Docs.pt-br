---
title: Módulo do ASP.NET Core
author: rick-anderson
description: Saiba como o módulo do ASP.NET Core permite que o servidor Web Kestrel use o IIS ou o IIS Express como um servidor proxy reverso.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/23/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: 789b0b2e74405256bb0e247ae5ea9697e3cb28e5
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/14/2018
ms.locfileid: "34153699"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="db89f-103">Módulo do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="db89f-103">ASP.NET Core Module</span></span>

<span data-ttu-id="db89f-104">Por [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl) e [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="db89f-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), and [Chris Ross](https://github.com/Tratcher)</span></span> 

<span data-ttu-id="db89f-105">O Módulo do ASP.NET Core permite que os aplicativos ASP.NET Core sejam executados por trás do IIS em uma configuração de proxy reverso.</span><span class="sxs-lookup"><span data-stu-id="db89f-105">The ASP.NET Core Module allows ASP.NET Core apps to run behind IIS in a reverse proxy configuration.</span></span> <span data-ttu-id="db89f-106">O IIS fornece recursos avançados de segurança e capacidade de gerenciamento de aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="db89f-106">IIS provides advanced web app security and manageability features.</span></span>

<span data-ttu-id="db89f-107">Versões do Windows compatíveis:</span><span class="sxs-lookup"><span data-stu-id="db89f-107">Supported Windows versions:</span></span>

* <span data-ttu-id="db89f-108">Windows 7 ou posterior</span><span class="sxs-lookup"><span data-stu-id="db89f-108">Windows 7 or later</span></span>
* <span data-ttu-id="db89f-109">Windows Server 2008 R2 ou posterior</span><span class="sxs-lookup"><span data-stu-id="db89f-109">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="db89f-110">O Módulo do ASP.NET Core só funciona com o Kestrel.</span><span class="sxs-lookup"><span data-stu-id="db89f-110">The ASP.NET Core Module only works with Kestrel.</span></span> <span data-ttu-id="db89f-111">O módulo é incompatível com o [HTTP.sys](xref:fundamentals/servers/httpsys) (chamado anteriormente de [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="db89f-111">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span>

## <a name="aspnet-core-module-description"></a><span data-ttu-id="db89f-112">Descrição de Módulo do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="db89f-112">ASP.NET Core Module description</span></span>

<span data-ttu-id="db89f-113">O Módulo do ASP.NET Core é um módulo nativo do IIS que se conecta ao pipeline do IIS para redirecionar solicitações da Web para aplicativos ASP.NET Core de back-end.</span><span class="sxs-lookup"><span data-stu-id="db89f-113">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to redirect web requests to backend ASP.NET Core apps.</span></span> <span data-ttu-id="db89f-114">Muitos módulos nativos, como a Autenticação do Windows, permanecem ativos.</span><span class="sxs-lookup"><span data-stu-id="db89f-114">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="db89f-115">Para saber mais sobre os módulos ativos do IIS com o módulo, confira [Módulos do IIS](xref:host-and-deploy/iis/modules).</span><span class="sxs-lookup"><span data-stu-id="db89f-115">To learn more about IIS modules active with the module, see [IIS modules](xref:host-and-deploy/iis/modules).</span></span>

<span data-ttu-id="db89f-116">Como os aplicativos ASP.NET Core são executados em um processo separado do processo de trabalho do IIS, o módulo também realiza o gerenciamento de processos.</span><span class="sxs-lookup"><span data-stu-id="db89f-116">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="db89f-117">O módulo inicia o processo para o aplicativo ASP.NET Core quando a primeira solicitação chega e reinicia o aplicativo quando ele falha.</span><span class="sxs-lookup"><span data-stu-id="db89f-117">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="db89f-118">Isso é basicamente o mesmo comportamento que o dos aplicativos ASP.NET 4.x que são executados dentro do processo do IIS e são gerenciados pelo [WAS (Serviço de Ativação de Processos do Windows)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="db89f-118">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="db89f-119">O diagrama a seguir ilustra a relação entre o IIS, o Módulo do ASP.NET Core e os aplicativos ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="db89f-119">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and ASP.NET Core apps:</span></span>

![Módulo do ASP.NET Core](aspnet-core-module/_static/ancm.png)

<span data-ttu-id="db89f-121">As solicitações chegam da Web para o driver do HTTP.sys no modo kernel.</span><span class="sxs-lookup"><span data-stu-id="db89f-121">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="db89f-122">O driver roteia as solicitações ao IIS na porta configurada do site, normalmente, a 80 (HTTP) ou a 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="db89f-122">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="db89f-123">O módulo encaminha as solicitações ao Kestrel em uma porta aleatória para o aplicativo, que não seja a porta 80/443.</span><span class="sxs-lookup"><span data-stu-id="db89f-123">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80/443.</span></span>

<span data-ttu-id="db89f-124">O módulo especifica a porta por meio de uma variável de ambiente na inicialização e o middleware de integração do IIS configura o servidor para escutar em `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="db89f-124">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="db89f-125">Outras verificações são executadas e as solicitações que não se originam do módulo são rejeitadas.</span><span class="sxs-lookup"><span data-stu-id="db89f-125">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="db89f-126">O módulo não é compatível com encaminhamento de HTTPS, portanto, as solicitações são encaminhadas por HTTP, mesmo se recebidas pelo IIS por HTTPS.</span><span class="sxs-lookup"><span data-stu-id="db89f-126">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="db89f-127">Depois que o Kestrel coleta uma solicitação do módulo, a solicitação é enviada por push ao pipeline do middleware do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="db89f-127">After Kestrel picks up a request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="db89f-128">O pipeline do middleware manipula a solicitação e a passa como uma instância de `HttpContext` para a lógica do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="db89f-128">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="db89f-129">A resposta do aplicativo é retornada ao IIS, que a retorna por push para o cliente HTTP que iniciou a solicitação.</span><span class="sxs-lookup"><span data-stu-id="db89f-129">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="db89f-130">O Módulo do ASP.NET Core tem algumas outras funções.</span><span class="sxs-lookup"><span data-stu-id="db89f-130">The ASP.NET Core Module has a few other functions.</span></span> <span data-ttu-id="db89f-131">O módulo pode:</span><span class="sxs-lookup"><span data-stu-id="db89f-131">The module can:</span></span>

* <span data-ttu-id="db89f-132">Definir variáveis de ambiente para o processo de trabalho.</span><span class="sxs-lookup"><span data-stu-id="db89f-132">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="db89f-133">Registrar a saída StdOut no armazenamento de arquivo para a solução de problemas de inicialização.</span><span class="sxs-lookup"><span data-stu-id="db89f-133">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="db89f-134">Encaminhar tokens de autenticação do Windows.</span><span class="sxs-lookup"><span data-stu-id="db89f-134">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="db89f-135">Como instalar e usar o Módulo do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="db89f-135">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="db89f-136">Para obter instruções detalhadas de como instalar e usar o Módulo do ASP.NET Core, confira [Hospedar no Windows com o IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="db89f-136">For detailed instructions on how to install and use the ASP.NET Core Module, see [Host on Windows with IIS](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="db89f-137">Para obter informações de como configurar o módulo, confira a [Referência de configuração do Módulo do ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="db89f-137">For information on configuring the module, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="db89f-138">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="db89f-138">Additional resources</span></span>

* [<span data-ttu-id="db89f-139">Hospedar no Windows com o IIS</span><span class="sxs-lookup"><span data-stu-id="db89f-139">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="db89f-140">Referência de configuração do Módulo do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="db89f-140">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="db89f-141">Repositório do GitHub do Módulo do ASP.NET Core (código-fonte)</span><span class="sxs-lookup"><span data-stu-id="db89f-141">ASP.NET Core Module GitHub repository (source code)</span></span>](https://github.com/aspnet/AspNetCoreModule)
