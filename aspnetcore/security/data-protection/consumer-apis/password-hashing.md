---
title: Hash de senhas no ASP.NET Core
author: rick-anderson
description: Saiba como o hash de senhas usando as APIs de proteção de dados do ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: 882ac9b256b0cdf5fd19dc4bd2757cac7e8ecad3
ms.sourcegitcommit: a742b55e4b8276a48b8b4394784554fecd883c84
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45538369"
---
# <a name="hash-passwords-in-aspnet-core"></a>Hash de senhas no ASP.NET Core

A base de código do proteção de dados inclui um pacote *Microsoft.AspNetCore.Cryptography.KeyDerivation* que contém funções de derivação de chave de criptografia. Esse pacote é um componente autônomo e não tem nenhuma dependência no restante do sistema de proteção de dados. Ele pode ser usado independentemente completamente. A origem existe junto com o código de proteção de dados base como uma conveniência.

O pacote no momento, oferece um método `KeyDerivation.Pbkdf2` que permite o hash de uma senha usando o [PBKDF2 algoritmo](https://tools.ietf.org/html/rfc2898#section-5.2). Essa API é muito semelhante à existente do .NET Framework [Rfc2898DeriveBytes tipo](/dotnet/api/system.security.cryptography.rfc2898derivebytes), mas há três diferenças importantes:

1. O `KeyDerivation.Pbkdf2` método dá suporte ao consumo PRFs vários (atualmente `HMACSHA1`, `HMACSHA256`, e `HMACSHA512`), enquanto o `Rfc2898DeriveBytes` digite dá suporte apenas à `HMACSHA1`.

2. O `KeyDerivation.Pbkdf2` método detecta o sistema operacional atual e tenta escolher a implementação mais otimizada de rotina, fornecendo um desempenho muito melhor em alguns casos. (No Windows 8, ele oferece cerca de 10 vezes a taxa de transferência `Rfc2898DeriveBytes`.)

3. O `KeyDerivation.Pbkdf2` método requer que o chamador especificar todos os parâmetros (salt, PRF e contagem de iteração). O `Rfc2898DeriveBytes` tipo fornece valores padrão para eles.

[!code-csharp[](password-hashing/samples/passwordhasher.cs)]

Consulte [código de origem] (https://github.com/aspnet/Identity/blob/master/src/Core/PasswordHasher.cs) para o ASP.NET Core Identity `PasswordHasher` caso de uso de tipo para um mundo real.
