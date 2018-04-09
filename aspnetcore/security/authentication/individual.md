---
title: Artigos com base em projetos do ASP.NET Core criados com as contas de usuário individuais
author: rick-anderson
description: Descobrir artigos com base em projetos do ASP.NET Core criados com as contas de usuário individuais.
manager: wpickett
ms.author: riande
ms.date: 11/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/individual
ms.openlocfilehash: 40715debb48c0a7121ce84d7843b8517b0973e74
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/22/2018
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a>Artigos com base em projetos do ASP.NET Core criados com as contas de usuário individuais

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
