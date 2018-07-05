---
uid: web-pages/overview/performance-and-traffic/14-analyzing-traffic
title: Controlando as informações de visitante (análise) para uma Web do ASP.NET (Razor) sites de páginas para | Microsoft Docs
author: tfitzmac
description: Depois que você tiver chegado a seu site vai, convém analisar o tráfego do site.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 360bc6e1-84c5-4b8e-a84c-ea48ab807aa4
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/performance-and-traffic/14-analyzing-traffic
msc.type: authoredcontent
ms.openlocfilehash: 48782606083b4aa1e32adf6163bcb3f2d9828bc3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387131"
---
<a name="tracking-visitor-information-analytics-for-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="aa81a-103">Controlando as informações de visitante (análise) para um Site do ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="aa81a-103">Tracking Visitor Information (Analytics) for an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="aa81a-104">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="aa81a-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="aa81a-105">Este artigo descreve como usar um auxiliar para adicionar a análise de sites para páginas em um site de páginas da Web do ASP.NET (Razor).</span><span class="sxs-lookup"><span data-stu-id="aa81a-105">This article describes how to use a helper to add website analytics to pages in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="aa81a-106">O que você aprenderá:</span><span class="sxs-lookup"><span data-stu-id="aa81a-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="aa81a-107">Como enviar informações sobre seu tráfego do site para um provedor de análise.</span><span class="sxs-lookup"><span data-stu-id="aa81a-107">How to send information about your website traffic to an analytics provider.</span></span>
> 
> <span data-ttu-id="aa81a-108">Estes são os recursos introduzidos no artigo de programação do ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="aa81a-108">These are the ASP.NET programming features introduced in the article:</span></span>
> 
> - <span data-ttu-id="aa81a-109">O `Analytics` auxiliar.</span><span class="sxs-lookup"><span data-stu-id="aa81a-109">The `Analytics` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="aa81a-110">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="aa81a-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="aa81a-111">Páginas da Web do ASP.NET (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="aa81a-111">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="aa81a-112">ASP.NET Web Helpers Library (pacote do NuGet)</span><span class="sxs-lookup"><span data-stu-id="aa81a-112">ASP.NET Web Helpers Library (NuGet package)</span></span>


<span data-ttu-id="aa81a-113">Análise é um termo geral para a tecnologia que mede o tráfego em seu site para que você possa entender como as pessoas usam o site.</span><span class="sxs-lookup"><span data-stu-id="aa81a-113">Analytics is a general term for technology that measures traffic on your website so you can understand how people use the site.</span></span> <span data-ttu-id="aa81a-114">Muitos serviços de análise estão disponíveis, incluindo os serviços do Google, Yahoo, StatCounter e outras pessoas.</span><span class="sxs-lookup"><span data-stu-id="aa81a-114">Many analytics services are available, including services from Google, Yahoo, StatCounter, and others.</span></span>

<span data-ttu-id="aa81a-115">A análise de maneira que funciona é que você se inscrever para uma conta com o provedor de análise, em que você registre o site que você deseja controlar. O provedor envia um trecho de código JavaScript que inclui uma ID ou código para sua conta de rastreamento.</span><span class="sxs-lookup"><span data-stu-id="aa81a-115">The way analytics works is that you sign up for an account with the analytics provider, where you register the site that you want to track. The provider sends you a snippet of JavaScript code that includes an ID or tracking code for your account.</span></span> <span data-ttu-id="aa81a-116">Você pode adicionar o trecho de JavaScript em páginas da web no site que você deseja rastrear. (Você normalmente adiciona o trecho de código de análise a uma página de layout ou de rodapé ou outra marcação HTML que aparece em cada página em seu site.) Quando os usuários solicitam uma página que contém um dos trechos de código JavaScript, o trecho de código envia informações sobre a página atual para o provedor de análise, que registra vários detalhes sobre a página.</span><span class="sxs-lookup"><span data-stu-id="aa81a-116">You add the JavaScript snippet to the web pages on the site that you want to track. (You typically add the analytics snippet to a footer or layout page or other HTML markup that appears on every page in your site.) When users request a page that contains one of these JavaScript snippets, the snippet sends information about the current page to the analytics provider, who records various details about the page.</span></span>

<span data-ttu-id="aa81a-117">Quando você deseja examinar as estatísticas de site, você faça logon no site do provedor de análise.</span><span class="sxs-lookup"><span data-stu-id="aa81a-117">When you want to have a look at your site statistics, you log into the analytics provider's website.</span></span> <span data-ttu-id="aa81a-118">Em seguida, você pode exibir todos os tipos de relatórios sobre seu site, como:</span><span class="sxs-lookup"><span data-stu-id="aa81a-118">You can then view all sorts of reports about your site, like:</span></span>

- <span data-ttu-id="aa81a-119">O número de exibições de página para páginas individuais.</span><span class="sxs-lookup"><span data-stu-id="aa81a-119">The number of page views for individual pages.</span></span> <span data-ttu-id="aa81a-120">Isso informa ao (aproximadamente) quantas pessoas estão visitando o site e quais páginas no seu site são os mais populares.</span><span class="sxs-lookup"><span data-stu-id="aa81a-120">This tells you (roughly) how many people are visiting the site, and which pages on your site are the most popular.</span></span>
- <span data-ttu-id="aa81a-121">Quanto tempo as pessoas gastam em páginas específicas.</span><span class="sxs-lookup"><span data-stu-id="aa81a-121">How long people spend on specific pages.</span></span> <span data-ttu-id="aa81a-122">Isso pode lhe coisas como se sua home page é manter o interesse das pessoas.</span><span class="sxs-lookup"><span data-stu-id="aa81a-122">This can tell you things like whether your home page is keeping people's interest.</span></span>
- <span data-ttu-id="aa81a-123">O que as pessoas de sites estavam antes de eles visitam o site.</span><span class="sxs-lookup"><span data-stu-id="aa81a-123">What sites people were on before they visited your site.</span></span> <span data-ttu-id="aa81a-124">Isso ajuda você a entender se o tráfego é proveniente de links de pesquisas e assim por diante.</span><span class="sxs-lookup"><span data-stu-id="aa81a-124">This helps you understand whether your traffic is coming from links, from searches, and so on.</span></span>
- <span data-ttu-id="aa81a-125">Quando as pessoas visitam seu site e quanto tempo eles permanecem.</span><span class="sxs-lookup"><span data-stu-id="aa81a-125">When people visit your site and how long they stay.</span></span>
- <span data-ttu-id="aa81a-126">Quais países os visitantes são de.</span><span class="sxs-lookup"><span data-stu-id="aa81a-126">What countries your visitors are from.</span></span>
- <span data-ttu-id="aa81a-127">Quais navegadores e sistemas operacionais os visitantes estão usando.</span><span class="sxs-lookup"><span data-stu-id="aa81a-127">What browsers and operating systems your visitors are using.</span></span>

    ![Ch14traffic-1](14-analyzing-traffic/_static/image1.jpg)

## <a name="using-a-helper-to-add-analytics-to-a-page"></a><span data-ttu-id="aa81a-129">Usando um auxiliar para adicionar o Analytics a uma página</span><span class="sxs-lookup"><span data-stu-id="aa81a-129">Using a Helper to Add Analytics to a Page</span></span>

<span data-ttu-id="aa81a-130">Páginas da Web ASP.NET inclui vários auxiliares de análise (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, e `Analytics.GetStatCounterHtml`) que facilitam a gerenciar os trechos de código JavaScript usados para análise.</span><span class="sxs-lookup"><span data-stu-id="aa81a-130">ASP.NET Web Pages includes several analytics helpers (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, and `Analytics.GetStatCounterHtml`) that make it easy to manage the JavaScript snippets used for analytics.</span></span> <span data-ttu-id="aa81a-131">Em vez de descobrir como e onde colocar o código JavaScript, tudo o que você precisa fazer é adicionar o auxiliar a uma página.</span><span class="sxs-lookup"><span data-stu-id="aa81a-131">Instead of figuring out how and where to put the JavaScript code, all you have to do is add the helper to a page.</span></span> <span data-ttu-id="aa81a-132">A única informação que você precisa fornecer é o nome da conta, ID ou código de rastreamento.</span><span class="sxs-lookup"><span data-stu-id="aa81a-132">The only information you need to provide is your account name, ID, or tracking code.</span></span> <span data-ttu-id="aa81a-133">(Para StatCounter, você também precisa fornecer alguns valores adicional.)</span><span class="sxs-lookup"><span data-stu-id="aa81a-133">(For StatCounter, you also have to provide a few additional values.)</span></span>

<span data-ttu-id="aa81a-134">Neste procedimento, você criará uma página de layout que usa o `GetGoogleHtml` auxiliar.</span><span class="sxs-lookup"><span data-stu-id="aa81a-134">In this procedure, you'll create a layout page that uses the `GetGoogleHtml` helper.</span></span> <span data-ttu-id="aa81a-135">Se você já tiver uma conta com um dos provedores de análise, você pode usar a conta e fazer alguns pequenos ajustes conforme necessário.</span><span class="sxs-lookup"><span data-stu-id="aa81a-135">If you already have an account with one of the other analytics providers, you can use that account instead and make slight adjustments as needed.</span></span>

> [!NOTE]
> <span data-ttu-id="aa81a-136">Quando você cria uma conta da análise, você pode registrar a URL do site que você deseja acompanhar.</span><span class="sxs-lookup"><span data-stu-id="aa81a-136">When you create an analytics account, you register the URL of the site that you want to be tracking.</span></span> <span data-ttu-id="aa81a-137">Se você estiver testando tudo em seu computador local, você não rastreando tráfego real (somente o tráfego é você), portanto, não será capaz de registro e exibir estatísticas de site.</span><span class="sxs-lookup"><span data-stu-id="aa81a-137">If you're testing everything on your local computer, you won't be tracking actual traffic (the only traffic is you), so you won't be able to record and view site statistics.</span></span> <span data-ttu-id="aa81a-138">Mas esse procedimento mostra como adicionar um auxiliar de análise para uma página.</span><span class="sxs-lookup"><span data-stu-id="aa81a-138">But this procedure shows how you add an analytics helper to a page.</span></span> <span data-ttu-id="aa81a-139">Quando você publica seu site, o site ao vivo enviará informações para seu provedor de análise.</span><span class="sxs-lookup"><span data-stu-id="aa81a-139">When you publish your site, the live site will send information to your analytics provider.</span></span>


1. <span data-ttu-id="aa81a-140">Adicionar o ASP.NET Web Helpers Library ao seu site, conforme descrito em [auxiliares de instalação em um Site de páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se você ainda não tenha adicionado.</span><span class="sxs-lookup"><span data-stu-id="aa81a-140">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="aa81a-141">Criar uma conta com o Google Analytics e registre o nome da conta.</span><span class="sxs-lookup"><span data-stu-id="aa81a-141">Create an account with Google Analytics and record the account name.</span></span>
3. <span data-ttu-id="aa81a-142">Criar uma página de layout denominada *Analytics.cshtml* e adicione a seguinte marcação:</span><span class="sxs-lookup"><span data-stu-id="aa81a-142">Create a layout page named *Analytics.cshtml* and add the following markup:</span></span>

    [!code-cshtml[Main](14-analyzing-traffic/samples/sample1.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="aa81a-143">Você deve colocar a chamada para o `Analytics` auxiliar no corpo da sua página da web (antes do `</body>` marca).</span><span class="sxs-lookup"><span data-stu-id="aa81a-143">You must place the call to the `Analytics` helper in the body of your web page (before the `</body>` tag).</span></span> <span data-ttu-id="aa81a-144">Caso contrário, o navegador não executará o script.</span><span class="sxs-lookup"><span data-stu-id="aa81a-144">Otherwise, the browser will not run the script.</span></span>

    <span data-ttu-id="aa81a-145">Se você estiver usando um provedor de análise diferente, use um dos seguintes auxiliares:</span><span class="sxs-lookup"><span data-stu-id="aa81a-145">If you're using a different analytics provider, use one of the following helpers instead:</span></span>

    - <span data-ttu-id="aa81a-146">(Yahoo) `@Analytics.GetYahooHtml("myaccount")`</span><span class="sxs-lookup"><span data-stu-id="aa81a-146">(Yahoo) `@Analytics.GetYahooHtml("myaccount")`</span></span>
    - <span data-ttu-id="aa81a-147">(StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`</span><span class="sxs-lookup"><span data-stu-id="aa81a-147">(StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`</span></span>
4. <span data-ttu-id="aa81a-148">Substitua `myaccount` com o nome da conta, ID ou código de rastreamento que você criou na etapa 1.</span><span class="sxs-lookup"><span data-stu-id="aa81a-148">Replace `myaccount` with the name of the account, ID, or tracking code that you created in step 1.</span></span>
5. <span data-ttu-id="aa81a-149">Execute a página no navegador.</span><span class="sxs-lookup"><span data-stu-id="aa81a-149">Run the page in the browser.</span></span> <span data-ttu-id="aa81a-150">(Certifique-se de que a página está selecionada na **arquivos** espaço de trabalho antes de executá-lo.)</span><span class="sxs-lookup"><span data-stu-id="aa81a-150">(Make sure the page is selected in the **Files** workspace before you run it.)</span></span>
6. <span data-ttu-id="aa81a-151">No navegador, exiba a origem da página.</span><span class="sxs-lookup"><span data-stu-id="aa81a-151">In the browser, view the page source.</span></span> <span data-ttu-id="aa81a-152">Você poderá ver o código de análise renderizado:</span><span class="sxs-lookup"><span data-stu-id="aa81a-152">You'll be able to see the rendered analytics code:</span></span>

    [!code-html[Main](14-analyzing-traffic/samples/sample2.html)]
7. <span data-ttu-id="aa81a-153">Faça logon no site do Google Analytics e examinar as estatísticas para o seu site.</span><span class="sxs-lookup"><span data-stu-id="aa81a-153">Log onto the Google Analytics site and examine the statistics for your site.</span></span> <span data-ttu-id="aa81a-154">Se você estiver executando a página em um site ativo, você pode ver uma entrada que registra a visita à sua página.</span><span class="sxs-lookup"><span data-stu-id="aa81a-154">If you're running the page on a live site, you see an entry that logs the visit to your page.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="aa81a-155">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="aa81a-155">Additional Resources</span></span>

- [<span data-ttu-id="aa81a-156">Site do Google Analytics</span><span class="sxs-lookup"><span data-stu-id="aa81a-156">Google Analytics site</span></span>](https://www.google.com/analytics/)
- [<span data-ttu-id="aa81a-157">Yahoo! Site do Web Analytics</span><span class="sxs-lookup"><span data-stu-id="aa81a-157">Yahoo! Web Analytics site</span></span>](http://help.yahoo.com/l/us/yahoo/ywa/)
- [<span data-ttu-id="aa81a-158">Site StatCounter</span><span class="sxs-lookup"><span data-stu-id="aa81a-158">StatCounter site</span></span>](http://statcounter.com/)
