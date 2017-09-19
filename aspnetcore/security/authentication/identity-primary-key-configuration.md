---
title: "Configurar o tipo de dados de chaves primárias de identidade"
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: 2afcdf89b2a39d82a4ba72dc780be71ac98ab664
ms.sourcegitcommit: 76d42f09f3e0dd2f2105493eca6b29994aa47706
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/19/2017
---
# <a name="configure-identity-primary-keys-data-type"></a>Configurar o tipo de dados de chaves primárias de identidade

Identidade do ASP.NET Core permite que você configure facilmente o tipo de dados que você deseja para as chaves primárias. Por padrão, o tipo de dados de cadeia de caracteres de usos de identidade, mas muito rapidamente, você pode substituir esse comportamento.

## <a name="how-to"></a>Como

1.  A primeira etapa é implementar o modelo de identidade e substituir o tipo de cadeia de caracteres com o tipo de dados que você deseja.

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4-6&range=7-13)]

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3-5&range=7-12)]
    
2.  Implementar o contexto do banco de dados de identidade com seus modelos e o tipo de dados que você deseja para chaves primárias

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]
    
3.  Use os modelos e o tipo de dados que você deseja para chaves primárias quando você declara que o serviço de identidade na classe de inicialização do aplicativo

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-79)]
