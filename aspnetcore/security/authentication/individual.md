---
title: "Artigos com base em projetos criados com as contas de usuário individuais"
author: rick-anderson
description: "Este documento contém artigos com base em projetos criados com as contas de usuário individuais."
keywords: "ASP.NET Core, autorização, IAuthorizationService"
ms.author: riande
manager: wpickett
ms.date: 11/30/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/individual
ms.openlocfilehash: 1864625e0ad6b4ec6fc2ada3fa7d93edec91b633
ms.sourcegitcommit: 037d3900f739dbaa2ba14158e3d7dc81478952ad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/01/2017
---
# <a name="articles-based-on-projects-created-with-individual-user-accounts"></a><span data-ttu-id="647c7-104">Artigos com base em projetos criados com as contas de usuário individuais</span><span class="sxs-lookup"><span data-stu-id="647c7-104">Articles based on projects created with individual user accounts</span></span>

<span data-ttu-id="647c7-105">Identidade do ASP.NET Core está incluída nos modelos de projeto no Visual Studio com a opção de "Contas de usuário individuais".</span><span class="sxs-lookup"><span data-stu-id="647c7-105">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="647c7-106">Os modelos de autenticação estão disponíveis no .NET Core CLI com `-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="647c7-106">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

<span data-ttu-id="647c7-107">Os artigos a seguir mostram como usar o código gerado em modelos do ASP.NET Core que usam contas de usuário individuais:</span><span class="sxs-lookup"><span data-stu-id="647c7-107">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="647c7-108">Autenticação de dois fatores com SMS</span><span class="sxs-lookup"><span data-stu-id="647c7-108">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="647c7-109">Confirmação de conta e de recuperação de senha no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="647c7-109">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="647c7-110">Criar um aplicativo do ASP.NET Core com dados de usuário protegidos por autorização</span><span class="sxs-lookup"><span data-stu-id="647c7-110">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)