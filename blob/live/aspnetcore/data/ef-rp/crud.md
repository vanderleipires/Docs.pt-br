---
title: "Páginas Razor com núcleo EF - CRUD - 2 de 8"
author: rick-anderson
description: "Mostra como criar, ler, atualizar, excluir com núcleo de EF"
keywords: ASP.NET Core, Entity Framework Core, CRUD, criar, ler, atualizar, excluir
ms.author: riande
manager: wpickett
ms.date: 10/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/crud
ms.openlocfilehash: 246e6307989f2660d84288ceac6793c422875f93
ms.sourcegitcommit: 87168cdc409e7a7257f92a0f48f9c5ab320b5b28
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/17/2018
---
# <a name="create-read-update-and-delete---ef-core-with-razor-pages-2-of-8"></a><span data-ttu-id="d5cd4-104">Criar, ler, atualizar e excluir - Core EF com páginas Razor (2 de 8)</span><span class="sxs-lookup"><span data-stu-id="d5cd4-104">Create, Read, Update, and Delete - EF Core with Razor Pages (2 of 8)</span></span>

<span data-ttu-id="d5cd4-105">Por [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d5cd4-105">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="d5cd4-106">Neste tutorial, o scaffolding CRUD (criar, ler, atualizar e excluir) código é revisado e personalizado.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-106">In this tutorial, the scaffolded CRUD (create, read, update, delete) code is reviewed and customized.</span></span>

<span data-ttu-id="d5cd4-107">Observação: Para minimizar a complexidade e manter esses tutoriais que voltadas EF Core, código de EF principal é usado nos arquivos code-behind páginas Razor.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-107">Note: To minimize complexity and keep these tutorials focused on EF Core, EF Core code is used in the Razor Pages code-behind files.</span></span> <span data-ttu-id="d5cd4-108">Alguns desenvolvedores usam um padrão de repositório ou camada de serviço em para criar uma camada de abstração entre a interface do usuário (páginas Razor) e a camada de acesso a dados.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-108">Some developers use a service layer or repository pattern in to create an abstraction layer between the UI (Razor Pages) and the data access layer.</span></span>

<span data-ttu-id="d5cd4-109">Neste tutorial, criar, editar, excluir e páginas Razor detalhes de *aluno* pasta são modificadas.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-109">In this tutorial, the Create, Edit, Delete, and Details Razor Pages in the *Student* folder are modified.</span></span>

<span data-ttu-id="d5cd4-110">O código de scaffolding usa o padrão a seguir para criar, editar e excluir páginas:</span><span class="sxs-lookup"><span data-stu-id="d5cd4-110">The scaffolded code uses the following pattern for Create, Edit, and Delete pages:</span></span>

* <span data-ttu-id="d5cd4-111">Obter e exibir os dados solicitados com o método HTTP GET `OnGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-111">Get and display the requested data with the HTTP GET method `OnGetAsync`.</span></span>
* <span data-ttu-id="d5cd4-112">Salvar as alterações dos dados com o método HTTP POST `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-112">Save changes to the data with the HTTP POST method `OnPostAsync`.</span></span>

<span data-ttu-id="d5cd4-113">As páginas de índice e detalhes de obtém e exibem os dados solicitados com o método HTTP GET`OnGetAsync`</span><span class="sxs-lookup"><span data-stu-id="d5cd4-113">The Index and Details pages get and display the requested data with the HTTP GET method `OnGetAsync`</span></span>

## <a name="replace-singleordefaultasync-with-firstordefaultasync"></a><span data-ttu-id="d5cd4-114">Substitua SingleOrDefaultAsync FirstOrDefaultAsync</span><span class="sxs-lookup"><span data-stu-id="d5cd4-114">Replace SingleOrDefaultAsync with FirstOrDefaultAsync</span></span>

<span data-ttu-id="d5cd4-115">O código gerado usa [SingleOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) para buscar a entidade solicitada.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-115">The generated code uses [SingleOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_)  to fetch the requested entity.</span></span> <span data-ttu-id="d5cd4-116">[FirstOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_) é mais eficiente em busca de uma entidade:</span><span class="sxs-lookup"><span data-stu-id="d5cd4-116">[FirstOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_) is more efficient at fetching one entity:</span></span>

* <span data-ttu-id="d5cd4-117">A menos que o código precisa verificar se não há mais de uma entidade retornada da consulta.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-117">Unless the code needs to verify that there is not more than one entity returned from the query.</span></span> 
* <span data-ttu-id="d5cd4-118">`SingleOrDefaultAsync`busca mais dados e funciona o desnecessária.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-118">`SingleOrDefaultAsync` fetches more data and does unnecessary work.</span></span>
* <span data-ttu-id="d5cd4-119">`SingleOrDefaultAsync`gera uma exceção se houver mais de uma entidade que se ajusta a parte do filtro.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-119">`SingleOrDefaultAsync` throws an exception if there is more than one entity that fits the filter part.</span></span>
*  <span data-ttu-id="d5cd4-120">`FirstOrDefaultAsync`não gerar se houver mais de uma entidade que se ajusta a parte do filtro.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-120">`FirstOrDefaultAsync` doesn't throw if there is more than one entity that fits the filter part.</span></span>

<span data-ttu-id="d5cd4-121">Substituir globalmente `SingleOrDefaultAsync` com `FirstOrDefaultAsync`.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-121">Globally replace `SingleOrDefaultAsync` with `FirstOrDefaultAsync`.</span></span> <span data-ttu-id="d5cd4-122">`SingleOrDefaultAsync`é usado em 5 locais:</span><span class="sxs-lookup"><span data-stu-id="d5cd4-122">`SingleOrDefaultAsync` is used in 5 places:</span></span>

* <span data-ttu-id="d5cd4-123">`OnGetAsync`Na página de detalhes.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-123">`OnGetAsync` in the Details page.</span></span>
* <span data-ttu-id="d5cd4-124">`OnGetAsync`e `OnPostAsync` nas páginas de editar e excluir.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-124">`OnGetAsync` and `OnPostAsync` in the Edit and Delete pages.</span></span>

<a name="FindAsync"></a>
### <a name="findasync"></a><span data-ttu-id="d5cd4-125">FindAsync</span><span class="sxs-lookup"><span data-stu-id="d5cd4-125">FindAsync</span></span>

<span data-ttu-id="d5cd4-126">Em grande parte do código scaffolding, [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) pode ser usado no lugar de `FirstOrDefaultAsync` ou `SingleOrDefaultAsync`.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-126">In much of the scaffolded code, [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) can be used in place of `FirstOrDefaultAsync` or `SingleOrDefaultAsync`.</span></span> 

<span data-ttu-id="d5cd4-127">`FindAsync`:</span><span class="sxs-lookup"><span data-stu-id="d5cd4-127">`FindAsync`:</span></span>

* <span data-ttu-id="d5cd4-128">Localiza uma entidade com a chave primária (PK).</span><span class="sxs-lookup"><span data-stu-id="d5cd4-128">Finds an entity with the primary key (PK).</span></span> <span data-ttu-id="d5cd4-129">Se uma entidade com o PK está sendo controlada pelo contexto, ele é retornado sem uma solicitação para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-129">If an entity with the PK is being tracked by the context, it is returned without a request to the DB.</span></span>
* <span data-ttu-id="d5cd4-130">É simples e conciso.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-130">Is simple and concise.</span></span>
* <span data-ttu-id="d5cd4-131">É otimizado para pesquisar uma única entidade.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-131">Is optimized to look up a single entity.</span></span>
* <span data-ttu-id="d5cd4-132">Pode ter benefícios de desempenho em algumas situações, mas eles raramente entram em jogo para cenários web normal.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-132">Can have perf benefits in some situations, but they rarely come into play for normal web scenarios.</span></span>
* <span data-ttu-id="d5cd4-133">Usa implicitamente [FirstAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) em vez de [SingleAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span><span class="sxs-lookup"><span data-stu-id="d5cd4-133">Implicitly uses [FirstAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) instead of [SingleAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span></span>
<span data-ttu-id="d5cd4-134">Mas se você deseja incluir outras entidades, encontrar não é mais apropriado.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-134">But if you want to Include other entities, then Find is no longer appropriate.</span></span> <span data-ttu-id="d5cd4-135">Isso significa que talvez seja necessário abandoná-localizar e mover para uma consulta à medida que avança seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-135">This means that you may need to abandon Find and move to a query as your app progresses.</span></span>

## <a name="customize-the-details-page"></a><span data-ttu-id="d5cd4-136">Personalizar a página de detalhes</span><span class="sxs-lookup"><span data-stu-id="d5cd4-136">Customize the Details page</span></span>

<span data-ttu-id="d5cd4-137">Navegue até `Pages/Students` página.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-137">Browse to `Pages/Students` page.</span></span> <span data-ttu-id="d5cd4-138">O **editar**, **detalhes**, e **excluir** links são gerados pelo [auxiliar de marca de âncora](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) no *páginas/alunos / Cshtml* arquivo.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-138">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Students/Index.cshtml* file.</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Students/Index1.cshtml?range=40-44)]

<span data-ttu-id="d5cd4-139">Selecione um link de detalhes.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-139">Select a Details link.</span></span> <span data-ttu-id="d5cd4-140">É a URL do formulário `http://localhost:5000/Students/Details?id=2`.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-140">The URL is of the form `http://localhost:5000/Students/Details?id=2`.</span></span> <span data-ttu-id="d5cd4-141">A identificação do aluno é passada usando uma cadeia de caracteres de consulta (`?id=2`).</span><span class="sxs-lookup"><span data-stu-id="d5cd4-141">The Student ID is passed using a query string (`?id=2`).</span></span>

<span data-ttu-id="d5cd4-142">Atualizar a edição, os detalhes e excluir páginas do Razor para usar o `"{id:int}"` modelo de rota.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-142">Update the Edit, Details, and Delete Razor Pages to use the `"{id:int}"` route template.</span></span> <span data-ttu-id="d5cd4-143">Altere a diretiva de página de cada uma dessas páginas de `@page` para `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-143">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span>

<span data-ttu-id="d5cd4-144">Uma solicitação para a página com o modelo de rota "{id: int}" que faz **não** incluem um número inteiro rota valor retorna um HTTP 404 (não encontrado) erro.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-144">A request to the page with the "{id:int}" route template that does **not** include a integer route value returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="d5cd4-145">Por exemplo, `http://localhost:5000/Students/Details` retornará um erro 404.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-145">For example, `http://localhost:5000/Students/Details` returns a 404 error.</span></span> <span data-ttu-id="d5cd4-146">Para tornar a ID opcional, acrescente `?` à restrição de rota:</span><span class="sxs-lookup"><span data-stu-id="d5cd4-146">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="d5cd4-147">Executar o aplicativo, clique em um link de detalhes e verifique se a URL está passando a ID como dados de rota (`http://localhost:5000/Students/Details/2`).</span><span class="sxs-lookup"><span data-stu-id="d5cd4-147">Run the app, click on a Details link, and verify the URL is passing the ID as route data (`http://localhost:5000/Students/Details/2`).</span></span>

<span data-ttu-id="d5cd4-148">Não altere globalmente `@page` para `@page "{id:int}"`, fazendo interrompe os links para a página inicial e criar páginas.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-148">Don't globally change `@page` to `@page "{id:int}"`, doing so breaks the links to the Home and Create pages.</span></span>

<!-- See https://github.com/aspnet/Scaffolding/issues/590 -->

### <a name="add-related-data"></a><span data-ttu-id="d5cd4-149">Adicionar dados relacionados</span><span class="sxs-lookup"><span data-stu-id="d5cd4-149">Add related data</span></span>

<span data-ttu-id="d5cd4-150">O código de scaffolding para a página de índice de alunos não inclui o `Enrollments` propriedade.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-150">The scaffolded code for the Students Index page does not include the `Enrollments` property.</span></span> <span data-ttu-id="d5cd4-151">Nesta seção, o conteúdo do `Enrollments` coleção é exibida na página de detalhes.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-151">In this section, the contents of the `Enrollments` collection is displayed in the Details page.</span></span>

<span data-ttu-id="d5cd4-152">O `OnGetAsync` método *Pages/Students/Details.cshtml.cs* usa o `FirstOrDefaultAsync` método para recuperar um único `Student` entidade.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-152">The `OnGetAsync` method of *Pages/Students/Details.cshtml.cs* uses the `FirstOrDefaultAsync` method to retrieve a single `Student` entity.</span></span> <span data-ttu-id="d5cd4-153">Adicione o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="d5cd4-153">Add the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Details.cshtml.cs?name=snippet_Details&highlight=8-12)]

<span data-ttu-id="d5cd4-154">O `Include` e `ThenInclude` métodos fazer com que o contexto para carregar o `Student.Enrollments` propriedade de navegação e dentro de cada registro de `Enrollment.Course` propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-154">The `Include` and `ThenInclude` methods cause the context to load the `Student.Enrollments` navigation property, and within each enrollment the `Enrollment.Course` navigation property.</span></span> <span data-ttu-id="d5cd4-155">Esses métodos são examinied em detalhes no tutorial relacionadas à leitura de dados.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-155">These methods are examinied in detail in the reading-related data tutorial.</span></span>

<span data-ttu-id="d5cd4-156">O `AsNoTracking` método melhora o desempenho em cenários quando as entidades retornadas não são atualizadas no contexto atual.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-156">The `AsNoTracking` method improves performance in scenarios when the entities returned are not updated in the current context.</span></span> <span data-ttu-id="d5cd4-157">`AsNoTracking`é abordada posteriormente neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-157">`AsNoTracking` is discussed later in this tutorial.</span></span>

### <a name="display-related-enrollments-on-the-details-page"></a><span data-ttu-id="d5cd4-158">Exibir registros relacionados na página de detalhes</span><span class="sxs-lookup"><span data-stu-id="d5cd4-158">Display related enrollments on the Details page</span></span>

<span data-ttu-id="d5cd4-159">Abra *Pages/Students/Details.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-159">Open *Pages/Students/Details.cshtml*.</span></span> <span data-ttu-id="d5cd4-160">Adicione o seguinte código realçado para exibir uma lista de registros:</span><span class="sxs-lookup"><span data-stu-id="d5cd4-160">Add the following highlighted code to display a list of enrollments:</span></span>

 <!--2do ricka. if doesn't change, remove dup -->
[!code-cshtml[Main](intro/samples/cu/Pages/Students/Details1.cshtml?highlight=32-53)]

<span data-ttu-id="d5cd4-161">Se o recuo do código está errado depois que o código será colado, pressione CTRL-K-D para corrigi-lo.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-161">If code indentation is wrong after the code is pasted, press CTRL-K-D to correct it.</span></span>

<span data-ttu-id="d5cd4-162">O código anterior percorre as entidades de `Enrollments` propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-162">The preceding code loops through the entities in the `Enrollments` navigation property.</span></span> <span data-ttu-id="d5cd4-163">Para cada registro, ele exibe o nome do curso e a classificação.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-163">For each enrollment, it displays the course title and the grade.</span></span> <span data-ttu-id="d5cd4-164">O título do curso é recuperado da entidade curso que é armazenada no `Course` propriedade de navegação de entidade de registros.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-164">The course title is retrieved from the Course entity that's stored in the `Course` navigation property of the Enrollments entity.</span></span>

<span data-ttu-id="d5cd4-165">Executar o aplicativo, selecione o **alunos** guia e, em seguida, clique no **detalhes** link para um aluno.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-165">Run the app, select the **Students** tab, and click the **Details** link for a student.</span></span> <span data-ttu-id="d5cd4-166">A lista de cursos e notas do aluno selecionado é exibida.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-166">The list of courses and grades for the selected student is displayed.</span></span>

## <a name="update-the-create-page"></a><span data-ttu-id="d5cd4-167">Atualizar a página de criação</span><span class="sxs-lookup"><span data-stu-id="d5cd4-167">Update the Create page</span></span>

<span data-ttu-id="d5cd4-168">Atualização de `OnPostAsync` método *Pages/Students/Create.cshtml.cs* com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="d5cd4-168">Update the `OnPostAsync` method in *Pages/Students/Create.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>
### <a name="tryupdatemodelasync"></a><span data-ttu-id="d5cd4-169">TryUpdateModelAsync</span><span class="sxs-lookup"><span data-stu-id="d5cd4-169">TryUpdateModelAsync</span></span>

<span data-ttu-id="d5cd4-170">Examine o [TryUpdateModelAsync](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) código:</span><span class="sxs-lookup"><span data-stu-id="d5cd4-170">Examine the [TryUpdateModelAsync](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_TryUpdateModelAsync)]

<span data-ttu-id="d5cd4-171">No código anterior, `TryUpdateModelAsync<Student>` tenta atualizar o `emptyStudent` objeto usando os valores de formulário postado do [conteúdo de página](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) propriedade o [PageModel](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="d5cd4-171">In the preceding code, `TryUpdateModelAsync<Student>` tries to update the `emptyStudent` object using the posted form values from the [PageContext](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) property in the [PageModel](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel?view=aspnetcore-2.0).</span></span> <span data-ttu-id="d5cd4-172">`TryUpdateModelAsync`atualiza apenas as propriedades listadas (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).</span><span class="sxs-lookup"><span data-stu-id="d5cd4-172">`TryUpdateModelAsync` only updates the properties listed (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).</span></span>

<span data-ttu-id="d5cd4-173">No exemplo anterior:</span><span class="sxs-lookup"><span data-stu-id="d5cd4-173">In the preceding sample:</span></span>

* <span data-ttu-id="d5cd4-174">O segundo argumento (` "student", // Prefix`) é o prefixo usa para procurar valores.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-174">The second argument (` "student", // Prefix`) is the prefix uses to look up values.</span></span> <span data-ttu-id="d5cd4-175">Não diferencia maiusculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-175">It's not case sensitive.</span></span>
* <span data-ttu-id="d5cd4-176">Os valores de formulário postado são convertidos para tipos no `Student` modelo usando [associação de modelo](xref:mvc/models/model-binding#how-model-binding-works).</span><span class="sxs-lookup"><span data-stu-id="d5cd4-176">The posted form values are converted to the types in the `Student` model using [model binding](xref:mvc/models/model-binding#how-model-binding-works).</span></span>

<a id="overpost"></a>
### <a name="overposting"></a><span data-ttu-id="d5cd4-177">Overposting</span><span class="sxs-lookup"><span data-stu-id="d5cd4-177">Overposting</span></span>

<span data-ttu-id="d5cd4-178">Usando `TryUpdateModel` atualizar os campos com valores postados é uma prática recomendada de segurança porque evita que overposting.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-178">Using `TryUpdateModel` to update fields with posted values is a security best practice because it prevents overposting.</span></span> <span data-ttu-id="d5cd4-179">Por exemplo, suponha que a entidade do aluno inclui um `Secret` que esta página da web não deve atualizar ou adicionar:</span><span class="sxs-lookup"><span data-stu-id="d5cd4-179">For example, suppose the Student entity includes a `Secret` property that this web page should not update or add:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

<span data-ttu-id="d5cd4-180">Mesmo que o aplicativo não tiver um `Secret` campo ao criar/atualizar Razor de página, um hacker pode definir o `Secret` valor por mais.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-180">Even if the app doesn't have a `Secret` field on the create/update Razor Page, a hacker could set the `Secret` value by overposting.</span></span> <span data-ttu-id="d5cd4-181">Um hacker pode usar uma ferramenta como o Fiddler ou escrever alguns JavaScript, postar um `Secret` formar o valor.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-181">A hacker could use a tool such as Fiddler, or write some JavaScript, to post a `Secret` form value.</span></span> <span data-ttu-id="d5cd4-182">O código original não limita os campos que o associador de modelo usa quando ele cria uma instância do aluno.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-182">The original code doesn't limit the fields that the model binder uses when it creates a Student instance.</span></span>

<span data-ttu-id="d5cd4-183">O valor que o hacker especificado para o `Secret` campo de formulário é atualizado no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-183">Whatever value the hacker specified for the `Secret` form field is updated in the DB.</span></span> <span data-ttu-id="d5cd4-184">A imagem a seguir mostra a adição de ferramenta Fiddler o `Secret` campo (com o valor "OverPost") para os valores de formulário postado.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-184">The following image shows the Fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.</span></span>

![Campo secreto adição do Fiddler](../ef-mvc/crud/_static/fiddler.png)

<span data-ttu-id="d5cd4-186">O valor "OverPost" foi adicionado com êxito para o `Secret` propriedade da linha inserida.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-186">The value "OverPost" is successfully added to the `Secret` property of the inserted row.</span></span> <span data-ttu-id="d5cd4-187">O designer do aplicativo nunca se destina a `Secret` propriedade a ser definida com a página de criação.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-187">The app designer never intended the `Secret` property to be set with the Create page.</span></span>

<a name="vm"></a>
### <a name="view-model"></a><span data-ttu-id="d5cd4-188">Modelo de exibição</span><span class="sxs-lookup"><span data-stu-id="d5cd4-188">View model</span></span>

<span data-ttu-id="d5cd4-189">Um modelo de exibição normalmente contém um subconjunto das propriedades incluídas no modelo usado pelo aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-189">A view model typically contains a subset of the properties included in the model used by the application.</span></span> <span data-ttu-id="d5cd4-190">O modelo de aplicativo é geralmente chamado de modelo de domínio.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-190">The application model is often called the domain model.</span></span> <span data-ttu-id="d5cd4-191">O modelo de domínio normalmente contém todas as propriedades necessárias pela entidade correspondente no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-191">The domain model typically contains all the properties required by the corresponding entity in the DB.</span></span> <span data-ttu-id="d5cd4-192">O modelo de exibição contém apenas as propriedades necessárias para a camada de interface do usuário (por exemplo, a página Criar).</span><span class="sxs-lookup"><span data-stu-id="d5cd4-192">The view model contains only the properties needed for the UI layer (for example, the Create page).</span></span> <span data-ttu-id="d5cd4-193">Além do modelo de exibição, alguns aplicativos usam um modelo de associação ou o modelo de entrada para passar dados entre a classe code-behind páginas Razor e o navegador.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-193">In addition to the view model, some apps use a binding model or input model to pass data between the Razor Pages code-behind class and the browser.</span></span> <span data-ttu-id="d5cd4-194">Considere o seguinte `Student` modelo de exibição:</span><span class="sxs-lookup"><span data-stu-id="d5cd4-194">Consider the following `Student` view model:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/StudentVM.cs)]

<span data-ttu-id="d5cd4-195">Exibir modelos fornecem uma maneira alternativa para evitar overposting.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-195">View models provide an alternative way to prevent overposting.</span></span> <span data-ttu-id="d5cd4-196">O modelo de exibição contém apenas as propriedades para exibir (exibição) ou atualizar.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-196">The view model contains only the properties to view (display) or update.</span></span>

<span data-ttu-id="d5cd4-197">O código a seguir usa o `StudentVM` modelo de exibição para criar um novo aluno:</span><span class="sxs-lookup"><span data-stu-id="d5cd4-197">The following code uses the `StudentVM` view model to create a new student:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="d5cd4-198">O [SetValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) método define os valores do objeto lendo os valores de outro [PropertyValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) objeto.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-198">The [SetValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) method sets the values of this object by reading values from another [PropertyValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) object.</span></span> <span data-ttu-id="d5cd4-199">`SetValues`usa a correspondência de nomes de propriedade.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-199">`SetValues` uses property name matching.</span></span> <span data-ttu-id="d5cd4-200">O tipo de modelo de exibição não precisa estar relacionado ao tipo de modelo, ele só precisa ter as propriedades que correspondem.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-200">The view model type doesn't need to be related to the model type, it just needs to have properties that match.</span></span>

<span data-ttu-id="d5cd4-201">Usando `StudentVM` requer [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Students/CreateVM.cshtml) ser atualizado para usar `StudentVM` em vez de `Student`.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-201">Using `StudentVM` requires [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Students/CreateVM.cshtml) be updated to use `StudentVM` rather than `Student`.</span></span>

<span data-ttu-id="d5cd4-202">Nas páginas Razor, o `PageModel` classe derivada é o modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-202">In Razor Pages, the `PageModel` derived class is the view model.</span></span> 

## <a name="update-the-edit-page"></a><span data-ttu-id="d5cd4-203">Atualizar a página de edição</span><span class="sxs-lookup"><span data-stu-id="d5cd4-203">Update the Edit page</span></span>

<span data-ttu-id="d5cd4-204">Atualize o arquivo de code-behind de página de edição:</span><span class="sxs-lookup"><span data-stu-id="d5cd4-204">Update the Edit page code-behind file:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Edit.cshtml.cs?name=snippet_OnPostAsync&highlight=20,36)]

<span data-ttu-id="d5cd4-205">As alterações de código são semelhantes a página de criação, com algumas exceções:</span><span class="sxs-lookup"><span data-stu-id="d5cd4-205">The code changes are similar to the Create page with a few exceptions:</span></span>

* <span data-ttu-id="d5cd4-206">`OnPostAsync`tem um `id` parâmetro.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-206">`OnPostAsync` has an optional `id` parameter.</span></span>
* <span data-ttu-id="d5cd4-207">O aluno atual é obtido do banco de dados, em vez de criar um aluno vazio.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-207">The current student is fetched from the DB, rather than creating an empty student.</span></span>
* <span data-ttu-id="d5cd4-208">`FirstOrDefaultAsync`foi substituído por [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync?view=efcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="d5cd4-208">`FirstOrDefaultAsync` has been replaced with [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync?view=efcore-2.0).</span></span> <span data-ttu-id="d5cd4-209">`FindAsync`é uma boa opção, ao selecionar uma entidade da chave primária.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-209">`FindAsync` is a good choice when selecting an entity from the primary key.</span></span> <span data-ttu-id="d5cd4-210">Consulte [FindAsync](#FindAsync) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-210">See [FindAsync](#FindAsync) for more information.</span></span>

### <a name="test-the-edit-and-create-pages"></a><span data-ttu-id="d5cd4-211">A edição de teste e criar páginas</span><span class="sxs-lookup"><span data-stu-id="d5cd4-211">Test the Edit and Create pages</span></span>

<span data-ttu-id="d5cd4-212">Criar e editar algumas entidades aluno.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-212">Create and edit a few student entities.</span></span>

## <a name="entity-states"></a><span data-ttu-id="d5cd4-213">Estados da entidade</span><span class="sxs-lookup"><span data-stu-id="d5cd4-213">Entity States</span></span>

<span data-ttu-id="d5cd4-214">Mantém o controle de contexto de banco de dados se entidades na memória estão em sincronia com suas linhas correspondentes no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-214">The DB context keeps track of whether entities in memory are in sync with their corresponding rows in the DB.</span></span> <span data-ttu-id="d5cd4-215">As informações de sincronização de contexto do banco de dados determinam o que acontece quando `SaveChanges` é chamado.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-215">The DB context sync information determines what happens when `SaveChanges` is called.</span></span> <span data-ttu-id="d5cd4-216">Por exemplo, quando uma nova entidade é passada para o `Add` método, o estado da entidade definida como `Added`.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-216">For example, when a new entity is passed to the `Add` method, that entity's state is set to `Added`.</span></span> <span data-ttu-id="d5cd4-217">Quando `SaveChanges` é chamado, o banco de dados contexto emite um comando INSERT SQL.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-217">When `SaveChanges` is called, the DB context issues a SQL INSERT command.</span></span>

<span data-ttu-id="d5cd4-218">Uma entidade pode estar em um dos seguintes estados:</span><span class="sxs-lookup"><span data-stu-id="d5cd4-218">An entity may be in one of the following states:</span></span>

* <span data-ttu-id="d5cd4-219">`Added`: A entidade ainda não existir no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-219">`Added`: The entity does not yet exist in the DB.</span></span> <span data-ttu-id="d5cd4-220">O `SaveChanges` método emite uma instrução INSERT.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-220">The `SaveChanges` method issues an INSERT statement.</span></span>

* <span data-ttu-id="d5cd4-221">`Unchanged`: Nenhuma alteração precisa ser salva com essa entidade.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-221">`Unchanged`: No changes need to be saved with this entity.</span></span> <span data-ttu-id="d5cd4-222">Uma entidade possui esse status quando ele é lido a partir do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-222">An entity has this status when it is read from the DB.</span></span>

* <span data-ttu-id="d5cd4-223">`Modified`: Foram modificados alguns ou todos os valores de propriedade da entidade.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-223">`Modified`: Some or all of the entity's property values have been modified.</span></span> <span data-ttu-id="d5cd4-224">O `SaveChanges` método emite uma instrução UPDATE.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-224">The `SaveChanges` method issues an UPDATE statement.</span></span>

* <span data-ttu-id="d5cd4-225">`Deleted`: A entidade foi marcada para exclusão.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-225">`Deleted`: The entity has been marked for deletion.</span></span> <span data-ttu-id="d5cd4-226">O `SaveChanges` método emite uma instrução DELETE.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-226">The `SaveChanges` method issues a DELETE statement.</span></span>

* <span data-ttu-id="d5cd4-227">`Detached`: A entidade não está sendo controlada pelo contexto de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-227">`Detached`: The entity isn't being tracked by the DB context.</span></span>

<span data-ttu-id="d5cd4-228">Em um aplicativo de área de trabalho, as alterações de estado geralmente são definidas automaticamente.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-228">In a desktop app, state changes are typically set automatically.</span></span> <span data-ttu-id="d5cd4-229">Uma entidade é leitura, as alterações são feitas e o estado da entidade a ser alterada automaticamente para `Modified`.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-229">An entity is read, changes are made, and the entity state to automatically be changed to `Modified`.</span></span> <span data-ttu-id="d5cd4-230">Chamando `SaveChanges` gera uma instrução de atualização do SQL que atualiza apenas as propriedades alteradas.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-230">Calling `SaveChanges` generates a SQL UPDATE statement that updates only the changed properties.</span></span>

<span data-ttu-id="d5cd4-231">Em um aplicativo web, o `DbContext` que lê uma entidade e exibe os dados for descartados depois que uma página é processada.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-231">In a web app, the `DbContext` that reads an entity and displays the data is disposed after a page is rendered.</span></span> <span data-ttu-id="d5cd4-232">Quando um páginas `OnPostAsync` método é chamado, é feita uma nova solicitação de web e com uma nova instância do `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-232">When a pages `OnPostAsync` method is called, a new web request is made and with a new instance of the `DbContext`.</span></span> <span data-ttu-id="d5cd4-233">Ler novamente a entidade no novo contexto simula o processamento de área de trabalho.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-233">Re-reading the entity in that new context simulates desktop processing.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="d5cd4-234">Atualizar a página de exclusão</span><span class="sxs-lookup"><span data-stu-id="d5cd4-234">Update the Delete page</span></span>

<span data-ttu-id="d5cd4-235">Nesta seção, o código é adicionado para implementar um erro personalizado de mensagem quando a chamada para `SaveChanges` falhar.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-235">In this section, code is added to implement a custom error message when the call to `SaveChanges` fails.</span></span> <span data-ttu-id="d5cd4-236">Adicione uma cadeia de caracteres para conter as possíveis mensagens de erro:</span><span class="sxs-lookup"><span data-stu-id="d5cd4-236">Add a string to contain possible error messages:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet1&highlight=12)]

<span data-ttu-id="d5cd4-237">Substitua o método `OnGetAsync` pelo seguinte código:</span><span class="sxs-lookup"><span data-stu-id="d5cd4-237">Replace the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnGetAsync&highlight=1,9,17-20)]

<span data-ttu-id="d5cd4-238">O código anterior contém o parâmetro opcional `saveChangesError`.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-238">The preceding code contains the optional parameter `saveChangesError`.</span></span> <span data-ttu-id="d5cd4-239">`saveChangesError`Indica se o método foi chamado após uma falha ao excluir o objeto do aluno.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-239">`saveChangesError` indicates whether the method was called after a failure to delete the student object.</span></span> <span data-ttu-id="d5cd4-240">A operação de exclusão pode falhar devido a problemas de rede temporários.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-240">The delete operation might fail because of transient network problems.</span></span> <span data-ttu-id="d5cd4-241">Erros de rede transitório serão mais prováveis na nuvem.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-241">Transient network errors are more likely in the cloud.</span></span> <span data-ttu-id="d5cd4-242">`saveChangesError`é falso quando a página de exclusão `OnGetAsync` é chamado a partir da interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-242">`saveChangesError`is false when the Delete page `OnGetAsync` is called from the UI.</span></span> <span data-ttu-id="d5cd4-243">Quando `OnGetAsync` é chamado pelo `OnPostAsync` (porque a operação de exclusão falhou), o `saveChangesError` parâmetro for true.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-243">When `OnGetAsync` is called by `OnPostAsync` (because the delete operation failed), the `saveChangesError` parameter is true.</span></span>

### <a name="the-delete-pages-onpostasync-method"></a><span data-ttu-id="d5cd4-244">O método de OnPostAsync de páginas de exclusão</span><span class="sxs-lookup"><span data-stu-id="d5cd4-244">The Delete pages OnPostAsync method</span></span>

<span data-ttu-id="d5cd4-245">Substitua o `OnPostAsync` com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="d5cd4-245">Replace the `OnPostAsync` with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="d5cd4-246">O código anterior recupera a entidade selecionada, em seguida, chama o `Remove` método para definir o status da entidade `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-246">The preceding code retrieves the selected entity, then calls the `Remove` method to set the entity's status to `Deleted`.</span></span> <span data-ttu-id="d5cd4-247">Quando `SaveChanges` é chamado, um SQL DELETE comando é gerado.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-247">When `SaveChanges` is called, a SQL DELETE command is generated.</span></span> <span data-ttu-id="d5cd4-248">Se `Remove` falhar:</span><span class="sxs-lookup"><span data-stu-id="d5cd4-248">If `Remove` fails:</span></span>

* <span data-ttu-id="d5cd4-249">A exceção de banco de dados é detectada.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-249">The DB exception is caught.</span></span>
* <span data-ttu-id="d5cd4-250">As páginas de exclusão `OnGetAsync` método for chamado com `saveChangesError=true`.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-250">The Delete pages `OnGetAsync` method is called with `saveChangesError=true`.</span></span>

### <a name="update-the-delete-razor-page"></a><span data-ttu-id="d5cd4-251">Atualizar a página de exclusão Razor</span><span class="sxs-lookup"><span data-stu-id="d5cd4-251">Update the Delete Razor Page</span></span>

<span data-ttu-id="d5cd4-252">Adicione a seguinte mensagem de erro realçado para excluir a página de Razor.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-252">Add the following highlighted error message to the Delete Razor Page.</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Students/Delete.cshtml?range=1-13&highlight=10)]

<span data-ttu-id="d5cd4-253">Exclusão de teste.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-253">Test Delete.</span></span>

## <a name="common-errors"></a><span data-ttu-id="d5cd4-254">Erros comuns</span><span class="sxs-lookup"><span data-stu-id="d5cd4-254">Common errors</span></span>

<span data-ttu-id="d5cd4-255">Aluno/Home ou outros links não funcionam:</span><span class="sxs-lookup"><span data-stu-id="d5cd4-255">Student/Home or other links don't work:</span></span>

<span data-ttu-id="d5cd4-256">Verifique se a página Razor contém corretas `@page` diretiva.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-256">Verify the Razor Page contains the correct `@page` directive.</span></span> <span data-ttu-id="d5cd4-257">Por exemplo, o aluno/Home Page de Razor deve **não** contêm um modelo de rota:</span><span class="sxs-lookup"><span data-stu-id="d5cd4-257">For example, The Student/Home Razor Page should **not** contain a route template:</span></span>

```cshtml
@page "{id:int}"
```

<span data-ttu-id="d5cd4-258">Cada página do Razor deve incluir a `@page` diretiva.</span><span class="sxs-lookup"><span data-stu-id="d5cd4-258">Each Razor Page must include the `@page` directive.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="d5cd4-259">[Anterior](xref:data/ef-rp/intro)
[Próximo](xref:data/ef-rp/sort-filter-page)</span><span class="sxs-lookup"><span data-stu-id="d5cd4-259">[Previous](xref:data/ef-rp/intro)
[Next](xref:data/ef-rp/sort-filter-page)</span></span>
