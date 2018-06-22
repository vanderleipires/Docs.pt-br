---
title: Artigos com base em projetos do ASP.NET Core criados com as contas de usuário individuais
author: rick-anderson
description: Descobrir artigos com base em projetos do ASP.NET Core criados com as contas de usuário individuais.
ms.author: riande
ms.date: 11/30/2017
uid: security/authentication/individual
ms.openlocfilehash: f7fb9e8cd1b5c4cc3283ddd7606a0bbd30f554d5
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274421"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a><span data-ttu-id="68228-103">Artigos com base em projetos do ASP.NET Core criados com as contas de usuário individuais</span><span class="sxs-lookup"><span data-stu-id="68228-103">Articles based on ASP.NET Core projects created with individual user accounts</span></span>

<span data-ttu-id="68228-104">Identidade do ASP.NET Core está incluída nos modelos de projeto no Visual Studio com a opção de "Contas de usuário individuais".</span><span class="sxs-lookup"><span data-stu-id="68228-104">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="68228-105">Os modelos de autenticação estão disponíveis no .NET Core CLI com `-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="68228-105">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new webapp -au Individual
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

::: moniker-end

<span data-ttu-id="68228-107">Os artigos a seguir mostram como usar o código gerado em modelos do ASP.NET Core que usam contas de usuário individuais:</span><span class="sxs-lookup"><span data-stu-id="68228-107">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="68228-108">Autenticação de dois fatores com SMS</span><span class="sxs-lookup"><span data-stu-id="68228-108">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="68228-109">Confirmação de conta e de recuperação de senha no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="68228-109">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="68228-110">Criar um aplicativo do ASP.NET Core com dados de usuário protegidos por autorização</span><span class="sxs-lookup"><span data-stu-id="68228-110">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
