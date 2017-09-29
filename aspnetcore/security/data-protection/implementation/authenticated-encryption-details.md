---
title: Detalhes de criptografia autenticada.
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 826e6d5d-9620-44e6-ad93-3b1d9969b70b
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/authenticated-encryption-details
ms.openlocfilehash: 4d0e63d7722071ab8806a217e96ee7ad7bf10286
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/28/2017
---
# <a name="authenticated-encryption-details"></a>Detalhes de criptografia autenticada.

<a name=data-protection-implementation-authenticated-encryption-details></a>

Chamadas para IDataProtector.Protect são as operações de criptografia autenticada. O método Protect oferece confidencialidade e a autenticidade e ela é vinculada à cadeia finalidade que foi usada para essa instância específica do IDataProtector derivam da sua raiz IDataProtectionProvider.

IDataProtector.Protect usa um parâmetro de texto sem formatação do byte [] e produz uma byte [] protegido carga, cujo formato é descrito abaixo. (Também há uma sobrecarga de método de extensão que usa um parâmetro de texto sem formatação da cadeia de caracteres e retorna uma carga protegido de cadeia de caracteres. Se essa API é usada ainda terá o formato de carga protegido o abaixo de estrutura, mas será [codificado base64url](https://tools.ietf.org/html/rfc4648#section-5).)

## <a name="protected-payload-format"></a>Formato de conteúdo protegido

O formato de carga protegido consiste em três componentes principais:

* Um cabeçalho mágico de 32 bits que identifica a versão do sistema de proteção de dados.

* Uma id de 128 bits chave que identifica a chave usada para proteger essa carga específica.

* O restante da carga protegido é [específico para o Criptografador encapsulado por esta chave](subkeyderivation.md#data-protection-implementation-subkey-derivation). No exemplo a seguir a chave representa um AES-256-CBC + Criptografador HMACSHA256 e a carga é mais subdividida da seguinte maneira: * modificador chave A 128 bits. * Um vetor de inicialização de 128 bits. * 48 bytes de saída de AES-256-CBC. * Uma marca de autenticação HMACSHA256.

Uma carga protegido exemplo ilustrada abaixo.

```
09 F0 C9 F0 80 9C 81 0C 19 66 19 40 95 36 53 F8
AA FF EE 57 57 2F 40 4C 3F 7F CC 9D CC D9 32 3E
84 17 99 16 EC BA 1F 4A A1 18 45 1F 2D 13 7A 28
79 6B 86 9C F8 B7 84 F9 26 31 FC B1 86 0A F1 56
61 CF 14 58 D3 51 6F CF 36 50 85 82 08 2D 3F 73
5F B0 AD 9E 1A B2 AE 13 57 90 C8 F5 7C 95 4E 6A
8A AA 06 EF 43 CA 19 62 84 7C 11 B2 C8 71 9D AA
52 19 2E 5B 4C 1E 54 F0 55 BE 88 92 12 C1 4B 5E
52 C9 74 A0
```

O formato de carga acima os primeiros 32 bits ou 4 bytes são o cabeçalho magic identifica a versão (09 F0 C9 F0)

O próximos 128 bits ou 16 bytes é o identificador de chave (80 9 81 de C 0C 19 66 19 40 95 36 53 F8 AA FF EE 57)

O restante contém a carga e é específico para o formato usado.

>[!WARNING]
> Todas as cargas protegidas para uma determinada chave começa com o mesmo cabeçalho de 20 bytes (valor mágico, id de chave). Os administradores podem usar esse fato para fins de diagnóstico para aproximar quando uma carga foi gerada. Por exemplo, a carga acima corresponde à chave {0c819c80-6619-4019-9536-53f8aaffee57}. Se depois de verificar se o repositório de chave achar que a data de ativação dessa chave específica era 2015-01-01 e data de expiração foi 2015-03-01, é razoável pressupor que a carga (se não violada) foi gerada dentro dessa janela, dê ou levar um pequeno fator em ambos os lados.
