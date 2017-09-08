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
# <a name="introduction"></a><span data-ttu-id="d8d86-103">Introdução</span><span class="sxs-lookup"><span data-stu-id="d8d86-103">Introduction</span></span>

<a name=security-authorization-introduction></a>

<span data-ttu-id="d8d86-104">Autorização é o processo que determina o que um usuário é capaz de fazer.</span><span class="sxs-lookup"><span data-stu-id="d8d86-104">Authorization refers to the process that determines what a user is able to do.</span></span> <span data-ttu-id="d8d86-105">Por exemplo, um usuário administrativo tem permissão para criar uma biblioteca de documentos, documentos de adicionar, editar documentos e excluí-los.</span><span class="sxs-lookup"><span data-stu-id="d8d86-105">For example, an administrative user is allowed to create a document library, add documents, edit documents, and delete them.</span></span> <span data-ttu-id="d8d86-106">Um usuário não administrativo trabalhando com a biblioteca só está autorizado a ler os documentos.</span><span class="sxs-lookup"><span data-stu-id="d8d86-106">A non-administrative user working with the library is only authorized to read the documents.</span></span>

<span data-ttu-id="d8d86-107">A autorização é ortogonal e independente de autenticação, que é o processo de verificação de quem é um usuário.</span><span class="sxs-lookup"><span data-stu-id="d8d86-107">Authorization is orthogonal and independent from authentication, which is the process of ascertaining who a user is.</span></span> <span data-ttu-id="d8d86-108">A autenticação pode criar um ou mais identidades para o usuário atual.</span><span class="sxs-lookup"><span data-stu-id="d8d86-108">Authentication may create one or more identities for the current user.</span></span>

## <a name="authorization-types"></a><span data-ttu-id="d8d86-109">Tipos de autorização</span><span class="sxs-lookup"><span data-stu-id="d8d86-109">Authorization Types</span></span>

<span data-ttu-id="d8d86-110">Autorização de ASP.NET Core fornece um simples declarativo [função](roles.md#security-authorization-role-based) e um [política avançada com base em](policies.md#security-authorization-policies-based) modelo.</span><span class="sxs-lookup"><span data-stu-id="d8d86-110">ASP.NET Core authorization provides a simple declarative [role](roles.md#security-authorization-role-based) and a [rich policy based](policies.md#security-authorization-policies-based) model.</span></span> <span data-ttu-id="d8d86-111">Autorização é expresso em requisitos e manipuladores de avaliar as declarações de um usuário em relação aos requisitos.</span><span class="sxs-lookup"><span data-stu-id="d8d86-111">Authorization is expressed in requirements, and handlers evaluate a user's claims against requirements.</span></span> <span data-ttu-id="d8d86-112">Verificações imperativas podem ser baseadas em políticas simples ou políticas que avalie a identidade do usuário e propriedades do recurso que o usuário está tentando acessar.</span><span class="sxs-lookup"><span data-stu-id="d8d86-112">Imperative checks can be based on simple policies or policies which evaluate both the user identity and properties of the resource that the user is attempting to access.</span></span>

## <a name="namespaces"></a><span data-ttu-id="d8d86-113">Namespaces</span><span class="sxs-lookup"><span data-stu-id="d8d86-113">Namespaces</span></span>

<span data-ttu-id="d8d86-114">Componentes de autorização, incluindo o `AuthorizeAttribute` e `AllowAnonymousAttribute` atributos encontrados no `Microsoft.AspNetCore.Authorization` namespace.</span><span class="sxs-lookup"><span data-stu-id="d8d86-114">Authorization components, including the `AuthorizeAttribute` and `AllowAnonymousAttribute` attributes are found in the `Microsoft.AspNetCore.Authorization` namespace.</span></span>
