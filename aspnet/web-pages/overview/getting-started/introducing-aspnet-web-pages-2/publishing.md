---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
title: Introdução ao ASP.NET Web Pages – publicar um Site usando o WebMatrix | Microsoft Docs
author: tfitzmac
description: Este tutorial é a parte final no conjunto de tutoriais que apresenta a páginas da Web ASP.NET e o Microsoft WebMatrix. Ele discute como publicar seu site t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 7e85c70e-1a88-4408-8b3d-29611c7713ed
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
msc.type: authoredcontent
ms.openlocfilehash: c6fa7692527b7aa65e93cd57ed5bd56f42e54bd6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37373481"
---
<a name="introducing-aspnet-web-pages---publishing-a-site-by-using-webmatrix"></a><span data-ttu-id="06a79-104">Introdução ao ASP.NET Web Pages - publicar um Site usando o WebMatrix</span><span class="sxs-lookup"><span data-stu-id="06a79-104">Introducing ASP.NET Web Pages - Publishing a Site by Using WebMatrix</span></span>
====================
<span data-ttu-id="06a79-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="06a79-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="06a79-106">Este tutorial é a parte final no conjunto de tutoriais que apresenta a páginas da Web ASP.NET e o Microsoft WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="06a79-106">This tutorial is the final installment in the tutorial set that introduces ASP.NET Web Pages and Microsoft WebMatrix.</span></span> <span data-ttu-id="06a79-107">Ele discute como publicar seu site na Internet para que outras pessoas possam trabalhar com ele.</span><span class="sxs-lookup"><span data-stu-id="06a79-107">It discusses how to publish your site to the Internet so that others can work with it.</span></span> <span data-ttu-id="06a79-108">Ele pressupõe que você tenha concluído a série por meio [criar uma aparência consistente para Sites de páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251585).</span><span class="sxs-lookup"><span data-stu-id="06a79-108">It assumes you have completed the series through [Creating a Consistent Look for ASP.NET Web Pages Sites](https://go.microsoft.com/fwlink/?LinkId=251585).</span></span>
> 
> <span data-ttu-id="06a79-109">Você aprenderá como publicar seu site usando:</span><span class="sxs-lookup"><span data-stu-id="06a79-109">You'll learn how to publish your site using:</span></span>
> 
> - <span data-ttu-id="06a79-110">Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="06a79-110">Microsoft Azure</span></span>
> - <span data-ttu-id="06a79-111">Empresa de hospedagem da Web</span><span class="sxs-lookup"><span data-stu-id="06a79-111">Web Hosting Company</span></span>


## <a name="about-publishing-your-site"></a><span data-ttu-id="06a79-112">Sobre como publicar seu Site</span><span class="sxs-lookup"><span data-stu-id="06a79-112">About Publishing Your Site</span></span>

<span data-ttu-id="06a79-113">Até agora, você fez seu trabalho em um computador local, incluindo testes de suas páginas.</span><span class="sxs-lookup"><span data-stu-id="06a79-113">Up to now, you've done all your work on a local computer, including testing your pages.</span></span> <span data-ttu-id="06a79-114">Para executar sua<em>. cshtml</em> páginas, você já usou o servidor web que está incorporado ao WebMatrix, ou seja, o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="06a79-114">To run your<em>.cshtml</em> pages, you've used the web server that's built into WebMatrix, namely IIS Express.</span></span> <span data-ttu-id="06a79-115">Mas é claro, ninguém pode ver o site que você criou, exceto que você.</span><span class="sxs-lookup"><span data-stu-id="06a79-115">But of course no one can see the site you've created except you.</span></span> <span data-ttu-id="06a79-116">Para permitir que outras pessoas trabalhem com o seu site, você precisa publicá-lo com a Internet.</span><span class="sxs-lookup"><span data-stu-id="06a79-116">To let others work with your site, you have to publish it to the Internet.</span></span>

<span data-ttu-id="06a79-117">A menos que você já tiver acesso a um servidor web pública, a publicação significa que você precisa ter uma conta com um *plataforma de nuvem* ou um *provedor de hospedagem*.</span><span class="sxs-lookup"><span data-stu-id="06a79-117">Unless you have access to a public web server already, publishing means that you have to have an account with a *cloud platform* or a *hosting provider*.</span></span> <span data-ttu-id="06a79-118">Uma plataforma de nuvem, como o Microsoft Azure fornece infraestrutura sob demanda para seus aplicativos.</span><span class="sxs-lookup"><span data-stu-id="06a79-118">A cloud platform, such as Microsoft Azure, provides on-demand infrastructure for your applications.</span></span> <span data-ttu-id="06a79-119">Um provedor de hospedagem é uma empresa que possui servidores web acessível publicamente e que será alugar você espaço para o seu site.</span><span class="sxs-lookup"><span data-stu-id="06a79-119">A hosting provider is a company that owns publicly accessible web servers and that will rent you space for your site.</span></span> <span data-ttu-id="06a79-120">Planos de execução de alguns dólares por mês (ou até mesmo gratuitos) de hospedagem para sites pequenos para centenas de dólares por mês para sites comerciais de alto volume.</span><span class="sxs-lookup"><span data-stu-id="06a79-120">Hosting plans run from a few dollars a month (or even free) for small sites to many hundreds of dollars a month for high-volume commercial websites.</span></span>

> [!NOTE]
> <span data-ttu-id="06a79-121">Você pode ter acesso a um servidor web público por meio do provedor de serviço da internet (ISP) que você usa para obter o serviço de internet em casa.</span><span class="sxs-lookup"><span data-stu-id="06a79-121">You might have access to a public web server via the internet service provider (ISP) that you use to get internet service at home.</span></span> <span data-ttu-id="06a79-122">No entanto, seu provedor de hospedagem deve dar suporte a páginas da Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="06a79-122">However, your hosting provider must support ASP.NET Web Pages.</span></span> <span data-ttu-id="06a79-123">Muitos provedores de Internet não, mas sempre vale a pena verificar.</span><span class="sxs-lookup"><span data-stu-id="06a79-123">Many ISPs don't, but it's always worth checking.</span></span>


<span data-ttu-id="06a79-124">Neste tutorial, daremos a você uma visão geral de como publicar.</span><span class="sxs-lookup"><span data-stu-id="06a79-124">In this tutorial, we'll give you an overview of how to publish.</span></span> <span data-ttu-id="06a79-125">Não é prático fornecer detalhes exatos para tudo, porque o processo um pouco diferente para cada provedor de hospedagem.</span><span class="sxs-lookup"><span data-stu-id="06a79-125">It's not practical to provide exact details for everything, because the process differs a bit for every hosting provider.</span></span> <span data-ttu-id="06a79-126">Mas, você obterá uma boa ideia de como funciona o processo.</span><span class="sxs-lookup"><span data-stu-id="06a79-126">But you'll get a good idea of how the process works.</span></span>

<span data-ttu-id="06a79-127">Este tutorial contém quatro seções:</span><span class="sxs-lookup"><span data-stu-id="06a79-127">This tutorial contains four sections:</span></span>

1. [<span data-ttu-id="06a79-128">Como configurar a página padrão</span><span class="sxs-lookup"><span data-stu-id="06a79-128">Setting up the default page</span></span>](#defaultpage)
2. <span data-ttu-id="06a79-129">Publicação (escolha uma das seguintes opções)</span><span class="sxs-lookup"><span data-stu-id="06a79-129">Publishing (choose one of the following)</span></span>  
 <span data-ttu-id="06a79-130">a.</span><span class="sxs-lookup"><span data-stu-id="06a79-130">a.</span></span> [<span data-ttu-id="06a79-131">Publicar seu Site no Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="06a79-131">Publishing Your Site to Microsoft Azure</span></span>](#azure)  
 <span data-ttu-id="06a79-132">b.</span><span class="sxs-lookup"><span data-stu-id="06a79-132">b.</span></span> [<span data-ttu-id="06a79-133">Publicar seu Site em uma empresa de hospedagem na Web</span><span class="sxs-lookup"><span data-stu-id="06a79-133">Publishing Your Site to a Web Hosting Company</span></span>](#host)
3. [<span data-ttu-id="06a79-134">Atualizar o Site ao vivo: republicação</span><span class="sxs-lookup"><span data-stu-id="06a79-134">Updating the Live Site: Republishing</span></span>](#update)

<a id="defaultpage"></a>
## <a name="setting-up-the-default-page"></a><span data-ttu-id="06a79-135">Como configurar a página padrão</span><span class="sxs-lookup"><span data-stu-id="06a79-135">Setting up the default page</span></span>

<span data-ttu-id="06a79-136">Quando um usuário navega para o endereço base para seu site da web, a página padrão para o seu site é exibida ao usuário.</span><span class="sxs-lookup"><span data-stu-id="06a79-136">When a user navigates to the base address for your web site, the default page for your site is displayed to the user.</span></span> <span data-ttu-id="06a79-137">Por exemplo, quando default. htm é definida como a página padrão para o site em www.contoso.com, navegando até <strong>www.contoso.com</strong> é o mesmo que navegando para <strong>www.contoso.com/Default.htm</strong>.</span><span class="sxs-lookup"><span data-stu-id="06a79-137">For example, when Default.htm is set as the default page for the site at www.contoso.com, then navigating to <strong>www.contoso.com</strong> is the same as navigating to <strong>www.contoso.com/Default.htm</strong>.</span></span>

<span data-ttu-id="06a79-138">No momento, seu site utiliza **cshtml** como a página padrão.</span><span class="sxs-lookup"><span data-stu-id="06a79-138">Currently, your site uses **Default.cshtml** as the default page.</span></span> <span data-ttu-id="06a79-139">Esta página é bom para sua página padrão, mas neste tutorial você não adicionou nenhum conteúdo para a página para que ele exibiria uma página em branco.</span><span class="sxs-lookup"><span data-stu-id="06a79-139">This page is fine for your default page, but in this tutorial you have not added any content to that page so it would display a blank page.</span></span> <span data-ttu-id="06a79-140">Abra default. cshtml e substitua o conteúdo com o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="06a79-140">Open Default.cshtml and replace the content with the following code.</span></span>

[!code-cshtml[Main](publishing/samples/sample1.cshtml)]

<span data-ttu-id="06a79-141">Agora seu site está pronto para publicação.</span><span class="sxs-lookup"><span data-stu-id="06a79-141">Now your site is ready for publication.</span></span> <span data-ttu-id="06a79-142">Primeiro, você verá como implantar o site para o Azure e como implantá-lo em uma empresa de hospedagem na web.</span><span class="sxs-lookup"><span data-stu-id="06a79-142">First, you will see how to deploy the site to Azure, and then how to deploy it to a web hosting company.</span></span> <span data-ttu-id="06a79-143">Qualquer opção funciona para seu site da web e você só precisa seguir uma das opções de implantação.</span><span class="sxs-lookup"><span data-stu-id="06a79-143">Either option works for your web site, and you only need to follow one of the deployment options.</span></span>

<a id="azure"></a>
## <a name="publishing-your-site-to-microsoft-azure"></a><span data-ttu-id="06a79-144">Publicar seu Site no Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="06a79-144">Publishing Your Site to Microsoft Azure</span></span>

<span data-ttu-id="06a79-145">Este tutorial primeiro mostrará a você como implantar seu site para o Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="06a79-145">This tutorial will first show you how to deploy your site to Microsoft Azure.</span></span> <span data-ttu-id="06a79-146">Ao entrar com uma conta da Microsoft, você pode criar até 10 sites gratuitos no Azure.</span><span class="sxs-lookup"><span data-stu-id="06a79-146">By signing in with a Microsoft account, you can create up to 10 free sites on Azure.</span></span> <span data-ttu-id="06a79-147">Esses sites gratuitos fornecem uma maneira conveniente para testar seus sites.</span><span class="sxs-lookup"><span data-stu-id="06a79-147">These free sites provide a convenient way to test your sites.</span></span> <span data-ttu-id="06a79-148">Você sempre pode excluir este site de exemplo posteriormente para evitar o uso de todos os seus sites gratuitos.</span><span class="sxs-lookup"><span data-stu-id="06a79-148">You can always delete this example site later to avoid using all of your free sites.</span></span> <span data-ttu-id="06a79-149">Você pode criar uma conta de avaliação gratuita em apenas alguns minutos.</span><span class="sxs-lookup"><span data-stu-id="06a79-149">You can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="06a79-150">Para obter detalhes, consulte [avaliação gratuita do Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="06a79-150">For details, see [Azure Free Trial](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span>

<span data-ttu-id="06a79-151">Na faixa de opções do WebMatrix, clique o **publicar** botão.</span><span class="sxs-lookup"><span data-stu-id="06a79-151">In the WebMatrix ribbon, click the **Publish** button.</span></span>

!['Publicar' botão na faixa de opções do WebMatrix](publishing/_static/image1.png)

<span data-ttu-id="06a79-153">O **publicar seu Site** caixa de diálogo é exibida.</span><span class="sxs-lookup"><span data-stu-id="06a79-153">The **Publish Your Site** dialog box is displayed.</span></span> <span data-ttu-id="06a79-154">Se você não entrou sua conta da Microsoft, a caixa de diálogo conterá uma **Introdução ao Azure** link.</span><span class="sxs-lookup"><span data-stu-id="06a79-154">If you have not signed in to your Microsoft account, the dialog box will contain a **Get started with Azure** link.</span></span> <span data-ttu-id="06a79-155">Clique neste link.</span><span class="sxs-lookup"><span data-stu-id="06a79-155">Click this link.</span></span>

![Publique seu site](publishing/_static/image2.png)

<span data-ttu-id="06a79-157">Se você não tiver entrado uma conta da Microsoft, novamente você terá a oportunidade de entrar.</span><span class="sxs-lookup"><span data-stu-id="06a79-157">If you have not signed in to a Microsoft account, you are again given the opportunity to sign in.</span></span> <span data-ttu-id="06a79-158">Você deve entrar uma conta da Microsoft para publicar seu site no Azure.</span><span class="sxs-lookup"><span data-stu-id="06a79-158">You must sign in to a Microsoft account to publish your site on Azure.</span></span>

![Entrar](publishing/_static/image3.png)

<span data-ttu-id="06a79-160">Depois de entrar sua conta da Microsoft, a caixa de diálogo contém links para criar um novo site no Azure ou se conectar a um de seus sites existentes no Azure.</span><span class="sxs-lookup"><span data-stu-id="06a79-160">After signing in to your Microsoft account, the dialog box contains links to create a new site on Azure or connect to one of your existing sites on Azure.</span></span>

![Criar novo Site](publishing/_static/image4.png)

<span data-ttu-id="06a79-162">Selecione **criar um novo site**.</span><span class="sxs-lookup"><span data-stu-id="06a79-162">Select **Create a new site**.</span></span>

<span data-ttu-id="06a79-163">Se tiver nomeado seu projeto **WebPagesMovies**, o nome padrão para seu site estará **webpagesmovies.azurewebsites.net**.</span><span class="sxs-lookup"><span data-stu-id="06a79-163">If you named your project **WebPagesMovies**, the default name for your site will be **webpagesmovies.azurewebsites.net**.</span></span> <span data-ttu-id="06a79-164">Esse nome padrão é provavelmente não está disponível, conforme indicado pelo ponto de exclamação vermelho.</span><span class="sxs-lookup"><span data-stu-id="06a79-164">This default name is most likely not available, as indicated by the red exclamation mark.</span></span>

![nome do site da Web padrão](publishing/_static/image5.png)

<span data-ttu-id="06a79-166">Altere o nome do site para algo que está disponível e selecione um local perto de seu local.</span><span class="sxs-lookup"><span data-stu-id="06a79-166">Change the site name to something that is available, and select a location that is close to your location.</span></span>

![nome do site alterado](publishing/_static/image6.png)

<span data-ttu-id="06a79-168">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="06a79-168">Click **OK**.</span></span>

<span data-ttu-id="06a79-169">O WebMatrix performss um teste para determinar se o servidor é compatível com o seu site.</span><span class="sxs-lookup"><span data-stu-id="06a79-169">WebMatrix performss a test to determine if the server is compatible with your site.</span></span>

![teste de compatibilidade](publishing/_static/image7.png)

<span data-ttu-id="06a79-171">Selecione **continuar**.</span><span class="sxs-lookup"><span data-stu-id="06a79-171">Select **Continue**.</span></span>

<span data-ttu-id="06a79-172">Os resultados do teste de compatibilidade são exibidos.</span><span class="sxs-lookup"><span data-stu-id="06a79-172">The results of the compatibility test are displayed.</span></span>

![resultados de compatibilidade](publishing/_static/image8.png)

<span data-ttu-id="06a79-174">Selecione **continuar**.</span><span class="sxs-lookup"><span data-stu-id="06a79-174">Select **Continue**.</span></span>

<span data-ttu-id="06a79-175">O WebMatrix exibe os arquivos e bancos de dados que serão publicados no site.</span><span class="sxs-lookup"><span data-stu-id="06a79-175">WebMatrix displays the files and databases that will be published to the site.</span></span> <span data-ttu-id="06a79-176">Como essa é a primeira vez que você está publicando o site, todos os arquivos são listados.</span><span class="sxs-lookup"><span data-stu-id="06a79-176">Since this is the first time you are publishing the site, all of the files are listed.</span></span> <span data-ttu-id="06a79-177">Você pode desmarcar um arquivo que não está pronto para ser publicado.</span><span class="sxs-lookup"><span data-stu-id="06a79-177">You can uncheck a file that is not ready to be published.</span></span> <span data-ttu-id="06a79-178">Em publicações subsequentes, somente os arquivos que foram alterados serão exibidos.</span><span class="sxs-lookup"><span data-stu-id="06a79-178">In the subsequent publications, only the files that have changed will be displayed.</span></span> <span data-ttu-id="06a79-179">Ver [atualizar o Site ao vivo: republicação](#update).</span><span class="sxs-lookup"><span data-stu-id="06a79-179">See [Updating the Live Site: Republishing](#update).</span></span>

![visualização de publicação](publishing/_static/image9.png)

<span data-ttu-id="06a79-181">Selecione **continuar**.</span><span class="sxs-lookup"><span data-stu-id="06a79-181">Select **Continue**.</span></span>

<span data-ttu-id="06a79-182">Depois que o site tiver sido implantado no Azure, será exibida uma mensagem que indica que a implantação for concluída.</span><span class="sxs-lookup"><span data-stu-id="06a79-182">After the site has been deployed to Azure, a message is displayed that indicates the deployment has completed.</span></span>

![publicação concluída](publishing/_static/image10.png)

<span data-ttu-id="06a79-184">O site e o banco de dados foi publicados no Azure e agora estão disponíveis ao público.</span><span class="sxs-lookup"><span data-stu-id="06a79-184">Your site and database have been published to Azure, and are now available to the public.</span></span> <span data-ttu-id="06a79-185">Clique no link na mensagem que indica a publicação estiver concluída e agora você verá seu site implantado.</span><span class="sxs-lookup"><span data-stu-id="06a79-185">Click the link in the message indicating publishing has completed, and you will now see your deployed site.</span></span> <span data-ttu-id="06a79-186">Você ou qualquer pessoa com acesso à Internet pode adicionar ou modificar registros no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="06a79-186">You or anyone with Internet access can add or modify records in the database.</span></span>

![](publishing/_static/image11.png)

<a id="host"></a>
## <a name="publishing-your-site-to-a-web-hosting-company"></a><span data-ttu-id="06a79-187">Publicar seu Site em uma empresa de hospedagem na Web</span><span class="sxs-lookup"><span data-stu-id="06a79-187">Publishing Your Site to a Web Hosting Company</span></span>

<span data-ttu-id="06a79-188">Se você decidir não publicar no Azure, você pode publicar em vez disso, seu site para uma empresa de hospedagem na web.</span><span class="sxs-lookup"><span data-stu-id="06a79-188">If you decide to not publish to Azure, you can instead publish your site to a web hosting company.</span></span>

<span data-ttu-id="06a79-189">Clique o **localizar host da web** link.</span><span class="sxs-lookup"><span data-stu-id="06a79-189">Click the **Find web hosting** link.</span></span>

!['Find hospedagem na web' botão na caixa de diálogo Configurações de publicação](publishing/_static/image12.png)

<span data-ttu-id="06a79-191">Ir para uma página no site da Microsoft que lista os provedores de hospedagem que dão suporte ao ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="06a79-191">You go to a page on the Microsoft site that lists hosting providers that support ASP.NET.</span></span>

![Página no site da Microsoft que lista os provedores de hospedagem](publishing/_static/image13.png)

<span data-ttu-id="06a79-193">Obviamente, pode ser difícil saber exatamente quais recursos de hospedagem que pode exigir a longo prazo.</span><span class="sxs-lookup"><span data-stu-id="06a79-193">Obviously, it can be difficult to know now exactly what hosting features you might require over the long term.</span></span> <span data-ttu-id="06a79-194">Aqui estão algumas coisas a considerar:</span><span class="sxs-lookup"><span data-stu-id="06a79-194">Here are a couple of things to consider:</span></span>

- <span data-ttu-id="06a79-195">Para fins do site WebPagesMovies, você não precisa ter um complemento separado para o SQL Server, que frequentemente custa extra.</span><span class="sxs-lookup"><span data-stu-id="06a79-195">For purposes of the WebPagesMovies site, you don't have to have a separate add-on for SQL Server, which often costs extra.</span></span> <span data-ttu-id="06a79-196">No seu site, você está usando o SQL Server Compact Edition, que é independente.</span><span class="sxs-lookup"><span data-stu-id="06a79-196">In your site, you're using SQL Server Compact Edition, which is self-contained.</span></span> <span data-ttu-id="06a79-197">No entanto, talvez seja necessário acesso ao SQL Server para algum trabalho futuro site que faria.</span><span class="sxs-lookup"><span data-stu-id="06a79-197">However, you might need SQL Server access for some future website work you do.</span></span> <span data-ttu-id="06a79-198">Se você achar que você pode, certifique-se de que você pode adicionar funcionalidade do SQL Server mais tarde.</span><span class="sxs-lookup"><span data-stu-id="06a79-198">If you think you might, make sure that you can add SQL Server capability later.</span></span>
- <span data-ttu-id="06a79-199">Verifique se o provedor de hospedagem oferece suporte ao protocolo de publicação de implantação da Web.</span><span class="sxs-lookup"><span data-stu-id="06a79-199">Check whether the hosting provider supports the Web Deploy publishing protocol.</span></span> <span data-ttu-id="06a79-200">Você pode publicar usando o protocolo FTP, mas é mais conveniente usar a implantação da Web.</span><span class="sxs-lookup"><span data-stu-id="06a79-200">You can publish by using FTP protocol, but it's more convenient to use Web Deploy.</span></span>

<span data-ttu-id="06a79-201">Alguns sites oferecem um período de avaliação gratuita.</span><span class="sxs-lookup"><span data-stu-id="06a79-201">Some sites offer a free trial period.</span></span> <span data-ttu-id="06a79-202">Uma avaliação gratuita é uma boa maneira de testar a publicação e de hospedagem enquanto você ainda está experimentando com o WebMatrix e páginas da Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="06a79-202">A free trial is a good way to try publishing and hosting while you're still experimenting with WebMatrix and ASP.NET Web Pages.</span></span>

<span data-ttu-id="06a79-203">Escolha um que você deseja.</span><span class="sxs-lookup"><span data-stu-id="06a79-203">Pick one that you like.</span></span> <span data-ttu-id="06a79-204">Para este tutorial, selecionamos DiscountASP.NET, porque enquanto estamos estivesse criando o tutorial, essa empresa tinha uma promoção que permitem que as pessoas hospede um site gratuito por alguns meses.</span><span class="sxs-lookup"><span data-stu-id="06a79-204">For this tutorial, we selected DiscountASP.NET, because while we were creating the tutorial, that company had a promotion that let people host a site free for a few months.</span></span>

> [!NOTE]
> <span data-ttu-id="06a79-205">Nossa escolha de um provedor de hospedagem para este tutorial não deve ser interpretada como um endosso dessa empresa sobre qualquer outro.</span><span class="sxs-lookup"><span data-stu-id="06a79-205">Our choice of a hosting provider for this tutorial shouldn't be interpreted as an endorsement of that company over any other.</span></span> <span data-ttu-id="06a79-206">Mas tivemos que escolher uma para fins ilustrativos e DiscountASP.NET é uma das muitas empresas que dá suporte a páginas da Web ASP.NET e o protocolo de implantação da Web para publicação.</span><span class="sxs-lookup"><span data-stu-id="06a79-206">But we had to pick one for illustration, and DiscountASP.NET is one of the many companies that supports ASP.NET Web Pages and the Web Deploy protocol for publishing.</span></span>


<span data-ttu-id="06a79-207">Normalmente, depois que você se inscrever com o provedor de hospedagem, a empresa envia um email que contém um nome de usuário e senha, a URL do servidor web e assim por diante.</span><span class="sxs-lookup"><span data-stu-id="06a79-207">Typically, after you've signed up with the hosting provider, the company sends you an email that contains a user name and password, the URL of the web server, and so on.</span></span> <span data-ttu-id="06a79-208">Se a empresa de hospedagem oferece suporte ao protocolo de implantação da Web, eles podem enviar você é um arquivo que contém as configurações de publicação ou permitem que você baixe um.</span><span class="sxs-lookup"><span data-stu-id="06a79-208">If the hosting company supports Web Deploy protocol, they might send you a file that contains publish settings, or let you download one.</span></span> <span data-ttu-id="06a79-209">Um arquivo de configurações de publicação simplifica o processo para você.</span><span class="sxs-lookup"><span data-stu-id="06a79-209">A publish settings file simplifies the process for you.</span></span>

<span data-ttu-id="06a79-210">Quando você se inscrever e está pronto para publicar, clique no **publicar** botão na faixa de opções do WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="06a79-210">When you've signed up and are ready to publish, click the **Publish** button in the WebMatrix ribbon.</span></span> <span data-ttu-id="06a79-211">O **configurações de publicação** caixa de diálogo é exibida.</span><span class="sxs-lookup"><span data-stu-id="06a79-211">The **Publish Settings** dialog box is displayed.</span></span>

<span data-ttu-id="06a79-212">Se o provedor de hospedagem enviado a você um arquivo de configurações de publicação, clique no **importar configurações de publicação** vincular e importe o arquivo.</span><span class="sxs-lookup"><span data-stu-id="06a79-212">If the hosting provider sent you a publish settings file, click the **Import publish settings** link and import the file.</span></span> <span data-ttu-id="06a79-213">Se você não tiver um arquivo de configurações de publicação, preencha os campos usando os valores que a empresa de hospedagem enviados a você por email.</span><span class="sxs-lookup"><span data-stu-id="06a79-213">If you don't have a publish settings file, fill in the fields by using the values that the hosting company sent you in email.</span></span> <span data-ttu-id="06a79-214">Aqui está o que o **configurações de publicação** caixa de diálogo pode ter aparência quando você terminar:</span><span class="sxs-lookup"><span data-stu-id="06a79-214">Here's what the **Publish Settings** dialog box might look like when you're done:</span></span>

![Configurações de publicação preenchido na caixa de diálogo 'Configurações de publicação'](publishing/_static/image14.png)

<span data-ttu-id="06a79-216">Clique em **validar Conexão**.</span><span class="sxs-lookup"><span data-stu-id="06a79-216">Click **Validate Connection**.</span></span> <span data-ttu-id="06a79-217">Se tudo estiver okey, a caixa de diálogo informa **conectado com êxito**, que significa que ele pode se comunicar com o servidor do provedor de hospedagem.</span><span class="sxs-lookup"><span data-stu-id="06a79-217">If everything is ok, the dialog box reports **Connected successfully**, which means it can communicate with the hosting provider's server.</span></span>

![Mensagem de êxito se publicar configurações estão corretas](publishing/_static/image15.png)

<span data-ttu-id="06a79-219">Se houver um problema, o WebMatrix faz o melhor para dizer o que é o problema:</span><span class="sxs-lookup"><span data-stu-id="06a79-219">If there's a problem, WebMatrix does its best to tell you what the problem is:</span></span>

![Mensagem de erro se não houver um problema com as configurações de publicação](publishing/_static/image16.png)

<span data-ttu-id="06a79-221">Clique em **salvar** para salvar suas configurações.</span><span class="sxs-lookup"><span data-stu-id="06a79-221">Click **Save** to save your settings.</span></span> <span data-ttu-id="06a79-222">O WebMatrix oferece para executar um teste para certificar-se de que ele possa se comunicar corretamente com o site de hospedagem:</span><span class="sxs-lookup"><span data-stu-id="06a79-222">WebMatrix offers to perform a test to make sure that it can communicate correctly with the hosting site:</span></span>

![Mensagem oferecendo para executar um teste do processo de publicação](publishing/_static/image17.png)

<span data-ttu-id="06a79-224">Clique em **Sim**.</span><span class="sxs-lookup"><span data-stu-id="06a79-224">Click **Yes**.</span></span> <span data-ttu-id="06a79-225">O WebMatrix carrega alguns arquivos de exemplo para o provedor de hospedagem.</span><span class="sxs-lookup"><span data-stu-id="06a79-225">WebMatrix uploads some sample files to the hosting provider.</span></span> <span data-ttu-id="06a79-226">Quando o teste de compatibilidade é concluído, o WebMatrix relata os resultados:</span><span class="sxs-lookup"><span data-stu-id="06a79-226">When the compatibility test is done, WebMatrix reports the results:</span></span>

![Resultados do teste de publicação](publishing/_static/image18.png)

<span data-ttu-id="06a79-228">Se você estiver pronto para começar, vá em frente e clique em **continuar** para iniciar o processo de publicação de verdade.</span><span class="sxs-lookup"><span data-stu-id="06a79-228">If you're ready to go, go ahead and click **Continue** to start the publish process for real.</span></span> <span data-ttu-id="06a79-229">O WebMatrix descobre quais arquivos estão em seu site e já estão no servidor de host (no momento, nenhum) e oferece uma visualização do processo de publicação:</span><span class="sxs-lookup"><span data-stu-id="06a79-229">WebMatrix figures out what files are in your site and are already on the host server (right now, none) and gives you a preview of the publish process:</span></span>

![Visualização dos quais arquivos que carregará o processo de publicação](publishing/_static/image19.png)

<span data-ttu-id="06a79-231">A lista de arquivos a serem publicados inclui páginas da web que você criou, como *Movies.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="06a79-231">The list of files to publish includes the web pages that you've created like *Movies.cshtml*.</span></span> <span data-ttu-id="06a79-232">A lista também inclui arquivos para os auxiliares que você tenha instalado, os arquivos para executar o SQL Server Compact Edition para seu banco de dados e assim por diante.</span><span class="sxs-lookup"><span data-stu-id="06a79-232">The list also includes files for helpers that you've installed, the files to run SQL Server Compact Edition for your database, and so on.</span></span> <span data-ttu-id="06a79-233">Como resultado, o processo de publicação inicial pode ser significativa.</span><span class="sxs-lookup"><span data-stu-id="06a79-233">As a result, the initial publish process can be substantial.</span></span>

<span data-ttu-id="06a79-234">Clique em **Continue**.</span><span class="sxs-lookup"><span data-stu-id="06a79-234">Click **Continue**.</span></span> <span data-ttu-id="06a79-235">O WebMatrix copia os arquivos ao servidor do provedor de hospedagem.</span><span class="sxs-lookup"><span data-stu-id="06a79-235">WebMatrix copies your files to the hosting provider's server.</span></span> <span data-ttu-id="06a79-236">Quando estiver pronto, os resultados são relatados na barra de status:</span><span class="sxs-lookup"><span data-stu-id="06a79-236">When it's done, the results are reported in the status bar:</span></span>

![Mensagem de barra de status quando o processo de publicação foi concluída com êxito](publishing/_static/image20.png)

<span data-ttu-id="06a79-238">Para ver seu site ativo, clique no link na barra de status.</span><span class="sxs-lookup"><span data-stu-id="06a79-238">To see your live site, click the link in the status bar.</span></span> <span data-ttu-id="06a79-239">Adicione *filmes* para a URL, e você verá o *Movies.cshtml* arquivo que você criou:</span><span class="sxs-lookup"><span data-stu-id="06a79-239">Add *Movies* to the URL, and you'll see the *Movies.cshtml* file that you created:</span></span>

![O site ao vivo mostrando a página de filmes](publishing/_static/image21.png)

<a id="update"></a>
## <a name="updating-the-live-site-republishing"></a><span data-ttu-id="06a79-241">Atualizar o Site ao vivo: republicação</span><span class="sxs-lookup"><span data-stu-id="06a79-241">Updating the Live Site: Republishing</span></span>

<span data-ttu-id="06a79-242">Depois de publicar seu site (para Azure ou em uma empresa de hospedagem na web), há duas cópias dele &mdash; a versão em seu computador e a versão do provedor de serviços.</span><span class="sxs-lookup"><span data-stu-id="06a79-242">Once you've published your site (to either Azure or a web hosting company), there are two copies of it &mdash; the version on your computer and the version on the service provider.</span></span> <span data-ttu-id="06a79-243">Provavelmente você desejará continuar a desenvolver o site (não bastasse, como parte do próximo conjunto de tutoriais).</span><span class="sxs-lookup"><span data-stu-id="06a79-243">You'll probably want to continue developing the site (if nothing else, as part of the next tutorial set).</span></span> <span data-ttu-id="06a79-244">Quando você fizer isso, você precisará publicar novamente seu site para copiar as alterações do seu computador para o provedor de serviços.</span><span class="sxs-lookup"><span data-stu-id="06a79-244">When you do, you have to republish your site in order to copy changes from your computer to the service provider.</span></span> <span data-ttu-id="06a79-245">O processo de publicação no WebMatrix pode determinar quais arquivos foram alterados em seu site e publicar apenas os arquivos.</span><span class="sxs-lookup"><span data-stu-id="06a79-245">The publish process in WebMatrix can determine what files have changed on your site and publish just those files.</span></span>

<span data-ttu-id="06a79-246">Para ver como funciona a republicação, abra o *Movies.cshtml* do site, faça algumas pequenas alterações e, em seguida, salve o arquivo.</span><span class="sxs-lookup"><span data-stu-id="06a79-246">To see how republishing works, open the *Movies.cshtml* site, make some small change, and then save the file.</span></span> <span data-ttu-id="06a79-247">Por exemplo, altere o título para `Movies - Updated`.</span><span class="sxs-lookup"><span data-stu-id="06a79-247">For example, change the title to `Movies - Updated`.</span></span>

<span data-ttu-id="06a79-248">Clique o **publicar** botão na faixa de opções.</span><span class="sxs-lookup"><span data-stu-id="06a79-248">Click the **Publish** button in the ribbon.</span></span> <span data-ttu-id="06a79-249">O WebMatrix determina o que é alterado e exibe uma visualização dos arquivos, que ele publicará.</span><span class="sxs-lookup"><span data-stu-id="06a79-249">WebMatrix determines what's changed and shows you a preview of the files it will publish.</span></span>

![A caixa de diálogo 'Publicar' mostrando alterados arquivos prontos para republicação](publishing/_static/image22.png)

> [!IMPORTANT] 
> 
> <span data-ttu-id="06a79-251">Por padrão, o WebMatrix publica seu banco de dados (*sdf* arquivo) somente na primeira vez que você publicar o site.</span><span class="sxs-lookup"><span data-stu-id="06a79-251">By default, WebMatrix publishes your database (*.sdf* file) only the first time you publish the site.</span></span> <span data-ttu-id="06a79-252">Depois que o site é publicado e as pessoas estão interagindo com o site, o banco de dados no site ativo normalmente tem dados reais do site.</span><span class="sxs-lookup"><span data-stu-id="06a79-252">Once your site is published and people are interacting with the website, the database on the live site typically has the site's real data.</span></span> <span data-ttu-id="06a79-253">Você precisa ter muito cuidado para não substituir o banco de dados ao vivo com o *sdf* arquivo no seu computador, que geralmente contém somente os dados de teste.</span><span class="sxs-lookup"><span data-stu-id="06a79-253">You have to be very careful not to overwrite the live database with the *.sdf* file that's on your computer, which usually contains only test data.</span></span> <span data-ttu-id="06a79-254">É por isso que você veja o aviso **publicação irá substituir quaisquer bancos de dados remotos**, e por que a caixa de seleção *WebPagesMovies.sdf* está desmarcada por padrão.</span><span class="sxs-lookup"><span data-stu-id="06a79-254">That's why you see the warning **Publishing will overwrite any remote databases**, and why the check box for *WebPagesMovies.sdf* is cleared by default.</span></span>


<span data-ttu-id="06a79-255">Clique em **Continue**.</span><span class="sxs-lookup"><span data-stu-id="06a79-255">Click **Continue**.</span></span> <span data-ttu-id="06a79-256">O WebMatrix publica os arquivos alterados e mostra uma mensagem de êxito, como ele fez na primeira vez que você publicou.</span><span class="sxs-lookup"><span data-stu-id="06a79-256">WebMatrix publishes the changed files and shows you a success message, like it did the first time you published.</span></span>

<span data-ttu-id="06a79-257">Vá para o site ativo (você pode clicar no link na mensagem de êxito, se ele ainda está mostrando) e verifique se que a alteração foi publicada.</span><span class="sxs-lookup"><span data-stu-id="06a79-257">Go to the live site (you can click the link in the success message if it's still showing) and verify that your change has been published.</span></span>

> [!TIP] 
> 
> <span data-ttu-id="06a79-258">**Editar arquivos remotamente**</span><span class="sxs-lookup"><span data-stu-id="06a79-258">**Editing Files Remotely**</span></span>
> 
> <span data-ttu-id="06a79-259">Como alternativa à alteração de seu site e, em seguida, republicar, você pode editar os arquivos remotos diretamente no WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="06a79-259">As an alternative to changing your site and then republishing, you can edit remote files directly in WebMatrix.</span></span> <span data-ttu-id="06a79-260">Nesse cenário, você abre um arquivo que é o provedor de serviço e o WebMatrix baixa uma cópia para edição.</span><span class="sxs-lookup"><span data-stu-id="06a79-260">In this scenario, you open a file that's on the service provider, and WebMatrix downloads a copy of it for you to edit.</span></span> <span data-ttu-id="06a79-261">Sempre que você salvar o arquivo, o WebMatrix envia as alterações para o site.</span><span class="sxs-lookup"><span data-stu-id="06a79-261">Every time you save the file, WebMatrix sends the changes to the site.</span></span>
> 
> <span data-ttu-id="06a79-262">Edição remota é uma maneira fácil de fazer alterações em seu site ativo.</span><span class="sxs-lookup"><span data-stu-id="06a79-262">Remote editing is an easy way to make changes to your live site.</span></span> <span data-ttu-id="06a79-263">No entanto, as alterações feitas dessa maneira não estão sincronizadas com os arquivos em seu site local.</span><span class="sxs-lookup"><span data-stu-id="06a79-263">However, the changes you make this way aren't synchronized with the files in your local site.</span></span> <span data-ttu-id="06a79-264">Para sincronizar os arquivos locais com o site remoto, você pode baixar os arquivos remotos.</span><span class="sxs-lookup"><span data-stu-id="06a79-264">To synchronize the local files with the remote site, you can download the remote files.</span></span> <span data-ttu-id="06a79-265">Esse processo funciona muito parecida com a publicação, exceto na ordem inversa.</span><span class="sxs-lookup"><span data-stu-id="06a79-265">This process works much like publishing, except in reverse.</span></span>
> 
> <span data-ttu-id="06a79-266">Não descreveremos mais sobre os recursos de edição remota e remote-download do WebMatrix aqui.</span><span class="sxs-lookup"><span data-stu-id="06a79-266">We won't describe more about the remote-editing and remote-download facilities of WebMatrix here.</span></span> <span data-ttu-id="06a79-267">Eles são bastante úteis se várias pessoas precisam trabalhar no mesmo site em computadores diferentes.</span><span class="sxs-lookup"><span data-stu-id="06a79-267">They're quite useful if multiple people have to work on the same site on different computers.</span></span> <span data-ttu-id="06a79-268">Para obter mais informações, consulte [publicar e editar um local remoto com o WebMatrix 2 Beta](https://go.microsoft.com/fwlink/?LinkId=251591).</span><span class="sxs-lookup"><span data-stu-id="06a79-268">For more information, see [Publish and Edit a Remote Site with WebMatrix 2 Beta](https://go.microsoft.com/fwlink/?LinkId=251591).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="06a79-269">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="06a79-269">Additional Resources</span></span>

- <span data-ttu-id="06a79-270">[Fórum do WebMatrix ASP.NET Web Pages ASP.NET](https://forums.asp.net/1224.aspx/1?WebMatrix+and+ASP+NET+Web+Pages), um ótimo lugar para postar perguntas e obtenha respostas.</span><span class="sxs-lookup"><span data-stu-id="06a79-270">[ASP.NET WebMatrix ASP.NET Web Pages forum](https://forums.asp.net/1224.aspx/1?WebMatrix+and+ASP+NET+Web+Pages), a great place to post questions and get answers.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="06a79-271">Anterior</span><span class="sxs-lookup"><span data-stu-id="06a79-271">Previous</span></span>](layouts.md)
