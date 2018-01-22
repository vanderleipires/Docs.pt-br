---
title: "Atualizando as páginas geradas"
author: rick-anderson
description: "Atualizando as páginas geradas com melhor exibição."
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/razor-pages/da1
ms.openlocfilehash: 201e8d9c77d8e022bc56ffcf46456fada6fcfe25
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="updating-the-generated-pages"></a><span data-ttu-id="cb58d-103">Atualizando as páginas geradas</span><span class="sxs-lookup"><span data-stu-id="cb58d-103">Updating the generated pages</span></span>

<span data-ttu-id="cb58d-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="cb58d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="cb58d-105">Temos um bom começo para o aplicativo de filme, mas a apresentação não é ideal.</span><span class="sxs-lookup"><span data-stu-id="cb58d-105">We have a good start to the movie app, but the presentation is not ideal.</span></span> <span data-ttu-id="cb58d-106">Você não deseja ver a hora (12:00:00 AM na imagem abaixo) e **ReleaseDate** deve ser **Release Date** (duas palavras).</span><span class="sxs-lookup"><span data-stu-id="cb58d-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![Aplicativo de filme aberto no Chrome mostrando os dados do filme](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="cb58d-108">Atualize o código gerado</span><span class="sxs-lookup"><span data-stu-id="cb58d-108">Update the generated code</span></span>

<span data-ttu-id="cb58d-109">Abra o arquivo *Models/Movie.cs* e adicione as linhas realçadas mostradas no seguinte código:</span><span class="sxs-lookup"><span data-stu-id="cb58d-109">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

<span data-ttu-id="cb58d-110">Clique com o botão direito do mouse em uma linha curvada vermelha > ** Ações Rápidas e Refatorações**.</span><span class="sxs-lookup"><span data-stu-id="cb58d-110">Right click on a red squiggly line > ** Quick Actions and Refactorings**.</span></span>

  ![Menu contextual mostra **> Ações Rápidas e Refatorações**.](da1/qa.png)

<span data-ttu-id="cb58d-112">Selecione `using System.ComponentModel.DataAnnotations;`</span><span class="sxs-lookup"><span data-stu-id="cb58d-112">Select `using System.ComponentModel.DataAnnotations;`</span></span>

  ![usando System.ComponentModel.DataAnnotations na parte superior da lista](da1/da.png)

  <span data-ttu-id="cb58d-114">O Visual Studio adiciona `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="cb58d-114">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

<span data-ttu-id="cb58d-115">Abordaremos [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) no próximo tutorial.</span><span class="sxs-lookup"><span data-stu-id="cb58d-115">We'll cover [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) in the next tutorial.</span></span> <span data-ttu-id="cb58d-116">O atributo [Display](https://docs.microsoft.com//aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) especifica o que deve ser exibido no nome de um campo (neste caso, “Release Date” em vez de “ReleaseDate”).</span><span class="sxs-lookup"><span data-stu-id="cb58d-116">The [Display](https://docs.microsoft.com//aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) attribute specifies what to display for the name of a field (in this case "Release Date" instead of "ReleaseDate").</span></span> <span data-ttu-id="cb58d-117">O atributo [DataType](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) especifica o tipo de dados (Data) e, portanto, as informações de hora armazenadas no campo não são exibidas.</span><span class="sxs-lookup"><span data-stu-id="cb58d-117">The [DataType](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute specifies the type of the data (Date), so the time information stored in the field is not displayed.</span></span>

<span data-ttu-id="cb58d-118">Procure Pages/Movies e focalize um link **Editar** para ver a URL de destino.</span><span class="sxs-lookup"><span data-stu-id="cb58d-118">Browse to Pages/Movies and  hover over an **Edit** link to see the target URL.</span></span>

![Janela do navegador com o mouse sobre o link Editar e a URL de link http://localhost:1234/Movies/Edit/5 é mostrada](da1/edit7.png)

<span data-ttu-id="cb58d-120">Os links **Editar**, **Detalhes** e **Excluir** são gerados pelo [Auxiliar de Marcação de Âncora](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) no arquivo *Pages/Movies/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="cb58d-120">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Movies/Index.cshtml* file.</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=16-18&range=32-)]

<span data-ttu-id="cb58d-121">Os [Auxiliares de Marcação](xref:mvc/views/tag-helpers/intro) permitem que o código do servidor participe da criação e renderização de elementos HTML em arquivos do Razor.</span><span class="sxs-lookup"><span data-stu-id="cb58d-121">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="cb58d-122">No código anterior, o `AnchorTagHelper` gera dinamicamente o valor do atributo `href` HTML da página Razor (a rota é relativa), o `asp-page` e a ID da rota (`asp-route-id`).</span><span class="sxs-lookup"><span data-stu-id="cb58d-122">In the preceding code, the `AnchorTagHelper` dynamically generates the HTML `href` attribute value from the Razor Page (the route is relative), the `asp-page`,  and the route id (`asp-route-id`).</span></span> <span data-ttu-id="cb58d-123">Consulte [Geração de URL para Páginas](xref:mvc/razor-pages/index#url-generation-for-pages) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="cb58d-123">See [URL generation for Pages](xref:mvc/razor-pages/index#url-generation-for-pages) for more information.</span></span>

<span data-ttu-id="cb58d-124">Use **Exibir Código-fonte** em seu navegador favorito para examinar a marcação gerada.</span><span class="sxs-lookup"><span data-stu-id="cb58d-124">Use **View Source** from your favorite browser to examine the generated markup.</span></span> <span data-ttu-id="cb58d-125">Uma parte do HTML gerado é mostrada abaixo:</span><span class="sxs-lookup"><span data-stu-id="cb58d-125">A portion of the generated HTML is shown below:</span></span>

```html
<td>
  <a href="/Movies/Edit?id=1">Edit</a> |
  <a href="/Movies/Details?id=1">Details</a> |
  <a href="/Movies/Delete?id=1">Delete</a>
</td>
```

<span data-ttu-id="cb58d-126">Os links gerados dinamicamente passam a ID de filme com uma cadeia de consulta (por exemplo, `http://localhost:5000/Movies/Details?id=2`).</span><span class="sxs-lookup"><span data-stu-id="cb58d-126">The dynamically-generated links pass the movie ID with a query string (for example, `http://localhost:5000/Movies/Details?id=2` ).</span></span> 

<span data-ttu-id="cb58d-127">Atualize as Páginas Editar, Detalhes e Excluir do Razor para que elas usem o modelo de rota “{id:int}”.</span><span class="sxs-lookup"><span data-stu-id="cb58d-127">Update the Edit, Details, and Delete Razor Pages to use the "{id:int}" route template.</span></span> <span data-ttu-id="cb58d-128">Altere a diretiva de página de cada uma dessas páginas de `@page` para `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="cb58d-128">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span> <span data-ttu-id="cb58d-129">Execute o aplicativo e, em seguida, exiba o código-fonte.</span><span class="sxs-lookup"><span data-stu-id="cb58d-129">Run the app and then view source.</span></span> <span data-ttu-id="cb58d-130">O HTML gerado adiciona a ID à parte do caminho da URL:</span><span class="sxs-lookup"><span data-stu-id="cb58d-130">The generated HTML adds the ID to the path portion of the URL:</span></span>

```html
<td>
  <a href="/Movies/Edit/1">Edit</a> |
  <a href="/Movies/Details/1">Details</a> |
  <a href="/Movies/Delete/1">Delete</a>
</td>
```

<span data-ttu-id="cb58d-131">Uma solicitação para a página com o modelo de rota “{id:int}” que **não** inclui o inteiro retornará um erro HTTP 404 (não encontrado).</span><span class="sxs-lookup"><span data-stu-id="cb58d-131">A request to the page with the "{id:int}" route template that does **not** include the integer will return an HTTP 404 (not found) error.</span></span> <span data-ttu-id="cb58d-132">Por exemplo, `http://localhost:5000/Movies/Details` retornará um erro 404.</span><span class="sxs-lookup"><span data-stu-id="cb58d-132">For example, `http://localhost:5000/Movies/Details` will return a 404 error.</span></span> <span data-ttu-id="cb58d-133">Para tornar a ID opcional, acrescente `?` à restrição de rota:</span><span class="sxs-lookup"><span data-stu-id="cb58d-133">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

### <a name="update-concurrency-exception-handling"></a><span data-ttu-id="cb58d-134">Atualizar o tratamento de exceção de simultaneidade</span><span class="sxs-lookup"><span data-stu-id="cb58d-134">Update concurrency exception handling</span></span>

<span data-ttu-id="cb58d-135">Atualize o método `OnPostAsync` no arquivo *Pages/Movies/Edit.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="cb58d-135">Update the `OnPostAsync` method in the *Pages/Movies/Edit.cshtml.cs* file.</span></span> <span data-ttu-id="cb58d-136">O seguinte código realçado mostra as alterações:</span><span class="sxs-lookup"><span data-stu-id="cb58d-136">The following highlighted code shows the changes:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit.cshtml.cs?name=snippet1&highlight=16-23)]

<span data-ttu-id="cb58d-137">O código anterior apenas detecta as exceções de simultaneidade quando o primeiro cliente simultâneo exclui o filme e o segundo cliente simultâneo posta alterações no filme.</span><span class="sxs-lookup"><span data-stu-id="cb58d-137">The previous code only detects concurrency exceptions when the first concurrent client deletes the movie, and the second concurrent client posts changes to the movie.</span></span>

<span data-ttu-id="cb58d-138">Para testar o bloco `catch`:</span><span class="sxs-lookup"><span data-stu-id="cb58d-138">To test the `catch` block:</span></span>

* <span data-ttu-id="cb58d-139">Definir um ponto de interrupção em `catch (DbUpdateConcurrencyException)`</span><span class="sxs-lookup"><span data-stu-id="cb58d-139">Set a breakpoint on `catch (DbUpdateConcurrencyException)`</span></span>
* <span data-ttu-id="cb58d-140">Edite um filme.</span><span class="sxs-lookup"><span data-stu-id="cb58d-140">Edit a movie.</span></span>
* <span data-ttu-id="cb58d-141">Em outra janela do navegador, selecione o link **Excluir** do mesmo filme e, em seguida, exclua o filme.</span><span class="sxs-lookup"><span data-stu-id="cb58d-141">In another browser window, select the **Delete** link for the same movie, and then delete the movie.</span></span>
* <span data-ttu-id="cb58d-142">Na janela do navegador anterior, poste as alterações no filme.</span><span class="sxs-lookup"><span data-stu-id="cb58d-142">In the previous browser window, post changes to the movie.</span></span>

<span data-ttu-id="cb58d-143">O código de produção geralmente detectará conflitos de simultaneidade quando dois ou mais clientes atualizarem um registro ao mesmo tempo.</span><span class="sxs-lookup"><span data-stu-id="cb58d-143">Production code would generally detect concurrency conflicts when two or more clients concurrently updated a record.</span></span> <span data-ttu-id="cb58d-144">Consulte [Tratando conflitos de simultaneidade](xref:data/ef-rp/concurrency) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="cb58d-144">See [Handling concurrency conflicts](xref:data/ef-rp/concurrency) for more information.</span></span>

### <a name="posting-and-binding-review"></a><span data-ttu-id="cb58d-145">Análise de postagem e associação</span><span class="sxs-lookup"><span data-stu-id="cb58d-145">Posting and binding review</span></span>

<span data-ttu-id="cb58d-146">Examine o arquivo *Pages/Movies/Edit.cshtml.cs*: [!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit.cshtml.cs?name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="cb58d-146">Examine the *Pages/Movies/Edit.cshtml.cs* file: [!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit.cshtml.cs?name=snippet2)]</span></span>

<span data-ttu-id="cb58d-147">Quando uma solicitação HTTP GET é feita para a página Movies/Edit (por exemplo, `http://localhost:5000/Movies/Edit/2`):</span><span class="sxs-lookup"><span data-stu-id="cb58d-147">When an HTTP GET request is made to the Movies/Edit page (for example, `http://localhost:5000/Movies/Edit/2`):</span></span>

* <span data-ttu-id="cb58d-148">O método `OnGetAsync` busca o filme do banco de dados e retorna o método `Page`.</span><span class="sxs-lookup"><span data-stu-id="cb58d-148">The `OnGetAsync` method fetches the movie from the database and returns the `Page` method.</span></span> 
* <span data-ttu-id="cb58d-149">O método `Page` renderiza a página Razor *Pages/Movies/Edit.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="cb58d-149">The `Page` method renders the *Pages/Movies/Edit.cshtml* Razor Page.</span></span> <span data-ttu-id="cb58d-150">O arquivo *Pages/Movies/Edit.cshtml* contém a diretiva de modelo (`@model RazorPagesMovie.Pages.Movies.EditModel`), que disponibiliza o modelo de filme na página.</span><span class="sxs-lookup"><span data-stu-id="cb58d-150">The *Pages/Movies/Edit.cshtml* file contains the model directive (`@model RazorPagesMovie.Pages.Movies.EditModel`), which makes the movie model available on the page.</span></span>
* <span data-ttu-id="cb58d-151">O formulário Editar é exibido com os valores do filme.</span><span class="sxs-lookup"><span data-stu-id="cb58d-151">The Edit form is displayed with the values from the movie.</span></span>

<span data-ttu-id="cb58d-152">Quando a página Movies/Edit é postada:</span><span class="sxs-lookup"><span data-stu-id="cb58d-152">When the Movies/Edit page is posted:</span></span>

* <span data-ttu-id="cb58d-153">Os valores de formulário na página são associados à propriedade `Movie`.</span><span class="sxs-lookup"><span data-stu-id="cb58d-153">The form values on the page are bound to the `Movie` property.</span></span> <span data-ttu-id="cb58d-154">O atributo `[BindProperty]` habilita a [Associação de modelos](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="cb58d-154">The `[BindProperty]` attribute enables [Model binding](xref:mvc/models/model-binding).</span></span>

  ```csharp
  [BindProperty]
  public Movie Movie { get; set; }
  ```

* <span data-ttu-id="cb58d-155">Se houver erros no estado do modelo (por exemplo, `ReleaseDate` não pode ser convertida em uma data), o formulário será postado novamente com os valores enviados.</span><span class="sxs-lookup"><span data-stu-id="cb58d-155">If there are errors in the model state (for example, `ReleaseDate` cannot be converted to a date), the form is posted again with the submitted values.</span></span>
* <span data-ttu-id="cb58d-156">Se não houver erros do modelo, o filme será salvo.</span><span class="sxs-lookup"><span data-stu-id="cb58d-156">If there are no model errors, the movie is saved.</span></span>

<span data-ttu-id="cb58d-157">Os métodos HTTP GET nas páginas Índice, Criar e Excluir do Razor seguem um padrão semelhante.</span><span class="sxs-lookup"><span data-stu-id="cb58d-157">The HTTP GET methods in the Index, Create, and Delete Razor pages follow a similar pattern.</span></span> <span data-ttu-id="cb58d-158">O método `OnPostAsync` HTTP POST na página Criar do Razor segue um padrão semelhante ao método `OnPostAsync` na página Editar do Razor.</span><span class="sxs-lookup"><span data-stu-id="cb58d-158">The HTTP POST `OnPostAsync` method in the Create Razor Page follows a similar pattern to the `OnPostAsync` method in the Edit Razor Page.</span></span>

<span data-ttu-id="cb58d-159">A pesquisa é adicionada no próximo tutorial.</span><span class="sxs-lookup"><span data-stu-id="cb58d-159">Search is added in the next tutorial.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="cb58d-160">[Anterior: Trabalhando com o SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[Adicionando uma pesquisa](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="cb58d-160">[Previous: Working with SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[Adding Search](xref:tutorials/razor-pages/search)</span></span>
