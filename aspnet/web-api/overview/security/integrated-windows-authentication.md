---
uid: web-api/overview/security/integrated-windows-authentication
title: Autenticação do Windows integrada | Microsoft Docs
author: MikeWasson
description: Descreve como usar a autenticação integrada do Windows na API Web ASP.NET.
ms.author: riande
ms.date: 12/18/2012
ms.assetid: 71ee4c78-c500-4d1c-b761-b4e161a291b5
msc.legacyurl: /web-api/overview/security/integrated-windows-authentication
msc.type: authoredcontent
ms.openlocfilehash: 13dead421abf7ded73cbb2e5f87e54b1a869b5d4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824275"
---
<a name="integrated-windows-authentication"></a><span data-ttu-id="2156b-103">Autenticação integrada do Windows</span><span class="sxs-lookup"><span data-stu-id="2156b-103">Integrated Windows Authentication</span></span>
====================
<span data-ttu-id="2156b-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="2156b-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="2156b-105">Autenticação integrada do Windows permite que os usuários façam logon com suas credenciais do Windows, usando Kerberos ou NTLM.</span><span class="sxs-lookup"><span data-stu-id="2156b-105">Integrated Windows authentication enables users to log in with their Windows credentials, using Kerberos or NTLM.</span></span> <span data-ttu-id="2156b-106">O cliente envia as credenciais no cabeçalho de autorização.</span><span class="sxs-lookup"><span data-stu-id="2156b-106">The client sends credentials in the Authorization header.</span></span> <span data-ttu-id="2156b-107">Autenticação do Windows é mais adequada para um ambiente de intranet.</span><span class="sxs-lookup"><span data-stu-id="2156b-107">Windows authentication is best suited for an intranet environment.</span></span> <span data-ttu-id="2156b-108">Para obter mais informações, consulte [autenticação do Windows](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).</span><span class="sxs-lookup"><span data-stu-id="2156b-108">For more information, see [Windows Authentication](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).</span></span>

| <span data-ttu-id="2156b-109">Vantagens</span><span class="sxs-lookup"><span data-stu-id="2156b-109">Advantages</span></span> | <span data-ttu-id="2156b-110">Desvantagens</span><span class="sxs-lookup"><span data-stu-id="2156b-110">Disadvantages</span></span> |
| --- | --- |
| <span data-ttu-id="2156b-111">-Incorporada ao IIS.</span><span class="sxs-lookup"><span data-stu-id="2156b-111">- Built into IIS.</span></span> <span data-ttu-id="2156b-112">-Não envia as credenciais do usuário na solicitação.</span><span class="sxs-lookup"><span data-stu-id="2156b-112">- Does not send the user credentials in the request.</span></span> <span data-ttu-id="2156b-113">-Se o computador cliente pertença ao domínio (por exemplo, o aplicativo de intranet), o usuário não precisa inserir credenciais.</span><span class="sxs-lookup"><span data-stu-id="2156b-113">- If the client computer belongs to the domain (for example, intranet application), the user does not need to enter credentials.</span></span> | <span data-ttu-id="2156b-114">-Não é recomendado para aplicativos da Internet.</span><span class="sxs-lookup"><span data-stu-id="2156b-114">- Not recommended for Internet applications.</span></span> <span data-ttu-id="2156b-115">-Requer suporte a Kerberos ou NTLM no cliente.</span><span class="sxs-lookup"><span data-stu-id="2156b-115">- Requires Kerberos or NTLM support in the client.</span></span> <span data-ttu-id="2156b-116">-Cliente deve estar no domínio do Active Directory.</span><span class="sxs-lookup"><span data-stu-id="2156b-116">- Client must be in the Active Directory domain.</span></span> |

> [!NOTE]
> <span data-ttu-id="2156b-117">Se o aplicativo estiver hospedado no Azure e você tiver um domínio do Active Directory local, considere a possibilidade de federar seu AD local com o Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="2156b-117">If your application is hosted on Azure and you have an on-premise Active Directory domain, consider federating your on-premise AD with Azure Active Directory.</span></span> <span data-ttu-id="2156b-118">Dessa forma, os usuários podem fazer com suas credenciais locais, mas a autenticação é realizada pelo Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2156b-118">That way, users can log in with their on-premise credentials, but the authentication is performed by Azure AD.</span></span> <span data-ttu-id="2156b-119">Para obter mais informações, consulte [autenticação do Azure](../../../visual-studio/overview/2012/windows-azure-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="2156b-119">For more information, see [Azure Authentication](../../../visual-studio/overview/2012/windows-azure-authentication.md).</span></span>


<span data-ttu-id="2156b-120">Para criar um aplicativo que usa a autenticação do Windows integrada, selecione o modelo "Aplicativo de Intranet" no Assistente de projeto MVC 4.</span><span class="sxs-lookup"><span data-stu-id="2156b-120">To create an application that uses Integrated Windows authentication, select the "Intranet Application" template in the MVC 4 project wizard.</span></span> <span data-ttu-id="2156b-121">Este modelo de projeto coloca a seguinte configuração no arquivo Web. config:</span><span class="sxs-lookup"><span data-stu-id="2156b-121">This project template puts the following setting in the Web.config file:</span></span>

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

<span data-ttu-id="2156b-122">No lado do cliente, autenticação do Windows integrada funciona com qualquer navegador que ofereça suporte a [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) esquema de autenticação, que inclui a maioria dos principais navegadores.</span><span class="sxs-lookup"><span data-stu-id="2156b-122">On the client side, Integrated Windows authentication works with any browser that supports the [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) authentication scheme, which includes most major browsers.</span></span> <span data-ttu-id="2156b-123">Para aplicativos de cliente .NET, o **HttpClient** classe dá suporte à autenticação do Windows:</span><span class="sxs-lookup"><span data-stu-id="2156b-123">For .NET client applications, the **HttpClient** class supports Windows authentication:</span></span>

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

<span data-ttu-id="2156b-124">A autenticação do Windows é vulnerável a ataques de solicitação forjada (CSRF) entre sites.</span><span class="sxs-lookup"><span data-stu-id="2156b-124">Windows authentication is vulnerable to cross-site request forgery (CSRF) attacks.</span></span> <span data-ttu-id="2156b-125">Ver [impedindo solicitação intersite forjada (CSRF) ataques](preventing-cross-site-request-forgery-csrf-attacks.md).</span><span class="sxs-lookup"><span data-stu-id="2156b-125">See [Preventing Cross-Site Request Forgery (CSRF) Attacks](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>
