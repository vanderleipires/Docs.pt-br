---
title: Configurar identidade
uid: security/authentication/identity-configuration
ms.openlocfilehash: 7ccd89360a8c7f5c8c6dfac76df42898e18a116a
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
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
