---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: Publicar o aplicativo no Azure App Service do Azure | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: 0290b392c1b292d0f3cc080dbfa25ec6103b2751
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400798"
---
<a name="publish-the-app-to-azure-azure-app-service"></a><span data-ttu-id="ea25e-102">Publicar o aplicativo no Azure App Service do Azure</span><span class="sxs-lookup"><span data-stu-id="ea25e-102">Publish the App to Azure Azure App Service</span></span>
====================
<span data-ttu-id="ea25e-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ea25e-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="ea25e-104">Baixe o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="ea25e-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="ea25e-105">Como a última etapa, você publicará o aplicativo no Azure.</span><span class="sxs-lookup"><span data-stu-id="ea25e-105">As the last step, you will publish the application to Azure.</span></span> <span data-ttu-id="ea25e-106">No Gerenciador de soluções, clique com botão direito no projeto e selecione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="ea25e-106">In Solution Explorer, right-click the project and select **Publish**.</span></span>

![](part-10/_static/image1.png)

<span data-ttu-id="ea25e-107">Clicando em **Publish** invoca o **publicar na Web** caixa de diálogo.</span><span class="sxs-lookup"><span data-stu-id="ea25e-107">Clicking **Publish** invokes the **Publish Web** dialog.</span></span> <span data-ttu-id="ea25e-108">Se você tiver marcado **Host na nuvem** quando você criou o projeto e, em seguida, a conexão e já estão configuradas.</span><span class="sxs-lookup"><span data-stu-id="ea25e-108">If you checked **Host in Cloud** when you first created the project, then the connection and settings are already configured.</span></span> <span data-ttu-id="ea25e-109">Nesse caso, basta clicar o **as configurações** guia e verifique &quot;executar migrações do Code First&quot;.</span><span class="sxs-lookup"><span data-stu-id="ea25e-109">In that case, just click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="ea25e-110">(Se você não marcou **Host na nuvem** no início, em seguida, siga as etapas a [próxima seção](#new-website).)</span><span class="sxs-lookup"><span data-stu-id="ea25e-110">(If you didn't check **Host in Cloud** at the beginning, then follow the steps in the [next section](#new-website).)</span></span>

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

<span data-ttu-id="ea25e-111">Para implantar o aplicativo, clique em **publicar**.</span><span class="sxs-lookup"><span data-stu-id="ea25e-111">To deploy the app, click **Publish**.</span></span> <span data-ttu-id="ea25e-112">Você pode exibir o andamento da publicação na **atividade de publicação de Web** janela.</span><span class="sxs-lookup"><span data-stu-id="ea25e-112">You can view the publishing progress in the **Web Publish Activity** window.</span></span> <span data-ttu-id="ea25e-113">(Da **modo de exibição** menu, selecione **Other Windows**, em seguida, selecione **a atividade de publicação de Web**.)</span><span class="sxs-lookup"><span data-stu-id="ea25e-113">(From the **View** menu, select **Other Windows**, then select **Web Publish Activity**.)</span></span>

![](part-10/_static/image4.png)

<span data-ttu-id="ea25e-114">Quando o Visual Studio terminar a implantação do aplicativo, o navegador padrão abre automaticamente a URL do site da Web implantado e o aplicativo que você criou agora está em execução na nuvem.</span><span class="sxs-lookup"><span data-stu-id="ea25e-114">When Visual Studio finishes deploying the app, the default browser automatically opens to the URL of the deployed website, and the application that you created is now running in the cloud.</span></span> <span data-ttu-id="ea25e-115">A URL na barra de endereços do navegador mostra que o site está sendo carregado da Internet.</span><span class="sxs-lookup"><span data-stu-id="ea25e-115">The URL in the browser address bar shows that the site is being loaded from the Internet.</span></span>

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a><span data-ttu-id="ea25e-116">Implantando para um novo site</span><span class="sxs-lookup"><span data-stu-id="ea25e-116">Deploying to a New Website</span></span>

<span data-ttu-id="ea25e-117">Se você não marcou **Host na nuvem** quando criou o projeto pela primeira vez, você pode configurar um novo aplicativo web agora.</span><span class="sxs-lookup"><span data-stu-id="ea25e-117">If you did not check **Host in Cloud** when you first created the project, you can configure a new web app now.</span></span> <span data-ttu-id="ea25e-118">No Gerenciador de soluções, clique com botão direito no projeto e selecione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="ea25e-118">In Solution Explorer, right-click the project and select **Publish**.</span></span> <span data-ttu-id="ea25e-119">Selecione o **perfil** guia e clique em **sites do Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="ea25e-119">Select the **Profile** tab and click **Microsoft Azure Websites**.</span></span> <span data-ttu-id="ea25e-120">Se você não entrou no Azure, você será solicitado a entrar.</span><span class="sxs-lookup"><span data-stu-id="ea25e-120">If you aren't currently signed in to Azure, you will be prompted to sign in.</span></span>

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

<span data-ttu-id="ea25e-121">No **sites existentes** caixa de diálogo, clique em **New**.</span><span class="sxs-lookup"><span data-stu-id="ea25e-121">In the **Existing Websites** dialog, click **New**.</span></span>

![](part-10/_static/image9.png)

<span data-ttu-id="ea25e-122">Insira um nome de site.</span><span class="sxs-lookup"><span data-stu-id="ea25e-122">Enter a site name.</span></span> <span data-ttu-id="ea25e-123">Selecione sua assinatura do Azure e a região.</span><span class="sxs-lookup"><span data-stu-id="ea25e-123">Select your Azure subscription and the region.</span></span> <span data-ttu-id="ea25e-124">Sob **servidor de banco de dados**, selecione **criar novo servidor**, ou selecione um servidor existente.</span><span class="sxs-lookup"><span data-stu-id="ea25e-124">Under **Database server**, select **Create New Server**, or select an existing server.</span></span> <span data-ttu-id="ea25e-125">Clique em **Criar**.</span><span class="sxs-lookup"><span data-stu-id="ea25e-125">Click **Create**.</span></span>

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

<span data-ttu-id="ea25e-126">Clique o **as configurações** guia e verifique &quot;executar migrações do Code First&quot;.</span><span class="sxs-lookup"><span data-stu-id="ea25e-126">Click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="ea25e-127">Em seguida, clique em **publicar**.</span><span class="sxs-lookup"><span data-stu-id="ea25e-127">Then click **Publish**.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ea25e-128">Anterior</span><span class="sxs-lookup"><span data-stu-id="ea25e-128">Previous</span></span>](part-9.md)
