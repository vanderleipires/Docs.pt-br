---
uid: web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
title: Classificação, paginação e filtragem de dados com a associação de modelos e formulários da web | Microsoft Docs
author: tfitzmac
description: Esta série de tutoriais demonstra aspectos básicos de como usar a associação de modelo com um projeto de Web Forms do ASP.NET. Associação de modelo torna a interação de dados mais simples-...
ms.author: aspnetcontent
ms.date: 02/27/2014
ms.assetid: 266e7866-e327-4687-b29d-627a0925e87d
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
msc.type: authoredcontent
ms.openlocfilehash: ded398bcbbb8d7d9e5c882a598bf9d6faa6ea81e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833870"
---
<a name="sorting-paging-and-filtering-data-with-model-binding-and-web-forms"></a><span data-ttu-id="2af08-104">Classificação, paginação e filtragem de dados com a associação de modelos e formulários da web</span><span class="sxs-lookup"><span data-stu-id="2af08-104">Sorting, paging, and filtering data with model binding and web forms</span></span>
====================
<span data-ttu-id="2af08-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="2af08-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="2af08-106">Esta série de tutoriais demonstra aspectos básicos de como usar a associação de modelo com um projeto de Web Forms do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2af08-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="2af08-107">Associação de modelo torna a interação de dados mais simples que lidam com dados de objetos de origem (como ObjectDataSource ou SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="2af08-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="2af08-108">Esta série começa com material introdutório e move para conceitos mais avançados em tutoriais posteriores.</span><span class="sxs-lookup"><span data-stu-id="2af08-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="2af08-109">Este tutorial mostra como adicionar classificação, paginação e filtragem dos dados por meio de associação de modelos.</span><span class="sxs-lookup"><span data-stu-id="2af08-109">This tutorial shows how to add sorting, paging, and filtering of the data through model binding.</span></span>
> 
> <span data-ttu-id="2af08-110">Este tutorial se baseia no projeto criado no primeiro [parte](retrieving-data.md) da série.</span><span class="sxs-lookup"><span data-stu-id="2af08-110">This tutorial builds on the project created in the first [part](retrieving-data.md) of the series.</span></span>
> 
> <span data-ttu-id="2af08-111">Você pode [baixar](https://go.microsoft.com/fwlink/?LinkId=286116) o projeto completo em c# ou VB.</span><span class="sxs-lookup"><span data-stu-id="2af08-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="2af08-112">O código para download funciona com o Visual Studio 2012 ou Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="2af08-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="2af08-113">Ele usa o modelo do Visual Studio 2012, que é ligeiramente diferente do que o modelo do Visual Studio 2013 mostrado neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="2af08-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="2af08-114">O que você vai criar</span><span class="sxs-lookup"><span data-stu-id="2af08-114">What you'll build</span></span>

<span data-ttu-id="2af08-115">Neste tutorial, você vai:</span><span class="sxs-lookup"><span data-stu-id="2af08-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="2af08-116">Habilitar a classificação e paginação dos dados</span><span class="sxs-lookup"><span data-stu-id="2af08-116">Enable sorting and paging of the data</span></span>
2. <span data-ttu-id="2af08-117">Habilitar a filtragem dos dados com base em uma seleção pelo usuário</span><span class="sxs-lookup"><span data-stu-id="2af08-117">Enable filtering of the data based on a selection by the user</span></span>

## <a name="add-sorting"></a><span data-ttu-id="2af08-118">Adicionar uma classificação</span><span class="sxs-lookup"><span data-stu-id="2af08-118">Add sorting</span></span>

<span data-ttu-id="2af08-119">Habilitar a classificação em GridView é muito fácil.</span><span class="sxs-lookup"><span data-stu-id="2af08-119">Enabling sorting in the GridView is very easy.</span></span> <span data-ttu-id="2af08-120">No arquivo Student.aspx, basta definir **AllowSorting** à **verdadeira** no GridView.</span><span class="sxs-lookup"><span data-stu-id="2af08-120">In the Student.aspx file, simply set **AllowSorting** to **true** in the GridView.</span></span> <span data-ttu-id="2af08-121">Não é necessário definir uma **SortExpression** valor para cada coluna como DataField é usado automaticamente.</span><span class="sxs-lookup"><span data-stu-id="2af08-121">You do not need to set a **SortExpression** value for each column as the DataField is automatically used.</span></span> <span data-ttu-id="2af08-122">O GridView modifica a consulta para incluir ordenando os dados pelo valor selecionado.</span><span class="sxs-lookup"><span data-stu-id="2af08-122">The GridView modifies the query to include ordering the data by the selected value.</span></span> <span data-ttu-id="2af08-123">O código realçado a seguir mostra a adição, que você precisa fazer para habilitar a classificação.</span><span class="sxs-lookup"><span data-stu-id="2af08-123">The highlighted code below shows the addition you need to make to enable sorting.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample1.aspx?highlight=5)]

<span data-ttu-id="2af08-124">Executar o aplicativo web e teste de classificação de registros de alunos por valores em colunas diferentes.</span><span class="sxs-lookup"><span data-stu-id="2af08-124">Run the web application, and test sorting student records by the values in different columns.</span></span>

![alunos de classificação](sorting-paging-and-filtering-data/_static/image2.png)

## <a name="add-paging"></a><span data-ttu-id="2af08-126">Adicionar paginação</span><span class="sxs-lookup"><span data-stu-id="2af08-126">Add paging</span></span>

<span data-ttu-id="2af08-127">Habilitar paginação também é muito fácil.</span><span class="sxs-lookup"><span data-stu-id="2af08-127">Enabling paging is also very easy.</span></span> <span data-ttu-id="2af08-128">Em GridView, defina as **AllowPaging** propriedade a ser **verdadeiro** e defina o **PageSize** propriedade para o número de registros que você deseja exibir em cada página.</span><span class="sxs-lookup"><span data-stu-id="2af08-128">In the GridView, set the **AllowPaging** property to **true** and set the **PageSize** property to the number of records you wish to display on each page.</span></span> <span data-ttu-id="2af08-129">Neste tutorial, você pode configurá-lo a 4.</span><span class="sxs-lookup"><span data-stu-id="2af08-129">In this tutorial, you can set it to 4.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample2.aspx?highlight=5)]

<span data-ttu-id="2af08-130">Executar o aplicativo web e observe que agora os registros são divididos em várias páginas com não mais do que 4 registros exibidos em uma única página.</span><span class="sxs-lookup"><span data-stu-id="2af08-130">Run the web application, and notice that now the records are divided over multiple pages with no more than 4 records displayed on a single page.</span></span>

![Adicionar paginação](sorting-paging-and-filtering-data/_static/image4.png)

<span data-ttu-id="2af08-132">Execução de consulta adiada melhora a eficiência do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2af08-132">Deferred query execution improves the application efficiency.</span></span> <span data-ttu-id="2af08-133">Em vez de recuperar todo o conjunto de dados, o GridView modifica a consulta para recuperar apenas os registros para a página atual.</span><span class="sxs-lookup"><span data-stu-id="2af08-133">Instead of retrieving the entire data set, the GridView modifies the query to retrieve only the records for the current page.</span></span>

## <a name="filter-records-by-user-selection"></a><span data-ttu-id="2af08-134">Filtrar registros por seleção do usuário</span><span class="sxs-lookup"><span data-stu-id="2af08-134">Filter records by user selection</span></span>

<span data-ttu-id="2af08-135">Associação de modelos adiciona vários atributos que permitem que você designar como definir o valor para um parâmetro em um método de associação de modelo.</span><span class="sxs-lookup"><span data-stu-id="2af08-135">Model binding adds several attributes which enable you to designate how to set the value for a parameter in a model binding method.</span></span> <span data-ttu-id="2af08-136">Esses atributos estão na **modelbinding** namespace.</span><span class="sxs-lookup"><span data-stu-id="2af08-136">These attributes are in the **System.Web.ModelBinding** namespace.</span></span> <span data-ttu-id="2af08-137">Entre elas estão:</span><span class="sxs-lookup"><span data-stu-id="2af08-137">They include:</span></span>

- <span data-ttu-id="2af08-138">Controle</span><span class="sxs-lookup"><span data-stu-id="2af08-138">Control</span></span>
- <span data-ttu-id="2af08-139">Cookie</span><span class="sxs-lookup"><span data-stu-id="2af08-139">Cookie</span></span>
- <span data-ttu-id="2af08-140">Formulário</span><span class="sxs-lookup"><span data-stu-id="2af08-140">Form</span></span>
- <span data-ttu-id="2af08-141">Perfil</span><span class="sxs-lookup"><span data-stu-id="2af08-141">Profile</span></span>
- <span data-ttu-id="2af08-142">QueryString</span><span class="sxs-lookup"><span data-stu-id="2af08-142">QueryString</span></span>
- <span data-ttu-id="2af08-143">RouteData</span><span class="sxs-lookup"><span data-stu-id="2af08-143">RouteData</span></span>
- <span data-ttu-id="2af08-144">Session</span><span class="sxs-lookup"><span data-stu-id="2af08-144">Session</span></span>
- <span data-ttu-id="2af08-145">Perfil de usuário</span><span class="sxs-lookup"><span data-stu-id="2af08-145">UserProfile</span></span>
- <span data-ttu-id="2af08-146">ViewState</span><span class="sxs-lookup"><span data-stu-id="2af08-146">ViewState</span></span>

<span data-ttu-id="2af08-147">Neste tutorial, você usará o valor de um controle para filtrar quais registros são exibidos em GridView.</span><span class="sxs-lookup"><span data-stu-id="2af08-147">In this tutorial, you will use a control's value to filter which records are displayed in the GridView.</span></span> <span data-ttu-id="2af08-148">Você adicionará os **controle** de atributo para o método de consulta que você criou anteriormente.</span><span class="sxs-lookup"><span data-stu-id="2af08-148">You will add the **Control** attribute to the query method you had created earlier.</span></span> <span data-ttu-id="2af08-149">Em um [mais tarde](using-query-string-values-to-retrieve-data.md) tutorial, você aplicará a **QueryString** de atributo para um parâmetro para especificar que o valor do parâmetro vem de um valor de cadeia de caracteres de consulta.</span><span class="sxs-lookup"><span data-stu-id="2af08-149">In a [later](using-query-string-values-to-retrieve-data.md) tutorial, you will apply the **QueryString** attribute to a parameter to specify that the parameter value comes from a query string value.</span></span>

<span data-ttu-id="2af08-150">Em primeiro lugar, acima ValidationSummary, adicione uma lista suspensa de filtragem que os alunos são mostrados.</span><span class="sxs-lookup"><span data-stu-id="2af08-150">First, above the ValidationSummary, add a drop down list for filtering which students are shown.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample3.aspx?highlight=3-11)]

<span data-ttu-id="2af08-151">No arquivo code-behind, modifique o método select para receber um valor do controle e defina o nome do parâmetro como o nome do controle que fornece o valor.</span><span class="sxs-lookup"><span data-stu-id="2af08-151">In the code-behind file, modify the select method to receive a value from the control, and set the name of the parameter to the name of the control that provides the value.</span></span>

<span data-ttu-id="2af08-152">Você deve adicionar uma **usando** instrução para o **modelbinding** namespace a resolver o atributo de controle.</span><span class="sxs-lookup"><span data-stu-id="2af08-152">You must add a **using** statement for the **System.Web.ModelBinding** namespace to resolve the Control attribute.</span></span>

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample4.cs)]

<span data-ttu-id="2af08-153">O código a seguir mostra o método select novamente trabalhado para filtrar os dados retornados com base no valor da lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="2af08-153">The following code shows the select method re-worked to filter the returned data based on the value of the drop down list.</span></span> <span data-ttu-id="2af08-154">Adicionando um atributo de controle antes de um parâmetro especifica que o valor para esse parâmetro vem de um controle com o mesmo nome.</span><span class="sxs-lookup"><span data-stu-id="2af08-154">Adding a control attribute before a parameter specifies that the value for this parameter comes from a control with the same name.</span></span>

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample5.cs)]

<span data-ttu-id="2af08-155">Executar o aplicativo web e selecione diferentes valores na lista suspensa lista para filtrar a lista de alunos.</span><span class="sxs-lookup"><span data-stu-id="2af08-155">Run the web application and select different values from the drop down list to filter the list of students.</span></span>

![alunos de filtro](sorting-paging-and-filtering-data/_static/image6.png)

## <a name="conclusion"></a><span data-ttu-id="2af08-157">Conclusão</span><span class="sxs-lookup"><span data-stu-id="2af08-157">Conclusion</span></span>

<span data-ttu-id="2af08-158">Neste tutorial, você habilitou a classificação e paginação dos dados.</span><span class="sxs-lookup"><span data-stu-id="2af08-158">In this tutorial, you enabled sorting and paging of the data.</span></span> <span data-ttu-id="2af08-159">Você também habilitado filtrando os dados pelo valor de um controle.</span><span class="sxs-lookup"><span data-stu-id="2af08-159">You also enabled filtering the data by the value of a control.</span></span>

<span data-ttu-id="2af08-160">Nos próximos [tutorial](integrating-jquery-ui.md) você aprimorará a interface do usuário, integrando um widget JQuery UI para o modelo de dados dinâmicos.</span><span class="sxs-lookup"><span data-stu-id="2af08-160">In the next [tutorial](integrating-jquery-ui.md) you will enhance the UI by integrating a JQuery UI widget into the dynamic data template.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2af08-161">[Anterior](updating-deleting-and-creating-data.md)
> [Próximo](integrating-jquery-ui.md)</span><span class="sxs-lookup"><span data-stu-id="2af08-161">[Previous](updating-deleting-and-creating-data.md)
[Next](integrating-jquery-ui.md)</span></span>
