---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: Usando o Web API 2 com o Entity Framework 6 | Microsoft Docs
author: MikeWasson
description: Este tutorial irá ensiná-lo a Noções básicas de criação de um aplicativo web com uma API da Web ASP.NET back-end. O tutorial usa o Entity Framework 6 para o layout de dados...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: 8e6d381509a121e3036ca3af91ea3b9bd0be33c2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871968"
---
<a name="using-web-api-2-with-entity-framework-6"></a><span data-ttu-id="a0f46-104">Usando o Web API 2 com o Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="a0f46-104">Using Web API 2 with Entity Framework 6</span></span>
====================
<span data-ttu-id="a0f46-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a0f46-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="a0f46-106">Baixe o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="a0f46-106">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

> <span data-ttu-id="a0f46-107">Este tutorial irá ensiná-lo a Noções básicas de criação de um aplicativo web com uma API da Web ASP.NET back-end.</span><span class="sxs-lookup"><span data-stu-id="a0f46-107">This tutorial will teach you the basics of creating a web application with an ASP.NET Web API back end.</span></span> <span data-ttu-id="a0f46-108">O tutorial usa o Entity Framework 6 para a camada de dados e Knockout. js para o aplicativo de JavaScript do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="a0f46-108">The tutorial uses Entity Framework 6 for the data layer, and Knockout.js for the client-side JavaScript application.</span></span> <span data-ttu-id="a0f46-109">O tutorial também mostra como implantar o aplicativo para aplicativos de Web do serviço de aplicativo do Azure.</span><span class="sxs-lookup"><span data-stu-id="a0f46-109">The tutorial also shows how to deploy the app to Azure App Service Web Apps.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="a0f46-110">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="a0f46-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="a0f46-111">2.1 da API da Web</span><span class="sxs-lookup"><span data-stu-id="a0f46-111">Web API 2.1</span></span>
> - [<span data-ttu-id="a0f46-112">Visual Studio 2013 Atualização 2</span><span class="sxs-lookup"><span data-stu-id="a0f46-112">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="a0f46-113">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="a0f46-113">Entity Framework 6</span></span>
> - <span data-ttu-id="a0f46-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="a0f46-114">.NET 4.5</span></span>
> - <span data-ttu-id="a0f46-115">[Knockout. js](http://knockoutjs.com/) 3.1</span><span class="sxs-lookup"><span data-stu-id="a0f46-115">[Knockout.js](http://knockoutjs.com/) 3.1</span></span>


<span data-ttu-id="a0f46-116">Este tutorial usa o ASP.NET Web API 2 com o Entity Framework 6 para criar um aplicativo web que manipula um banco de dados de back-end.</span><span class="sxs-lookup"><span data-stu-id="a0f46-116">This tutorial uses ASP.NET Web API 2 with Entity Framework 6 to create a web application that manipulates a back-end database.</span></span> <span data-ttu-id="a0f46-117">Aqui está uma captura de tela do aplicativo que você criará.</span><span class="sxs-lookup"><span data-stu-id="a0f46-117">Here is a screen shot of the application that you will create.</span></span>

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

<span data-ttu-id="a0f46-118">O aplicativo usa um design de aplicativo de página única (SPA).</span><span class="sxs-lookup"><span data-stu-id="a0f46-118">The app uses a single-page application (SPA) design.</span></span> <span data-ttu-id="a0f46-119">"Aplicativo de página única" é o termo geral para um aplicativo web que carrega uma única página HTML e, em seguida, atualiza a página dinamicamente, em vez de carregar as novas páginas.</span><span class="sxs-lookup"><span data-stu-id="a0f46-119">"Single-page application" is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="a0f46-120">Após o carregamento de página inicial, o aplicativo se comunica com o servidor por meio de solicitações do AJAX.</span><span class="sxs-lookup"><span data-stu-id="a0f46-120">After the initial page load, the app talks with the server through AJAX requests.</span></span> <span data-ttu-id="a0f46-121">O AJAX solicitações retornar dados JSON, que o aplicativo usa para atualizar a interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="a0f46-121">The AJAX requests return JSON data, which the app uses to update the UI.</span></span>

<span data-ttu-id="a0f46-122">AJAX não é novo, mas atualmente há estruturas de JavaScript que tornam mais fácil de criar e manter um grande aplicativo SPA sofisticado.</span><span class="sxs-lookup"><span data-stu-id="a0f46-122">AJAX isn't new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="a0f46-123">Este tutorial usa [Knockout. js](http://knockoutjs.com/), mas você pode usar qualquer estrutura de cliente JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a0f46-123">This tutorial uses [Knockout.js](http://knockoutjs.com/), but you can use any JavaScript client framework.</span></span>

<span data-ttu-id="a0f46-124">Aqui estão os principais blocos de construção para este aplicativo:</span><span class="sxs-lookup"><span data-stu-id="a0f46-124">Here are the main building blocks for this app:</span></span>

- <span data-ttu-id="a0f46-125">ASP.NET MVC cria a página HTML.</span><span class="sxs-lookup"><span data-stu-id="a0f46-125">ASP.NET MVC creates the HTML page.</span></span>
- <span data-ttu-id="a0f46-126">ASP.NET Web API trata as solicitações do AJAX e retorna dados JSON.</span><span class="sxs-lookup"><span data-stu-id="a0f46-126">ASP.NET Web API handles the AJAX requests and returns JSON data.</span></span>
- <span data-ttu-id="a0f46-127">Knockout. js dados-associa os elementos HTML para os dados JSON.</span><span class="sxs-lookup"><span data-stu-id="a0f46-127">Knockout.js data-binds the HTML elements to the JSON data.</span></span>
- <span data-ttu-id="a0f46-128">Entity Framework se comunica com o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="a0f46-128">Entity Framework talks to the database.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="a0f46-129">Consulte este aplicativo em execução no Azure</span><span class="sxs-lookup"><span data-stu-id="a0f46-129">See this App Running on Azure</span></span>

<span data-ttu-id="a0f46-130">Você gostaria de ver o site concluído em execução como um aplicativo web em tempo real?</span><span class="sxs-lookup"><span data-stu-id="a0f46-130">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="a0f46-131">Você pode implantar uma versão completa do aplicativo para sua conta do Azure, clicando no botão a seguir.</span><span class="sxs-lookup"><span data-stu-id="a0f46-131">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

<span data-ttu-id="a0f46-132">Você precisa de uma conta do Azure para implantar essa solução no Azure.</span><span class="sxs-lookup"><span data-stu-id="a0f46-132">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="a0f46-133">Se você não tiver uma conta, você tem as seguintes opções:</span><span class="sxs-lookup"><span data-stu-id="a0f46-133">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="a0f46-134">[Abra uma conta do Azure gratuitamente](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -você obtém créditos você pode usar para experimentar os serviços do Azure pagos e mesmo depois que eles são usados até que você pode manter a conta e livre de usar os serviços do Azure.</span><span class="sxs-lookup"><span data-stu-id="a0f46-134">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="a0f46-135">[Ativar os benefícios de assinante do MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -assinatura do MSDN fornece créditos de cada mês em que você pode usar para os serviços do Azure pagos.</span><span class="sxs-lookup"><span data-stu-id="a0f46-135">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="a0f46-136">Criar o projeto</span><span class="sxs-lookup"><span data-stu-id="a0f46-136">Create the Project</span></span>

<span data-ttu-id="a0f46-137">Abra o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a0f46-137">Open Visual Studio.</span></span> <span data-ttu-id="a0f46-138">Do **arquivo** menu, selecione **novo**, em seguida, selecione **projeto**.</span><span class="sxs-lookup"><span data-stu-id="a0f46-138">From the **File** menu, select **New**, then select **Project**.</span></span> <span data-ttu-id="a0f46-139">(Ou clique em **novo projeto** na página inicial.)</span><span class="sxs-lookup"><span data-stu-id="a0f46-139">(Or click **New Project** on the Start page.)</span></span>

<span data-ttu-id="a0f46-140">No **novo projeto** caixa de diálogo, clique em **Web** no painel esquerdo e **aplicativo Web ASP.NET** no painel central.</span><span class="sxs-lookup"><span data-stu-id="a0f46-140">In the **New Project** dialog, click **Web** in the left pane and **ASP.NET Web Application** in the middle pane.</span></span> <span data-ttu-id="a0f46-141">Nomeie o projeto BookService e clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="a0f46-141">Name the project BookService and click **OK**.</span></span>

[![](part-1/_static/image4.png)](part-1/_static/image3.png)

<span data-ttu-id="a0f46-142">No **novo projeto ASP.NET** caixa de diálogo, selecione o **API da Web** modelo.</span><span class="sxs-lookup"><span data-stu-id="a0f46-142">In the **New ASP.NET Project** dialog, select the **Web API** template.</span></span>

[![](part-1/_static/image6.png)](part-1/_static/image5.png)

<span data-ttu-id="a0f46-143">Se você deseja hospedar o projeto em um serviço de aplicativo do Azure, deixe o **Host na nuvem** caixa marcada.</span><span class="sxs-lookup"><span data-stu-id="a0f46-143">If you want to host the project in a Azure App Service, leave the **Host in the cloud** box checked.</span></span>

<span data-ttu-id="a0f46-144">Clique em **OK** para criar o projeto.</span><span class="sxs-lookup"><span data-stu-id="a0f46-144">Click **OK** to create the project.</span></span>

## <a name="configure-azure-settings-optional"></a><span data-ttu-id="a0f46-145">Definir configurações do Azure (opcionais)</span><span class="sxs-lookup"><span data-stu-id="a0f46-145">Configure Azure Settings (Optional)</span></span>

<span data-ttu-id="a0f46-146">Se você deixou o **Host na nuvem** opção marcada, o Visual Studio será solicitado a entrar no Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="a0f46-146">If you left the **Host in Cloud** option checked, Visual Studio will prompt you to sign in to Microsoft Azure</span></span>

[![](part-1/_static/image8.png)](part-1/_static/image7.png)

<span data-ttu-id="a0f46-147">Depois de entrar Azure, o Visual Studio solicitará que você configurar o aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="a0f46-147">After you sign in to Azure, Visual Studio prompts you to configure the web app.</span></span> <span data-ttu-id="a0f46-148">Insira um nome para o site, selecione sua assinatura do Azure e selecione uma região geográfica.</span><span class="sxs-lookup"><span data-stu-id="a0f46-148">Enter a name for the site, select your Azure subscription, and select a geographical region.</span></span> <span data-ttu-id="a0f46-149">Em **o servidor de banco de dados**, selecione **criar novo servidor**.</span><span class="sxs-lookup"><span data-stu-id="a0f46-149">Under **Database server**, select **Create new server**.</span></span> <span data-ttu-id="a0f46-150">Insira um nome de usuário administrador e uma senha.</span><span class="sxs-lookup"><span data-stu-id="a0f46-150">Enter an administrator username and password.</span></span>

[![](part-1/_static/image10.png)](part-1/_static/image9.png)

> [!div class="step-by-step"]
> [<span data-ttu-id="a0f46-151">Avançar</span><span class="sxs-lookup"><span data-stu-id="a0f46-151">Next</span></span>](part-2.md)
