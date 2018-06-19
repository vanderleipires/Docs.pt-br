---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-5
title: Criar objetos de transferência de dados (DTOs) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 0fd07176-b74b-48f0-9fac-0f02e3ffa213
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-5
msc.type: authoredcontent
ms.openlocfilehash: 35e01f959072b96204de3e2ce3d507635720e110
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878679"
---
<a name="create-data-transfer-objects-dtos"></a><span data-ttu-id="08617-102">Criar objetos de transferência de dados (DTOs)</span><span class="sxs-lookup"><span data-stu-id="08617-102">Create Data Transfer Objects (DTOs)</span></span>
====================
<span data-ttu-id="08617-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="08617-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="08617-104">Baixe o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="08617-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="08617-105">Direita agora, nossa API da web expõe as entidades de banco de dados ao cliente.</span><span class="sxs-lookup"><span data-stu-id="08617-105">Right now, our web API exposes the database entities to the client.</span></span> <span data-ttu-id="08617-106">O cliente recebe os dados que mapeia diretamente para as tabelas de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="08617-106">The client receives data that maps directly to your database tables.</span></span> <span data-ttu-id="08617-107">No entanto, que não é sempre uma boa ideia.</span><span class="sxs-lookup"><span data-stu-id="08617-107">However, that's not always a good idea.</span></span> <span data-ttu-id="08617-108">Às vezes você deseja alterar a forma dos dados que você enviar ao cliente.</span><span class="sxs-lookup"><span data-stu-id="08617-108">Sometimes you want to change the shape of the data that you send to client.</span></span> <span data-ttu-id="08617-109">Por exemplo, é possível:</span><span class="sxs-lookup"><span data-stu-id="08617-109">For example, you might want to:</span></span>

- <span data-ttu-id="08617-110">Remova as referências circulares (consulte a seção anterior).</span><span class="sxs-lookup"><span data-stu-id="08617-110">Remove circular references (see previous section).</span></span>
- <span data-ttu-id="08617-111">Oculte propriedades específicas que os clientes não devem para exibir.</span><span class="sxs-lookup"><span data-stu-id="08617-111">Hide particular properties that clients are not supposed to view.</span></span>
- <span data-ttu-id="08617-112">Omita algumas propriedades para reduzir o tamanho da carga.</span><span class="sxs-lookup"><span data-stu-id="08617-112">Omit some properties in order to reduce payload size.</span></span>
- <span data-ttu-id="08617-113">Mesclar os gráficos de objetos que contêm objetos aninhados, para torná-las mais conveniente para os clientes.</span><span class="sxs-lookup"><span data-stu-id="08617-113">Flatten object graphs that contain nested objects, to make them more convenient for clients.</span></span>
- <span data-ttu-id="08617-114">Evite "excesso de lançamento" vulnerabilidades.</span><span class="sxs-lookup"><span data-stu-id="08617-114">Avoid "over-posting" vulnerabilities.</span></span> <span data-ttu-id="08617-115">(Consulte [validação de modelo](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) para obter uma discussão de lançamento excedente.)</span><span class="sxs-lookup"><span data-stu-id="08617-115">(See [Model Validation](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) for a discussion of over-posting.)</span></span>
- <span data-ttu-id="08617-116">Desacopla a camada de serviço de sua camada de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="08617-116">Decouple your service layer from your database layer.</span></span>

<span data-ttu-id="08617-117">Para fazer isso, você pode definir um *o objeto de transferência de dados* (DTO).</span><span class="sxs-lookup"><span data-stu-id="08617-117">To accomplish this, you can define a *data transfer object* (DTO).</span></span> <span data-ttu-id="08617-118">Um DTO é um objeto que define como os dados serão enviados pela rede.</span><span class="sxs-lookup"><span data-stu-id="08617-118">A DTO is an object that defines how the data will be sent over the network.</span></span> <span data-ttu-id="08617-119">Vamos ver como isso funciona com a entidade de catálogo.</span><span class="sxs-lookup"><span data-stu-id="08617-119">Let's see how that works with the Book entity.</span></span> <span data-ttu-id="08617-120">Na pasta de modelos, adicione duas classes DTO:</span><span class="sxs-lookup"><span data-stu-id="08617-120">In the Models folder, add two DTO classes:</span></span>

[!code-csharp[Main](part-5/samples/sample1.cs)]

<span data-ttu-id="08617-121">O `BookDetailDTO` classe inclui todas as propriedades do modelo de catálogo, exceto que `AuthorName` é uma cadeia de caracteres que conterá o nome do autor.</span><span class="sxs-lookup"><span data-stu-id="08617-121">The `BookDetailDTO` class includes all of the properties from the Book model, except that `AuthorName` is a string that will hold the author name.</span></span> <span data-ttu-id="08617-122">O `BookDTO` classe contém um subconjunto de propriedades de `BookDetailDTO`.</span><span class="sxs-lookup"><span data-stu-id="08617-122">The `BookDTO` class contains a subset of properties from `BookDetailDTO`.</span></span>

<span data-ttu-id="08617-123">Em seguida, substitua os dois métodos GET no `BooksController` classe com versões que retornar DTOs.</span><span class="sxs-lookup"><span data-stu-id="08617-123">Next, replace the two GET methods in the `BooksController` class, with versions that return DTOs.</span></span> <span data-ttu-id="08617-124">Vamos usar o LINQ **selecione** instrução converter de entidades de catálogo em DTOs.</span><span class="sxs-lookup"><span data-stu-id="08617-124">We'll use the LINQ **Select** statement to convert from Book entities into DTOs.</span></span>

[!code-csharp[Main](part-5/samples/sample2.cs)]

<span data-ttu-id="08617-125">Aqui está o SQL gerado pelo novo `GetBooks` método.</span><span class="sxs-lookup"><span data-stu-id="08617-125">Here is the SQL generated by the new `GetBooks` method.</span></span> <span data-ttu-id="08617-126">Você pode ver que o EF converte o LINQ **selecione** em uma instrução SQL SELECT.</span><span class="sxs-lookup"><span data-stu-id="08617-126">You can see that EF translates the LINQ **Select** into a SQL SELECT statement.</span></span>

[!code-sql[Main](part-5/samples/sample3.sql)]

<span data-ttu-id="08617-127">Finalmente, modifique o `PostBook` método para retornar um DTO.</span><span class="sxs-lookup"><span data-stu-id="08617-127">Finally, modify the `PostBook` method to return a DTO.</span></span>

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="08617-128">Neste tutorial, estamos convertendo em DTOs manualmente no código.</span><span class="sxs-lookup"><span data-stu-id="08617-128">In this tutorial, we're converting to DTOs manually in code.</span></span> <span data-ttu-id="08617-129">Outra opção é usar uma biblioteca como [AutoMapper](http://automapper.org/) que manipula a conversão automaticamente.</span><span class="sxs-lookup"><span data-stu-id="08617-129">Another option is to use a library like [AutoMapper](http://automapper.org/) that handles the conversion automatically.</span></span>
> 
> [!div class="step-by-step"]
> <span data-ttu-id="08617-130">[Anterior](part-4.md)
> [Próximo](part-6.md)</span><span class="sxs-lookup"><span data-stu-id="08617-130">[Previous](part-4.md)
[Next](part-6.md)</span></span>
