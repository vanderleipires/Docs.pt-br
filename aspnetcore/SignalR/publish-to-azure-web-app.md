---
title: Publicar um núcleo de ASP.NET SignalR aplicativo para o aplicativo Web do Azure
author: rachelappel
description: Publicar um núcleo de ASP.NET SignalR aplicativo para o aplicativo Web do Azure
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 04/20/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: fd7d38ad47d9004db2ae7b5858dc22609943f601
ms.sourcegitcommit: 2ab550f8c46e1a8a5d45e58be44d151c676af256
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2018
---
# <a name="publish-an-aspnet-core-signalr-app-to-an-azure-web-app"></a><span data-ttu-id="c9655-103">Publicar um núcleo de ASP.NET SignalR aplicativo para um aplicativo Web do Azure</span><span class="sxs-lookup"><span data-stu-id="c9655-103">Publish an ASP.NET Core SignalR app to an Azure Web App</span></span>

<span data-ttu-id="c9655-104">[Aplicativo Web do Azure](/azure/app-service/app-service-web-overview) é um [a computação em nuvem Microsoft](https://azure.microsoft.com/) serviço de plataforma para hospedar aplicativos web, incluindo o ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c9655-104">[Azure Web App](/azure/app-service/app-service-web-overview) is a [Microsoft cloud computing](https://azure.microsoft.com/) platform service for hosting web apps, including ASP.NET Core.</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="c9655-105">Publique o aplicativo</span><span class="sxs-lookup"><span data-stu-id="c9655-105">Publish the app</span></span>

<span data-ttu-id="c9655-106">Visual Studio fornece ferramentas internas para a publicação para um aplicativo Web do Azure.</span><span class="sxs-lookup"><span data-stu-id="c9655-106">Visual Studio provides built-in tools for publishing to an Azure Web App.</span></span> <span data-ttu-id="c9655-107">Usuário do Visual Studio Code pode usar [CLI do Azure](/cli/azure) comandos para publicar aplicativos no Azure.</span><span class="sxs-lookup"><span data-stu-id="c9655-107">Visual Studio Code user can use [Azure CLI](/cli/azure) commands to publish apps to Azure.</span></span> <span data-ttu-id="c9655-108">Este artigo aborda a publicação usando as ferramentas do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c9655-108">This article covers publishing using the tools in Visual Studio.</span></span> <span data-ttu-id="c9655-109">Para publicar um aplicativo usando a CLI do Azure, consulte [publicar um aplicativo do ASP.NET Core para o Azure com ferramentas de linha de comando](xref:tutorials/publish-to-azure-webapp-using-cli).</span><span class="sxs-lookup"><span data-stu-id="c9655-109">To publish an app using Azure CLI, see [Publish an ASP.NET Core app to Azure with command line tools](xref:tutorials/publish-to-azure-webapp-using-cli).</span></span>

<span data-ttu-id="c9655-110">Clique com botão direito no projeto no **Solution Explorer** e selecione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="c9655-110">Right-click on the project in **Solution Explorer** and select **Publish**.</span></span> <span data-ttu-id="c9655-111">Confirme se **criar novo** check-in a **escolher um destino de publicação** caixa de diálogo e selecione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="c9655-111">Confirm that **Create new** is checked in the **Pick a publish target** dialog, and select **Publish**.</span></span>

![Selecionar destino de publicação](publish-to-azure-web-app/_static/pick-publish-target-dialog.png)

<span data-ttu-id="c9655-113">Insira as seguintes informações no **criar serviço de aplicativo** caixa de diálogo e selecione **criar**.</span><span class="sxs-lookup"><span data-stu-id="c9655-113">Enter the following information in the **Create App Service** dialog and select **Create**.</span></span>

| <span data-ttu-id="c9655-114">Item</span><span class="sxs-lookup"><span data-stu-id="c9655-114">Item</span></span> | <span data-ttu-id="c9655-115">Descrição</span><span class="sxs-lookup"><span data-stu-id="c9655-115">Description</span></span> |
| ---- | ----------- |
| <span data-ttu-id="c9655-116">**Nome do aplicativo**</span><span class="sxs-lookup"><span data-stu-id="c9655-116">**App name**</span></span> | <span data-ttu-id="c9655-117">Um nome exclusivo do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c9655-117">A unique name of the app.</span></span> |
| <span data-ttu-id="c9655-118">**Assinatura**</span><span class="sxs-lookup"><span data-stu-id="c9655-118">**Subscription**</span></span> | <span data-ttu-id="c9655-119">A assinatura do Azure que usa o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c9655-119">The Azure subscription that the app uses.</span></span> |
| <span data-ttu-id="c9655-120">**Grupo de recursos**</span><span class="sxs-lookup"><span data-stu-id="c9655-120">**Resource Group**</span></span> | <span data-ttu-id="c9655-121">O grupo de recursos relacionados ao qual pertence o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c9655-121">The group of related resources to which the app belongs.</span></span>  |
| <span data-ttu-id="c9655-122">**Plano de hospedagem**</span><span class="sxs-lookup"><span data-stu-id="c9655-122">**Hosting Plan**</span></span> | <span data-ttu-id="c9655-123">O plano de preço para o aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="c9655-123">The pricing plan for the web app.</span></span> |

![Criar serviço de aplicativo](publish-to-azure-web-app/_static/create-app-service-dialog.png)

<span data-ttu-id="c9655-125">O Visual Studio executará as seguintes tarefas:</span><span class="sxs-lookup"><span data-stu-id="c9655-125">Visual Studio completes the following tasks:</span></span>

* <span data-ttu-id="c9655-126">Cria um perfil de publicação que contém as configurações de publicação.</span><span class="sxs-lookup"><span data-stu-id="c9655-126">Creates a Publish Profile containing publish settings.</span></span>
* <span data-ttu-id="c9655-127">Cria ou usa um existente *aplicativo Web do Azure* com os detalhes fornecidos.</span><span class="sxs-lookup"><span data-stu-id="c9655-127">Creates or uses an existing *Azure Web App* with the provided details.</span></span>
* <span data-ttu-id="c9655-128">Publica o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c9655-128">Publishes the app.</span></span>
* <span data-ttu-id="c9655-129">Inicia um navegador, com o aplicativo web publicado carregado.</span><span class="sxs-lookup"><span data-stu-id="c9655-129">Launches a browser, with the published web app loaded.</span></span>

<span data-ttu-id="c9655-130">Observe o formato da URL para o aplicativo é *.azurewebsites {nome do aplicativo} .net*.</span><span class="sxs-lookup"><span data-stu-id="c9655-130">Notice the format of the URL for the app is *{app name}.azurewebsites.net*.</span></span> <span data-ttu-id="c9655-131">Por exemplo, um aplicativo chamado `SignalRChattR` tem uma URL semelhante a `https://signalrchattr.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="c9655-131">For example, an app named `SignalRChattR` has a URL that looks like `https://signalrchattr.azurewebsites.net`.</span></span>

<span data-ttu-id="c9655-132">Se ocorrer um erro de HTTP 502.2, consulte [versão de visualização de implantar o ASP.NET Core para o serviço de aplicativo do Azure](xref:host-and-deploy/azure-apps/index) resolvê-lo.</span><span class="sxs-lookup"><span data-stu-id="c9655-132">If an HTTP 502.2 error occurs, see [Deploy ASP.NET Core preview release to Azure App Service](xref:host-and-deploy/azure-apps/index) to resolve it.</span></span>

## <a name="configure-signalr-web-app"></a><span data-ttu-id="c9655-133">Configurar o aplicativo web de SignalR</span><span class="sxs-lookup"><span data-stu-id="c9655-133">Configure SignalR web app</span></span>

<span data-ttu-id="c9655-134">ASP.NET SignalR Core aplicativos que são publicados como um aplicativo Web do Azure deve ter [ARR afinidade](https://en.wikipedia.org/wiki/Application_Request_Routing) habilitado.</span><span class="sxs-lookup"><span data-stu-id="c9655-134">ASP.NET Core SignalR apps that are published as an Azure Web App must have [ARR Affinity](https://en.wikipedia.org/wiki/Application_Request_Routing) enabled.</span></span> <span data-ttu-id="c9655-135">[O WebSocket](xref:fundamentals/websockets) deve ser habilitado, para permitir que o transporte de WebSocket à função.</span><span class="sxs-lookup"><span data-stu-id="c9655-135">[WebSockets](xref:fundamentals/websockets) should be enabled, to allow the WebSockets transport to function.</span></span>

<span data-ttu-id="c9655-136">No portal do Azure, navegue até **configurações do aplicativo** para seu aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="c9655-136">In the Azure portal, navigate to **App Settings** for your web app.</span></span> <span data-ttu-id="c9655-137">Definir **WebSockets** para **na**e verifique se **ARR afinidade** é **em**.</span><span class="sxs-lookup"><span data-stu-id="c9655-137">Set **WebSockets** to **On**, and verify **ARR Affinity** is **On**.</span></span>

![Configurações do aplicativo Web do Azure no portal do Azure](publish-to-azure-web-app/_static/azure-web-app-settings.png)

 <span data-ttu-id="c9655-139">O WebSocket e outros transportes [são limitados com base no plano de serviço de aplicativo](/azure/azure-subscription-service-limits#app-service-limits).</span><span class="sxs-lookup"><span data-stu-id="c9655-139">WebSockets and other transports [are limited based on the App Service Plan](/azure/azure-subscription-service-limits#app-service-limits).</span></span>

## <a name="related-resources"></a><span data-ttu-id="c9655-140">Recursos relacionados</span><span class="sxs-lookup"><span data-stu-id="c9655-140">Related resources</span></span>

* [<span data-ttu-id="c9655-141">Publicar um aplicativo do ASP.NET Core para o Azure com ferramentas de linha de comando</span><span class="sxs-lookup"><span data-stu-id="c9655-141">Publish an ASP.NET Core app to Azure with command line tools</span></span>](xref:tutorials/publish-to-azure-webapp-using-cli?tabs=windows)
* [<span data-ttu-id="c9655-142">Publicar um aplicativo do ASP.NET Core para o Azure com o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c9655-142">Publish an ASP.NET Core app to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)
* [<span data-ttu-id="c9655-143">Hospedar e implantar aplicativos de visualização do ASP.NET Core no Azure</span><span class="sxs-lookup"><span data-stu-id="c9655-143">Host and deploy ASP.NET Core Preview apps on Azure</span></span>](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
