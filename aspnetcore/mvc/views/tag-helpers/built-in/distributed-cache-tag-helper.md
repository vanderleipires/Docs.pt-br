---
title: Auxiliar de Marca de Cache Distribuído no ASP.NET Core
author: pkellner
description: Mostra como trabalhar com o Auxiliar de Marca de Cache
ms.author: riande
ms.date: 02/14/2017
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: a35a5795c086273e773c613c483fc6343c694bf2
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342166"
---
# <a name="distributed-cache-tag-helper-in-aspnet-core"></a><span data-ttu-id="0108a-103">Auxiliar de Marca de Cache Distribuído no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0108a-103">Distributed Cache Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="0108a-104">Por [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="0108a-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="0108a-105">O Auxiliar de Marca de Cache Distribuído fornece a capacidade de melhorar consideravelmente o desempenho do aplicativo ASP.NET Core armazenando seu conteúdo em cache em uma fonte de cache distribuído.</span><span class="sxs-lookup"><span data-stu-id="0108a-105">The Distributed Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to a distributed cache source.</span></span>

<span data-ttu-id="0108a-106">O Auxiliar de Marca de Cache Distribuído herda da mesma classe base do Auxiliar de Marca de Cache.</span><span class="sxs-lookup"><span data-stu-id="0108a-106">The Distributed Cache Tag Helper inherits from the same base class as the Cache Tag Helper.</span></span> <span data-ttu-id="0108a-107">Todos os atributos associados ao Auxiliar de Marca de Cache também funcionarão no Auxiliar de Marca Distribuído.</span><span class="sxs-lookup"><span data-stu-id="0108a-107">All attributes associated with the Cache Tag Helper will also work on the Distributed Tag Helper.</span></span>

<span data-ttu-id="0108a-108">O Auxiliar de Marca de Cache Distribuído segue o **Princípio de Dependências Explícitas**, conhecido como **Injeção de Construtor**.</span><span class="sxs-lookup"><span data-stu-id="0108a-108">The Distributed Cache Tag Helper follows the **Explicit Dependencies Principle** known as **Constructor Injection**.</span></span> <span data-ttu-id="0108a-109">Especificamente, o contêiner de interface `IDistributedCache` é passado para o construtor do Auxiliar de Marca de Cache Distribuído.</span><span class="sxs-lookup"><span data-stu-id="0108a-109">Specifically, the `IDistributedCache` interface container is passed into the Distributed Cache Tag Helper's constructor.</span></span> <span data-ttu-id="0108a-110">Se nenhuma implementação concreta específica de `IDistributedCache` tiver sido criada em `ConfigureServices`, geralmente encontrada em startup.cs, o Auxiliar de Marca de Cache Distribuído usará o mesmo provedor em memória para armazenar os dados em cache que o Auxiliar de Marca de Cache básico.</span><span class="sxs-lookup"><span data-stu-id="0108a-110">If no specific concrete implementation of `IDistributedCache` has been created in `ConfigureServices`, usually found in startup.cs, then the Distributed Cache Tag Helper will use the same in-memory provider for storing cached data as the basic Cache Tag Helper.</span></span>

## <a name="distributed-cache-tag-helper-attributes"></a><span data-ttu-id="0108a-111">Atributos do auxiliar de marca de cache distribuído</span><span class="sxs-lookup"><span data-stu-id="0108a-111">Distributed Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a><span data-ttu-id="0108a-112">prioridade expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by habilitada</span><span class="sxs-lookup"><span data-stu-id="0108a-112">enabled expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by priority</span></span>

<span data-ttu-id="0108a-113">Consulte Auxiliar de Marca de Cache para obter as definições.</span><span class="sxs-lookup"><span data-stu-id="0108a-113">See Cache Tag Helper for definitions.</span></span> <span data-ttu-id="0108a-114">O Auxiliar de Marca de Cache Distribuído herda da mesma classe do Auxiliar de Marca de Cache. Portanto, todos esses atributos são comuns ao Auxiliar de Marca de Cache.</span><span class="sxs-lookup"><span data-stu-id="0108a-114">Distributed Cache Tag Helper inherits from the same class as Cache Tag Helper so all these attributes are common from Cache Tag Helper.</span></span>

- - -

### <a name="name-required"></a><span data-ttu-id="0108a-115">nome (obrigatório)</span><span class="sxs-lookup"><span data-stu-id="0108a-115">name (required)</span></span>

| <span data-ttu-id="0108a-116">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="0108a-116">Attribute Type</span></span>    | <span data-ttu-id="0108a-117">Valor de exemplo</span><span class="sxs-lookup"><span data-stu-id="0108a-117">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="0108a-118">cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="0108a-118">string</span></span>    | <span data-ttu-id="0108a-119">"my-distributed-cache-unique-key-101"</span><span class="sxs-lookup"><span data-stu-id="0108a-119">"my-distributed-cache-unique-key-101"</span></span>     |

<span data-ttu-id="0108a-120">O atributo `name` obrigatório é usado como uma chave para esse cache armazenado de cada instância de um Auxiliar de Marca de Cache Distribuído.</span><span class="sxs-lookup"><span data-stu-id="0108a-120">The required `name` attribute is used as a key to that cache stored for each instance of a Distributed Cache Tag Helper.</span></span> <span data-ttu-id="0108a-121">Ao contrário do Auxiliar de Marca de Cache básico que atribui uma chave a cada instância do Auxiliar de Marca de Cache com base no nome da página do Razor e na localização do Auxiliar de Marca na página do Razor, o Auxiliar de Marca de Cache Distribuído somente localiza suas chaves no atributo `name`</span><span class="sxs-lookup"><span data-stu-id="0108a-121">Unlike the basic Cache Tag Helper that assigns a key to each Cache Tag Helper instance based on the Razor page name and location of the Tag Helper in the razor page, the Distributed Cache Tag Helper only bases its key on the attribute `name`</span></span>

<span data-ttu-id="0108a-122">Exemplo de uso:</span><span class="sxs-lookup"><span data-stu-id="0108a-122">Usage Example:</span></span>

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a><span data-ttu-id="0108a-123">Implementações de IDistributedCache do Auxiliar de Marca de Cache Distribuído</span><span class="sxs-lookup"><span data-stu-id="0108a-123">Distributed Cache Tag Helper IDistributedCache implementations</span></span>

<span data-ttu-id="0108a-124">Há duas implementações de `IDistributedCache` internas do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0108a-124">There are two implementations of `IDistributedCache` built in to ASP.NET Core.</span></span> <span data-ttu-id="0108a-125">Uma é baseada no SQL Server e a outra, no Redis.</span><span class="sxs-lookup"><span data-stu-id="0108a-125">One is based on SQL Server and the other is based on Redis.</span></span> <span data-ttu-id="0108a-126">Os detalhes dessas implementações podem ser encontrados em <xref:performance/caching/distributed>.</span><span class="sxs-lookup"><span data-stu-id="0108a-126">Details of these implementations can be found at <xref:performance/caching/distributed>.</span></span> <span data-ttu-id="0108a-127">Ambas as implementações envolvem a definição de uma instância de `IDistributedCache` no *Startup.cs* do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0108a-127">Both implementations involve setting an instance of `IDistributedCache` in ASP.NET Core's *Startup.cs*.</span></span>

<span data-ttu-id="0108a-128">Não há nenhum atributo de marca especificamente associado ao uso de uma implementação específica de `IDistributedCache`.</span><span class="sxs-lookup"><span data-stu-id="0108a-128">There are no tag attributes specifically associated with using any specific implementation of `IDistributedCache`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0108a-129">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="0108a-129">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
