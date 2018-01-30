---
title: Hash de senha
author: rick-anderson
description: "Este documento explica como hash senhas usando as APIs de proteção de dados ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: 536499687865b4e4d5b1d9c4076623b5ac1ffdbe
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="password-hashing"></a>Hash de senha

O código de proteção de dados base inclui um pacote *Microsoft.AspNetCore.Cryptography.KeyDerivation* que contém funções de derivação de chave de criptografia. Este pacote é um componente autônomo e não tem nenhuma dependência no restante do sistema de proteção de dados. Ele pode ser usado independentemente completamente. A origem existe junto com o código de proteção de dados base como uma conveniência.

O pacote atualmente oferece um método `KeyDerivation.Pbkdf2` que permite o hash de uma senha usando o [PBKDF2 algoritmo](https://tools.ietf.org/html/rfc2898#section-5.2). Essa API é muito semelhante à existente do .NET Framework [Rfc2898DeriveBytes tipo](https://docs.microsoft.com/dotnet/api/system.security.cryptography.rfc2898derivebytes), mas há três diferenças importantes:

1. O `KeyDerivation.Pbkdf2` método dá suporte ao consumo vários PRFs (atualmente `HMACSHA1`, `HMACSHA256`, e `HMACSHA512`), enquanto o `Rfc2898DeriveBytes` digite dá suporte apenas à `HMACSHA1`.

2. O `KeyDerivation.Pbkdf2` método detecta o sistema operacional atual e tenta escolha a implementação mais otimizada da rotina, fornecendo um desempenho muito melhor em determinados casos. (No Windows 8, ele oferece cerca de 10 vezes a taxa de transferência `Rfc2898DeriveBytes`.)

3. O `KeyDerivation.Pbkdf2` método requer que o chamador pode especificar todos os parâmetros (salt, PRF e contagem de iteração). O `Rfc2898DeriveBytes` tipo fornece valores padrão para eles.

[!code-csharp[Main](password-hashing/samples/passwordhasher.cs)]

Consulte o código-fonte para o ASP.NET Core Identity `PasswordHasher` caso de uso de tipo para um mundo real.
