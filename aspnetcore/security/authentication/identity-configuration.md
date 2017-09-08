---
title: Configurar identidade
uid: security/authentication/identity-configuration
ms.openlocfilehash: 7ccd89360a8c7f5c8c6dfac76df42898e18a116a
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
# <a name="configure-identity"></a><span data-ttu-id="9bc4b-102">Configurar identidade</span><span class="sxs-lookup"><span data-stu-id="9bc4b-102">Configure Identity</span></span>

<span data-ttu-id="9bc4b-103">A identidade do ASP.NET Core possui alguns comportamentos padrão que você pode substituir facilmente na classe de inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9bc4b-103">ASP.NET Core Identity has some default behaviors that you can override easily in your application's startup class.</span></span>

## <a name="passwords-policy"></a><span data-ttu-id="9bc4b-104">Política de senhas</span><span class="sxs-lookup"><span data-stu-id="9bc4b-104">Passwords policy</span></span>

<span data-ttu-id="9bc4b-105">Por padrão, a identidade requer que as senhas conter um caractere maiusculo, caractere minúsculo e dígitos.</span><span class="sxs-lookup"><span data-stu-id="9bc4b-105">By default, Identity requires that passwords contain an uppercase character, lowercase character, and digits.</span></span> <span data-ttu-id="9bc4b-106">Também há algumas outras restrições.</span><span class="sxs-lookup"><span data-stu-id="9bc4b-106">There are also some other restrictions.</span></span> <span data-ttu-id="9bc4b-107">Se você deseja simplificar a restrições de senha, você pode fazer isso na classe de inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9bc4b-107">If you want to simplify password restrictions, you can do that in the startup class of your application.</span></span>

<span data-ttu-id="9bc4b-108">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=60-65)]</span><span class="sxs-lookup"><span data-stu-id="9bc4b-108">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=60-65)]</span></span>

## <a name="applications-cookie-settings"></a><span data-ttu-id="9bc4b-109">Configurações de cookie do aplicativo</span><span class="sxs-lookup"><span data-stu-id="9bc4b-109">Application's cookie settings</span></span>

<span data-ttu-id="9bc4b-110">Como a política de senhas, todas as configurações de cookie do aplicativo podem ser alteradas na classe de inicialização.</span><span class="sxs-lookup"><span data-stu-id="9bc4b-110">Like the passwords policy, all the settings of the application's cookie can be changed in the startup class.</span></span>

<span data-ttu-id="9bc4b-111">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=72-80)]</span><span class="sxs-lookup"><span data-stu-id="9bc4b-111">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=72-80)]</span></span>

## <a name="users-lockout"></a><span data-ttu-id="9bc4b-112">Bloqueio do usuário</span><span class="sxs-lookup"><span data-stu-id="9bc4b-112">User's lockout</span></span>

<span data-ttu-id="9bc4b-113">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=67-70)]</span><span class="sxs-lookup"><span data-stu-id="9bc4b-113">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=67-70)]</span></span>
