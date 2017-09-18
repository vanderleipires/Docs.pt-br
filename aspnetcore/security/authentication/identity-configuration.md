---
title: Configurar a identidade do ASP.NET Core
author: AdrienTorris
description: "Entender os valores padrão de identidade do ASP.NET Core e configure as várias propriedades de identidade para usar valores personalizados."
keywords: "Autenticação do ASP.NET Core, identidade, segurança"
ms.author: scaddie
manager: wpickett
ms.date: 09/18/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-configuration
ms.openlocfilehash: 629fcc2941b2d2fda9604a3eac04be3d0f5294b2
ms.sourcegitcommit: ddefc78270bd9b5ae0b1bd8de6c45f6977e7dceb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/18/2017
---
# <a name="configure-identity"></a>Configurar identidade

A identidade do ASP.NET Core possui alguns comportamentos padrão que você pode substituir facilmente na classe de inicialização do aplicativo.

## <a name="passwords-policy"></a>Política de senhas

Por padrão, a identidade requer que as senhas conter um caractere maiusculo, caractere minúsculo e dígitos. Também há algumas outras restrições. Se você deseja simplificar a restrições de senha, você pode fazer isso na classe de inicialização do aplicativo.

[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=60-65)]

## <a name="applications-cookie-settings"></a>Configurações de cookie do aplicativo

Como a política de senhas, todas as configurações de cookie do aplicativo podem ser alteradas na classe de inicialização.

[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=72-80)]

## <a name="users-lockout"></a>Bloqueio do usuário

[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=67-70)]
