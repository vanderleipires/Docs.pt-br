---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
title: 'Usando o Entity Framework 4.0 e o controle ObjectDataSource, parte 3: classificando e filtrando | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais se baseia no aplicativo web Contoso University que é criado pelo Getting Started with a série de tutoriais do Entity Framework 4.0. EU...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 2990bd10-590d-43d5-9529-6b503ce5455d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
msc.type: authoredcontent
ms.openlocfilehash: 48d859191877ba951e233f19873d52625fe180ac
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378904"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-3-sorting-and-filtering"></a><span data-ttu-id="02744-104">Usando o Entity Framework 4.0 e o controle ObjectDataSource, parte 3: classificando e filtrando</span><span class="sxs-lookup"><span data-stu-id="02744-104">Using the Entity Framework 4.0 and the ObjectDataSource Control, Part 3: Sorting and Filtering</span></span>
====================
<span data-ttu-id="02744-105">por [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="02744-105">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="02744-106">Esta série de tutoriais baseia-se no aplicativo web Contoso University que é criado pela [Introdução ao Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) série de tutoriais.</span><span class="sxs-lookup"><span data-stu-id="02744-106">This tutorial series builds on the Contoso University web application that is created by the [Getting Started with the Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) tutorial series.</span></span> <span data-ttu-id="02744-107">Se você não concluir os tutoriais anteriores, como um ponto de partida para este tutorial, você pode [baixar o aplicativo](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) que você teria criado.</span><span class="sxs-lookup"><span data-stu-id="02744-107">If you didn't complete the earlier tutorials, as a starting point for this tutorial you can [download the application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) that you would have created.</span></span> <span data-ttu-id="02744-108">Você também pode [baixar o aplicativo](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) que é criado pela série de tutoriais completa.</span><span class="sxs-lookup"><span data-stu-id="02744-108">You can also [download the application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) that is created by the complete tutorial series.</span></span> <span data-ttu-id="02744-109">Se você tiver dúvidas sobre os tutoriais, você pode postá-los para o [Fórum do Entity Framework do ASP.NET](https://forums.asp.net/1227.aspx).</span><span class="sxs-lookup"><span data-stu-id="02744-109">If you have questions about the tutorials, you can post them to the [ASP.NET Entity Framework forum](https://forums.asp.net/1227.aspx).</span></span>


<span data-ttu-id="02744-110">No tutorial anterior, você implementou o padrão de repositório em um aplicativo web de n camadas que usa o Entity Framework e o `ObjectDataSource` controle.</span><span class="sxs-lookup"><span data-stu-id="02744-110">In the previous tutorial you implemented the repository pattern in an n-tier web application that uses the Entity Framework and the `ObjectDataSource` control.</span></span> <span data-ttu-id="02744-111">Este tutorial mostra como fazer a classificação e filtragem e lidar com cenários de detalhes mestre.</span><span class="sxs-lookup"><span data-stu-id="02744-111">This tutorial shows how to do sorting and filtering and handle master-detail scenarios.</span></span> <span data-ttu-id="02744-112">Você adicionará os seguintes aprimoramentos para o *Departments.aspx* página:</span><span class="sxs-lookup"><span data-stu-id="02744-112">You'll add the following enhancements to the *Departments.aspx* page:</span></span>

- <span data-ttu-id="02744-113">Uma caixa de texto para permitir que os usuários selecionem departamentos por nome.</span><span class="sxs-lookup"><span data-stu-id="02744-113">A text box to allow users to select departments by name.</span></span>
- <span data-ttu-id="02744-114">Uma lista de cursos para cada departamento, que é mostrada na grade.</span><span class="sxs-lookup"><span data-stu-id="02744-114">A list of courses for each department that's shown in the grid.</span></span>
- <span data-ttu-id="02744-115">A capacidade de classificar clicando em títulos de coluna.</span><span class="sxs-lookup"><span data-stu-id="02744-115">The ability to sort by clicking column headings.</span></span>

<span data-ttu-id="02744-116">[![Para Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="02744-116">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)</span></span>

## <a name="adding-the-ability-to-sort-gridview-columns"></a><span data-ttu-id="02744-117">Adicionando a capacidade para classificar colunas de GridView</span><span class="sxs-lookup"><span data-stu-id="02744-117">Adding the Ability to Sort GridView Columns</span></span>

<span data-ttu-id="02744-118">Abra o *Departments.aspx* da página e adicionar um `SortParameterName="sortExpression"` de atributo para o `ObjectDataSource` controle chamado `DepartmentsObjectDataSource`.</span><span class="sxs-lookup"><span data-stu-id="02744-118">Open the *Departments.aspx* page and add a `SortParameterName="sortExpression"` attribute to the `ObjectDataSource` control named `DepartmentsObjectDataSource`.</span></span> <span data-ttu-id="02744-119">(Posteriormente, você criará um `GetDepartments` método que usa um parâmetro chamado `sortExpression`.) A marcação para a marca de abertura do controle agora se parece com o exemplo a seguir.</span><span class="sxs-lookup"><span data-stu-id="02744-119">(Later you'll create a `GetDepartments` method that takes a parameter named `sortExpression`.) The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample1.aspx)]

<span data-ttu-id="02744-120">Adicione a `AllowSorting="true"` atributo à marca de abertura do `GridView` controle.</span><span class="sxs-lookup"><span data-stu-id="02744-120">Add the `AllowSorting="true"` attribute to the opening tag of the `GridView` control.</span></span> <span data-ttu-id="02744-121">A marcação para a marca de abertura do controle agora se parece com o exemplo a seguir.</span><span class="sxs-lookup"><span data-stu-id="02744-121">The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample2.aspx)]

<span data-ttu-id="02744-122">Na *Departments.aspx.cs*, definir a ordem de classificação padrão chamando o `GridView` do controle `Sort` método a partir de `Page_Load` método:</span><span class="sxs-lookup"><span data-stu-id="02744-122">In *Departments.aspx.cs*, set the default sort order by calling the `GridView` control's `Sort` method from the `Page_Load` method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample3.cs)]

<span data-ttu-id="02744-123">Você pode adicionar código que classifica ou filtra na classe de lógica de negócios ou a classe de repositório.</span><span class="sxs-lookup"><span data-stu-id="02744-123">You can add code that sorts or filters in either the business logic class or the repository class.</span></span> <span data-ttu-id="02744-124">Se você fizer isso na classe de lógica de negócios, classificação ou filtragem de trabalho será feito depois que os dados são recuperados do banco de dados, porque a classe de lógica de negócios está trabalhando com um `IEnumerable` objeto retornado pelo repositório.</span><span class="sxs-lookup"><span data-stu-id="02744-124">If you do it in the business logic class, the sorting or filtering work will be done after the data is retrieved from the database, because the business logic class is working with an `IEnumerable` object returned by the repository.</span></span> <span data-ttu-id="02744-125">Se você adicionar a classificação e filtragem de código na classe de repositório e fazer isso antes de uma expressão LINQ ou consulta de objeto foi convertida em um `IEnumerable` do objeto, os comandos serão transmitidos para o banco de dados para processamento, que é geralmente mais eficiente.</span><span class="sxs-lookup"><span data-stu-id="02744-125">If you add sorting and filtering code in the repository class and you do it before a LINQ expression or object query has been converted to an `IEnumerable` object, your commands will be passed through to the database for processing, which is typically more efficient.</span></span> <span data-ttu-id="02744-126">Neste tutorial você implementará classificação e filtragem de forma que faz com que o processamento a ser feito pelo banco de dados — ou seja, no repositório.</span><span class="sxs-lookup"><span data-stu-id="02744-126">In this tutorial you'll implement sorting and filtering in a way that causes the processing to be done by the database — that is, in the repository.</span></span>

<span data-ttu-id="02744-127">Para adicionar o recurso de classificação, você deve adicionar um novo método para a interface de repositório e classes de repositório, bem como para a classe de lógica de negócios.</span><span class="sxs-lookup"><span data-stu-id="02744-127">To add sorting capability, you must add a new method to the repository interface and repository classes as well as to the business logic class.</span></span> <span data-ttu-id="02744-128">No *ISchoolRepository.cs* do arquivo, adicione uma nova `GetDepartments` método que usa um `sortExpression` parâmetro que será usado para classificar a lista de departamentos que é retornada:</span><span class="sxs-lookup"><span data-stu-id="02744-128">In the *ISchoolRepository.cs* file, add a new `GetDepartments` method that takes a `sortExpression` parameter that will be used to sort the list of departments that's returned:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample4.cs)]

<span data-ttu-id="02744-129">O `sortExpression` parâmetro especificará a coluna para a classificação e a direção de classificação.</span><span class="sxs-lookup"><span data-stu-id="02744-129">The `sortExpression` parameter will specify the column to sort on and the sort direction.</span></span>

<span data-ttu-id="02744-130">Adicione código para o novo método para o *SchoolRepository.cs* arquivo:</span><span class="sxs-lookup"><span data-stu-id="02744-130">Add code for the new method to the *SchoolRepository.cs* file:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample5.cs)]

<span data-ttu-id="02744-131">Alterar existente sem parâmetros `GetDepartments` método para chamar o novo método:</span><span class="sxs-lookup"><span data-stu-id="02744-131">Change the existing parameterless `GetDepartments` method to call the new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample6.cs)]

<span data-ttu-id="02744-132">No projeto de teste, adicione o seguinte método novo à *MockSchoolRepository.cs*:</span><span class="sxs-lookup"><span data-stu-id="02744-132">In the test project, add the following new method to *MockSchoolRepository.cs*:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample7.cs)]

<span data-ttu-id="02744-133">Se você fosse criar os testes de unidade que dependem desse método retornar uma lista classificada, você precisa classificar a lista antes de retorná-lo.</span><span class="sxs-lookup"><span data-stu-id="02744-133">If you were going to create any unit tests that depended on this method returning a sorted list, you would need to sort the list before returning it.</span></span> <span data-ttu-id="02744-134">Você não criar testes como esse neste tutorial, para que o método pode apenas retornar a lista não classificada de departamentos.</span><span class="sxs-lookup"><span data-stu-id="02744-134">You won't be creating tests like that in this tutorial, so the method can just return the unsorted list of departments.</span></span>

<span data-ttu-id="02744-135">No *SchoolBL.cs* de arquivo, adicione o seguinte método à classe de lógica de negócios:</span><span class="sxs-lookup"><span data-stu-id="02744-135">In the *SchoolBL.cs* file, add the following new method to the business logic class:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample8.cs)]

<span data-ttu-id="02744-136">Esse código passa o parâmetro de classificação para o método de repositório.</span><span class="sxs-lookup"><span data-stu-id="02744-136">This code passes the sort parameter to the repository method.</span></span>

<span data-ttu-id="02744-137">Execute o *Departments.aspx* página.</span><span class="sxs-lookup"><span data-stu-id="02744-137">Run the *Departments.aspx* page.</span></span>

<span data-ttu-id="02744-138">[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="02744-138">[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)</span></span>

<span data-ttu-id="02744-139">Agora você pode clicar em qualquer cabeçalho de coluna para classificar por essa coluna.</span><span class="sxs-lookup"><span data-stu-id="02744-139">You can now click any column heading to sort by that column.</span></span> <span data-ttu-id="02744-140">Se a coluna já está classificada, clicando no título inverte a direção de classificação.</span><span class="sxs-lookup"><span data-stu-id="02744-140">If the column is already sorted, clicking the heading reverses the sort direction.</span></span>

## <a name="adding-a-search-box"></a><span data-ttu-id="02744-141">Adicionando uma caixa de pesquisa</span><span class="sxs-lookup"><span data-stu-id="02744-141">Adding a Search Box</span></span>

<span data-ttu-id="02744-142">Nesta seção você irá adicionar uma caixa de texto de pesquisa, vinculá-lo para o `ObjectDataSource` controlar usando um parâmetro de controle e adicionar um método à classe de lógica de negócios para dar suporte à filtragem.</span><span class="sxs-lookup"><span data-stu-id="02744-142">In this section you'll add a search text box, link it to the `ObjectDataSource` control using a control parameter, and add a method to the business logic class to support filtering.</span></span>

<span data-ttu-id="02744-143">Abra o *Departments.aspx* da página e adicione a seguinte marcação entre o título e o primeiro `ObjectDataSource` controle:</span><span class="sxs-lookup"><span data-stu-id="02744-143">Open the *Departments.aspx* page and add the following markup between the heading and the first `ObjectDataSource` control:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample9.aspx)]

<span data-ttu-id="02744-144">No `ObjectDataSource` controle denominado `DepartmentsObjectDataSource`, faça o seguinte:</span><span class="sxs-lookup"><span data-stu-id="02744-144">In the `ObjectDataSource` control named `DepartmentsObjectDataSource`, do the following:</span></span>

- <span data-ttu-id="02744-145">Adicionar um `SelectParameters` elemento para um parâmetro denominado `nameSearchString` que obtém o valor inserido no `SearchTextBox` controle.</span><span class="sxs-lookup"><span data-stu-id="02744-145">Add a `SelectParameters` element for a parameter named `nameSearchString` that gets the value entered in the `SearchTextBox` control.</span></span>
- <span data-ttu-id="02744-146">Alterar o `SelectMethod` valor de atributo `GetDepartmentsByName`.</span><span class="sxs-lookup"><span data-stu-id="02744-146">Change the `SelectMethod` attribute value to `GetDepartmentsByName`.</span></span> <span data-ttu-id="02744-147">(Você criará esse método posteriormente.)</span><span class="sxs-lookup"><span data-stu-id="02744-147">(You'll create this method later.)</span></span>

<span data-ttu-id="02744-148">A marcação para o `ObjectDataSource` controle agora é semelhante ao exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="02744-148">The markup for the `ObjectDataSource` control now resembles the following example:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample10.aspx)]

<span data-ttu-id="02744-149">Na *ISchoolRepository.cs*, adicione uma `GetDepartmentsByName` método que usa ambos `sortExpression` e `nameSearchString` parâmetros:</span><span class="sxs-lookup"><span data-stu-id="02744-149">In *ISchoolRepository.cs*, add a `GetDepartmentsByName` method that takes both `sortExpression` and `nameSearchString` parameters:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample11.cs)]

<span data-ttu-id="02744-150">Na *SchoolRepository.cs*, adicione o seguinte método:</span><span class="sxs-lookup"><span data-stu-id="02744-150">In *SchoolRepository.cs*, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample12.cs)]

<span data-ttu-id="02744-151">Esse código usa um `Where` método para selecionar os itens que contêm a cadeia de caracteres de pesquisa.</span><span class="sxs-lookup"><span data-stu-id="02744-151">This code uses a `Where` method to select items that contain the search string.</span></span> <span data-ttu-id="02744-152">Se a cadeia de caracteres de pesquisa está vazia, todos os registros serão selecionados.</span><span class="sxs-lookup"><span data-stu-id="02744-152">If the search string is empty, all records will be selected.</span></span> <span data-ttu-id="02744-153">Observe que, quando você especifica o método chama juntos em uma instrução como esta (`Include`, em seguida, `OrderBy`, em seguida, `Where`), o `Where` método sempre deve ser o último.</span><span class="sxs-lookup"><span data-stu-id="02744-153">Note that when you specify method calls together in one statement like this (`Include`, then `OrderBy`, then `Where`), the `Where` method must always be last.</span></span>

<span data-ttu-id="02744-154">Alterar existente `GetDepartments` método que usa um `sortExpression` parâmetro ao chamar o novo método:</span><span class="sxs-lookup"><span data-stu-id="02744-154">Change the existing `GetDepartments` method that takes a `sortExpression` parameter to call the new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample13.cs)]

<span data-ttu-id="02744-155">Na *MockSchoolRepository.cs* no projeto de teste, adicione o seguinte método:</span><span class="sxs-lookup"><span data-stu-id="02744-155">In *MockSchoolRepository.cs* in the test project, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample14.cs)]

<span data-ttu-id="02744-156">Na *SchoolBL.cs*, adicione o seguinte método:</span><span class="sxs-lookup"><span data-stu-id="02744-156">In *SchoolBL.cs*, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample15.cs)]

<span data-ttu-id="02744-157">Execute o *Departments.aspx* página e insira uma cadeia de caracteres de pesquisa para certificar-se de que a lógica de seleção funciona.</span><span class="sxs-lookup"><span data-stu-id="02744-157">Run the *Departments.aspx* page and enter a search string to make sure that the selection logic works.</span></span> <span data-ttu-id="02744-158">Deixe a caixa de texto vazia e tente fazer uma pesquisa para certificar-se de que todos os registros são retornados.</span><span class="sxs-lookup"><span data-stu-id="02744-158">Leave the text box empty and try a search to make sure that all records are returned.</span></span>

<span data-ttu-id="02744-159">[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="02744-159">[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)</span></span>

## <a name="adding-a-details-column-for-each-grid-row"></a><span data-ttu-id="02744-160">Adicionando uma coluna de detalhes para cada linha de grade</span><span class="sxs-lookup"><span data-stu-id="02744-160">Adding a Details Column for Each Grid Row</span></span>

<span data-ttu-id="02744-161">Em seguida, você deseja ver todos os cursos para cada departamento exibido na célula à direita da grade.</span><span class="sxs-lookup"><span data-stu-id="02744-161">Next, you want to see all of the courses for each department displayed in the right-hand cell of the grid.</span></span> <span data-ttu-id="02744-162">Para fazer isso, você usará um aninhados `GridView` databind e controle-o para dados do `Courses` propriedade de navegação do `Department` entidade.</span><span class="sxs-lookup"><span data-stu-id="02744-162">To do this, you'll use a nested `GridView` control and databind it to data from the `Courses` navigation property of the `Department` entity.</span></span>

<span data-ttu-id="02744-163">Abra *Departments.aspx* e na marcação para o `GridView` controlar, especificar um manipulador para o `RowDataBound` eventos.</span><span class="sxs-lookup"><span data-stu-id="02744-163">Open *Departments.aspx* and in the markup for the `GridView` control, specify a handler for the `RowDataBound` event.</span></span> <span data-ttu-id="02744-164">A marcação para a marca de abertura do controle agora se parece com o exemplo a seguir.</span><span class="sxs-lookup"><span data-stu-id="02744-164">The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample16.aspx)]

<span data-ttu-id="02744-165">Adicione um novo `TemplateField` elemento após o `Administrator` campo de modelo:</span><span class="sxs-lookup"><span data-stu-id="02744-165">Add a new `TemplateField` element after the `Administrator` template field:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample17.aspx)]

<span data-ttu-id="02744-166">Essa marcação cria um aninhada `GridView` controle que mostra o número e o título de uma lista de cursos.</span><span class="sxs-lookup"><span data-stu-id="02744-166">This markup creates a nested `GridView` control that shows the course number and title of a list of courses.</span></span> <span data-ttu-id="02744-167">Ele não especifica uma fonte de dados porque você vai databind-lo no código no `RowDataBound` manipulador.</span><span class="sxs-lookup"><span data-stu-id="02744-167">It does not specify a data source because you'll databind it in code in the `RowDataBound` handler.</span></span>

<span data-ttu-id="02744-168">Abra *Departments.aspx.cs* e adicione o seguinte manipulador para o `RowDataBound` evento:</span><span class="sxs-lookup"><span data-stu-id="02744-168">Open *Departments.aspx.cs* and add the following handler for the `RowDataBound` event:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample18.cs)]

<span data-ttu-id="02744-169">Esse código obtém os `Department` converte a entidade de argumentos do evento, o `Courses` propriedade de navegação para um `List` coleta e databinds aninhada `GridView` à coleção.</span><span class="sxs-lookup"><span data-stu-id="02744-169">This code gets the `Department` entity from the event arguments, converts the `Courses` navigation property to a `List` collection, and databinds the nested `GridView` to the collection.</span></span>

<span data-ttu-id="02744-170">Abra o *SchoolRepository.cs* de arquivo e especifique o carregamento adiantado para a `Courses` propriedade de navegação, chamando o `Include` método na consulta de objeto que você cria no `GetDepartmentsByName` método.</span><span class="sxs-lookup"><span data-stu-id="02744-170">Open the *SchoolRepository.cs* file and specify eager loading for the `Courses` navigation property by calling the `Include` method in the object query that you create in the `GetDepartmentsByName` method.</span></span> <span data-ttu-id="02744-171">O `return` instrução no `GetDepartmentsByName` método agora é semelhante ao exemplo a seguir.</span><span class="sxs-lookup"><span data-stu-id="02744-171">The `return` statement in the `GetDepartmentsByName` method now resembles the following example.</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample19.cs)]

<span data-ttu-id="02744-172">Execute a página.</span><span class="sxs-lookup"><span data-stu-id="02744-172">Run the page.</span></span> <span data-ttu-id="02744-173">Além da classificação e filtragem de recurso que você adicionou anteriormente, o controle GridView agora mostra detalhes do curso aninhado para cada departamento.</span><span class="sxs-lookup"><span data-stu-id="02744-173">In addition to the sorting and filtering capability that you added earlier, the GridView control now shows nested course details for each department.</span></span>

<span data-ttu-id="02744-174">[![Para Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="02744-174">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)</span></span>

<span data-ttu-id="02744-175">Isso conclui a introdução aos cenários de filtragem, classificação e master-detail.</span><span class="sxs-lookup"><span data-stu-id="02744-175">This completes the introduction to sorting, filtering, and master-detail scenarios.</span></span> <span data-ttu-id="02744-176">O próximo tutorial, você verá como manipular a simultaneidade.</span><span class="sxs-lookup"><span data-stu-id="02744-176">In the next tutorial, you'll see how to handle concurrency.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="02744-177">[Anterior](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
> [Próximo](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)</span><span class="sxs-lookup"><span data-stu-id="02744-177">[Previous](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
[Next](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)</span></span>
