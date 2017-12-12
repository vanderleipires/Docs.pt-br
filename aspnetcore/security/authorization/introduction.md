---
title: "Introdução à autorização"
author: rick-anderson
description: "Este documento fornece uma explicação básica de autorização e explica como autorização se relaciona com ASP.NET Core."
keywords: "ASP.NET Core, autorização"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: a6a556ed-ba59-4107-9358-44cf20e5931b
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/introduction
ms.openlocfilehash: 192cc494c2378f77207a4b1c17b0b0a73ca642ed
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="introduction"></a><span data-ttu-id="d1121-104">Introdução</span><span class="sxs-lookup"><span data-stu-id="d1121-104">Introduction</span></span>

<a name="security-authorization-introduction"></a>

<span data-ttu-id="d1121-105">Autorização é o processo que determina o que um usuário é capaz de fazer.</span><span class="sxs-lookup"><span data-stu-id="d1121-105">Authorization refers to the process that determines what a user is able to do.</span></span> <span data-ttu-id="d1121-106">Por exemplo, um usuário administrativo tem permissão para criar uma biblioteca de documentos, documentos de adicionar, editar documentos e excluí-los.</span><span class="sxs-lookup"><span data-stu-id="d1121-106">For example, an administrative user is allowed to create a document library, add documents, edit documents, and delete them.</span></span> <span data-ttu-id="d1121-107">Um usuário não administrativo trabalhando com a biblioteca só está autorizado a ler os documentos.</span><span class="sxs-lookup"><span data-stu-id="d1121-107">A non-administrative user working with the library is only authorized to read the documents.</span></span>

<span data-ttu-id="d1121-108">A autorização é ortogonal e independente de autenticação, que é o processo de verificação de quem é um usuário.</span><span class="sxs-lookup"><span data-stu-id="d1121-108">Authorization is orthogonal and independent from authentication, which is the process of ascertaining who a user is.</span></span> <span data-ttu-id="d1121-109">A autenticação pode criar um ou mais identidades para o usuário atual.</span><span class="sxs-lookup"><span data-stu-id="d1121-109">Authentication may create one or more identities for the current user.</span></span>

## <a name="authorization-types"></a><span data-ttu-id="d1121-110">Tipos de autorização</span><span class="sxs-lookup"><span data-stu-id="d1121-110">Authorization Types</span></span>

<span data-ttu-id="d1121-111">Autorização de ASP.NET Core fornece um simples declarativo [função](roles.md) e um [política avançada com base em](policies.md) modelo.</span><span class="sxs-lookup"><span data-stu-id="d1121-111">ASP.NET Core authorization provides a simple declarative [role](roles.md) and a [rich policy based](policies.md) model.</span></span> <span data-ttu-id="d1121-112">Autorização é expresso em requisitos e manipuladores de avaliar as declarações de um usuário em relação aos requisitos.</span><span class="sxs-lookup"><span data-stu-id="d1121-112">Authorization is expressed in requirements, and handlers evaluate a user's claims against requirements.</span></span> <span data-ttu-id="d1121-113">Verificações imperativas podem ser baseadas em políticas simples ou políticas que avalie a identidade do usuário e propriedades do recurso que o usuário está tentando acessar.</span><span class="sxs-lookup"><span data-stu-id="d1121-113">Imperative checks can be based on simple policies or policies which evaluate both the user identity and properties of the resource that the user is attempting to access.</span></span>

## <a name="namespaces"></a><span data-ttu-id="d1121-114">Namespaces</span><span class="sxs-lookup"><span data-stu-id="d1121-114">Namespaces</span></span>

<span data-ttu-id="d1121-115">Componentes de autorização, incluindo o `AuthorizeAttribute` e `AllowAnonymousAttribute` atributos encontrados no `Microsoft.AspNetCore.Authorization` namespace.</span><span class="sxs-lookup"><span data-stu-id="d1121-115">Authorization components, including the `AuthorizeAttribute` and `AllowAnonymousAttribute` attributes are found in the `Microsoft.AspNetCore.Authorization` namespace.</span></span>
