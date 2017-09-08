---
title: "Provedores de proteção de dados efêmero"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: af6ea1d0-0d9d-41df-a870-5dda24978e2f
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-storage-ephemeral
ms.openlocfilehash: 6b564f082eefad993ac938407e0f3b657d83fb52
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
# <a name="ephemeral-data-protection-providers"></a><span data-ttu-id="b77c5-103">Provedores de proteção de dados efêmero</span><span class="sxs-lookup"><span data-stu-id="b77c5-103">Ephemeral data protection providers</span></span>

<a name=data-protection-implementation-key-storage-ephemeral></a>

<span data-ttu-id="b77c5-104">Há cenários em que um aplicativo precisa de um folheto de propaganda IDataProtectionProvider.</span><span class="sxs-lookup"><span data-stu-id="b77c5-104">There are scenarios where an application needs a throwaway IDataProtectionProvider.</span></span> <span data-ttu-id="b77c5-105">Por exemplo, o desenvolvedor pode apenas suas experiências em um aplicativo de console único, ou o aplicativo em si é transitório (é inserido no script ou uma unidade de projeto de teste).</span><span class="sxs-lookup"><span data-stu-id="b77c5-105">For example, the developer might just be experimenting in a one-off console application, or the application itself is transient (it's scripted or a unit test project).</span></span> <span data-ttu-id="b77c5-106">Para dar suporte a esses cenários o pacote Microsoft.AspNetCore.DataProtection inclui um tipo EphemeralDataProtectionProvider.</span><span class="sxs-lookup"><span data-stu-id="b77c5-106">To support these scenarios the package Microsoft.AspNetCore.DataProtection includes a type EphemeralDataProtectionProvider.</span></span> <span data-ttu-id="b77c5-107">Esse tipo fornece uma implementação básica de IDataProtectionProvider cujo repositório de chave é mantido somente na memória e não é gravado em qualquer armazenamento de backup.</span><span class="sxs-lookup"><span data-stu-id="b77c5-107">This type provides a basic implementation of IDataProtectionProvider whose key repository is held solely in-memory and isn't written out to any backing store.</span></span>

<span data-ttu-id="b77c5-108">Cada instância de EphemeralDataProtectionProvider usa sua própria chave mestra exclusiva.</span><span class="sxs-lookup"><span data-stu-id="b77c5-108">Each instance of EphemeralDataProtectionProvider uses its own unique master key.</span></span> <span data-ttu-id="b77c5-109">Portanto, se um IDataProtector com raiz em um EphemeralDataProtectionProvider gera uma carga protegida, essa carga pode somente ser desprotegida por um IDataProtector equivalente (considerando os mesmos [finalidade](../consumer-apis/purpose-strings.md#data-protection-consumer-apis-purposes) cadeia) vinculados ao mesmo tempo Instância de EphemeralDataProtectionProvider.</span><span class="sxs-lookup"><span data-stu-id="b77c5-109">Therefore, if an IDataProtector rooted at an EphemeralDataProtectionProvider generates a protected payload, that payload can only be unprotected by an equivalent IDataProtector (given the same [purpose](../consumer-apis/purpose-strings.md#data-protection-consumer-apis-purposes) chain) rooted at the same EphemeralDataProtectionProvider instance.</span></span>

<span data-ttu-id="b77c5-110">O exemplo a seguir demonstra Criando um EphemeralDataProtectionProvider e usá-lo para proteger e Desproteger dados.</span><span class="sxs-lookup"><span data-stu-id="b77c5-110">The following sample demonstrates instantiating an EphemeralDataProtectionProvider and using it to protect and unprotect data.</span></span>

```none
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
