---
title: Trabalhando com os dados no ASP.NET Core
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: a8fb7eb7-e0e5-4394-84f3-1f1dbe0ba2ef
ms.technology: aspnet
ms.prod: asp.net-core
ms.openlocfilehash: 3566127476289ae085a9161132b103638bc9b068
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
# <a name="working-with-data-in-aspnet-core"></a><span data-ttu-id="a243b-103">Trabalhando com os dados no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a243b-103">Working with Data in ASP.NET Core</span></span> 

*   [<span data-ttu-id="a243b-104">Introdução ao ASP.NET Core e ao Entity Framework Core usando o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a243b-104">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](ef-mvc/index.md)
    *   [<span data-ttu-id="a243b-105">Introdução</span><span class="sxs-lookup"><span data-stu-id="a243b-105">Getting started</span></span>](ef-mvc/intro.md)
    *   [<span data-ttu-id="a243b-106">Operações Create, Read, Update e Delete</span><span class="sxs-lookup"><span data-stu-id="a243b-106">Create, Read, Update, and Delete operations</span></span>](ef-mvc/crud.md)
    *   [<span data-ttu-id="a243b-107">Classificação, filtragem, paginação e agrupamento</span><span class="sxs-lookup"><span data-stu-id="a243b-107">Sorting, filtering, paging, and grouping</span></span>](ef-mvc/sort-filter-page.md)
    *   [<span data-ttu-id="a243b-108">Migrações</span><span class="sxs-lookup"><span data-stu-id="a243b-108">Migrations</span></span>](ef-mvc/migrations.md)
    *   [<span data-ttu-id="a243b-109">Criando um modelo de dados complexo</span><span class="sxs-lookup"><span data-stu-id="a243b-109">Creating a complex data model</span></span>](ef-mvc/complex-data-model.md)
    *   [<span data-ttu-id="a243b-110">Lendo dados relacionados</span><span class="sxs-lookup"><span data-stu-id="a243b-110">Reading related data</span></span>](ef-mvc/read-related-data.md)
    *   [<span data-ttu-id="a243b-111">Atualizando dados relacionados</span><span class="sxs-lookup"><span data-stu-id="a243b-111">Updating related data</span></span>](ef-mvc/update-related-data.md)
    *   [<span data-ttu-id="a243b-112">Tratando conflitos de simultaneidade</span><span class="sxs-lookup"><span data-stu-id="a243b-112">Handling concurrency conflicts</span></span>](ef-mvc/concurrency.md)
    *   [<span data-ttu-id="a243b-113">Herança</span><span class="sxs-lookup"><span data-stu-id="a243b-113">Inheritance</span></span>](ef-mvc/inheritance.md)
    *   [<span data-ttu-id="a243b-114">Tópicos avançados</span><span class="sxs-lookup"><span data-stu-id="a243b-114">Advanced topics</span></span>](ef-mvc/advanced.md)
* <span data-ttu-id="a243b-115">[ASP.NET Core com o EF Core – novo banco de dados](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) (site de documentação do Entity Framework Core)</span><span class="sxs-lookup"><span data-stu-id="a243b-115">[ASP.NET Core with EF Core - new database](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) (Entity Framework Core documentation site)</span></span>
* <span data-ttu-id="a243b-116">[ASP.NET Core com o EF Core – banco de dados existente](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db) (site de documentação do Entity Framework Core)</span><span class="sxs-lookup"><span data-stu-id="a243b-116">[ASP.NET Core with EF Core - existing database](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db) (Entity Framework Core documentation site)</span></span>
*   [<span data-ttu-id="a243b-117">Introdução ao ASP.NET Core e ao Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="a243b-117">Getting Started with ASP.NET Core and Entity Framework 6</span></span>](entity-framework-6.md)
*   [<span data-ttu-id="a243b-118">Armazenamento do Azure</span><span class="sxs-lookup"><span data-stu-id="a243b-118">Azure Storage</span></span>](azure-storage/index.md)
    *   [<span data-ttu-id="a243b-119">Adicionando o Armazenamento do Azure Usando o Visual Studio Connected Services</span><span class="sxs-lookup"><span data-stu-id="a243b-119">Adding Azure Storage by Using Visual Studio Connected Services</span></span>](https://azure.microsoft.com/documentation/articles/vs-azure-tools-connected-services-storage/)
    *   [<span data-ttu-id="a243b-120">Introdução ao Armazenamento de Blobs do Azure e ao Visual Studio Connected Services</span><span class="sxs-lookup"><span data-stu-id="a243b-120">Get Started with Azure Blob storage and Visual Studio Connected Services</span></span>](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-blobs/)
    *   [<span data-ttu-id="a243b-121">Introdução ao Armazenamento de Filas e ao Visual Studio Connected Services</span><span class="sxs-lookup"><span data-stu-id="a243b-121">Get Started with Queue Storage and Visual Studio Connected Services</span></span>](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-queues/)
    *   [<span data-ttu-id="a243b-122">Introdução ao Armazenamento de Tabelas do Azure e ao Visual Studio Connected Services</span><span class="sxs-lookup"><span data-stu-id="a243b-122">How to Get Started with Azure Table Storage and Visual Studio Connected Services</span></span>](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-tables/)
