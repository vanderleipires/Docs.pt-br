---
title: Auxiliar de Marca de Cache Distribuído no ASP.NET Core
author: pkellner
description: Saiba como usar o Auxiliar de marca de cache distribuído.
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: a5b33451a763c297c6d7885855a321c43435abb4
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325205"
---
# <a name="distributed-cache-tag-helper-in-aspnet-core"></a><span data-ttu-id="dd248-103">Auxiliar de Marca de Cache Distribuído no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dd248-103">Distributed Cache Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="dd248-104">Por [Peter Kellner](http://peterkellner.net) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="dd248-104">By [Peter Kellner](http://peterkellner.net) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="dd248-105">O Auxiliar de Marca de Cache Distribuído fornece a capacidade de melhorar consideravelmente o desempenho do aplicativo ASP.NET Core armazenando seu conteúdo em cache em uma fonte de cache distribuído.</span><span class="sxs-lookup"><span data-stu-id="dd248-105">The Distributed Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to a distributed cache source.</span></span>

<span data-ttu-id="dd248-106">Para obter uma visão geral dos Auxiliares de Marca, confira <xref:mvc/views/tag-helpers/intro>.</span><span class="sxs-lookup"><span data-stu-id="dd248-106">For an overview of Tag Helpers, see <xref:mvc/views/tag-helpers/intro>.</span></span>

<span data-ttu-id="dd248-107">O Auxiliar de Marca de Cache Distribuído herda da mesma classe base do Auxiliar de Marca de Cache.</span><span class="sxs-lookup"><span data-stu-id="dd248-107">The Distributed Cache Tag Helper inherits from the same base class as the Cache Tag Helper.</span></span> <span data-ttu-id="dd248-108">Todos os atributos de [Auxiliar de Marca de Cache](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper) estão disponíveis ao Auxiliar de Marca Distribuído.</span><span class="sxs-lookup"><span data-stu-id="dd248-108">All of the [Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper) attributes are available to the Distributed Tag Helper.</span></span>

<span data-ttu-id="dd248-109">O Auxiliar de Marca de Cache distribuído usa [injeção de construtor](xref:fundamentals/dependency-injection#constructor-injection-behavior).</span><span class="sxs-lookup"><span data-stu-id="dd248-109">The Distributed Cache Tag Helper uses [constructor injection](xref:fundamentals/dependency-injection#constructor-injection-behavior).</span></span> <span data-ttu-id="dd248-110">A interface <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> é passada para o construtor do Auxiliar de Marca de Cache Distribuído.</span><span class="sxs-lookup"><span data-stu-id="dd248-110">The <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface is passed into the Distributed Cache Tag Helper's constructor.</span></span> <span data-ttu-id="dd248-111">Se nenhuma implementação concreta de `IDistributedCache` for criada em `Startup.ConfigureServices` (*Startup.cs*), o Auxiliar de Marca de Cache Distribuído usará o mesmo provedor na memória para armazenar dados em cache como o [Auxiliar de Marca de Cache](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="dd248-111">If no concrete implementation of `IDistributedCache` is created in `Startup.ConfigureServices` (*Startup.cs*), the Distributed Cache Tag Helper uses the same in-memory provider for storing cached data as the [Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span></span>

## <a name="distributed-cache-tag-helper-attributes"></a><span data-ttu-id="dd248-112">Atributos do auxiliar de marca de cache distribuído</span><span class="sxs-lookup"><span data-stu-id="dd248-112">Distributed Cache Tag Helper Attributes</span></span>

### <a name="attributes-shared-with-the-cache-tag-helper"></a><span data-ttu-id="dd248-113">Atributos compartilhados com o Auxiliar de Marca de Cache</span><span class="sxs-lookup"><span data-stu-id="dd248-113">Attributes shared with the Cache Tag Helper</span></span>

* `enabled`
* `expires-on`
* `expires-after`
* `expires-sliding`
* `vary-by-header`
* `vary-by-query`
* `vary-by-route`
* `vary-by-cookie`
* `vary-by-user`
* `vary-by priority`

<span data-ttu-id="dd248-114">O Auxiliar de Marca de Cache Distribuído herda da mesma classe que o Auxiliar de Marca de Cache.</span><span class="sxs-lookup"><span data-stu-id="dd248-114">The Distributed Cache Tag Helper inherits from the same class as Cache Tag Helper.</span></span> <span data-ttu-id="dd248-115">Para obter descrições desses atributos, confira [Auxiliar de Marca de Cache](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="dd248-115">For descriptions of these attributes, see the [Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span></span>

### <a name="name"></a><span data-ttu-id="dd248-116">name</span><span class="sxs-lookup"><span data-stu-id="dd248-116">name</span></span>

| <span data-ttu-id="dd248-117">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="dd248-117">Attribute Type</span></span> | <span data-ttu-id="dd248-118">Exemplo</span><span class="sxs-lookup"><span data-stu-id="dd248-118">Example</span></span>                               |
| -------------- | ------------------------------------- |
| <span data-ttu-id="dd248-119">Cadeia de Caracteres</span><span class="sxs-lookup"><span data-stu-id="dd248-119">String</span></span>         | `my-distributed-cache-unique-key-101` |

<span data-ttu-id="dd248-120">`name` é obrigatório.</span><span class="sxs-lookup"><span data-stu-id="dd248-120">`name` is required.</span></span> <span data-ttu-id="dd248-121">O atributo `name` é usado como uma chave para cada instância de cache armazenada.</span><span class="sxs-lookup"><span data-stu-id="dd248-121">The `name` attribute is used as a key for each stored cache instance.</span></span> <span data-ttu-id="dd248-122">Ao contrário do Auxiliar de Marca de Cache que atribui uma chave de cache a cada instância com base no nome da página do Razor e na localização na página do Razor, o Auxiliar de Marca de Cache Distribuído somente localiza suas chaves no atributo `name`.</span><span class="sxs-lookup"><span data-stu-id="dd248-122">Unlike the Cache Tag Helper that assigns a cache key to each instance based on the Razor page name and location in the Razor page, the Distributed Cache Tag Helper only bases its key on the attribute `name`.</span></span>

<span data-ttu-id="dd248-123">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="dd248-123">Example:</span></span>

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a><span data-ttu-id="dd248-124">Implementações de IDistributedCache do Auxiliar de Marca de Cache Distribuído</span><span class="sxs-lookup"><span data-stu-id="dd248-124">Distributed Cache Tag Helper IDistributedCache implementations</span></span>

<span data-ttu-id="dd248-125">Há duas implementações de <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> internas do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="dd248-125">There are two implementations of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> built in to ASP.NET Core.</span></span> <span data-ttu-id="dd248-126">Uma é baseada no SQL Server e a outra, no Redis.</span><span class="sxs-lookup"><span data-stu-id="dd248-126">One is based on SQL Server, and the other is based on Redis.</span></span> <span data-ttu-id="dd248-127">Os detalhes dessas implementações podem ser encontrados em <xref:performance/caching/distributed>.</span><span class="sxs-lookup"><span data-stu-id="dd248-127">Details of these implementations can be found at <xref:performance/caching/distributed>.</span></span> <span data-ttu-id="dd248-128">Ambas as implementações envolvem configurar de uma instância do `IDistributedCache` em `Startup`.</span><span class="sxs-lookup"><span data-stu-id="dd248-128">Both implementations involve setting an instance of `IDistributedCache` in `Startup`.</span></span>

<span data-ttu-id="dd248-129">Não há nenhum atributo de marca especificamente associado ao uso de uma implementação específica de `IDistributedCache`.</span><span class="sxs-lookup"><span data-stu-id="dd248-129">There are no tag attributes specifically associated with using any specific implementation of `IDistributedCache`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dd248-130">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="dd248-130">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
