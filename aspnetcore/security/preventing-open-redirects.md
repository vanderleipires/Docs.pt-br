---
title: Evitando ataques de redirecionamento aberto em um aplicativo do ASP.NET Core | Microsoft Docs
author: ardalis
description: Mostra como evitar ataques de redirecionamento aberto em um aplicativo do ASP.NET Core
keywords: "Ataque de redirecionamento aberta do ASP.NET Core, segurança,"
ms.author: riande
manager: wpickett
ms.date: 07/07/2017
ms.topic: article
ms.assetid: 4604e563-e91a-4ecd-b7ed-00b3f1eee2b5
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/preventing-open-redirects
ms.openlocfilehash: 4083845a77eb19d9ba9beb389a92ceb5c14edbde
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="preventing-open-redirect-attacks-in-an-aspnet-core-app"></a><span data-ttu-id="b0656-104">Evitando ataques de redirecionamento aberto em um aplicativo do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b0656-104">Preventing Open Redirect Attacks in an ASP.NET Core app</span></span>

<span data-ttu-id="b0656-105">Um aplicativo web que redireciona para uma URL que é especificada por meio de solicitação, como os dados de formulário ou querystring potencialmente pode ser violado para redirecionar usuários para uma URL externa e mal-intencionados.</span><span class="sxs-lookup"><span data-stu-id="b0656-105">A web app that redirects to a URL that is specified via the request such as the querystring or form data can potentially be tampered with to redirect users to an external, malicious URL.</span></span> <span data-ttu-id="b0656-106">Essa violação é chamado de um ataque de redirecionamento aberto.</span><span class="sxs-lookup"><span data-stu-id="b0656-106">This tampering is called an open redirection attack.</span></span>

<span data-ttu-id="b0656-107">Sempre que a lógica do aplicativo redireciona para uma URL específica, você deve verificar se a URL de redirecionamento não foi adulterada.</span><span class="sxs-lookup"><span data-stu-id="b0656-107">Whenever your application logic redirects to a specified URL, you must verify that the redirection URL hasn't been tampered with.</span></span> <span data-ttu-id="b0656-108">Núcleo do ASP.NET tem funcionalidade interna para ajudar a proteger aplicativos contra ataques de redirecionamento aberto (também conhecido como open redirecionamento).</span><span class="sxs-lookup"><span data-stu-id="b0656-108">ASP.NET Core has built-in functionality to help protect apps from open redirect (also known as open redirection) attacks.</span></span>

## <a name="what-is-an-open-redirect-attack"></a><span data-ttu-id="b0656-109">O que é um ataque de redirecionamento aberto?</span><span class="sxs-lookup"><span data-stu-id="b0656-109">What is an open redirect attack?</span></span>

<span data-ttu-id="b0656-110">Aplicativos da Web com frequência redirecionam os usuários para uma página de logon quando acessarem os recursos que exigem autenticação.</span><span class="sxs-lookup"><span data-stu-id="b0656-110">Web applications frequently redirect users to a login page when they access resources that require authentication.</span></span> <span data-ttu-id="b0656-111">O redirecionamento typlically inclui um `returnUrl` parâmetro querystring para que o usuário pode ser retornado para a URL solicitada originalmente depois que eles efetuou com êxito.</span><span class="sxs-lookup"><span data-stu-id="b0656-111">The redirection typlically includes a `returnUrl` querystring parameter so that the user can be returned to the originally requested URL after they have successfully logged in.</span></span> <span data-ttu-id="b0656-112">Depois que o usuário é autenticado, ele será redirecionado para a URL que tinha originalmente solicitada.</span><span class="sxs-lookup"><span data-stu-id="b0656-112">After the user authenticates, they are redirected to the URL they had originally requested.</span></span>

<span data-ttu-id="b0656-113">Como a URL de destino é especificada na querystring da solicitação, um usuário mal-intencionado pode violar querystring.</span><span class="sxs-lookup"><span data-stu-id="b0656-113">Because the destination URL is specified in the querystring of the request, a malicious user could tamper with the querystring.</span></span> <span data-ttu-id="b0656-114">Uma querystring violado pode permitir que o site redirecionar o usuário a um site externo, mal-intencionado.</span><span class="sxs-lookup"><span data-stu-id="b0656-114">A tampered querystring could allow the site to redirect the user to an external, malicious site.</span></span> <span data-ttu-id="b0656-115">Essa técnica é chamada de um ataque de redirecionamento (ou redirecionamento) aberto.</span><span class="sxs-lookup"><span data-stu-id="b0656-115">This technique is called an open redirect (or redirection) attack.</span></span>

### <a name="an-example-attack"></a><span data-ttu-id="b0656-116">Um ataque de exemplo</span><span class="sxs-lookup"><span data-stu-id="b0656-116">An example attack</span></span>

<span data-ttu-id="b0656-117">Um usuário mal-intencionado poderá desenvolver um ataque de objetivo de permitir que o usuário mal-intencionado acesso a informações confidenciais em seu aplicativo ou as credenciais de um usuário.</span><span class="sxs-lookup"><span data-stu-id="b0656-117">A malicious user could develop an attack intended to allow the malicious user access to a user's credentials or sensitive information on your app.</span></span> <span data-ttu-id="b0656-118">Para iniciar o ataque, eles convencer o usuário clicar em um link para a página de logon do site, com um `returnUrl` valor de querystring adicionada à URL.</span><span class="sxs-lookup"><span data-stu-id="b0656-118">To begin the attack, they convince the user to click a link to your site's login page, with a `returnUrl` querystring value added to the URL.</span></span> <span data-ttu-id="b0656-119">Por exemplo, o [NerdDinner.com](http://nerddinner.com) (gravado para o ASP.NET MVC) do aplicativo de exemplo inclui tal uma página de logon aqui: ``http://nerddinner.com/Account/LogOn?returnUrl=/Home/About``.</span><span class="sxs-lookup"><span data-stu-id="b0656-119">For example, the [NerdDinner.com](http://nerddinner.com) sample application (written for ASP.NET MVC) includes such a login page here: ``http://nerddinner.com/Account/LogOn?returnUrl=/Home/About``.</span></span> <span data-ttu-id="b0656-120">O ataque, em seguida, siga estas etapas:</span><span class="sxs-lookup"><span data-stu-id="b0656-120">The attack then follows these steps:</span></span>

1. <span data-ttu-id="b0656-121">Usuário clica em um link para ``http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn`` (Observe que a URL segundo é nerddi**n**er, não nerddi**nn**er).</span><span class="sxs-lookup"><span data-stu-id="b0656-121">User clicks a link to ``http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn`` (note, second URL is nerddi**n**er, not nerddi**nn**er).</span></span>
2. <span data-ttu-id="b0656-122">O usuário fizer logon com êxito.</span><span class="sxs-lookup"><span data-stu-id="b0656-122">The user logs in successfully.</span></span>
3. <span data-ttu-id="b0656-123">O usuário é redirecionado (pelo site) para ``http://nerddiner.com/Account/LogOn`` (site mal-intencionado que se parece com um site real).</span><span class="sxs-lookup"><span data-stu-id="b0656-123">The user is redirected (by the site) to ``http://nerddiner.com/Account/LogOn`` (malicious site that looks like real site).</span></span>
4. <span data-ttu-id="b0656-124">O usuário fizer logon novamente (fornecendo mal-intencionado suas credenciais do site) e é redirecionado para o site real.</span><span class="sxs-lookup"><span data-stu-id="b0656-124">The user logs in again (giving malicious site their credentials) and is redirected back to the real site.</span></span>

<span data-ttu-id="b0656-125">O usuário provavelmente vai achar sua primeira tentativa de logon falha, e sua segunda foi bem-sucedida.</span><span class="sxs-lookup"><span data-stu-id="b0656-125">The user will likely believe their first attempt to log in failed, and their second one was successful.</span></span> <span data-ttu-id="b0656-126">Eles provavelmente permanecem sem reconhecimento de suas credenciais foram comprometidas.</span><span class="sxs-lookup"><span data-stu-id="b0656-126">They'll most likely remain unaware their credentials have been compromised.</span></span>

![Processo de ataques de redirecionamento aberto](preventing-open-redirects/_static/open-redirection-attack-process.png)

<span data-ttu-id="b0656-128">Além das páginas de logon, alguns sites fornecem redirecionamento páginas ou pontos de extremidade.</span><span class="sxs-lookup"><span data-stu-id="b0656-128">In addition to login pages, some sites provide redirect pages or endpoints.</span></span> <span data-ttu-id="b0656-129">Imagine que seu aplicativo tem uma página com um redirecionamento aberto, ``/Home/Redirect``.</span><span class="sxs-lookup"><span data-stu-id="b0656-129">Imagine your app has a page with an open redirect, ``/Home/Redirect``.</span></span> <span data-ttu-id="b0656-130">Um invasor pode criar, por exemplo, um link em um email que vão para ``[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login``.</span><span class="sxs-lookup"><span data-stu-id="b0656-130">An attacker could create, for example, a link in an email that goes to ``[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login``.</span></span> <span data-ttu-id="b0656-131">Um usuário normal na URL e ver começa com o nome do site.</span><span class="sxs-lookup"><span data-stu-id="b0656-131">A typical user will look at the URL and see it begins with your site name.</span></span> <span data-ttu-id="b0656-132">Confiar em que, eles clicarem no link.</span><span class="sxs-lookup"><span data-stu-id="b0656-132">Trusting that, they will click the link.</span></span> <span data-ttu-id="b0656-133">O redirecionamento aberto, em seguida, envia o usuário para o site de phishing, que é idêntico ao seu, e o usuário provavelmente é de logon para o que eles acreditam seu site.</span><span class="sxs-lookup"><span data-stu-id="b0656-133">The open redirect would then send the user to the phishing site, which looks identical to yours, and the user would likely login to what they believe is your site.</span></span>

## <a name="protecting-against-open-redirect-attacks"></a><span data-ttu-id="b0656-134">Proteção contra ataques de redirecionamento aberto</span><span class="sxs-lookup"><span data-stu-id="b0656-134">Protecting against open redirect attacks</span></span>

<span data-ttu-id="b0656-135">Ao desenvolver aplicativos da web, trate todos os dados fornecidos pelo usuário como não confiável.</span><span class="sxs-lookup"><span data-stu-id="b0656-135">When developing web applications, treat all user-provided data as untrustworthy.</span></span> <span data-ttu-id="b0656-136">Se seu aplicativo tem funcionalidade que redireciona o usuário com base no conteúdo da URL, certifique-se de que tais redirecionamentos só são feitos localmente em seu aplicativo (ou uma URL conhecida, e não qualquer URL que pode ser fornecido na querystring).</span><span class="sxs-lookup"><span data-stu-id="b0656-136">If your application has functionality that redirects the user based on the contents of the URL,  ensure that such redirects are only done locally within your app (or to a known URL, not any URL that may be supplied in the querystring).</span></span>

### <a name="localredirect"></a><span data-ttu-id="b0656-137">LocalRedirect</span><span class="sxs-lookup"><span data-stu-id="b0656-137">LocalRedirect</span></span>

<span data-ttu-id="b0656-138">Use o ``LocalRedirect`` método auxiliar de base de `Controller` classe:</span><span class="sxs-lookup"><span data-stu-id="b0656-138">Use the ``LocalRedirect`` helper method from the base `Controller` class:</span></span>

```csharp
public IActionResult SomeAction(string redirectUrl)
{
    return LocalRedirect(redirectUrl);
}
```

<span data-ttu-id="b0656-139">``LocalRedirect``lançará uma exceção se uma URL de local não for especificada.</span><span class="sxs-lookup"><span data-stu-id="b0656-139">``LocalRedirect`` will throw an exception if a non-local URL is specified.</span></span> <span data-ttu-id="b0656-140">Caso contrário, ele se comporta exatamente como o ``Redirect`` método.</span><span class="sxs-lookup"><span data-stu-id="b0656-140">Otherwise, it behaves just like the ``Redirect`` method.</span></span>

### <a name="islocalurl"></a><span data-ttu-id="b0656-141">IsLocalUrl</span><span class="sxs-lookup"><span data-stu-id="b0656-141">IsLocalUrl</span></span>

<span data-ttu-id="b0656-142">Use o [IsLocalUrl](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iurlhelper#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) método de teste URLs antes de redirecionar:</span><span class="sxs-lookup"><span data-stu-id="b0656-142">Use the [IsLocalUrl](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iurlhelper#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) method to test URLs before redirecting:</span></span>

<span data-ttu-id="b0656-143">O exemplo a seguir mostra como verificar se uma URL é local antes de redirecionar.</span><span class="sxs-lookup"><span data-stu-id="b0656-143">The following example shows how to check whether a URL is local before redirecting.</span></span>

```csharp
private IActionResult RedirectToLocal(string returnUrl)
{
    if (Url.IsLocalUrl(returnUrl))
    {
        return Redirect(returnUrl);
    }
    else
    {
        return RedirectToAction(nameof(HomeController.Index), "Home");
    }
}
```

<span data-ttu-id="b0656-144">O `IsLocalUrl` método impede que os usuários inadvertidamente sendo redirecionado para um site mal-intencionado.</span><span class="sxs-lookup"><span data-stu-id="b0656-144">The `IsLocalUrl` method protects users from being inadvertently redirected to a malicious site.</span></span> <span data-ttu-id="b0656-145">Você pode registrar os detalhes da URL que foi fornecida uma URL de local não é fornecida em uma situação em que você esperava um URL local.</span><span class="sxs-lookup"><span data-stu-id="b0656-145">You can log the details of the URL that was provided when a non-local URL is supplied in a situation where you expected a local URL.</span></span> <span data-ttu-id="b0656-146">URLs de redirecionamento de log podem ajudar no diagnóstico de ataques de redirecionamento.</span><span class="sxs-lookup"><span data-stu-id="b0656-146">Logging redirect URLs may help in diagnosing redirection attacks.</span></span>
