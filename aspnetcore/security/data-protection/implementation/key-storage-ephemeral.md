---
title: Provedores de proteção de dados efêmero no núcleo do ASP.NET
author: rick-anderson
description: Saiba os detalhes de implementação do ASP.NET Core efêmera proteção de provedores de dados.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-storage-ephemeral
ms.openlocfilehash: e4b0014ab3bdbf90b91383e8a33102f94faa8153
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279461"
---
# <a name="ephemeral-data-protection-providers-in-aspnet-core"></a>Provedores de proteção de dados efêmero no núcleo do ASP.NET

<a name="data-protection-implementation-key-storage-ephemeral"></a>

Há cenários em que um aplicativo precisa de um folheto de propaganda `IDataProtectionProvider`. Por exemplo, o desenvolvedor pode apenas suas experiências em um aplicativo de console único, ou o aplicativo em si é transitório (é inserido no script ou uma unidade de projeto de teste). Para dar suporte a esses cenários de [Microsoft.AspNetCore.DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/) pacote inclui um tipo `EphemeralDataProtectionProvider`. Esse tipo fornece uma implementação básica de `IDataProtectionProvider` cujo repositório de chave é mantido somente na memória e não é gravado em qualquer armazenamento de backup.

Cada instância de `EphemeralDataProtectionProvider` usa sua própria chave mestra exclusiva. Portanto, se um `IDataProtector` com raiz em um `EphemeralDataProtectionProvider` gera uma carga protegida, essa carga só pode ser desprotegida por um equivalente `IDataProtector` (forem os mesmos [finalidade](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-consumer-apis-purposes) cadeia) vinculados ao mesmo `EphemeralDataProtectionProvider` instância.

O exemplo a seguir demonstra a instanciação de um `EphemeralDataProtectionProvider` e usá-lo para proteger e Desproteger dados.

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
