---
uid: webhooks/receiving/dependencies
title: Dependências do receptor de ASP.NET WebHooks | Microsoft Docs
author: rick-anderson
description: Injeção de dependência no ASP.NET WebHooks e dependências do destinatário.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.technology: ''
ms.prod: .net-framework
ms.openlocfilehash: f9726c746c8934594e26f2871f9b867c192374bb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26529905"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a><span data-ttu-id="3f08a-103">Dependências de destinatário do ASP.NET WebHooks</span><span class="sxs-lookup"><span data-stu-id="3f08a-103">ASP.NET WebHooks receiver dependencies</span></span>

<span data-ttu-id="3f08a-104">Microsoft ASP.NET WebHooks destina-se com a injeção de dependência em mente.</span><span class="sxs-lookup"><span data-stu-id="3f08a-104">Microsoft ASP.NET WebHooks is designed with dependency injection in mind.</span></span> <span data-ttu-id="3f08a-105">A maioria das dependências do sistema podem ser substituídas por implementações alternativas usando um mecanismo de injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="3f08a-105">Most dependencies in the system can be replaced with alternative implementations using a dependency injection engine.</span></span>

<span data-ttu-id="3f08a-106">Consulte [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) para obter uma lista de dependências do destinatário.</span><span class="sxs-lookup"><span data-stu-id="3f08a-106">Please see [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) for a list of receiver dependencies.</span></span> <span data-ttu-id="3f08a-107">Se nenhuma dependência foi registrada, uma implementação padrão será usada.</span><span class="sxs-lookup"><span data-stu-id="3f08a-107">If no dependency has been registered, a default implementation is used.</span></span> <span data-ttu-id="3f08a-108">Consulte [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) para obter uma lista das implementações do padrão.</span><span class="sxs-lookup"><span data-stu-id="3f08a-108">Please see [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) for a list of default implementations.</span></span>
