---
title: "Auxiliar de Marca de Cache Distribuído no ASP.NET Core"
author: pkellner
description: Mostra como trabalhar com o Auxiliar de Marca de Cache
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: 710477732b865e2e3821102d34545bbd4e0a5919
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="distributed-cache-tag-helper"></a><span data-ttu-id="383ad-103">Auxiliar de Marca de Cache Distribuído</span><span class="sxs-lookup"><span data-stu-id="383ad-103">Distributed Cache Tag Helper</span></span>

<span data-ttu-id="383ad-104">Por [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="383ad-104">By [Peter Kellner](http://peterkellner.net)</span></span> 


<span data-ttu-id="383ad-105">O Auxiliar de Marca de Cache Distribuído fornece a capacidade de melhorar consideravelmente o desempenho do aplicativo ASP.NET Core armazenando seu conteúdo em cache em uma fonte de cache distribuído.</span><span class="sxs-lookup"><span data-stu-id="383ad-105">The Distributed Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to a distributed cache source.</span></span>

<span data-ttu-id="383ad-106">O Auxiliar de Marca de Cache Distribuído herda da mesma classe base do Auxiliar de Marca de Cache.</span><span class="sxs-lookup"><span data-stu-id="383ad-106">The Distributed Cache Tag Helper inherits from the same base class as the Cache Tag Helper.</span></span>  <span data-ttu-id="383ad-107">Todos os atributos associados ao Auxiliar de Marca de Cache também funcionarão no Auxiliar de Marca Distribuído.</span><span class="sxs-lookup"><span data-stu-id="383ad-107">All attributes associated with the Cache Tag Helper will also work on the Distributed Tag Helper.</span></span>


<span data-ttu-id="383ad-108">O Auxiliar de Marca de Cache Distribuído segue o **Princípio de Dependências Explícitas**, conhecido como **Injeção de Construtor**.</span><span class="sxs-lookup"><span data-stu-id="383ad-108">The Distributed Cache Tag Helper follows the **Explicit Dependencies Principle** known as **Constructor Injection**.</span></span>  <span data-ttu-id="383ad-109">Especificamente, o contêiner de interface `IDistributedCache` é passado para o construtor do Auxiliar de Marca de Cache Distribuído.</span><span class="sxs-lookup"><span data-stu-id="383ad-109">Specifically, the `IDistributedCache` interface container is passed into the Distributed Cache Tag Helper's constructor.</span></span>  <span data-ttu-id="383ad-110">Se nenhuma implementação concreta específica de `IDistributedCache` tiver sido criada em `ConfigureServices`, geralmente encontrada em startup.cs, o Auxiliar de Marca de Cache Distribuído usará o mesmo provedor em memória para armazenar os dados em cache que o Auxiliar de Marca de Cache básico.</span><span class="sxs-lookup"><span data-stu-id="383ad-110">If no specific concrete implementation of `IDistributedCache` has been created in `ConfigureServices`, usually found in startup.cs, then the Distributed Cache Tag Helper will use the same in-memory provider for storing cached data as the basic Cache Tag Helper.</span></span>

## <a name="distributed-cache-tag-helper-attributes"></a><span data-ttu-id="383ad-111">Atributos do auxiliar de marca de cache distribuído</span><span class="sxs-lookup"><span data-stu-id="383ad-111">Distributed Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a><span data-ttu-id="383ad-112">prioridade expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by habilitada</span><span class="sxs-lookup"><span data-stu-id="383ad-112">enabled expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by priority</span></span>

<span data-ttu-id="383ad-113">Consulte Auxiliar de Marca de Cache para obter as definições.</span><span class="sxs-lookup"><span data-stu-id="383ad-113">See Cache Tag Helper for definitions.</span></span> <span data-ttu-id="383ad-114">O Auxiliar de Marca de Cache Distribuído herda da mesma classe do Auxiliar de Marca de Cache. Portanto, todos esses atributos são comuns ao Auxiliar de Marca de Cache.</span><span class="sxs-lookup"><span data-stu-id="383ad-114">Distributed Cache Tag Helper inherits from the same class as Cache Tag Helper so all these attributes are common from Cache Tag Helper.</span></span>

- - -

### <a name="name-required"></a><span data-ttu-id="383ad-115">nome (obrigatório)</span><span class="sxs-lookup"><span data-stu-id="383ad-115">name (required)</span></span>

| <span data-ttu-id="383ad-116">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="383ad-116">Attribute Type</span></span>    | <span data-ttu-id="383ad-117">Valor de exemplo</span><span class="sxs-lookup"><span data-stu-id="383ad-117">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="383ad-118">cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="383ad-118">string</span></span>    | <span data-ttu-id="383ad-119">"my-distributed-cache-unique-key-101"</span><span class="sxs-lookup"><span data-stu-id="383ad-119">"my-distributed-cache-unique-key-101"</span></span>     |

<span data-ttu-id="383ad-120">O atributo `name` obrigatório é usado como uma chave para esse cache armazenado de cada instância de um Auxiliar de Marca de Cache Distribuído.</span><span class="sxs-lookup"><span data-stu-id="383ad-120">The required `name` attribute is used as a key to that cache stored for each instance of a Distributed Cache Tag Helper.</span></span>  <span data-ttu-id="383ad-121">Ao contrário do Auxiliar de Marca de Cache básico que atribui uma chave a cada instância do Auxiliar de Marca de Cache com base no nome da página do Razor e na localização do auxiliar de marca na página do Razor, o Auxiliar de Marca de Cache Distribuído somente localiza suas chaves no atributo `name`</span><span class="sxs-lookup"><span data-stu-id="383ad-121">Unlike the basic Cache Tag Helper that assigns a key to each Cache Tag Helper instance based on the Razor page name and location of the tag helper in the razor page, the Distributed Cache Tag Helper only bases it's key on the attribute `name`</span></span>

<span data-ttu-id="383ad-122">Exemplo de uso:</span><span class="sxs-lookup"><span data-stu-id="383ad-122">Usage Example:</span></span>

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a><span data-ttu-id="383ad-123">Implementações de IDistributedCache do Auxiliar de Marca de Cache Distribuído</span><span class="sxs-lookup"><span data-stu-id="383ad-123">Distributed Cache Tag Helper IDistributedCache Implementations</span></span>

<span data-ttu-id="383ad-124">Há duas implementações de `IDistributedCache` internas do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="383ad-124">There are two implementations of `IDistributedCache` built in to ASP.NET Core.</span></span>  <span data-ttu-id="383ad-125">Uma é baseada no **SQL Server** e a outra, no **Redis**.</span><span class="sxs-lookup"><span data-stu-id="383ad-125">One is based on **Sql Server** and the other is based on **Redis**.</span></span> <span data-ttu-id="383ad-126">Os detalhes dessas implementações podem ser encontrados no recurso referenciado abaixo intitulado "Trabalhando com um cache distribuído".</span><span class="sxs-lookup"><span data-stu-id="383ad-126">Details of these implementations can be found at the resource referenced below named "Working with a distributed cache".</span></span> <span data-ttu-id="383ad-127">Ambas as implementações envolvem a definição de uma instância de `IDistributedCache` no **startup.cs** do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="383ad-127">Both implementations involve setting an instance of `IDistributedCache` in ASP.NET Core's **startup.cs**.</span></span>

<span data-ttu-id="383ad-128">Não há nenhum atributo de marca especificamente associado ao uso de uma implementação específica de `IDistributedCache`.</span><span class="sxs-lookup"><span data-stu-id="383ad-128">There are no tag attributes specifically associated with using any specific implementation of `IDistributedCache`.</span></span>



- - -



## <a name="additional-resources"></a><span data-ttu-id="383ad-129">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="383ad-129">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
