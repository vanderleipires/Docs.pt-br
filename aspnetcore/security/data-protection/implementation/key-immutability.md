---
title: "A imutabilidade de chave e alterar as configurações"
author: rick-anderson
description: "Este documento descreve os detalhes de implementação de ASP.NET Core dados proteção chave imutabilidade APIs."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 425b8ba9769c2b5ac635693b045e52c110f25205
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
# <a name="key-immutability-and-changing-settings"></a><span data-ttu-id="f5708-103">A imutabilidade de chave e alterar as configurações</span><span class="sxs-lookup"><span data-stu-id="f5708-103">Key Immutability and changing settings</span></span>

<span data-ttu-id="f5708-104">Depois que um objeto é persistido para o armazenamento de backup, sua representação para sempre é fixo.</span><span class="sxs-lookup"><span data-stu-id="f5708-104">Once an object is persisted to the backing store, its representation is forever fixed.</span></span> <span data-ttu-id="f5708-105">Novos dados podem ser adicionados ao repositório de backup, mas os dados existentes nunca podem ser modificados.</span><span class="sxs-lookup"><span data-stu-id="f5708-105">New data can be added to the backing store, but existing data can never be mutated.</span></span> <span data-ttu-id="f5708-106">O objetivo principal deste comportamento é evitar a corrupção de dados.</span><span class="sxs-lookup"><span data-stu-id="f5708-106">The primary purpose of this behavior is to prevent data corruption.</span></span>

<span data-ttu-id="f5708-107">Uma consequência esse comportamento é que, quando uma chave é gravada para armazenamento de backup, é imutável.</span><span class="sxs-lookup"><span data-stu-id="f5708-107">One consequence of this behavior is that once a key is written to the backing store, it's immutable.</span></span> <span data-ttu-id="f5708-108">As datas de criação, ativação e expiração nunca podem ser alteradas, embora ele pode ser revogada usando `IKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="f5708-108">Its creation, activation, and expiration dates can never be changed, though it can revoked by using `IKeyManager`.</span></span> <span data-ttu-id="f5708-109">Além disso, suas informações algorítmicos subjacentes, material de chave mestre e criptografia propriedades rest também são imutáveis.</span><span class="sxs-lookup"><span data-stu-id="f5708-109">Additionally, its underlying algorithmic information, master keying material, and encryption at rest properties are also immutable.</span></span>

<span data-ttu-id="f5708-110">Se o desenvolvedor altera qualquer configuração que afeta a persistência de chave, essas alterações não entram em vigor até a próxima vez que uma chave é gerada, através de uma chamada explícita para `IKeyManager.CreateNewKey` ou por meio do sistema proteção de dados próprio [chave automática geração](key-management.md#data-protection-implementation-key-management) comportamento.</span><span class="sxs-lookup"><span data-stu-id="f5708-110">If the developer changes any setting that affects key persistence, those changes won't go into effect until the next time a key is generated, either via an explicit call to `IKeyManager.CreateNewKey` or via the data protection system's own [automatic key generation](key-management.md#data-protection-implementation-key-management) behavior.</span></span> <span data-ttu-id="f5708-111">As configurações que afetam a persistência de chave são da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="f5708-111">The settings that affect key persistence are as follows:</span></span>

* [<span data-ttu-id="f5708-112">O tempo de vida de chave padrão</span><span class="sxs-lookup"><span data-stu-id="f5708-112">The default key lifetime</span></span>](key-management.md#data-protection-implementation-key-management)

* [<span data-ttu-id="f5708-113">A criptografia de chave no mecanismo de rest</span><span class="sxs-lookup"><span data-stu-id="f5708-113">The key encryption at rest mechanism</span></span>](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)

* [<span data-ttu-id="f5708-114">As informações de algoritmos contidas na chave do</span><span class="sxs-lookup"><span data-stu-id="f5708-114">The algorithmic information contained within the key</span></span>](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

<span data-ttu-id="f5708-115">Se você precisar essas configurações de anteriores a próxima chave automática sem interrupção tempo, considere a possibilidade de fazer uma chamada explícita para `IKeyManager.CreateNewKey` para forçar a criação de uma nova chave.</span><span class="sxs-lookup"><span data-stu-id="f5708-115">If you need these settings to kick in earlier than the next automatic key rolling time, consider making an explicit call to `IKeyManager.CreateNewKey` to force the creation of a new key.</span></span> <span data-ttu-id="f5708-116">Lembre-se de fornecer uma data de ativação explícita ({agora + 2 dias} é uma boa regra prática para permitir um tempo para que a alteração se propague) e a data de expiração na chamada.</span><span class="sxs-lookup"><span data-stu-id="f5708-116">Remember to provide an explicit activation date ({ now + 2 days } is a good rule of thumb to allow time for the change to propagate) and expiration date in the call.</span></span>

>[!TIP]
> <span data-ttu-id="f5708-117">Tocar o repositório de todos os aplicativos devem especificar as mesmas configurações com o `IDataProtectionBuilder` métodos de extensão.</span><span class="sxs-lookup"><span data-stu-id="f5708-117">All applications touching the repository should specify the same settings with the `IDataProtectionBuilder` extension methods.</span></span> <span data-ttu-id="f5708-118">Caso contrário, as propriedades da chave persistente dependerá do aplicativo específico que invocou as rotinas de geração de chave.</span><span class="sxs-lookup"><span data-stu-id="f5708-118">Otherwise, the properties of the persisted key will be dependent on the particular application that invoked the key generation routines.</span></span>
