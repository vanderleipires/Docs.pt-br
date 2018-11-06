---
uid: web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
title: Adicionar camada de lógica comercial para um projeto que usa a associação de modelos e formulários da web | Microsoft Docs
author: Rick-Anderson
description: Esta série de tutoriais demonstra aspectos básicos de como usar a associação de modelo com um projeto de Web Forms do ASP.NET. Associação de modelo torna a interação de dados mais simples-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 7ef664b3-1cc8-4cbf-bb18-9f0f3a3ada2b
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
msc.type: authoredcontent
ms.openlocfilehash: 4ba1830ce51f257e18880f752b249dbeb77f504e
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021645"
---
<a name="adding-business-logic-layer-to-a-project-that-uses-model-binding-and-web-forms"></a><span data-ttu-id="b06b2-104">Adicionar camada de lógica comercial para um projeto que usa a associação de modelos e formulários da web</span><span class="sxs-lookup"><span data-stu-id="b06b2-104">Adding business logic layer to a project that uses model binding and web forms</span></span>
====================
<span data-ttu-id="b06b2-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="b06b2-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="b06b2-106">Esta série de tutoriais demonstra aspectos básicos de como usar a associação de modelo com um projeto de Web Forms do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b06b2-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="b06b2-107">Associação de modelo torna a interação de dados mais simples que lidam com dados de objetos de origem (como ObjectDataSource ou SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="b06b2-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="b06b2-108">Esta série começa com material introdutório e move para conceitos mais avançados em tutoriais posteriores.</span><span class="sxs-lookup"><span data-stu-id="b06b2-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="b06b2-109">Este tutorial mostra como usar a associação de modelo com uma camada de lógica de negócios.</span><span class="sxs-lookup"><span data-stu-id="b06b2-109">This tutorial shows how to use model binding with a business logic layer.</span></span> <span data-ttu-id="b06b2-110">Você definirá o membro OnCallingDataMethods para especificar que um objeto que não seja a página atual é usado para chamar os métodos de dados.</span><span class="sxs-lookup"><span data-stu-id="b06b2-110">You will set the OnCallingDataMethods member to specify that an object other than the current page is used to call the data methods.</span></span>
> 
> <span data-ttu-id="b06b2-111">Este tutorial se baseia no projeto criado a [anteriores](retrieving-data.md) partes da série.</span><span class="sxs-lookup"><span data-stu-id="b06b2-111">This tutorial builds on the project created in the [earlier](retrieving-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="b06b2-112">Você pode [baixar](https://go.microsoft.com/fwlink/?LinkId=286116) o projeto completo em c# ou VB.</span><span class="sxs-lookup"><span data-stu-id="b06b2-112">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="b06b2-113">O código para download funciona com o Visual Studio 2012 ou Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="b06b2-113">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="b06b2-114">Ele usa o modelo do Visual Studio 2012, que é ligeiramente diferente do que o modelo do Visual Studio 2013 mostrado neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="b06b2-114">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="b06b2-115">O que você vai criar</span><span class="sxs-lookup"><span data-stu-id="b06b2-115">What you'll build</span></span>

<span data-ttu-id="b06b2-116">Associação de modelos permite que você coloque o código de interação de dados em que o arquivo de code-behind para uma página da web ou em uma classe de lógica de negócios diferentes.</span><span class="sxs-lookup"><span data-stu-id="b06b2-116">Model binding enables you to put your data interaction code in either the code-behind file for a web page or in a separate business logic class.</span></span> <span data-ttu-id="b06b2-117">Os tutoriais anteriores mostraram como usar os arquivos code-behind para o código de interação de dados.</span><span class="sxs-lookup"><span data-stu-id="b06b2-117">The previous tutorials have shown how to use the code-behind files for data interaction code.</span></span> <span data-ttu-id="b06b2-118">Essa abordagem funciona para sites pequenos, mas pode levar a repetições e a maior dificuldade de código durante a manutenção de um site grande.</span><span class="sxs-lookup"><span data-stu-id="b06b2-118">This approach works for small sites but it can lead to code repetition and greater difficulty when maintaining a large site.</span></span> <span data-ttu-id="b06b2-119">Ele também pode ser muito difícil de testar o código que reside no code-behind arquivos porque não há nenhuma camada de abstração programaticamente.</span><span class="sxs-lookup"><span data-stu-id="b06b2-119">It can also be very difficult to programmatically test code that resides in code behind files because there is no abstraction layer.</span></span>

<span data-ttu-id="b06b2-120">Para centralizar o código de interação de dados, você pode criar uma camada de lógica de negócios que contém toda a lógica para interagir com dados.</span><span class="sxs-lookup"><span data-stu-id="b06b2-120">To centralize the data interaction code, you can create a business logic layer that contains all of the logic for interacting with data.</span></span> <span data-ttu-id="b06b2-121">Você, em seguida, chama a camada de lógica comercial de suas páginas da web.</span><span class="sxs-lookup"><span data-stu-id="b06b2-121">You then call the business logic layer from your web pages.</span></span> <span data-ttu-id="b06b2-122">Este tutorial mostra como mover todo o código que você tenha escrito nos tutoriais anteriores em uma camada de lógica de negócios e, em seguida, usar esse código nas páginas.</span><span class="sxs-lookup"><span data-stu-id="b06b2-122">This tutorial shows how to move all of the code you have written in the previous tutorials into a business logic layer, and then use that code from the pages.</span></span>

<span data-ttu-id="b06b2-123">Neste tutorial, você vai:</span><span class="sxs-lookup"><span data-stu-id="b06b2-123">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="b06b2-124">Mover o código de arquivos code-behind para uma camada de lógica de negócios</span><span class="sxs-lookup"><span data-stu-id="b06b2-124">Move the code from code-behind files to a business logic layer</span></span>
2. <span data-ttu-id="b06b2-125">Alterar os controles ligados a dados para chamar os métodos na camada de lógica de negócios</span><span class="sxs-lookup"><span data-stu-id="b06b2-125">Change your data bound controls to call the methods in the business logic layer</span></span>

## <a name="create-business-logic-layer"></a><span data-ttu-id="b06b2-126">Criar camada de lógica comercial</span><span class="sxs-lookup"><span data-stu-id="b06b2-126">Create business logic layer</span></span>

<span data-ttu-id="b06b2-127">Agora, você criará a classe que é chamada de páginas da web.</span><span class="sxs-lookup"><span data-stu-id="b06b2-127">Now, you will create the class that is called from the web pages.</span></span> <span data-ttu-id="b06b2-128">Os métodos nessa classe semelhante aos métodos usados nos tutoriais anteriores e incluem os atributos de provedor de valor.</span><span class="sxs-lookup"><span data-stu-id="b06b2-128">The methods in this class look similar to the methods you used in the previous tutorials and include the value provider attributes.</span></span>

<span data-ttu-id="b06b2-129">Primeiro, adicione uma nova pasta denominada **BLL**.</span><span class="sxs-lookup"><span data-stu-id="b06b2-129">First, add a new folder called **BLL**.</span></span>

![Adicionar pasta](adding-business-logic-layer/_static/image1.png)

<span data-ttu-id="b06b2-131">Na pasta BLL, crie uma nova classe chamada **SchoolBL.cs**.</span><span class="sxs-lookup"><span data-stu-id="b06b2-131">In the BLL folder, create a new class named **SchoolBL.cs**.</span></span> <span data-ttu-id="b06b2-132">Ela conterá todas as operações de dados residiam originalmente em arquivos code-behind.</span><span class="sxs-lookup"><span data-stu-id="b06b2-132">It will contain all of the data operations that originally resided in code-behind files.</span></span> <span data-ttu-id="b06b2-133">Os métodos são quase os mesmos que os métodos no arquivo code-behind, mas incluem algumas alterações.</span><span class="sxs-lookup"><span data-stu-id="b06b2-133">The methods are almost the same as the methods in the code-behind file, but will include some changes.</span></span>

<span data-ttu-id="b06b2-134">A alteração mais importante a observar é que você está executando não é mais o código de dentro de uma instância de **página** classe.</span><span class="sxs-lookup"><span data-stu-id="b06b2-134">The most important change to note is that you are no longer executing the code from within an instance of **Page** class.</span></span> <span data-ttu-id="b06b2-135">A classe de página contém o **TryUpdateModel** método e o **ModelState** propriedade.</span><span class="sxs-lookup"><span data-stu-id="b06b2-135">The Page class contains the **TryUpdateModel** method and the **ModelState** property.</span></span> <span data-ttu-id="b06b2-136">Quando esse código é movido para uma camada de lógica de negócios, você não precisa de uma instância da classe de página para chamar esses membros.</span><span class="sxs-lookup"><span data-stu-id="b06b2-136">When this code is moved to a business logic layer, you no longer have an instance of the Page class to call these members.</span></span> <span data-ttu-id="b06b2-137">Para contornar esse problema, você deve adicionar uma **ModelMethodContext** parâmetro para qualquer método que acessa TryUpdateModel ou ModelState.</span><span class="sxs-lookup"><span data-stu-id="b06b2-137">To get around this issue, you must add a **ModelMethodContext** parameter to any method that accesses TryUpdateModel or ModelState.</span></span> <span data-ttu-id="b06b2-138">Você pode usar esse parâmetro ModelMethodContext para chamar TryUpdateModel ou recuperar ModelState.</span><span class="sxs-lookup"><span data-stu-id="b06b2-138">You use this ModelMethodContext parameter to call TryUpdateModel or retrieve ModelState.</span></span> <span data-ttu-id="b06b2-139">Você não precisa alterar nada na página da web para levar em conta esse novo parâmetro.</span><span class="sxs-lookup"><span data-stu-id="b06b2-139">You do not need to change anything in the web page to account for this new parameter.</span></span>

<span data-ttu-id="b06b2-140">Substitua o código no SchoolBL.cs com o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="b06b2-140">Replace the code in SchoolBL.cs with the following code.</span></span>

[!code-csharp[Main](adding-business-logic-layer/samples/sample1.cs)]

## <a name="revise-existing-pages-to-retrieve-data-from-business-logic-layer"></a><span data-ttu-id="b06b2-141">Revisar páginas existentes para recuperar dados da camada de lógica comercial</span><span class="sxs-lookup"><span data-stu-id="b06b2-141">Revise existing pages to retrieve data from business logic layer</span></span>

<span data-ttu-id="b06b2-142">Por fim, você converterá as páginas Students.aspx, AddStudent.aspx e Courses.aspx do uso de consultas no código code-behind usando a camada de lógica de negócios.</span><span class="sxs-lookup"><span data-stu-id="b06b2-142">Finally, you will convert the pages Students.aspx, AddStudent.aspx, and Courses.aspx from using queries in the code-behind file to using the business logic layer.</span></span>

<span data-ttu-id="b06b2-143">Em que os arquivos code-behind para estudantes, AddStudent e cursos, exclua ou comente os métodos de consulta a seguir:</span><span class="sxs-lookup"><span data-stu-id="b06b2-143">In the code-behind files for Students, AddStudent, and Courses, delete or comment out the following query methods:</span></span>

- <span data-ttu-id="b06b2-144">studentsGrid\_GetData</span><span class="sxs-lookup"><span data-stu-id="b06b2-144">studentsGrid\_GetData</span></span>
- <span data-ttu-id="b06b2-145">studentsGrid\_UpdateItem</span><span class="sxs-lookup"><span data-stu-id="b06b2-145">studentsGrid\_UpdateItem</span></span>
- <span data-ttu-id="b06b2-146">studentsGrid\_DeleteItem</span><span class="sxs-lookup"><span data-stu-id="b06b2-146">studentsGrid\_DeleteItem</span></span>
- <span data-ttu-id="b06b2-147">addStudentForm\_InsertItem</span><span class="sxs-lookup"><span data-stu-id="b06b2-147">addStudentForm\_InsertItem</span></span>
- <span data-ttu-id="b06b2-148">coursesGrid\_GetData</span><span class="sxs-lookup"><span data-stu-id="b06b2-148">coursesGrid\_GetData</span></span>

<span data-ttu-id="b06b2-149">Agora você não deve ter nenhum código no arquivo de code-behind referente às operações de dados.</span><span class="sxs-lookup"><span data-stu-id="b06b2-149">You now should have no code in the code-behind file that pertains to data operations.</span></span>

<span data-ttu-id="b06b2-150">O **OnCallingDataMethods** manipulador de eventos permite que você especifique um objeto a ser usado para os métodos de dados.</span><span class="sxs-lookup"><span data-stu-id="b06b2-150">The **OnCallingDataMethods** event handler enables you to specify an object to use for the data methods.</span></span> <span data-ttu-id="b06b2-151">No Students.aspx, adicione um valor para o manipulador de eventos e altere os nomes dos métodos de dados para os nomes dos métodos na classe de lógica de negócios.</span><span class="sxs-lookup"><span data-stu-id="b06b2-151">In Students.aspx, add a value for that event handler and change the names of the data methods to the names of the methods in the business logic class.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample2.aspx?highlight=3-4,8)]

<span data-ttu-id="b06b2-152">No arquivo de code-behind para Students.aspx, defina o manipulador de eventos para o evento CallingDataMethods.</span><span class="sxs-lookup"><span data-stu-id="b06b2-152">In the code-behind file for Students.aspx, define the event handler for the CallingDataMethods event.</span></span> <span data-ttu-id="b06b2-153">No manipulador de eventos, você especifica a classe de lógica de negócios para operações de dados.</span><span class="sxs-lookup"><span data-stu-id="b06b2-153">In this event handler, you specify the business logic class for data operations.</span></span>

[!code-csharp[Main](adding-business-logic-layer/samples/sample3.cs)]

<span data-ttu-id="b06b2-154">No AddStudent.aspx, fazer alterações semelhantes.</span><span class="sxs-lookup"><span data-stu-id="b06b2-154">In AddStudent.aspx, make similar changes.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample4.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample5.cs)]

<span data-ttu-id="b06b2-155">No Courses.aspx, fazer alterações semelhantes.</span><span class="sxs-lookup"><span data-stu-id="b06b2-155">In Courses.aspx, make similar changes.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample6.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample7.cs)]

<span data-ttu-id="b06b2-156">Executar o aplicativo e observe que todas as páginas funcionam conforme eles tinham anteriormente.</span><span class="sxs-lookup"><span data-stu-id="b06b2-156">Run the application and notice that all of the pages function as they had previously.</span></span> <span data-ttu-id="b06b2-157">A lógica de validação também funciona corretamente.</span><span class="sxs-lookup"><span data-stu-id="b06b2-157">The validation logic also works correctly.</span></span>

## <a name="conclusion"></a><span data-ttu-id="b06b2-158">Conclusão</span><span class="sxs-lookup"><span data-stu-id="b06b2-158">Conclusion</span></span>

<span data-ttu-id="b06b2-159">Neste tutorial, você estruturados novamente seu aplicativo para usar uma camada de acesso a dados e a camada de lógica comercial.</span><span class="sxs-lookup"><span data-stu-id="b06b2-159">In this tutorial, you re-structured your application to use a data access layer and business logic layer.</span></span> <span data-ttu-id="b06b2-160">Você especificou que os controles de dados usam um objeto que não é a página atual para operações de dados.</span><span class="sxs-lookup"><span data-stu-id="b06b2-160">You specified that the data controls use an object that is not the current page for data operations.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="b06b2-161">Anterior</span><span class="sxs-lookup"><span data-stu-id="b06b2-161">Previous</span></span>](using-query-string-values-to-retrieve-data.md)
