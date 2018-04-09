---
title: Limite o tempo de vida das cargas protegidos no núcleo do ASP.NET
author: rick-anderson
description: Saiba como limitar o tempo de vida de uma carga protegido usando as APIs de proteção de dados do ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/consumer-apis/limited-lifetime-payloads
ms.openlocfilehash: 324887b3d29de989ad855c4e78fd5a235fdb560e
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/22/2018
---
# <a name="limit-the-lifetime-of-protected-payloads-in-aspnet-core"></a><span data-ttu-id="9e112-103">Limite o tempo de vida das cargas protegidos no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="9e112-103">Limit the lifetime of protected payloads in ASP.NET Core</span></span>

<span data-ttu-id="9e112-104">Há cenários onde quer que o desenvolvedor do aplicativo para criar uma carga protegida que expira após um período de tempo definido.</span><span class="sxs-lookup"><span data-stu-id="9e112-104">There are scenarios where the application developer wants to create a protected payload that expires after a set period of time.</span></span> <span data-ttu-id="9e112-105">Por exemplo, a carga protegida pode representar um token de redefinição de senha que deve ser válido somente por uma hora.</span><span class="sxs-lookup"><span data-stu-id="9e112-105">For instance, the protected payload might represent a password reset token that should only be valid for one hour.</span></span> <span data-ttu-id="9e112-106">É certamente possível para o desenvolvedor criar seu próprio formato de carga que contém uma data de expiração incorporados e os desenvolvedores avançados talvez queira fazer isso mesmo assim, mas para a maioria dos desenvolvedores gerenciar esses expirações pode crescer entediante.</span><span class="sxs-lookup"><span data-stu-id="9e112-106">It's certainly possible for the developer to create their own payload format that contains an embedded expiration date, and advanced developers may wish to do this anyway, but for the majority of developers managing these expirations can grow tedious.</span></span>

<span data-ttu-id="9e112-107">Para facilitar isso para nossos público de desenvolvedor, o pacote [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) contém APIs do utilitário para criar cargas expirarem automaticamente após um período de tempo definido.</span><span class="sxs-lookup"><span data-stu-id="9e112-107">To make this easier for our developer audience, the package [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) contains utility APIs for creating payloads that automatically expire after a set period of time.</span></span> <span data-ttu-id="9e112-108">Essas APIs de suspensão do `ITimeLimitedDataProtector` tipo.</span><span class="sxs-lookup"><span data-stu-id="9e112-108">These APIs hang off of the `ITimeLimitedDataProtector` type.</span></span>

## <a name="api-usage"></a><span data-ttu-id="9e112-109">Uso de API</span><span class="sxs-lookup"><span data-stu-id="9e112-109">API usage</span></span>

<span data-ttu-id="9e112-110">O `ITimeLimitedDataProtector` interface é a interface principal para proteger e ao desproteger cargas de tempo limitado / expiração automática.</span><span class="sxs-lookup"><span data-stu-id="9e112-110">The `ITimeLimitedDataProtector` interface is the core interface for protecting and unprotecting time-limited / self-expiring payloads.</span></span> <span data-ttu-id="9e112-111">Para criar uma instância de um `ITimeLimitedDataProtector`, você precisará de uma instância de uma expressão [IDataProtector](xref:security/data-protection/consumer-apis/overview) construído com uma finalidade específica.</span><span class="sxs-lookup"><span data-stu-id="9e112-111">To create an instance of an `ITimeLimitedDataProtector`, you'll first need an instance of a regular [IDataProtector](xref:security/data-protection/consumer-apis/overview) constructed with a specific purpose.</span></span> <span data-ttu-id="9e112-112">Uma vez o `IDataProtector` instância estiver disponível, chame o `IDataProtector.ToTimeLimitedDataProtector` método de extensão para retornar um protetor com recursos internos de expiração.</span><span class="sxs-lookup"><span data-stu-id="9e112-112">Once the `IDataProtector` instance is available, call the `IDataProtector.ToTimeLimitedDataProtector` extension method to get back a protector with built-in expiration capabilities.</span></span>

<span data-ttu-id="9e112-113">`ITimeLimitedDataProtector` apresenta os seguintes métodos de extensão e de superfície de API:</span><span class="sxs-lookup"><span data-stu-id="9e112-113">`ITimeLimitedDataProtector` exposes the following API surface and extension methods:</span></span>

* <span data-ttu-id="9e112-114">CreateProtector (objetivo de cadeia de caracteres): ITimeLimitedDataProtector - esta API é semelhante à existente `IDataProtectionProvider.CreateProtector` em que ele pode ser usado para criar [finalidade cadeias](xref:security/data-protection/consumer-apis/purpose-strings) de um protetor de tempo limite de raiz.</span><span class="sxs-lookup"><span data-stu-id="9e112-114">CreateProtector(string purpose) : ITimeLimitedDataProtector - This API is similar to the existing `IDataProtectionProvider.CreateProtector` in that it can be used to create [purpose chains](xref:security/data-protection/consumer-apis/purpose-strings) from a root time-limited protector.</span></span>

* <span data-ttu-id="9e112-115">Proteger (byte [] texto sem formatação, expiração de DateTimeOffset): byte]</span><span class="sxs-lookup"><span data-stu-id="9e112-115">Protect(byte[] plaintext, DateTimeOffset expiration) : byte[]</span></span>

* <span data-ttu-id="9e112-116">Proteger (texto sem formatação do byte [], tempo de vida de TimeSpan): byte]</span><span class="sxs-lookup"><span data-stu-id="9e112-116">Protect(byte[] plaintext, TimeSpan lifetime) : byte[]</span></span>

* <span data-ttu-id="9e112-117">Proteger (byte [] texto sem formatação): byte]</span><span class="sxs-lookup"><span data-stu-id="9e112-117">Protect(byte[] plaintext) : byte[]</span></span>

* <span data-ttu-id="9e112-118">Proteger (cadeia de caracteres em texto sem formatação, expiração de DateTimeOffset): cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="9e112-118">Protect(string plaintext, DateTimeOffset expiration) : string</span></span>

* <span data-ttu-id="9e112-119">Proteger (texto sem formatação de cadeia de caracteres, tempo de vida de TimeSpan): cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="9e112-119">Protect(string plaintext, TimeSpan lifetime) : string</span></span>

* <span data-ttu-id="9e112-120">Proteger (cadeia de caracteres em texto sem formatação): cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="9e112-120">Protect(string plaintext) : string</span></span>

<span data-ttu-id="9e112-121">Além das principais `Protect` métodos que levam apenas o texto não criptografado, não há novas sobrecargas que permitem especificar a data de validade da carga.</span><span class="sxs-lookup"><span data-stu-id="9e112-121">In addition to the core `Protect` methods which take only the plaintext, there are new overloads which allow specifying the payload's expiration date.</span></span> <span data-ttu-id="9e112-122">A data de validade pode ser especificada como uma data absoluta (por meio de um `DateTimeOffset`) ou como uma hora relativa (do sistema atual de tempo, por meio de um `TimeSpan`).</span><span class="sxs-lookup"><span data-stu-id="9e112-122">The expiration date can be specified as an absolute date (via a `DateTimeOffset`) or as a relative time (from the current system time, via a `TimeSpan`).</span></span> <span data-ttu-id="9e112-123">Se uma sobrecarga que não tem uma expiração é chamada, a carga será considerada nunca para expirar.</span><span class="sxs-lookup"><span data-stu-id="9e112-123">If an overload which doesn't take an expiration is called, the payload is assumed never to expire.</span></span>

* <span data-ttu-id="9e112-124">Desproteger (byte [] protectedData, limite de expiração de DateTimeOffset): byte]</span><span class="sxs-lookup"><span data-stu-id="9e112-124">Unprotect(byte[] protectedData, out DateTimeOffset expiration) : byte[]</span></span>

* <span data-ttu-id="9e112-125">Desproteger (byte [] protectedData): byte]</span><span class="sxs-lookup"><span data-stu-id="9e112-125">Unprotect(byte[] protectedData) : byte[]</span></span>

* <span data-ttu-id="9e112-126">Desproteger (protectedData de cadeia de caracteres, limite de expiração de DateTimeOffset): cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="9e112-126">Unprotect(string protectedData, out DateTimeOffset expiration) : string</span></span>

* <span data-ttu-id="9e112-127">Desproteger (cadeia de caracteres protectedData): cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="9e112-127">Unprotect(string protectedData) : string</span></span>

<span data-ttu-id="9e112-128">O `Unprotect` métodos retornam os dados desprotegidos originais.</span><span class="sxs-lookup"><span data-stu-id="9e112-128">The `Unprotect` methods return the original unprotected data.</span></span> <span data-ttu-id="9e112-129">Se a carga ainda não tiver expirado, a expiração absoluta é retornada como um parâmetro junto com os dados desprotegidos originais out opcional.</span><span class="sxs-lookup"><span data-stu-id="9e112-129">If the payload hasn't yet expired, the absolute expiration is returned as an optional out parameter along with the original unprotected data.</span></span> <span data-ttu-id="9e112-130">Se a carga tiver expirada, todas as sobrecargas do método Desproteger lançará CryptographicException.</span><span class="sxs-lookup"><span data-stu-id="9e112-130">If the payload is expired, all overloads of the Unprotect method will throw CryptographicException.</span></span>

>[!WARNING]
> <span data-ttu-id="9e112-131">Ele não tem recomendável usar essas APIs para proteger cargas que requerem a persistência de longo prazo ou indefinida.</span><span class="sxs-lookup"><span data-stu-id="9e112-131">It's not advised to use these APIs to protect payloads which require long-term or indefinite persistence.</span></span> <span data-ttu-id="9e112-132">"Pode suportar para as cargas protegidas sejam irrecuperáveis permanentemente após um mês?"</span><span class="sxs-lookup"><span data-stu-id="9e112-132">"Can I afford for the protected payloads to be permanently unrecoverable after a month?"</span></span> <span data-ttu-id="9e112-133">pode servir como uma boa regra prática; Se a resposta for nenhum desenvolvedores, em seguida, considere APIs alternativas.</span><span class="sxs-lookup"><span data-stu-id="9e112-133">can serve as a good rule of thumb; if the answer is no then developers should consider alternative APIs.</span></span>

<span data-ttu-id="9e112-134">O exemplo abaixo usa o [caminhos de código não DI](xref:security/data-protection/configuration/non-di-scenarios) para instanciar o sistema de proteção de dados.</span><span class="sxs-lookup"><span data-stu-id="9e112-134">The sample below uses the [non-DI code paths](xref:security/data-protection/configuration/non-di-scenarios) for instantiating the data protection system.</span></span> <span data-ttu-id="9e112-135">Para executar este exemplo, certifique-se de que você adicionou uma referência ao pacote Microsoft.AspNetCore.DataProtection.Extensions primeiro.</span><span class="sxs-lookup"><span data-stu-id="9e112-135">To run this sample, ensure that you have first added a reference to the Microsoft.AspNetCore.DataProtection.Extensions package.</span></span>

[!code-csharp[](limited-lifetime-payloads/samples/limitedlifetimepayloads.cs)]
