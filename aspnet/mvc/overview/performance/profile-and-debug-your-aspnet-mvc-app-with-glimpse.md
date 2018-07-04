---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: Analisar e depurar seu aplicativo ASP.NET MVC com Glimpse | Microsoft Docs
author: Rick-Anderson
description: Visão rápida é prosperando e aumentando a família de pacotes do NuGet de software livre que fornece desempenho detalhados, depuração e informações de diagnóstico para o ASP.NET um...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: ef65b512645d3f013ea34d3557da93254425c419
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391064"
---
<a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a><span data-ttu-id="52af7-103">Analisar e depurar seu aplicativo ASP.NET MVC com Glimpse</span><span class="sxs-lookup"><span data-stu-id="52af7-103">Profile and debug your ASP.NET MVC app with Glimpse</span></span>
====================
<span data-ttu-id="52af7-104">por [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="52af7-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="52af7-105">Visão rápida é prosperando e aumentando a família de pacotes do NuGet de software livre que fornece desempenho detalhados, depuração e informações de diagnóstico para aplicativos ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="52af7-105">Glimpse is a thriving and growing family of open source NuGet packages that provides detailed performance, debugging and diagnostic information for ASP.NET apps.</span></span> <span data-ttu-id="52af7-106">Ele é trivial para instalar, leve, com rapidez e exibe as principais métricas de desempenho na parte inferior de cada página.</span><span class="sxs-lookup"><span data-stu-id="52af7-106">It's trivial to install, lightweight, ultra-fast, and displays key performance metrics at the bottom of every page.</span></span> <span data-ttu-id="52af7-107">Ele permite fazer uma busca detalhada em seu aplicativo quando você precisa descobrir o que está acontecendo no servidor.</span><span class="sxs-lookup"><span data-stu-id="52af7-107">It allows you to drill down into your app when you need to find out what's going on at the server.</span></span> <span data-ttu-id="52af7-108">Visão rápida fornece informações muito valiosas que é recomendável que usá-lo em todo seu ciclo de desenvolvimento, incluindo o seu ambiente de teste do Azure.</span><span class="sxs-lookup"><span data-stu-id="52af7-108">Glimpse provides so much valuable information we recommend you use it throughout your development cycle, including your Azure test environment.</span></span> <span data-ttu-id="52af7-109">Enquanto [Fiddler](http://www.telerik.com/fiddler) e o [ferramentas de desenvolvimento F-12](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) fornecem um cliente modo de exibição, a visão rápida fornece uma exibição detalhada do servidor.</span><span class="sxs-lookup"><span data-stu-id="52af7-109">While [Fiddler](http://www.telerik.com/fiddler) and the [F-12 development tools](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) provide a client side view, Glimpse provides a detailed view from the server.</span></span> <span data-ttu-id="52af7-110">Este tutorial se concentrará em usando o ASP.NET MVC de amostra e pacotes do EF, mas muitos outros pacotes estão disponíveis.</span><span class="sxs-lookup"><span data-stu-id="52af7-110">This tutorial will focus on using the Glimpse ASP.NET MVC and EF packages, but many other packages are available.</span></span> <span data-ttu-id="52af7-111">Sempre que possível, eu vinculará a apropriado [dê uma olhada docs](http://getglimpse.com/Docs/) que ajudam a manter.</span><span class="sxs-lookup"><span data-stu-id="52af7-111">Where possible I will link to the appropriate [Glimpse docs](http://getglimpse.com/Docs/) which I help maintain.</span></span> <span data-ttu-id="52af7-112">Visão rápida é um projeto de código-fonte aberto, você também pode contribuir com o código-fonte e os documentos.</span><span class="sxs-lookup"><span data-stu-id="52af7-112">Glimpse is an open source project, you too can contribute to the source code and the docs.</span></span>


- [<span data-ttu-id="52af7-113">Instalando a amostra</span><span class="sxs-lookup"><span data-stu-id="52af7-113">Installing Glimpse</span></span>](#ig)
- [<span data-ttu-id="52af7-114">Habilitar Glimpse para localhost</span><span class="sxs-lookup"><span data-stu-id="52af7-114">Enable Glimpse for localhost</span></span>](#eg)
- [<span data-ttu-id="52af7-115">Na guia da linha do tempo</span><span class="sxs-lookup"><span data-stu-id="52af7-115">The Timeline tab</span></span>](#Time)
- [<span data-ttu-id="52af7-116">Associação de modelos</span><span class="sxs-lookup"><span data-stu-id="52af7-116">Model Binding</span></span>](#mb)
- [<span data-ttu-id="52af7-117">Rotas</span><span class="sxs-lookup"><span data-stu-id="52af7-117">Routes</span></span>](#route)
- [<span data-ttu-id="52af7-118">Usando a amostra no Azure</span><span class="sxs-lookup"><span data-stu-id="52af7-118">Using Glimpse on Azure</span></span>](#da)
- [<span data-ttu-id="52af7-119">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="52af7-119">Additional Resources</span></span>](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a><span data-ttu-id="52af7-120">Instalando a amostra</span><span class="sxs-lookup"><span data-stu-id="52af7-120">Installing Glimpse</span></span>

<span data-ttu-id="52af7-121">Você pode instalar Glimpse partir do console do Gerenciador de pacotes NuGet ou o **gerenciar pacotes NuGet** console.</span><span class="sxs-lookup"><span data-stu-id="52af7-121">You can install Glimpse from the NuGet package manager console or from the **Manage NuGet Packages** console.</span></span> <span data-ttu-id="52af7-122">Para esta demonstração, vou instalar os pacotes Mvc5 e EF6:</span><span class="sxs-lookup"><span data-stu-id="52af7-122">For this demo, I'll install the Mvc5 and EF6 packages:</span></span>

![instalar a prévia do NuGet Dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

<span data-ttu-id="52af7-124">Pesquise *Glimpse.EF*</span><span class="sxs-lookup"><span data-stu-id="52af7-124">Search for *Glimpse.EF*</span></span>

![Glimpse.EF do NuGet install dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

<span data-ttu-id="52af7-126">Selecionando **pacotes instalados**, você pode ver os módulos dependentes do Glimpse instalados:</span><span class="sxs-lookup"><span data-stu-id="52af7-126">By selecting **Installed packages**, you can see the Glimpse dependent modules installed:</span></span>

![Os pacotes de prévia instalados de DLg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

<span data-ttu-id="52af7-128">Os seguintes comandos instalam os módulos de visão rápida MVC5 e o EF6 no console do Gerenciador de pacote:</span><span class="sxs-lookup"><span data-stu-id="52af7-128">The following commands install Glimpse MVC5 and EF6 modules from the package manager console:</span></span>

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a><span data-ttu-id="52af7-129">Habilitar Glimpse para localhost</span><span class="sxs-lookup"><span data-stu-id="52af7-129">Enable Glimpse for localhost</span></span>

<span data-ttu-id="52af7-130">Navegue até http://localhost:&lt; a porta n º&gt;/glimpse.axd e clique no <strong>ativar Glimpse</strong> botão.</span><span class="sxs-lookup"><span data-stu-id="52af7-130">Navigate to http://localhost:&lt;port #&gt;/glimpse.axd and click the <strong>Turn Glimpse On</strong> button.</span></span>

![Página de axd visão rápida](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

<span data-ttu-id="52af7-132">Se você tiver sua barra de favoritos exibida, você pode arrastar e soltar os botões de amostra e adicioná-los como bookmarklets:</span><span class="sxs-lookup"><span data-stu-id="52af7-132">If you have your favorites bar displayed, you can drag and drop the Glimpse buttons and add them as bookmarklets:</span></span>

![IE com Glimpse boookmarklets](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

<span data-ttu-id="52af7-134">Agora, você pode navegar seu aplicativo e o **cabeças backup exibição** (HUD) é mostrada na parte inferior da página.</span><span class="sxs-lookup"><span data-stu-id="52af7-134">You can now navigate your app, and the **Heads Up Display** (HUD) is shown at the bottom of the page.</span></span>

![Página do Gerenciador de contatos com HUD](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

<span data-ttu-id="52af7-136">O [página de visão rápida HUD](http://getglimpse.com/Docs/Heads-up-Display) fornece detalhes sobre as informações de medição de tempo mostradas acima.</span><span class="sxs-lookup"><span data-stu-id="52af7-136">The [Glimpse HUD page](http://getglimpse.com/Docs/Heads-up-Display) details the timing information shown above.</span></span> <span data-ttu-id="52af7-137">As exibições de dados o HUD desempenho discreto podem notificá-lo de um problema imediatamente - antes de chegar ao ciclo de teste.</span><span class="sxs-lookup"><span data-stu-id="52af7-137">The unobtrusive performance data the HUD displays can notify you of a problem immediately - before you get to the test cycle.</span></span> <span data-ttu-id="52af7-138">Clicar na &quot;g&quot; no canto inferior direito abre o painel de visão rápida:</span><span class="sxs-lookup"><span data-stu-id="52af7-138">Clicking on the &quot;g&quot; in the lower right corner brings up the Glimpse panel:</span></span>

![Painel de visão rápida](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

<span data-ttu-id="52af7-140">Na imagem acima, o [guia de execução](http://getglimpse.com/Docs/Execution-Tab) for selecionado, que mostra detalhes de medição de tempo das ações e os filtros no pipeline.</span><span class="sxs-lookup"><span data-stu-id="52af7-140">In the image above, the [Execution tab](http://getglimpse.com/Docs/Execution-Tab) is selected, which shows timing details of the actions and filters in the pipeline.</span></span> <span data-ttu-id="52af7-141">Você pode ver minha [temporizador de filtro de inspeção parar](http://www.nuget.org/packages/StopWatch/) começam em 6 de estágio do pipeline.</span><span class="sxs-lookup"><span data-stu-id="52af7-141">You can see my [Stop Watch filter timer](http://www.nuget.org/packages/StopWatch/) start at stage 6 of the pipeline.</span></span> <span data-ttu-id="52af7-142">Embora minha temporizador leve possa fornecer útil perfil/dados de tempo, ele perde todo o tempo gasto na autorização e o modo de exibição de renderização.</span><span class="sxs-lookup"><span data-stu-id="52af7-142">While my light weight timer can provide useful profile/timing data, it misses all the time spent in authorization and rendering the view.</span></span> <span data-ttu-id="52af7-143">Você pode ler sobre meu timer em [criar o perfil e a hora de seu aplicativo ASP.NET MVC todo o caminho para o Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span><span class="sxs-lookup"><span data-stu-id="52af7-143">You can read about my timer at [Profile and Time your ASP.NET MVC app all the way to Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span></span> <span data-ttu-id="52af7-144">O [guias](http://getglimpse.com/Docs/Tabs) página fornece links para informações detalhadas sobre cada guia.</span><span class="sxs-lookup"><span data-stu-id="52af7-144">The [Tabs](http://getglimpse.com/Docs/Tabs) page provides links to detailed information on each tab.</span></span>

<a id="Time"></a>
## <a name="the-timeline-tab"></a><span data-ttu-id="52af7-145">Na guia da linha do tempo</span><span class="sxs-lookup"><span data-stu-id="52af7-145">The Timeline tab</span></span>

<span data-ttu-id="52af7-146">Eu modifiquei de Tom Dykstra pendentes [tutorial do EF 6/MVC 5](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) com o código a seguir, altere para o controlador instrutores:</span><span class="sxs-lookup"><span data-stu-id="52af7-146">I've modified Tom Dykstra's outstanding [EF 6/MVC 5 tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) with the following code change to the instructors controller:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

<span data-ttu-id="52af7-147">O código acima permite que eu passe na cadeia de caracteres de consulta (`eager`) para controlar adiantado ou explícito, carregamento de dados.</span><span class="sxs-lookup"><span data-stu-id="52af7-147">The code above allows me to pass in query string (`eager`) to control eager or explicit loading of data.</span></span> <span data-ttu-id="52af7-148">Na imagem abaixo, o carregamento explícito é usado e a página de medição de tempo mostra cada registro carregado no `Index` método de ação:</span><span class="sxs-lookup"><span data-stu-id="52af7-148">In the image below, explicit loading is used and the timing page shows each enrollment loaded in the `Index` action method:</span></span>

![carregamento explícito](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

<span data-ttu-id="52af7-150">No código a seguir, adiantado é especificado, e cada registro é buscado após o `Index` é chamado de modo de exibição:</span><span class="sxs-lookup"><span data-stu-id="52af7-150">In the following code, eager is specified, and each enrollment is fetched after the `Index` view is called:</span></span>

![adiantado é especificado](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

<span data-ttu-id="52af7-152">Você pode passar o mouse sobre um segmento de tempo para obter informações detalhadas de tempo:</span><span class="sxs-lookup"><span data-stu-id="52af7-152">You can hover over a time segment to get detailed timing information:</span></span>

![Passe o mouse para ver o tempo detalhados](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a><span data-ttu-id="52af7-154">Associação de modelos</span><span class="sxs-lookup"><span data-stu-id="52af7-154">Model Binding</span></span>

<span data-ttu-id="52af7-155">O [guia de associação de modelo](http://getglimpse.com/Docs/Model-Binding-Tab) fornece uma grande quantidade de informações para ajudá-lo a entender como as variáveis de formulário são associadas e por que alguns não são associados conforme o esperado.</span><span class="sxs-lookup"><span data-stu-id="52af7-155">The [model binding tab](http://getglimpse.com/Docs/Model-Binding-Tab) provides a wealth of information to help you understand how your form variables are bound and why some are not bound as would expect.</span></span> <span data-ttu-id="52af7-156">A imagem abaixo mostra o **?**</span><span class="sxs-lookup"><span data-stu-id="52af7-156">The image below shows the **?**</span></span> <span data-ttu-id="52af7-157">ícone, você poderá clicar para abrir a página de ajuda de amostra para esse recurso.</span><span class="sxs-lookup"><span data-stu-id="52af7-157">icon, which you can click on to bring up the glimpse help page for that feature.</span></span>

![Dê uma olhada a exibição do modelo de associação](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a><span data-ttu-id="52af7-159">Rotas</span><span class="sxs-lookup"><span data-stu-id="52af7-159">Routes</span></span>

 <span data-ttu-id="52af7-160">Guia de visão rápida de rotas pode ajudará você depurar e entender o roteamento.</span><span class="sxs-lookup"><span data-stu-id="52af7-160">The Glimpse Routes tab will can help you debug and understand routing.</span></span> <span data-ttu-id="52af7-161">Na imagem abaixo, a rota de produto é selecionada (e aparece em verde, uma convenção de visão rápida).</span><span class="sxs-lookup"><span data-stu-id="52af7-161">In the image below, the product route is selected (and it shows in green, a Glimpse convention).</span></span> <span data-ttu-id="52af7-162">![nome do produto selecionado](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) tokens de dados, áreas e restrições de rota também são exibidos.</span><span class="sxs-lookup"><span data-stu-id="52af7-162">![product name selected](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) Route constraints, Areas and data tokens are also displayed.</span></span> <span data-ttu-id="52af7-163">Ver [rotas Glimpse](http://getglimpse.com/Docs/Routes-Tab) e [roteamento de atributo no ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="52af7-163">See [Glimpse Routes](http://getglimpse.com/Docs/Routes-Tab) and [Attribute Routing in ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) for more information.</span></span> 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a><span data-ttu-id="52af7-164">Usando a amostra no Azure</span><span class="sxs-lookup"><span data-stu-id="52af7-164">Using Glimpse on Azure</span></span>

<span data-ttu-id="52af7-165">A política de segurança padrão Glimpse permite apenas dados de amostra a serem exibidos no host local.</span><span class="sxs-lookup"><span data-stu-id="52af7-165">The Glimpse default security policy only allows Glimpse data to be displayed from local host.</span></span> <span data-ttu-id="52af7-166">Você pode alterar essa política de segurança para que você possa exibir esses dados em um servidor remoto (por exemplo, um aplicativo web no Azure).</span><span class="sxs-lookup"><span data-stu-id="52af7-166">You can change this security policy so you can view this data on a remote server (such as a web app on Azure).</span></span> <span data-ttu-id="52af7-167">Para ambientes de teste no Azure, adicione a marca realçada até a parte inferior da *web.confg* arquivo para habilitar a visão rápida:</span><span class="sxs-lookup"><span data-stu-id="52af7-167">For test environments on Azure, add the highlighted mark up to the bottom of the *web.confg* file to enable Glimpse:</span></span>

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

<span data-ttu-id="52af7-168">Com essa alteração sozinha, qualquer usuário pode ver os dados de amostra em um site remoto.</span><span class="sxs-lookup"><span data-stu-id="52af7-168">With this change alone, any user can see your Glimpse data on a remote site.</span></span> <span data-ttu-id="52af7-169">Considere adicionar a marcação acima a um perfil de publicação para que ele tem apenas implantado um aplicada quando você usa esse perfil de publicação (por exemplo, seu proifle de teste do Azure.) Para restringir os dados de amostra, adicionaremos o `canViewGlimpseData` função e só permitir que os usuários nessa função para exibir dados de amostra.</span><span class="sxs-lookup"><span data-stu-id="52af7-169">Consider adding the markup above to a publish profile so it's only deployed an applyed when you use that publish profile (for example, your Azure test proifle.) To restrict Glimpse data, we will add the `canViewGlimpseData` role and only allow users in this role to view Glimpse data.</span></span>

<span data-ttu-id="52af7-170">Remova os comentários do *GlimpseSecurityPolicy.cs* do arquivo e altere o [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) chamar de `Administrator` para o `canViewGlimpseData` função:</span><span class="sxs-lookup"><span data-stu-id="52af7-170">Remove the comments from the *GlimpseSecurityPolicy.cs* file and change the [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) call from `Administrator` to the `canViewGlimpseData` role:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> <span data-ttu-id="52af7-171">Security - dados avançados fornecidos pelo ideia poderia expor a segurança do seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="52af7-171">Security - The rich data provided by Glimpse could expose the security of your app.</span></span> <span data-ttu-id="52af7-172">Microsoft não realizou uma auditoria de segurança do Glimpse para uso em aplicativos de produções.</span><span class="sxs-lookup"><span data-stu-id="52af7-172">Microsoft has not performed a security audit of Glimpse for use on productions apps.</span></span>


<span data-ttu-id="52af7-173">Para obter informações sobre como adicionar funções, consulte minha [implantar um aplicativo da web ASP.NET MVC 5 seguro com associação, OAuth e banco de dados SQL no Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) tutorial.</span><span class="sxs-lookup"><span data-stu-id="52af7-173">For information on adding roles, see my [Deploy a Secure ASP.NET MVC 5 web app with Membership, OAuth, and SQL Database to Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) tutorial.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="52af7-174">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="52af7-174">Additional Resources</span></span>

- [<span data-ttu-id="52af7-175">Implantar um aplicativo ASP.NET MVC 5 seguro com associação, OAuth e banco de dados SQL do Azure</span><span class="sxs-lookup"><span data-stu-id="52af7-175">Deploy a Secure ASP.NET MVC 5 app with Membership, OAuth, and SQL Database to Azure</span></span>](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- <span data-ttu-id="52af7-176">[Dê uma olhada configuração](http://getglimpse.com/Docs/Configuration) -página de documentação sobre como configurar as guias, política de tempo de execução, registro em log e muito mais.</span><span class="sxs-lookup"><span data-stu-id="52af7-176">[Glimpse Configuration](http://getglimpse.com/Docs/Configuration) - Doc page on configuring tabs, runtime policy, logging and more.</span></span>
