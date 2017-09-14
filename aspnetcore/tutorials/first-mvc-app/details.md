---
title: "Examinando os métodos Details e Delete"
author: rick-anderson
description: "A exibição e o método do controlador Details em um aplicativo ASP.NET Core MVC simples."
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: get-started-article
ms.assetid: 870192b4-8d4f-45c7-8c14-83d02bc0ad79
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/details
ms.openlocfilehash: bab93a2faa122d9d6d2e71367519baa09bd76bd1
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
# <a name="examining-the-details-and-delete-methods"></a><span data-ttu-id="f1825-104">Examinando os métodos Details e Delete</span><span class="sxs-lookup"><span data-stu-id="f1825-104">Examining the Details and Delete methods</span></span>

<span data-ttu-id="f1825-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f1825-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f1825-106">Abra o controlador Movie e examine o método `Details`:</span><span class="sxs-lookup"><span data-stu-id="f1825-106">Open the Movie controller and examine the `Details` method:</span></span>

<span data-ttu-id="f1825-107">[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]</span><span class="sxs-lookup"><span data-stu-id="f1825-107">[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]</span></span>

<span data-ttu-id="f1825-108">O mecanismo de scaffolding MVC que criou este método de ação adiciona um comentário mostrando uma solicitação HTTP que invoca o método.</span><span class="sxs-lookup"><span data-stu-id="f1825-108">The MVC scaffolding engine that created this action method adds a comment showing an HTTP request that invokes the method.</span></span> <span data-ttu-id="f1825-109">Nesse caso, é uma solicitação GET com três segmentos de URL, o controlador `Movies`, o método `Details` e um valor `id`.</span><span class="sxs-lookup"><span data-stu-id="f1825-109">In this case it's a GET request with three URL segments, the `Movies` controller, the `Details` method and an `id` value.</span></span> <span data-ttu-id="f1825-110">Lembre-se que esses segmentos são definidos em *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="f1825-110">Recall these segments are defined in *Startup.cs*.</span></span>

<span data-ttu-id="f1825-111">[!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]</span><span class="sxs-lookup"><span data-stu-id="f1825-111">[!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]</span></span>

<span data-ttu-id="f1825-112">O EF facilita a pesquisa de dados usando o método `SingleOrDefaultAsync`.</span><span class="sxs-lookup"><span data-stu-id="f1825-112">EF makes it easy to search for data using the `SingleOrDefaultAsync` method.</span></span> <span data-ttu-id="f1825-113">Um recurso de segurança importante interno do método é que o código verifica se o método de pesquisa encontrou um filme antes de tentar fazer algo com ele.</span><span class="sxs-lookup"><span data-stu-id="f1825-113">An important security feature built into the method is that the code verifies that the search method has found a movie before it tries to do anything with it.</span></span> <span data-ttu-id="f1825-114">Por exemplo, um hacker pode introduzir erros no site alterando a URL criada pelos links de `http://localhost:xxxx/Movies/Details/1` para algo como `http://localhost:xxxx/Movies/Details/12345` (ou algum outro valor que não representa um filme real).</span><span class="sxs-lookup"><span data-stu-id="f1825-114">For example, a hacker could introduce errors into the site by changing the URL created by the links from `http://localhost:xxxx/Movies/Details/1` to something like  `http://localhost:xxxx/Movies/Details/12345` (or some other value that doesn't represent an actual movie).</span></span> <span data-ttu-id="f1825-115">Se você não marcou um filme nulo, o aplicativo gerará uma exceção.</span><span class="sxs-lookup"><span data-stu-id="f1825-115">If you did not check for a null movie, the app would throw an exception.</span></span>

<span data-ttu-id="f1825-116">Examine os métodos `Delete` e `DeleteConfirmed`.</span><span class="sxs-lookup"><span data-stu-id="f1825-116">Examine the `Delete` and `DeleteConfirmed` methods.</span></span>

<span data-ttu-id="f1825-117">[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete)]</span><span class="sxs-lookup"><span data-stu-id="f1825-117">[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete)]</span></span>

<span data-ttu-id="f1825-118">Observe que o método `HTTP GET Delete` não exclui o filme especificado, mas retorna uma exibição do filme em que você pode enviar (HttpPost) a exclusão.</span><span class="sxs-lookup"><span data-stu-id="f1825-118">Note that the `HTTP GET Delete` method doesn't delete the specified movie, it returns a view of the movie where you can submit (HttpPost) the deletion.</span></span> <span data-ttu-id="f1825-119">A execução de uma operação de exclusão em resposta a uma solicitação GET (ou, de fato, a execução de uma operação de edição, criação ou qualquer outra operação que altera dados) abre uma falha de segurança.</span><span class="sxs-lookup"><span data-stu-id="f1825-119">Performing a delete operation in response to a GET request (or for that matter, performing an edit operation, create operation, or any other operation that changes data) opens up a security hole.</span></span>

<span data-ttu-id="f1825-120">O método `[HttpPost]` que exclui os dados é chamado `DeleteConfirmed` para fornecer ao método HTTP POST um nome ou uma assinatura exclusiva.</span><span class="sxs-lookup"><span data-stu-id="f1825-120">The `[HttpPost]` method that deletes the data is named `DeleteConfirmed` to give the HTTP POST method a unique signature or name.</span></span> <span data-ttu-id="f1825-121">As duas assinaturas de método são mostradas abaixo:</span><span class="sxs-lookup"><span data-stu-id="f1825-121">The two method signatures are shown below:</span></span>

<span data-ttu-id="f1825-122">[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete2)]</span><span class="sxs-lookup"><span data-stu-id="f1825-122">[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete2)]</span></span>

<span data-ttu-id="f1825-123">[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete3)]</span><span class="sxs-lookup"><span data-stu-id="f1825-123">[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete3)]</span></span>


<span data-ttu-id="f1825-124">O CLR (Common Language Runtime) exige que os métodos sobrecarregados tenham uma assinatura de parâmetro exclusiva (mesmo nome de método, mas uma lista diferente de parâmetros).</span><span class="sxs-lookup"><span data-stu-id="f1825-124">The common language runtime (CLR) requires overloaded methods to have a unique parameter signature (same method name but different list of parameters).</span></span> <span data-ttu-id="f1825-125">No entanto, aqui você precisa de dois métodos `Delete` – um para GET e outro para POST – que têm a mesma assinatura de parâmetro.</span><span class="sxs-lookup"><span data-stu-id="f1825-125">However, here you need two `Delete` methods -- one for GET and one for POST -- that both have the same parameter signature.</span></span> <span data-ttu-id="f1825-126">(Ambos precisam aceitar um único inteiro como parâmetro.)</span><span class="sxs-lookup"><span data-stu-id="f1825-126">(They both need to accept a single integer as a parameter.)</span></span>

<span data-ttu-id="f1825-127">Há duas abordagens para esse problema e uma delas é fornecer aos métodos nomes diferentes.</span><span class="sxs-lookup"><span data-stu-id="f1825-127">There are two approaches to this problem, one is to give the methods different names.</span></span> <span data-ttu-id="f1825-128">Foi isso o que o mecanismo de scaffolding fez no exemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="f1825-128">That's what the scaffolding mechanism did in the preceding example.</span></span> <span data-ttu-id="f1825-129">No entanto, isso apresenta um pequeno problema: o ASP.NET mapeia os segmentos de URL para os métodos de ação por nome e se você renomear um método, o roteamento normalmente não conseguirá encontrar esse método.</span><span class="sxs-lookup"><span data-stu-id="f1825-129">However, this introduces a small problem: ASP.NET maps segments of a URL to action methods by name, and if you rename a method, routing normally wouldn't be able to find that method.</span></span> <span data-ttu-id="f1825-130">A solução é o que você vê no exemplo, que é adicionar o atributo `ActionName("Delete")` ao método `DeleteConfirmed`.</span><span class="sxs-lookup"><span data-stu-id="f1825-130">The solution is what you see in the example, which is to add the `ActionName("Delete")` attribute to the `DeleteConfirmed` method.</span></span> <span data-ttu-id="f1825-131">Esse atributo executa o mapeamento para o sistema de roteamento, de modo que uma URL que inclui /Delete/ para uma solicitação POST encontre o método `DeleteConfirmed`.</span><span class="sxs-lookup"><span data-stu-id="f1825-131">That attribute performs mapping for the routing system so that a URL that includes /Delete/ for a POST request will find the `DeleteConfirmed` method.</span></span>

<span data-ttu-id="f1825-132">Outra solução alternativa comum para métodos que têm nomes e assinaturas idênticos é alterar artificialmente a assinatura do método POST para incluir um parâmetro extra (não utilizado).</span><span class="sxs-lookup"><span data-stu-id="f1825-132">Another common work around for methods that have identical names and signatures is to artificially change the signature of the POST method to include an extra (unused) parameter.</span></span> <span data-ttu-id="f1825-133">Foi isso o que fizemos em uma postagem anterior quando adicionamos o parâmetro `notUsed`.</span><span class="sxs-lookup"><span data-stu-id="f1825-133">That's what we did in a previous post when we added the `notUsed` parameter.</span></span> <span data-ttu-id="f1825-134">Você pode fazer o mesmo aqui para o método `[HttpPost] Delete`:</span><span class="sxs-lookup"><span data-stu-id="f1825-134">You could do the same thing here for the `[HttpPost] Delete` method:</span></span>

```csharp
// POST: Movies/Delete/6
[ValidateAntiForgeryToken]
public async Task<IActionResult> Delete(int id, bool notUsed)
```

<span data-ttu-id="f1825-135">Obrigado por concluir esta introdução ao ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="f1825-135">Thanks for completing this introduction to ASP.NET Core MVC.</span></span> <span data-ttu-id="f1825-136">Agradecemos todos os comentários deixados.</span><span class="sxs-lookup"><span data-stu-id="f1825-136">We appreciate any comments you leave.</span></span> <span data-ttu-id="f1825-137">[Introdução ao MVC e ao EF Core](xref:data/ef-mvc/intro) é um excelente acompanhamento para este tutorial.</span><span class="sxs-lookup"><span data-stu-id="f1825-137">[Getting started with MVC and EF Core](xref:data/ef-mvc/intro) is an excellent follow up to this tutorial.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="f1825-138">Anterior</span><span class="sxs-lookup"><span data-stu-id="f1825-138">Previous</span></span>](validation.md)
