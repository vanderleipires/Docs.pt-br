---
title: "Artigos com base em projetos criados com as contas de usuário individuais"
author: rick-anderson
description: "Este documento contém artigos com base em projetos criados com as contas de usuário individuais."
ms.author: riande
manager: wpickett
ms.date: 11/30/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/individual
ms.openlocfilehash: 844514f2970b761ec65589765eb21421cd1962a1
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="articles-based-on-projects-created-with-individual-user-accounts"></a><span data-ttu-id="f8b84-103">Artigos com base em projetos criados com as contas de usuário individuais</span><span class="sxs-lookup"><span data-stu-id="f8b84-103">Articles based on projects created with individual user accounts</span></span>

<span data-ttu-id="f8b84-104">Identidade do ASP.NET Core está incluída nos modelos de projeto no Visual Studio com a opção de "Contas de usuário individuais".</span><span class="sxs-lookup"><span data-stu-id="f8b84-104">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="f8b84-105">Os modelos de autenticação estão disponíveis no .NET Core CLI com `-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="f8b84-105">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

<span data-ttu-id="f8b84-106">Os artigos a seguir mostram como usar o código gerado em modelos do ASP.NET Core que usam contas de usuário individuais:</span><span class="sxs-lookup"><span data-stu-id="f8b84-106">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="f8b84-107">Autenticação de dois fatores com SMS</span><span class="sxs-lookup"><span data-stu-id="f8b84-107">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="f8b84-108">Confirmação de conta e de recuperação de senha no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f8b84-108">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="f8b84-109">Criar um aplicativo do ASP.NET Core com dados de usuário protegidos por autorização</span><span class="sxs-lookup"><span data-stu-id="f8b84-109">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
