---
title: "Introdução à autorização"
author: rick-anderson
description: "Este documento fornece uma explicação básica de autorização e explica como autorização se relaciona com ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/introduction
ms.openlocfilehash: 3fef6d38672af8871c04b65834789a39a7df8487
ms.sourcegitcommit: d43c84c4c80527c85e49d53691b293669557a79d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/20/2018
---
# <a name="introduction"></a>Introdução

<a name="security-authorization-introduction"></a>

Autorização é o processo que determina o que um usuário é capaz de fazer. Por exemplo, um usuário administrativo tem permissão para criar uma biblioteca de documentos, adiciona-los, edita-los e excluí-los. Um usuário não administrativo trabalhando com esta biblioteca só está autorizado a ler os documentos.

A autorização é ortogonal e independente de autenticação, que é o processo de verificação de quem é o usuário. A autenticação pode criar uma ou mais identidades para o usuário atual.

## <a name="authorization-types"></a>Tipos de autorização

A autorização do ASP.NET Core fornece um modelo simples de [funções](roles.md) declarativas [baseado em políticas](policies.md). Ela é expressa em requisitos e os manipuladores avaliam as reivindicações de um usuário em relação aos requisitos. Verificações imperativas podem ser baseadas em políticas simples ou políticas que avaliem a identidade do usuário e propriedades do recurso que o usuário está tentando acessar.

## <a name="namespaces"></a>Namespaces

Componentes de autorização, incluindo os atributos `AuthorizeAttribute` e `AllowAnonymousAttribute`, são encontrados no namespace `Microsoft.AspNetCore.Authorization`.

Consulte a documentação em [autorização simples](xref:security/authorization/simple).
