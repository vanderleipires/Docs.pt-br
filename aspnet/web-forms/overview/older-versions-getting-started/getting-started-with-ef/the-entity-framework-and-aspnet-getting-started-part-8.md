---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
title: Introdução ao banco de dados do Entity Framework 4.0 primeiro e o ASP.NET 4 Web Forms - parte 8 | Microsoft Docs
author: tdykstra
description: O aplicativo web de exemplo Contoso University demonstra como criar aplicativos Web Forms do ASP.NET usando o Entity Framework. O aplicativo de exemplo é...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: aaadd9bb-5508-448c-ad57-5497dff90e13
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
msc.type: authoredcontent
ms.openlocfilehash: 035cce022d1b3697b825a96487529dbc9675d90e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887883"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-8"></a><span data-ttu-id="6a912-104">Introdução ao banco de dados do Entity Framework 4.0 primeiro e 4 Web Forms do ASP.NET - parte 8</span><span class="sxs-lookup"><span data-stu-id="6a912-104">Getting Started with Entity Framework 4.0 Database First and ASP.NET 4 Web Forms - Part 8</span></span>
====================
<span data-ttu-id="6a912-105">por [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="6a912-105">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="6a912-106">O aplicativo web de exemplo Contoso University demonstra como criar aplicativos Web Forms do ASP.NET usando o Entity Framework 4.0 e o Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="6a912-106">The Contoso University sample web application demonstrates how to create ASP.NET Web Forms applications using the Entity Framework 4.0 and Visual Studio 2010.</span></span> <span data-ttu-id="6a912-107">Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial da série](the-entity-framework-and-aspnet-getting-started-part-1.md)</span><span class="sxs-lookup"><span data-stu-id="6a912-107">For information about the tutorial series, see [the first tutorial in the series](the-entity-framework-and-aspnet-getting-started-part-1.md)</span></span>


## <a name="using-dynamic-data-functionality-to-format-and-validate-data"></a><span data-ttu-id="6a912-108">Usando a funcionalidade de dados dinâmicos para formatar e validar dados</span><span class="sxs-lookup"><span data-stu-id="6a912-108">Using Dynamic Data Functionality to Format and Validate Data</span></span>

<span data-ttu-id="6a912-109">No tutorial anterior, você implementou os procedimentos armazenados.</span><span class="sxs-lookup"><span data-stu-id="6a912-109">In the previous tutorial you implemented stored procedures.</span></span> <span data-ttu-id="6a912-110">Este tutorial mostra como a funcionalidade de dados dinâmicos pode fornecer os seguintes benefícios:</span><span class="sxs-lookup"><span data-stu-id="6a912-110">This tutorial will show you how Dynamic Data functionality can provide the following benefits:</span></span>

- <span data-ttu-id="6a912-111">Campos são formatados automaticamente de acordo com o tipo de dados de exibição.</span><span class="sxs-lookup"><span data-stu-id="6a912-111">Fields are automatically formatted for display based on their data type.</span></span>
- <span data-ttu-id="6a912-112">Campos são validados automaticamente com base em seu tipo de dados.</span><span class="sxs-lookup"><span data-stu-id="6a912-112">Fields are automatically validated based on their data type.</span></span>
- <span data-ttu-id="6a912-113">Você pode adicionar metadados para o modelo de dados para personalizar o comportamento de formatação e validação.</span><span class="sxs-lookup"><span data-stu-id="6a912-113">You can add metadata to the data model to customize formatting and validation behavior.</span></span> <span data-ttu-id="6a912-114">Quando você fizer isso, você pode adicionar as regras de formatação e validação em um só lugar e automaticamente são aplicados em todos os lugares pode acessar os campos usando controles de dados dinâmicos.</span><span class="sxs-lookup"><span data-stu-id="6a912-114">When you do this, you can add the formatting and validation rules in just one place, and they're automatically applied everywhere you access the fields using Dynamic Data controls.</span></span>

<span data-ttu-id="6a912-115">Para ver como isso funciona, você alterará os controles usados para exibir e editar os campos existentes no *Students.aspx* página e você adicionará formatação e validação de metadados para os campos nome e a data do `Student` tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="6a912-115">To see how this works, you'll change the controls you use to display and edit fields in the existing *Students.aspx* page, and you'll add formatting and validation metadata to the name and date fields of the `Student` entity type.</span></span>

<span data-ttu-id="6a912-116">[![Image01](the-entity-framework-and-aspnet-getting-started-part-8/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6a912-116">[![Image01](the-entity-framework-and-aspnet-getting-started-part-8/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image1.png)</span></span>

## <a name="using-dynamicfield-and-dynamiccontrol-controls"></a><span data-ttu-id="6a912-117">Usando os controles de DynamicControl e DynamicField</span><span class="sxs-lookup"><span data-stu-id="6a912-117">Using DynamicField and DynamicControl Controls</span></span>

<span data-ttu-id="6a912-118">Abra o *Students.aspx* página e no `StudentsGridView` controle substituir o **nome** e **data de inscrição** `TemplateField` elementos com a seguinte marcação:</span><span class="sxs-lookup"><span data-stu-id="6a912-118">Open the *Students.aspx* page and in the `StudentsGridView` control replace the **Name** and **Enrollment Date** `TemplateField` elements with the following markup:</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample1.aspx)]

<span data-ttu-id="6a912-119">Essa marcação usa `DynamicControl` controla no lugar de `TextBox` e `Label` controles na student nome de campo do modelo e, em seguida, usa um `DynamicField` controle para a data de inscrição.</span><span class="sxs-lookup"><span data-stu-id="6a912-119">This markup uses `DynamicControl` controls in place of `TextBox` and `Label` controls in the student name template field, and it uses a `DynamicField` control for the enrollment date.</span></span> <span data-ttu-id="6a912-120">Nenhuma cadeia de caracteres de formato é especificadas.</span><span class="sxs-lookup"><span data-stu-id="6a912-120">No format strings are specified.</span></span>

<span data-ttu-id="6a912-121">Adicionar um `ValidationSummary` controle após o `StudentsGridView` controle.</span><span class="sxs-lookup"><span data-stu-id="6a912-121">Add a `ValidationSummary` control after the `StudentsGridView` control.</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample2.aspx)]

<span data-ttu-id="6a912-122">No `SearchGridView` controle substitua a marcação para o **nome** e **data de inscrição** colunas como você foram no `StudentsGridView` controlar, exceto omitir o `EditItemTemplate` elemento.</span><span class="sxs-lookup"><span data-stu-id="6a912-122">In the `SearchGridView` control replace the markup for the **Name** and **Enrollment Date** columns as you did in the `StudentsGridView` control, except omit the `EditItemTemplate` element.</span></span> <span data-ttu-id="6a912-123">O `Columns` elemento o `SearchGridView` controle agora contém a seguinte marcação:</span><span class="sxs-lookup"><span data-stu-id="6a912-123">The `Columns` element of the `SearchGridView` control now contains the following markup:</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample3.aspx)]

<span data-ttu-id="6a912-124">Abra *Students.aspx.cs* e adicione o seguinte `using` instrução:</span><span class="sxs-lookup"><span data-stu-id="6a912-124">Open *Students.aspx.cs* and add the following `using` statement:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample4.cs)]

<span data-ttu-id="6a912-125">Adicionar um manipulador para a página `Init` evento:</span><span class="sxs-lookup"><span data-stu-id="6a912-125">Add a handler for the page's `Init` event:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample5.cs)]

<span data-ttu-id="6a912-126">Esse código especifica que dados dinâmicos fornecerá formatação e validação nesses controles de associação de dados para campos do `Student` entidade.</span><span class="sxs-lookup"><span data-stu-id="6a912-126">This code specifies that Dynamic Data will provide formatting and validation in these data-bound controls for fields of the `Student` entity.</span></span> <span data-ttu-id="6a912-127">Se você receber uma mensagem de erro semelhante ao seguinte exemplo, quando você executar a página, isso normalmente significa você esqueceu de chamar o `EnableDynamicData` método `Page_Init`:</span><span class="sxs-lookup"><span data-stu-id="6a912-127">If you get an error message like the following example when you run the page, it typically means you've forgotten to call the `EnableDynamicData` method in `Page_Init`:</span></span>

`Could not determine a MetaTable. A MetaTable could not be determined for the data source 'StudentsEntityDataSource' and one could not be inferred from the request URL.`

<span data-ttu-id="6a912-128">Execute a página.</span><span class="sxs-lookup"><span data-stu-id="6a912-128">Run the page.</span></span>

<span data-ttu-id="6a912-129">[![Image03](the-entity-framework-and-aspnet-getting-started-part-8/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="6a912-129">[![Image03](the-entity-framework-and-aspnet-getting-started-part-8/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image3.png)</span></span>

<span data-ttu-id="6a912-130">No **data de inscrição** coluna, a hora é exibida junto com a data, porque o tipo de propriedade é `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="6a912-130">In the **Enrollment Date** column, the time is displayed along with the date because the property type is `DateTime`.</span></span> <span data-ttu-id="6a912-131">Isso será corrigido posteriormente.</span><span class="sxs-lookup"><span data-stu-id="6a912-131">You'll fix that later.</span></span>

<span data-ttu-id="6a912-132">Agora, observe que os dados dinâmicos automaticamente fornece validação de dados básico.</span><span class="sxs-lookup"><span data-stu-id="6a912-132">For now, notice that Dynamic Data automatically provides basic data validation.</span></span> <span data-ttu-id="6a912-133">Por exemplo, clique em **editar**, desmarque o campo de data, clique em **atualização**, e você verá que os dados dinâmicos automaticamente torna isso um campo obrigatório porque o valor não é anulável no modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="6a912-133">For example, click **Edit**, clear the date field, click **Update**, and you see that Dynamic Data automatically makes this a required field because the value is not nullable in the data model.</span></span> <span data-ttu-id="6a912-134">A página exibe um asterisco após o campo e uma mensagem de erro no `ValidationSummary` controle:</span><span class="sxs-lookup"><span data-stu-id="6a912-134">The page displays an asterisk after the field and an error message in the `ValidationSummary` control:</span></span>

<span data-ttu-id="6a912-135">[![Image05](the-entity-framework-and-aspnet-getting-started-part-8/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="6a912-135">[![Image05](the-entity-framework-and-aspnet-getting-started-part-8/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image5.png)</span></span>

<span data-ttu-id="6a912-136">Você pode omitir o `ValidationSummary` controle, porque você também pode manter o ponteiro do mouse sobre o asterisco para ver a mensagem de erro:</span><span class="sxs-lookup"><span data-stu-id="6a912-136">You could omit the `ValidationSummary` control, because you can also hold the mouse pointer over the asterisk to see the error message:</span></span>

<span data-ttu-id="6a912-137">[![Image06](the-entity-framework-and-aspnet-getting-started-part-8/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="6a912-137">[![Image06](the-entity-framework-and-aspnet-getting-started-part-8/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image7.png)</span></span>

<span data-ttu-id="6a912-138">Dados dinâmicos também validará que os dados digitados no **data de inscrição** campo for uma data válida:</span><span class="sxs-lookup"><span data-stu-id="6a912-138">Dynamic Data will also validate that data entered in the **Enrollment Date** field is a valid date:</span></span>

<span data-ttu-id="6a912-139">[![Image04](the-entity-framework-and-aspnet-getting-started-part-8/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="6a912-139">[![Image04](the-entity-framework-and-aspnet-getting-started-part-8/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image9.png)</span></span>

<span data-ttu-id="6a912-140">Como você pode ver, essa é uma mensagem de erro genérica.</span><span class="sxs-lookup"><span data-stu-id="6a912-140">As you can see, this is a generic error message.</span></span> <span data-ttu-id="6a912-141">Na próxima seção, você verá como personalizar mensagens, bem como a validação e regras de formatação.</span><span class="sxs-lookup"><span data-stu-id="6a912-141">In the next section you'll see how to customize messages as well as validation and formatting rules.</span></span>

## <a name="adding-metadata-to-the-data-model"></a><span data-ttu-id="6a912-142">Adicionar metadados para o modelo de dados</span><span class="sxs-lookup"><span data-stu-id="6a912-142">Adding Metadata to the Data Model</span></span>

<span data-ttu-id="6a912-143">Normalmente, você deseja personalizar a funcionalidade fornecida pelo dados dinâmicos.</span><span class="sxs-lookup"><span data-stu-id="6a912-143">Typically, you want to customize the functionality provided by Dynamic Data.</span></span> <span data-ttu-id="6a912-144">Por exemplo, você pode alterar como os dados são exibidos e o conteúdo das mensagens de erro.</span><span class="sxs-lookup"><span data-stu-id="6a912-144">For example, you might change how data is displayed and the content of error messages.</span></span> <span data-ttu-id="6a912-145">Você normalmente também personalizar regras de validação de dados para fornecer mais funcionalidade do que o que fornece dados dinâmicos automaticamente com base nos tipos de dados.</span><span class="sxs-lookup"><span data-stu-id="6a912-145">You typically also customize data validation rules to provide more functionality than what Dynamic Data provides automatically based on data types.</span></span> <span data-ttu-id="6a912-146">Para fazer isso, você deve criar classes parciais que correspondem aos tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="6a912-146">To do this, you create partial classes that correspond to entity types.</span></span>

<span data-ttu-id="6a912-147">Em **Solution Explorer**, com o botão direito do **ContosoUniversity** projeto, selecione **adicionar referência**e adicione uma referência a `System.ComponentModel.DataAnnotations`.</span><span class="sxs-lookup"><span data-stu-id="6a912-147">In **Solution Explorer**, right-click the **ContosoUniversity** project, select **Add Reference**, and add a reference to `System.ComponentModel.DataAnnotations`.</span></span>

<span data-ttu-id="6a912-148">[![Image11](the-entity-framework-and-aspnet-getting-started-part-8/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="6a912-148">[![Image11](the-entity-framework-and-aspnet-getting-started-part-8/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image11.png)</span></span>

<span data-ttu-id="6a912-149">No *DAL* pasta, crie um novo arquivo de classe, nomeie-o *Student.cs*e substitua o código do modelo com o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="6a912-149">In the *DAL* folder, create a new class file, name it *Student.cs*, and replace the template code in it with the following code.</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample6.cs)]

<span data-ttu-id="6a912-150">Esse código cria uma classe parcial para a `Student` entidade.</span><span class="sxs-lookup"><span data-stu-id="6a912-150">This code creates a partial class for the `Student` entity.</span></span> <span data-ttu-id="6a912-151">O `MetadataType` atributo aplicado a essa classe parcial identifica a classe que você está usando para especificar os metadados.</span><span class="sxs-lookup"><span data-stu-id="6a912-151">The `MetadataType` attribute applied to this partial class identifies the class that you're using to specify metadata.</span></span> <span data-ttu-id="6a912-152">A classe de metadados pode ter qualquer nome, mas usar o nome da entidade mais "Metadados" é uma prática comum.</span><span class="sxs-lookup"><span data-stu-id="6a912-152">The metadata class can have any name, but using the entity name plus "Metadata" is a common practice.</span></span>

<span data-ttu-id="6a912-153">Os atributos aplicados às propriedades de classe de metadados que especificam a formatação, validação, regras e mensagens de erro.</span><span class="sxs-lookup"><span data-stu-id="6a912-153">The attributes applied to properties in the metadata class specify formatting, validation, rules, and error messages.</span></span> <span data-ttu-id="6a912-154">Os atributos mostrados aqui terá os seguintes resultados:</span><span class="sxs-lookup"><span data-stu-id="6a912-154">The attributes shown here will have the following results:</span></span>

- <span data-ttu-id="6a912-155">`EnrollmentDate` será exibido como uma data (sem uma hora).</span><span class="sxs-lookup"><span data-stu-id="6a912-155">`EnrollmentDate` will display as a date (without a time).</span></span>
- <span data-ttu-id="6a912-156">Os campos de nome devem ser 25 caracteres ou menos em tamanho e uma mensagem de erro personalizada é fornecido.</span><span class="sxs-lookup"><span data-stu-id="6a912-156">Both name fields must be 25 characters or less in length, and a custom error message is provided.</span></span>
- <span data-ttu-id="6a912-157">Ambos os campos de nome são necessários, e uma mensagem de erro personalizada é fornecida.</span><span class="sxs-lookup"><span data-stu-id="6a912-157">Both name fields are required, and a custom error message is provided.</span></span>

<span data-ttu-id="6a912-158">Execute o *Students.aspx* página novamente, e você vê que as datas são exibidas agora sem vezes:</span><span class="sxs-lookup"><span data-stu-id="6a912-158">Run the *Students.aspx* page again, and you see that the dates are now displayed without times:</span></span>

<span data-ttu-id="6a912-159">[![Image08](the-entity-framework-and-aspnet-getting-started-part-8/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="6a912-159">[![Image08](the-entity-framework-and-aspnet-getting-started-part-8/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image13.png)</span></span>

<span data-ttu-id="6a912-160">Editar uma linha e tentar limpar os valores nos campos de nome.</span><span class="sxs-lookup"><span data-stu-id="6a912-160">Edit a row and try to clear the values in the name fields.</span></span> <span data-ttu-id="6a912-161">Os asteriscos que indicam erros de campo aparecem assim que você deixar um campo, antes de clicar em **atualização**.</span><span class="sxs-lookup"><span data-stu-id="6a912-161">The asterisks indicating field errors appear as soon as you leave a field, before you click **Update**.</span></span> <span data-ttu-id="6a912-162">Quando você clica em **atualização**, a página exibe o texto da mensagem de erro especificado.</span><span class="sxs-lookup"><span data-stu-id="6a912-162">When you click **Update**, the page displays the error message text you specified.</span></span>

<span data-ttu-id="6a912-163">[![Image10](the-entity-framework-and-aspnet-getting-started-part-8/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="6a912-163">[![Image10](the-entity-framework-and-aspnet-getting-started-part-8/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image15.png)</span></span>

<span data-ttu-id="6a912-164">Tente digitar os nomes de mais de 25 caracteres, clique em **atualização**, e a página exibe o texto da mensagem de erro especificado.</span><span class="sxs-lookup"><span data-stu-id="6a912-164">Try to enter names that are longer than 25 characters, click **Update**, and the page displays the error message text you specified.</span></span>

<span data-ttu-id="6a912-165">[![Image09](the-entity-framework-and-aspnet-getting-started-part-8/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image17.png)</span><span class="sxs-lookup"><span data-stu-id="6a912-165">[![Image09](the-entity-framework-and-aspnet-getting-started-part-8/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image17.png)</span></span>

<span data-ttu-id="6a912-166">Agora que você configurar essas regras de formatação e validação de metadados do modelo de dados, as regras serão aplicadas automaticamente em cada página que exibe ou permite que as alterações nesses campos, desde que você use `DynamicControl` ou `DynamicField` controles.</span><span class="sxs-lookup"><span data-stu-id="6a912-166">Now that you've set up these formatting and validation rules in the data model metadata, the rules will automatically be applied on every page that displays or allows changes to these fields, so long as you use `DynamicControl` or `DynamicField` controls.</span></span> <span data-ttu-id="6a912-167">Isso reduz a quantidade de código redundante você precisa escrever, que torna a programação e testes, e garante que a validação e formatação de dados são consistentes ao longo de um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="6a912-167">This reduces the amount of redundant code you have to write, which makes programming and testing easier, and it ensures that data formatting and validation are consistent throughout an application.</span></span>

## <a name="more-information"></a><span data-ttu-id="6a912-168">Mais Informações</span><span class="sxs-lookup"><span data-stu-id="6a912-168">More Information</span></span>

<span data-ttu-id="6a912-169">Isso conclui esta série de tutoriais de Introdução com o Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="6a912-169">This concludes this series of tutorials on Getting Started with the Entity Framework.</span></span> <span data-ttu-id="6a912-170">Para obter mais recursos para ajudá-lo a aprender a usar o Entity Framework, continue com [primeiro tutorial na próxima série de tutoriais do Entity Framework](../continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md) ou visite os seguintes sites:</span><span class="sxs-lookup"><span data-stu-id="6a912-170">For more resources to help you learn how to use the Entity Framework, continue with [the first tutorial in the next Entity Framework tutorial series](../continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md) or visit the following sites:</span></span>

- [<span data-ttu-id="6a912-171">Perguntas Frequentes do Entity Framework</span><span class="sxs-lookup"><span data-stu-id="6a912-171">Entity Framework FAQ</span></span>](http://www.ef-faq.org/introduction.html)
- [<span data-ttu-id="6a912-172">Blog da equipe do Entity Framework</span><span class="sxs-lookup"><span data-stu-id="6a912-172">The Entity Framework Team Blog</span></span>](https://blogs.msdn.com/b/adonet/)
- [<span data-ttu-id="6a912-173">Entity Framework na biblioteca MSDN</span><span class="sxs-lookup"><span data-stu-id="6a912-173">Entity Framework in the MSDN Library</span></span>](https://msdn.microsoft.com/library/bb399572.aspx)
- [<span data-ttu-id="6a912-174">Entity Framework do MSDN Data Developer Center</span><span class="sxs-lookup"><span data-stu-id="6a912-174">Entity Framework in the MSDN Data Developer Center</span></span>](https://msdn.microsoft.com/data/ef.aspx)
- [<span data-ttu-id="6a912-175">Visão geral de controle de servidor Web EntityDataSource na biblioteca MSDN</span><span class="sxs-lookup"><span data-stu-id="6a912-175">EntityDataSource Web Server Control Overview in the MSDN Library</span></span>](https://msdn.microsoft.com/library/cc488502.aspx)
- [<span data-ttu-id="6a912-176">Controle de EntityDataSource referência da API na biblioteca MSDN</span><span class="sxs-lookup"><span data-stu-id="6a912-176">EntityDataSource control API reference in the MSDN Library</span></span>](https://msdn.microsoft.com/library/system.web.ui.webcontrols.entitydatasource.aspx)
- [<span data-ttu-id="6a912-177">Fóruns do Entity Framework no MSDN</span><span class="sxs-lookup"><span data-stu-id="6a912-177">Entity Framework Forums on MSDN</span></span>](https://social.msdn.microsoft.com/forums/adodotnetentityframework/)
- [<span data-ttu-id="6a912-178">Blog de Julie Lerman</span><span class="sxs-lookup"><span data-stu-id="6a912-178">Julie Lerman's blog</span></span>](http://thedatafarm.com/blog/)

> [!div class="step-by-step"]
> [<span data-ttu-id="6a912-179">Anterior</span><span class="sxs-lookup"><span data-stu-id="6a912-179">Previous</span></span>](the-entity-framework-and-aspnet-getting-started-part-7.md)
