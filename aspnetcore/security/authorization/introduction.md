---
title: "Introdução"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: a6a556ed-ba59-4107-9358-44cf20e5931b
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/introduction
ms.openlocfilehash: 040525505a982fc1be1901effb9186a8fe1cdbdf
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
# <a name="introduction"></a>Introdução

<a name=security-authorization-introduction></a>

Autorização é o processo que determina o que um usuário é capaz de fazer. Por exemplo, um usuário administrativo tem permissão para criar uma biblioteca de documentos, documentos de adicionar, editar documentos e excluí-los. Um usuário não administrativo trabalhando com a biblioteca só está autorizado a ler os documentos.

A autorização é ortogonal e independente de autenticação, que é o processo de verificação de quem é um usuário. A autenticação pode criar um ou mais identidades para o usuário atual.

## <a name="authorization-types"></a>Tipos de autorização

Autorização de ASP.NET Core fornece um simples declarativo [função](roles.md#security-authorization-role-based) e um [política avançada com base em](policies.md#security-authorization-policies-based) modelo. Autorização é expresso em requisitos e manipuladores de avaliar as declarações de um usuário em relação aos requisitos. Verificações imperativas podem ser baseadas em políticas simples ou políticas que avalie a identidade do usuário e propriedades do recurso que o usuário está tentando acessar.

## <a name="namespaces"></a>Namespaces

Componentes de autorização, incluindo o `AuthorizeAttribute` e `AllowAnonymousAttribute` atributos encontrados no `Microsoft.AspNetCore.Authorization` namespace.
