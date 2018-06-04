---
title: Examine os métodos Details e Delete de um aplicativo ASP.NET Core
author: rick-anderson
description: Saiba mais sobre o método e a exibição Details do controlador em um aplicativo ASP.NET Core MVC básico.
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/details
ms.openlocfilehash: b392f956888a740a4a8c7c553996fc85ce63bd4b
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34729630"
---
# <a name="examine-the-details-and-delete-methods-of-an-aspnet-core-app"></a><span data-ttu-id="7d870-103">Examine os métodos Details e Delete de um aplicativo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7d870-103">Examine the Details and Delete methods of an ASP.NET Core app</span></span>

<span data-ttu-id="7d870-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7d870-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7d870-105">Abra o controlador Movie e examine o método `Details`:</span><span class="sxs-lookup"><span data-stu-id="7d870-105">Open the Movie controller and examine the `Details` method:</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="7d870-106">[!code-csharp[](start-mvc/sample/MvcMovie21/Controllers/MoviesController.cs?name=snippet_details)]</span><span class="sxs-lookup"><span data-stu-id="7d870-106">[!code-csharp[](start-mvc/sample/MvcMovie21/Controllers/MoviesController.cs?name=snippet_details)]</span></span>

::: moniker-end
::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="7d870-107">[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]</span><span class="sxs-lookup"><span data-stu-id="7d870-107">[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]</span></span>

::: moniker-end

<span data-ttu-id="7d870-108">O mecanismo de scaffolding MVC que criou este método de ação adiciona um comentário mostrando uma solicitação HTTP que invoca o método.</span><span class="sxs-lookup"><span data-stu-id="7d870-108">The MVC scaffolding engine that created this action method adds a comment showing an HTTP request that invokes the method.</span></span> <span data-ttu-id="7d870-109">Nesse caso, é uma solicitação GET com três segmentos de URL, o controlador `Movies`, o método `Details` e um valor `id`.</span><span class="sxs-lookup"><span data-stu-id="7d870-109">In this case it's a GET request with three URL segments, the `Movies` controller, the `Details` method and an `id` value.</span></span> <span data-ttu-id="7d870-110">Lembre-se que esses segmentos são definidos em *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="7d870-110">Recall these segments are defined in *Startup.cs*.</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

<span data-ttu-id="7d870-111">O EF facilita a pesquisa de dados usando o método `SingleOrDefaultAsync`.</span><span class="sxs-lookup"><span data-stu-id="7d870-111">EF makes it easy to search for data using the `SingleOrDefaultAsync` method.</span></span> <span data-ttu-id="7d870-112">Um recurso de segurança importante interno do método é que o código verifica se o método de pesquisa encontrou um filme antes de tentar fazer algo com ele.</span><span class="sxs-lookup"><span data-stu-id="7d870-112">An important security feature built into the method is that the code verifies that the search method has found a movie before it tries to do anything with it.</span></span> <span data-ttu-id="7d870-113">Por exemplo, um hacker pode introduzir erros no site alterando a URL criada pelos links de `http://localhost:xxxx/Movies/Details/1` para algo como `http://localhost:xxxx/Movies/Details/12345` (ou algum outro valor que não representa um filme real).</span><span class="sxs-lookup"><span data-stu-id="7d870-113">For example, a hacker could introduce errors into the site by changing the URL created by the links from `http://localhost:xxxx/Movies/Details/1` to something like  `http://localhost:xxxx/Movies/Details/12345` (or some other value that doesn't represent an actual movie).</span></span> <span data-ttu-id="7d870-114">Se você não marcou um filme nulo, o aplicativo gerará uma exceção.</span><span class="sxs-lookup"><span data-stu-id="7d870-114">If you didn't check for a null movie, the app would throw an exception.</span></span>

<span data-ttu-id="7d870-115">Examine os métodos `Delete` e `DeleteConfirmed`.</span><span class="sxs-lookup"><span data-stu-id="7d870-115">Examine the `Delete` and `DeleteConfirmed` methods.</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="7d870-116">[!code-csharp[](start-mvc/sample/MvcMovie21/Controllers/MoviesController.cs?name=snippet_delete)]</span><span class="sxs-lookup"><span data-stu-id="7d870-116">[!code-csharp[](start-mvc/sample/MvcMovie21/Controllers/MoviesController.cs?name=snippet_delete)]</span></span>

::: moniker-end
::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="7d870-117">[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete)]</span><span class="sxs-lookup"><span data-stu-id="7d870-117">[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete)]</span></span>

::: moniker-end

<span data-ttu-id="7d870-118">Observe que o método `HTTP GET Delete` não exclui o filme especificado, mas retorna uma exibição do filme em que você pode enviar (HttpPost) a exclusão.</span><span class="sxs-lookup"><span data-stu-id="7d870-118">Note that the `HTTP GET Delete` method doesn't delete the specified movie, it returns a view of the movie where you can submit (HttpPost) the deletion.</span></span> <span data-ttu-id="7d870-119">A execução de uma operação de exclusão em resposta a uma solicitação GET (ou, de fato, a execução de uma operação de edição, criação ou qualquer outra operação que altera dados) abre uma falha de segurança.</span><span class="sxs-lookup"><span data-stu-id="7d870-119">Performing a delete operation in response to a GET request (or for that matter, performing an edit operation, create operation, or any other operation that changes data) opens up a security hole.</span></span>

<span data-ttu-id="7d870-120">O método `[HttpPost]` que exclui os dados é chamado `DeleteConfirmed` para fornecer ao método HTTP POST um nome ou uma assinatura exclusiva.</span><span class="sxs-lookup"><span data-stu-id="7d870-120">The `[HttpPost]` method that deletes the data is named `DeleteConfirmed` to give the HTTP POST method a unique signature or name.</span></span> <span data-ttu-id="7d870-121">As duas assinaturas de método são mostradas abaixo:</span><span class="sxs-lookup"><span data-stu-id="7d870-121">The two method signatures are shown below:</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete2)]

[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete3)]


<span data-ttu-id="7d870-122">O CLR (Common Language Runtime) exige que os métodos sobrecarregados tenham uma assinatura de parâmetro exclusiva (mesmo nome de método, mas uma lista diferente de parâmetros).</span><span class="sxs-lookup"><span data-stu-id="7d870-122">The common language runtime (CLR) requires overloaded methods to have a unique parameter signature (same method name but different list of parameters).</span></span> <span data-ttu-id="7d870-123">No entanto, aqui você precisa de dois métodos `Delete` – um para GET e outro para POST – que têm a mesma assinatura de parâmetro.</span><span class="sxs-lookup"><span data-stu-id="7d870-123">However, here you need two `Delete` methods -- one for GET and one for POST -- that both have the same parameter signature.</span></span> <span data-ttu-id="7d870-124">(Ambos precisam aceitar um único inteiro como parâmetro.)</span><span class="sxs-lookup"><span data-stu-id="7d870-124">(They both need to accept a single integer as a parameter.)</span></span>

<span data-ttu-id="7d870-125">Há duas abordagens para esse problema e uma delas é fornecer aos métodos nomes diferentes.</span><span class="sxs-lookup"><span data-stu-id="7d870-125">There are two approaches to this problem, one is to give the methods different names.</span></span> <span data-ttu-id="7d870-126">Foi isso o que o mecanismo de scaffolding fez no exemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="7d870-126">That's what the scaffolding mechanism did in the preceding example.</span></span> <span data-ttu-id="7d870-127">No entanto, isso apresenta um pequeno problema: o ASP.NET mapeia os segmentos de URL para os métodos de ação por nome e se você renomear um método, o roteamento normalmente não conseguirá encontrar esse método.</span><span class="sxs-lookup"><span data-stu-id="7d870-127">However, this introduces a small problem: ASP.NET maps segments of a URL to action methods by name, and if you rename a method, routing normally wouldn't be able to find that method.</span></span> <span data-ttu-id="7d870-128">A solução é o que você vê no exemplo, que é adicionar o atributo `ActionName("Delete")` ao método `DeleteConfirmed`.</span><span class="sxs-lookup"><span data-stu-id="7d870-128">The solution is what you see in the example, which is to add the `ActionName("Delete")` attribute to the `DeleteConfirmed` method.</span></span> <span data-ttu-id="7d870-129">Esse atributo executa o mapeamento para o sistema de roteamento, de modo que uma URL que inclui /Delete/ para uma solicitação POST encontre o método `DeleteConfirmed`.</span><span class="sxs-lookup"><span data-stu-id="7d870-129">That attribute performs mapping for the routing system so that a URL that includes /Delete/ for a POST request will find the `DeleteConfirmed` method.</span></span>

<span data-ttu-id="7d870-130">Outra solução alternativa comum para métodos que têm nomes e assinaturas idênticos é alterar artificialmente a assinatura do método POST para incluir um parâmetro extra (não utilizado).</span><span class="sxs-lookup"><span data-stu-id="7d870-130">Another common work around for methods that have identical names and signatures is to artificially change the signature of the POST method to include an extra (unused) parameter.</span></span> <span data-ttu-id="7d870-131">Foi isso o que fizemos em uma postagem anterior quando adicionamos o parâmetro `notUsed`.</span><span class="sxs-lookup"><span data-stu-id="7d870-131">That's what we did in a previous post when we added the `notUsed` parameter.</span></span> <span data-ttu-id="7d870-132">Você pode fazer o mesmo aqui para o método `[HttpPost] Delete`:</span><span class="sxs-lookup"><span data-stu-id="7d870-132">You could do the same thing here for the `[HttpPost] Delete` method:</span></span>

```csharp
// POST: Movies/Delete/6
[ValidateAntiForgeryToken]
public async Task<IActionResult> Delete(int id, bool notUsed)
```

### <a name="publish-to-azure"></a><span data-ttu-id="7d870-133">Publicar no Azure</span><span class="sxs-lookup"><span data-stu-id="7d870-133">Publish to Azure</span></span>

<span data-ttu-id="7d870-134">Confira as instruções sobre como publicar este aplicativo no Azure usando o Visual Studio em [Publicar um aplicativo Web ASP.NET Core no Serviço de Aplicativo do Azure usando o Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs).</span><span class="sxs-lookup"><span data-stu-id="7d870-134">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on how to publish this app to Azure using Visual Studio.</span></span>  <span data-ttu-id="7d870-135">O aplicativo também pode ser publicado a partir da [linha de comando](xref:tutorials/publish-to-azure-webapp-using-cli).</span><span class="sxs-lookup"><span data-stu-id="7d870-135">The app can also be published from the [command line](xref:tutorials/publish-to-azure-webapp-using-cli).</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="7d870-136">Anterior</span><span class="sxs-lookup"><span data-stu-id="7d870-136">Previous</span></span>](validation.md)
