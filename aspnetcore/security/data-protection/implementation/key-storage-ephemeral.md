---
title: "Provedores de proteção de dados efêmero"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: af6ea1d0-0d9d-41df-a870-5dda24978e2f
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-storage-ephemeral
ms.openlocfilehash: ee8dccac3ba990b110758042192779426b01fc53
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/13/2017
---
# <a name="ephemeral-data-protection-providers"></a>Provedores de proteção de dados efêmero

<a name="data-protection-implementation-key-storage-ephemeral"></a>

Há cenários em que um aplicativo precisa de um folheto de propaganda IDataProtectionProvider. Por exemplo, o desenvolvedor pode apenas suas experiências em um aplicativo de console único, ou o aplicativo em si é transitório (é inserido no script ou uma unidade de projeto de teste). Para dar suporte a esses cenários o pacote Microsoft.AspNetCore.DataProtection inclui um tipo EphemeralDataProtectionProvider. Esse tipo fornece uma implementação básica de IDataProtectionProvider cujo repositório de chave é mantido somente na memória e não é gravado em qualquer armazenamento de backup.

Cada instância de EphemeralDataProtectionProvider usa sua própria chave mestra exclusiva. Portanto, se um IDataProtector com raiz em um EphemeralDataProtectionProvider gera uma carga protegida, essa carga pode somente ser desprotegida por um IDataProtector equivalente (considerando os mesmos [finalidade](../consumer-apis/purpose-strings.md#data-protection-consumer-apis-purposes) cadeia) vinculados ao mesmo tempo Instância de EphemeralDataProtectionProvider.

O exemplo a seguir demonstra Criando um EphemeralDataProtectionProvider e usá-lo para proteger e Desproteger dados.

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
