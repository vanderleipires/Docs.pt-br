---
title: "Núcleo do ASP.NET MVC com núcleo EF - classificação, filtro, paginação - 3 de 10"
author: tdykstra
description: "Neste tutorial, você adicionará a classificação, filtragem e paginação funcionalidade para a página usando o ASP.NET Core e o Entity Framework Core."
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/sort-filter-page
ms.openlocfilehash: feb4a50c9e5602064e7d493b6991485949903f47
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="sorting-filtering-paging-and-grouping---ef-core-with-aspnet-core-mvc-tutorial-3-of-10"></a><span data-ttu-id="39b41-103">A classificação, filtragem, paginação e agrupando - Core de EF com o tutorial do MVC do ASP.NET Core (3 de 10)</span><span class="sxs-lookup"><span data-stu-id="39b41-103">Sorting, filtering, paging, and grouping - EF Core with ASP.NET Core MVC tutorial (3 of 10)</span></span>

<span data-ttu-id="39b41-104">Por [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="39b41-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="39b41-105">O aplicativo web de exemplo Contoso University demonstra como criar aplicativos do ASP.NET MVC de núcleo da web usando o Entity Framework Core e o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="39b41-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="39b41-106">Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial na série](intro.md).</span><span class="sxs-lookup"><span data-stu-id="39b41-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="39b41-107">No tutorial anterior, você implementou um conjunto de páginas da web para operações CRUD básicas para entidades do aluno.</span><span class="sxs-lookup"><span data-stu-id="39b41-107">In the previous tutorial, you implemented a set of web pages for basic CRUD operations for Student entities.</span></span> <span data-ttu-id="39b41-108">Neste tutorial você adicionará a classificação, filtragem e funcionalidade de paginação para a página de índice de alunos.</span><span class="sxs-lookup"><span data-stu-id="39b41-108">In this tutorial you'll add sorting, filtering, and paging functionality to the Students Index page.</span></span> <span data-ttu-id="39b41-109">Você também criará uma página que faz o agrupamento simples.</span><span class="sxs-lookup"><span data-stu-id="39b41-109">You'll also create a page that does simple grouping.</span></span>

<span data-ttu-id="39b41-110">A ilustração a seguir mostra a aparência de página quando terminar.</span><span class="sxs-lookup"><span data-stu-id="39b41-110">The following illustration shows what the page will look like when you're done.</span></span> <span data-ttu-id="39b41-111">Os cabeçalhos de coluna são links que o usuário pode clicar para classificar por essa coluna.</span><span class="sxs-lookup"><span data-stu-id="39b41-111">The column headings are links that the user can click to sort by that column.</span></span> <span data-ttu-id="39b41-112">Clicar em uma coluna de título repetidamente alterna entre crescente e decrescente de classificação.</span><span class="sxs-lookup"><span data-stu-id="39b41-112">Clicking a column heading repeatedly toggles between ascending and descending sort order.</span></span>

![Página de índice de alunos](sort-filter-page/_static/paging.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a><span data-ttu-id="39b41-114">Adicionar Links de classificação da coluna para a página de índice de alunos</span><span class="sxs-lookup"><span data-stu-id="39b41-114">Add Column Sort Links to the Students Index Page</span></span>

<span data-ttu-id="39b41-115">Para adicionar uma classificação para a página de índice do aluno, você alterará o `Index` método do controlador alunos e adicione o código para o modo de exibição do índice do aluno.</span><span class="sxs-lookup"><span data-stu-id="39b41-115">To add sorting to the Student Index page, you'll change the `Index` method of the Students controller and add code to the Student Index view.</span></span>

### <a name="add-sorting-functionality-to-the-index-method"></a><span data-ttu-id="39b41-116">Adicionar a funcionalidade de classificação para o método de índice</span><span class="sxs-lookup"><span data-stu-id="39b41-116">Add sorting Functionality to the Index method</span></span>

<span data-ttu-id="39b41-117">Em *StudentsController.cs*, substitua o `Index` método com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="39b41-117">In *StudentsController.cs*, replace the `Index` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly)]

<span data-ttu-id="39b41-118">Esse código recebe um `sortOrder` parâmetro da cadeia de consulta na URL.</span><span class="sxs-lookup"><span data-stu-id="39b41-118">This code receives a `sortOrder` parameter from the query string in the URL.</span></span> <span data-ttu-id="39b41-119">O valor de cadeia de caracteres de consulta é fornecido pelo MVC do ASP.NET Core como um parâmetro para o método de ação.</span><span class="sxs-lookup"><span data-stu-id="39b41-119">The query string value is provided by ASP.NET Core MVC as a parameter to the action method.</span></span> <span data-ttu-id="39b41-120">O parâmetro será uma cadeia de caracteres que é o "Nome" ou "Data", opcionalmente seguido por um sublinhado e a cadeia de caracteres "desc" para especificar a ordem decrescente.</span><span class="sxs-lookup"><span data-stu-id="39b41-120">The parameter will be a string that's either "Name" or "Date", optionally followed by an underscore and the string "desc" to specify descending order.</span></span> <span data-ttu-id="39b41-121">A ordem de classificação crescente é padrão.</span><span class="sxs-lookup"><span data-stu-id="39b41-121">The default sort order is ascending.</span></span>

<span data-ttu-id="39b41-122">Na primeira vez em que a página de índice é solicitada, não há nenhuma cadeia de caracteres de consulta.</span><span class="sxs-lookup"><span data-stu-id="39b41-122">The first time the Index page is requested, there's no query string.</span></span> <span data-ttu-id="39b41-123">Os alunos são exibidos em ordem crescente pelo sobrenome, o que é o padrão, conforme estabelecido pelo caso leva a algo no `switch` instrução.</span><span class="sxs-lookup"><span data-stu-id="39b41-123">The students are displayed in ascending order by last name, which is the default as established by the fall-through case in the `switch` statement.</span></span> <span data-ttu-id="39b41-124">Quando o usuário clica em um hiperlink de título de coluna, apropriado `sortOrder` valor é fornecido na cadeia de caracteres de consulta.</span><span class="sxs-lookup"><span data-stu-id="39b41-124">When the user clicks a column heading hyperlink, the appropriate `sortOrder` value is provided in the query string.</span></span>

<span data-ttu-id="39b41-125">Os dois `ViewData` elementos (NameSortParm e DateSortParm) são usados pelo modo de exibição para configurar os hiperlinks de título de coluna com os valores de cadeia de caracteres de consulta apropriada.</span><span class="sxs-lookup"><span data-stu-id="39b41-125">The two `ViewData` elements (NameSortParm and DateSortParm) are used by the view to configure the column heading hyperlinks with the appropriate query string values.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly&highlight=3-4)]

<span data-ttu-id="39b41-126">Essas são instruções ternários.</span><span class="sxs-lookup"><span data-stu-id="39b41-126">These are ternary statements.</span></span> <span data-ttu-id="39b41-127">A primeira delas Especifica que o `sortOrder` parâmetro é nulo ou vazio, NameSortParm deve ser definido como "name_desc"; caso contrário, ele deve ser definido como uma cadeia de caracteres vazia.</span><span class="sxs-lookup"><span data-stu-id="39b41-127">The first one specifies that if the `sortOrder` parameter is null or empty, NameSortParm should be set to "name_desc"; otherwise, it should be set to an empty string.</span></span> <span data-ttu-id="39b41-128">Essas duas instruções habilitar o modo de exibição definir a coluna de hiperlinks do título da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="39b41-128">These two statements enable the view to set the column heading hyperlinks as follows:</span></span>

|  <span data-ttu-id="39b41-129">Ordem de classificação atual</span><span class="sxs-lookup"><span data-stu-id="39b41-129">Current sort order</span></span>  | <span data-ttu-id="39b41-130">Hiperlink do último nome</span><span class="sxs-lookup"><span data-stu-id="39b41-130">Last Name Hyperlink</span></span> | <span data-ttu-id="39b41-131">Hiperlink de data</span><span class="sxs-lookup"><span data-stu-id="39b41-131">Date Hyperlink</span></span> |
|:--------------------:|:-------------------:|:--------------:|
| <span data-ttu-id="39b41-132">Último nome em ordem crescente</span><span class="sxs-lookup"><span data-stu-id="39b41-132">Last Name ascending</span></span>  | <span data-ttu-id="39b41-133">descending</span><span class="sxs-lookup"><span data-stu-id="39b41-133">descending</span></span>          | <span data-ttu-id="39b41-134">ascending</span><span class="sxs-lookup"><span data-stu-id="39b41-134">ascending</span></span>      |
| <span data-ttu-id="39b41-135">Último nome em ordem decrescente</span><span class="sxs-lookup"><span data-stu-id="39b41-135">Last Name descending</span></span> | <span data-ttu-id="39b41-136">ascending</span><span class="sxs-lookup"><span data-stu-id="39b41-136">ascending</span></span>           | <span data-ttu-id="39b41-137">ascending</span><span class="sxs-lookup"><span data-stu-id="39b41-137">ascending</span></span>      |
| <span data-ttu-id="39b41-138">Data em ordem crescente</span><span class="sxs-lookup"><span data-stu-id="39b41-138">Date ascending</span></span>       | <span data-ttu-id="39b41-139">ascending</span><span class="sxs-lookup"><span data-stu-id="39b41-139">ascending</span></span>           | <span data-ttu-id="39b41-140">descending</span><span class="sxs-lookup"><span data-stu-id="39b41-140">descending</span></span>     |
| <span data-ttu-id="39b41-141">Data em ordem decrescente</span><span class="sxs-lookup"><span data-stu-id="39b41-141">Date descending</span></span>      | <span data-ttu-id="39b41-142">ascending</span><span class="sxs-lookup"><span data-stu-id="39b41-142">ascending</span></span>           | <span data-ttu-id="39b41-143">ascending</span><span class="sxs-lookup"><span data-stu-id="39b41-143">ascending</span></span>      |

<span data-ttu-id="39b41-144">O método usa LINQ to Entities para especificar a coluna para classificar por.</span><span class="sxs-lookup"><span data-stu-id="39b41-144">The method uses LINQ to Entities to specify the column to sort by.</span></span> <span data-ttu-id="39b41-145">O código cria um `IQueryable` variável antes da instrução switch, modifica-lo a instrução switch e chama o `ToListAsync` método após o `switch` instrução.</span><span class="sxs-lookup"><span data-stu-id="39b41-145">The code creates an `IQueryable` variable before the switch statement, modifies it in the switch statement, and calls the `ToListAsync` method after the `switch` statement.</span></span> <span data-ttu-id="39b41-146">Quando você criar e modificar `IQueryable` variáveis, nenhuma consulta é enviada para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="39b41-146">When you create and modify `IQueryable` variables, no query is sent to the database.</span></span> <span data-ttu-id="39b41-147">A consulta não é executada até que você converta o `IQueryable` objeto em uma coleção, chamando um método como `ToListAsync`.</span><span class="sxs-lookup"><span data-stu-id="39b41-147">The query isn't executed until you convert the `IQueryable` object into a collection by calling a method such as `ToListAsync`.</span></span> <span data-ttu-id="39b41-148">Portanto, esse código resulta em uma única consulta que não é executada até que o `return View` instrução.</span><span class="sxs-lookup"><span data-stu-id="39b41-148">Therefore, this code results in a single query that's not executed until the `return View` statement.</span></span>

<span data-ttu-id="39b41-149">Este código poderia ficar detalhado com um grande número de colunas.</span><span class="sxs-lookup"><span data-stu-id="39b41-149">This code could get verbose with a large number of columns.</span></span> <span data-ttu-id="39b41-150">[O último tutorial nesta série](advanced.md#dynamic-linq) mostra como escrever código que permite que você passe o nome do `OrderBy` coluna em uma variável de cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="39b41-150">[The last tutorial in this series](advanced.md#dynamic-linq) shows how to write code that lets you pass the name of the `OrderBy` column in a string variable.</span></span>

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a><span data-ttu-id="39b41-151">Adicionar hiperlinks de título de coluna para a exibição do índice do aluno</span><span class="sxs-lookup"><span data-stu-id="39b41-151">Add column heading hyperlinks to the Student Index view</span></span>

<span data-ttu-id="39b41-152">Substitua o código em *Views/Students/Index.cshtml*, com o código a seguir para adicionar hiperlinks de título de coluna.</span><span class="sxs-lookup"><span data-stu-id="39b41-152">Replace the code in *Views/Students/Index.cshtml*, with the following code to add column heading hyperlinks.</span></span> <span data-ttu-id="39b41-153">As linhas alteradas são realçadas.</span><span class="sxs-lookup"><span data-stu-id="39b41-153">The changed lines are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index2.cshtml?highlight=16,22)]

<span data-ttu-id="39b41-154">Esse código usa as informações no `ViewData` valores de cadeia de caracteres de propriedades para configurar hiperlinks com a consulta apropriada.</span><span class="sxs-lookup"><span data-stu-id="39b41-154">This code uses the information in `ViewData` properties to set up hyperlinks with the appropriate query string values.</span></span>

<span data-ttu-id="39b41-155">Executar o aplicativo, selecione o **alunos** guia e, em seguida, clique no **Sobrenome** e **data de inscrição** títulos de coluna para verificar essa classificação funciona.</span><span class="sxs-lookup"><span data-stu-id="39b41-155">Run the app, select the **Students** tab, and click the **Last Name** and **Enrollment Date** column headings to verify that sorting works.</span></span>

![Página de índice de alunos na ordem do nome](sort-filter-page/_static/name-order.png)

## <a name="add-a-search-box-to-the-students-index-page"></a><span data-ttu-id="39b41-157">Adicione uma caixa de pesquisa para a página de índice de alunos</span><span class="sxs-lookup"><span data-stu-id="39b41-157">Add a Search Box to the Students Index page</span></span>

<span data-ttu-id="39b41-158">Para adicionar a filtragem para a página de índice de alunos, você adicionará uma caixa de texto e um botão de envio para o modo de exibição e fazer as alterações correspondentes no `Index` método.</span><span class="sxs-lookup"><span data-stu-id="39b41-158">To add filtering to the Students Index page, you'll add a text box and a submit button to the view and make corresponding changes in the `Index` method.</span></span> <span data-ttu-id="39b41-159">A caixa de texto permitirá que você insira uma cadeia de caracteres a ser pesquisado no nome e o último campos de nome.</span><span class="sxs-lookup"><span data-stu-id="39b41-159">The text box will let you enter a string to search for in the first name and last name fields.</span></span>

### <a name="add-filtering-functionality-to-the-index-method"></a><span data-ttu-id="39b41-160">Adicionar a funcionalidade de filtragem para o método de índice</span><span class="sxs-lookup"><span data-stu-id="39b41-160">Add filtering functionality to the Index method</span></span>

<span data-ttu-id="39b41-161">Em *StudentsController.cs*, substitua o `Index` método com o código a seguir (as alterações são realçadas).</span><span class="sxs-lookup"><span data-stu-id="39b41-161">In *StudentsController.cs*, replace the `Index` method with the following code (the changes are highlighted).</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

<span data-ttu-id="39b41-162">Você adicionou um `searchString` parâmetro para o `Index` método.</span><span class="sxs-lookup"><span data-stu-id="39b41-162">You've added a `searchString` parameter to the `Index` method.</span></span> <span data-ttu-id="39b41-163">O valor de cadeia de caracteres de pesquisa é recebido em uma caixa de texto que você adicionará à exibição de índice.</span><span class="sxs-lookup"><span data-stu-id="39b41-163">The search string value is received from a text box that you'll add to the Index view.</span></span> <span data-ttu-id="39b41-164">Você também adicionou à instrução LINQ um onde cláusula que seleciona somente os alunos cujo primeiro nome ou sobrenome contém a cadeia de caracteres de pesquisa.</span><span class="sxs-lookup"><span data-stu-id="39b41-164">You've also added to the LINQ statement a where clause that selects only students whose first name or last name contains the search string.</span></span> <span data-ttu-id="39b41-165">A instrução que adiciona onde cláusula é executada somente se houver um valor de pesquisa.</span><span class="sxs-lookup"><span data-stu-id="39b41-165">The statement that adds the where clause is executed only if there's a value to search for.</span></span>

> [!NOTE]
> <span data-ttu-id="39b41-166">Aqui você estiver chamando o `Where` método em um `IQueryable` objeto e o filtro serão processado no servidor.</span><span class="sxs-lookup"><span data-stu-id="39b41-166">Here you are calling the `Where` method on an `IQueryable` object, and the filter will be processed on the server.</span></span> <span data-ttu-id="39b41-167">Em alguns cenários você pode chamar o `Where` método como um método de extensão em uma coleção de memória.</span><span class="sxs-lookup"><span data-stu-id="39b41-167">In some scenarios you might be calling the `Where` method as an extension method on an in-memory collection.</span></span> <span data-ttu-id="39b41-168">(Por exemplo, suponha que você alterar a referência ao `_context.Students` para que em vez de um EF `DbSet` faz referência a um método de repositório que retorna um `IEnumerable` coleção.) O resultado normalmente é o mesmo, mas em alguns casos pode ser diferente.</span><span class="sxs-lookup"><span data-stu-id="39b41-168">(For example, suppose you change the reference to `_context.Students` so that instead of an EF `DbSet` it references a repository method that returns an `IEnumerable` collection.) The result would normally be the same but in some cases may be different.</span></span>
>
><span data-ttu-id="39b41-169">Por exemplo, a implementação do .NET Framework do `Contains` método executa uma comparação que diferencia maiusculas de minúsculas por padrão, mas no SQL Server, isso é determinado pela configuração de agrupamento de instância do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="39b41-169">For example, the .NET Framework implementation of the `Contains` method performs a case-sensitive comparison by default, but in SQL Server this is determined by the collation setting of the SQL Server instance.</span></span> <span data-ttu-id="39b41-170">Essa configuração padrão para maiusculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="39b41-170">That setting defaults to case-insensitive.</span></span> <span data-ttu-id="39b41-171">Você poderia chamar o `ToUpper` método para fazer o teste explicitamente maiusculas de minúsculas: *onde (s = > s.LastName.ToUpper(). Contains(searchString.ToUpper())*.</span><span class="sxs-lookup"><span data-stu-id="39b41-171">You could call the `ToUpper` method to make the test explicitly case-insensitive:  *Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())*.</span></span> <span data-ttu-id="39b41-172">Garantiria que resultados permaneçam o mesmo se você alterar o código mais tarde para usar um repositório que retorna um `IEnumerable` coleção em vez de um `IQueryable` objeto.</span><span class="sxs-lookup"><span data-stu-id="39b41-172">That would ensure that results stay the same if you change the code later to use a repository which returns   an `IEnumerable` collection instead of an `IQueryable` object.</span></span> <span data-ttu-id="39b41-173">(Quando você chama o `Contains` método em um `IEnumerable` coleção, você obtém a implementação do .NET Framework; quando chamá-lo em um `IQueryable` do objeto, você obtém a implementação de provedor de banco de dados.) No entanto, há uma penalidade de desempenho para essa solução.</span><span class="sxs-lookup"><span data-stu-id="39b41-173">(When you call the `Contains` method on an `IEnumerable` collection, you get the .NET Framework implementation; when you call it on an `IQueryable` object, you get the database provider implementation.) However, there's a performance penalty for this solution.</span></span> <span data-ttu-id="39b41-174">O `ToUpper` código colocar uma função na cláusula WHERE da instrução SELECT TSQL.</span><span class="sxs-lookup"><span data-stu-id="39b41-174">The `ToUpper` code would put a function in the WHERE clause of the TSQL SELECT statement.</span></span> <span data-ttu-id="39b41-175">Que possam impedir que o otimizador de uso de um índice.</span><span class="sxs-lookup"><span data-stu-id="39b41-175">That would prevent the optimizer from using an index.</span></span> <span data-ttu-id="39b41-176">Considerando que o SQL geralmente é instalado como maiusculas e minúsculas, é melhor evitar o `ToUpper` código até você migrar para um repositório de dados diferencia maiusculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="39b41-176">Given that SQL is mostly installed as case-insensitive, it's best to avoid the `ToUpper` code until you migrate to a case-sensitive data store.</span></span>

### <a name="add-a-search-box-to-the-student-index-view"></a><span data-ttu-id="39b41-177">Adicione uma caixa de pesquisa para o modo de exibição de índice do aluno</span><span class="sxs-lookup"><span data-stu-id="39b41-177">Add a Search Box to the Student Index View</span></span>

<span data-ttu-id="39b41-178">Em *Views/Student/Index.cshtml*, adicione o código realçado imediatamente antes da abertura tag da tabela para criar uma legenda, uma caixa de texto e um **pesquisa** botão.</span><span class="sxs-lookup"><span data-stu-id="39b41-178">In *Views/Student/Index.cshtml*, add the highlighted code immediately before the opening table tag in order to create a caption, a text box, and a **Search** button.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index3.cshtml?range=9-23&highlight=5-13)]

<span data-ttu-id="39b41-179">Esse código usa o `<form>` [auxiliar de marca](xref:mvc/views/tag-helpers/intro) para adicionar a caixa de texto de pesquisa e o botão.</span><span class="sxs-lookup"><span data-stu-id="39b41-179">This code uses the `<form>` [tag helper](xref:mvc/views/tag-helpers/intro) to add the search text box and button.</span></span> <span data-ttu-id="39b41-180">Por padrão, o `<form>` auxiliar de marca envia dados de formulário com uma POSTAGEM, o que significa que parâmetros são passados no corpo da mensagem HTTP e não na URL como cadeias de caracteres de consulta.</span><span class="sxs-lookup"><span data-stu-id="39b41-180">By default, the `<form>` tag helper submits form data with a POST, which means that parameters are passed in the HTTP message body and not in the URL as query strings.</span></span> <span data-ttu-id="39b41-181">Quando você especificar HTTP GET, os dados do formulário são passados na URL como cadeias de caracteres de consulta, que permite aos usuários indicar a URL.</span><span class="sxs-lookup"><span data-stu-id="39b41-181">When you specify HTTP GET, the form data is passed in the URL as query strings, which enables users to bookmark the URL.</span></span> <span data-ttu-id="39b41-182">É recomendável o W3C diretrizes que você deve usar obter quando a ação não resulta em uma atualização.</span><span class="sxs-lookup"><span data-stu-id="39b41-182">The W3C guidelines recommend that you should use GET when the action doesn't result in an update.</span></span>

<span data-ttu-id="39b41-183">Executar o aplicativo, selecione o **alunos** guia, insira uma cadeia de caracteres de pesquisa e clique em Pesquisar para verificar se a filtragem está funcionando.</span><span class="sxs-lookup"><span data-stu-id="39b41-183">Run the app, select the **Students** tab, enter a search string, and click Search to verify that filtering is working.</span></span>

![Página de índice de alunos com filtragem](sort-filter-page/_static/filtering.png)

<span data-ttu-id="39b41-185">Observe que a URL contém a cadeia de caracteres de pesquisa.</span><span class="sxs-lookup"><span data-stu-id="39b41-185">Notice that the URL contains the search string.</span></span>

```html
http://localhost:5813/Students?SearchString=an
```

<span data-ttu-id="39b41-186">Se você marcar essa página, você obterá a lista filtrada quando você usa o indicador.</span><span class="sxs-lookup"><span data-stu-id="39b41-186">If you bookmark this page, you'll get the filtered list when you use the bookmark.</span></span> <span data-ttu-id="39b41-187">Adicionando `method="get"` para o `form` marca é o que causou a cadeia de caracteres de consulta a ser gerado.</span><span class="sxs-lookup"><span data-stu-id="39b41-187">Adding `method="get"` to the `form` tag is what caused the query string to be generated.</span></span>

<span data-ttu-id="39b41-188">Neste estágio, se você clicar em um link de classificação de cabeçalho de coluna perderá o valor do filtro que você inseriu no **pesquisa** caixa.</span><span class="sxs-lookup"><span data-stu-id="39b41-188">At this stage, if you click a column heading sort link you'll lose the filter value that you entered in the **Search** box.</span></span> <span data-ttu-id="39b41-189">Isso será corrigido na próxima seção.</span><span class="sxs-lookup"><span data-stu-id="39b41-189">You'll fix that in the next section.</span></span>

## <a name="add-paging-functionality-to-the-students-index-page"></a><span data-ttu-id="39b41-190">Adicionar a funcionalidade de paginação para a página de índice de alunos</span><span class="sxs-lookup"><span data-stu-id="39b41-190">Add paging functionality to the Students Index page</span></span>

<span data-ttu-id="39b41-191">Para adicionar a paginação para a página de índice de alunos, você criará uma `PaginatedList` classe que usa `Skip` e `Take` instruções para filtrar os dados no servidor em vez de recuperar sempre todas as linhas da tabela.</span><span class="sxs-lookup"><span data-stu-id="39b41-191">To add paging to the Students Index page, you'll create a `PaginatedList` class that uses `Skip` and `Take` statements to filter data on the server instead of always retrieving all rows of the table.</span></span> <span data-ttu-id="39b41-192">Em seguida, você poderá fazer alterações adicionais no `Index` método e adicione botões de paginação para o `Index` exibição.</span><span class="sxs-lookup"><span data-stu-id="39b41-192">Then you'll make additional changes in the `Index` method and add paging buttons to the `Index` view.</span></span> <span data-ttu-id="39b41-193">A ilustração a seguir mostra os botões de paginação.</span><span class="sxs-lookup"><span data-stu-id="39b41-193">The following illustration shows the paging buttons.</span></span>

![Os alunos índice página com links de paginação](sort-filter-page/_static/paging.png)

<span data-ttu-id="39b41-195">Na pasta do projeto, criar `PaginatedList.cs`e, em seguida, substitua o código de modelo com o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="39b41-195">In the project folder, create `PaginatedList.cs`, and then replace the template code with the following code.</span></span>

[!code-csharp[Main](intro/samples/cu/PaginatedList.cs)]

<span data-ttu-id="39b41-196">O `CreateAsync` método nesse código usa o tamanho da página e o número da página e aplica as `Skip` e `Take` instruções para o `IQueryable`.</span><span class="sxs-lookup"><span data-stu-id="39b41-196">The `CreateAsync` method in this code takes page size and page number and applies the appropriate `Skip` and `Take` statements to the `IQueryable`.</span></span> <span data-ttu-id="39b41-197">Quando `ToListAsync` é chamado de `IQueryable`, ela retornará uma lista que contém somente a página solicitada.</span><span class="sxs-lookup"><span data-stu-id="39b41-197">When `ToListAsync` is called on the `IQueryable`, it will return a List containing only the requested page.</span></span> <span data-ttu-id="39b41-198">As propriedades `HasPreviousPage` e `HasNextPage` pode ser usado para habilitar ou desabilitar **anterior** e **próximo** botões de paginação.</span><span class="sxs-lookup"><span data-stu-id="39b41-198">The properties `HasPreviousPage` and `HasNextPage` can be used to enable or disable **Previous** and **Next** paging buttons.</span></span>

<span data-ttu-id="39b41-199">Um `CreateAsync` método é usado em vez de um construtor para criar o `PaginatedList<T>` porque construtores não podem executar código assíncrono do objeto.</span><span class="sxs-lookup"><span data-stu-id="39b41-199">A `CreateAsync` method is used instead of a constructor to create the `PaginatedList<T>` object because constructors can't run asynchronous code.</span></span>

## <a name="add-paging-functionality-to-the-index-method"></a><span data-ttu-id="39b41-200">Adicionar a funcionalidade de paginação para o método de índice</span><span class="sxs-lookup"><span data-stu-id="39b41-200">Add paging functionality to the Index method</span></span>

<span data-ttu-id="39b41-201">Em *StudentsController.cs*, substitua o `Index` método com o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="39b41-201">In *StudentsController.cs*, replace the `Index` method with the following code.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilterPage&highlight=1-5,7,11-18,45-46)]

<span data-ttu-id="39b41-202">Esse código adiciona um parâmetro de número de página, um parâmetro de ordem de classificação atual e um parâmetro de filtro atual para a assinatura do método.</span><span class="sxs-lookup"><span data-stu-id="39b41-202">This code adds a page number parameter, a current sort order parameter, and a current filter parameter to the method signature.</span></span>

```csharp
public async Task<IActionResult> Index(
    string sortOrder,
    string currentFilter,
    string searchString,
    int? page)
```

<span data-ttu-id="39b41-203">Na primeira vez em que a página é exibida, ou se o usuário ainda não clicou uma paginação ou link de classificação, todos os parâmetros será nulos.</span><span class="sxs-lookup"><span data-stu-id="39b41-203">The first time the page is displayed, or if the user hasn't clicked a paging or sorting link, all the parameters will be null.</span></span>  <span data-ttu-id="39b41-204">Se um link de paginação é clicado, a variável de página conterá o número da página para exibir.</span><span class="sxs-lookup"><span data-stu-id="39b41-204">If a paging link is clicked, the page variable will contain the page number to display.</span></span>

<span data-ttu-id="39b41-205">O `ViewData` elemento chamado CurrentSort fornece a exibição com a ordem de classificação atual, porque isso deve ser incluído nos links de paginação para manter a ordem de classificação igual de paginação.</span><span class="sxs-lookup"><span data-stu-id="39b41-205">The `ViewData` element named CurrentSort provides the view with the current sort order, because this must be included in the paging links in order to keep the sort order the same while paging.</span></span>

<span data-ttu-id="39b41-206">O `ViewData` elemento chamado FiltroAtual fornece a exibição com a cadeia de caracteres do filtro atual.</span><span class="sxs-lookup"><span data-stu-id="39b41-206">The `ViewData` element named CurrentFilter provides the view with the current filter string.</span></span> <span data-ttu-id="39b41-207">Esse valor deve ser incluído nos links de paginação para manter as configurações de filtro durante a paginação e deve ser restaurado para a caixa de texto quando a página é exibida novamente.</span><span class="sxs-lookup"><span data-stu-id="39b41-207">This value must be included in the paging links in order to maintain the filter settings during paging, and it must be restored to the text box when the page is redisplayed.</span></span>

<span data-ttu-id="39b41-208">Se a cadeia de caracteres de pesquisa for alterada durante a paginação, a página deve ser redefinido como 1, porque o novo filtro pode resultar em dados diferentes a serem exibidos.</span><span class="sxs-lookup"><span data-stu-id="39b41-208">If the search string is changed during paging, the page has to be reset to 1, because the new filter can result in different data to display.</span></span> <span data-ttu-id="39b41-209">A cadeia de caracteres de pesquisa é alterada quando um valor é inserido na caixa de texto e o botão de envio é pressionado.</span><span class="sxs-lookup"><span data-stu-id="39b41-209">The search string is changed when a value is entered in the text box and the Submit button is pressed.</span></span> <span data-ttu-id="39b41-210">Nesse caso, o `searchString` parâmetro não é nulo.</span><span class="sxs-lookup"><span data-stu-id="39b41-210">In that case, the `searchString` parameter isn't null.</span></span>

```csharp
if (searchString != null)
{
    page = 1;
}
else
{
    searchString = currentFilter;
}
```

<span data-ttu-id="39b41-211">No final do `Index` método, o `PaginatedList.CreateAsync` método converte a consulta do aluno em uma única página de alunos em um tipo de coleção que oferece suporte à paginação.</span><span class="sxs-lookup"><span data-stu-id="39b41-211">At the end of the `Index` method, the `PaginatedList.CreateAsync` method converts the student query to a single page of students in a collection type that supports paging.</span></span> <span data-ttu-id="39b41-212">Página única de alunos é então passada para o modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="39b41-212">That single page of students is then passed to the view.</span></span>

```csharp
return View(await PaginatedList<Student>.CreateAsync(students.AsNoTracking(), page ?? 1, pageSize));
```

<span data-ttu-id="39b41-213">O `PaginatedList.CreateAsync` método usa um número de página.</span><span class="sxs-lookup"><span data-stu-id="39b41-213">The `PaginatedList.CreateAsync` method takes a page number.</span></span> <span data-ttu-id="39b41-214">Os dois pontos de interrogação representar o operador de união de null.</span><span class="sxs-lookup"><span data-stu-id="39b41-214">The two question marks represent the null-coalescing operator.</span></span> <span data-ttu-id="39b41-215">O operador de união null define um valor padrão para um tipo anulável. a expressão `(page ?? 1)` significa retorna o valor de `page` se ele tem um valor ou retornará 1 se `page` é nulo.</span><span class="sxs-lookup"><span data-stu-id="39b41-215">The null-coalescing operator defines a default value for a nullable type; the expression `(page ?? 1)` means return the value of `page` if it has a value, or return 1 if `page` is null.</span></span>

## <a name="add-paging-links-to-the-student-index-view"></a><span data-ttu-id="39b41-216">Adicionar links de paginação para o modo de exibição do índice do aluno</span><span class="sxs-lookup"><span data-stu-id="39b41-216">Add paging links to the Student Index view</span></span>

<span data-ttu-id="39b41-217">Em *Views/Students/Index.cshtml*, substitua o código existente com o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="39b41-217">In *Views/Students/Index.cshtml*, replace the existing code with the following code.</span></span> <span data-ttu-id="39b41-218">As alterações são realçadas.</span><span class="sxs-lookup"><span data-stu-id="39b41-218">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index.cshtml?highlight=1,27,30,33,61-79)]

<span data-ttu-id="39b41-219">O `@model` instrução na parte superior da página especifica o modo de exibição agora obtém um `PaginatedList<T>` do objeto, em vez de um `List<T>` objeto.</span><span class="sxs-lookup"><span data-stu-id="39b41-219">The `@model` statement at the top of the page specifies that the view now gets a `PaginatedList<T>` object instead of a `List<T>` object.</span></span>

<span data-ttu-id="39b41-220">Os links de cabeçalho de coluna usam a cadeia de caracteres de consulta para passar a cadeia de caracteres de pesquisa atual para o controlador para que o usuário pode classificar nos resultados do filtro:</span><span class="sxs-lookup"><span data-stu-id="39b41-220">The column header links use the query string to pass the current search string to the controller so that the user can sort within filter results:</span></span>

```html
<a asp-action="Index" asp-route-sortOrder="@ViewData["DateSortParm"]" asp-route-currentFilter ="@ViewData["CurrentFilter"]">Enrollment Date</a>
```

<span data-ttu-id="39b41-221">Os botões de paginação são exibidos por auxiliares de marca:</span><span class="sxs-lookup"><span data-stu-id="39b41-221">The paging buttons are displayed by tag helpers:</span></span>

```html
<a asp-action="Index"
   asp-route-sortOrder="@ViewData["CurrentSort"]"
   asp-route-page="@(Model.PageIndex - 1)"
   asp-route-currentFilter="@ViewData["CurrentFilter"]"
   class="btn btn-default @prevDisabled">
   Previous
</a>
```

<span data-ttu-id="39b41-222">Execute o aplicativo e vá para a página de alunos.</span><span class="sxs-lookup"><span data-stu-id="39b41-222">Run the app and go to the Students page.</span></span>

![Os alunos índice página com links de paginação](sort-filter-page/_static/paging.png)

<span data-ttu-id="39b41-224">Clique nos links de paginação em ordens de classificação diferente para tornar-se de que funciona de paginação.</span><span class="sxs-lookup"><span data-stu-id="39b41-224">Click the paging links in different sort orders to make sure paging works.</span></span> <span data-ttu-id="39b41-225">Em seguida, digite uma cadeia de caracteres de pesquisa e tente paginação novamente para confirmar se paginação também funciona corretamente com a classificação e filtragem.</span><span class="sxs-lookup"><span data-stu-id="39b41-225">Then enter a search string and try paging again to verify that paging also works correctly with sorting and filtering.</span></span>

## <a name="create-an-about-page-that-shows-student-statistics"></a><span data-ttu-id="39b41-226">Criar uma página sobre que mostra as estatísticas do aluno</span><span class="sxs-lookup"><span data-stu-id="39b41-226">Create an About page that shows Student statistics</span></span>

<span data-ttu-id="39b41-227">Para o site do Contoso University **sobre** página, você exibirá quantas alunos registrados para cada data de registro.</span><span class="sxs-lookup"><span data-stu-id="39b41-227">For the Contoso University website's **About** page, you'll display how many students have enrolled for each enrollment date.</span></span> <span data-ttu-id="39b41-228">Isso exige cálculos simples e agrupamento nos grupos.</span><span class="sxs-lookup"><span data-stu-id="39b41-228">This requires grouping and simple calculations on the groups.</span></span> <span data-ttu-id="39b41-229">Para fazer isso, você fará o seguinte:</span><span class="sxs-lookup"><span data-stu-id="39b41-229">To accomplish this, you'll do the following:</span></span>

* <span data-ttu-id="39b41-230">Crie uma classe de modelo de exibição para os dados que você precisa passar para o modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="39b41-230">Create a view model class for the data that you need to pass to the view.</span></span>

* <span data-ttu-id="39b41-231">Modifique o método sobre no controlador Home.</span><span class="sxs-lookup"><span data-stu-id="39b41-231">Modify the About method in the Home controller.</span></span>

* <span data-ttu-id="39b41-232">Modificar o modo de exibição sobre.</span><span class="sxs-lookup"><span data-stu-id="39b41-232">Modify the About view.</span></span>

### <a name="create-the-view-model"></a><span data-ttu-id="39b41-233">Criar o modelo de exibição</span><span class="sxs-lookup"><span data-stu-id="39b41-233">Create the view model</span></span>

<span data-ttu-id="39b41-234">Criar um *SchoolViewModels* pasta o *modelos* pasta.</span><span class="sxs-lookup"><span data-stu-id="39b41-234">Create a *SchoolViewModels* folder in the *Models* folder.</span></span>

<span data-ttu-id="39b41-235">Na nova pasta, adicionar um arquivo de classe *EnrollmentDateGroup.cs* e substitua o código de modelo com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="39b41-235">In the new folder, add a class file *EnrollmentDateGroup.cs* and replace the template code with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="modify-the-home-controller"></a><span data-ttu-id="39b41-236">Modificar controlador principal</span><span class="sxs-lookup"><span data-stu-id="39b41-236">Modify the Home Controller</span></span>

<span data-ttu-id="39b41-237">Em *HomeController*, adicione o seguinte usando instruções na parte superior do arquivo:</span><span class="sxs-lookup"><span data-stu-id="39b41-237">In *HomeController.cs*, add the following using statements at the top of the file:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings1)]

<span data-ttu-id="39b41-238">Adicionar uma variável de classe para o contexto do banco de dados imediatamente após a chave de abertura para a classe e obter uma instância do contexto do ASP.NET Core DI:</span><span class="sxs-lookup"><span data-stu-id="39b41-238">Add a class variable for the database context immediately after the opening curly brace for the class, and get an instance of the context from ASP.NET Core DI:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_AddContext&highlight=3,5,7)]

<span data-ttu-id="39b41-239">Substitua o método `About` pelo seguinte código:</span><span class="sxs-lookup"><span data-stu-id="39b41-239">Replace the `About` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseDbSet)]

<span data-ttu-id="39b41-240">A instrução LINQ agrupa as entidades de alunos por data de inscrição, calcula o número de entidades em cada grupo e armazena os resultados em uma coleção de `EnrollmentDateGroup` exibir objetos de modelo.</span><span class="sxs-lookup"><span data-stu-id="39b41-240">The LINQ statement groups the student entities by enrollment date, calculates the number of entities in each group, and stores the results in a collection of `EnrollmentDateGroup` view model objects.</span></span>
> [!NOTE] 
> <span data-ttu-id="39b41-241">Na 1.0 versão do Entity Framework Core, o conjunto de resultados inteiro será retornado ao cliente e agrupamento é feito no cliente.</span><span class="sxs-lookup"><span data-stu-id="39b41-241">In the 1.0 version of Entity Framework Core, the entire result set is returned to the client, and grouping is done on the client.</span></span> <span data-ttu-id="39b41-242">Em alguns cenários, isso pode criar problemas de desempenho.</span><span class="sxs-lookup"><span data-stu-id="39b41-242">In some scenarios this could create performance problems.</span></span> <span data-ttu-id="39b41-243">Certifique-se de testar o desempenho com os volumes de dados de produção e, se for necessário usar SQL bruto para fazer o agrupamento do servidor.</span><span class="sxs-lookup"><span data-stu-id="39b41-243">Be sure to test performance with production volumes of data, and if necessary use raw SQL to do the grouping on the server.</span></span> <span data-ttu-id="39b41-244">Para obter informações sobre como usar o SQL não processada, consulte [o último tutorial nesta série](advanced.md).</span><span class="sxs-lookup"><span data-stu-id="39b41-244">For information about how to use raw SQL, see [the last tutorial in this series](advanced.md).</span></span>

### <a name="modify-the-about-view"></a><span data-ttu-id="39b41-245">Modificar a sobre o modo de exibição</span><span class="sxs-lookup"><span data-stu-id="39b41-245">Modify the About View</span></span>

<span data-ttu-id="39b41-246">Substitua o código no *Views/Home/About.cshtml* arquivo com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="39b41-246">Replace the code in the *Views/Home/About.cshtml* file with the following code:</span></span>

[!code-html[](intro/samples/cu/Views/Home/About.cshtml)]

<span data-ttu-id="39b41-247">Execute o aplicativo e vá para a página sobre.</span><span class="sxs-lookup"><span data-stu-id="39b41-247">Run the app and go to the About page.</span></span> <span data-ttu-id="39b41-248">A contagem de alunos para cada data de registro é exibida em uma tabela.</span><span class="sxs-lookup"><span data-stu-id="39b41-248">The count of students for each enrollment date is displayed in a table.</span></span>

![Sobre a página](sort-filter-page/_static/about.png)

## <a name="summary"></a><span data-ttu-id="39b41-250">Resumo</span><span class="sxs-lookup"><span data-stu-id="39b41-250">Summary</span></span>

<span data-ttu-id="39b41-251">Neste tutorial, você viu como realizar a classificação, filtragem, paginação e agrupamento.</span><span class="sxs-lookup"><span data-stu-id="39b41-251">In this tutorial, you've seen how to perform sorting, filtering, paging, and grouping.</span></span> <span data-ttu-id="39b41-252">O seguinte tutorial, você aprenderá como lidar com as alterações do modelo de dados usando as migrações.</span><span class="sxs-lookup"><span data-stu-id="39b41-252">In the next tutorial, you'll learn how to handle data model changes by using migrations.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="39b41-253">[Anterior](crud.md)
[Próximo](migrations.md)</span><span class="sxs-lookup"><span data-stu-id="39b41-253">[Previous](crud.md)
[Next](migrations.md)</span></span>  
