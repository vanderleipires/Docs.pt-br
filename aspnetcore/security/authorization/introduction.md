---
title: Introdução à autorização no núcleo do ASP.NET
author: rick-anderson
description: Conheça os fundamentos da autorização e como funciona a autorização em aplicativos do ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/introduction
ms.openlocfilehash: 7ba1966dcaf1ce0510f489cfe0ff11501faffa56
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/22/2018
---
# <a name="introduction-to-authorization-in-aspnet-core"></a>Introdução à autorização no núcleo do ASP.NET

<a name="security-authorization-introduction"></a>

Autorização é o processo que determina o que um usuário pode fazer. Por exemplo, um usuário administrativo tem permissão para criar uma biblioteca de documentos e adicionar, editar e excluir documentos. Um usuário não administrativo trabalhando com esta biblioteca só está autorizado a ler os documentos.

A autorização é ortogonal e independente de autenticação, que é o processo de verificação de quem é o usuário. A autenticação pode criar uma ou mais identidades para o usuário atual.

## <a name="authorization-types"></a>Tipos de autorização

A autorização do ASP.NET Core fornece um modelo simples de [funções](xref:security/authorization/roles) declarativas [baseado em políticas](xref:security/authorization/policies). Ela é expressa em requisitos e os manipuladores avaliam as reivindicações de um usuário em relação aos requisitos. Verificações imperativas podem ser baseadas em políticas simples ou políticas que avaliem a identidade do usuário e propriedades do recurso que o usuário está tentando acessar.

## <a name="namespaces"></a>Namespaces

Componentes de autorização, incluindo os atributo `AuthorizeAttribute` e `AllowAnonymousAttribute` , são encontrados no namespace `Microsoft.AspNetCore.Authorization`.

Consulte a documentação em [autorização simples](xref:security/authorization/simple).
