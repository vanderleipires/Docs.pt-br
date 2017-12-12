---
uid: web-api/overview/security/integrated-windows-authentication
title: "Autenticação integrada do Windows | Microsoft Docs"
author: MikeWasson
description: "Descreve como usar a autenticação integrada do Windows na API da Web do ASP.NET."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/18/2012
ms.topic: article
ms.assetid: 71ee4c78-c500-4d1c-b761-b4e161a291b5
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/integrated-windows-authentication
msc.type: authoredcontent
ms.openlocfilehash: bf5f55d98d61cdfdd246a847f41a6f1c65f00bfc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="integrated-windows-authentication"></a><span data-ttu-id="8f6e4-103">Autenticação integrada do Windows</span><span class="sxs-lookup"><span data-stu-id="8f6e4-103">Integrated Windows Authentication</span></span>
====================
<span data-ttu-id="8f6e4-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8f6e4-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="8f6e4-105">Autenticação integrada do Windows permite que os usuários façam logon com suas credenciais do Windows, usando Kerberos ou NTLM.</span><span class="sxs-lookup"><span data-stu-id="8f6e4-105">Integrated Windows authentication enables users to log in with their Windows credentials, using Kerberos or NTLM.</span></span> <span data-ttu-id="8f6e4-106">O cliente envia as credenciais no cabeçalho de autorização.</span><span class="sxs-lookup"><span data-stu-id="8f6e4-106">The client sends credentials in the Authorization header.</span></span> <span data-ttu-id="8f6e4-107">Autenticação do Windows é mais adequada para um ambiente de intranet.</span><span class="sxs-lookup"><span data-stu-id="8f6e4-107">Windows authentication is best suited for an intranet environment.</span></span> <span data-ttu-id="8f6e4-108">Para obter mais informações, consulte [autenticação do Windows](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).</span><span class="sxs-lookup"><span data-stu-id="8f6e4-108">For more information, see [Windows Authentication](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).</span></span>

| <span data-ttu-id="8f6e4-109">Vantagens</span><span class="sxs-lookup"><span data-stu-id="8f6e4-109">Advantages</span></span> | <span data-ttu-id="8f6e4-110">Desvantagens</span><span class="sxs-lookup"><span data-stu-id="8f6e4-110">Disadvantages</span></span> |
| --- | --- |
| <span data-ttu-id="8f6e4-111">-Integradas no IIS.</span><span class="sxs-lookup"><span data-stu-id="8f6e4-111">- Built into IIS.</span></span> <span data-ttu-id="8f6e4-112">-Não envia as credenciais do usuário na solicitação.</span><span class="sxs-lookup"><span data-stu-id="8f6e4-112">- Does not send the user credentials in the request.</span></span> <span data-ttu-id="8f6e4-113">-Se o computador cliente pertence ao domínio (por exemplo, o aplicativo intranet), o usuário não precisa inserir credenciais.</span><span class="sxs-lookup"><span data-stu-id="8f6e4-113">- If the client computer belongs to the domain (for example, intranet application), the user does not need to enter credentials.</span></span> | <span data-ttu-id="8f6e4-114">-Não é recomendado para aplicativos da Internet.</span><span class="sxs-lookup"><span data-stu-id="8f6e4-114">- Not recommended for Internet applications.</span></span> <span data-ttu-id="8f6e4-115">-Requer suporte a Kerberos ou NTLM no cliente.</span><span class="sxs-lookup"><span data-stu-id="8f6e4-115">- Requires Kerberos or NTLM support in the client.</span></span> <span data-ttu-id="8f6e4-116">-O cliente deve estar no domínio do Active Directory.</span><span class="sxs-lookup"><span data-stu-id="8f6e4-116">- Client must be in the Active Directory domain.</span></span> |

> [!NOTE]
> <span data-ttu-id="8f6e4-117">Se seu aplicativo é hospedado no Azure e você tiver um domínio do Active Directory local, considere a possibilidade de Federação do AD local com o Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="8f6e4-117">If your application is hosted on Azure and you have an on-premise Active Directory domain, consider federating your on-premise AD with Azure Active Directory.</span></span> <span data-ttu-id="8f6e4-118">Dessa forma, os usuários podem fazer com suas credenciais locais, mas a autenticação é realizada pelo AD do Azure.</span><span class="sxs-lookup"><span data-stu-id="8f6e4-118">That way, users can log in with their on-premise credentials, but the authentication is performed by Azure AD.</span></span> <span data-ttu-id="8f6e4-119">Para obter mais informações, consulte [autenticação Azure](../../../visual-studio/overview/2012/windows-azure-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="8f6e4-119">For more information, see [Azure Authentication](../../../visual-studio/overview/2012/windows-azure-authentication.md).</span></span>


<span data-ttu-id="8f6e4-120">Para criar um aplicativo que usa a autenticação integrada do Windows, selecione o modelo de "Aplicativo Intranet" no Assistente de projeto MVC 4.</span><span class="sxs-lookup"><span data-stu-id="8f6e4-120">To create an application that uses Integrated Windows authentication, select the "Intranet Application" template in the MVC 4 project wizard.</span></span> <span data-ttu-id="8f6e4-121">Este modelo de projeto coloca a configuração a seguir no arquivo Web. config:</span><span class="sxs-lookup"><span data-stu-id="8f6e4-121">This project template puts the following setting in the Web.config file:</span></span>

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

<span data-ttu-id="8f6e4-122">No lado do cliente, a autenticação integrada do Windows funciona com qualquer navegador que ofereça suporte a [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) esquema de autenticação, que inclui a maioria dos navegadores principais.</span><span class="sxs-lookup"><span data-stu-id="8f6e4-122">On the client side, Integrated Windows authentication works with any browser that supports the [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) authentication scheme, which includes most major browsers.</span></span> <span data-ttu-id="8f6e4-123">Para aplicativos de cliente .NET, o **HttpClient** classe dá suporte à autenticação do Windows:</span><span class="sxs-lookup"><span data-stu-id="8f6e4-123">For .NET client applications, the **HttpClient** class supports Windows authentication:</span></span>

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

<span data-ttu-id="8f6e4-124">Autenticação do Windows é vulnerável a ataques CSRF (falsificação) de solicitação entre sites.</span><span class="sxs-lookup"><span data-stu-id="8f6e4-124">Windows authentication is vulnerable to cross-site request forgery (CSRF) attacks.</span></span> <span data-ttu-id="8f6e4-125">Consulte [impedindo ataques CSRF (falsificação) de solicitação entre sites](preventing-cross-site-request-forgery-csrf-attacks.md).</span><span class="sxs-lookup"><span data-stu-id="8f6e4-125">See [Preventing Cross-Site Request Forgery (CSRF) Attacks](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>
