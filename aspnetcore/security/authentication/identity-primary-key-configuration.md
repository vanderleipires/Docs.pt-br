---
title: "Configurar o tipo de dados de chave primária de identidade"
author: AdrienTorris
description: "Este artigo descreve as etapas para configurar o tipo de dados desejado, usado para a chave primária da identidade do ASP.NET Core."
manager: wpickett
ms.author: scaddie
ms.date: 09/28/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: 66631e46640e294c934aa563518509b96f5cd158
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="configure-the-aspnet-core-identity-primary-key-data-type"></a><span data-ttu-id="d26b9-103">Configurar o tipo de dados de chave primária de identidade do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d26b9-103">Configure the ASP.NET Core Identity primary key data type</span></span>

<span data-ttu-id="d26b9-104">Identidade do ASP.NET Core permite que você configure o tipo de dados usado para representar uma chave primária.</span><span class="sxs-lookup"><span data-stu-id="d26b9-104">ASP.NET Core Identity allows you to configure the data type used to represent a primary key.</span></span> <span data-ttu-id="d26b9-105">Identidade usa o `string` tipo de dados por padrão.</span><span class="sxs-lookup"><span data-stu-id="d26b9-105">Identity uses the `string` data type by default.</span></span> <span data-ttu-id="d26b9-106">Você pode substituir esse comportamento.</span><span class="sxs-lookup"><span data-stu-id="d26b9-106">You can override this behavior.</span></span>

## <a name="customize-the-primary-key-data-type"></a><span data-ttu-id="d26b9-107">Personalizar o tipo de dados de chave primária</span><span class="sxs-lookup"><span data-stu-id="d26b9-107">Customize the primary key data type</span></span>

1. <span data-ttu-id="d26b9-108">Criar uma implementação personalizada do [IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1) classe.</span><span class="sxs-lookup"><span data-stu-id="d26b9-108">Create a custom implementation of the [IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1) class.</span></span> <span data-ttu-id="d26b9-109">Representa o tipo a ser usado para criar objetos de usuário.</span><span class="sxs-lookup"><span data-stu-id="d26b9-109">It represents the type to be used for creating user objects.</span></span> <span data-ttu-id="d26b9-110">No exemplo a seguir, o padrão `string` tipo é substituído pelo `Guid`.</span><span class="sxs-lookup"><span data-stu-id="d26b9-110">In the following example, the default `string` type is replaced with `Guid`.</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4&range=7-13)]

1. <span data-ttu-id="d26b9-111">Criar uma implementação personalizada do [IdentityRole](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1) classe.</span><span class="sxs-lookup"><span data-stu-id="d26b9-111">Create a custom implementation of the [IdentityRole](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1) class.</span></span> <span data-ttu-id="d26b9-112">Representa o tipo a ser usado para criar objetos de função.</span><span class="sxs-lookup"><span data-stu-id="d26b9-112">It represents the type to be used for creating role objects.</span></span> <span data-ttu-id="d26b9-113">No exemplo a seguir, o padrão `string` tipo é substituído pelo `Guid`.</span><span class="sxs-lookup"><span data-stu-id="d26b9-113">In the following example, the default `string` type is replaced with `Guid`.</span></span>
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3&range=7-12)]
    
1. <span data-ttu-id="d26b9-114">Crie uma classe de contexto de banco de dados personalizado.</span><span class="sxs-lookup"><span data-stu-id="d26b9-114">Create a custom database context class.</span></span> <span data-ttu-id="d26b9-115">Ela herda da classe de contexto de banco de dados do Entity Framework usada para identidade.</span><span class="sxs-lookup"><span data-stu-id="d26b9-115">It inherits from the Entity Framework database context class used for Identity.</span></span> <span data-ttu-id="d26b9-116">O `TUser` e `TRole` argumentos referenciar as classes de usuário e a função personalizadas criadas na etapa anterior, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="d26b9-116">The `TUser` and `TRole` arguments reference the custom user and role classes created in the previous step, respectively.</span></span> <span data-ttu-id="d26b9-117">O `Guid` tipo de dados é definido para a chave primária.</span><span class="sxs-lookup"><span data-stu-id="d26b9-117">The `Guid` data type is defined for the primary key.</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]
    
1. <span data-ttu-id="d26b9-118">Registre a classe de contexto de banco de dados personalizados ao adicionar o serviço de identidade na classe de inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d26b9-118">Register the custom database context class when adding the Identity service in the app's startup class.</span></span>

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d26b9-119">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d26b9-119">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)
    
    <span data-ttu-id="d26b9-120">O `AddEntityFrameworkStores` método não aceita um `TKey` argumento como fazia no ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="d26b9-120">The `AddEntityFrameworkStores` method doesn't accept a `TKey` argument as it did in ASP.NET Core 1.x.</span></span> <span data-ttu-id="d26b9-121">Tipo de dados da chave primária é deduzido analisando a `DbContext` objeto.</span><span class="sxs-lookup"><span data-stu-id="d26b9-121">The primary key's data type is inferred by analyzing the `DbContext` object.</span></span>
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=6-8&range=25-37)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d26b9-122">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d26b9-122">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
    
    <span data-ttu-id="d26b9-123">O `AddEntityFrameworkStores` método aceita um `TKey` argumento indicando o tipo de dados da chave primária.</span><span class="sxs-lookup"><span data-stu-id="d26b9-123">The `AddEntityFrameworkStores` method accepts a `TKey` argument indicating the primary key's data type.</span></span>
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-55)]
    
    ---

## <a name="test-the-changes"></a><span data-ttu-id="d26b9-124">Teste as alterações</span><span class="sxs-lookup"><span data-stu-id="d26b9-124">Test the changes</span></span>

<span data-ttu-id="d26b9-125">Após a conclusão das alterações de configuração, a propriedade que representa a chave primária reflete o novo tipo de dados.</span><span class="sxs-lookup"><span data-stu-id="d26b9-125">Upon completion of the configuration changes, the property representing the primary key reflects the new data type.</span></span> <span data-ttu-id="d26b9-126">O exemplo a seguir demonstra como acessar a propriedade em um controlador MVC.</span><span class="sxs-lookup"><span data-stu-id="d26b9-126">The following example demonstrates accessing the property in an MVC controller.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Controllers/AccountController.cs?name=snippet_GetCurrentUserId&highlight=6)]
