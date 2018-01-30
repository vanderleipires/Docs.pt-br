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
# <a name="introduction"></a><span data-ttu-id="c8cda-103">Introdução</span><span class="sxs-lookup"><span data-stu-id="c8cda-103">Introduction</span></span>

<a name="security-authorization-introduction"></a>

<span data-ttu-id="c8cda-104">Autorização é o processo que determina o que um usuário é capaz de fazer.</span><span class="sxs-lookup"><span data-stu-id="c8cda-104">Authorization refers to the process that determines what a user is able to do.</span></span> <span data-ttu-id="c8cda-105">Por exemplo, um usuário administrativo tem permissão para criar uma biblioteca de documentos, documentos de adicionar, editar documentos e excluí-los.</span><span class="sxs-lookup"><span data-stu-id="c8cda-105">For example, an administrative user is allowed to create a document library, add documents, edit documents, and delete them.</span></span> <span data-ttu-id="c8cda-106">Um usuário não administrativo trabalhando com a biblioteca só está autorizado a ler os documentos.</span><span class="sxs-lookup"><span data-stu-id="c8cda-106">A non-administrative user working with the library is only authorized to read the documents.</span></span>

<span data-ttu-id="c8cda-107">A autorização é ortogonal e independente de autenticação, que é o processo de verificação de quem é um usuário.</span><span class="sxs-lookup"><span data-stu-id="c8cda-107">Authorization is orthogonal and independent from authentication, which is the process of ascertaining who a user is.</span></span> <span data-ttu-id="c8cda-108">A autenticação pode criar um ou mais identidades para o usuário atual.</span><span class="sxs-lookup"><span data-stu-id="c8cda-108">Authentication may create one or more identities for the current user.</span></span>

## <a name="authorization-types"></a><span data-ttu-id="c8cda-109">Tipos de autorização</span><span class="sxs-lookup"><span data-stu-id="c8cda-109">Authorization Types</span></span>

<span data-ttu-id="c8cda-110">Autorização de ASP.NET Core fornece um simples declarativo [função](roles.md) e um [política avançada com base em](policies.md) modelo.</span><span class="sxs-lookup"><span data-stu-id="c8cda-110">ASP.NET Core authorization provides a simple declarative [role](roles.md) and a [rich policy based](policies.md) model.</span></span> <span data-ttu-id="c8cda-111">Autorização é expresso em requisitos e manipuladores de avaliar as declarações de um usuário em relação aos requisitos.</span><span class="sxs-lookup"><span data-stu-id="c8cda-111">Authorization is expressed in requirements, and handlers evaluate a user's claims against requirements.</span></span> <span data-ttu-id="c8cda-112">Verificações imperativas podem ser baseadas em políticas simples ou políticas que avalie a identidade do usuário e propriedades do recurso que o usuário está tentando acessar.</span><span class="sxs-lookup"><span data-stu-id="c8cda-112">Imperative checks can be based on simple policies or policies which evaluate both the user identity and properties of the resource that the user is attempting to access.</span></span>

## <a name="namespaces"></a><span data-ttu-id="c8cda-113">Namespaces</span><span class="sxs-lookup"><span data-stu-id="c8cda-113">Namespaces</span></span>

<span data-ttu-id="c8cda-114">Componentes de autorização, incluindo o `AuthorizeAttribute` e `AllowAnonymousAttribute` atributos encontrados no `Microsoft.AspNetCore.Authorization` namespace.</span><span class="sxs-lookup"><span data-stu-id="c8cda-114">Authorization components, including the `AuthorizeAttribute` and `AllowAnonymousAttribute` attributes are found in the `Microsoft.AspNetCore.Authorization` namespace.</span></span>
