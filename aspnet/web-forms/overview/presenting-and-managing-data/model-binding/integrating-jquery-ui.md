---
uid: web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
title: Integrando o JQuery UI Datepicker com o modelo de associação e formulários da web | Microsoft Docs
author: tfitzmac
description: Esta série de tutoriais demonstra aspectos básicos do uso de associação de modelo com um projeto Web Forms do ASP.NET. Associação de modelo torna a interação de dados mais estreita-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 3cbab37b-fb0f-4751-9ec4-74e068c3f380
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: 126262b440f3e914a7fac3f0b7eeadb4f648d2bb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="integrating-jquery-ui-datepicker-with-model-binding-and-web-forms"></a><span data-ttu-id="e148e-104">Integrando o JQuery UI Datepicker com o modelo de associação e formulários da web</span><span class="sxs-lookup"><span data-stu-id="e148e-104">Integrating JQuery UI Datepicker with model binding and web forms</span></span>
====================
<span data-ttu-id="e148e-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="e148e-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="e148e-106">Esta série de tutoriais demonstra aspectos básicos do uso de associação de modelo com um projeto Web Forms do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e148e-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="e148e-107">Associação de modelo facilita a interação de dados mais direta de lidar com dados de objetos de origem (como ObjectDataSource ou SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="e148e-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="e148e-108">Esta série começa com material introdutório e move para conceitos mais avançados em tutoriais subsequentes.</span><span class="sxs-lookup"><span data-stu-id="e148e-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="e148e-109">Este tutorial mostra como adicionar o JQuery UI [widget Datepicker](http://jqueryui.com/datepicker/) para um formulário da Web e usar o modelo de associação para atualizar o banco de dados com o valor selecionado.</span><span class="sxs-lookup"><span data-stu-id="e148e-109">This tutorial shows how to add the JQuery UI [Datepicker widget](http://jqueryui.com/datepicker/) to a Web Form, and use model binding to update the database with the selected value.</span></span>
> 
> <span data-ttu-id="e148e-110">Este tutorial baseia-se no projeto criado no [primeiro](retrieving-data.md) e [segundo](updating-deleting-and-creating-data.md) partes da série.</span><span class="sxs-lookup"><span data-stu-id="e148e-110">This tutorial builds on the project created in the [first](retrieving-data.md) and [second](updating-deleting-and-creating-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="e148e-111">Você pode [baixar](https://go.microsoft.com/fwlink/?LinkId=286116) o projeto completo em c# ou VB.</span><span class="sxs-lookup"><span data-stu-id="e148e-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="e148e-112">O código para download funciona com o Visual Studio 2012 ou Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="e148e-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="e148e-113">Ele usa o modelo do Visual Studio 2012, que é ligeiramente diferente do que o modelo do Visual Studio 2013 mostrado neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="e148e-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="e148e-114">Será possível compilar</span><span class="sxs-lookup"><span data-stu-id="e148e-114">What you'll build</span></span>

<span data-ttu-id="e148e-115">Neste tutorial, você vai:</span><span class="sxs-lookup"><span data-stu-id="e148e-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="e148e-116">Adicionar uma propriedade ao modelo para registrar a data de inscrição do aluno</span><span class="sxs-lookup"><span data-stu-id="e148e-116">Add a property to your model to record the student's enrollment date</span></span>
2. <span data-ttu-id="e148e-117">Permitir que o usuário selecionar a data de registro usando o widget JQuery UI Datepicker</span><span class="sxs-lookup"><span data-stu-id="e148e-117">Enable the user to select the enrollment date using the JQuery UI Datepicker widget</span></span>
3. <span data-ttu-id="e148e-118">Aplicar regras de validação para a data de inscrição</span><span class="sxs-lookup"><span data-stu-id="e148e-118">Enforce validation rules for the enrollment date</span></span>

<span data-ttu-id="e148e-119">O widget JQuery UI Datepicker permite aos usuários selecionar uma data em um calendário que aparece quando o usuário interage com o campo.</span><span class="sxs-lookup"><span data-stu-id="e148e-119">The JQuery UI Datepicker widget enables users to easily select a date from a calendar that pops up when the user interacts with the field.</span></span> <span data-ttu-id="e148e-120">Pode ser mais conveniente para os usuários que manualmente digitando uma data usar este widget.</span><span class="sxs-lookup"><span data-stu-id="e148e-120">Using this widget can be more convenient for users than manually typing a date.</span></span> <span data-ttu-id="e148e-121">Integrar o widget Datepicker em uma página que usa uma associação de modelo para operações de dados requer apenas uma pequena quantidade de trabalho adicional.</span><span class="sxs-lookup"><span data-stu-id="e148e-121">Integrating the Datepicker widget into a page that uses model binding for data operations requires only a small amount of additional work.</span></span>

## <a name="add-a-new-property-to-the-model"></a><span data-ttu-id="e148e-122">Adicionar uma nova propriedade ao modelo</span><span class="sxs-lookup"><span data-stu-id="e148e-122">Add a new property to the model</span></span>

<span data-ttu-id="e148e-123">Primeiro, você adicionará um **Datetime** propriedade seu aluno do modelo e migrar essa alteração no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e148e-123">First, you will add a **Datetime** property to your Student model and migrate that change to the database.</span></span> <span data-ttu-id="e148e-124">Abra **UniversityModels.cs**e adicione o código realçado para o modelo do aluno.</span><span class="sxs-lookup"><span data-stu-id="e148e-124">Open **UniversityModels.cs**, and add the highlighted code to the Student model.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample1.cs?highlight=16-18)]

<span data-ttu-id="e148e-125">O **RangeAttribute** é incluída para impor regras de validação para a propriedade.</span><span class="sxs-lookup"><span data-stu-id="e148e-125">The **RangeAttribute** is included to enforce validation rules for the property.</span></span> <span data-ttu-id="e148e-126">Para este tutorial, vamos pressupor que Contoso University foi fundado em 1º de janeiro de 2013 e, portanto, as datas de registro anteriores não são válidas.</span><span class="sxs-lookup"><span data-stu-id="e148e-126">For this tutorial, we will assume that Contoso University was founded on January 1st, 2013 and therefore earlier enrollment dates are not valid.</span></span>

<span data-ttu-id="e148e-127">Na janela Gerenciamento de pacotes, adicione uma migração, executando o comando **AddEnrollmentDate migração adicionar**.</span><span class="sxs-lookup"><span data-stu-id="e148e-127">In the Package Management window, add a migration by running the command **add-migration AddEnrollmentDate**.</span></span> <span data-ttu-id="e148e-128">Observe que o código de migração adiciona a nova coluna de data e hora para a tabela de alunos.</span><span class="sxs-lookup"><span data-stu-id="e148e-128">Notice that the migration code adds the new Datetime column to the Student table.</span></span> <span data-ttu-id="e148e-129">Para corresponder ao valor especificado no RangeAttribute, adicione um valor padrão para a nova coluna, conforme mostrado no código realçado abaixo.</span><span class="sxs-lookup"><span data-stu-id="e148e-129">To match the value you specified in the RangeAttribute, add a default value for the new column, as shown in the highlighted code below.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample2.cs?highlight=11)]

<span data-ttu-id="e148e-130">Salve suas alterações no arquivo de migração.</span><span class="sxs-lookup"><span data-stu-id="e148e-130">Save your change to the migration file.</span></span>

<span data-ttu-id="e148e-131">Não é necessário propagar os dados novamente.</span><span class="sxs-lookup"><span data-stu-id="e148e-131">You do not need to seed the data again.</span></span> <span data-ttu-id="e148e-132">Por isso, abra **Configuration.cs** na pasta migrações e remova ou comentar o código de **semente** método.</span><span class="sxs-lookup"><span data-stu-id="e148e-132">Therefore, open **Configuration.cs** in the Migrations folder and remove or comment out the code in the **Seed** method.</span></span> <span data-ttu-id="e148e-133">Salve e feche o arquivo.</span><span class="sxs-lookup"><span data-stu-id="e148e-133">Save and close the file.</span></span>

<span data-ttu-id="e148e-134">Agora, execute o comando **Atualizar banco de dados**.</span><span class="sxs-lookup"><span data-stu-id="e148e-134">Now, run the command **update-database**.</span></span> <span data-ttu-id="e148e-135">Observe que a coluna agora existe no banco de dados e todos os registros existentes têm o valor padrão para EnrollmentDate.</span><span class="sxs-lookup"><span data-stu-id="e148e-135">Notice that the column now exists in the database and all of the existing records have the default value for EnrollmentDate.</span></span>

## <a name="add-dynamic-controls-for-enrollment-date"></a><span data-ttu-id="e148e-136">Adicionar controles dinâmicos para a data de inscrição</span><span class="sxs-lookup"><span data-stu-id="e148e-136">Add dynamic controls for enrollment date</span></span>

<span data-ttu-id="e148e-137">Agora você irá adicionar controles para exibir e editar a data de inscrição.</span><span class="sxs-lookup"><span data-stu-id="e148e-137">You will now add controls for displaying and editing the enrollment date.</span></span> <span data-ttu-id="e148e-138">Neste ponto, o valor é editado por meio de uma caixa de texto.</span><span class="sxs-lookup"><span data-stu-id="e148e-138">At this point, the value is edited through a text box.</span></span> <span data-ttu-id="e148e-139">Posteriormente no tutorial, você irá alterar a caixa de texto para o widget JQuery Datepicker.</span><span class="sxs-lookup"><span data-stu-id="e148e-139">Later in the tutorial, you will change the text box to the JQuery Datepicker widget.</span></span>

<span data-ttu-id="e148e-140">Primeiro, é importante observar que não é necessário fazer qualquer alteração de **AddStudent.aspx** arquivo.</span><span class="sxs-lookup"><span data-stu-id="e148e-140">First, it is important to note that you do not need to make any change to the **AddStudent.aspx** file.</span></span> <span data-ttu-id="e148e-141">O controle DynamicEntity automaticamente processe a nova propriedade.</span><span class="sxs-lookup"><span data-stu-id="e148e-141">The DynamicEntity control will automatically render the new property.</span></span>

<span data-ttu-id="e148e-142">Abra **Students.aspx**e adicione o seguinte código realçado.</span><span class="sxs-lookup"><span data-stu-id="e148e-142">Open **Students.aspx**, and add the following highlighted code.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample3.aspx?highlight=13)]

<span data-ttu-id="e148e-143">Execute o aplicativo e observe que você pode definir o valor da data de inscrição digitando uma data.</span><span class="sxs-lookup"><span data-stu-id="e148e-143">Run the application and notice that you can set the value of the enrollment date by typing a date.</span></span> <span data-ttu-id="e148e-144">Ao adicionar um novo aluno:</span><span class="sxs-lookup"><span data-stu-id="e148e-144">When adding a new student:</span></span>

![definir a data](integrating-jquery-ui/_static/image1.png)

<span data-ttu-id="e148e-146">Ou editar um valor existente:</span><span class="sxs-lookup"><span data-stu-id="e148e-146">Or, editing an existing value:</span></span>

![Data da edição](integrating-jquery-ui/_static/image2.png)

<span data-ttu-id="e148e-148">Digitar a data funciona, mas ele pode não ser a experiência do cliente que você deseja fornecer.</span><span class="sxs-lookup"><span data-stu-id="e148e-148">Typing the date works, but it might not be the customer experience you wish to provide.</span></span> <span data-ttu-id="e148e-149">Na próxima seção, você permitirá selecionar uma data por meio de um calendário.</span><span class="sxs-lookup"><span data-stu-id="e148e-149">In the next section, you will enable selecting a date through a calendar.</span></span>

## <a name="install-nuget-package-to-work-with-jquery-ui"></a><span data-ttu-id="e148e-150">Instalar o pacote do NuGet para funcionar com JQuery UI</span><span class="sxs-lookup"><span data-stu-id="e148e-150">Install NuGet package to work with JQuery UI</span></span>

<span data-ttu-id="e148e-151">O **suco de interface do usuário** pacote NuGet permite uma fácil integração de widgets JQuery UI em seu aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="e148e-151">The **Juice UI** NuGet package enables easy integration of the JQuery UI widgets into your web application.</span></span> <span data-ttu-id="e148e-152">Para usar este pacote, instale-o através do NuGet.</span><span class="sxs-lookup"><span data-stu-id="e148e-152">To use this package, install it through NuGet.</span></span>

![Adicionar suco de interface do usuário](integrating-jquery-ui/_static/image3.png)

<span data-ttu-id="e148e-154">A versão da interface do usuário de suco de instalação pode entrar em conflito com a versão do JQuery em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e148e-154">The version of Juice UI that you install may conflict with the version of JQuery in your application.</span></span> <span data-ttu-id="e148e-155">Antes de continuar com este tutorial, tente executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e148e-155">Before proceeding with this tutorial, try running your application.</span></span> <span data-ttu-id="e148e-156">Se você encontrar um erro de JavaScript, você precisa reconciliar a versão JQuery.</span><span class="sxs-lookup"><span data-stu-id="e148e-156">If you encounter a JavaScript error, you need to reconcile the JQuery version.</span></span> <span data-ttu-id="e148e-157">Você pode adicionar a versão esperada do JQuery para a pasta de Scripts (versão 1.8.2 em vez de escrever este tutorial) ou Site.master Especifica o caminho para o arquivo JQuery.</span><span class="sxs-lookup"><span data-stu-id="e148e-157">You can either add the expected version of JQuery to your Scripts folder (version 1.8.2 at time of writing this tutorial), or in Site.master specify the path to the JQuery file.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample4.aspx)]

## <a name="customize-datetime-template-to-include-datepicker-widget"></a><span data-ttu-id="e148e-158">Personalizar o modelo de data e hora para incluir o widget Datepicker</span><span class="sxs-lookup"><span data-stu-id="e148e-158">Customize DateTime template to include Datepicker widget</span></span>

<span data-ttu-id="e148e-159">Você adicionará o widget Datepicker no modelo de dados dinâmicos para a edição de um valor datetime.</span><span class="sxs-lookup"><span data-stu-id="e148e-159">You will add the Datepicker widget to the dynamic data template for editing a datetime value.</span></span> <span data-ttu-id="e148e-160">Adicionando o widget para o modelo, ele automaticamente será renderizado no formulário para adicionar um novo aluno e na exibição de grade para edição de estudantes.</span><span class="sxs-lookup"><span data-stu-id="e148e-160">By adding the widget to the template, it is automatically rendered in both the form for adding a new student and in the grid view for editing students.</span></span> <span data-ttu-id="e148e-161">Abra **DateTime\_Edit.ascx**e adicione o seguinte código realçado.</span><span class="sxs-lookup"><span data-stu-id="e148e-161">Open **DateTime\_Edit.ascx**, and add the following highlighted code.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample5.aspx?highlight=3)]

<span data-ttu-id="e148e-162">O arquivo code-behind, você definirá as datas de mínimas e máxima para o selecionador de data.</span><span class="sxs-lookup"><span data-stu-id="e148e-162">In the code-behind file, you will set the minimum and maximum dates for the DatePicker.</span></span> <span data-ttu-id="e148e-163">Ao definir esses valores, você impedirá que os usuários de navegar para datas inválidas.</span><span class="sxs-lookup"><span data-stu-id="e148e-163">By setting these values, you will prevent users from navigating to invalid dates.</span></span> <span data-ttu-id="e148e-164">Você vai recuperar os valores mínimo e máximo do **RangeAttribute** na propriedade de data e hora, se houver.</span><span class="sxs-lookup"><span data-stu-id="e148e-164">You will retrieve the minimum and maximum values from the **RangeAttribute** on the DateTime property, if one is provided.</span></span> <span data-ttu-id="e148e-165">Abra **DateTime\_Edit.ascx.cs**e adicione o seguinte código para a página\_método de carga.</span><span class="sxs-lookup"><span data-stu-id="e148e-165">Open **DateTime\_Edit.ascx.cs**, and add the following highlighted code to the Page\_Load method.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample6.cs?highlight=9-14)]

<span data-ttu-id="e148e-166">Execute o aplicativo web e navegue até a página AddStudent.</span><span class="sxs-lookup"><span data-stu-id="e148e-166">Run the web application and navigate to the AddStudent page.</span></span> <span data-ttu-id="e148e-167">Forneça valores para os campos e observe que, quando você clicar na caixa de texto de data de inscrição, o calendário será exibido.</span><span class="sxs-lookup"><span data-stu-id="e148e-167">Provide values for the fields and notice that when you click on the text box for Enrollment Date, the calendar is displayed.</span></span>

![seletor de data](integrating-jquery-ui/_static/image4.png)

<span data-ttu-id="e148e-169">Escolha uma data e, em seguida, clique em **inserir**.</span><span class="sxs-lookup"><span data-stu-id="e148e-169">Pick a date, and click **Insert**.</span></span> <span data-ttu-id="e148e-170">O RangeAttribute impõe validação no servidor.</span><span class="sxs-lookup"><span data-stu-id="e148e-170">The RangeAttribute enforces validation on the server.</span></span> <span data-ttu-id="e148e-171">Definindo a propriedade minDate de Datepicker, você também deve aplicar a validação no cliente.</span><span class="sxs-lookup"><span data-stu-id="e148e-171">By setting the minDate property on the Datepicker, you also apply validation on the client.</span></span> <span data-ttu-id="e148e-172">O calendário não permite que o usuário navegar para uma data antes do valor de minDate.</span><span class="sxs-lookup"><span data-stu-id="e148e-172">The calendar does not let the user navigate to a date prior to the value of minDate.</span></span>

<span data-ttu-id="e148e-173">Quando você editar um registro na exibição de grade, o calendário também é exibido.</span><span class="sxs-lookup"><span data-stu-id="e148e-173">When you edit a record in the grid view, the calendar is also displayed.</span></span>

![DatePicker em GridView](integrating-jquery-ui/_static/image5.png)

## <a name="conclusion"></a><span data-ttu-id="e148e-175">Conclusão</span><span class="sxs-lookup"><span data-stu-id="e148e-175">Conclusion</span></span>

<span data-ttu-id="e148e-176">Neste tutorial, você aprendeu como incorporar um widget JQuery em um formulário da web que usa a associação de modelo.</span><span class="sxs-lookup"><span data-stu-id="e148e-176">In this tutorial, you learned how to incorporate a JQuery widget into a web form that uses model binding.</span></span>

<span data-ttu-id="e148e-177">Na próxima [tutorial](using-query-string-values-to-retrieve-data.md), você usará um valor de cadeia de caracteres de consulta ao selecionar os dados.</span><span class="sxs-lookup"><span data-stu-id="e148e-177">In the next [tutorial](using-query-string-values-to-retrieve-data.md), you will use a query string value when selecting data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e148e-178">[Anterior](sorting-paging-and-filtering-data.md)
> [Próximo](using-query-string-values-to-retrieve-data.md)</span><span class="sxs-lookup"><span data-stu-id="e148e-178">[Previous](sorting-paging-and-filtering-data.md)
[Next](using-query-string-values-to-retrieve-data.md)</span></span>
