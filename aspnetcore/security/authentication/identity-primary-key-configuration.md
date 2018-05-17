---
title: Configurar o tipo de dados de chave primária de identidade no núcleo do ASP.NET
author: AdrienTorris
description: Saiba mais sobre as etapas para configurar o tipo de dados desejado, usado para a chave primária da identidade do ASP.NET Core.
manager: wpickett
ms.author: scaddie
ms.date: 09/28/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: 49d5ef94abeb5bd616c5ddbcdd4358a58a8e63a4
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/12/2018
---
# <a name="configure-identity-primary-key-data-type-in-aspnet-core"></a>Configurar o tipo de dados de chave primária de identidade no núcleo do ASP.NET

Identidade do ASP.NET Core permite que você configure o tipo de dados usado para representar uma chave primária. Identidade usa o `string` tipo de dados por padrão. Você pode substituir esse comportamento.

## <a name="customize-the-primary-key-data-type"></a>Personalizar o tipo de dados de chave primária

1. Criar uma implementação personalizada do [IdentityUser](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1) classe. Representa o tipo a ser usado para criar objetos de usuário. No exemplo a seguir, o padrão `string` tipo é substituído pelo `Guid`.

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4&range=7-13)]

2. Criar uma implementação personalizada do [IdentityRole](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1) classe. Representa o tipo a ser usado para criar objetos de função. No exemplo a seguir, o padrão `string` tipo é substituído pelo `Guid`.

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3&range=7-12)]

3. Crie uma classe de contexto de banco de dados personalizado. Ela herda da classe de contexto de banco de dados do Entity Framework usada para identidade. O `TUser` e `TRole` argumentos referenciar as classes de usuário e a função personalizadas criadas na etapa anterior, respectivamente. O `Guid` tipo de dados é definido para a chave primária.

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]

4. Registre a classe de contexto de banco de dados personalizados ao adicionar o serviço de identidade na classe de inicialização do aplicativo.

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

   O `AddEntityFrameworkStores` método não aceita um `TKey` argumento como fazia no ASP.NET Core 1. x. Tipo de dados da chave primária é deduzido analisando a `DbContext` objeto.

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=6-8&range=25-37)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

   O `AddEntityFrameworkStores` método aceita um `TKey` argumento indicando o tipo de dados da chave primária.

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-55)]

   ---

## <a name="test-the-changes"></a>Teste as alterações

Após a conclusão das alterações de configuração, a propriedade que representa a chave primária reflete o novo tipo de dados. O exemplo a seguir demonstra como acessar a propriedade em um controlador MVC.

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Controllers/AccountController.cs?name=snippet_GetCurrentUserId&highlight=6)]
