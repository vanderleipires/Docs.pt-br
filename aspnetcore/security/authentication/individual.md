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
# <a name="articles-based-on-projects-created-with-individual-user-accounts"></a>Artigos com base em projetos criados com as contas de usuário individuais

Identidade do ASP.NET Core está incluída nos modelos de projeto no Visual Studio com a opção de "Contas de usuário individuais".

Os modelos de autenticação estão disponíveis no .NET Core CLI com `-au Individual`:

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

Os artigos a seguir mostram como usar o código gerado em modelos do ASP.NET Core que usam contas de usuário individuais:

* [Autenticação de dois fatores com SMS](xref:security/authentication/2fa)
* [Confirmação de conta e de recuperação de senha no ASP.NET Core](xref:security/authentication/accconfirm)
* [Criar um aplicativo do ASP.NET Core com dados de usuário protegidos por autorização](xref:security/authorization/secure-data)
