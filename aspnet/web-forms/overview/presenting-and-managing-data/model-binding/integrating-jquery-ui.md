---
uid: web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
title: A integração do JQuery UI Datepicker com associação de modelos e formulários da web | Microsoft Docs
author: tfitzmac
description: Esta série de tutoriais demonstra aspectos básicos de como usar a associação de modelo com um projeto de Web Forms do ASP.NET. Associação de modelo torna a interação de dados mais simples-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 3cbab37b-fb0f-4751-9ec4-74e068c3f380
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: 7d60d2945dbf9daca33422ab82b9265fedc2ba31
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393821"
---
<a name="integrating-jquery-ui-datepicker-with-model-binding-and-web-forms"></a><span data-ttu-id="1ebed-104">A integração do JQuery UI Datepicker com associação de modelos e formulários da web</span><span class="sxs-lookup"><span data-stu-id="1ebed-104">Integrating JQuery UI Datepicker with model binding and web forms</span></span>
====================
<span data-ttu-id="1ebed-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="1ebed-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="1ebed-106">Esta série de tutoriais demonstra aspectos básicos de como usar a associação de modelo com um projeto de Web Forms do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1ebed-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="1ebed-107">Associação de modelo torna a interação de dados mais simples que lidam com dados de objetos de origem (como ObjectDataSource ou SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="1ebed-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="1ebed-108">Esta série começa com material introdutório e move para conceitos mais avançados em tutoriais posteriores.</span><span class="sxs-lookup"><span data-stu-id="1ebed-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="1ebed-109">Este tutorial mostra como adicionar o JQuery UI [widget Datepicker](http://jqueryui.com/datepicker/) a um formulário da Web e usar o modelo de associação para atualizar o banco de dados com o valor selecionado.</span><span class="sxs-lookup"><span data-stu-id="1ebed-109">This tutorial shows how to add the JQuery UI [Datepicker widget](http://jqueryui.com/datepicker/) to a Web Form, and use model binding to update the database with the selected value.</span></span>
> 
> <span data-ttu-id="1ebed-110">Este tutorial se baseia no projeto criado a [primeira](retrieving-data.md) e [segundo](updating-deleting-and-creating-data.md) partes da série.</span><span class="sxs-lookup"><span data-stu-id="1ebed-110">This tutorial builds on the project created in the [first](retrieving-data.md) and [second](updating-deleting-and-creating-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="1ebed-111">Você pode [baixar](https://go.microsoft.com/fwlink/?LinkId=286116) o projeto completo em c# ou VB.</span><span class="sxs-lookup"><span data-stu-id="1ebed-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="1ebed-112">O código para download funciona com o Visual Studio 2012 ou Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="1ebed-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="1ebed-113">Ele usa o modelo do Visual Studio 2012, que é ligeiramente diferente do que o modelo do Visual Studio 2013 mostrado neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="1ebed-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="1ebed-114">O que você vai criar</span><span class="sxs-lookup"><span data-stu-id="1ebed-114">What you'll build</span></span>

<span data-ttu-id="1ebed-115">Neste tutorial, você vai:</span><span class="sxs-lookup"><span data-stu-id="1ebed-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="1ebed-116">Adicionar uma propriedade ao seu modelo para registrar a data de registro do aluno</span><span class="sxs-lookup"><span data-stu-id="1ebed-116">Add a property to your model to record the student's enrollment date</span></span>
2. <span data-ttu-id="1ebed-117">Permitir que o usuário selecionar a data de registro usando o widget Datepicker do JQuery UI</span><span class="sxs-lookup"><span data-stu-id="1ebed-117">Enable the user to select the enrollment date using the JQuery UI Datepicker widget</span></span>
3. <span data-ttu-id="1ebed-118">Impor regras de validação para a data de registro</span><span class="sxs-lookup"><span data-stu-id="1ebed-118">Enforce validation rules for the enrollment date</span></span>

<span data-ttu-id="1ebed-119">O widget Datepicker do JQuery UI permite aos usuários selecionar facilmente uma data em um calendário pop-up quando o usuário interage com o campo.</span><span class="sxs-lookup"><span data-stu-id="1ebed-119">The JQuery UI Datepicker widget enables users to easily select a date from a calendar that pops up when the user interacts with the field.</span></span> <span data-ttu-id="1ebed-120">Usar este widget pode ser mais conveniente para os usuários do que digitar manualmente uma data.</span><span class="sxs-lookup"><span data-stu-id="1ebed-120">Using this widget can be more convenient for users than manually typing a date.</span></span> <span data-ttu-id="1ebed-121">Integrar o widget Datepicker em uma página que usa a associação de modelo para operações de dados requer apenas uma pequena quantidade de trabalho adicional.</span><span class="sxs-lookup"><span data-stu-id="1ebed-121">Integrating the Datepicker widget into a page that uses model binding for data operations requires only a small amount of additional work.</span></span>

## <a name="add-a-new-property-to-the-model"></a><span data-ttu-id="1ebed-122">Adicionar uma nova propriedade ao modelo</span><span class="sxs-lookup"><span data-stu-id="1ebed-122">Add a new property to the model</span></span>

<span data-ttu-id="1ebed-123">Primeiro, você adicionará uma **Datetime** propriedade para seus estudantes de modelo e migrar essa alteração no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="1ebed-123">First, you will add a **Datetime** property to your Student model and migrate that change to the database.</span></span> <span data-ttu-id="1ebed-124">Abra **UniversityModels.cs**e adicione o código realçado ao modelo de aluno.</span><span class="sxs-lookup"><span data-stu-id="1ebed-124">Open **UniversityModels.cs**, and add the highlighted code to the Student model.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample1.cs?highlight=16-18)]

<span data-ttu-id="1ebed-125">O **RangeAttribute** é incluída para impor regras de validação para a propriedade.</span><span class="sxs-lookup"><span data-stu-id="1ebed-125">The **RangeAttribute** is included to enforce validation rules for the property.</span></span> <span data-ttu-id="1ebed-126">Para este tutorial, vamos pressupor que a Contoso University foi fundada em 1º de janeiro de 2013 e, portanto, as datas de registro anteriores não são válidas.</span><span class="sxs-lookup"><span data-stu-id="1ebed-126">For this tutorial, we will assume that Contoso University was founded on January 1st, 2013 and therefore earlier enrollment dates are not valid.</span></span>

<span data-ttu-id="1ebed-127">Na janela Gerenciamento de pacotes, adicione uma migração, executando o comando **AddEnrollmentDate migração adicionar**.</span><span class="sxs-lookup"><span data-stu-id="1ebed-127">In the Package Management window, add a migration by running the command **add-migration AddEnrollmentDate**.</span></span> <span data-ttu-id="1ebed-128">Observe que o código de migração adiciona a nova coluna de data e hora para a tabela aluno.</span><span class="sxs-lookup"><span data-stu-id="1ebed-128">Notice that the migration code adds the new Datetime column to the Student table.</span></span> <span data-ttu-id="1ebed-129">Para corresponder ao valor especificado no RangeAttribute, adicione um valor padrão para a nova coluna, conforme mostrado no código realçado a seguir.</span><span class="sxs-lookup"><span data-stu-id="1ebed-129">To match the value you specified in the RangeAttribute, add a default value for the new column, as shown in the highlighted code below.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample2.cs?highlight=11)]

<span data-ttu-id="1ebed-130">Salve a alteração no arquivo de migração.</span><span class="sxs-lookup"><span data-stu-id="1ebed-130">Save your change to the migration file.</span></span>

<span data-ttu-id="1ebed-131">Não é preciso propagar os dados novamente.</span><span class="sxs-lookup"><span data-stu-id="1ebed-131">You do not need to seed the data again.</span></span> <span data-ttu-id="1ebed-132">Portanto, abra **Configuration.cs** na pasta migrações e remova ou comente o código na **semente** método.</span><span class="sxs-lookup"><span data-stu-id="1ebed-132">Therefore, open **Configuration.cs** in the Migrations folder and remove or comment out the code in the **Seed** method.</span></span> <span data-ttu-id="1ebed-133">Salve e feche o arquivo.</span><span class="sxs-lookup"><span data-stu-id="1ebed-133">Save and close the file.</span></span>

<span data-ttu-id="1ebed-134">Agora, execute o comando **update-database**.</span><span class="sxs-lookup"><span data-stu-id="1ebed-134">Now, run the command **update-database**.</span></span> <span data-ttu-id="1ebed-135">Observe que agora existe a coluna no banco de dados e todos os registros existentes têm o valor padrão para EnrollmentDate.</span><span class="sxs-lookup"><span data-stu-id="1ebed-135">Notice that the column now exists in the database and all of the existing records have the default value for EnrollmentDate.</span></span>

## <a name="add-dynamic-controls-for-enrollment-date"></a><span data-ttu-id="1ebed-136">Adicionar controles dinâmicos para a data de registro</span><span class="sxs-lookup"><span data-stu-id="1ebed-136">Add dynamic controls for enrollment date</span></span>

<span data-ttu-id="1ebed-137">Agora você irá adicionar controles para exibir e editar a data de inscrição.</span><span class="sxs-lookup"><span data-stu-id="1ebed-137">You will now add controls for displaying and editing the enrollment date.</span></span> <span data-ttu-id="1ebed-138">Neste ponto, o valor será editado por meio de uma caixa de texto.</span><span class="sxs-lookup"><span data-stu-id="1ebed-138">At this point, the value is edited through a text box.</span></span> <span data-ttu-id="1ebed-139">Posteriormente no tutorial, você irá alterar a caixa de texto para o widget de Datepicker do JQuery.</span><span class="sxs-lookup"><span data-stu-id="1ebed-139">Later in the tutorial, you will change the text box to the JQuery Datepicker widget.</span></span>

<span data-ttu-id="1ebed-140">Primeiro, é importante observar que você não precisa fazer nenhuma alteração para o **AddStudent.aspx** arquivo.</span><span class="sxs-lookup"><span data-stu-id="1ebed-140">First, it is important to note that you do not need to make any change to the **AddStudent.aspx** file.</span></span> <span data-ttu-id="1ebed-141">O controle DynamicEntity renderizará automaticamente a nova propriedade.</span><span class="sxs-lookup"><span data-stu-id="1ebed-141">The DynamicEntity control will automatically render the new property.</span></span>

<span data-ttu-id="1ebed-142">Abra **Students.aspx**e adicione o seguinte código realçado.</span><span class="sxs-lookup"><span data-stu-id="1ebed-142">Open **Students.aspx**, and add the following highlighted code.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample3.aspx?highlight=13)]

<span data-ttu-id="1ebed-143">Execute o aplicativo e observe que você pode definir o valor da data de inscrição digitando uma data.</span><span class="sxs-lookup"><span data-stu-id="1ebed-143">Run the application and notice that you can set the value of the enrollment date by typing a date.</span></span> <span data-ttu-id="1ebed-144">Ao adicionar um novo aluno:</span><span class="sxs-lookup"><span data-stu-id="1ebed-144">When adding a new student:</span></span>

![definir a data](integrating-jquery-ui/_static/image1.png)

<span data-ttu-id="1ebed-146">Ou, edição de um valor existente:</span><span class="sxs-lookup"><span data-stu-id="1ebed-146">Or, editing an existing value:</span></span>

![Editar Data](integrating-jquery-ui/_static/image2.png)

<span data-ttu-id="1ebed-148">Digitar a data funciona, mas ele pode não ser a experiência do cliente que você deseja fornecer.</span><span class="sxs-lookup"><span data-stu-id="1ebed-148">Typing the date works, but it might not be the customer experience you wish to provide.</span></span> <span data-ttu-id="1ebed-149">Na próxima seção, você habilitará a seleção de uma data por meio de um calendário.</span><span class="sxs-lookup"><span data-stu-id="1ebed-149">In the next section, you will enable selecting a date through a calendar.</span></span>

## <a name="install-nuget-package-to-work-with-jquery-ui"></a><span data-ttu-id="1ebed-150">Instalar o pacote do NuGet para trabalhar com o JQuery UI</span><span class="sxs-lookup"><span data-stu-id="1ebed-150">Install NuGet package to work with JQuery UI</span></span>

<span data-ttu-id="1ebed-151">O **suco de interface do usuário do** pacote NuGet permite fácil integração dos widgets do JQuery UI ao seu aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="1ebed-151">The **Juice UI** NuGet package enables easy integration of the JQuery UI widgets into your web application.</span></span> <span data-ttu-id="1ebed-152">Para usar este pacote, instalá-lo por meio do NuGet.</span><span class="sxs-lookup"><span data-stu-id="1ebed-152">To use this package, install it through NuGet.</span></span>

![Adicionar suco da interface do usuário](integrating-jquery-ui/_static/image3.png)

<span data-ttu-id="1ebed-154">A versão de suco de interface do usuário que você instala pode entrar em conflito com a versão do JQuery em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1ebed-154">The version of Juice UI that you install may conflict with the version of JQuery in your application.</span></span> <span data-ttu-id="1ebed-155">Antes de continuar com este tutorial, tente executar seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1ebed-155">Before proceeding with this tutorial, try running your application.</span></span> <span data-ttu-id="1ebed-156">Se você encontrar um erro de JavaScript, você precisa reconciliar a versão do JQuery.</span><span class="sxs-lookup"><span data-stu-id="1ebed-156">If you encounter a JavaScript error, you need to reconcile the JQuery version.</span></span> <span data-ttu-id="1ebed-157">Você pode adicionar a versão esperada do JQuery para sua pasta de Scripts (versão 1.8.2 no momento da redação deste tutorial) ou no site, especifique o caminho para o arquivo do JQuery.</span><span class="sxs-lookup"><span data-stu-id="1ebed-157">You can either add the expected version of JQuery to your Scripts folder (version 1.8.2 at time of writing this tutorial), or in Site.master specify the path to the JQuery file.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample4.aspx)]

## <a name="customize-datetime-template-to-include-datepicker-widget"></a><span data-ttu-id="1ebed-158">Personalizar o modelo de data e hora para incluir o widget Datepicker</span><span class="sxs-lookup"><span data-stu-id="1ebed-158">Customize DateTime template to include Datepicker widget</span></span>

<span data-ttu-id="1ebed-159">Você adicionará o widget Datepicker o modelo de dados dinâmicos para a edição de um valor de data e hora.</span><span class="sxs-lookup"><span data-stu-id="1ebed-159">You will add the Datepicker widget to the dynamic data template for editing a datetime value.</span></span> <span data-ttu-id="1ebed-160">Adicionando o widget para o modelo, ele automaticamente será renderizado no formulário para adicionar um novo aluno e na exibição de grade para alunos de edição.</span><span class="sxs-lookup"><span data-stu-id="1ebed-160">By adding the widget to the template, it is automatically rendered in both the form for adding a new student and in the grid view for editing students.</span></span> <span data-ttu-id="1ebed-161">Abra **DateTime\_Edit.ascx**e adicione o seguinte código realçado.</span><span class="sxs-lookup"><span data-stu-id="1ebed-161">Open **DateTime\_Edit.ascx**, and add the following highlighted code.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample5.aspx?highlight=3)]

<span data-ttu-id="1ebed-162">O arquivo code-behind, você definirá as datas mínimas e máxima para o DatePicker.</span><span class="sxs-lookup"><span data-stu-id="1ebed-162">In the code-behind file, you will set the minimum and maximum dates for the DatePicker.</span></span> <span data-ttu-id="1ebed-163">Ao definir esses valores, você impedirá que os usuários naveguem para datas inválidas.</span><span class="sxs-lookup"><span data-stu-id="1ebed-163">By setting these values, you will prevent users from navigating to invalid dates.</span></span> <span data-ttu-id="1ebed-164">Você vai recuperar os valores mínimo e máximo do **RangeAttribute** na propriedade de data e hora, se fornecido.</span><span class="sxs-lookup"><span data-stu-id="1ebed-164">You will retrieve the minimum and maximum values from the **RangeAttribute** on the DateTime property, if one is provided.</span></span> <span data-ttu-id="1ebed-165">Abra **DateTime\_Edit.ascx.cs**e adicione o seguinte código realçado para a página\_método de carga.</span><span class="sxs-lookup"><span data-stu-id="1ebed-165">Open **DateTime\_Edit.ascx.cs**, and add the following highlighted code to the Page\_Load method.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample6.cs?highlight=9-14)]

<span data-ttu-id="1ebed-166">Executar o aplicativo web e navegue até a página AddStudent.</span><span class="sxs-lookup"><span data-stu-id="1ebed-166">Run the web application and navigate to the AddStudent page.</span></span> <span data-ttu-id="1ebed-167">Forneça valores para os campos e observe que, quando você clica na caixa de texto para data de registro, o calendário é exibido.</span><span class="sxs-lookup"><span data-stu-id="1ebed-167">Provide values for the fields and notice that when you click on the text box for Enrollment Date, the calendar is displayed.</span></span>

![seletor de data](integrating-jquery-ui/_static/image4.png)

<span data-ttu-id="1ebed-169">Escolha uma data e, em seguida, clique em **inserir**.</span><span class="sxs-lookup"><span data-stu-id="1ebed-169">Pick a date, and click **Insert**.</span></span> <span data-ttu-id="1ebed-170">O RangeAttribute impõe a validação no servidor.</span><span class="sxs-lookup"><span data-stu-id="1ebed-170">The RangeAttribute enforces validation on the server.</span></span> <span data-ttu-id="1ebed-171">Definindo a propriedade minDate no Datepicker, você também deve aplicar validação no cliente.</span><span class="sxs-lookup"><span data-stu-id="1ebed-171">By setting the minDate property on the Datepicker, you also apply validation on the client.</span></span> <span data-ttu-id="1ebed-172">O calendário não permite que o usuário navegar para uma data antes do valor de minDate.</span><span class="sxs-lookup"><span data-stu-id="1ebed-172">The calendar does not let the user navigate to a date prior to the value of minDate.</span></span>

<span data-ttu-id="1ebed-173">Quando você edita um registro na exibição de grade, o calendário também é exibido.</span><span class="sxs-lookup"><span data-stu-id="1ebed-173">When you edit a record in the grid view, the calendar is also displayed.</span></span>

![DatePicker em GridView](integrating-jquery-ui/_static/image5.png)

## <a name="conclusion"></a><span data-ttu-id="1ebed-175">Conclusão</span><span class="sxs-lookup"><span data-stu-id="1ebed-175">Conclusion</span></span>

<span data-ttu-id="1ebed-176">Neste tutorial, você aprendeu como incorporar um widget JQuery em um formulário da web que usa a associação de modelos.</span><span class="sxs-lookup"><span data-stu-id="1ebed-176">In this tutorial, you learned how to incorporate a JQuery widget into a web form that uses model binding.</span></span>

<span data-ttu-id="1ebed-177">Nos próximos [tutorial](using-query-string-values-to-retrieve-data.md), você usará um valor de cadeia de caracteres de consulta ao selecionar os dados.</span><span class="sxs-lookup"><span data-stu-id="1ebed-177">In the next [tutorial](using-query-string-values-to-retrieve-data.md), you will use a query string value when selecting data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1ebed-178">[Anterior](sorting-paging-and-filtering-data.md)
> [Próximo](using-query-string-values-to-retrieve-data.md)</span><span class="sxs-lookup"><span data-stu-id="1ebed-178">[Previous](sorting-paging-and-filtering-data.md)
[Next](using-query-string-values-to-retrieve-data.md)</span></span>
