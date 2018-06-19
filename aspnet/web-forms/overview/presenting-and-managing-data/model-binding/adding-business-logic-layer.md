---
uid: web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
title: Adicionar camada de lógica de negócios para um projeto que usa o modelo de associação e formulários da web | Microsoft Docs
author: tfitzmac
description: Esta série de tutoriais demonstra aspectos básicos do uso de associação de modelo com um projeto Web Forms do ASP.NET. Associação de modelo torna a interação de dados mais estreita-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 7ef664b3-1cc8-4cbf-bb18-9f0f3a3ada2b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
msc.type: authoredcontent
ms.openlocfilehash: 25e887bdc316abf65c780bb6c8d075e938e85064
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892745"
---
<a name="adding-business-logic-layer-to-a-project-that-uses-model-binding-and-web-forms"></a><span data-ttu-id="5601e-104">Adicionar camada de lógica de negócios para um projeto que usa o modelo de associação e formulários da web</span><span class="sxs-lookup"><span data-stu-id="5601e-104">Adding business logic layer to a project that uses model binding and web forms</span></span>
====================
<span data-ttu-id="5601e-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="5601e-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="5601e-106">Esta série de tutoriais demonstra aspectos básicos do uso de associação de modelo com um projeto Web Forms do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5601e-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="5601e-107">Associação de modelo facilita a interação de dados mais direta de lidar com dados de objetos de origem (como ObjectDataSource ou SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="5601e-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="5601e-108">Esta série começa com material introdutório e move para conceitos mais avançados em tutoriais subsequentes.</span><span class="sxs-lookup"><span data-stu-id="5601e-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="5601e-109">Este tutorial mostra como usar a associação de modelo com uma camada de lógica de negócios.</span><span class="sxs-lookup"><span data-stu-id="5601e-109">This tutorial shows how to use model binding with a business logic layer.</span></span> <span data-ttu-id="5601e-110">Defina o membro OnCallingDataMethods para especificar que um objeto que não sejam a página atual é usado para chamar os métodos de dados.</span><span class="sxs-lookup"><span data-stu-id="5601e-110">You will set the OnCallingDataMethods member to specify that an object other than the current page is used to call the data methods.</span></span>
> 
> <span data-ttu-id="5601e-111">Este tutorial baseia-se no projeto criado no [anteriores](retrieving-data.md) partes da série.</span><span class="sxs-lookup"><span data-stu-id="5601e-111">This tutorial builds on the project created in the [earlier](retrieving-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="5601e-112">Você pode [baixar](https://go.microsoft.com/fwlink/?LinkId=286116) o projeto completo em c# ou VB.</span><span class="sxs-lookup"><span data-stu-id="5601e-112">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="5601e-113">O código para download funciona com o Visual Studio 2012 ou Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="5601e-113">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="5601e-114">Ele usa o modelo do Visual Studio 2012, que é ligeiramente diferente do que o modelo do Visual Studio 2013 mostrado neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="5601e-114">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="5601e-115">Será possível compilar</span><span class="sxs-lookup"><span data-stu-id="5601e-115">What you'll build</span></span>

<span data-ttu-id="5601e-116">Associação de modelo permite que você coloque o código de interação de dados em que o arquivo de code-behind para uma página da web ou em uma classe de lógica de negócios diferentes.</span><span class="sxs-lookup"><span data-stu-id="5601e-116">Model binding enables you to put your data interaction code in either the code-behind file for a web page or in a separate business logic class.</span></span> <span data-ttu-id="5601e-117">Os tutoriais anteriores mostraram como usar os arquivos code-behind para o código de interação de dados.</span><span class="sxs-lookup"><span data-stu-id="5601e-117">The previous tutorials have shown how to use the code-behind files for data interaction code.</span></span> <span data-ttu-id="5601e-118">Essa abordagem funciona para pequenos sites, mas pode levar a repetição e a maior dificuldade de código durante a manutenção de um site grande.</span><span class="sxs-lookup"><span data-stu-id="5601e-118">This approach works for small sites but it can lead to code repetition and greater difficulty when maintaining a large site.</span></span> <span data-ttu-id="5601e-119">Ele também pode ser muito difícil testar o código que reside no code-behind arquivos porque não há nenhuma camada de abstração programaticamente.</span><span class="sxs-lookup"><span data-stu-id="5601e-119">It can also be very difficult to programmatically test code that resides in code behind files because there is no abstraction layer.</span></span>

<span data-ttu-id="5601e-120">Para centralizar o código de interação de dados, você pode criar uma camada de lógica de negócios que contém toda a lógica para interagir com dados.</span><span class="sxs-lookup"><span data-stu-id="5601e-120">To centralize the data interaction code, you can create a business logic layer that contains all of the logic for interacting with data.</span></span> <span data-ttu-id="5601e-121">Chame a camada de lógica de negócios de suas páginas da web.</span><span class="sxs-lookup"><span data-stu-id="5601e-121">You then call the business logic layer from your web pages.</span></span> <span data-ttu-id="5601e-122">Este tutorial mostra como mover todo o código que você tenha escrito nos tutoriais anteriores em uma camada de lógica de negócios e, em seguida, usar esse código das páginas.</span><span class="sxs-lookup"><span data-stu-id="5601e-122">This tutorial shows how to move all of the code you have written in the previous tutorials into a business logic layer, and then use that code from the pages.</span></span>

<span data-ttu-id="5601e-123">Neste tutorial, você vai:</span><span class="sxs-lookup"><span data-stu-id="5601e-123">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="5601e-124">Mova o código de arquivos code-behind para uma camada de lógica de negócios</span><span class="sxs-lookup"><span data-stu-id="5601e-124">Move the code from code-behind files to a business logic layer</span></span>
2. <span data-ttu-id="5601e-125">Alterar os controles de dados vinculados para chamar os métodos na camada de lógica de negócios</span><span class="sxs-lookup"><span data-stu-id="5601e-125">Change your data bound controls to call the methods in the business logic layer</span></span>

## <a name="create-business-logic-layer"></a><span data-ttu-id="5601e-126">Criar camada de lógica comercial</span><span class="sxs-lookup"><span data-stu-id="5601e-126">Create business logic layer</span></span>

<span data-ttu-id="5601e-127">Agora, você criará a classe que é chamada de páginas da web.</span><span class="sxs-lookup"><span data-stu-id="5601e-127">Now, you will create the class that is called from the web pages.</span></span> <span data-ttu-id="5601e-128">Os métodos dessa classe semelhante para os métodos usados nos tutoriais anteriores e incluem os atributos de provedor de valor.</span><span class="sxs-lookup"><span data-stu-id="5601e-128">The methods in this class look similar to the methods you used in the previous tutorials and include the value provider attributes.</span></span>

<span data-ttu-id="5601e-129">Primeiro, adicione uma nova pasta chamada **BLL**.</span><span class="sxs-lookup"><span data-stu-id="5601e-129">First, add a new folder called **BLL**.</span></span>

![Adicionar pasta](adding-business-logic-layer/_static/image1.png)

<span data-ttu-id="5601e-131">Na pasta BLL, crie uma nova classe chamada **SchoolBL.cs**.</span><span class="sxs-lookup"><span data-stu-id="5601e-131">In the BLL folder, create a new class named **SchoolBL.cs**.</span></span> <span data-ttu-id="5601e-132">Ele conterá todas as operações de dados residiam originalmente em arquivos code-behind.</span><span class="sxs-lookup"><span data-stu-id="5601e-132">It will contain all of the data operations that originally resided in code-behind files.</span></span> <span data-ttu-id="5601e-133">Os métodos são quase o mesmo que os métodos no arquivo code-behind, mas incluem algumas alterações.</span><span class="sxs-lookup"><span data-stu-id="5601e-133">The methods are almost the same as the methods in the code-behind file, but will include some changes.</span></span>

<span data-ttu-id="5601e-134">A alteração mais importante a observar é que você não estiver executando o código de dentro de uma instância de **página** classe.</span><span class="sxs-lookup"><span data-stu-id="5601e-134">The most important change to note is that you are no longer executing the code from within an instance of **Page** class.</span></span> <span data-ttu-id="5601e-135">A classe da página contém o **TryUpdateModel** método e o **ModelState** propriedade.</span><span class="sxs-lookup"><span data-stu-id="5601e-135">The Page class contains the **TryUpdateModel** method and the **ModelState** property.</span></span> <span data-ttu-id="5601e-136">Quando esse código é movido para uma camada de lógica de negócios, você não tiver uma instância da classe de página para chamar esses membros.</span><span class="sxs-lookup"><span data-stu-id="5601e-136">When this code is moved to a business logic layer, you no longer have an instance of the Page class to call these members.</span></span> <span data-ttu-id="5601e-137">Para contornar esse problema, você deve adicionar um **ModelMethodContext** parâmetro para qualquer método que acessa TryUpdateModel ou ModelState.</span><span class="sxs-lookup"><span data-stu-id="5601e-137">To get around this issue, you must add a **ModelMethodContext** parameter to any method that accesses TryUpdateModel or ModelState.</span></span> <span data-ttu-id="5601e-138">Você pode usar esse parâmetro ModelMethodContext para chamar TryUpdateModel ou recuperar ModelState.</span><span class="sxs-lookup"><span data-stu-id="5601e-138">You use this ModelMethodContext parameter to call TryUpdateModel or retrieve ModelState.</span></span> <span data-ttu-id="5601e-139">Você não precisa alterar nada na página da web para a conta para esse novo parâmetro.</span><span class="sxs-lookup"><span data-stu-id="5601e-139">You do not need to change anything in the web page to account for this new parameter.</span></span>

<span data-ttu-id="5601e-140">Substitua o código em SchoolBL.cs com o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="5601e-140">Replace the code in SchoolBL.cs with the following code.</span></span>

[!code-csharp[Main](adding-business-logic-layer/samples/sample1.cs)]

## <a name="revise-existing-pages-to-retrieve-data-from-business-logic-layer"></a><span data-ttu-id="5601e-141">Revisar páginas existentes para recuperar dados da camada de lógica comercial</span><span class="sxs-lookup"><span data-stu-id="5601e-141">Revise existing pages to retrieve data from business logic layer</span></span>

<span data-ttu-id="5601e-142">Por fim, você converterá as páginas Students.aspx, AddStudent.aspx e Courses.aspx uso de consultas o arquivo code-behind para usar a camada de lógica de negócios.</span><span class="sxs-lookup"><span data-stu-id="5601e-142">Finally, you will convert the pages Students.aspx, AddStudent.aspx, and Courses.aspx from using queries in the code-behind file to using the business logic layer.</span></span>

<span data-ttu-id="5601e-143">Em que os arquivos code-behind para estudantes, AddStudent e cursos, exclua ou comente a métodos de consulta a seguir:</span><span class="sxs-lookup"><span data-stu-id="5601e-143">In the code-behind files for Students, AddStudent, and Courses, delete or comment out the following query methods:</span></span>

- <span data-ttu-id="5601e-144">studentsGrid\_GetData</span><span class="sxs-lookup"><span data-stu-id="5601e-144">studentsGrid\_GetData</span></span>
- <span data-ttu-id="5601e-145">studentsGrid\_UpdateItem</span><span class="sxs-lookup"><span data-stu-id="5601e-145">studentsGrid\_UpdateItem</span></span>
- <span data-ttu-id="5601e-146">studentsGrid\_DeleteItem</span><span class="sxs-lookup"><span data-stu-id="5601e-146">studentsGrid\_DeleteItem</span></span>
- <span data-ttu-id="5601e-147">addStudentForm\_InsertItem</span><span class="sxs-lookup"><span data-stu-id="5601e-147">addStudentForm\_InsertItem</span></span>
- <span data-ttu-id="5601e-148">coursesGrid\_GetData</span><span class="sxs-lookup"><span data-stu-id="5601e-148">coursesGrid\_GetData</span></span>

<span data-ttu-id="5601e-149">Agora você não deve ter nenhum código no arquivo de code-behind que pertencem a operações de dados.</span><span class="sxs-lookup"><span data-stu-id="5601e-149">You now should have no code in the code-behind file that pertains to data operations.</span></span>

<span data-ttu-id="5601e-150">O **OnCallingDataMethods** manipulador de eventos permite que você especifique um objeto a ser usado para os métodos de dados.</span><span class="sxs-lookup"><span data-stu-id="5601e-150">The **OnCallingDataMethods** event handler enables you to specify an object to use for the data methods.</span></span> <span data-ttu-id="5601e-151">No Students.aspx, adicione um valor para o manipulador de eventos e altere os nomes dos métodos para os nomes dos métodos na classe de lógica de negócios.</span><span class="sxs-lookup"><span data-stu-id="5601e-151">In Students.aspx, add a value for that event handler and change the names of the data methods to the names of the methods in the business logic class.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample2.aspx?highlight=3-4,8)]

<span data-ttu-id="5601e-152">No arquivo de code-behind para Students.aspx, defina o manipulador de eventos para o evento CallingDataMethods.</span><span class="sxs-lookup"><span data-stu-id="5601e-152">In the code-behind file for Students.aspx, define the event handler for the CallingDataMethods event.</span></span> <span data-ttu-id="5601e-153">No manipulador de eventos, você deve especificar a classe de lógica de negócios para operações de dados.</span><span class="sxs-lookup"><span data-stu-id="5601e-153">In this event handler, you specify the business logic class for data operations.</span></span>

[!code-csharp[Main](adding-business-logic-layer/samples/sample3.cs)]

<span data-ttu-id="5601e-154">Em AddStudent.aspx, fazer alterações semelhantes.</span><span class="sxs-lookup"><span data-stu-id="5601e-154">In AddStudent.aspx, make similar changes.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample4.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample5.cs)]

<span data-ttu-id="5601e-155">Em Courses.aspx, fazer alterações semelhantes.</span><span class="sxs-lookup"><span data-stu-id="5601e-155">In Courses.aspx, make similar changes.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample6.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample7.cs)]

<span data-ttu-id="5601e-156">Execute o aplicativo e observe que todas as páginas funcionem como tinham anteriormente.</span><span class="sxs-lookup"><span data-stu-id="5601e-156">Run the application and notice that all of the pages function as they had previously.</span></span> <span data-ttu-id="5601e-157">A lógica de validação também funciona corretamente.</span><span class="sxs-lookup"><span data-stu-id="5601e-157">The validation logic also works correctly.</span></span>

## <a name="conclusion"></a><span data-ttu-id="5601e-158">Conclusão</span><span class="sxs-lookup"><span data-stu-id="5601e-158">Conclusion</span></span>

<span data-ttu-id="5601e-159">Neste tutorial, você estruturado novamente seu aplicativo para usar uma camada de acesso a dados e a camada de lógica comercial.</span><span class="sxs-lookup"><span data-stu-id="5601e-159">In this tutorial, you re-structured your application to use a data access layer and business logic layer.</span></span> <span data-ttu-id="5601e-160">Você especificou que os controles de dados usam um objeto que não é a página atual para operações de dados.</span><span class="sxs-lookup"><span data-stu-id="5601e-160">You specified that the data controls use an object that is not the current page for data operations.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="5601e-161">Anterior</span><span class="sxs-lookup"><span data-stu-id="5601e-161">Previous</span></span>](using-query-string-values-to-retrieve-data.md)
