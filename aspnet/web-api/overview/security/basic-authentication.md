---
uid: web-api/overview/security/basic-authentication
title: "Autenticação básica no ASP.NET Web API | Microsoft Docs"
author: MikeWasson
description: "Descreve como usar a autenticação básica no API Web do ASP.NET."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/02/2014
ms.topic: article
ms.assetid: 41423767-0021-47c3-9e53-0021b457c39f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/basic-authentication
msc.type: authoredcontent
ms.openlocfilehash: 4b8e6410668b2db289488bb4b6cd26d881e70f4c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="basic-authentication-in-aspnet-web-api"></a><span data-ttu-id="59d9f-103">Autenticação básica na API da Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="59d9f-103">Basic Authentication in ASP.NET Web API</span></span>
====================
<span data-ttu-id="59d9f-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="59d9f-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="59d9f-105">Autenticação básica é definida em [RFC 2617, autenticação HTTP: Basic e a autenticação Digest acesso](http://www.ietf.org/rfc/rfc2617.txt).</span><span class="sxs-lookup"><span data-stu-id="59d9f-105">Basic authentication is defined in [RFC 2617, HTTP Authentication: Basic and Digest Access Authentication](http://www.ietf.org/rfc/rfc2617.txt).</span></span>

<span data-ttu-id="59d9f-106">Desvantagens</span><span class="sxs-lookup"><span data-stu-id="59d9f-106">Disadvantages</span></span>

- <span data-ttu-id="59d9f-107">As credenciais do usuário são enviadas na solicitação.</span><span class="sxs-lookup"><span data-stu-id="59d9f-107">User credentials are sent in the request.</span></span>
- <span data-ttu-id="59d9f-108">As credenciais são enviadas como texto sem formatação.</span><span class="sxs-lookup"><span data-stu-id="59d9f-108">Credentials are sent as plaintext.</span></span>
- <span data-ttu-id="59d9f-109">As credenciais são enviadas com cada solicitação.</span><span class="sxs-lookup"><span data-stu-id="59d9f-109">Credentials are sent with every request.</span></span>
- <span data-ttu-id="59d9f-110">Nenhuma maneira de fazer logoff, exceto por encerrar a sessão do navegador.</span><span class="sxs-lookup"><span data-stu-id="59d9f-110">No way to log out, except by ending the browser session.</span></span>
- <span data-ttu-id="59d9f-111">Vulnerável a sites solicitação CSRF (falsificação); requer medidas anti-CSRF.</span><span class="sxs-lookup"><span data-stu-id="59d9f-111">Vulnerable to cross-site request forgery (CSRF); requires anti-CSRF measures.</span></span>

<span data-ttu-id="59d9f-112">Vantagens</span><span class="sxs-lookup"><span data-stu-id="59d9f-112">Advantages</span></span>

- <span data-ttu-id="59d9f-113">Padrão da Internet.</span><span class="sxs-lookup"><span data-stu-id="59d9f-113">Internet standard.</span></span>
- <span data-ttu-id="59d9f-114">Suporte para todos os principais navegadores.</span><span class="sxs-lookup"><span data-stu-id="59d9f-114">Supported by all major browsers.</span></span>
- <span data-ttu-id="59d9f-115">Protocolo relativamente simple.</span><span class="sxs-lookup"><span data-stu-id="59d9f-115">Relatively simple protocol.</span></span>

<span data-ttu-id="59d9f-116">Autenticação básica funciona da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="59d9f-116">Basic authentication works as follows:</span></span>

1. <span data-ttu-id="59d9f-117">Se uma solicitação requer autenticação, o servidor retorna 401 (não autorizado).</span><span class="sxs-lookup"><span data-stu-id="59d9f-117">If a request requires authentication, the server returns 401 (Unauthorized).</span></span> <span data-ttu-id="59d9f-118">A resposta inclui um cabeçalho WWW-Authenticate, indicando que o servidor oferece suporte à autenticação básica.</span><span class="sxs-lookup"><span data-stu-id="59d9f-118">The response includes a WWW-Authenticate header, indicating the server supports Basic authentication.</span></span>
2. <span data-ttu-id="59d9f-119">O cliente envia outra solicitação, com as credenciais do cliente no cabeçalho de autorização.</span><span class="sxs-lookup"><span data-stu-id="59d9f-119">The client sends another request, with the client credentials in the Authorization header.</span></span> <span data-ttu-id="59d9f-120">As credenciais são formatadas como a cadeia de caracteres "nome: senha", codificada em base64.</span><span class="sxs-lookup"><span data-stu-id="59d9f-120">The credentials are formatted as the string "name:password", base64-encoded.</span></span> <span data-ttu-id="59d9f-121">As credenciais não são criptografadas.</span><span class="sxs-lookup"><span data-stu-id="59d9f-121">The credentials are not encrypted.</span></span>

<span data-ttu-id="59d9f-122">Autenticação básica é executada dentro do contexto de um "Território".</span><span class="sxs-lookup"><span data-stu-id="59d9f-122">Basic authentication is performed within the context of a "realm."</span></span> <span data-ttu-id="59d9f-123">O servidor inclui o nome do território no cabeçalho WWW-Authenticate.</span><span class="sxs-lookup"><span data-stu-id="59d9f-123">The server includes the name of the realm in the WWW-Authenticate header.</span></span> <span data-ttu-id="59d9f-124">As credenciais do usuário são válidas dentro desse realm.</span><span class="sxs-lookup"><span data-stu-id="59d9f-124">The user's credentials are valid within that realm.</span></span> <span data-ttu-id="59d9f-125">O escopo exato de um território é definido pelo servidor.</span><span class="sxs-lookup"><span data-stu-id="59d9f-125">The exact scope of a realm is defined by the server.</span></span> <span data-ttu-id="59d9f-126">Por exemplo, você pode definir vários territórios em ordem para a partição de recursos.</span><span class="sxs-lookup"><span data-stu-id="59d9f-126">For example, you might define several realms in order to partition resources.</span></span>

![](basic-authentication/_static/image1.png)

<span data-ttu-id="59d9f-127">Como as credenciais são enviadas sem criptografia, autenticação básica é segura via HTTPS.</span><span class="sxs-lookup"><span data-stu-id="59d9f-127">Because the credentials are sent unencrypted, Basic authentication is only secure over HTTPS.</span></span> <span data-ttu-id="59d9f-128">Consulte [trabalhar com SSL na API da Web](working-with-ssl-in-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="59d9f-128">See [Working with SSL in Web API](working-with-ssl-in-web-api.md).</span></span>

<span data-ttu-id="59d9f-129">Autenticação básica também é vulnerável a ataques CSRF.</span><span class="sxs-lookup"><span data-stu-id="59d9f-129">Basic authentication is also vulnerable to CSRF attacks.</span></span> <span data-ttu-id="59d9f-130">Depois que o usuário insere as credenciais, o navegador envia automaticamente-los em solicitações subsequentes ao mesmo domínio, para a duração da sessão.</span><span class="sxs-lookup"><span data-stu-id="59d9f-130">After the user enters credentials, the browser automatically sends them on subsequent requests to the same domain, for the duration of the session.</span></span> <span data-ttu-id="59d9f-131">Isso inclui solicitações do AJAX.</span><span class="sxs-lookup"><span data-stu-id="59d9f-131">This includes AJAX requests.</span></span> <span data-ttu-id="59d9f-132">Consulte [impedindo ataques CSRF (falsificação) de solicitação entre sites](preventing-cross-site-request-forgery-csrf-attacks.md).</span><span class="sxs-lookup"><span data-stu-id="59d9f-132">See [Preventing Cross-Site Request Forgery (CSRF) Attacks](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>

## <a name="basic-authentication-with-iis"></a><span data-ttu-id="59d9f-133">Autenticação básica com o IIS</span><span class="sxs-lookup"><span data-stu-id="59d9f-133">Basic Authentication with IIS</span></span>

<span data-ttu-id="59d9f-134">O IIS dá suporte à autenticação básica, mas há uma limitação: O usuário é autenticado com as credenciais do Windows.</span><span class="sxs-lookup"><span data-stu-id="59d9f-134">IIS supports Basic authentication, but there is a caveat: The user is authenticated against their Windows credentials.</span></span> <span data-ttu-id="59d9f-135">Isso significa que o usuário deve ter uma conta no domínio do servidor.</span><span class="sxs-lookup"><span data-stu-id="59d9f-135">That means the user must have an account on the server's domain.</span></span> <span data-ttu-id="59d9f-136">Para um site voltado ao público, você geralmente deseja autenticar em um provedor de associação do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="59d9f-136">For a public-facing web site, you typically want to authenticate against an ASP.NET membership provider.</span></span>

<span data-ttu-id="59d9f-137">Para habilitar a autenticação básica usando o IIS, defina o modo de autenticação para "Windows" no Web. config do projeto ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="59d9f-137">To enable Basic authentication using IIS, set the authentication mode to "Windows" in the Web.config of your ASP.NET project:</span></span>

[!code-xml[Main](basic-authentication/samples/sample1.xml)]

<span data-ttu-id="59d9f-138">Nesse modo, o IIS usa as credenciais do Windows para autenticar.</span><span class="sxs-lookup"><span data-stu-id="59d9f-138">In this mode, IIS uses Windows credentials to authenticate.</span></span> <span data-ttu-id="59d9f-139">Além disso, você deve habilitar a autenticação básica no IIS.</span><span class="sxs-lookup"><span data-stu-id="59d9f-139">In addition, you must enable Basic authentication in IIS.</span></span> <span data-ttu-id="59d9f-140">No Gerenciador do IIS, vá para a exibição de recursos, selecione a autenticação e habilitar a autenticação básica.</span><span class="sxs-lookup"><span data-stu-id="59d9f-140">In IIS Manager, go to Features View, select Authentication, and enable Basic authentication.</span></span>

![](basic-authentication/_static/image2.png)

<span data-ttu-id="59d9f-141">No seu projeto de API da Web, adicione o `[Authorize]` atributo para as ações do controlador que precisam de autenticação.</span><span class="sxs-lookup"><span data-stu-id="59d9f-141">In your Web API project, add the `[Authorize]` attribute for any controller actions that need authentication.</span></span>

<span data-ttu-id="59d9f-142">Um cliente se autentica definindo o cabeçalho de autorização na solicitação.</span><span class="sxs-lookup"><span data-stu-id="59d9f-142">A client authenticates itself by setting the Authorization header in the request.</span></span> <span data-ttu-id="59d9f-143">Clientes de navegador executam esta etapa automaticamente.</span><span class="sxs-lookup"><span data-stu-id="59d9f-143">Browser clients perform this step automatically.</span></span> <span data-ttu-id="59d9f-144">Clientes de nonbrowser serão necessário definir o cabeçalho.</span><span class="sxs-lookup"><span data-stu-id="59d9f-144">Nonbrowser clients will need to set the header.</span></span>

## <a name="basic-authentication-with-custom-membership"></a><span data-ttu-id="59d9f-145">Autenticação básica com associação personalizado</span><span class="sxs-lookup"><span data-stu-id="59d9f-145">Basic Authentication with Custom Membership</span></span>

<span data-ttu-id="59d9f-146">Conforme mencionado, a autenticação básica integradas no IIS usa as credenciais do Windows.</span><span class="sxs-lookup"><span data-stu-id="59d9f-146">As mentioned, the Basic Authentication built into IIS uses Windows credentials.</span></span> <span data-ttu-id="59d9f-147">Isso significa que você precisa criar contas para os usuários no servidor de hospedagem.</span><span class="sxs-lookup"><span data-stu-id="59d9f-147">That means you need to create accounts for your users on the hosting server.</span></span> <span data-ttu-id="59d9f-148">Mas, para um aplicativo de internet, contas de usuário são normalmente armazenadas no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="59d9f-148">But for an internet application, user accounts are typically stored in an external database.</span></span>

<span data-ttu-id="59d9f-149">O código a seguir como um módulo HTTP que realiza a autenticação básica.</span><span class="sxs-lookup"><span data-stu-id="59d9f-149">The following code how an HTTP module that performs Basic Authentication.</span></span> <span data-ttu-id="59d9f-150">Você pode conectar facilmente em um provedor de associação do ASP.NET, substituindo o `CheckPassword` método, que é um método fictício neste exemplo.</span><span class="sxs-lookup"><span data-stu-id="59d9f-150">You can easily plug in an ASP.NET membership provider by replacing the `CheckPassword` method, which is a dummy method in this example.</span></span>

<span data-ttu-id="59d9f-151">API Web 2, você deve considerar gravando um [filtro autenticação](authentication-filters.md) ou [middleware OWIN](../../../aspnet/overview/owin-and-katana/index.md), em vez de um módulo HTTP.</span><span class="sxs-lookup"><span data-stu-id="59d9f-151">In Web API 2, you should consider writing an [authentication filter](authentication-filters.md) or [OWIN middleware](../../../aspnet/overview/owin-and-katana/index.md), instead of an HTTP module.</span></span>

[!code-csharp[Main](basic-authentication/samples/sample2.cs)]

<span data-ttu-id="59d9f-152">Para habilitar o módulo HTTP, adicione o seguinte ao arquivo Web. config no **System. webServer** seção:</span><span class="sxs-lookup"><span data-stu-id="59d9f-152">To enable the HTTP module, add the following to your web.config file in the **system.webServer** section:</span></span>

[!code-xml[Main](basic-authentication/samples/sample3.xml?highlight=4)]

<span data-ttu-id="59d9f-153">Substitua "YourAssemblyName" com o nome do assembly (não incluindo a extensão "dll").</span><span class="sxs-lookup"><span data-stu-id="59d9f-153">Replace "YourAssemblyName" with the name of the assembly (not including the "dll" extension).</span></span>

<span data-ttu-id="59d9f-154">Você deve desabilitar outros esquemas de autenticação, como autenticação de formulários ou Windows.</span><span class="sxs-lookup"><span data-stu-id="59d9f-154">You should disable other authentication schemes, such as Forms or Windows auth.</span></span>
