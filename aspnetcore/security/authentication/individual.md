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