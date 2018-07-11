---
uid: mvc/overview/getting-started/introduction/getting-started
title: Introdução ao ASP.NET MVC 5 | Microsoft Docs
author: Rick-Anderson
description: 'Observação: Uma versão atualizada deste tutorial está disponível aqui usando o Visual Studio 2015. O novo tutorial usa o ASP.NET Core MVC 6, que fornece muitos improvem...'
ms.author: aspnetcontent
ms.date: 05/28/2015
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 85d5d3292ff99ade6995c710e2728c41255def4c
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38211742"
---
<a name="getting-started-with-aspnet-mvc-5"></a><span data-ttu-id="a55bb-104">Introdução ao ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="a55bb-104">Getting Started with ASP.NET MVC 5</span></span>
====================
<span data-ttu-id="a55bb-105">por [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="a55bb-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [consider RP](../../../../includes/razor.md)]

 <span data-ttu-id="a55bb-106">Este tutorial lhe ensinará os conceitos básicos da criação de um aplicativo de web de ASP.NET MVC 5 usando [Visual Studio 2017](https://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="a55bb-106">This tutorial will teach you the basics of building an ASP.NET MVC 5 web app using [Visual Studio 2017](https://www.visualstudio.com/).</span></span> <span data-ttu-id="a55bb-107">Fonte final para o tutorial localizada [GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)</span><span class="sxs-lookup"><span data-stu-id="a55bb-107">Final Source for tutorial located on [GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)</span></span>


 <span data-ttu-id="a55bb-108">Este tutorial foi escrito por [Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[ @scottgu ](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](https://twitter.com/shanselman) ) , e [Rick Anderson](https://twitter.com/RickAndMSFT) ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )</span><span class="sxs-lookup"><span data-stu-id="a55bb-108">This tutorial was written by [Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[@scottgu](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](https://twitter.com/shanselman) ), and [Rick Anderson](https://twitter.com/RickAndMSFT) ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) )</span></span>

 <span data-ttu-id="a55bb-109">Você precisa de uma conta do Azure para implantar esse aplicativo no Azure:</span><span class="sxs-lookup"><span data-stu-id="a55bb-109">You need an Azure account to deploy this app to Azure:</span></span>

 - <span data-ttu-id="a55bb-110">Você pode [abrir uma conta do Azure gratuitamente](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -obtenha créditos você pode usar para experimentar serviços pagos do Azure e até mesmo após eles serem utilizados, você pode manter a conta e utilizar os serviços do Azure gratuitos.</span><span class="sxs-lookup"><span data-stu-id="a55bb-110">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
 - <span data-ttu-id="a55bb-111">Você pode [ativar benefícios de assinante do MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -sua assinatura do MSDN concede créditos todos os meses que você pode usar para serviços pagos do Azure.</span><span class="sxs-lookup"><span data-stu-id="a55bb-111">You can [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>


## <a name="getting-started"></a><span data-ttu-id="a55bb-112">Guia de Introdução</span><span class="sxs-lookup"><span data-stu-id="a55bb-112">Getting Started</span></span>

<span data-ttu-id="a55bb-113">Comece instalando e executando [Visual Studio 2017](https://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="a55bb-113">Start by installing and running [Visual Studio 2017](https://www.visualstudio.com/).</span></span>

<span data-ttu-id="a55bb-114">Visual Studio é um IDE ou ambiente de desenvolvimento integrado.</span><span class="sxs-lookup"><span data-stu-id="a55bb-114">Visual Studio is an IDE, or integrated development environment.</span></span> <span data-ttu-id="a55bb-115">Assim como usar o Microsoft Word para gravar documentos, você usará um IDE para criar aplicativos.</span><span class="sxs-lookup"><span data-stu-id="a55bb-115">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="a55bb-116">No Visual Studio, há uma lista na parte inferior, mostrando várias opções disponíveis para você.</span><span class="sxs-lookup"><span data-stu-id="a55bb-116">In Visual Studio there's a list along the bottom showing various options available to you.</span></span> <span data-ttu-id="a55bb-117">Há também um menu que fornece outra maneira de executar tarefas no IDE.</span><span class="sxs-lookup"><span data-stu-id="a55bb-117">There's also a menu that provides another way to perform tasks in the IDE.</span></span> <span data-ttu-id="a55bb-118">(Por exemplo, em vez de selecionar **novo projeto** da **inicie** página, você pode usar o menu e selecione **arquivo** &gt; **denovoprojeto**.)</span><span class="sxs-lookup"><span data-stu-id="a55bb-118">(For example, instead of selecting **New Project** from the **Start** page, you can use the menu and select **File** &gt; **New Project**.)</span></span>


![](getting-started/_static/image1.png)  


## <a name="creating-your-first-application"></a><span data-ttu-id="a55bb-119">Criando seu primeiro aplicativo</span><span class="sxs-lookup"><span data-stu-id="a55bb-119">Creating Your First Application</span></span>

<span data-ttu-id="a55bb-120">Clique em **novo projeto**, em seguida, selecione Visual c# à esquerda, em seguida, **Web** e, em seguida, selecione **aplicativo Web ASP.NET (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="a55bb-120">Click **New Project**, then select Visual C# on the left, then **Web** and then select **ASP.NET Web Application (.NET Framework)**.</span></span> <span data-ttu-id="a55bb-121">Nomeie o projeto "como MvcMovie" e, em seguida, clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="a55bb-121">Name your project "MvcMovie" and then click **OK**.</span></span>

![](getting-started/_static/image2.png)

<span data-ttu-id="a55bb-122">No **novo projeto ASP.NET** caixa de diálogo, clique em **MVC** e, em seguida, clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="a55bb-122">In the **New ASP.NET Project** dialog, click **MVC** and then click **OK**.</span></span>

![](getting-started/_static/image3.png)

<span data-ttu-id="a55bb-123">Visual Studio usou um modelo padrão para o projeto do ASP.NET MVC que você acabou de criar, portanto, você tem agora um aplicativo de trabalho sem fazer nada!</span><span class="sxs-lookup"><span data-stu-id="a55bb-123">Visual Studio used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="a55bb-124">Isso é um simples "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="a55bb-124">This is a simple "Hello World!"</span></span> <span data-ttu-id="a55bb-125">projeto e ele á um bom lugar para iniciar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="a55bb-125">project, and it's a good place to start your application.</span></span>

![](getting-started/_static/image4.png)

<span data-ttu-id="a55bb-126">Clique em F5 para iniciar a depuração.</span><span class="sxs-lookup"><span data-stu-id="a55bb-126">Click F5 to start debugging.</span></span> <span data-ttu-id="a55bb-127">F5 faz com que o Visual Studio para iniciar [IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) e executar seu aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="a55bb-127">F5 causes Visual Studio to start [IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) and run your web app.</span></span> <span data-ttu-id="a55bb-128">Em seguida, o Visual Studio inicia um navegador e abre a página inicial do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="a55bb-128">Visual Studio then launches a browser and opens the application's home page.</span></span> <span data-ttu-id="a55bb-129">Observe que a barra de endereços do navegador diz `localhost:port#` e não algo como `example.com`.</span><span class="sxs-lookup"><span data-stu-id="a55bb-129">Notice that the address bar of the browser says `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="a55bb-130">Isso ocorre porque `localhost` sempre aponta para o seu próprio computador local, que nesse caso está executando o aplicativo que você acabou de criar.</span><span class="sxs-lookup"><span data-stu-id="a55bb-130">That's because `localhost` always points to your own local computer, which in this case is running the application you just built.</span></span> <span data-ttu-id="a55bb-131">Quando o Visual Studio executa um projeto da web, uma porta aleatória é usada para o servidor web.</span><span class="sxs-lookup"><span data-stu-id="a55bb-131">When Visual Studio runs a web project, a random port is used for the web server.</span></span> <span data-ttu-id="a55bb-132">Na imagem abaixo, o número da porta for 1234.</span><span class="sxs-lookup"><span data-stu-id="a55bb-132">In the image below, the port number is 1234.</span></span> <span data-ttu-id="a55bb-133">Quando você executa o aplicativo, você verá um número de porta diferente.</span><span class="sxs-lookup"><span data-stu-id="a55bb-133">When you run the application, you'll see a different port number.</span></span>

![](getting-started/_static/image5.png)

<span data-ttu-id="a55bb-134">Prontos nesse modelo padrão fornece páginas Home, contato e sobre.</span><span class="sxs-lookup"><span data-stu-id="a55bb-134">Right out of the box this default template gives you Home, Contact and About pages.</span></span> <span data-ttu-id="a55bb-135">A imagem acima não mostra a **página inicial**, **sobre** e **contato** links.</span><span class="sxs-lookup"><span data-stu-id="a55bb-135">The image above doesn't show the **Home**, **About** and **Contact** links.</span></span> <span data-ttu-id="a55bb-136">Dependendo do tamanho da janela do navegador, você talvez seja necessário clicar no ícone de navegação para ver esses links.</span><span class="sxs-lookup"><span data-stu-id="a55bb-136">Depending on the size of your browser window, you might need to click the navigation icon to see these links.</span></span>

![](getting-started/_static/image6.png)  

<span data-ttu-id="a55bb-137">O aplicativo também fornece suporte para se registrar e fazer logon.</span><span class="sxs-lookup"><span data-stu-id="a55bb-137">The application also provides support to register and log in.</span></span> <span data-ttu-id="a55bb-138">A próxima etapa é alterar como este aplicativo funciona e aprender um pouco sobre o ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="a55bb-138">The next step is to change how this application works and learn a little bit about ASP.NET MVC.</span></span> <span data-ttu-id="a55bb-139">Feche o aplicativo ASP.NET MVC e vamos alterar algum código.</span><span class="sxs-lookup"><span data-stu-id="a55bb-139">Close the ASP.NET MVC application and let's change some code.</span></span>

<span data-ttu-id="a55bb-140">Para obter uma lista de tutoriais atuais, consulte [MVC artigos recomendado](../mvc-learning-sequence.md).</span><span class="sxs-lookup"><span data-stu-id="a55bb-140">For a list of current tutorials, see [MVC recommended articles](../mvc-learning-sequence.md).</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="a55bb-141">Consulte esse aplicativo em execução no Azure</span><span class="sxs-lookup"><span data-stu-id="a55bb-141">See this App Running on Azure</span></span>

<span data-ttu-id="a55bb-142">Você gostaria de ver o site concluído em execução como um aplicativo web ao vivo?</span><span class="sxs-lookup"><span data-stu-id="a55bb-142">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="a55bb-143">Você pode implantar uma versão completa do aplicativo para sua conta do Azure, simplesmente clicando no botão a seguir.</span><span class="sxs-lookup"><span data-stu-id="a55bb-143">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

<span data-ttu-id="a55bb-144">Você precisa de uma conta do Azure para implantar essa solução no Azure.</span><span class="sxs-lookup"><span data-stu-id="a55bb-144">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="a55bb-145">Se você ainda não tiver uma conta, você tem as seguintes opções:</span><span class="sxs-lookup"><span data-stu-id="a55bb-145">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="a55bb-146">[Abrir uma conta do Azure gratuitamente](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -obtenha créditos você pode usar para experimentar serviços pagos do Azure e até mesmo após eles serem utilizados, você pode manter a conta e utilizar os serviços do Azure gratuitos.</span><span class="sxs-lookup"><span data-stu-id="a55bb-146">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="a55bb-147">[Ativar benefícios de assinante do MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -sua assinatura do MSDN concede créditos todos os meses que você pode usar para serviços pagos do Azure.</span><span class="sxs-lookup"><span data-stu-id="a55bb-147">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="a55bb-148">Avançar</span><span class="sxs-lookup"><span data-stu-id="a55bb-148">Next</span></span>](adding-a-controller.md)
