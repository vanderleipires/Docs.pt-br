---
uid: signalr/overview/deployment/using-signalr-with-azure-web-sites
title: "Usando o SignalR com aplicativos Web no serviço de aplicativo do Azure | Microsoft Docs"
author: pfletcher
description: "Este documento descreve como configurar um aplicativo de SignalR é executado no Microsoft Azure. Versões de software usado no tutorial do Visual Studio 2013 ou Vis.."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/01/2015
ms.topic: article
ms.assetid: 2a7517a0-b88c-4162-ade3-9bf6ca7062fd
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/deployment/using-signalr-with-azure-web-sites
msc.type: authoredcontent
ms.openlocfilehash: 8386441690a3fb479ffb941ebd7c0b2f83870781
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="using-signalr-with-web-apps-in-azure-app-service"></a><span data-ttu-id="ff1e0-104">Usando o SignalR com aplicativos Web no serviço de aplicativo do Azure</span><span class="sxs-lookup"><span data-stu-id="ff1e0-104">Using SignalR with Web Apps in Azure App Service</span></span>
====================
<span data-ttu-id="ff1e0-105">por [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="ff1e0-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="ff1e0-106">Este documento descreve como configurar um aplicativo de SignalR é executado no Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="ff1e0-106">This document describes how to configure a SignalR application that runs on Microsoft Azure.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="ff1e0-107">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="ff1e0-107">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="ff1e0-108">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) ou o Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="ff1e0-108">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) or Visual Studio 2012</span></span>
> - <span data-ttu-id="ff1e0-109">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="ff1e0-109">.NET 4.5</span></span>
> - <span data-ttu-id="ff1e0-110">SignalR versão 2</span><span class="sxs-lookup"><span data-stu-id="ff1e0-110">SignalR version 2</span></span>
> - <span data-ttu-id="ff1e0-111">SDK 2.3 do Azure para Visual Studio 2013 ou 2012</span><span class="sxs-lookup"><span data-stu-id="ff1e0-111">Azure SDK 2.3 for Visual Studio 2013 or 2012</span></span>
>   
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="ff1e0-112">Perguntas e comentários</span><span class="sxs-lookup"><span data-stu-id="ff1e0-112">Questions and comments</span></span>
> 
> <span data-ttu-id="ff1e0-113">Deixe comentários em como você gostou neste tutorial e o que podemos melhorar nos comentários na parte inferior da página.</span><span class="sxs-lookup"><span data-stu-id="ff1e0-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="ff1e0-114">Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, você poderá postá-los para o [ASP.NET SignalR fórum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/), ou o [fóruns do Microsoft Azure](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).</span><span class="sxs-lookup"><span data-stu-id="ff1e0-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/), or the [Microsoft Azure forums](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).</span></span>


## <a name="table-of-contents"></a><span data-ttu-id="ff1e0-115">Sumário</span><span class="sxs-lookup"><span data-stu-id="ff1e0-115">Table of Contents</span></span>

- [<span data-ttu-id="ff1e0-116">Introdução</span><span class="sxs-lookup"><span data-stu-id="ff1e0-116">Introduction</span></span>](#introduction)
- [<span data-ttu-id="ff1e0-117">Implantando um aplicativo Web de SignalR ao serviço de aplicativo do Azure</span><span class="sxs-lookup"><span data-stu-id="ff1e0-117">Deploying a SignalR Web App to Azure App Service</span></span>](#deploying)
- [<span data-ttu-id="ff1e0-118">Habilitar WebSockets no serviço de aplicativo do Azure</span><span class="sxs-lookup"><span data-stu-id="ff1e0-118">Enabling WebSockets on Azure App Service</span></span>](#websocket)
- [<span data-ttu-id="ff1e0-119">Usando o Backplane de Cache Redis do Azure</span><span class="sxs-lookup"><span data-stu-id="ff1e0-119">Using the Azure Redis Cache Backplane</span></span>](#backplane)
- [<span data-ttu-id="ff1e0-120">Próximas Etapas</span><span class="sxs-lookup"><span data-stu-id="ff1e0-120">Next Steps</span></span>](#nextsteps)

<a id="introduction"></a>
## <a name="introduction"></a><span data-ttu-id="ff1e0-121">Introdução</span><span class="sxs-lookup"><span data-stu-id="ff1e0-121">Introduction</span></span>

<span data-ttu-id="ff1e0-122">O SignalR do ASP.NET pode ser usado para colocar um novo nível de interatividade entre servidores e web ou de clientes .NET.</span><span class="sxs-lookup"><span data-stu-id="ff1e0-122">ASP.NET SignalR can be used to bring a new level of interactivity between servers and web or .NET clients.</span></span> <span data-ttu-id="ff1e0-123">Quando hospedado no Azure, os aplicativos SignalR podem aproveitar altamente disponível, escalonável e ambiente de alto desempenho que em execução na nuvem fornece.</span><span class="sxs-lookup"><span data-stu-id="ff1e0-123">When hosted in Azure, SignalR applications can take advantage of the highly available, scalable, and performant environment that running in the cloud provides.</span></span>

<a id="deploying"></a>
## <a name="deploying-a-signalr-web-app-to-azure-app-service"></a><span data-ttu-id="ff1e0-124">Implantando um aplicativo Web de SignalR ao serviço de aplicativo do Azure</span><span class="sxs-lookup"><span data-stu-id="ff1e0-124">Deploying a SignalR Web App to Azure App Service</span></span>

<span data-ttu-id="ff1e0-125">SignalR não adiciona qualquer complicações específicas para implantar um aplicativo do Azure versus implantando em um servidor local.</span><span class="sxs-lookup"><span data-stu-id="ff1e0-125">SignalR doesn't add any particular complications to deploying an application to Azure versus deploying to an on-premises server.</span></span> <span data-ttu-id="ff1e0-126">Um aplicativo que usa o SignalR pode ser hospedado no Azure sem alterações na configuração ou outras configurações (embora para suporte a WebSockets, consulte [habilitar WebSockets no serviço de aplicativo do Azure](#websocket) abaixo.) Para este tutorial, você implantará o aplicativo criado no [Tutorial de Introdução](../getting-started/tutorial-getting-started-with-signalr.md) no Azure.</span><span class="sxs-lookup"><span data-stu-id="ff1e0-126">An application that uses SignalR can be hosted in Azure without any changes in configuration or other settings (though for WebSockets support, see [Enabling WebSockets on Azure App Service](#websocket) below.) For this tutorial, you'll deploy the application created in the [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md) to Azure.</span></span>

<span data-ttu-id="ff1e0-127">**Pré-requisitos**</span><span class="sxs-lookup"><span data-stu-id="ff1e0-127">**Prerequisites**</span></span>

- <span data-ttu-id="ff1e0-128">Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="ff1e0-128">Visual Studio 2013.</span></span> <span data-ttu-id="ff1e0-129">Se você não tiver o Visual Studio, o Visual Studio 2013 Express para Web está incluído na instalação do SDK do Azure.</span><span class="sxs-lookup"><span data-stu-id="ff1e0-129">If you don't have Visual Studio, Visual Studio 2013 Express for Web is included in the install of the Azure SDK.</span></span>
- <span data-ttu-id="ff1e0-130">[SDK 2.3 do Azure para Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) ou [SDK 2.3 do Azure para Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).</span><span class="sxs-lookup"><span data-stu-id="ff1e0-130">[Azure SDK 2.3 for Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) or [Azure SDK 2.3 for Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).</span></span>
- <span data-ttu-id="ff1e0-131">Para concluir este tutorial, você precisará de uma assinatura do Azure.</span><span class="sxs-lookup"><span data-stu-id="ff1e0-131">To complete this tutorial, you will need an Azure subscription.</span></span> <span data-ttu-id="ff1e0-132">Você pode [ativar os benefícios de assinante do MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), ou [inscrever-se para uma assinatura de avaliação](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ff1e0-132">You can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), or [sign up for a trial subscription](https://azure.microsoft.com/pricing/free-trial/).</span></span>

### <a name="deploying-a-signalr-web-app-to-azure"></a><span data-ttu-id="ff1e0-133">Implantar um aplicativo web do SignalR no Azure</span><span class="sxs-lookup"><span data-stu-id="ff1e0-133">Deploying a SignalR web app to Azure</span></span>

1. <span data-ttu-id="ff1e0-134">Concluir o [Tutorial de Introdução](../getting-started/tutorial-getting-started-with-signalr.md), ou baixe o projeto concluído de [Galeria de códigos](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span><span class="sxs-lookup"><span data-stu-id="ff1e0-134">Complete the [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md), or download the finished project from [Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span></span>
2. <span data-ttu-id="ff1e0-135">No Visual Studio, selecione **criar**, **publicar SignalR bate-papo**.</span><span class="sxs-lookup"><span data-stu-id="ff1e0-135">In Visual Studio, select **Build**, **Publish SignalR Chat**.</span></span>
3. <span data-ttu-id="ff1e0-136">Na caixa de diálogo "Publicar na Web", selecione "Windows Azure Web Sites".</span><span class="sxs-lookup"><span data-stu-id="ff1e0-136">In the "Publish Web" dialog, select "Windows Azure Web Sites".</span></span>

    ![Selecione os Sites do Azure](using-signalr-with-azure-web-sites/_static/image1.png)
4. <span data-ttu-id="ff1e0-138">Se você não tiver entrado sua conta da Microsoft, clique em **entrar...**  na caixa de diálogo "Selecionar Site existente" e de entrada.</span><span class="sxs-lookup"><span data-stu-id="ff1e0-138">If you aren't signed in to your Microsoft account, click **Sign In...** in the "Select Existing Web Site" dialog, and sign in.</span></span>

    ![Selecione o Site da Web existente](using-signalr-with-azure-web-sites/_static/image2.png)    ![Entrar no Azure](using-signalr-with-azure-web-sites/_static/image3.png)
5. <span data-ttu-id="ff1e0-141">Na caixa de diálogo "Selecionar Site existente", clique em **novo**.</span><span class="sxs-lookup"><span data-stu-id="ff1e0-141">In the "Select Existing Web Site" dialog, click **New**.</span></span>

    ![Novo Site](using-signalr-with-azure-web-sites/_static/image4.png)
6. <span data-ttu-id="ff1e0-143">Na caixa de diálogo "Criar o site no Windows Azure", digite um nome exclusivo do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ff1e0-143">In the "Create site on Windows Azure" dialog, enter a unique app name.</span></span> <span data-ttu-id="ff1e0-144">Selecione a região mais próxima de você no menu suspenso de região.</span><span class="sxs-lookup"><span data-stu-id="ff1e0-144">Select the region closest to you in the Region dropdown.</span></span> <span data-ttu-id="ff1e0-145">Clique em **Criar**.</span><span class="sxs-lookup"><span data-stu-id="ff1e0-145">Click **Create**.</span></span>

    ![Criar o site no Azure](using-signalr-with-azure-web-sites/_static/image5.png)
7. <span data-ttu-id="ff1e0-147">Na caixa de diálogo "Publicar na Web", clique em **publicar**.</span><span class="sxs-lookup"><span data-stu-id="ff1e0-147">In the "Publish Web" dialog, click **Publish**.</span></span>

    ![publicar o site](using-signalr-with-azure-web-sites/_static/image6.png)
8. <span data-ttu-id="ff1e0-149">Quando o aplicativo for concluída publicação, o aplicativo de SignalR Chat hospedado em aplicativos de Web do serviço de aplicativo do Azure será aberto em um navegador.</span><span class="sxs-lookup"><span data-stu-id="ff1e0-149">When the app has completed publishing, the SignalR Chat application hosted in Azure App Service Web Apps will open in a browser.</span></span>

    ![Abrir em um navegador de site](using-signalr-with-azure-web-sites/_static/image7.png)

<a id="websocket"></a>
### <a name="enabling-websockets-on-azure-app-service-web-apps"></a><span data-ttu-id="ff1e0-151">Habilitar WebSockets em aplicativos Web do serviço de aplicativo do Azure</span><span class="sxs-lookup"><span data-stu-id="ff1e0-151">Enabling WebSockets on Azure App Service Web Apps</span></span>

<span data-ttu-id="ff1e0-152">O WebSocket precisa ser habilitado explicitamente em seu aplicativo web a ser usado em um aplicativo do SignalR; Caso contrário, serão usados outros protocolos (consulte [transportes e Fallbacks](../getting-started/introduction-to-signalr.md#transports) para obter detalhes).</span><span class="sxs-lookup"><span data-stu-id="ff1e0-152">WebSockets needs to be explicitly enabled in your web app to be used in a SignalR application; otherwise, other protocols will be used (See [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports) for details).</span></span>

<span data-ttu-id="ff1e0-153">Para usar o WebSocket em aplicativos de Web do serviço de aplicativo do Azure, habilitá-la na seção de configuração do aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="ff1e0-153">In order to use WebSockets on Azure App Service Web Apps, enable it in the configuration section of the web app.</span></span> <span data-ttu-id="ff1e0-154">Para fazer isso, abra o seu aplicativo web a [Portal de gerenciamento](https://manage.windowsazure.com/)e selecione Configurar.</span><span class="sxs-lookup"><span data-stu-id="ff1e0-154">To do this, open your web app in the [Azure Management Portal](https://manage.windowsazure.com/), and select Configure.</span></span>

![Guia Configurar](using-signalr-with-azure-web-sites/_static/image8.png)

<span data-ttu-id="ff1e0-156">Na parte superior da página de configuração, certifique-se de que o .NET 4.5 é usado para seu aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="ff1e0-156">At the top of the configuration page, ensure that .NET 4.5 is used for your web app.</span></span>

![Configuração do .NET framework versão 4.5](using-signalr-with-azure-web-sites/_static/image9.png)

<span data-ttu-id="ff1e0-158">Na página de configuração, no **WebSockets** configuração, selecione **em**.</span><span class="sxs-lookup"><span data-stu-id="ff1e0-158">On the configuration page, in the **WebSockets** setting, select **On**.</span></span>

![Configuração de WebSockets: no](using-signalr-with-azure-web-sites/_static/image10.png)

<span data-ttu-id="ff1e0-160">Na parte inferior da página de configuração, selecione **salvar** para salvar suas alterações.</span><span class="sxs-lookup"><span data-stu-id="ff1e0-160">At the bottom of the Configuration page, select **Save** to save your changes.</span></span>

![Salvar configurações](using-signalr-with-azure-web-sites/_static/image11.png)

<a id="backplane"></a>
## <a name="using-the-azure-redis-cache-backplane"></a><span data-ttu-id="ff1e0-162">Usando o Backplane de Cache Redis do Azure</span><span class="sxs-lookup"><span data-stu-id="ff1e0-162">Using the Azure Redis Cache Backplane</span></span>

<span data-ttu-id="ff1e0-163">Se você usar várias instâncias do seu aplicativo web, e os usuários dessas instâncias precisam interagir com um outro (de forma que, por exemplo, mensagens de chat criadas em uma instância podem alcançar os usuários conectados a outras instâncias), o [Cache Redis do Azure backplane](../performance/scaleout-with-redis.md) devem ser implementados em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ff1e0-163">If you use multiple instances for your web app, and the users of those instances need to interact with one another (so that, for instance, chat messages created in one instance can reach the users connected to other instances), the [Azure Redis Cache backplane](../performance/scaleout-with-redis.md) must be implemented in your application.</span></span>

<a id="nextsteps"></a>
## <a name="next-steps"></a><span data-ttu-id="ff1e0-164">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="ff1e0-164">Next Steps</span></span>

<span data-ttu-id="ff1e0-165">Para obter mais informações sobre aplicativos Web no serviço de aplicativo do Azure, consulte [visão geral de aplicativos Web](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).</span><span class="sxs-lookup"><span data-stu-id="ff1e0-165">For more information on Web Apps in Azure App Service, see [Web Apps overview](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).</span></span>
