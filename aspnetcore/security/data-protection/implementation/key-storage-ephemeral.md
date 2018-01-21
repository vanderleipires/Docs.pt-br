---
title: "Provedores de proteção de dados efêmero"
author: rick-anderson
description: "Este documento explica os detalhes da implementação do ASP.NET Core efêmera proteção de provedores de dados."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-storage-ephemeral
ms.openlocfilehash: 9c1d03373c9d7fb6dffb3583c58aa593fd3875f4
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="ephemeral-data-protection-providers"></a><span data-ttu-id="7a162-103">Provedores de proteção de dados efêmero</span><span class="sxs-lookup"><span data-stu-id="7a162-103">Ephemeral data protection providers</span></span>

<a name="data-protection-implementation-key-storage-ephemeral"></a>

<span data-ttu-id="7a162-104">Há cenários em que um aplicativo precisa de um folheto de propaganda `IDataProtectionProvider`.</span><span class="sxs-lookup"><span data-stu-id="7a162-104">There are scenarios where an application needs a throwaway `IDataProtectionProvider`.</span></span> <span data-ttu-id="7a162-105">Por exemplo, o desenvolvedor pode apenas suas experiências em um aplicativo de console único, ou o aplicativo em si é transitório (é inserido no script ou uma unidade de projeto de teste).</span><span class="sxs-lookup"><span data-stu-id="7a162-105">For example, the developer might just be experimenting in a one-off console application, or the application itself is transient (it's scripted or a unit test project).</span></span> <span data-ttu-id="7a162-106">Para dar suporte a esses cenários de [Microsoft.AspNetCore.DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/) pacote inclui um tipo `EphemeralDataProtectionProvider`.</span><span class="sxs-lookup"><span data-stu-id="7a162-106">To support these scenarios the [Microsoft.AspNetCore.DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/) package includes a type `EphemeralDataProtectionProvider`.</span></span> <span data-ttu-id="7a162-107">Esse tipo fornece uma implementação básica de `IDataProtectionProvider` cujo repositório de chave é mantido somente na memória e não é gravado em qualquer armazenamento de backup.</span><span class="sxs-lookup"><span data-stu-id="7a162-107">This type provides a basic implementation of `IDataProtectionProvider` whose key repository is held solely in-memory and isn't written out to any backing store.</span></span>

<span data-ttu-id="7a162-108">Cada instância de `EphemeralDataProtectionProvider` usa sua própria chave mestra exclusiva.</span><span class="sxs-lookup"><span data-stu-id="7a162-108">Each instance of `EphemeralDataProtectionProvider` uses its own unique master key.</span></span> <span data-ttu-id="7a162-109">Portanto, se um `IDataProtector` com raiz em um `EphemeralDataProtectionProvider` gera uma carga protegida, essa carga só pode ser desprotegida por um equivalente `IDataProtector` (forem os mesmos [finalidade](../consumer-apis/purpose-strings.md#data-protection-consumer-apis-purposes) cadeia) vinculados ao mesmo `EphemeralDataProtectionProvider` instância.</span><span class="sxs-lookup"><span data-stu-id="7a162-109">Therefore, if an `IDataProtector` rooted at an `EphemeralDataProtectionProvider` generates a protected payload, that payload can only be unprotected by an equivalent `IDataProtector` (given the same [purpose](../consumer-apis/purpose-strings.md#data-protection-consumer-apis-purposes) chain) rooted at the same `EphemeralDataProtectionProvider` instance.</span></span>

<span data-ttu-id="7a162-110">O exemplo a seguir demonstra a instanciação de um `EphemeralDataProtectionProvider` e usá-lo para proteger e Desproteger dados.</span><span class="sxs-lookup"><span data-stu-id="7a162-110">The following sample demonstrates instantiating an `EphemeralDataProtectionProvider` and using it to protect and unprotect data.</span></span>

```csharp
using System;
using Microsoft.AspNetCore.DataProtection;

public class Program
{
    public static void Main(string[] args)
    {
        const string purpose = "Ephemeral.App.v1";

        // create an ephemeral provider and demonstrate that it can round-trip a payload
        var provider = new EphemeralDataProtectionProvider();
        var protector = provider.CreateProtector(purpose);
        Console.Write("Enter input: ");
        string input = Console.ReadLine();

        // protect the payload
        string protectedPayload = protector.Protect(input);
        Console.WriteLine($"Protect returned: {protectedPayload}");

        // unprotect the payload
        string unprotectedPayload = protector.Unprotect(protectedPayload);
        Console.WriteLine($"Unprotect returned: {unprotectedPayload}");

        // if I create a new ephemeral provider, it won't be able to unprotect existing
        // payloads, even if I specify the same purpose
        provider = new EphemeralDataProtectionProvider();
        protector = provider.CreateProtector(purpose);
        unprotectedPayload = protector.Unprotect(protectedPayload); // THROWS
    }
}

/*
* SAMPLE OUTPUT
*
* Enter input: Hello!
* Protect returned: CfDJ8AAAAAAAAAAAAAAAAAAAAA...uGoxWLjGKtm1SkNACQ
* Unprotect returned: Hello!
* << throws CryptographicException >>
*/
```
