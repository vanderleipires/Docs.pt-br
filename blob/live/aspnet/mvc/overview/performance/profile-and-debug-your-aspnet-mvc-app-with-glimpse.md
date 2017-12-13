---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: "Criar o perfil e depurar seu aplicativo ASP.NET MVC com prévia | Microsoft Docs"
author: Rick-Anderson
description: "Visão rápida é um prosperando e aumentando a família de pacotes do NuGet código-fonte aberto que fornece desempenho detalhada, depuração e informações de diagnóstico para o ASP.NET um..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: 98b21a54ba00a8c82c3be7ba4e39d44041ed42c6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a><span data-ttu-id="f8152-103">Criar o perfil e depurar seu aplicativo ASP.NET MVC com prévia</span><span class="sxs-lookup"><span data-stu-id="f8152-103">Profile and debug your ASP.NET MVC app with Glimpse</span></span>
====================
<span data-ttu-id="f8152-104">Por [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="f8152-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="f8152-105">Visão rápida é um prosperando e aumentando a família de pacotes do NuGet código-fonte aberto que fornece desempenho detalhada, depuração e informações de diagnóstico para aplicativos ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f8152-105">Glimpse is a thriving and growing family of open source NuGet packages that provides detailed performance, debugging and diagnostic information for ASP.NET apps.</span></span> <span data-ttu-id="f8152-106">Ele é simples de instalar, leve, ultrarrápido e exibe as principais métricas de desempenho na parte inferior de cada página.</span><span class="sxs-lookup"><span data-stu-id="f8152-106">It's trivial to install, lightweight, ultra-fast, and displays key performance metrics at the bottom of every page.</span></span> <span data-ttu-id="f8152-107">Ele permite que você fazer drill down em seu aplicativo quando você precisa saber o que está acontecendo no servidor.</span><span class="sxs-lookup"><span data-stu-id="f8152-107">It allows you to drill down into your app when you need to find out what's going on at the server.</span></span> <span data-ttu-id="f8152-108">Visão rápida fornece muito valiosas informações, que recomendamos que você pode usá-lo durante o ciclo de desenvolvimento, incluindo o seu ambiente de teste do Azure.</span><span class="sxs-lookup"><span data-stu-id="f8152-108">Glimpse provides so much valuable information we recommend you use it throughout your development cycle, including your Azure test environment.</span></span> <span data-ttu-id="f8152-109">Enquanto [Fiddler](http://www.telerik.com/fiddler) e [ferramentas de desenvolvimento F-12](https://msdn.microsoft.com/en-us/library/ie/gg589512(v=vs.85).aspx) fornecem um lado do cliente exibição, a visão rápida fornece uma exibição detalhada do servidor.</span><span class="sxs-lookup"><span data-stu-id="f8152-109">While [Fiddler](http://www.telerik.com/fiddler) and the [F-12 development tools](https://msdn.microsoft.com/en-us/library/ie/gg589512(v=vs.85).aspx) provide a client side view, Glimpse provides a detailed view from the server.</span></span> <span data-ttu-id="f8152-110">Este tutorial se concentrará em usando o ASP.NET MVC de amostra e pacotes EF, mas muitos outros pacotes estão disponíveis.</span><span class="sxs-lookup"><span data-stu-id="f8152-110">This tutorial will focus on using the Glimpse ASP.NET MVC and EF packages, but many other packages are available.</span></span> <span data-ttu-id="f8152-111">Sempre que possível, será vinculado ao apropriado [vislumbrar documentos](http://getglimpse.com/Docs/) que ajudam a manter.</span><span class="sxs-lookup"><span data-stu-id="f8152-111">Where possible I will link to the appropriate [Glimpse docs](http://getglimpse.com/Docs/) which I help maintain.</span></span> <span data-ttu-id="f8152-112">Visão rápida é um projeto, você também pode contribuir com o código-fonte e os documentos.</span><span class="sxs-lookup"><span data-stu-id="f8152-112">Glimpse is an open source project, you too can contribute to the source code and the docs.</span></span>


- [<span data-ttu-id="f8152-113">Instalação prévia</span><span class="sxs-lookup"><span data-stu-id="f8152-113">Installing Glimpse</span></span>](#ig)
- [<span data-ttu-id="f8152-114">Habilitar prévia para localhost</span><span class="sxs-lookup"><span data-stu-id="f8152-114">Enable Glimpse for localhost</span></span>](#eg)
- [<span data-ttu-id="f8152-115">Na guia Cronograma</span><span class="sxs-lookup"><span data-stu-id="f8152-115">The Timeline tab</span></span>](#Time)
- [<span data-ttu-id="f8152-116">Associação de modelos</span><span class="sxs-lookup"><span data-stu-id="f8152-116">Model Binding</span></span>](#mb)
- [<span data-ttu-id="f8152-117">Rotas</span><span class="sxs-lookup"><span data-stu-id="f8152-117">Routes</span></span>](#route)
- [<span data-ttu-id="f8152-118">Usando a amostra no Azure</span><span class="sxs-lookup"><span data-stu-id="f8152-118">Using Glimpse on Azure</span></span>](#da)
- [<span data-ttu-id="f8152-119">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="f8152-119">Additional Resources</span></span>](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a><span data-ttu-id="f8152-120">Instalação prévia</span><span class="sxs-lookup"><span data-stu-id="f8152-120">Installing Glimpse</span></span>

<span data-ttu-id="f8152-121">Você pode instalar prévia de console do Gerenciador de pacote do NuGet ou do **gerenciar pacotes NuGet** console.</span><span class="sxs-lookup"><span data-stu-id="f8152-121">You can install Glimpse from the NuGet package manager console or from the **Manage NuGet Packages** console.</span></span> <span data-ttu-id="f8152-122">Para esta demonstração, eu instalará os pacotes Mvc5 e EF6:</span><span class="sxs-lookup"><span data-stu-id="f8152-122">For this demo, I'll install the Mvc5 and EF6 packages:</span></span>

![instalar prévia do NuGet Dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

<span data-ttu-id="f8152-124">Procurar *Glimpse.EF*</span><span class="sxs-lookup"><span data-stu-id="f8152-124">Search for *Glimpse.EF*</span></span>

![Glimpse.EF do NuGet instalação dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

<span data-ttu-id="f8152-126">Selecionando **pacotes instalados**, você pode ver os módulos dependentes prévia instalados:</span><span class="sxs-lookup"><span data-stu-id="f8152-126">By selecting **Installed packages**, you can see the Glimpse dependent modules installed:</span></span>

![Instalado pacotes de amostra do DLg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

<span data-ttu-id="f8152-128">Os comandos a seguir instalam os módulos MVC5 prévia e EF6 do console do Gerenciador de pacote:</span><span class="sxs-lookup"><span data-stu-id="f8152-128">The following commands install Glimpse MVC5 and EF6 modules from the package manager console:</span></span>

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a><span data-ttu-id="f8152-129">Habilitar prévia para localhost</span><span class="sxs-lookup"><span data-stu-id="f8152-129">Enable Glimpse for localhost</span></span>

<span data-ttu-id="f8152-130">Navegue até http://localhost:&lt;porta #&gt;/glimpse.axd e clique no **ativar prévia** botão.</span><span class="sxs-lookup"><span data-stu-id="f8152-130">Navigate to http://localhost:&lt;port #&gt;/glimpse.axd and click the **Turn Glimpse On** button.</span></span>

![Página de axd prévia](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

<span data-ttu-id="f8152-132">Se você tiver a barra de favoritos exibida, você pode arrastar e soltar os botões de amostra e adicioná-los como bookmarklets:</span><span class="sxs-lookup"><span data-stu-id="f8152-132">If you have your favorites bar displayed, you can drag and drop the Glimpse buttons and add them as bookmarklets:</span></span>

![IE com boookmarklets de amostra](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

<span data-ttu-id="f8152-134">Agora você pode navegar seu aplicativo e o **cabeçotes a exibição** (HUD) é mostrado na parte inferior da página.</span><span class="sxs-lookup"><span data-stu-id="f8152-134">You can now navigate your app, and the **Heads Up Display** (HUD) is shown at the bottom of the page.</span></span>

![Página do Gerenciador de contato com HUD](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

<span data-ttu-id="f8152-136">O [página prévia HUD](http://getglimpse.com/Docs/Heads-up-Display) fornece detalhes sobre as informações de tempo mostradas acima.</span><span class="sxs-lookup"><span data-stu-id="f8152-136">The [Glimpse HUD page](http://getglimpse.com/Docs/Heads-up-Display) details the timing information shown above.</span></span> <span data-ttu-id="f8152-137">As exibições de dados a HUD desempenho discreto podem notificá-lo de um problema imediatamente - antes de obter o ciclo de teste.</span><span class="sxs-lookup"><span data-stu-id="f8152-137">The unobtrusive performance data the HUD displays can notify you of a problem immediately - before you get to the test cycle.</span></span> <span data-ttu-id="f8152-138">Clicar no &quot;g&quot; no canto inferior direito exibe o painel de amostra:</span><span class="sxs-lookup"><span data-stu-id="f8152-138">Clicking on the &quot;g&quot; in the lower right corner brings up the Glimpse panel:</span></span>

![Painel de visão rápida](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

<span data-ttu-id="f8152-140">Na imagem acima, o [guia execução](http://getglimpse.com/Docs/Execution-Tab) for selecionada, que mostra detalhes de tempo das ações e os filtros no pipeline.</span><span class="sxs-lookup"><span data-stu-id="f8152-140">In the image above, the [Execution tab](http://getglimpse.com/Docs/Execution-Tab) is selected, which shows timing details of the actions and filters in the pipeline.</span></span> <span data-ttu-id="f8152-141">Você pode ver meu [timer de filtro de inspeção parar](http://www.nuget.org/packages/StopWatch/) Iniciar etapa 6 do pipeline.</span><span class="sxs-lookup"><span data-stu-id="f8152-141">You can see my [Stop Watch filter timer](http://www.nuget.org/packages/StopWatch/) start at stage 6 of the pipeline.</span></span> <span data-ttu-id="f8152-142">Enquanto o timer de peso leve pode fornecer útil perfil/dados de tempo, erros todo o tempo gasto na autorização e o modo de exibição de renderização.</span><span class="sxs-lookup"><span data-stu-id="f8152-142">While my light weight timer can provide useful profile/timing data, it misses all the time spent in authorization and rendering the view.</span></span> <span data-ttu-id="f8152-143">Você pode ler sobre meu timer em [de perfil e a hora de seu aplicativo ASP.NET MVC para Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span><span class="sxs-lookup"><span data-stu-id="f8152-143">You can read about my timer at [Profile and Time your ASP.NET MVC app all the way to Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span></span> <span data-ttu-id="f8152-144">O [guias](http://getglimpse.com/Docs/Tabs) página fornece links para informações detalhadas sobre cada guia.</span><span class="sxs-lookup"><span data-stu-id="f8152-144">The [Tabs](http://getglimpse.com/Docs/Tabs) page provides links to detailed information on each tab.</span></span>

<a id="Time"></a>
## <a name="the-timeline-tab"></a><span data-ttu-id="f8152-145">Na guia Cronograma</span><span class="sxs-lookup"><span data-stu-id="f8152-145">The Timeline tab</span></span>

<span data-ttu-id="f8152-146">Modificar de Tom Dykstra pendentes [tutorial de 6/MVC 5 EF](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) com o código a seguir, altere para o controlador de professores:</span><span class="sxs-lookup"><span data-stu-id="f8152-146">I've modified Tom Dykstra's outstanding [EF 6/MVC 5 tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) with the following code change to the instructors controller:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

<span data-ttu-id="f8152-147">O código acima permite passar na cadeia de caracteres de consulta (`eager`) para controlar eager ou explícita de carregamento de dados.</span><span class="sxs-lookup"><span data-stu-id="f8152-147">The code above allows me to pass in query string (`eager`) to control eager or explicit loading of data.</span></span> <span data-ttu-id="f8152-148">Na imagem abaixo, o carregamento explícito é usado e a página de controle de tempo mostra cada registro carregado no `Index` método de ação:</span><span class="sxs-lookup"><span data-stu-id="f8152-148">In the image below, explicit loading is used and the timing page shows each enrollment loaded in the `Index` action method:</span></span>

![carregamento explícito](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

<span data-ttu-id="f8152-150">No código a seguir, eager for especificado, e cada registro é buscado após o `Index` é chamado de modo de exibição:</span><span class="sxs-lookup"><span data-stu-id="f8152-150">In the following code, eager is specified, and each enrollment is fetched after the `Index` view is called:</span></span>

![eager for especificado](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

<span data-ttu-id="f8152-152">Você pode focalizar um segmento de tempo para obter informações detalhadas de tempo:</span><span class="sxs-lookup"><span data-stu-id="f8152-152">You can hover over a time segment to get detailed timing information:</span></span>

![Focalize tempo detalhados](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a><span data-ttu-id="f8152-154">Associação de modelo</span><span class="sxs-lookup"><span data-stu-id="f8152-154">Model Binding</span></span>

<span data-ttu-id="f8152-155">O [guia modelo de associação](http://getglimpse.com/Docs/Model-Binding-Tab) fornece uma grande quantidade de informações para ajudá-lo a entender como as variáveis de formulário são limitadas e por que alguns não são associados conforme o esperado.</span><span class="sxs-lookup"><span data-stu-id="f8152-155">The [model binding tab](http://getglimpse.com/Docs/Model-Binding-Tab) provides a wealth of information to help you understand how your form variables are bound and why some are not bound as would expect.</span></span> <span data-ttu-id="f8152-156">A imagem abaixo mostra o **?**</span><span class="sxs-lookup"><span data-stu-id="f8152-156">The image below shows the **?**</span></span> <span data-ttu-id="f8152-157">ícone, você pode clicar em para abrir a página de ajuda de amostra para esse recurso.</span><span class="sxs-lookup"><span data-stu-id="f8152-157">icon, which you can click on to bring up the glimpse help page for that feature.</span></span>

![mostra a exibição do modelo de associação](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a><span data-ttu-id="f8152-159">Rotas</span><span class="sxs-lookup"><span data-stu-id="f8152-159">Routes</span></span>

 <span data-ttu-id="f8152-160">Na guia rotas prévia pode ajudará você depurar e entender o roteamento.</span><span class="sxs-lookup"><span data-stu-id="f8152-160">The Glimpse Routes tab will can help you debug and understand routing.</span></span> <span data-ttu-id="f8152-161">Na imagem abaixo, a rota de produto é selecionada (e aparece em verde, uma convenção de amostra).</span><span class="sxs-lookup"><span data-stu-id="f8152-161">In the image below, the product route is selected (and it shows in green, a Glimpse convention).</span></span> <span data-ttu-id="f8152-162">![nome do produto selecionado](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) tokens de dados, restrições e áreas de rota também são exibidos.</span><span class="sxs-lookup"><span data-stu-id="f8152-162">![product name selected](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) Route constraints, Areas and data tokens are also displayed.</span></span> <span data-ttu-id="f8152-163">Consulte [prévia rotas](http://getglimpse.com/Docs/Routes-Tab) e [roteamento de atributo no ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="f8152-163">See [Glimpse Routes](http://getglimpse.com/Docs/Routes-Tab) and [Attribute Routing in ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) for more information.</span></span> 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a><span data-ttu-id="f8152-164">Usando a amostra no Azure</span><span class="sxs-lookup"><span data-stu-id="f8152-164">Using Glimpse on Azure</span></span>

<span data-ttu-id="f8152-165">A política de segurança padrão prévia apenas permite que os dados de amostra a ser exibida no host local.</span><span class="sxs-lookup"><span data-stu-id="f8152-165">The Glimpse default security policy only allows Glimpse data to be displayed from local host.</span></span> <span data-ttu-id="f8152-166">Você pode alterar essa política de segurança para que você pode exibir esses dados em um servidor remoto (como um aplicativo web no Azure).</span><span class="sxs-lookup"><span data-stu-id="f8152-166">You can change this security policy so you can view this data on a remote server (such as a web app on Azure).</span></span> <span data-ttu-id="f8152-167">Para ambientes de teste no Azure, adicione a marca realçada até a parte inferior da *confg* arquivo para habilitar a amostra:</span><span class="sxs-lookup"><span data-stu-id="f8152-167">For test environments on Azure, add the highlighted mark up to the bottom of the *web.confg* file to enable Glimpse:</span></span>

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

<span data-ttu-id="f8152-168">Com essa alteração sozinha, qualquer usuário pode ver os dados de amostra em um local remoto.</span><span class="sxs-lookup"><span data-stu-id="f8152-168">With this change alone, any user can see your Glimpse data on a remote site.</span></span> <span data-ttu-id="f8152-169">Considere adicionar a marcação acima para um perfil de publicação para que ele tem apenas implantado um aplicada quando você usar esse perfil de publicação (por exemplo, proifle o teste do Azure.) Para restringir os dados de amostra, adicionaremos o `canViewGlimpseData` função e só permitir que os usuários nesta função para exibir dados de amostra.</span><span class="sxs-lookup"><span data-stu-id="f8152-169">Consider adding the markup above to a publish profile so it's only deployed an applyed when you use that publish profile (for example, your Azure test proifle.) To restrict Glimpse data, we will add the `canViewGlimpseData` role and only allow users in this role to view Glimpse data.</span></span>

<span data-ttu-id="f8152-170">Remova os comentários do *GlimpseSecurityPolicy.cs* de arquivo e altere o [IsInRole](https://msdn.microsoft.com/en-us/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) chamar de `Administrator` para o `canViewGlimpseData` função:</span><span class="sxs-lookup"><span data-stu-id="f8152-170">Remove the comments from the *GlimpseSecurityPolicy.cs* file and change the [IsInRole](https://msdn.microsoft.com/en-us/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) call from `Administrator` to the `canViewGlimpseData` role:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> <span data-ttu-id="f8152-171">Segurança - os dados ricos fornecidos pelo prévia podem expor a segurança do seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="f8152-171">Security - The rich data provided by Glimpse could expose the security of your app.</span></span> <span data-ttu-id="f8152-172">Microsoft não realizou uma auditoria de segurança de amostra para uso em aplicativos de produção.</span><span class="sxs-lookup"><span data-stu-id="f8152-172">Microsoft has not performed a security audit of Glimpse for use on productions apps.</span></span>


<span data-ttu-id="f8152-173">Para obter informações sobre a adição de funções, consulte Meus [implantar um aplicativo da web seguro ASP.NET MVC 5 com associação, OAuth e o banco de dados do SQL Azure](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) tutorial.</span><span class="sxs-lookup"><span data-stu-id="f8152-173">For information on adding roles, see my [Deploy a Secure ASP.NET MVC 5 web app with Membership, OAuth, and SQL Database to Azure](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) tutorial.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="f8152-174">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="f8152-174">Additional Resources</span></span>

- [<span data-ttu-id="f8152-175">Implantar um aplicativo de seguro ASP.NET MVC 5 com associação, OAuth e o banco de dados SQL do Azure</span><span class="sxs-lookup"><span data-stu-id="f8152-175">Deploy a Secure ASP.NET MVC 5 app with Membership, OAuth, and SQL Database to Azure</span></span>](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- <span data-ttu-id="f8152-176">[Mostra a configuração](http://getglimpse.com/Docs/Configuration) -página de documento sobre como configurar guias, diretiva de tempo de execução, registro em log e muito mais.</span><span class="sxs-lookup"><span data-stu-id="f8152-176">[Glimpse Configuration](http://getglimpse.com/Docs/Configuration) - Doc page on configuring tabs, runtime policy, logging and more.</span></span>
