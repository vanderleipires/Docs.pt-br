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
ms.openlocfilehash: a6bd81a4e5796c1d0a0033c2b8d5a6ba9282564c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="introduction"></a>Introdução

<a name="security-authorization-introduction"></a>

Autorização é o processo que determina o que um usuário é capaz de fazer. Por exemplo, um usuário administrativo tem permissão para criar uma biblioteca de documentos, documentos de adicionar, editar documentos e excluí-los. Um usuário não administrativo trabalhando com a biblioteca só está autorizado a ler os documentos.

A autorização é ortogonal e independente de autenticação, que é o processo de verificação de quem é um usuário. A autenticação pode criar um ou mais identidades para o usuário atual.

## <a name="authorization-types"></a>Tipos de autorização

Autorização de ASP.NET Core fornece um simples declarativo [função](roles.md) e um [política avançada com base em](policies.md) modelo. Autorização é expresso em requisitos e manipuladores de avaliar as declarações de um usuário em relação aos requisitos. Verificações imperativas podem ser baseadas em políticas simples ou políticas que avalie a identidade do usuário e propriedades do recurso que o usuário está tentando acessar.

## <a name="namespaces"></a>Namespaces

Componentes de autorização, incluindo o `AuthorizeAttribute` e `AllowAnonymousAttribute` atributos encontrados no `Microsoft.AspNetCore.Authorization` namespace.
