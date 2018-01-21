---
title: "Introdução à autorização"
author: rick-anderson
description: "Este documento fornece uma explicação básica de autorização e explica como autorização se relaciona com ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/introduction
ms.openlocfilehash: 6f4f1fb4f2776db10a1640049885e31e9a54011a
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="introduction"></a>Introdução

<a name="security-authorization-introduction"></a>

Autorização é o processo que determina o que um usuário é capaz de fazer. Por exemplo, um usuário administrativo tem permissão para criar uma biblioteca de documentos, documentos de adicionar, editar documentos e excluí-los. Um usuário não administrativo trabalhando com a biblioteca só está autorizado a ler os documentos.

A autorização é ortogonal e independente de autenticação, que é o processo de verificação de quem é um usuário. A autenticação pode criar um ou mais identidades para o usuário atual.

## <a name="authorization-types"></a>Tipos de autorização

Autorização de ASP.NET Core fornece um simples declarativo [função](roles.md) e um [política avançada com base em](policies.md) modelo. Autorização é expresso em requisitos e manipuladores de avaliar as declarações de um usuário em relação aos requisitos. Verificações imperativas podem ser baseadas em políticas simples ou políticas que avalie a identidade do usuário e propriedades do recurso que o usuário está tentando acessar.

## <a name="namespaces"></a>Namespaces

Componentes de autorização, incluindo o `AuthorizeAttribute` e `AllowAnonymousAttribute` atributos encontrados no `Microsoft.AspNetCore.Authorization` namespace.
