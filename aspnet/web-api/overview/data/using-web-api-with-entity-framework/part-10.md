---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: Publicar o aplicativo do Azure do serviço de aplicativo do Azure | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: cc8a9199144e9fac041435938ea8899374ea199f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30867808"
---
<a name="publish-the-app-to-azure-azure-app-service"></a><span data-ttu-id="ba394-102">Publicar o aplicativo do Azure do serviço de aplicativo do Azure</span><span class="sxs-lookup"><span data-stu-id="ba394-102">Publish the App to Azure Azure App Service</span></span>
====================
<span data-ttu-id="ba394-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ba394-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="ba394-104">Baixe o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="ba394-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="ba394-105">Como a última etapa, você publicará o aplicativo no Azure.</span><span class="sxs-lookup"><span data-stu-id="ba394-105">As the last step, you will publish the application to Azure.</span></span> <span data-ttu-id="ba394-106">No Gerenciador de soluções, clique com o botão direito e selecione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="ba394-106">In Solution Explorer, right-click the project and select **Publish**.</span></span>

![](part-10/_static/image1.png)

<span data-ttu-id="ba394-107">Clicando em **publicar** invoca o **Publicar Web** caixa de diálogo.</span><span class="sxs-lookup"><span data-stu-id="ba394-107">Clicking **Publish** invokes the **Publish Web** dialog.</span></span> <span data-ttu-id="ba394-108">Se você tiver marcado **Host na nuvem** quando criou o projeto e, em seguida, a conexão e já estão configuradas.</span><span class="sxs-lookup"><span data-stu-id="ba394-108">If you checked **Host in Cloud** when you first created the project, then the connection and settings are already configured.</span></span> <span data-ttu-id="ba394-109">Nesse caso, basta clicar o **configurações** guia e verificar &quot;executar migrações do Code First&quot;.</span><span class="sxs-lookup"><span data-stu-id="ba394-109">In that case, just click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="ba394-110">(Se você não marcar **Host na nuvem** no início, em seguida, siga as etapas a [próxima seção](#new-website).)</span><span class="sxs-lookup"><span data-stu-id="ba394-110">(If you didn't check **Host in Cloud** at the beginning, then follow the steps in the [next section](#new-website).)</span></span>

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

<span data-ttu-id="ba394-111">Para implantar o aplicativo, clique em **publicar**.</span><span class="sxs-lookup"><span data-stu-id="ba394-111">To deploy the app, click **Publish**.</span></span> <span data-ttu-id="ba394-112">Você pode exibir o andamento da publicação na **atividade de publicar Web** janela.</span><span class="sxs-lookup"><span data-stu-id="ba394-112">You can view the publishing progress in the **Web Publish Activity** window.</span></span> <span data-ttu-id="ba394-113">(Da **exibição** menu, selecione **outras janelas**, em seguida, selecione **atividade de publicar Web**.)</span><span class="sxs-lookup"><span data-stu-id="ba394-113">(From the **View** menu, select **Other Windows**, then select **Web Publish Activity**.)</span></span>

![](part-10/_static/image4.png)

<span data-ttu-id="ba394-114">Quando o Visual Studio terminar a implantação do aplicativo, o navegador padrão é aberto automaticamente para a URL do site da Web implantado e o aplicativo que você criou agora está em execução na nuvem.</span><span class="sxs-lookup"><span data-stu-id="ba394-114">When Visual Studio finishes deploying the app, the default browser automatically opens to the URL of the deployed website, and the application that you created is now running in the cloud.</span></span> <span data-ttu-id="ba394-115">A URL na barra de endereços do navegador mostra que o site está sendo carregado pela Internet.</span><span class="sxs-lookup"><span data-stu-id="ba394-115">The URL in the browser address bar shows that the site is being loaded from the Internet.</span></span>

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a><span data-ttu-id="ba394-116">Implantando um novo site</span><span class="sxs-lookup"><span data-stu-id="ba394-116">Deploying to a New Website</span></span>

<span data-ttu-id="ba394-117">Se você não marcar **Host na nuvem** quando o projeto foi criado, você pode configurar um novo aplicativo web agora.</span><span class="sxs-lookup"><span data-stu-id="ba394-117">If you did not check **Host in Cloud** when you first created the project, you can configure a new web app now.</span></span> <span data-ttu-id="ba394-118">No Gerenciador de soluções, clique com o botão direito e selecione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="ba394-118">In Solution Explorer, right-click the project and select **Publish**.</span></span> <span data-ttu-id="ba394-119">Selecione o **perfil** guia e clique em **sites do Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="ba394-119">Select the **Profile** tab and click **Microsoft Azure Websites**.</span></span> <span data-ttu-id="ba394-120">Se você não estiver conectado atualmente no Azure, você será solicitado a entrar.</span><span class="sxs-lookup"><span data-stu-id="ba394-120">If you aren't currently signed in to Azure, you will be prompted to sign in.</span></span>

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

<span data-ttu-id="ba394-121">No **de sites da Web** caixa de diálogo, clique em **novo**.</span><span class="sxs-lookup"><span data-stu-id="ba394-121">In the **Existing Websites** dialog, click **New**.</span></span>

![](part-10/_static/image9.png)

<span data-ttu-id="ba394-122">Insira um nome de site.</span><span class="sxs-lookup"><span data-stu-id="ba394-122">Enter a site name.</span></span> <span data-ttu-id="ba394-123">Selecione sua assinatura do Azure e a região.</span><span class="sxs-lookup"><span data-stu-id="ba394-123">Select your Azure subscription and the region.</span></span> <span data-ttu-id="ba394-124">Em **o servidor de banco de dados**, selecione **criar novo servidor**, ou selecione um servidor existente.</span><span class="sxs-lookup"><span data-stu-id="ba394-124">Under **Database server**, select **Create New Server**, or select an existing server.</span></span> <span data-ttu-id="ba394-125">Clique em **Criar**.</span><span class="sxs-lookup"><span data-stu-id="ba394-125">Click **Create**.</span></span>

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

<span data-ttu-id="ba394-126">Clique o **configurações** guia e verificar &quot;executar migrações do Code First&quot;.</span><span class="sxs-lookup"><span data-stu-id="ba394-126">Click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="ba394-127">Em seguida, clique em **publicar**.</span><span class="sxs-lookup"><span data-stu-id="ba394-127">Then click **Publish**.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ba394-128">Anterior</span><span class="sxs-lookup"><span data-stu-id="ba394-128">Previous</span></span>](part-9.md)
