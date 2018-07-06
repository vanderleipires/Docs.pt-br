---
uid: webhooks/receiving/dependencies
title: Dependências de receptor de WebHooks do ASP.NET | Microsoft Docs
author: rick-anderson
description: Dependências de receptor e injeção de dependência no ASP.NET WebHooks.
ms.author: aspnetcontent
ms.date: 01/17/2012
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.openlocfilehash: 05dcfac121e7974fd83c5b3736616479574944a1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817831"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a>Dependências de receptor de WebHooks do ASP.NET

WebHooks do Microsoft ASP.NET foi projetado com injeção de dependência em mente. A maioria das dependências do sistema podem ser substituídas por implementações alternativas usando um mecanismo de injeção de dependência.

Consulte [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) para obter uma lista de dependências de receptor. Se nenhuma dependência tiver sido registrada, uma implementação padrão será usada. Consulte [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) para obter uma lista das implementações padrão.
