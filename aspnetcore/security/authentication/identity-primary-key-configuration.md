---
title: "Configurar o tipo de dados de chaves primárias de identidade"
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: 2afcdf89b2a39d82a4ba72dc780be71ac98ab664
ms.sourcegitcommit: 76d42f09f3e0dd2f2105493eca6b29994aa47706
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/19/2017
---
# <a name="configure-identity-primary-keys-data-type"></a><span data-ttu-id="aa5cd-102">Configurar o tipo de dados de chaves primárias de identidade</span><span class="sxs-lookup"><span data-stu-id="aa5cd-102">Configure Identity primary keys data type</span></span>

<span data-ttu-id="aa5cd-103">Identidade do ASP.NET Core permite que você configure facilmente o tipo de dados que você deseja para as chaves primárias.</span><span class="sxs-lookup"><span data-stu-id="aa5cd-103">ASP.NET Core Identity allows you to easily configure the data type you want for the primary keys.</span></span> <span data-ttu-id="aa5cd-104">Por padrão, o tipo de dados de cadeia de caracteres de usos de identidade, mas muito rapidamente, você pode substituir esse comportamento.</span><span class="sxs-lookup"><span data-stu-id="aa5cd-104">By default, Identity uses string data type but you can very quickly override this behavior.</span></span>

## <a name="how-to"></a><span data-ttu-id="aa5cd-105">Como</span><span class="sxs-lookup"><span data-stu-id="aa5cd-105">How to</span></span>

1.  <span data-ttu-id="aa5cd-106">A primeira etapa é implementar o modelo de identidade e substituir o tipo de cadeia de caracteres com o tipo de dados que você deseja.</span><span class="sxs-lookup"><span data-stu-id="aa5cd-106">The first step is to implement the Identity's model, and override the string type with the data type you want.</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4-6&range=7-13)]

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3-5&range=7-12)]
    
2.  <span data-ttu-id="aa5cd-107">Implementar o contexto do banco de dados de identidade com seus modelos e o tipo de dados que você deseja para chaves primárias</span><span class="sxs-lookup"><span data-stu-id="aa5cd-107">Implement the database context of Identity with your models and the data type you want for primary keys</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]
    
3.  <span data-ttu-id="aa5cd-108">Use os modelos e o tipo de dados que você deseja para chaves primárias quando você declara que o serviço de identidade na classe de inicialização do aplicativo</span><span class="sxs-lookup"><span data-stu-id="aa5cd-108">Use your models and the data type you want for primary keys when you declare the identity service in your application's startup class</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-79)]
