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
# <a name="configure-identity"></a><span data-ttu-id="cd4a0-104">Configurar identidade</span><span class="sxs-lookup"><span data-stu-id="cd4a0-104">Configure Identity</span></span>

<span data-ttu-id="cd4a0-105">A identidade do ASP.NET Core possui alguns comportamentos padrão que você pode substituir facilmente na classe de inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="cd4a0-105">ASP.NET Core Identity has some default behaviors that you can override easily in your application's startup class.</span></span>

## <a name="passwords-policy"></a><span data-ttu-id="cd4a0-106">Política de senhas</span><span class="sxs-lookup"><span data-stu-id="cd4a0-106">Passwords policy</span></span>

<span data-ttu-id="cd4a0-107">Por padrão, a identidade requer que as senhas conter um caractere maiusculo, caractere minúsculo e dígitos.</span><span class="sxs-lookup"><span data-stu-id="cd4a0-107">By default, Identity requires that passwords contain an uppercase character, lowercase character, and digits.</span></span> <span data-ttu-id="cd4a0-108">Também há algumas outras restrições.</span><span class="sxs-lookup"><span data-stu-id="cd4a0-108">There are also some other restrictions.</span></span> <span data-ttu-id="cd4a0-109">Se você deseja simplificar a restrições de senha, você pode fazer isso na classe de inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="cd4a0-109">If you want to simplify password restrictions, you can do that in the startup class of your application.</span></span>

[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=60-65)]

## <a name="applications-cookie-settings"></a><span data-ttu-id="cd4a0-110">Configurações de cookie do aplicativo</span><span class="sxs-lookup"><span data-stu-id="cd4a0-110">Application's cookie settings</span></span>

<span data-ttu-id="cd4a0-111">Como a política de senhas, todas as configurações de cookie do aplicativo podem ser alteradas na classe de inicialização.</span><span class="sxs-lookup"><span data-stu-id="cd4a0-111">Like the passwords policy, all the settings of the application's cookie can be changed in the startup class.</span></span>

[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=72-80)]

## <a name="users-lockout"></a><span data-ttu-id="cd4a0-112">Bloqueio do usuário</span><span class="sxs-lookup"><span data-stu-id="cd4a0-112">User's lockout</span></span>

[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=67-70)]
