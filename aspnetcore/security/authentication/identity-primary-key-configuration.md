---
title: "Configurar o tipo de dados de chaves primárias de identidade"
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: e6661708d003aa50204e7f79d3070442a3440391
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
# <a name="configure-identity-primary-keys-data-type"></a>Configurar o tipo de dados de chaves primárias de identidade

Identidade do ASP.NET Core permite que você configure facilmente o tipo de dados que você deseja para as chaves primárias. Por padrão, o tipo de dados de cadeia de caracteres de usos de identidade, mas muito rapidamente, você pode substituir esse comportamento.

## <a name="how-to"></a>Como

1.  A primeira etapa é implementar o modelo de identidade e substituir o tipo de cadeia de caracteres com o tipo de dados que você deseja.

    [!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4-6&range=7-13)]

    [!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3-5&range=7-12)]
    
2.  Implementar o contexto do banco de dados de identidade com seus modelos e o tipo de dados que você deseja para chaves primárias

    [!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]
    
3.  Use os modelos e o tipo de dados que você deseja para chaves primárias quando você declara que o serviço de identidade na classe de inicialização do aplicativo

    [!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-79)]
