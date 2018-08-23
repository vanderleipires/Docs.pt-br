---
uid: webhooks/receiving/dependencies
title: Dependências de receptor de WebHooks do ASP.NET | Microsoft Docs
author: rick-anderson
description: Dependências de receptor e injeção de dependência no ASP.NET WebHooks.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.openlocfilehash: c44cfe3ed310aa728a989b108c410e8786e4f514
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832425"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a>Dependências de receptor de WebHooks do ASP.NET

WebHooks do Microsoft ASP.NET foi projetado com injeção de dependência em mente. A maioria das dependências do sistema podem ser substituídas por implementações alternativas usando um mecanismo de injeção de dependência.

Consulte [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) para obter uma lista de dependências de receptor. Se nenhuma dependência tiver sido registrada, uma implementação padrão será usada. Consulte [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) para obter uma lista das implementações padrão.
