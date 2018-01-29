---
title: "Núcleo do ASP.NET MVC com núcleo EF - avançado - 10 de 10"
author: tdykstra
description: "Este tutorial apresenta vários tópicos que são úteis a serem consideradas quando você vá além do básico do desenvolvimento de aplicativos web ASP.NET que usam o Entity Framework Core."
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/advanced
ms.openlocfilehash: b1d1cb8710595acd72bd5a0786bf1b1fed8b1d79
ms.sourcegitcommit: 09b342b45e7372ba9ebf17f35eee331e5a08fb26
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/26/2018
---
# <a name="advanced-topics---ef-core-with-aspnet-core-mvc-tutorial-10-of-10"></a><span data-ttu-id="7dd0f-103">Tópicos avançados - Core de EF com o tutorial do MVC do ASP.NET Core (10 de 10)</span><span class="sxs-lookup"><span data-stu-id="7dd0f-103">Advanced topics - EF Core with ASP.NET Core MVC tutorial (10 of 10)</span></span>

<span data-ttu-id="7dd0f-104">Por [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7dd0f-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7dd0f-105">O aplicativo web de exemplo Contoso University demonstra como criar aplicativos do ASP.NET MVC de núcleo da web usando o Entity Framework Core e o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="7dd0f-106">Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial na série](intro.md).</span><span class="sxs-lookup"><span data-stu-id="7dd0f-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="7dd0f-107">No tutorial anterior, você implementou herança de tabela por hierarquia.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-107">In the previous tutorial, you implemented table-per-hierarchy inheritance.</span></span> <span data-ttu-id="7dd0f-108">Este tutorial apresenta vários tópicos que são úteis a serem consideradas quando você vá além do básico do desenvolvimento de aplicativos web ASP.NET Core que usam o Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-108">This tutorial introduces several topics that are useful to be aware of when you go beyond the basics of developing ASP.NET Core web applications that use Entity Framework Core.</span></span>

## <a name="raw-sql-queries"></a><span data-ttu-id="7dd0f-109">Consultas SQL bruto</span><span class="sxs-lookup"><span data-stu-id="7dd0f-109">Raw SQL Queries</span></span>

<span data-ttu-id="7dd0f-110">Uma das vantagens de usar o Entity Framework é que ela evita vincular seu código muito semelhante a um método específico de armazenamento de dados.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-110">One of the advantages of using the Entity Framework is that it avoids tying your code too closely to a particular method of storing data.</span></span> <span data-ttu-id="7dd0f-111">Ele faz isso através da geração de consultas SQL e comandos para você, que também libera você da necessidade de gravá-los.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-111">It does this by generating SQL queries and commands for you, which also frees you from having to write them yourself.</span></span> <span data-ttu-id="7dd0f-112">Mas há casos excepcionais, quando você precisa executar consultas específicas de SQL que você criou manualmente.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-112">But there are exceptional scenarios when you need to run specific SQL queries that you have manually created.</span></span> <span data-ttu-id="7dd0f-113">Para esses cenários, a API do Entity Framework código primeiro inclui métodos que permitem que você passe comandos SQL diretamente para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-113">For these scenarios, the Entity Framework Code First API includes methods that enable you to pass SQL commands directly to the database.</span></span> <span data-ttu-id="7dd0f-114">Você tem as seguintes opções no EF Core 1.0:</span><span class="sxs-lookup"><span data-stu-id="7dd0f-114">You have the following options in EF Core 1.0:</span></span>

* <span data-ttu-id="7dd0f-115">Use o `DbSet.FromSql` método para consultas que retornam tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-115">Use the `DbSet.FromSql` method for queries that return entity types.</span></span> <span data-ttu-id="7dd0f-116">Os objetos retornados devem ser do tipo esperado pelo `DbSet` objeto e eles automaticamente estiverem controladas pelo contexto de banco de dados, a menos que você [desativar rastreamento](crud.md#no-tracking-queries).</span><span class="sxs-lookup"><span data-stu-id="7dd0f-116">The returned objects must be of the type expected by the `DbSet` object, and they're automatically tracked by the database context unless you [turn tracking off](crud.md#no-tracking-queries).</span></span>

* <span data-ttu-id="7dd0f-117">Use o `Database.ExecuteSqlCommand` para comandos sem consulta.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-117">Use the `Database.ExecuteSqlCommand` for non-query commands.</span></span>

<span data-ttu-id="7dd0f-118">Se você precisar executar uma consulta que retorna tipos que não são entidades, você pode usar o ADO.NET com a conexão de banco de dados fornecido pelo EF.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-118">If you need to run a query that returns types that aren't entities, you can use ADO.NET with the database connection provided by EF.</span></span> <span data-ttu-id="7dd0f-119">Os dados retornados não são controlados pelo contexto de banco de dados, mesmo se você usar esse método para recuperar tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-119">The returned data isn't tracked by the database context, even if you use this method to retrieve entity types.</span></span>

<span data-ttu-id="7dd0f-120">Como sempre é verdadeiro quando você executa comandos SQL em um aplicativo web, você deve tomar precauções para proteger o site contra ataques de injeção de SQL.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-120">As is always true when you execute SQL commands in a web application, you must take precautions to protect your site against SQL injection attacks.</span></span> <span data-ttu-id="7dd0f-121">Uma maneira de fazer isso é usar consultas parametrizadas para certificar-se de que as cadeias de caracteres enviadas por uma página da web não podem ser interpretadas como comandos SQL.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-121">One way to do that is to use parameterized queries to make sure that strings submitted by a web page can't be interpreted as SQL commands.</span></span> <span data-ttu-id="7dd0f-122">Neste tutorial você usará consultas parametrizadas ao integrar a entrada do usuário em uma consulta.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-122">In this tutorial you'll use parameterized queries when integrating user input into a query.</span></span>

## <a name="call-a-query-that-returns-entities"></a><span data-ttu-id="7dd0f-123">Chamar uma consulta que retorna entidades</span><span class="sxs-lookup"><span data-stu-id="7dd0f-123">Call a query that returns entities</span></span>

<span data-ttu-id="7dd0f-124">O `DbSet<TEntity>` classe fornece um método que você pode usar para executar uma consulta que retorna uma entidade do tipo `TEntity`.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-124">The `DbSet<TEntity>` class provides a method that you can use to execute a query that returns an entity of type `TEntity`.</span></span> <span data-ttu-id="7dd0f-125">Para ver como isso funciona, alterará o código de `Details` método do controlador de departamento.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-125">To see how this works you'll change the code in the `Details` method of the Department controller.</span></span>

<span data-ttu-id="7dd0f-126">Em *DepartmentsController.cs*, no `Details` método, substitua o código que recupera um departamento com um `FromSql` chamada de método, como mostra o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="7dd0f-126">In *DepartmentsController.cs*, in the `Details` method, replace the code that retrieves a department with a `FromSql` method call, as shown in the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_RawSQL&highlight=8,9,10,13)]

<span data-ttu-id="7dd0f-127">Para verificar se o novo código funciona corretamente, selecione o **departamentos** guia e, em seguida, **detalhes** para um dos departamentos.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-127">To verify that the new code works correctly, select the **Departments** tab and then **Details** for one of the departments.</span></span>

![Detalhes do departamento](advanced/_static/department-details.png)

## <a name="call-a-query-that-returns-other-types"></a><span data-ttu-id="7dd0f-129">Chamar uma consulta que retorna os outros tipos</span><span class="sxs-lookup"><span data-stu-id="7dd0f-129">Call a query that returns other types</span></span>

<span data-ttu-id="7dd0f-130">Anteriormente, você criou uma grade de estatísticas de estudante para a página sobre que mostrou o número de alunos para cada data de registro.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-130">Earlier you created a student statistics grid for the About page that showed the number of students for each enrollment date.</span></span> <span data-ttu-id="7dd0f-131">Você obteve os dados do conjunto de entidades de alunos (`_context.Students`) e usada LINQ para projetar os resultados em uma lista de `EnrollmentDateGroup` exibir objetos de modelo.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-131">You got the data from the Students entity set (`_context.Students`) and used LINQ to project the results into a list of `EnrollmentDateGroup` view model objects.</span></span> <span data-ttu-id="7dd0f-132">Suponha que você deseja gravar o SQL em vez de usar o LINQ.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-132">Suppose you want to write the SQL itself rather than using LINQ.</span></span> <span data-ttu-id="7dd0f-133">Para fazer o que você precisa executar uma consulta SQL que retorna algo diferente de objetos de entidade.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-133">To do that you need to run a SQL query that returns something other than entity objects.</span></span> <span data-ttu-id="7dd0f-134">No EF Core 1.0, uma maneira de fazer isso é escrever código ADO.NET e obter a conexão de banco de dados do EF.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-134">In EF Core 1.0, one way to do that is write ADO.NET code and get the database connection from EF.</span></span>

<span data-ttu-id="7dd0f-135">Em *HomeController*, substitua o `About` método com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="7dd0f-135">In *HomeController.cs*, replace the `About` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseRawSQL&highlight=3-32)]

<span data-ttu-id="7dd0f-136">Adicionar um uso instrução:</span><span class="sxs-lookup"><span data-stu-id="7dd0f-136">Add a using statement:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings2)]

<span data-ttu-id="7dd0f-137">Execute o aplicativo e vá para a página sobre.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-137">Run the app and go to the About page.</span></span> <span data-ttu-id="7dd0f-138">Ele exibe os mesmos dados que antes.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-138">It displays the same data it did before.</span></span>

![Sobre a página](advanced/_static/about.png)

## <a name="call-an-update-query"></a><span data-ttu-id="7dd0f-140">Chamar uma consulta update</span><span class="sxs-lookup"><span data-stu-id="7dd0f-140">Call an update query</span></span>

<span data-ttu-id="7dd0f-141">Suponha que os administradores de Contoso University desejam executar alterações globais no banco de dados, como alterar o número de créditos para cada curso.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-141">Suppose Contoso University administrators want to perform global changes in the database, such as changing the number of credits for every course.</span></span> <span data-ttu-id="7dd0f-142">Se a university tiver um grande número de cursos, poderá ser ineficiente para recuperá-las como entidades e alterá-los individualmente.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-142">If the university has a large number of courses, it would be inefficient to retrieve them all as entities and change them individually.</span></span> <span data-ttu-id="7dd0f-143">Nesta seção, você implementará uma página da web que permite que o usuário especifique um fator alterar o número de créditos para todos os cursos e fará a alteração, executando uma instrução SQL UPDATE.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-143">In this section you'll implement a web page that enables the user to specify a factor by which to change the number of credits for all courses, and you'll make the change by executing a SQL UPDATE statement.</span></span> <span data-ttu-id="7dd0f-144">A página da web será semelhante a ilustração a seguir:</span><span class="sxs-lookup"><span data-stu-id="7dd0f-144">The web page will look like the following illustration:</span></span>

![Página de atualização de curso créditos](advanced/_static/update-credits.png)

<span data-ttu-id="7dd0f-146">Em *CoursesContoller.cs*, adicionar métodos UpdateCourseCredits para HttpGet e HttpPost:</span><span class="sxs-lookup"><span data-stu-id="7dd0f-146">In *CoursesContoller.cs*, add UpdateCourseCredits methods for HttpGet and HttpPost:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdateGet)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdatePost)]

<span data-ttu-id="7dd0f-147">Quando o controlador processa uma solicitação HttpGet, nada será retornado no `ViewData["RowsAffected"]`, e o modo de exibição exibe uma caixa de texto e um botão de envio, conforme mostrado na ilustração anterior.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-147">When the controller processes an HttpGet request, nothing is returned in `ViewData["RowsAffected"]`, and the view displays an empty text box and a submit button, as shown in the preceding illustration.</span></span>

<span data-ttu-id="7dd0f-148">Quando o **atualização** botão é clicado, o método HttpPost é chamado e multiplicador tem o valor inserido na caixa de texto.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-148">When the **Update** button is clicked, the HttpPost method is called, and multiplier has the value entered in the text box.</span></span> <span data-ttu-id="7dd0f-149">O código, em seguida, executa o SQL que atualiza os cursos e retorna o número de linhas afetadas para o modo de exibição `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-149">The code then executes the SQL that updates courses and returns the number of affected rows to the view in `ViewData`.</span></span> <span data-ttu-id="7dd0f-150">Quando o modo de exibição obtém uma `RowsAffected` valor, ele exibe o número de linhas atualizadas.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-150">When the view gets a `RowsAffected` value, it displays the number of rows updated.</span></span>

<span data-ttu-id="7dd0f-151">Em **Gerenciador de soluções**, com o botão direito do *exibições/cursos* pasta e clique **Adicionar > Novo Item**.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-151">In **Solution Explorer**, right-click the *Views/Courses* folder, and then click **Add > New Item**.</span></span>

<span data-ttu-id="7dd0f-152">No **Adicionar Novo Item** caixa de diálogo, clique em **ASP.NET** em **instalado** no painel esquerdo, clique em **exibir página MVC**e nomeie o novo modo de exibição  *UpdateCourseCredits.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-152">In the **Add New Item** dialog, click **ASP.NET** under **Installed** in the left pane, click **MVC View Page**, and name the new view *UpdateCourseCredits.cshtml*.</span></span>

<span data-ttu-id="7dd0f-153">Em *Views/Courses/UpdateCourseCredits.cshtml*, substitua o código de modelo com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="7dd0f-153">In *Views/Courses/UpdateCourseCredits.cshtml*, replace the template code with the following code:</span></span>

[!code-html[Main](intro/samples/cu/Views/Courses/UpdateCourseCredits.cshtml)]

<span data-ttu-id="7dd0f-154">Execute o `UpdateCourseCredits` método selecionando o **cursos** guia, em seguida, adicionando "/ UpdateCourseCredits" ao final da URL na barra de endereços do navegador (por exemplo: `http://localhost:5813/Courses/UpdateCourseCredits`).</span><span class="sxs-lookup"><span data-stu-id="7dd0f-154">Run the `UpdateCourseCredits` method by selecting the **Courses** tab, then adding "/UpdateCourseCredits" to the end of the URL in the browser's address bar (for example: `http://localhost:5813/Courses/UpdateCourseCredits`).</span></span> <span data-ttu-id="7dd0f-155">Insira um número na caixa de texto:</span><span class="sxs-lookup"><span data-stu-id="7dd0f-155">Enter a number in the text box:</span></span>

![Página de atualização de curso créditos](advanced/_static/update-credits.png)

<span data-ttu-id="7dd0f-157">Clique em **Atualizar**.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-157">Click **Update**.</span></span> <span data-ttu-id="7dd0f-158">Você ver o número de linhas afetadas:</span><span class="sxs-lookup"><span data-stu-id="7dd0f-158">You see the number of rows affected:</span></span>

![Página de atualização curso créditos linhas afetadas](advanced/_static/update-credits-rows-affected.png)

<span data-ttu-id="7dd0f-160">Clique em **voltar para a lista** para ver a lista de cursos com o número de créditos de revisado.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-160">Click **Back to List** to see the list of courses with the revised number of credits.</span></span>

<span data-ttu-id="7dd0f-161">Observe que o código de produção deve assegurar que sempre atualiza resultar em dados válidos.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-161">Note that production code would ensure that updates always result in valid data.</span></span> <span data-ttu-id="7dd0f-162">O código simplificado mostrado aqui pode multiplique o número de créditos suficiente resultar em um número maior que 5.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-162">The simplified code shown here could multiply the number of credits enough to result in numbers greater than 5.</span></span> <span data-ttu-id="7dd0f-163">(O `Credits` propriedade tem um `[Range(0, 5)]` atributo.) Consulta update funcionaria, mas os dados inválidos podem causar resultados inesperados em outras partes do sistema que assumem que o número de créditos é 5 ou menos.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-163">(The `Credits` property has a `[Range(0, 5)]` attribute.) The update query would work but the invalid data could cause unexpected results in other parts of the system that assume the number of credits is 5 or less.</span></span>

<span data-ttu-id="7dd0f-164">Para obter mais informações sobre consultas SQL brutas, consulte [bruto consultas de SQL](https://docs.microsoft.com/ef/core/querying/raw-sql).</span><span class="sxs-lookup"><span data-stu-id="7dd0f-164">For more information about raw SQL queries, see [Raw SQL Queries](https://docs.microsoft.com/ef/core/querying/raw-sql).</span></span>

## <a name="examine-sql-sent-to-the-database"></a><span data-ttu-id="7dd0f-165">Examine SQL enviada ao banco de dados</span><span class="sxs-lookup"><span data-stu-id="7dd0f-165">Examine SQL sent to the database</span></span>

<span data-ttu-id="7dd0f-166">Às vezes é útil ser capaz de ver as consultas SQL reais que são enviadas para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-166">Sometimes it's helpful to be able to see the actual SQL queries that are sent to the database.</span></span> <span data-ttu-id="7dd0f-167">Funcionalidade de registro em log internos para ASP.NET Core será usada automaticamente pelo EF principais para gravar logs que contêm o SQL para consultas e atualizações.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-167">Built-in logging functionality for ASP.NET Core is automatically used by EF Core to write logs that contain the SQL for queries and updates.</span></span> <span data-ttu-id="7dd0f-168">Nesta seção, você verá alguns exemplos de log SQL.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-168">In this section you'll see some examples of SQL logging.</span></span>

<span data-ttu-id="7dd0f-169">Abra *StudentsController.cs* e no `Details` método defina um ponto de interrupção a `if (student == null)` instrução.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-169">Open *StudentsController.cs* and in the `Details` method set a breakpoint on the `if (student == null)` statement.</span></span>

<span data-ttu-id="7dd0f-170">Executar o aplicativo no modo de depuração e vá para a página de detalhes para um aluno.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-170">Run the app in debug mode, and go to the Details page for a student.</span></span>

<span data-ttu-id="7dd0f-171">Vá para o **saída** mostrando a depuração de janela de saída, e você vê a consulta:</span><span class="sxs-lookup"><span data-stu-id="7dd0f-171">Go to the **Output** window showing debug output, and you see the query:</span></span>

```
Microsoft.EntityFrameworkCore.Database.Command:Information: Executed DbCommand (56ms) [Parameters=[@__id_0='?'], CommandType='Text', CommandTimeout='30']
SELECT TOP(2) [s].[ID], [s].[Discriminator], [s].[FirstName], [s].[LastName], [s].[EnrollmentDate]
FROM [Person] AS [s]
WHERE ([s].[Discriminator] = N'Student') AND ([s].[ID] = @__id_0)
ORDER BY [s].[ID]
Microsoft.EntityFrameworkCore.Database.Command:Information: Executed DbCommand (122ms) [Parameters=[@__id_0='?'], CommandType='Text', CommandTimeout='30']
SELECT [s.Enrollments].[EnrollmentID], [s.Enrollments].[CourseID], [s.Enrollments].[Grade], [s.Enrollments].[StudentID], [e.Course].[CourseID], [e.Course].[Credits], [e.Course].[DepartmentID], [e.Course].[Title]
FROM [Enrollment] AS [s.Enrollments]
INNER JOIN [Course] AS [e.Course] ON [s.Enrollments].[CourseID] = [e.Course].[CourseID]
INNER JOIN (
    SELECT TOP(1) [s0].[ID]
    FROM [Person] AS [s0]
    WHERE ([s0].[Discriminator] = N'Student') AND ([s0].[ID] = @__id_0)
    ORDER BY [s0].[ID]
) AS [t] ON [s.Enrollments].[StudentID] = [t].[ID]
ORDER BY [t].[ID]
```

<span data-ttu-id="7dd0f-172">Você observará alguma coisa que pode ser surpreendente: o SQL seleciona linhas até 2 (`TOP(2)`) da tabela Person.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-172">You'll notice something here that might surprise you: the SQL selects up to 2 rows (`TOP(2)`) from the Person table.</span></span> <span data-ttu-id="7dd0f-173">O `SingleOrDefaultAsync` método não resolve a 1 linha no servidor.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-173">The `SingleOrDefaultAsync` method doesn't resolve to 1 row on the server.</span></span> <span data-ttu-id="7dd0f-174">Eis o porquê:</span><span class="sxs-lookup"><span data-stu-id="7dd0f-174">Here's why:</span></span>

* <span data-ttu-id="7dd0f-175">Se a consulta retornaria várias linhas, o método retornará nulo.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-175">If the query would return multiple rows, the method returns null.</span></span>
* <span data-ttu-id="7dd0f-176">Para determinar se a consulta retornaria várias linhas, EF tem que verificar se ele retorna pelo menos 2.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-176">To determine whether the query would return multiple rows, EF has to check if it returns at least 2.</span></span>

<span data-ttu-id="7dd0f-177">Observe que você não precisa usar o modo de depuração e parar no ponto de interrupção para obter saída de log no **saída** janela.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-177">Note that you don't have to use debug mode and stop at a breakpoint to get logging output in the **Output** window.</span></span> <span data-ttu-id="7dd0f-178">É um modo conveniente para parar o log no ponto em que você deseja examinar a saída.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-178">It's just a convenient way to stop the logging at the point you want to look at the output.</span></span> <span data-ttu-id="7dd0f-179">Se você não fizer isso, o registro em log continuará e você precise rolar para baixo para localizar as partes do que seu interesse.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-179">If you don't do that, logging continues and you have to scroll back to find the parts you're interested in.</span></span>

## <a name="repository-and-unit-of-work-patterns"></a><span data-ttu-id="7dd0f-180">Repositório e unidade de padrões de trabalho</span><span class="sxs-lookup"><span data-stu-id="7dd0f-180">Repository and unit of work patterns</span></span>

<span data-ttu-id="7dd0f-181">Muitos desenvolvedores escrevem código para implementar o repositório e a unidade de padrões de trabalho como um wrapper em torno de código que funciona com o Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-181">Many developers write code to implement the repository and unit of work patterns as a wrapper around code that works with the Entity Framework.</span></span> <span data-ttu-id="7dd0f-182">Esses padrões destinam-se para criar uma camada de abstração entre a camada de acesso a dados e a camada de lógica comercial de um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-182">These patterns are intended to create an abstraction layer between the data access layer and the business logic layer of an application.</span></span> <span data-ttu-id="7dd0f-183">Implementando esses padrões podem ajudar a isolar seu aplicativo de alterações no repositório de dados e pode facilitar o teste de unidade automatizado ou desenvolvimento controlado por testes (TDD).</span><span class="sxs-lookup"><span data-stu-id="7dd0f-183">Implementing these patterns can help insulate your application from changes in the data store and can facilitate automated unit testing or test-driven development (TDD).</span></span> <span data-ttu-id="7dd0f-184">No entanto, gravar código adicional para implementar esses padrões nem sempre é a melhor escolha para aplicativos que usam o EF, por vários motivos:</span><span class="sxs-lookup"><span data-stu-id="7dd0f-184">However, writing additional code to implement these patterns isn't always the best choice for applications that use EF, for several reasons:</span></span>

* <span data-ttu-id="7dd0f-185">A própria classe de contexto EF protege seu código de código específico do repositório de dados.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-185">The EF context class itself insulates your code from data-store-specific code.</span></span>

* <span data-ttu-id="7dd0f-186">A classe de contexto EF pode agir como uma classe de unidade de trabalho para o banco de dados de atualizações que você faça usando EF.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-186">The EF context class can act as a unit-of-work class for database updates that you do using EF.</span></span>

* <span data-ttu-id="7dd0f-187">EF inclui recursos para implementar TDD sem escrever código de repositório.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-187">EF includes features for implementing TDD without writing repository code.</span></span>

<span data-ttu-id="7dd0f-188">Para obter informações sobre como implementar o repositório e a unidade de padrões de trabalho, consulte [a versão do Entity Framework 5 desta série de tutoriais](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application).</span><span class="sxs-lookup"><span data-stu-id="7dd0f-188">For information about how to implement the repository and unit of work patterns, see [the Entity Framework 5 version of this tutorial series](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application).</span></span>

<span data-ttu-id="7dd0f-189">Entity Framework Core implementa um provedor de banco de dados na memória que pode ser usado para teste.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-189">Entity Framework Core implements an in-memory database provider that can be used for testing.</span></span> <span data-ttu-id="7dd0f-190">Para obter mais informações, consulte [testes com InMemory](https://docs.microsoft.com/ef/core/miscellaneous/testing/in-memory).</span><span class="sxs-lookup"><span data-stu-id="7dd0f-190">For more information, see [Testing with InMemory](https://docs.microsoft.com/ef/core/miscellaneous/testing/in-memory).</span></span>

## <a name="automatic-change-detection"></a><span data-ttu-id="7dd0f-191">Detecção de alterações automáticas</span><span class="sxs-lookup"><span data-stu-id="7dd0f-191">Automatic change detection</span></span>

<span data-ttu-id="7dd0f-192">O Entity Framework determina como uma entidade foi alterado (e, portanto, as atualizações que precisam ser enviados para o banco de dados), comparando os valores atuais de uma entidade com os valores originais.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-192">The Entity Framework determines how an entity has changed (and therefore which updates need to be sent to the database) by comparing the current values of an entity with the original values.</span></span> <span data-ttu-id="7dd0f-193">Os valores originais são armazenados quando a entidade é consultada ou anexada.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-193">The original values are stored when the entity is queried or attached.</span></span> <span data-ttu-id="7dd0f-194">Alguns dos métodos que causam a detecção de alterações automáticas são os seguintes:</span><span class="sxs-lookup"><span data-stu-id="7dd0f-194">Some of the methods that cause automatic change detection are the following:</span></span>

* <span data-ttu-id="7dd0f-195">DbContext.SaveChanges</span><span class="sxs-lookup"><span data-stu-id="7dd0f-195">DbContext.SaveChanges</span></span>

* <span data-ttu-id="7dd0f-196">DbContext.Entry</span><span class="sxs-lookup"><span data-stu-id="7dd0f-196">DbContext.Entry</span></span>

* <span data-ttu-id="7dd0f-197">ChangeTracker.Entries</span><span class="sxs-lookup"><span data-stu-id="7dd0f-197">ChangeTracker.Entries</span></span>

<span data-ttu-id="7dd0f-198">Se você estiver rastreando um grande número de entidades e chamar um desses métodos muitas vezes em um loop, você pode obter melhorias significativas de desempenho desativar temporariamente detecção de alterações automáticas usando o `ChangeTracker.AutoDetectChangesEnabled` propriedade.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-198">If you're tracking a large number of entities and you call one of these methods many times in a loop, you might get significant performance improvements by temporarily turning off automatic change detection using the `ChangeTracker.AutoDetectChangesEnabled` property.</span></span> <span data-ttu-id="7dd0f-199">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="7dd0f-199">For example:</span></span>

```csharp
_context.ChangeTracker.AutoDetectChangesEnabled = false;
```

## <a name="entity-framework-core-source-code-and-development-plans"></a><span data-ttu-id="7dd0f-200">Planos de desenvolvimento e o código de origem de Framework Core do entidade</span><span class="sxs-lookup"><span data-stu-id="7dd0f-200">Entity Framework Core source code and development plans</span></span>

<span data-ttu-id="7dd0f-201">A fonte do Entity Framework Core está em [https://github.com/aspnet/EntityFrameworkCore](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="7dd0f-201">The Entity Framework Core source is at [https://github.com/aspnet/EntityFrameworkCore](https://github.com/aspnet/EntityFrameworkCore).</span></span> <span data-ttu-id="7dd0f-202">O repositório de núcleo EF contém compilações noturnas, acompanhamento, especificações de recurso, design de anotações de reuniões, e [o roteiro para desenvolvimento futuro](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap).</span><span class="sxs-lookup"><span data-stu-id="7dd0f-202">The EF Core repository contains nightly builds, issue tracking, feature specs, design meeting notes, and [the roadmap for future development](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap).</span></span> <span data-ttu-id="7dd0f-203">Arquivo ou localizar bugs e contribuir.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-203">You can file or find bugs, and contribute.</span></span>

<span data-ttu-id="7dd0f-204">Embora o código-fonte estiver aberto, Entity Framework Core tem suporte completo como um produto da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-204">Although the source code is open, Entity Framework Core is fully supported as a Microsoft product.</span></span> <span data-ttu-id="7dd0f-205">A equipe do Microsoft Entity Framework mantém controle sobre quais contribuições são aceitos e testa todas as alterações de código para assegurar a qualidade de cada versão.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-205">The Microsoft Entity Framework team keeps control over which contributions are accepted and tests all code changes to ensure the quality of each release.</span></span>

## <a name="reverse-engineer-from-existing-database"></a><span data-ttu-id="7dd0f-206">Engenharia reversa de banco de dados existente</span><span class="sxs-lookup"><span data-stu-id="7dd0f-206">Reverse engineer from existing database</span></span>

<span data-ttu-id="7dd0f-207">Para fazer engenharia reversa um modelo de dados, incluindo classes de entidade de banco de dados existente, use o [scaffold dbcontext](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell#scaffold-dbcontext) comando.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-207">To reverse engineer a data model including entity classes from an existing database, use the [scaffold-dbcontext](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell#scaffold-dbcontext) command.</span></span> <span data-ttu-id="7dd0f-208">Consulte o [tutorial de Introdução](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db).</span><span class="sxs-lookup"><span data-stu-id="7dd0f-208">See the [getting-started tutorial](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db).</span></span>

<a id="dynamic-linq"></a>
## <a name="use-dynamic-linq-to-simplify-sort-selection-code"></a><span data-ttu-id="7dd0f-209">Usar o LINQ dinâmico para simplificar o código de classificação de seleção</span><span class="sxs-lookup"><span data-stu-id="7dd0f-209">Use dynamic LINQ to simplify sort selection code</span></span>

<span data-ttu-id="7dd0f-210">O [terceiro tutorial nesta série](sort-filter-page.md) mostra como escrever um código LINQ, codificar nomes de colunas em um `switch` instrução.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-210">The [third tutorial in this series](sort-filter-page.md) shows how to write LINQ code by hard-coding column names in a `switch` statement.</span></span> <span data-ttu-id="7dd0f-211">Com duas colunas à sua escolha, isso funciona bem, mas se você tiver muitas colunas o código poderia ficar detalhado.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-211">With two columns to choose from, this works fine, but if you have many columns the code could get verbose.</span></span> <span data-ttu-id="7dd0f-212">Para resolver esse problema, você pode usar o `EF.Property` método para especificar o nome da propriedade como uma cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-212">To solve that problem, you can use the `EF.Property` method to specify the name of the property as a string.</span></span> <span data-ttu-id="7dd0f-213">Para usar essa abordagem, substitua o `Index` método o `StudentsController` com o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-213">To try out this approach, replace the `Index` method in the `StudentsController` with the following code.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DynamicLinq)]

## <a name="next-steps"></a><span data-ttu-id="7dd0f-214">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="7dd0f-214">Next steps</span></span>

<span data-ttu-id="7dd0f-215">Isso conclui esta série de tutoriais sobre como usar o Entity Framework Core em um aplicativo ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-215">This completes this series of tutorials on using the Entity Framework Core in an ASP.NET MVC application.</span></span>

<span data-ttu-id="7dd0f-216">Para obter mais informações sobre o EF Core, consulte o [documentação do Entity Framework Core](https://docs.microsoft.com/ef/core).</span><span class="sxs-lookup"><span data-stu-id="7dd0f-216">For more information about EF Core, see the [Entity Framework Core documentation](https://docs.microsoft.com/ef/core).</span></span> <span data-ttu-id="7dd0f-217">Um livro também está disponível: [Entity Framework Core em ação](https://www.manning.com/books/entity-framework-core-in-action).</span><span class="sxs-lookup"><span data-stu-id="7dd0f-217">A book is also available: [Entity Framework Core in Action](https://www.manning.com/books/entity-framework-core-in-action).</span></span>

<span data-ttu-id="7dd0f-218">Para obter informações sobre como implantar um aplicativo web, consulte [Host e implantar](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="7dd0f-218">For information on how to deploy a web app, see [Host and deploy](xref:host-and-deploy/index).</span></span>

<span data-ttu-id="7dd0f-219">Para obter informações sobre outros tópicos relacionados ao ASP.NET MVC de núcleo, como autenticação e autorização, consulte o [documentação do ASP.NET Core](https://docs.microsoft.com/aspnet/core/).</span><span class="sxs-lookup"><span data-stu-id="7dd0f-219">For information about other topics related to ASP.NET Core MVC, such as authentication and authorization, see the [ASP.NET Core documentation](https://docs.microsoft.com/aspnet/core/).</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="7dd0f-220">Confirmações</span><span class="sxs-lookup"><span data-stu-id="7dd0f-220">Acknowledgments</span></span>

<span data-ttu-id="7dd0f-221">Tom Dykstra e Rick Anderson (twitter @RickAndMSFT) gravou neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-221">Tom Dykstra and Rick Anderson (twitter @RickAndMSFT) wrote this tutorial.</span></span> <span data-ttu-id="7dd0f-222">Rowan Miller, Diego Vega e outros membros da equipe do Entity Framework assistido com revisões de código e ajudaram a depurar problemas ocorreu enquanto estamos foram escrevendo código para os tutoriais.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-222">Rowan Miller, Diego Vega, and other members of the Entity Framework team assisted with code reviews and helped debug issues that arose while we were writing code for the tutorials.</span></span>

## <a name="common-errors"></a><span data-ttu-id="7dd0f-223">Erros comuns</span><span class="sxs-lookup"><span data-stu-id="7dd0f-223">Common errors</span></span>  

### <a name="contosouniversitydll-used-by-another-process"></a><span data-ttu-id="7dd0f-224">ContosoUniversity.dll usado por outro processo</span><span class="sxs-lookup"><span data-stu-id="7dd0f-224">ContosoUniversity.dll used by another process</span></span>

<span data-ttu-id="7dd0f-225">Mensagem de erro:</span><span class="sxs-lookup"><span data-stu-id="7dd0f-225">Error message:</span></span>

> <span data-ttu-id="7dd0f-226">Não é possível abrir '... bin\Debug\netcoreapp1.0\ContosoUniversity.dll' para gravação – ' o processo não pode acessar o arquivo '... \bin\Debug\netcoreapp1.0\ContosoUniversity.dll' porque ele está sendo usado por outro processo.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-226">Cannot open '...bin\Debug\netcoreapp1.0\ContosoUniversity.dll' for writing -- 'The process cannot access the file '...\bin\Debug\netcoreapp1.0\ContosoUniversity.dll' because it is being used by another process.</span></span>

<span data-ttu-id="7dd0f-227">Solução:</span><span class="sxs-lookup"><span data-stu-id="7dd0f-227">Solution:</span></span>

<span data-ttu-id="7dd0f-228">Pare o site no IIS Express.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-228">Stop the site in IIS Express.</span></span> <span data-ttu-id="7dd0f-229">Vá para bandeja de sistema do Windows, localizar o IIS Express e com o botão direito no ícone, selecione o site da Universidade de Contoso e, em seguida, clique em **parar Site**.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-229">Go to the Windows System Tray, find IIS Express and right-click its icon, select the Contoso University site, and then click **Stop Site**.</span></span>

### <a name="migration-scaffolded-with-no-code-in-up-and-down-methods"></a><span data-ttu-id="7dd0f-230">Scaffold sem nenhum código em cima e métodos de migração</span><span class="sxs-lookup"><span data-stu-id="7dd0f-230">Migration scaffolded with no code in Up and Down methods</span></span>

<span data-ttu-id="7dd0f-231">Possível causa:</span><span class="sxs-lookup"><span data-stu-id="7dd0f-231">Possible cause:</span></span>

<span data-ttu-id="7dd0f-232">Os comandos de CLI EF não fechar automaticamente e salvar arquivos de código.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-232">The EF CLI commands don't automatically close and save code files.</span></span> <span data-ttu-id="7dd0f-233">Se você tem alterações não salvas quando você executa o `migrations add` comando EF não encontrará suas alterações.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-233">If you have unsaved changes when you run the `migrations add` command, EF won't find your changes.</span></span>

<span data-ttu-id="7dd0f-234">Solução:</span><span class="sxs-lookup"><span data-stu-id="7dd0f-234">Solution:</span></span>

<span data-ttu-id="7dd0f-235">Execute o `migrations remove` de comando, salvar as alterações de código e execute novamente o `migrations add` comando.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-235">Run the `migrations remove` command, save your code changes and rerun the `migrations add` command.</span></span>

### <a name="errors-while-running-database-update"></a><span data-ttu-id="7dd0f-236">Erros durante a atualização do banco de dados em execução</span><span class="sxs-lookup"><span data-stu-id="7dd0f-236">Errors while running database update</span></span>

<span data-ttu-id="7dd0f-237">É possível obter outros erros ao fazer alterações de esquema em um banco de dados que contém dados existentes.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-237">It's possible to get other errors when making schema changes in a database that has existing data.</span></span> <span data-ttu-id="7dd0f-238">Se você obtiver erros de migração que não é possível resolver, você pode alterar o nome do banco de dados na cadeia de conexão ou excluir o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-238">If you get migration errors you can't resolve, you can either change the database name in the connection string or delete the database.</span></span> <span data-ttu-id="7dd0f-239">Com um novo banco de dados, não há nenhum dado para migrar e o comando de atualização de banco de dados é muito mais probabilidade de ser concluído sem erros.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-239">With a new database, there's no data to migrate, and the update-database command is much more likely to complete without errors.</span></span>

<span data-ttu-id="7dd0f-240">A abordagem mais simples é renomear o banco de dados em *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-240">The simplest approach is to rename the database in *appsettings.json*.</span></span> <span data-ttu-id="7dd0f-241">Na próxima vez que você executar `database update`, será criado um novo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-241">The next time you run `database update`, a new database will be created.</span></span>

<span data-ttu-id="7dd0f-242">Para excluir um banco de dados SSOX, o banco de dados, clique **excluir**e, em seguida, o **excluir banco de dados** select da caixa de diálogo **fechar conexões existentes** e clique em  **Okey**.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-242">To delete a database in SSOX, right-click the database, click **Delete**, and then in the **Delete Database** dialog box select **Close existing connections** and click **OK**.</span></span>

<span data-ttu-id="7dd0f-243">Para excluir um banco de dados usando a CLI, execute o `database drop` comando CLI:</span><span class="sxs-lookup"><span data-stu-id="7dd0f-243">To delete a database by using the CLI, run the `database drop` CLI command:</span></span>

```console
dotnet ef database drop
```

### <a name="error-locating-sql-server-instance"></a><span data-ttu-id="7dd0f-244">Instância do SQL Server localização de erro</span><span class="sxs-lookup"><span data-stu-id="7dd0f-244">Error locating SQL Server instance</span></span>

<span data-ttu-id="7dd0f-245">Mensagem de erro:</span><span class="sxs-lookup"><span data-stu-id="7dd0f-245">Error Message:</span></span>

> <span data-ttu-id="7dd0f-246">Ocorreu um erro relacionado à rede ou específico da instância ao estabelecer uma conexão com o SQL Server.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-246">A network-related or instance-specific error occurred while establishing a connection to SQL Server.</span></span> <span data-ttu-id="7dd0f-247">O servidor não foi encontrado ou não estava acessível.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-247">The server was not found or was not accessible.</span></span> <span data-ttu-id="7dd0f-248">Verifique se o nome da instância está correto e se o SQL Server está configurado para permitir conexões remotas.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-248">Verify that the instance name is correct and that SQL Server is configured to allow remote connections.</span></span> <span data-ttu-id="7dd0f-249">(provedor: Interfaces de rede do SQL, erro: 26 - erro ao localizar servidor/instância especificada)</span><span class="sxs-lookup"><span data-stu-id="7dd0f-249">(provider: SQL Network Interfaces, error: 26 - Error Locating Server/Instance Specified)</span></span>

<span data-ttu-id="7dd0f-250">Solução:</span><span class="sxs-lookup"><span data-stu-id="7dd0f-250">Solution:</span></span>

<span data-ttu-id="7dd0f-251">Verifique a cadeia de caracteres de conexão.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-251">Check the connection string.</span></span> <span data-ttu-id="7dd0f-252">Se você excluiu manualmente o arquivo de banco de dados, altere o nome do banco de dados na cadeia de caracteres de construção para começar com um novo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="7dd0f-252">If you have manually deleted the database file, change the name of the database in the construction string to start over with a new database.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="7dd0f-253">Anterior</span><span class="sxs-lookup"><span data-stu-id="7dd0f-253">Previous</span></span>](inheritance.md)
