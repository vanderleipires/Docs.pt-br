---
uid: web-api/overview/security/basic-authentication
title: Autenticação básica na API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: Descreve como usar a autenticação básica na API Web ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/02/2014
ms.topic: article
ms.assetid: 41423767-0021-47c3-9e53-0021b457c39f
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/security/basic-authentication
msc.type: authoredcontent
ms.openlocfilehash: 9d5610eb61088a8e7573ba6399c771f0957ff437
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395325"
---
<a name="basic-authentication-in-aspnet-web-api"></a><span data-ttu-id="867b6-103">Autenticação básica na API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="867b6-103">Basic Authentication in ASP.NET Web API</span></span>
====================
<span data-ttu-id="867b6-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="867b6-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="867b6-105">Autenticação básica é definida no [RFC 2617, autenticação HTTP: autenticação básica e Digest Access](http://www.ietf.org/rfc/rfc2617.txt).</span><span class="sxs-lookup"><span data-stu-id="867b6-105">Basic authentication is defined in [RFC 2617, HTTP Authentication: Basic and Digest Access Authentication](http://www.ietf.org/rfc/rfc2617.txt).</span></span>

<span data-ttu-id="867b6-106">Desvantagens</span><span class="sxs-lookup"><span data-stu-id="867b6-106">Disadvantages</span></span>

- <span data-ttu-id="867b6-107">As credenciais do usuário são enviadas na solicitação.</span><span class="sxs-lookup"><span data-stu-id="867b6-107">User credentials are sent in the request.</span></span>
- <span data-ttu-id="867b6-108">As credenciais são enviadas como texto sem formatação.</span><span class="sxs-lookup"><span data-stu-id="867b6-108">Credentials are sent as plaintext.</span></span>
- <span data-ttu-id="867b6-109">As credenciais são enviadas com cada solicitação.</span><span class="sxs-lookup"><span data-stu-id="867b6-109">Credentials are sent with every request.</span></span>
- <span data-ttu-id="867b6-110">Nenhuma maneira de fazer logoff, exceto encerrando a sessão do navegador.</span><span class="sxs-lookup"><span data-stu-id="867b6-110">No way to log out, except by ending the browser session.</span></span>
- <span data-ttu-id="867b6-111">Vulnerável a cruzada solicitação intersite forjada (CSRF); requer que medidas anti-CSRF.</span><span class="sxs-lookup"><span data-stu-id="867b6-111">Vulnerable to cross-site request forgery (CSRF); requires anti-CSRF measures.</span></span>

<span data-ttu-id="867b6-112">Vantagens</span><span class="sxs-lookup"><span data-stu-id="867b6-112">Advantages</span></span>

- <span data-ttu-id="867b6-113">Padrão da Internet.</span><span class="sxs-lookup"><span data-stu-id="867b6-113">Internet standard.</span></span>
- <span data-ttu-id="867b6-114">Suporte para todos os principais navegadores.</span><span class="sxs-lookup"><span data-stu-id="867b6-114">Supported by all major browsers.</span></span>
- <span data-ttu-id="867b6-115">Protocolo relativamente simple.</span><span class="sxs-lookup"><span data-stu-id="867b6-115">Relatively simple protocol.</span></span>

<span data-ttu-id="867b6-116">Autenticação básica funciona da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="867b6-116">Basic authentication works as follows:</span></span>

1. <span data-ttu-id="867b6-117">Se uma solicitação requer autenticação, o servidor retorna 401 (não autorizado).</span><span class="sxs-lookup"><span data-stu-id="867b6-117">If a request requires authentication, the server returns 401 (Unauthorized).</span></span> <span data-ttu-id="867b6-118">A resposta inclui um cabeçalho WWW-Authenticate, que indica que o servidor dá suporte à autenticação básica.</span><span class="sxs-lookup"><span data-stu-id="867b6-118">The response includes a WWW-Authenticate header, indicating the server supports Basic authentication.</span></span>
2. <span data-ttu-id="867b6-119">O cliente envia outra solicitação, com as credenciais do cliente no cabeçalho de autorização.</span><span class="sxs-lookup"><span data-stu-id="867b6-119">The client sends another request, with the client credentials in the Authorization header.</span></span> <span data-ttu-id="867b6-120">As credenciais são formatadas como a cadeia de caracteres "nome: senha", codificado na base64.</span><span class="sxs-lookup"><span data-stu-id="867b6-120">The credentials are formatted as the string "name:password", base64-encoded.</span></span> <span data-ttu-id="867b6-121">As credenciais não são criptografadas.</span><span class="sxs-lookup"><span data-stu-id="867b6-121">The credentials are not encrypted.</span></span>

<span data-ttu-id="867b6-122">Autenticação básica é executada dentro do contexto de um "Território".</span><span class="sxs-lookup"><span data-stu-id="867b6-122">Basic authentication is performed within the context of a "realm."</span></span> <span data-ttu-id="867b6-123">O servidor inclui o nome do território no cabeçalho WWW-Authenticate.</span><span class="sxs-lookup"><span data-stu-id="867b6-123">The server includes the name of the realm in the WWW-Authenticate header.</span></span> <span data-ttu-id="867b6-124">As credenciais do usuário são válidas dentro desse realm.</span><span class="sxs-lookup"><span data-stu-id="867b6-124">The user's credentials are valid within that realm.</span></span> <span data-ttu-id="867b6-125">O escopo exato de um território é definido pelo servidor.</span><span class="sxs-lookup"><span data-stu-id="867b6-125">The exact scope of a realm is defined by the server.</span></span> <span data-ttu-id="867b6-126">Por exemplo, você pode definir vários territórios em ordem para a partição de recursos.</span><span class="sxs-lookup"><span data-stu-id="867b6-126">For example, you might define several realms in order to partition resources.</span></span>

![](basic-authentication/_static/image1.png)

<span data-ttu-id="867b6-127">Como as credenciais são enviadas sem criptografia, autenticação básica é segura via HTTPS.</span><span class="sxs-lookup"><span data-stu-id="867b6-127">Because the credentials are sent unencrypted, Basic authentication is only secure over HTTPS.</span></span> <span data-ttu-id="867b6-128">Ver [trabalhando com SSL na API Web](working-with-ssl-in-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="867b6-128">See [Working with SSL in Web API](working-with-ssl-in-web-api.md).</span></span>

<span data-ttu-id="867b6-129">A autenticação básica também é vulnerável a ataques de CSRF.</span><span class="sxs-lookup"><span data-stu-id="867b6-129">Basic authentication is also vulnerable to CSRF attacks.</span></span> <span data-ttu-id="867b6-130">Depois que o usuário insere as credenciais, o navegador automaticamente envia-os em solicitações subsequentes ao mesmo domínio, para a duração da sessão.</span><span class="sxs-lookup"><span data-stu-id="867b6-130">After the user enters credentials, the browser automatically sends them on subsequent requests to the same domain, for the duration of the session.</span></span> <span data-ttu-id="867b6-131">Isso inclui solicitações AJAX.</span><span class="sxs-lookup"><span data-stu-id="867b6-131">This includes AJAX requests.</span></span> <span data-ttu-id="867b6-132">Ver [impedindo solicitação intersite forjada (CSRF) ataques](preventing-cross-site-request-forgery-csrf-attacks.md).</span><span class="sxs-lookup"><span data-stu-id="867b6-132">See [Preventing Cross-Site Request Forgery (CSRF) Attacks](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>

## <a name="basic-authentication-with-iis"></a><span data-ttu-id="867b6-133">Autenticação básica com o IIS</span><span class="sxs-lookup"><span data-stu-id="867b6-133">Basic Authentication with IIS</span></span>

<span data-ttu-id="867b6-134">O IIS dá suporte à autenticação básica, mas há uma limitação: O usuário é autenticado em relação às suas credenciais do Windows.</span><span class="sxs-lookup"><span data-stu-id="867b6-134">IIS supports Basic authentication, but there is a caveat: The user is authenticated against their Windows credentials.</span></span> <span data-ttu-id="867b6-135">Isso significa que o usuário deve ter uma conta no domínio do servidor.</span><span class="sxs-lookup"><span data-stu-id="867b6-135">That means the user must have an account on the server's domain.</span></span> <span data-ttu-id="867b6-136">Para um site voltado ao público, você geralmente deseja autenticar em relação a um provedor de associação do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="867b6-136">For a public-facing web site, you typically want to authenticate against an ASP.NET membership provider.</span></span>

<span data-ttu-id="867b6-137">Para habilitar a autenticação básica usando o IIS, defina o modo de autenticação como "Windows" em Web. config do seu projeto do ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="867b6-137">To enable Basic authentication using IIS, set the authentication mode to "Windows" in the Web.config of your ASP.NET project:</span></span>

[!code-xml[Main](basic-authentication/samples/sample1.xml)]

<span data-ttu-id="867b6-138">Nesse modo, o IIS usa as credenciais do Windows para autenticar.</span><span class="sxs-lookup"><span data-stu-id="867b6-138">In this mode, IIS uses Windows credentials to authenticate.</span></span> <span data-ttu-id="867b6-139">Além disso, você deve habilitar a autenticação básica no IIS.</span><span class="sxs-lookup"><span data-stu-id="867b6-139">In addition, you must enable Basic authentication in IIS.</span></span> <span data-ttu-id="867b6-140">No Gerenciador do IIS, vá para a exibição de recursos, selecione a autenticação e habilitar a autenticação básica.</span><span class="sxs-lookup"><span data-stu-id="867b6-140">In IIS Manager, go to Features View, select Authentication, and enable Basic authentication.</span></span>

![](basic-authentication/_static/image2.png)

<span data-ttu-id="867b6-141">Em seu projeto de API da Web, adicione o `[Authorize]` atributo para quaisquer ações do controlador que precisam de autenticação.</span><span class="sxs-lookup"><span data-stu-id="867b6-141">In your Web API project, add the `[Authorize]` attribute for any controller actions that need authentication.</span></span>

<span data-ttu-id="867b6-142">Um cliente se autentica, definindo o cabeçalho de autorização na solicitação.</span><span class="sxs-lookup"><span data-stu-id="867b6-142">A client authenticates itself by setting the Authorization header in the request.</span></span> <span data-ttu-id="867b6-143">Clientes de navegador executam automaticamente essa etapa.</span><span class="sxs-lookup"><span data-stu-id="867b6-143">Browser clients perform this step automatically.</span></span> <span data-ttu-id="867b6-144">Clientes nonbrowser serão necessário definir o cabeçalho.</span><span class="sxs-lookup"><span data-stu-id="867b6-144">Nonbrowser clients will need to set the header.</span></span>

## <a name="basic-authentication-with-custom-membership"></a><span data-ttu-id="867b6-145">Autenticação básica com associação personalizado</span><span class="sxs-lookup"><span data-stu-id="867b6-145">Basic Authentication with Custom Membership</span></span>

<span data-ttu-id="867b6-146">Conforme mencionado, a autenticação básica integradas no IIS usa as credenciais do Windows.</span><span class="sxs-lookup"><span data-stu-id="867b6-146">As mentioned, the Basic Authentication built into IIS uses Windows credentials.</span></span> <span data-ttu-id="867b6-147">Isso significa que você precisa criar contas para os usuários no servidor de hospedagem.</span><span class="sxs-lookup"><span data-stu-id="867b6-147">That means you need to create accounts for your users on the hosting server.</span></span> <span data-ttu-id="867b6-148">Mas, para um aplicativo de internet, as contas de usuário normalmente são armazenadas em um banco de dados externo.</span><span class="sxs-lookup"><span data-stu-id="867b6-148">But for an internet application, user accounts are typically stored in an external database.</span></span>

<span data-ttu-id="867b6-149">O código a seguir como um módulo HTTP que realiza a autenticação básica.</span><span class="sxs-lookup"><span data-stu-id="867b6-149">The following code how an HTTP module that performs Basic Authentication.</span></span> <span data-ttu-id="867b6-150">Você pode conectar facilmente em um provedor de associação do ASP.NET, substituindo o `CheckPassword` método, que é um método fictício neste exemplo.</span><span class="sxs-lookup"><span data-stu-id="867b6-150">You can easily plug in an ASP.NET membership provider by replacing the `CheckPassword` method, which is a dummy method in this example.</span></span>

<span data-ttu-id="867b6-151">Na API Web 2, você deve considerar escrever uma [filtro de autenticação](authentication-filters.md) ou [middleware OWIN](../../../aspnet/overview/owin-and-katana/index.md), em vez de um módulo HTTP.</span><span class="sxs-lookup"><span data-stu-id="867b6-151">In Web API 2, you should consider writing an [authentication filter](authentication-filters.md) or [OWIN middleware](../../../aspnet/overview/owin-and-katana/index.md), instead of an HTTP module.</span></span>

[!code-csharp[Main](basic-authentication/samples/sample2.cs)]

<span data-ttu-id="867b6-152">Para habilitar o módulo HTTP, adicione o seguinte ao arquivo Web. config na **System. webServer** seção:</span><span class="sxs-lookup"><span data-stu-id="867b6-152">To enable the HTTP module, add the following to your web.config file in the **system.webServer** section:</span></span>

[!code-xml[Main](basic-authentication/samples/sample3.xml?highlight=4)]

<span data-ttu-id="867b6-153">Substitua "YourAssemblyName" com o nome do assembly (não incluindo a extensão "dll").</span><span class="sxs-lookup"><span data-stu-id="867b6-153">Replace "YourAssemblyName" with the name of the assembly (not including the "dll" extension).</span></span>

<span data-ttu-id="867b6-154">Você deve desabilitar a outras esquemas de autenticação, como autenticação de formulários ou Windows.</span><span class="sxs-lookup"><span data-stu-id="867b6-154">You should disable other authentication schemes, such as Forms or Windows auth.</span></span>
