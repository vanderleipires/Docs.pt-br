---
title: Limitar o tempo de vida das cargas protegidos
author: rick-anderson
description: "Este documento explica como limitar o tempo de vida de uma carga protegido usando as APIs de proteção de dados ASP.NET Core."
keywords: "ASP.NET Core, proteção de dados, o tempo de vida de carga"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 000175e2-10fc-43dd-bfc2-51e004b97b44
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/limited-lifetime-payloads
ms.openlocfilehash: 3fe2e97db118a92cf6fa003b9ce8a9dedf253136
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="limiting-the-lifetime-of-protected-payloads"></a><span data-ttu-id="cccd8-104">Limitar o tempo de vida das cargas protegidos</span><span class="sxs-lookup"><span data-stu-id="cccd8-104">Limiting the lifetime of protected payloads</span></span>

<span data-ttu-id="cccd8-105">Há cenários onde quer que o desenvolvedor do aplicativo para criar uma carga protegida que expira após um período de tempo definido.</span><span class="sxs-lookup"><span data-stu-id="cccd8-105">There are scenarios where the application developer wants to create a protected payload that expires after a set period of time.</span></span> <span data-ttu-id="cccd8-106">Por exemplo, a carga protegida pode representar um token de redefinição de senha que deve ser válido somente por uma hora.</span><span class="sxs-lookup"><span data-stu-id="cccd8-106">For instance, the protected payload might represent a password reset token that should only be valid for one hour.</span></span> <span data-ttu-id="cccd8-107">É certamente possível para o desenvolvedor criar seu próprio formato de carga que contém uma data de expiração incorporados e os desenvolvedores avançados talvez queira fazer isso mesmo assim, mas para a maioria dos desenvolvedores gerenciar esses expirações pode crescer entediante.</span><span class="sxs-lookup"><span data-stu-id="cccd8-107">It is certainly possible for the developer to create their own payload format that contains an embedded expiration date, and advanced developers may wish to do this anyway, but for the majority of developers managing these expirations can grow tedious.</span></span>

<span data-ttu-id="cccd8-108">Para facilitar isso para nossos público de desenvolvedor, o pacote [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) contém APIs do utilitário para criar cargas expirarem automaticamente após um período de tempo definido.</span><span class="sxs-lookup"><span data-stu-id="cccd8-108">To make this easier for our developer audience, the package [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) contains utility APIs for creating payloads that automatically expire after a set period of time.</span></span> <span data-ttu-id="cccd8-109">Essas APIs de suspensão do `ITimeLimitedDataProtector` tipo.</span><span class="sxs-lookup"><span data-stu-id="cccd8-109">These APIs hang off of the `ITimeLimitedDataProtector` type.</span></span>

## <a name="api-usage"></a><span data-ttu-id="cccd8-110">Uso de API</span><span class="sxs-lookup"><span data-stu-id="cccd8-110">API usage</span></span>

<span data-ttu-id="cccd8-111">O `ITimeLimitedDataProtector` interface é a interface principal para proteger e ao desproteger cargas de tempo limitado / expiração automática.</span><span class="sxs-lookup"><span data-stu-id="cccd8-111">The `ITimeLimitedDataProtector` interface is the core interface for protecting and unprotecting time-limited / self-expiring payloads.</span></span> <span data-ttu-id="cccd8-112">Para criar uma instância de um `ITimeLimitedDataProtector`, você precisará de uma instância de uma expressão [IDataProtector](overview.md) construído com uma finalidade específica.</span><span class="sxs-lookup"><span data-stu-id="cccd8-112">To create an instance of an `ITimeLimitedDataProtector`, you'll first need an instance of a regular [IDataProtector](overview.md) constructed with a specific purpose.</span></span> <span data-ttu-id="cccd8-113">Uma vez o `IDataProtector` instância estiver disponível, chame o `IDataProtector.ToTimeLimitedDataProtector` método de extensão para retornar um protetor com recursos internos de expiração.</span><span class="sxs-lookup"><span data-stu-id="cccd8-113">Once the `IDataProtector` instance is available, call the `IDataProtector.ToTimeLimitedDataProtector` extension method to get back a protector with built-in expiration capabilities.</span></span>

<span data-ttu-id="cccd8-114">`ITimeLimitedDataProtector`apresenta os seguintes métodos de extensão e de superfície de API:</span><span class="sxs-lookup"><span data-stu-id="cccd8-114">`ITimeLimitedDataProtector` exposes the following API surface and extension methods:</span></span>

* <span data-ttu-id="cccd8-115">CreateProtector (objetivo de cadeia de caracteres): ITimeLimitedDataProtector - esta API é semelhante à existente `IDataProtectionProvider.CreateProtector` em que ele pode ser usado para criar [finalidade cadeias](purpose-strings.md) de um protetor de tempo limite de raiz.</span><span class="sxs-lookup"><span data-stu-id="cccd8-115">CreateProtector(string purpose) : ITimeLimitedDataProtector - This API is similar to the existing `IDataProtectionProvider.CreateProtector` in that it can be used to create [purpose chains](purpose-strings.md) from a root time-limited protector.</span></span>

* <span data-ttu-id="cccd8-116">Proteger (byte [] texto sem formatação, expiração de DateTimeOffset): byte]</span><span class="sxs-lookup"><span data-stu-id="cccd8-116">Protect(byte[] plaintext, DateTimeOffset expiration) : byte[]</span></span>

* <span data-ttu-id="cccd8-117">Proteger (texto sem formatação do byte [], tempo de vida de TimeSpan): byte]</span><span class="sxs-lookup"><span data-stu-id="cccd8-117">Protect(byte[] plaintext, TimeSpan lifetime) : byte[]</span></span>

* <span data-ttu-id="cccd8-118">Proteger (byte [] texto sem formatação): byte]</span><span class="sxs-lookup"><span data-stu-id="cccd8-118">Protect(byte[] plaintext) : byte[]</span></span>

* <span data-ttu-id="cccd8-119">Proteger (cadeia de caracteres em texto sem formatação, expiração de DateTimeOffset): cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="cccd8-119">Protect(string plaintext, DateTimeOffset expiration) : string</span></span>

* <span data-ttu-id="cccd8-120">Proteger (texto sem formatação de cadeia de caracteres, tempo de vida de TimeSpan): cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="cccd8-120">Protect(string plaintext, TimeSpan lifetime) : string</span></span>

* <span data-ttu-id="cccd8-121">Proteger (cadeia de caracteres em texto sem formatação): cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="cccd8-121">Protect(string plaintext) : string</span></span>

<span data-ttu-id="cccd8-122">Além das principais `Protect` métodos que levam apenas o texto não criptografado, não há novas sobrecargas que permitem especificar a data de validade da carga.</span><span class="sxs-lookup"><span data-stu-id="cccd8-122">In addition to the core `Protect` methods which take only the plaintext, there are new overloads which allow specifying the payload's expiration date.</span></span> <span data-ttu-id="cccd8-123">A data de validade pode ser especificada como uma data absoluta (por meio de um `DateTimeOffset`) ou como uma hora relativa (do sistema atual de tempo, por meio de um `TimeSpan`).</span><span class="sxs-lookup"><span data-stu-id="cccd8-123">The expiration date can be specified as an absolute date (via a `DateTimeOffset`) or as a relative time (from the current system time, via a `TimeSpan`).</span></span> <span data-ttu-id="cccd8-124">Se uma sobrecarga que não tem uma expiração é chamada, a carga será considerada nunca para expirar.</span><span class="sxs-lookup"><span data-stu-id="cccd8-124">If an overload which doesn't take an expiration is called, the payload is assumed never to expire.</span></span>

* <span data-ttu-id="cccd8-125">Desproteger (byte [] protectedData, limite de expiração de DateTimeOffset): byte]</span><span class="sxs-lookup"><span data-stu-id="cccd8-125">Unprotect(byte[] protectedData, out DateTimeOffset expiration) : byte[]</span></span>

* <span data-ttu-id="cccd8-126">Desproteger (byte [] protectedData): byte]</span><span class="sxs-lookup"><span data-stu-id="cccd8-126">Unprotect(byte[] protectedData) : byte[]</span></span>

* <span data-ttu-id="cccd8-127">Desproteger (protectedData de cadeia de caracteres, limite de expiração de DateTimeOffset): cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="cccd8-127">Unprotect(string protectedData, out DateTimeOffset expiration) : string</span></span>

* <span data-ttu-id="cccd8-128">Desproteger (cadeia de caracteres protectedData): cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="cccd8-128">Unprotect(string protectedData) : string</span></span>

<span data-ttu-id="cccd8-129">O `Unprotect` métodos retornam os dados desprotegidos originais.</span><span class="sxs-lookup"><span data-stu-id="cccd8-129">The `Unprotect` methods return the original unprotected data.</span></span> <span data-ttu-id="cccd8-130">Se a carga ainda não tiver expirado, a expiração absoluta é retornada como um parâmetro junto com os dados desprotegidos originais out opcional.</span><span class="sxs-lookup"><span data-stu-id="cccd8-130">If the payload hasn't yet expired, the absolute expiration is returned as an optional out parameter along with the original unprotected data.</span></span> <span data-ttu-id="cccd8-131">Se a carga tiver expirada, todas as sobrecargas do método Desproteger lançará CryptographicException.</span><span class="sxs-lookup"><span data-stu-id="cccd8-131">If the payload is expired, all overloads of the Unprotect method will throw CryptographicException.</span></span>

>[!WARNING]
> <span data-ttu-id="cccd8-132">Não é recomendável usar essas APIs para proteger cargas que requerem a persistência de longo prazo ou indefinida.</span><span class="sxs-lookup"><span data-stu-id="cccd8-132">It is not advised to use these APIs to protect payloads which require long-term or indefinite persistence.</span></span> <span data-ttu-id="cccd8-133">"Pode suportar para as cargas protegidas sejam irrecuperáveis permanentemente após um mês?"</span><span class="sxs-lookup"><span data-stu-id="cccd8-133">"Can I afford for the protected payloads to be permanently unrecoverable after a month?"</span></span> <span data-ttu-id="cccd8-134">pode servir como uma boa regra prática; Se a resposta for nenhum desenvolvedores, em seguida, considere APIs alternativas.</span><span class="sxs-lookup"><span data-stu-id="cccd8-134">can serve as a good rule of thumb; if the answer is no then developers should consider alternative APIs.</span></span>

<span data-ttu-id="cccd8-135">O exemplo abaixo usa o [caminhos de código não DI](../configuration/non-di-scenarios.md) para instanciar o sistema de proteção de dados.</span><span class="sxs-lookup"><span data-stu-id="cccd8-135">The sample below uses the [non-DI code paths](../configuration/non-di-scenarios.md) for instantiating the data protection system.</span></span> <span data-ttu-id="cccd8-136">Para executar este exemplo, certifique-se de que você adicionou uma referência ao pacote Microsoft.AspNetCore.DataProtection.Extensions primeiro.</span><span class="sxs-lookup"><span data-stu-id="cccd8-136">To run this sample, ensure that you have first added a reference to the Microsoft.AspNetCore.DataProtection.Extensions package.</span></span>

[!code-csharp[Main](limited-lifetime-payloads/samples/limitedlifetimepayloads.cs)]
