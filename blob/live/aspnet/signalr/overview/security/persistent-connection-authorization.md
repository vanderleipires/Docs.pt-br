---
uid: signalr/overview/security/persistent-connection-authorization
title: "Autenticação e autorização para conexões persistentes SignalR | Microsoft Docs"
author: pfletcher
description: "Este tópico descreve como implantar a autorização em uma conexão persistente. Para obter informações gerais sobre como integrar a segurança em um aplicativo de SignalR,..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: e264677b-9c01-47ec-94f9-3cd8f08f94af
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/security/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 9c6fff86ae6b1b65e6ba9922b6b8448643ef1f15
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="authentication-and-authorization-for-signalr-persistent-connections"></a><span data-ttu-id="5ebf2-104">Autenticação e autorização para conexões persistentes SignalR</span><span class="sxs-lookup"><span data-stu-id="5ebf2-104">Authentication and Authorization for SignalR Persistent Connections</span></span>
====================
<span data-ttu-id="5ebf2-105">por [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="5ebf2-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="5ebf2-106">Este tópico descreve como implantar a autorização em uma conexão persistente.</span><span class="sxs-lookup"><span data-stu-id="5ebf2-106">This topic describes how to enforce authorization on a persistent connection.</span></span> <span data-ttu-id="5ebf2-107">Para obter informações gerais sobre como integrar a segurança em um aplicativo SignalR, consulte [Introdução à segurança](introduction-to-security.md).</span><span class="sxs-lookup"><span data-stu-id="5ebf2-107">For general information about integrating security into a SignalR application, see [Introduction to Security](introduction-to-security.md).</span></span> 
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="5ebf2-108">Versões de software usadas neste tópico</span><span class="sxs-lookup"><span data-stu-id="5ebf2-108">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="5ebf2-109">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="5ebf2-109">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="5ebf2-110">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="5ebf2-110">.NET 4.5</span></span>
> - <span data-ttu-id="5ebf2-111">SignalR versão 2</span><span class="sxs-lookup"><span data-stu-id="5ebf2-111">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="5ebf2-112">Versões anteriores deste tópico</span><span class="sxs-lookup"><span data-stu-id="5ebf2-112">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="5ebf2-113">Para obter informações sobre versões anteriores do SignalR, consulte [versões mais antigas do SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="5ebf2-113">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="5ebf2-114">Perguntas e comentários</span><span class="sxs-lookup"><span data-stu-id="5ebf2-114">Questions and comments</span></span>
> 
> <span data-ttu-id="5ebf2-115">Deixe comentários em como você gostou neste tutorial e o que podemos melhorar nos comentários na parte inferior da página.</span><span class="sxs-lookup"><span data-stu-id="5ebf2-115">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="5ebf2-116">Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, você poderá postá-los para o [ASP.NET SignalR fórum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="5ebf2-116">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="enforce-authorization"></a><span data-ttu-id="5ebf2-117">Implantar a autorização</span><span class="sxs-lookup"><span data-stu-id="5ebf2-117">Enforce authorization</span></span>

<span data-ttu-id="5ebf2-118">Para impor regras de autorização ao usar um [PersistentConnection](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) você deve substituir o `AuthorizeRequest` método.</span><span class="sxs-lookup"><span data-stu-id="5ebf2-118">To enforce authorization rules when using a [PersistentConnection](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) you must override the `AuthorizeRequest` method.</span></span> <span data-ttu-id="5ebf2-119">Não é possível usar o `Authorize` atributo com conexões persistentes.</span><span class="sxs-lookup"><span data-stu-id="5ebf2-119">You cannot use the `Authorize` attribute with persistent connections.</span></span> <span data-ttu-id="5ebf2-120">O `AuthorizeRequest` método é chamado pelo SignalR Framework antes de cada solicitação para verificar se o usuário está autorizado a executar a ação solicitada.</span><span class="sxs-lookup"><span data-stu-id="5ebf2-120">The `AuthorizeRequest` method is called by the SignalR Framework before every request to verify that the user is authorized to perform the requested action.</span></span> <span data-ttu-id="5ebf2-121">O `AuthorizeRequest` método não é chamado do cliente; em vez disso, autenticar o usuário por meio do mecanismo de autenticação padrão do seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5ebf2-121">The `AuthorizeRequest` method is not called from the client; instead, you authenticate the user through your application's standard authentication mechanism.</span></span>

<span data-ttu-id="5ebf2-122">O exemplo a seguir mostra como limitar solicitações para usuários autenticados.</span><span class="sxs-lookup"><span data-stu-id="5ebf2-122">The example below shows how to limit requests to authenticated users.</span></span>

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

<span data-ttu-id="5ebf2-123">Você pode adicionar qualquer lógica de autorização personalizado no método AuthorizeRequest; como verificar se um usuário pertencer a uma função específica.</span><span class="sxs-lookup"><span data-stu-id="5ebf2-123">You can add any customized authorization logic in the AuthorizeRequest method; such as, checking whether a user belongs to a particular role.</span></span>
