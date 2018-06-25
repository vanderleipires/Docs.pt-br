---
title: Páginas Razor com o EF Core no ASP.NET Core – CRUD – 2 de 8
author: rick-anderson
description: Mostra como criar, ler, atualizar e excluir com o EF Core
ms.author: riande
ms.date: 10/15/2017
uid: data/ef-rp/crud
ms.openlocfilehash: 17d48cae50745508a64a9fb8a153b7b891e64a23
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278681"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---crud---2-of-8"></a><span data-ttu-id="154f5-103">Páginas Razor com o EF Core no ASP.NET Core – CRUD – 2 de 8</span><span class="sxs-lookup"><span data-stu-id="154f5-103">Razor Pages with EF Core in ASP.NET Core - CRUD - 2 of 8</span></span>

<span data-ttu-id="154f5-104">Por [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="154f5-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="154f5-105">Neste tutorial, o código CRUD (criar, ler, atualizar e excluir) gerado por scaffolding é examinado e personalizado.</span><span class="sxs-lookup"><span data-stu-id="154f5-105">In this tutorial, the scaffolded CRUD (create, read, update, delete) code is reviewed and customized.</span></span>

<span data-ttu-id="154f5-106">Observação: para minimizar a complexidade e manter o foco destes tutoriais no EF Core, o código do EF Core é usado nos modelos de página das Páginas do Razor.</span><span class="sxs-lookup"><span data-stu-id="154f5-106">Note: To minimize complexity and keep these tutorials focused on EF Core, EF Core code is used in the Razor Pages page models.</span></span> <span data-ttu-id="154f5-107">Alguns desenvolvedores usam um padrão de repositório ou de camada de serviço para criar uma camada de abstração entre a interface do usuário (Páginas do Razor) e a camada de acesso a dados.</span><span class="sxs-lookup"><span data-stu-id="154f5-107">Some developers use a service layer or repository pattern in to create an abstraction layer between the UI (Razor Pages) and the data access layer.</span></span>

<span data-ttu-id="154f5-108">Neste tutorial, as Páginas Criar, Editar, Excluir e Detalhes do Razor na pasta *Student* são modificadas.</span><span class="sxs-lookup"><span data-stu-id="154f5-108">In this tutorial, the Create, Edit, Delete, and Details Razor Pages in the *Student* folder are modified.</span></span>

<span data-ttu-id="154f5-109">O código gerado por scaffolding usa o seguinte padrão para as páginas Criar, Editar e Excluir:</span><span class="sxs-lookup"><span data-stu-id="154f5-109">The scaffolded code uses the following pattern for Create, Edit, and Delete pages:</span></span>

* <span data-ttu-id="154f5-110">Obtenha e exiba os dados solicitados com o método HTTP GET `OnGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="154f5-110">Get and display the requested data with the HTTP GET method `OnGetAsync`.</span></span>
* <span data-ttu-id="154f5-111">Salve as alterações nos dados com o método HTTP POST `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="154f5-111">Save changes to the data with the HTTP POST method `OnPostAsync`.</span></span>

<span data-ttu-id="154f5-112">As páginas Índice e Detalhes obtêm e exibem os dados solicitados com o método HTTP GET `OnGetAsync`</span><span class="sxs-lookup"><span data-stu-id="154f5-112">The Index and Details pages get and display the requested data with the HTTP GET method `OnGetAsync`</span></span>

## <a name="replace-singleordefaultasync-with-firstordefaultasync"></a><span data-ttu-id="154f5-113">Substitua SingleOrDefaultAsync com FirstOrDefaultAsync</span><span class="sxs-lookup"><span data-stu-id="154f5-113">Replace SingleOrDefaultAsync with FirstOrDefaultAsync</span></span>

<span data-ttu-id="154f5-114">O código gerado usa [SingleOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) para buscar a entidade solicitada.</span><span class="sxs-lookup"><span data-stu-id="154f5-114">The generated code uses [SingleOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_)  to fetch the requested entity.</span></span> <span data-ttu-id="154f5-115">[FirstOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_) é mais eficiente na busca de uma entidade:</span><span class="sxs-lookup"><span data-stu-id="154f5-115">[FirstOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_) is more efficient at fetching one entity:</span></span>

* <span data-ttu-id="154f5-116">A menos que o código precise verificar se não há mais de uma entidade retornada da consulta.</span><span class="sxs-lookup"><span data-stu-id="154f5-116">Unless the code needs to verify that there's not more than one entity returned from the query.</span></span> 
* <span data-ttu-id="154f5-117">`SingleOrDefaultAsync` busca mais dados e faz o trabalho desnecessário.</span><span class="sxs-lookup"><span data-stu-id="154f5-117">`SingleOrDefaultAsync` fetches more data and does unnecessary work.</span></span>
* <span data-ttu-id="154f5-118">`SingleOrDefaultAsync` gera uma exceção se há mais de uma entidade que se ajusta à parte do filtro.</span><span class="sxs-lookup"><span data-stu-id="154f5-118">`SingleOrDefaultAsync` throws an exception if there's more than one entity that fits the filter part.</span></span>
*  <span data-ttu-id="154f5-119">`FirstOrDefaultAsync` não gera uma exceção se há mais de uma entidade que se ajusta à parte do filtro.</span><span class="sxs-lookup"><span data-stu-id="154f5-119">`FirstOrDefaultAsync` doesn't throw if there's more than one entity that fits the filter part.</span></span>

<span data-ttu-id="154f5-120">Substitua `SingleOrDefaultAsync` globalmente com `FirstOrDefaultAsync`.</span><span class="sxs-lookup"><span data-stu-id="154f5-120">Globally replace `SingleOrDefaultAsync` with `FirstOrDefaultAsync`.</span></span> <span data-ttu-id="154f5-121">`SingleOrDefaultAsync` é usado em cinco locais:</span><span class="sxs-lookup"><span data-stu-id="154f5-121">`SingleOrDefaultAsync` is used in 5 places:</span></span>

* <span data-ttu-id="154f5-122">`OnGetAsync` na página Detalhes.</span><span class="sxs-lookup"><span data-stu-id="154f5-122">`OnGetAsync` in the Details page.</span></span>
* <span data-ttu-id="154f5-123">`OnGetAsync` e `OnPostAsync` nas páginas Editar e Excluir.</span><span class="sxs-lookup"><span data-stu-id="154f5-123">`OnGetAsync` and `OnPostAsync` in the Edit and Delete pages.</span></span>

<a name="FindAsync"></a>
### <a name="findasync"></a><span data-ttu-id="154f5-124">FindAsync</span><span class="sxs-lookup"><span data-stu-id="154f5-124">FindAsync</span></span>

<span data-ttu-id="154f5-125">Em grande parte do código gerado por scaffolding, [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) pode ser usado no lugar de `FirstOrDefaultAsync` ou `SingleOrDefaultAsync`.</span><span class="sxs-lookup"><span data-stu-id="154f5-125">In much of the scaffolded code, [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) can be used in place of `FirstOrDefaultAsync` or `SingleOrDefaultAsync`.</span></span> 

<span data-ttu-id="154f5-126">`FindAsync`:</span><span class="sxs-lookup"><span data-stu-id="154f5-126">`FindAsync`:</span></span>

* <span data-ttu-id="154f5-127">Encontra uma entidade com o PK (chave primária).</span><span class="sxs-lookup"><span data-stu-id="154f5-127">Finds an entity with the primary key (PK).</span></span> <span data-ttu-id="154f5-128">Se uma entidade com o PK está sendo controlada pelo contexto, ela é retornada sem uma solicitação para o BD.</span><span class="sxs-lookup"><span data-stu-id="154f5-128">If an entity with the PK is being tracked by the context, it's returned without a request to the DB.</span></span>
* <span data-ttu-id="154f5-129">É simples e conciso.</span><span class="sxs-lookup"><span data-stu-id="154f5-129">Is simple and concise.</span></span>
* <span data-ttu-id="154f5-130">É otimizado para pesquisar uma única entidade.</span><span class="sxs-lookup"><span data-stu-id="154f5-130">Is optimized to look up a single entity.</span></span>
* <span data-ttu-id="154f5-131">Pode ter benefícios de desempenho em algumas situações, mas raramente entram em jogo para cenários da Web normais.</span><span class="sxs-lookup"><span data-stu-id="154f5-131">Can have perf benefits in some situations, but they rarely come into play for normal web scenarios.</span></span>
* <span data-ttu-id="154f5-132">Usa [FirstAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) em vez de [SingleAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) de forma implícita.</span><span class="sxs-lookup"><span data-stu-id="154f5-132">Implicitly uses [FirstAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) instead of [SingleAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span></span>
<span data-ttu-id="154f5-133">Mas se você deseja incluir outras entidades, Localizar não é mais apropriado.</span><span class="sxs-lookup"><span data-stu-id="154f5-133">But if you want to Include other entities, then Find is no longer appropriate.</span></span> <span data-ttu-id="154f5-134">Isso significa que talvez seja necessário abandonar Localizar e passar para uma consulta à medida que o aplicativo avança.</span><span class="sxs-lookup"><span data-stu-id="154f5-134">This means that you may need to abandon Find and move to a query as your app progresses.</span></span>

## <a name="customize-the-details-page"></a><span data-ttu-id="154f5-135">Personalizar a página Detalhes</span><span class="sxs-lookup"><span data-stu-id="154f5-135">Customize the Details page</span></span>

<span data-ttu-id="154f5-136">Navegue para a página `Pages/Students`.</span><span class="sxs-lookup"><span data-stu-id="154f5-136">Browse to `Pages/Students` page.</span></span> <span data-ttu-id="154f5-137">Os links **Editar**, **Detalhes** e **Excluir** são gerados pelo [Auxiliar de Marcação de Âncora](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) no arquivo *Pages/Students/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="154f5-137">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Students/Index.cshtml* file.</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Students/Index1.cshtml?range=40-44)]

<span data-ttu-id="154f5-138">Selecione um link Detalhes.</span><span class="sxs-lookup"><span data-stu-id="154f5-138">Select a Details link.</span></span> <span data-ttu-id="154f5-139">A URL tem o formato `http://localhost:5000/Students/Details?id=2`.</span><span class="sxs-lookup"><span data-stu-id="154f5-139">The URL is of the form `http://localhost:5000/Students/Details?id=2`.</span></span> <span data-ttu-id="154f5-140">A ID do Aluno é passada com uma cadeia de caracteres de consulta (`?id=2`).</span><span class="sxs-lookup"><span data-stu-id="154f5-140">The Student ID is passed using a query string (`?id=2`).</span></span>

<span data-ttu-id="154f5-141">Atualize as Páginas Editar, Detalhes e Excluir do Razor para que elas usem o modelo de rota `"{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="154f5-141">Update the Edit, Details, and Delete Razor Pages to use the `"{id:int}"` route template.</span></span> <span data-ttu-id="154f5-142">Altere a diretiva de página de cada uma dessas páginas de `@page` para `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="154f5-142">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span>

<span data-ttu-id="154f5-143">Uma solicitação para a página com o modelo de rota "{id:int}" que **não** inclui um valor inteiro de rota retorna um erro HTTP 404 (não encontrado).</span><span class="sxs-lookup"><span data-stu-id="154f5-143">A request to the page with the "{id:int}" route template that does **not** include a integer route value returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="154f5-144">Por exemplo, `http://localhost:5000/Students/Details` retorna um erro 404.</span><span class="sxs-lookup"><span data-stu-id="154f5-144">For example, `http://localhost:5000/Students/Details` returns a 404 error.</span></span> <span data-ttu-id="154f5-145">Para tornar a ID opcional, acrescente `?` à restrição de rota:</span><span class="sxs-lookup"><span data-stu-id="154f5-145">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="154f5-146">Execute o aplicativo, clique em um link Detalhes e verifique se a URL está passando a ID como dados de rota (`http://localhost:5000/Students/Details/2`).</span><span class="sxs-lookup"><span data-stu-id="154f5-146">Run the app, click on a Details link, and verify the URL is passing the ID as route data (`http://localhost:5000/Students/Details/2`).</span></span>

<span data-ttu-id="154f5-147">Não altere `@page` globalmente para `@page "{id:int}"`, pois isso desfaz os links para as páginas Início e Criar.</span><span class="sxs-lookup"><span data-stu-id="154f5-147">Don't globally change `@page` to `@page "{id:int}"`, doing so breaks the links to the Home and Create pages.</span></span>

<!-- See https://github.com/aspnet/Scaffolding/issues/590 -->

### <a name="add-related-data"></a><span data-ttu-id="154f5-148">Adicionar dados relacionados</span><span class="sxs-lookup"><span data-stu-id="154f5-148">Add related data</span></span>

<span data-ttu-id="154f5-149">O código gerado por scaffolding para a página Índice de Alunos não inclui a propriedade `Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="154f5-149">The scaffolded code for the Students Index page doesn't include the `Enrollments` property.</span></span> <span data-ttu-id="154f5-150">Nesta seção, o conteúdo da coleção `Enrollments` é exibido na página Detalhes.</span><span class="sxs-lookup"><span data-stu-id="154f5-150">In this section, the contents of the `Enrollments` collection is displayed in the Details page.</span></span>

<span data-ttu-id="154f5-151">O método `OnGetAsync` de *Pages/Students/Details.cshtml.cs* usa o método `FirstOrDefaultAsync` para recuperar uma única entidade `Student`.</span><span class="sxs-lookup"><span data-stu-id="154f5-151">The `OnGetAsync` method of *Pages/Students/Details.cshtml.cs* uses the `FirstOrDefaultAsync` method to retrieve a single `Student` entity.</span></span> <span data-ttu-id="154f5-152">Adicione o seguinte código realçado:</span><span class="sxs-lookup"><span data-stu-id="154f5-152">Add the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Students/Details.cshtml.cs?name=snippet_Details&highlight=8-12)]

<span data-ttu-id="154f5-153">Os métodos `Include` e `ThenInclude` fazem com que o contexto carregue a propriedade de navegação `Student.Enrollments` e, dentro de cada registro, a propriedade de navegação `Enrollment.Course`.</span><span class="sxs-lookup"><span data-stu-id="154f5-153">The `Include` and `ThenInclude` methods cause the context to load the `Student.Enrollments` navigation property, and within each enrollment the `Enrollment.Course` navigation property.</span></span> <span data-ttu-id="154f5-154">Esses métodos são examinados em detalhes no tutorial de dados relacionado à leitura.</span><span class="sxs-lookup"><span data-stu-id="154f5-154">These methods are examined in detail in the reading-related data tutorial.</span></span>

<span data-ttu-id="154f5-155">O método `AsNoTracking` melhora o desempenho em cenários em que as entidades retornadas não são atualizadas no contexto atual.</span><span class="sxs-lookup"><span data-stu-id="154f5-155">The `AsNoTracking` method improves performance in scenarios when the entities returned are not updated in the current context.</span></span> <span data-ttu-id="154f5-156">`AsNoTracking` é abordado mais adiante neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="154f5-156">`AsNoTracking` is discussed later in this tutorial.</span></span>

### <a name="display-related-enrollments-on-the-details-page"></a><span data-ttu-id="154f5-157">Exibir registros relacionados na página Detalhes</span><span class="sxs-lookup"><span data-stu-id="154f5-157">Display related enrollments on the Details page</span></span>

<span data-ttu-id="154f5-158">Abra *Pages/Students/Details.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="154f5-158">Open *Pages/Students/Details.cshtml*.</span></span> <span data-ttu-id="154f5-159">Adicione o seguinte código realçado para exibir uma lista de registros:</span><span class="sxs-lookup"><span data-stu-id="154f5-159">Add the following highlighted code to display a list of enrollments:</span></span>

 <span data-ttu-id="154f5-160"><!--2do ricka. if doesn't change, remove dup --> [!code-cshtml[](intro/samples/cu/Pages/Students/Details1.cshtml?highlight=32-53)]</span><span class="sxs-lookup"><span data-stu-id="154f5-160"><!--2do ricka. if doesn't change, remove dup --> [!code-cshtml[](intro/samples/cu/Pages/Students/Details1.cshtml?highlight=32-53)]</span></span>

<span data-ttu-id="154f5-161">Se o recuo do código estiver incorreto depois que o código for colado, pressione CTRL-K-D para corrigi-lo.</span><span class="sxs-lookup"><span data-stu-id="154f5-161">If code indentation is wrong after the code is pasted, press CTRL-K-D to correct it.</span></span>

<span data-ttu-id="154f5-162">O código anterior percorre as entidades na propriedade de navegação `Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="154f5-162">The preceding code loops through the entities in the `Enrollments` navigation property.</span></span> <span data-ttu-id="154f5-163">Para cada registro, ele exibe o nome do curso e a nota.</span><span class="sxs-lookup"><span data-stu-id="154f5-163">For each enrollment, it displays the course title and the grade.</span></span> <span data-ttu-id="154f5-164">O título do curso é recuperado da entidade Course, que é armazenada na propriedade de navegação `Course` da entidade Enrollments.</span><span class="sxs-lookup"><span data-stu-id="154f5-164">The course title is retrieved from the Course entity that's stored in the `Course` navigation property of the Enrollments entity.</span></span>

<span data-ttu-id="154f5-165">Execute o aplicativo, selecione a guia **Alunos** e clique no link **Detalhes** de um aluno.</span><span class="sxs-lookup"><span data-stu-id="154f5-165">Run the app, select the **Students** tab, and click the **Details** link for a student.</span></span> <span data-ttu-id="154f5-166">A lista de cursos e notas do aluno selecionado é exibida.</span><span class="sxs-lookup"><span data-stu-id="154f5-166">The list of courses and grades for the selected student is displayed.</span></span>

## <a name="update-the-create-page"></a><span data-ttu-id="154f5-167">Atualizar a página Criar</span><span class="sxs-lookup"><span data-stu-id="154f5-167">Update the Create page</span></span>

<span data-ttu-id="154f5-168">Atualize o método `OnPostAsync` em *Pages/Students/Create.cshtml.cs* com o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="154f5-168">Update the `OnPostAsync` method in *Pages/Students/Create.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>
### <a name="tryupdatemodelasync"></a><span data-ttu-id="154f5-169">TryUpdateModelAsync</span><span class="sxs-lookup"><span data-stu-id="154f5-169">TryUpdateModelAsync</span></span>

<span data-ttu-id="154f5-170">Examine o código [TryUpdateModelAsync](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_):</span><span class="sxs-lookup"><span data-stu-id="154f5-170">Examine the [TryUpdateModelAsync](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_TryUpdateModelAsync)]

<span data-ttu-id="154f5-171">No código anterior, `TryUpdateModelAsync<Student>` tenta atualizar o objeto `emptyStudent` usando os valores de formulário postados da propriedade [PageContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) no [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="154f5-171">In the preceding code, `TryUpdateModelAsync<Student>` tries to update the `emptyStudent` object using the posted form values from the [PageContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) property in the [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel?view=aspnetcore-2.0).</span></span> <span data-ttu-id="154f5-172">`TryUpdateModelAsync` atualiza apenas as propriedades listadas (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).</span><span class="sxs-lookup"><span data-stu-id="154f5-172">`TryUpdateModelAsync` only updates the properties listed (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).</span></span>

<span data-ttu-id="154f5-173">Na amostra anterior:</span><span class="sxs-lookup"><span data-stu-id="154f5-173">In the preceding sample:</span></span>

* <span data-ttu-id="154f5-174">O segundo argumento (` "student", // Prefix`) é o prefixo usado para pesquisar valores.</span><span class="sxs-lookup"><span data-stu-id="154f5-174">The second argument (` "student", // Prefix`) is the prefix uses to look up values.</span></span> <span data-ttu-id="154f5-175">Não diferencia maiúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="154f5-175">It's not case sensitive.</span></span>
* <span data-ttu-id="154f5-176">Os valores de formulário postados são convertidos nos tipos no modelo `Student` usando a [associação de modelos](xref:mvc/models/model-binding#how-model-binding-works).</span><span class="sxs-lookup"><span data-stu-id="154f5-176">The posted form values are converted to the types in the `Student` model using [model binding](xref:mvc/models/model-binding#how-model-binding-works).</span></span>

<a id="overpost"></a>
### <a name="overposting"></a><span data-ttu-id="154f5-177">Excesso de postagem</span><span class="sxs-lookup"><span data-stu-id="154f5-177">Overposting</span></span>

<span data-ttu-id="154f5-178">O uso de `TryUpdateModel` para atualizar campos com valores postados é uma melhor prática de segurança porque ele impede o excesso de postagem.</span><span class="sxs-lookup"><span data-stu-id="154f5-178">Using `TryUpdateModel` to update fields with posted values is a security best practice because it prevents overposting.</span></span> <span data-ttu-id="154f5-179">Por exemplo, suponha que a entidade Student inclua uma propriedade `Secret` que esta página da Web não deve atualizar nem adicionar:</span><span class="sxs-lookup"><span data-stu-id="154f5-179">For example, suppose the Student entity includes a `Secret` property that this web page shouldn't update or add:</span></span>

[!code-csharp[](intro/samples/cu/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

<span data-ttu-id="154f5-180">Mesmo que o aplicativo não tenha um campo `Secret` na Página criar/atualizar do Razor, um invasor pode definir o valor `Secret` por excesso de postagem.</span><span class="sxs-lookup"><span data-stu-id="154f5-180">Even if the app doesn't have a `Secret` field on the create/update Razor Page, a hacker could set the `Secret` value by overposting.</span></span> <span data-ttu-id="154f5-181">Um invasor pode usar uma ferramenta como o Fiddler ou escrever um JavaScript para postar um valor de formulário `Secret`.</span><span class="sxs-lookup"><span data-stu-id="154f5-181">A hacker could use a tool such as Fiddler, or write some JavaScript, to post a `Secret` form value.</span></span> <span data-ttu-id="154f5-182">O código original não limita os campos que o associador de modelos usa quando ele cria uma instância Student.</span><span class="sxs-lookup"><span data-stu-id="154f5-182">The original code doesn't limit the fields that the model binder uses when it creates a Student instance.</span></span>

<span data-ttu-id="154f5-183">Seja qual for o valor que o invasor especificou para o campo de formulário `Secret`, ele será atualizado no BD.</span><span class="sxs-lookup"><span data-stu-id="154f5-183">Whatever value the hacker specified for the `Secret` form field is updated in the DB.</span></span> <span data-ttu-id="154f5-184">A imagem a seguir mostra a ferramenta Fiddler adicionando o campo `Secret` (com o valor "OverPost") aos valores de formulário postados.</span><span class="sxs-lookup"><span data-stu-id="154f5-184">The following image shows the Fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.</span></span>

![Fiddler adicionando o campo Secreto](../ef-mvc/crud/_static/fiddler.png)

<span data-ttu-id="154f5-186">O valor "OverPost" foi adicionado com êxito à propriedade `Secret` da linha inserida.</span><span class="sxs-lookup"><span data-stu-id="154f5-186">The value "OverPost" is successfully added to the `Secret` property of the inserted row.</span></span> <span data-ttu-id="154f5-187">O designer do aplicativo nunca desejou que a propriedade `Secret` fosse definida com a página Criar.</span><span class="sxs-lookup"><span data-stu-id="154f5-187">The app designer never intended the `Secret` property to be set with the Create page.</span></span>

<a name="vm"></a>
### <a name="view-model"></a><span data-ttu-id="154f5-188">Modelo de exibição</span><span class="sxs-lookup"><span data-stu-id="154f5-188">View model</span></span>

<span data-ttu-id="154f5-189">Um modelo de exibição normalmente contém um subconjunto das propriedades incluídas no modelo usado pelo aplicativo.</span><span class="sxs-lookup"><span data-stu-id="154f5-189">A view model typically contains a subset of the properties included in the model used by the application.</span></span> <span data-ttu-id="154f5-190">O modelo de aplicativo costuma ser chamado de modelo de domínio.</span><span class="sxs-lookup"><span data-stu-id="154f5-190">The application model is often called the domain model.</span></span> <span data-ttu-id="154f5-191">O modelo de domínio normalmente contém todas as propriedades necessárias para a entidade correspondente no BD.</span><span class="sxs-lookup"><span data-stu-id="154f5-191">The domain model typically contains all the properties required by the corresponding entity in the DB.</span></span> <span data-ttu-id="154f5-192">O modelo de exibição contém apenas as propriedades necessárias para a camada de interface do usuário (por exemplo, a página Criar).</span><span class="sxs-lookup"><span data-stu-id="154f5-192">The view model contains only the properties needed for the UI layer (for example, the Create page).</span></span> <span data-ttu-id="154f5-193">Além do modelo de exibição, alguns aplicativos usam um modelo de associação ou modelo de entrada para passar dados entre a classe de modelo de página das Páginas do Razor e o navegador.</span><span class="sxs-lookup"><span data-stu-id="154f5-193">In addition to the view model, some apps use a binding model or input model to pass data between the Razor Pages page model class and the browser.</span></span> <span data-ttu-id="154f5-194">Considere o seguinte modelo de exibição `Student`:</span><span class="sxs-lookup"><span data-stu-id="154f5-194">Consider the following `Student` view model:</span></span>

[!code-csharp[](intro/samples/cu/Models/StudentVM.cs)]

<span data-ttu-id="154f5-195">Os modelos de exibição fornecem uma maneira alternativa para impedir o excesso de postagem.</span><span class="sxs-lookup"><span data-stu-id="154f5-195">View models provide an alternative way to prevent overposting.</span></span> <span data-ttu-id="154f5-196">O modelo de exibição contém apenas as propriedades a serem exibidas ou atualizadas.</span><span class="sxs-lookup"><span data-stu-id="154f5-196">The view model contains only the properties to view (display) or update.</span></span>

<span data-ttu-id="154f5-197">O seguinte código usa o modelo de exibição `StudentVM` para criar um novo aluno:</span><span class="sxs-lookup"><span data-stu-id="154f5-197">The following code uses the `StudentVM` view model to create a new student:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="154f5-198">O método [SetValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) define os valores desse objeto lendo os valores de outro objeto [PropertyValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues).</span><span class="sxs-lookup"><span data-stu-id="154f5-198">The [SetValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) method sets the values of this object by reading values from another [PropertyValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) object.</span></span> <span data-ttu-id="154f5-199">`SetValues` usa a correspondência de nomes de propriedade.</span><span class="sxs-lookup"><span data-stu-id="154f5-199">`SetValues` uses property name matching.</span></span> <span data-ttu-id="154f5-200">O tipo de modelo de exibição não precisa estar relacionado ao tipo de modelo, apenas precisa ter as propriedades correspondentes.</span><span class="sxs-lookup"><span data-stu-id="154f5-200">The view model type doesn't need to be related to the model type, it just needs to have properties that match.</span></span>

<span data-ttu-id="154f5-201">O uso de `StudentVM` exige a atualização de [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Students/CreateVM.cshtml) para usar `StudentVM` em vez de `Student`.</span><span class="sxs-lookup"><span data-stu-id="154f5-201">Using `StudentVM` requires [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Students/CreateVM.cshtml) be updated to use `StudentVM` rather than `Student`.</span></span>

<span data-ttu-id="154f5-202">Nas Páginas do Razor, a classe derivada `PageModel` é o modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="154f5-202">In Razor Pages, the `PageModel` derived class is the view model.</span></span> 

## <a name="update-the-edit-page"></a><span data-ttu-id="154f5-203">Atualizar a página Editar</span><span class="sxs-lookup"><span data-stu-id="154f5-203">Update the Edit page</span></span>

<span data-ttu-id="154f5-204">Atualize o modelo de página para a página de edição.</span><span class="sxs-lookup"><span data-stu-id="154f5-204">Update the page model for the Edit page.</span></span> <span data-ttu-id="154f5-205">As alterações principais são realçadas:</span><span class="sxs-lookup"><span data-stu-id="154f5-205">The major changes are highlighted:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Students/Edit.cshtml.cs?name=snippet_OnPostAsync&highlight=20,36)]

<span data-ttu-id="154f5-206">As alterações de código são semelhantes à página Criar, com algumas exceções:</span><span class="sxs-lookup"><span data-stu-id="154f5-206">The code changes are similar to the Create page with a few exceptions:</span></span>

* <span data-ttu-id="154f5-207">`OnPostAsync` tem um parâmetro `id` opcional.</span><span class="sxs-lookup"><span data-stu-id="154f5-207">`OnPostAsync` has an optional `id` parameter.</span></span>
* <span data-ttu-id="154f5-208">O aluno atual é buscado do BD, em vez de criar um aluno vazio.</span><span class="sxs-lookup"><span data-stu-id="154f5-208">The current student is fetched from the DB, rather than creating an empty student.</span></span>
* <span data-ttu-id="154f5-209">`FirstOrDefaultAsync` foi substituído por [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync?view=efcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="154f5-209">`FirstOrDefaultAsync` has been replaced with [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync?view=efcore-2.0).</span></span> <span data-ttu-id="154f5-210">`FindAsync` é uma boa opção ao selecionar uma entidade da chave primária.</span><span class="sxs-lookup"><span data-stu-id="154f5-210">`FindAsync` is a good choice when selecting an entity from the primary key.</span></span> <span data-ttu-id="154f5-211">Consulte [FindAsync](#FindAsync) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="154f5-211">See [FindAsync](#FindAsync) for more information.</span></span>

### <a name="test-the-edit-and-create-pages"></a><span data-ttu-id="154f5-212">Testar as páginas Editar e Criar</span><span class="sxs-lookup"><span data-stu-id="154f5-212">Test the Edit and Create pages</span></span>

<span data-ttu-id="154f5-213">Crie e edite algumas entidades de aluno.</span><span class="sxs-lookup"><span data-stu-id="154f5-213">Create and edit a few student entities.</span></span>

## <a name="entity-states"></a><span data-ttu-id="154f5-214">Estados da entidade</span><span class="sxs-lookup"><span data-stu-id="154f5-214">Entity States</span></span>

<span data-ttu-id="154f5-215">O contexto de BD controla se as entidades em memória estão em sincronia com suas linhas correspondentes no BD.</span><span class="sxs-lookup"><span data-stu-id="154f5-215">The DB context keeps track of whether entities in memory are in sync with their corresponding rows in the DB.</span></span> <span data-ttu-id="154f5-216">As informações de sincronização de contexto do BD determinam o que acontece quando `SaveChanges` é chamado.</span><span class="sxs-lookup"><span data-stu-id="154f5-216">The DB context sync information determines what happens when `SaveChanges` is called.</span></span> <span data-ttu-id="154f5-217">Por exemplo, quando uma nova entidade é passada para o método `Add`, o estado da entidade é definido como `Added`.</span><span class="sxs-lookup"><span data-stu-id="154f5-217">For example, when a new entity is passed to the `Add` method, that entity's state is set to `Added`.</span></span> <span data-ttu-id="154f5-218">Quando `SaveChanges` é chamado, o contexto de BD emite um comando SQL INSERT.</span><span class="sxs-lookup"><span data-stu-id="154f5-218">When `SaveChanges` is called, the DB context issues a SQL INSERT command.</span></span>

<span data-ttu-id="154f5-219">Uma entidade pode estar em um dos seguintes estados:</span><span class="sxs-lookup"><span data-stu-id="154f5-219">An entity may be in one of the following states:</span></span>

* <span data-ttu-id="154f5-220">`Added`: a entidade ainda não existe no BD.</span><span class="sxs-lookup"><span data-stu-id="154f5-220">`Added`: The entity doesn't yet exist in the DB.</span></span> <span data-ttu-id="154f5-221">O método `SaveChanges` emite uma instrução INSERT.</span><span class="sxs-lookup"><span data-stu-id="154f5-221">The `SaveChanges` method issues an INSERT statement.</span></span>

* <span data-ttu-id="154f5-222">`Unchanged`: nenhuma alteração precisa ser salva com essa entidade.</span><span class="sxs-lookup"><span data-stu-id="154f5-222">`Unchanged`: No changes need to be saved with this entity.</span></span> <span data-ttu-id="154f5-223">Uma entidade tem esse status quando é lida do BD.</span><span class="sxs-lookup"><span data-stu-id="154f5-223">An entity has this status when it's read from the DB.</span></span>

* <span data-ttu-id="154f5-224">`Modified`: alguns ou todos os valores de propriedade da entidade foram modificados.</span><span class="sxs-lookup"><span data-stu-id="154f5-224">`Modified`: Some or all of the entity's property values have been modified.</span></span> <span data-ttu-id="154f5-225">O método `SaveChanges` emite uma instrução UPDATE.</span><span class="sxs-lookup"><span data-stu-id="154f5-225">The `SaveChanges` method issues an UPDATE statement.</span></span>

* <span data-ttu-id="154f5-226">`Deleted`: a entidade foi marcada para exclusão.</span><span class="sxs-lookup"><span data-stu-id="154f5-226">`Deleted`: The entity has been marked for deletion.</span></span> <span data-ttu-id="154f5-227">O método `SaveChanges` emite uma instrução DELETE.</span><span class="sxs-lookup"><span data-stu-id="154f5-227">The `SaveChanges` method issues a DELETE statement.</span></span>

* <span data-ttu-id="154f5-228">`Detached`: a entidade não está sendo controlada pelo contexto de BD.</span><span class="sxs-lookup"><span data-stu-id="154f5-228">`Detached`: The entity isn't being tracked by the DB context.</span></span>

<span data-ttu-id="154f5-229">Em um aplicativo da área de trabalho, em geral, as alterações de estado são definidas automaticamente.</span><span class="sxs-lookup"><span data-stu-id="154f5-229">In a desktop app, state changes are typically set automatically.</span></span> <span data-ttu-id="154f5-230">Uma entidade é lida, as alterações são feitas e o estado da entidade é alterado automaticamente para `Modified`.</span><span class="sxs-lookup"><span data-stu-id="154f5-230">An entity is read, changes are made, and the entity state to automatically be changed to `Modified`.</span></span> <span data-ttu-id="154f5-231">A chamada a `SaveChanges` gera uma instrução SQL UPDATE que atualiza apenas as propriedades alteradas.</span><span class="sxs-lookup"><span data-stu-id="154f5-231">Calling `SaveChanges` generates a SQL UPDATE statement that updates only the changed properties.</span></span>

<span data-ttu-id="154f5-232">Em um aplicativo Web, o `DbContext` que lê uma entidade e exibe os dados é descartado depois que uma página é renderizada.</span><span class="sxs-lookup"><span data-stu-id="154f5-232">In a web app, the `DbContext` that reads an entity and displays the data is disposed after a page is rendered.</span></span> <span data-ttu-id="154f5-233">Quando o método `OnPostAsync` de uma página é chamado, é feita uma nova solicitação da Web e com uma nova instância do `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="154f5-233">When a page's `OnPostAsync` method is called, a new web request is made and with a new instance of the `DbContext`.</span></span> <span data-ttu-id="154f5-234">A nova leitura da entidade nesse novo contexto simula o processamento da área de trabalho.</span><span class="sxs-lookup"><span data-stu-id="154f5-234">Re-reading the entity in that new context simulates desktop processing.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="154f5-235">Atualizar a página Excluir</span><span class="sxs-lookup"><span data-stu-id="154f5-235">Update the Delete page</span></span>

<span data-ttu-id="154f5-236">Nesta seção, o código é adicionado para implementar uma mensagem de erro personalizada quando há falha na chamada a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="154f5-236">In this section, code is added to implement a custom error message when the call to `SaveChanges` fails.</span></span> <span data-ttu-id="154f5-237">Adicione uma cadeia de caracteres para que ela contenha as possíveis mensagens de erro:</span><span class="sxs-lookup"><span data-stu-id="154f5-237">Add a string to contain possible error messages:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet1&highlight=12)]

<span data-ttu-id="154f5-238">Substitua o método `OnGetAsync` pelo seguinte código:</span><span class="sxs-lookup"><span data-stu-id="154f5-238">Replace the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnGetAsync&highlight=1,9,17-20)]

<span data-ttu-id="154f5-239">O código anterior contém o parâmetro opcional `saveChangesError`.</span><span class="sxs-lookup"><span data-stu-id="154f5-239">The preceding code contains the optional parameter `saveChangesError`.</span></span> <span data-ttu-id="154f5-240">`saveChangesError` indica se o método foi chamado após uma falha ao excluir o objeto de aluno.</span><span class="sxs-lookup"><span data-stu-id="154f5-240">`saveChangesError` indicates whether the method was called after a failure to delete the student object.</span></span> <span data-ttu-id="154f5-241">A operação de exclusão pode falhar devido a problemas de rede temporários.</span><span class="sxs-lookup"><span data-stu-id="154f5-241">The delete operation might fail because of transient network problems.</span></span> <span data-ttu-id="154f5-242">Erros de rede transitórios serão mais prováveis de ocorrerem na nuvem.</span><span class="sxs-lookup"><span data-stu-id="154f5-242">Transient network errors are more likely in the cloud.</span></span> <span data-ttu-id="154f5-243">`saveChangesError` é falso quando a página Excluir `OnGetAsync` é chamada na interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="154f5-243">`saveChangesError`is false when the Delete page `OnGetAsync` is called from the UI.</span></span> <span data-ttu-id="154f5-244">Quando `OnGetAsync` é chamado por `OnPostAsync` (devido à falha da operação de exclusão), o parâmetro `saveChangesError` é verdadeiro.</span><span class="sxs-lookup"><span data-stu-id="154f5-244">When `OnGetAsync` is called by `OnPostAsync` (because the delete operation failed), the `saveChangesError` parameter is true.</span></span>

### <a name="the-delete-pages-onpostasync-method"></a><span data-ttu-id="154f5-245">O método OnPostAsync nas páginas Excluir</span><span class="sxs-lookup"><span data-stu-id="154f5-245">The Delete pages OnPostAsync method</span></span>

<span data-ttu-id="154f5-246">Substitua `OnPostAsync` pelo seguinte código:</span><span class="sxs-lookup"><span data-stu-id="154f5-246">Replace the `OnPostAsync` with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="154f5-247">O código anterior recupera a entidade selecionada e, em seguida, chama o método `Remove` para definir o status da entidade como `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="154f5-247">The preceding code retrieves the selected entity, then calls the `Remove` method to set the entity's status to `Deleted`.</span></span> <span data-ttu-id="154f5-248">Quando `SaveChanges` é chamado, um comando SQL DELETE é gerado.</span><span class="sxs-lookup"><span data-stu-id="154f5-248">When `SaveChanges` is called, a SQL DELETE command is generated.</span></span> <span data-ttu-id="154f5-249">Se `Remove` falhar:</span><span class="sxs-lookup"><span data-stu-id="154f5-249">If `Remove` fails:</span></span>

* <span data-ttu-id="154f5-250">A exceção de BD é capturada.</span><span class="sxs-lookup"><span data-stu-id="154f5-250">The DB exception is caught.</span></span>
* <span data-ttu-id="154f5-251">O método `OnGetAsync` das páginas Excluir é chamado com `saveChangesError=true`.</span><span class="sxs-lookup"><span data-stu-id="154f5-251">The Delete pages `OnGetAsync` method is called with `saveChangesError=true`.</span></span>

### <a name="update-the-delete-razor-page"></a><span data-ttu-id="154f5-252">Atualizar a Página Excluir do Razor</span><span class="sxs-lookup"><span data-stu-id="154f5-252">Update the Delete Razor Page</span></span>

<span data-ttu-id="154f5-253">Adicione a mensagem de erro realçada a seguir à Página Excluir do Razor.</span><span class="sxs-lookup"><span data-stu-id="154f5-253">Add the following highlighted error message to the Delete Razor Page.</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Students/Delete.cshtml?range=1-13&highlight=10)]

<span data-ttu-id="154f5-254">Exclusão de teste.</span><span class="sxs-lookup"><span data-stu-id="154f5-254">Test Delete.</span></span>

## <a name="common-errors"></a><span data-ttu-id="154f5-255">Erros comuns</span><span class="sxs-lookup"><span data-stu-id="154f5-255">Common errors</span></span>

<span data-ttu-id="154f5-256">Student/Home ou outros links não funcionam:</span><span class="sxs-lookup"><span data-stu-id="154f5-256">Student/Home or other links don't work:</span></span>

<span data-ttu-id="154f5-257">Verifique se a Página do Razor contém a diretiva `@page` correta.</span><span class="sxs-lookup"><span data-stu-id="154f5-257">Verify the Razor Page contains the correct `@page` directive.</span></span> <span data-ttu-id="154f5-258">Por exemplo, a Página Aluno/Home do Razor **não** deve conter um modelo de rota:</span><span class="sxs-lookup"><span data-stu-id="154f5-258">For example, The Student/Home Razor Page should **not** contain a route template:</span></span>

```cshtml
@page "{id:int}"
```

<span data-ttu-id="154f5-259">Cada Página do Razor deve incluir a diretiva `@page`.</span><span class="sxs-lookup"><span data-stu-id="154f5-259">Each Razor Page must include the `@page` directive.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="154f5-260">[Anterior](xref:data/ef-rp/intro)
> [Próximo](xref:data/ef-rp/sort-filter-page)</span><span class="sxs-lookup"><span data-stu-id="154f5-260">[Previous](xref:data/ef-rp/intro)
[Next](xref:data/ef-rp/sort-filter-page)</span></span>
