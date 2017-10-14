---
title: "Guia de Introdução com as APIs de proteção de dados"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 39b7a73c-29d4-4137-b311-49529adcf3cb
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/using-data-protection
ms.openlocfilehash: 9489b55b1de626b77bcbe21cb9678e561b559099
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/13/2017
---
# <a name="getting-started-with-the-data-protection-apis"></a><span data-ttu-id="6273a-103">Guia de Introdução com as APIs de proteção de dados</span><span class="sxs-lookup"><span data-stu-id="6273a-103">Getting Started with the Data Protection APIs</span></span>

<a name="security-data-protection-getting-started"></a>

<span data-ttu-id="6273a-104">Em seus dados mais simples de proteção consiste as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="6273a-104">At its simplest protecting data consists of the following steps:</span></span>

1. <span data-ttu-id="6273a-105">Crie um tipo de dados protetor de um provedor de proteção de dados.</span><span class="sxs-lookup"><span data-stu-id="6273a-105">Create a data protector from a data protection provider.</span></span>

2. <span data-ttu-id="6273a-106">Chame o método de proteção com os dados que você deseja proteger.</span><span class="sxs-lookup"><span data-stu-id="6273a-106">Call the Protect method with the data you want to protect.</span></span>

3. <span data-ttu-id="6273a-107">Chame o método Unprotect com os dados que você deseja transformar novamente em texto sem formatação.</span><span class="sxs-lookup"><span data-stu-id="6273a-107">Call the Unprotect method with the data you want to turn back into plain text.</span></span>

<span data-ttu-id="6273a-108">A maioria das estruturas, como ASP.NET ou SignalR já configurar o sistema de proteção de dados e adicioná-lo para um contêiner de serviço, que você pode acessar por meio de injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="6273a-108">Most frameworks such as ASP.NET or SignalR already configure the data protection system and add it to a service container you access via dependency injection.</span></span> <span data-ttu-id="6273a-109">O exemplo a seguir demonstra como configurar um contêiner de serviço para injeção de dependência e registrando a pilha de proteção de dados, recebendo o provedor de proteção de dados por meio de DI, criando um protetor e desproteção e proteção de dados</span><span class="sxs-lookup"><span data-stu-id="6273a-109">The following sample demonstrates configuring a service container for dependency injection and registering the data protection stack, receiving the data protection provider via DI, creating a protector and protecting then unprotecting data</span></span>

[!code-csharp[Main](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

<span data-ttu-id="6273a-110">Quando você cria um protetor você deve fornecer um ou mais [cadeias de caracteres de finalidade](consumer-apis/purpose-strings.md).</span><span class="sxs-lookup"><span data-stu-id="6273a-110">When you create a protector you must provide one or more [Purpose Strings](consumer-apis/purpose-strings.md).</span></span> <span data-ttu-id="6273a-111">Uma cadeia de caracteres de finalidade fornece isolamento entre os consumidores, por exemplo um protetor criado com uma cadeia de caracteres de fim de "green" não poderá desproteger dados fornecidos por um protetor com a finalidade de "roxo".</span><span class="sxs-lookup"><span data-stu-id="6273a-111">A purpose string provides isolation between consumers, for example a protector created with a purpose string of "green" would not be able to unprotect data provided by a protector with a purpose of "purple".</span></span>

>[!TIP]
> <span data-ttu-id="6273a-112">Instâncias de IDataProtectionProvider e IDataProtector são thread-safe para chamadores vários.</span><span class="sxs-lookup"><span data-stu-id="6273a-112">Instances of IDataProtectionProvider and IDataProtector are thread-safe for multiple callers.</span></span> <span data-ttu-id="6273a-113">Destina-se quando um componente obtém uma referência a um IDataProtector por meio de uma chamada para CreateProtector, ele usará essa referência para várias chamadas para proteger e Desproteger.</span><span class="sxs-lookup"><span data-stu-id="6273a-113">It is intended that once a component gets a reference to an IDataProtector via a call to CreateProtector, it will use that reference for multiple calls to Protect and Unprotect.</span></span>
>
><span data-ttu-id="6273a-114">Uma chamada para desproteger lançará CryptographicException se a carga protegida não pode ser verificada ou decifrada.</span><span class="sxs-lookup"><span data-stu-id="6273a-114">A call to Unprotect will throw CryptographicException if the protected payload cannot be verified or deciphered.</span></span> <span data-ttu-id="6273a-115">Alguns componentes poderá ignorar erros durante Desproteger operações; um componente que lê os cookies de autenticação pode manipular esse erro e tratar a solicitação, como não se tivesse nenhum cookie de em vez de falhar a solicitação.</span><span class="sxs-lookup"><span data-stu-id="6273a-115">Some components may wish to ignore errors during unprotect operations; a component which reads authentication cookies might handle this error and treat the request as if it had no cookie at all rather than fail the request outright.</span></span> <span data-ttu-id="6273a-116">Componentes que deseja que esse comportamento especificamente devem capturar CryptographicException em vez de assimilação todas as exceções.</span><span class="sxs-lookup"><span data-stu-id="6273a-116">Components which want this behavior should specifically catch CryptographicException instead of swallowing all exceptions.</span></span>
