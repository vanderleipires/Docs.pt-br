---
uid: web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
title: Usando valores de cadeia de caracteres de consulta para filtrar dados com associação de modelo e formulários da web | Microsoft Docs
author: tfitzmac
description: Esta série de tutoriais demonstra aspectos básicos do uso de associação de modelo com um projeto Web Forms do ASP.NET. Associação de modelo torna a interação de dados mais estreita-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: b90978bd-795d-4871-9ade-1671caff5730
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
msc.type: authoredcontent
ms.openlocfilehash: 03d20decf0eeff6062fbc6f8dd66f644b405c7cc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="using-query-string-values-to-filter-data-with-model-binding-and-web-forms"></a><span data-ttu-id="b3ae2-104">Usando valores de cadeia de caracteres de consulta para filtrar dados com o modelo de associação e formulários da web</span><span class="sxs-lookup"><span data-stu-id="b3ae2-104">Using query string values to filter data with model binding and web forms</span></span>
====================
<span data-ttu-id="b3ae2-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="b3ae2-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="b3ae2-106">Esta série de tutoriais demonstra aspectos básicos do uso de associação de modelo com um projeto Web Forms do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b3ae2-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="b3ae2-107">Associação de modelo facilita a interação de dados mais direta de lidar com dados de objetos de origem (como ObjectDataSource ou SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="b3ae2-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="b3ae2-108">Esta série começa com material introdutório e move para conceitos mais avançados em tutoriais subsequentes.</span><span class="sxs-lookup"><span data-stu-id="b3ae2-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="b3ae2-109">Este tutorial mostra como passar um valor de cadeia de caracteres de consulta e use esse valor para recuperar os dados por meio de associação de modelo.</span><span class="sxs-lookup"><span data-stu-id="b3ae2-109">This tutorial shows how to pass a value in the query string and use that value to retrieve data through model binding.</span></span>
> 
> <span data-ttu-id="b3ae2-110">Este tutorial baseia-se no projeto criado no [anteriores](retrieving-data.md) partes da série.</span><span class="sxs-lookup"><span data-stu-id="b3ae2-110">This tutorial builds on the project created in the [earlier](retrieving-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="b3ae2-111">Você pode [baixar](https://go.microsoft.com/fwlink/?LinkId=286116) o projeto completo em c# ou VB.</span><span class="sxs-lookup"><span data-stu-id="b3ae2-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="b3ae2-112">O código para download funciona com o Visual Studio 2012 ou Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="b3ae2-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="b3ae2-113">Ele usa o modelo do Visual Studio 2012, que é ligeiramente diferente do que o modelo do Visual Studio 2013 mostrado neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="b3ae2-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="b3ae2-114">Será possível compilar</span><span class="sxs-lookup"><span data-stu-id="b3ae2-114">What you'll build</span></span>

<span data-ttu-id="b3ae2-115">Neste tutorial, você vai:</span><span class="sxs-lookup"><span data-stu-id="b3ae2-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="b3ae2-116">Adicione uma nova página para mostrar os cursos registrados para um aluno</span><span class="sxs-lookup"><span data-stu-id="b3ae2-116">Add a new page to show the enrolled courses for a student</span></span>
2. <span data-ttu-id="b3ae2-117">Recuperar os cursos registrados do aluno selecionado com base em um valor de cadeia de caracteres de consulta</span><span class="sxs-lookup"><span data-stu-id="b3ae2-117">Retrieve the enrolled courses for the selected student based on a value in the query string</span></span>
3. <span data-ttu-id="b3ae2-118">Adicionar um hiperlink com um valor de cadeia de caracteres de consulta do modo de exibição de grade para a nova página</span><span class="sxs-lookup"><span data-stu-id="b3ae2-118">Add a hyperlink with a query string value from the grid view to the new page</span></span>

<span data-ttu-id="b3ae2-119">As etapas neste tutorial são bastante semelhantes ao que você fez no anteriores [tutorial](sorting-paging-and-filtering-data.md) para filtrar os alunos exibidos com base na seleção do usuário em uma lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="b3ae2-119">The steps in this tutorial are fairly similar to what you did in the earlier [tutorial](sorting-paging-and-filtering-data.md) to filter the displayed students based on the user selection in a drop down list.</span></span> <span data-ttu-id="b3ae2-120">Neste tutorial, você usou o **controle** atributo do método select para especificar que o valor do parâmetro vêm de um controle.</span><span class="sxs-lookup"><span data-stu-id="b3ae2-120">In that tutorial, you used the **Control** attribute in the select method to specify that the parameter value comes from a control.</span></span> <span data-ttu-id="b3ae2-121">Neste tutorial, você usará o **QueryString** atributo do método select para especificar que o valor do parâmetro é proveniente de cadeia de caracteres de consulta.</span><span class="sxs-lookup"><span data-stu-id="b3ae2-121">In this tutorial, you'll use the **QueryString** attribute in the select method to specify that the parameter value comes from the query string.</span></span>

## <a name="add-new-page-for-displaying-a-students-courses"></a><span data-ttu-id="b3ae2-122">Adicionar nova página para exibir os cursos de student</span><span class="sxs-lookup"><span data-stu-id="b3ae2-122">Add new page for displaying a student's courses</span></span>

<span data-ttu-id="b3ae2-123">Adicionar um novo formulário da web que usa a página mestra Site.master e nomeie a página **cursos**.</span><span class="sxs-lookup"><span data-stu-id="b3ae2-123">Add a new web form that uses the Site.master master page, and name the page **Courses**.</span></span>

<span data-ttu-id="b3ae2-124">No **Courses.aspx** de arquivo, adicione uma exibição de grade para exibir os cursos do aluno selecionado.</span><span class="sxs-lookup"><span data-stu-id="b3ae2-124">In the **Courses.aspx** file, add a grid view to display the courses for the selected student.</span></span>

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample1.aspx)]

## <a name="define-the-select-method"></a><span data-ttu-id="b3ae2-125">Definir o método select</span><span class="sxs-lookup"><span data-stu-id="b3ae2-125">Define the select method</span></span>

<span data-ttu-id="b3ae2-126">Em **Courses.aspx.cs**, você irá adicionar método select com o nome especificado na exibição de grade **SelectMethod** propriedade.</span><span class="sxs-lookup"><span data-stu-id="b3ae2-126">In **Courses.aspx.cs**, you will add the select method with the name you specified in the grid view's **SelectMethod** property.</span></span> <span data-ttu-id="b3ae2-127">Nesse método, você vai definir a consulta para recuperar os cursos student e especificar que o parâmetro vem de um valor de cadeia de caracteres de consulta com o mesmo nome que o parâmetro.</span><span class="sxs-lookup"><span data-stu-id="b3ae2-127">In that method, you'll define the query for retrieving a student's courses, and specify that the parameter comes from a query string value with the same name as the parameter.</span></span>

<span data-ttu-id="b3ae2-128">Primeiro, você deve adicionar o seguinte **usando** instruções.</span><span class="sxs-lookup"><span data-stu-id="b3ae2-128">First, you must add the following **using** statements.</span></span>

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample2.cs)]

<span data-ttu-id="b3ae2-129">Em seguida, adicione o seguinte código para Courses.aspx.cs:</span><span class="sxs-lookup"><span data-stu-id="b3ae2-129">Then, add the following code to Courses.aspx.cs:</span></span>

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample3.cs)]

<span data-ttu-id="b3ae2-130">O atributo de QueryString significa que um valor de cadeia de caracteres de consulta chamado StudentID é automaticamente atribuído ao parâmetro nesse método.</span><span class="sxs-lookup"><span data-stu-id="b3ae2-130">The QueryString attribute means that a query string value named StudentID is automatically assigned to the parameter in this method.</span></span>

## <a name="add-hyperlink-with-query-string-value"></a><span data-ttu-id="b3ae2-131">Adicionar hiperlink com valor de cadeia de caracteres de consulta</span><span class="sxs-lookup"><span data-stu-id="b3ae2-131">Add hyperlink with query string value</span></span>

<span data-ttu-id="b3ae2-132">O modo de exibição de grade em Students.aspx, você adicionará um campo de hiperlink que contém links para a nova página de cursos.</span><span class="sxs-lookup"><span data-stu-id="b3ae2-132">In the grid view on Students.aspx, you will add a hyperlink field that links to your new Courses page.</span></span> <span data-ttu-id="b3ae2-133">O hiperlink incluirá um valor de cadeia de caracteres de consulta com a identificação do aluno.</span><span class="sxs-lookup"><span data-stu-id="b3ae2-133">The hyperlink will include a query string value with the student's id.</span></span>

<span data-ttu-id="b3ae2-134">No Students.aspx, adicione o campo a seguir para as colunas de exibição de grade logo abaixo do campo de Total de créditos.</span><span class="sxs-lookup"><span data-stu-id="b3ae2-134">In Students.aspx, add the following field to the grid view columns just below the field for Total Credits.</span></span>

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample4.aspx?highlight=7-8)]

<span data-ttu-id="b3ae2-135">Execute o aplicativo e observe que a exibição de grade agora inclui o link de cursos.</span><span class="sxs-lookup"><span data-stu-id="b3ae2-135">Run the application and notice that the grid view now includes the Courses link.</span></span>

![Adicionar hiperlink](using-query-string-values-to-retrieve-data/_static/image1.png)

<span data-ttu-id="b3ae2-137">Quando você clica em um dos links, você verá os cursos registrados que student.</span><span class="sxs-lookup"><span data-stu-id="b3ae2-137">When you click one of the links, you'll see that student's enrolled courses.</span></span>

![Mostrar cursos](using-query-string-values-to-retrieve-data/_static/image2.png)

## <a name="conclusion"></a><span data-ttu-id="b3ae2-139">Conclusão</span><span class="sxs-lookup"><span data-stu-id="b3ae2-139">Conclusion</span></span>

<span data-ttu-id="b3ae2-140">Neste tutorial, você adicionou um link com um valor de cadeia de caracteres de consulta.</span><span class="sxs-lookup"><span data-stu-id="b3ae2-140">In this tutorial, you added a link with a query string value.</span></span> <span data-ttu-id="b3ae2-141">Você usou esse valor de cadeia de caracteres de consulta para o valor do parâmetro do método select.</span><span class="sxs-lookup"><span data-stu-id="b3ae2-141">You used that query string value for the parameter value in the select method.</span></span>

<span data-ttu-id="b3ae2-142">Na próxima [tutorial](adding-business-logic-layer.md), você moverá o código dos arquivos code-behind em uma camada de lógica de negócios e uma camada de acesso a dados.</span><span class="sxs-lookup"><span data-stu-id="b3ae2-142">In the next [tutorial](adding-business-logic-layer.md), you will move the code from the code-behind files into a business logic layer and a data access layer.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b3ae2-143">[Anterior](integrating-jquery-ui.md)
> [Próximo](adding-business-logic-layer.md)</span><span class="sxs-lookup"><span data-stu-id="b3ae2-143">[Previous](integrating-jquery-ui.md)
[Next](adding-business-logic-layer.md)</span></span>
