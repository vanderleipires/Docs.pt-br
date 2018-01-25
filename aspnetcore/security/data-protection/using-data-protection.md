---
title: "Guia de Introdução com as APIs de proteção de dados"
author: rick-anderson
description: "Este documento explica como usar as APIs de proteção de dados ASP.NET Core para proteger e ao desproteger dados em um aplicativo."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/using-data-protection
ms.openlocfilehash: c6a631b6dc4a7855b11031dfcef42b17906754b0
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
# <a name="getting-started-with-the-data-protection-apis"></a><span data-ttu-id="0e050-103">Guia de Introdução com as APIs de proteção de dados</span><span class="sxs-lookup"><span data-stu-id="0e050-103">Getting Started with the Data Protection APIs</span></span>

<a name="security-data-protection-getting-started"></a>

<span data-ttu-id="0e050-104">Em seus dados mais simples, protegendo consiste as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="0e050-104">At its simplest, protecting data consists of the following steps:</span></span>

1. <span data-ttu-id="0e050-105">Crie um tipo de dados protetor de um provedor de proteção de dados.</span><span class="sxs-lookup"><span data-stu-id="0e050-105">Create a data protector from a data protection provider.</span></span>

2. <span data-ttu-id="0e050-106">Chamar o `Protect` método com os dados que você deseja proteger.</span><span class="sxs-lookup"><span data-stu-id="0e050-106">Call the `Protect` method with the data you want to protect.</span></span>

3. <span data-ttu-id="0e050-107">Chamar o `Unprotect` método com os dados que você deseja transformar novamente em texto sem formatação.</span><span class="sxs-lookup"><span data-stu-id="0e050-107">Call the `Unprotect` method with the data you want to turn back into plain text.</span></span>

<span data-ttu-id="0e050-108">A maioria das estruturas e modelos de aplicativo, como ASP.NET ou SignalR, já que configurar o sistema de proteção de dados e adicioná-lo para um contêiner de serviço, que você pode acessar por meio de injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="0e050-108">Most frameworks and app models, such as ASP.NET or SignalR, already configure the data protection system and add it to a service container you access via dependency injection.</span></span> <span data-ttu-id="0e050-109">O exemplo a seguir demonstra como configurar um contêiner de serviço para injeção de dependência e registrando a pilha de proteção de dados, recebendo o provedor de proteção de dados por meio de DI, criando um protetor e desproteção e proteção de dados</span><span class="sxs-lookup"><span data-stu-id="0e050-109">The following sample demonstrates configuring a service container for dependency injection and registering the data protection stack, receiving the data protection provider via DI, creating a protector and protecting then unprotecting data</span></span>

[!code-csharp[Main](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

<span data-ttu-id="0e050-110">Quando você cria um protetor você deve fornecer um ou mais [cadeias de caracteres de finalidade](consumer-apis/purpose-strings.md).</span><span class="sxs-lookup"><span data-stu-id="0e050-110">When you create a protector you must provide one or more [Purpose Strings](consumer-apis/purpose-strings.md).</span></span> <span data-ttu-id="0e050-111">Uma cadeia de caracteres de finalidade fornece isolamento entre os consumidores.</span><span class="sxs-lookup"><span data-stu-id="0e050-111">A purpose string provides isolation between consumers.</span></span> <span data-ttu-id="0e050-112">Por exemplo, um protetor criado com uma cadeia de caracteres de fim de "green" não será possível desproteger dados fornecidos por um protetor com a finalidade de "roxo".</span><span class="sxs-lookup"><span data-stu-id="0e050-112">For example, a protector created with a purpose string of "green" wouldn't be able to unprotect data provided by a protector with a purpose of "purple".</span></span>

>[!TIP]
> <span data-ttu-id="0e050-113">Instâncias do `IDataProtectionProvider` e `IDataProtector` são thread-safe para chamadores vários.</span><span class="sxs-lookup"><span data-stu-id="0e050-113">Instances of `IDataProtectionProvider` and `IDataProtector` are thread-safe for multiple callers.</span></span> <span data-ttu-id="0e050-114">Ele foi desenvolvido que depois que um componente obtém uma referência a um `IDataProtector` por meio de uma chamada para `CreateProtector`, ele usará essa referência para várias chamadas para `Protect` e `Unprotect`.</span><span class="sxs-lookup"><span data-stu-id="0e050-114">It's intended that once a component gets a reference to an `IDataProtector` via a call to `CreateProtector`, it will use that reference for multiple calls to `Protect` and `Unprotect`.</span></span>
>
><span data-ttu-id="0e050-115">Uma chamada para `Unprotect` lançará CryptographicException se a carga protegida não pode ser verificada ou decifrada.</span><span class="sxs-lookup"><span data-stu-id="0e050-115">A call to `Unprotect` will throw CryptographicException if the protected payload cannot be verified or deciphered.</span></span> <span data-ttu-id="0e050-116">Alguns componentes poderá ignorar erros durante Desproteger operações; um componente que lê os cookies de autenticação pode manipular esse erro e tratar a solicitação, como não se tivesse nenhum cookie de em vez de falhar a solicitação.</span><span class="sxs-lookup"><span data-stu-id="0e050-116">Some components may wish to ignore errors during unprotect operations; a component which reads authentication cookies might handle this error and treat the request as if it had no cookie at all rather than fail the request outright.</span></span> <span data-ttu-id="0e050-117">Componentes que deseja que esse comportamento especificamente devem capturar CryptographicException em vez de assimilação todas as exceções.</span><span class="sxs-lookup"><span data-stu-id="0e050-117">Components which want this behavior should specifically catch CryptographicException instead of swallowing all exceptions.</span></span>
