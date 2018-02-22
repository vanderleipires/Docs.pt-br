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
# <a name="introduction"></a><span data-ttu-id="2299d-103">Introdução</span><span class="sxs-lookup"><span data-stu-id="2299d-103">Introduction</span></span>

<a name="security-authorization-introduction"></a>

<span data-ttu-id="2299d-104">Autorização é o processo que determina o que um usuário é capaz de fazer.</span><span class="sxs-lookup"><span data-stu-id="2299d-104">Authorization refers to the process that determines what a user is able to do.</span></span> <span data-ttu-id="2299d-105">Por exemplo, um usuário administrativo tem permissão para criar uma biblioteca de documentos, documentos de adicionar, editar documentos e excluí-los.</span><span class="sxs-lookup"><span data-stu-id="2299d-105">For example, an administrative user is allowed to create a document library, add documents, edit documents, and delete them.</span></span> <span data-ttu-id="2299d-106">Um usuário não administrativo trabalhando com a biblioteca só está autorizado a ler os documentos.</span><span class="sxs-lookup"><span data-stu-id="2299d-106">A non-administrative user working with the library is only authorized to read the documents.</span></span>

<span data-ttu-id="2299d-107">A autorização é ortogonal e independente de autenticação, que é o processo de verificação de quem é um usuário.</span><span class="sxs-lookup"><span data-stu-id="2299d-107">Authorization is orthogonal and independent from authentication, which is the process of ascertaining who a user is.</span></span> <span data-ttu-id="2299d-108">A autenticação pode criar um ou mais identidades para o usuário atual.</span><span class="sxs-lookup"><span data-stu-id="2299d-108">Authentication may create one or more identities for the current user.</span></span>

## <a name="authorization-types"></a><span data-ttu-id="2299d-109">Tipos de autorização</span><span class="sxs-lookup"><span data-stu-id="2299d-109">Authorization types</span></span>

<span data-ttu-id="2299d-110">Autorização de ASP.NET Core fornece um simples, declarativo [função](roles.md) e uma rica [baseado em políticas](policies.md) modelo.</span><span class="sxs-lookup"><span data-stu-id="2299d-110">ASP.NET Core authorization provides a simple, declarative [role](roles.md) and a rich [policy-based](policies.md) model.</span></span> <span data-ttu-id="2299d-111">Autorização é expresso em requisitos e manipuladores de avaliar as declarações de um usuário em relação aos requisitos.</span><span class="sxs-lookup"><span data-stu-id="2299d-111">Authorization is expressed in requirements, and handlers evaluate a user's claims against requirements.</span></span> <span data-ttu-id="2299d-112">Verificações imperativas podem ser baseadas em políticas simples ou políticas que avalie a identidade do usuário e propriedades do recurso que o usuário está tentando acessar.</span><span class="sxs-lookup"><span data-stu-id="2299d-112">Imperative checks can be based on simple policies or policies which evaluate both the user identity and properties of the resource that the user is attempting to access.</span></span>

## <a name="namespaces"></a><span data-ttu-id="2299d-113">Namespaces</span><span class="sxs-lookup"><span data-stu-id="2299d-113">Namespaces</span></span>

<span data-ttu-id="2299d-114">Componentes de autorização, incluindo o `AuthorizeAttribute` e `AllowAnonymousAttribute` atributos, são encontrados no `Microsoft.AspNetCore.Authorization` namespace.</span><span class="sxs-lookup"><span data-stu-id="2299d-114">Authorization components, including the `AuthorizeAttribute` and `AllowAnonymousAttribute` attributes, are found in the `Microsoft.AspNetCore.Authorization` namespace.</span></span>

<span data-ttu-id="2299d-115">Consulte a documentação em [simples de autorização](xref:security/authorization/simple).</span><span class="sxs-lookup"><span data-stu-id="2299d-115">Consult the documentation on [simple authorization](xref:security/authorization/simple).</span></span>
