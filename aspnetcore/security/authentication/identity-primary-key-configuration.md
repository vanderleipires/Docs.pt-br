---
title: "Configurar o tipo de dados de chaves primárias de identidade"
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: e6661708d003aa50204e7f79d3070442a3440391
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
# <a name="configure-identity-primary-keys-data-type"></a><span data-ttu-id="56845-102">Configurar o tipo de dados de chaves primárias de identidade</span><span class="sxs-lookup"><span data-stu-id="56845-102">Configure Identity primary keys data type</span></span>

<span data-ttu-id="56845-103">Identidade do ASP.NET Core permite que você configure facilmente o tipo de dados que você deseja para as chaves primárias.</span><span class="sxs-lookup"><span data-stu-id="56845-103">ASP.NET Core Identity allows you to easily configure the data type you want for the primary keys.</span></span> <span data-ttu-id="56845-104">Por padrão, o tipo de dados de cadeia de caracteres de usos de identidade, mas muito rapidamente, você pode substituir esse comportamento.</span><span class="sxs-lookup"><span data-stu-id="56845-104">By default, Identity uses string data type but you can very quickly override this behavior.</span></span>

## <a name="how-to"></a><span data-ttu-id="56845-105">Como</span><span class="sxs-lookup"><span data-stu-id="56845-105">How to</span></span>

1.  <span data-ttu-id="56845-106">A primeira etapa é implementar o modelo de identidade e substituir o tipo de cadeia de caracteres com o tipo de dados que você deseja.</span><span class="sxs-lookup"><span data-stu-id="56845-106">The first step is to implement the Identity's model, and override the string type with the data type you want.</span></span>

    <span data-ttu-id="56845-107">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4-6&range=7-13)]</span><span class="sxs-lookup"><span data-stu-id="56845-107">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4-6&range=7-13)]</span></span>

    <span data-ttu-id="56845-108">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3-5&range=7-12)]</span><span class="sxs-lookup"><span data-stu-id="56845-108">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3-5&range=7-12)]</span></span>
    
2.  <span data-ttu-id="56845-109">Implementar o contexto do banco de dados de identidade com seus modelos e o tipo de dados que você deseja para chaves primárias</span><span class="sxs-lookup"><span data-stu-id="56845-109">Implement the database context of Identity with your models and the data type you want for primary keys</span></span>

    <span data-ttu-id="56845-110">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]</span><span class="sxs-lookup"><span data-stu-id="56845-110">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]</span></span>
    
3.  <span data-ttu-id="56845-111">Use os modelos e o tipo de dados que você deseja para chaves primárias quando você declara que o serviço de identidade na classe de inicialização do aplicativo</span><span class="sxs-lookup"><span data-stu-id="56845-111">Use your models and the data type you want for primary keys when you declare the identity service in your application's startup class</span></span>

    <span data-ttu-id="56845-112">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-79)]</span><span class="sxs-lookup"><span data-stu-id="56845-112">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-79)]</span></span>
