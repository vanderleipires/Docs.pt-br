---
uid: web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
title: Atualizar, excluir e criar dados com o modelo de associação e formulários da web | Microsoft Docs
author: tfitzmac
description: Esta série de tutoriais demonstra aspectos básicos do uso de associação de modelo com um projeto Web Forms do ASP.NET. Associação de modelo torna a interação de dados mais estreita-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 602baa94-5a4f-46eb-a717-7a9e539c1db4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
msc.type: authoredcontent
ms.openlocfilehash: e6536f7858afde5faf3aedd34f3cbe95c5ed0d53
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30885839"
---
<a name="updating-deleting-and-creating-data-with-model-binding-and-web-forms"></a><span data-ttu-id="36d11-104">Atualizar, excluir e criar dados com o modelo de associação e formulários da web</span><span class="sxs-lookup"><span data-stu-id="36d11-104">Updating, deleting, and creating data with model binding and web forms</span></span>
====================
<span data-ttu-id="36d11-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="36d11-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="36d11-106">Esta série de tutoriais demonstra aspectos básicos do uso de associação de modelo com um projeto Web Forms do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="36d11-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="36d11-107">Associação de modelo facilita a interação de dados mais direta de lidar com dados de objetos de origem (como ObjectDataSource ou SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="36d11-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="36d11-108">Esta série começa com material introdutório e move para conceitos mais avançados em tutoriais subsequentes.</span><span class="sxs-lookup"><span data-stu-id="36d11-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="36d11-109">Este tutorial mostra como criar, atualizar e excluir dados com associação de modelo.</span><span class="sxs-lookup"><span data-stu-id="36d11-109">This tutorial shows how to create, update, and delete data with model binding.</span></span> <span data-ttu-id="36d11-110">Você definirá as propriedades a seguir:</span><span class="sxs-lookup"><span data-stu-id="36d11-110">You will set the following properties:</span></span>
> 
> - <span data-ttu-id="36d11-111">DeleteMethod</span><span class="sxs-lookup"><span data-stu-id="36d11-111">DeleteMethod</span></span>
> - <span data-ttu-id="36d11-112">InsertMethod</span><span class="sxs-lookup"><span data-stu-id="36d11-112">InsertMethod</span></span>
> - <span data-ttu-id="36d11-113">UpdateMethod</span><span class="sxs-lookup"><span data-stu-id="36d11-113">UpdateMethod</span></span>
> 
> <span data-ttu-id="36d11-114">Essas propriedades recebem o nome do método que trata a operação correspondente.</span><span class="sxs-lookup"><span data-stu-id="36d11-114">These properties receive the name of the method that handles the corresponding operation.</span></span> <span data-ttu-id="36d11-115">Dentro desse método, você pode fornecer a lógica para interagir com os dados.</span><span class="sxs-lookup"><span data-stu-id="36d11-115">Within that method, you provide the logic for interacting with the data.</span></span>
> 
> <span data-ttu-id="36d11-116">Este tutorial baseia-se no projeto criado no primeiro [parte](retrieving-data.md) da série.</span><span class="sxs-lookup"><span data-stu-id="36d11-116">This tutorial builds on the project created in the first [part](retrieving-data.md) of the series.</span></span>
> 
> <span data-ttu-id="36d11-117">Você pode [baixar](https://go.microsoft.com/fwlink/?LinkId=286116) o projeto completo em c# ou VB.</span><span class="sxs-lookup"><span data-stu-id="36d11-117">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="36d11-118">O código para download funciona com o Visual Studio 2012 ou Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="36d11-118">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="36d11-119">Ele usa o modelo do Visual Studio 2012, que é ligeiramente diferente do que o modelo do Visual Studio 2013 mostrado neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="36d11-119">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="36d11-120">Será possível compilar</span><span class="sxs-lookup"><span data-stu-id="36d11-120">What you'll build</span></span>

<span data-ttu-id="36d11-121">Neste tutorial, você vai:</span><span class="sxs-lookup"><span data-stu-id="36d11-121">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="36d11-122">Adicionar modelos de dados dinâmicos</span><span class="sxs-lookup"><span data-stu-id="36d11-122">Add dynamic data templates</span></span>
2. <span data-ttu-id="36d11-123">Habilitar atualização e exclusão de dados por meio de métodos de associação de modelo</span><span class="sxs-lookup"><span data-stu-id="36d11-123">Enable updating and deleting data through model binding methods</span></span>
3. <span data-ttu-id="36d11-124">Aplicar regras de validação de dados - permitem a criação de um novo registro no banco de dados</span><span class="sxs-lookup"><span data-stu-id="36d11-124">Apply data validation rules - Enable creating a new record in the database</span></span>

## <a name="add-dynamic-data-templates"></a><span data-ttu-id="36d11-125">Adicionar modelos de dados dinâmicos</span><span class="sxs-lookup"><span data-stu-id="36d11-125">Add dynamic data templates</span></span>

<span data-ttu-id="36d11-126">Para fornecer a melhor experiência de usuário e minimizar a repetição de código, você usará os modelos de dados dinâmicos.</span><span class="sxs-lookup"><span data-stu-id="36d11-126">To provide the best user experience and minimize code repetition, you will use dynamic data templates.</span></span> <span data-ttu-id="36d11-127">Você pode facilmente integrar modelos predefinidos dados dinâmicos em seu site existente ao instalar um pacote do NuGet.</span><span class="sxs-lookup"><span data-stu-id="36d11-127">You can easily integrate pre-built dynamic data templates into your existing site by installing a NuGet package.</span></span>

<span data-ttu-id="36d11-128">Do **gerenciar pacotes NuGet**, instale o **DynamicDataTemplatesCS**.</span><span class="sxs-lookup"><span data-stu-id="36d11-128">From the **Manage NuGet Packages**, install the **DynamicDataTemplatesCS**.</span></span>

![modelos de dados dinâmicos](updating-deleting-and-creating-data/_static/image1.png)

<span data-ttu-id="36d11-130">Observe que o projeto agora inclui uma pasta denominada **DynamicData**.</span><span class="sxs-lookup"><span data-stu-id="36d11-130">Notice that your project now includes a folder named **DynamicData**.</span></span> <span data-ttu-id="36d11-131">Nessa pasta, você encontrará os modelos que são aplicados automaticamente aos controles dinâmicos em seus formulários da web.</span><span class="sxs-lookup"><span data-stu-id="36d11-131">In that folder, you will find the templates that are automatically applied to dynamic controls in your web forms.</span></span>

![pasta de dados dinâmicos](updating-deleting-and-creating-data/_static/image2.png)

## <a name="enable-updating-and-deleting"></a><span data-ttu-id="36d11-133">Ativar a atualização e exclusão</span><span class="sxs-lookup"><span data-stu-id="36d11-133">Enable updating and deleting</span></span>

<span data-ttu-id="36d11-134">Habilitar os usuários a atualizar e excluir registros no banco de dados é muito semelhante ao processo de recuperação de dados.</span><span class="sxs-lookup"><span data-stu-id="36d11-134">Enabling users to update and delete records in the database is very similar to the process for retrieving data.</span></span> <span data-ttu-id="36d11-135">No **UpdateMethod** e **DeleteMethod** propriedades, que você especificar os nomes dos métodos que executam essas operações.</span><span class="sxs-lookup"><span data-stu-id="36d11-135">In the **UpdateMethod** and **DeleteMethod** properties, you specify the names of the methods that perform those operations.</span></span> <span data-ttu-id="36d11-136">Com um controle GridView, você também pode especificar a geração automática de editar e excluir botões.</span><span class="sxs-lookup"><span data-stu-id="36d11-136">With a GridView control, you can also specify the automatic generation of edit and delete buttons.</span></span> <span data-ttu-id="36d11-137">O seguinte código mostra as adições para o código de GridView.</span><span class="sxs-lookup"><span data-stu-id="36d11-137">The following highlighted code shows the additions to the GridView code.</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample1.aspx?highlight=4-5)]

<span data-ttu-id="36d11-138">O arquivo code-behind, adicione um usando a instrução para **System.Data.Entity.Infrastructure**.</span><span class="sxs-lookup"><span data-stu-id="36d11-138">In the code-behind file, add a using statement for **System.Data.Entity.Infrastructure**.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample2.cs)]

<span data-ttu-id="36d11-139">Em seguida, adicione a seguinte atualização e exclusão métodos.</span><span class="sxs-lookup"><span data-stu-id="36d11-139">Then, add the following update and delete methods.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample3.cs)]

<span data-ttu-id="36d11-140">O **TryUpdateModel** método aplica-se os valores correspondentes de associação de dados do formulário da web para o item de dados.</span><span class="sxs-lookup"><span data-stu-id="36d11-140">The **TryUpdateModel** method applies the matching data-bound values from the web form to the data item.</span></span> <span data-ttu-id="36d11-141">O item de dados é recuperado com base no valor do parâmetro id.</span><span class="sxs-lookup"><span data-stu-id="36d11-141">The data item is retrieved based on the value of the id parameter.</span></span>

## <a name="enforce-validation-requirements"></a><span data-ttu-id="36d11-142">Impor requisitos de validação</span><span class="sxs-lookup"><span data-stu-id="36d11-142">Enforce validation requirements</span></span>

<span data-ttu-id="36d11-143">Os atributos de validação que são aplicados às propriedades na classe aluno FirstName, LastName e ano são aplicados automaticamente ao atualizar os dados.</span><span class="sxs-lookup"><span data-stu-id="36d11-143">The validation attributes that you applied to the FirstName, LastName, and Year properties in the Student class are automatically enforced when updating the data.</span></span> <span data-ttu-id="36d11-144">Os controles DynamicField adicionar validadores de cliente e servidor com base nos atributos de validação.</span><span class="sxs-lookup"><span data-stu-id="36d11-144">The DynamicField controls add client and server validators based on the validation attributes.</span></span> <span data-ttu-id="36d11-145">As propriedades de nome e sobrenome são ambos necessários.</span><span class="sxs-lookup"><span data-stu-id="36d11-145">The FirstName and LastName properties are both required.</span></span> <span data-ttu-id="36d11-146">Nome não pode exceder 20 caracteres de comprimento e LastName não pode exceder 40 caracteres.</span><span class="sxs-lookup"><span data-stu-id="36d11-146">FirstName cannot exceed 20 characters in length, and LastName cannot exceed 40 characters.</span></span> <span data-ttu-id="36d11-147">Ano deve ser um valor válido para a enumeração AcademicYear.</span><span class="sxs-lookup"><span data-stu-id="36d11-147">Year must be a valid value for the AcademicYear enumeration.</span></span>

<span data-ttu-id="36d11-148">Se o usuário violar um dos requisitos de validação, a atualização não continuará.</span><span class="sxs-lookup"><span data-stu-id="36d11-148">If the user violates one of the validation requirements, the update does not proceed.</span></span> <span data-ttu-id="36d11-149">Para ver a mensagem de erro, adicione um controle ValidationSummary acima GridView.</span><span class="sxs-lookup"><span data-stu-id="36d11-149">To see the error message, add a ValidationSummary control above the GridView.</span></span> <span data-ttu-id="36d11-150">Para exibir os erros de validação da associação de modelo, defina o **ShowModelStateErrors** propriedade definida como **true**.</span><span class="sxs-lookup"><span data-stu-id="36d11-150">To display the validation errors from model binding, set the **ShowModelStateErrors** property set to **true**.</span></span> 

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample4.aspx)]

<span data-ttu-id="36d11-151">Executar o aplicativo da web, atualizar e excluir todos os registros.</span><span class="sxs-lookup"><span data-stu-id="36d11-151">Run the web application, and update and delete any of the records.</span></span>

![Atualizar dados](updating-deleting-and-creating-data/_static/image3.png)

<span data-ttu-id="36d11-153">Observe que, quando no modo de edição de valor para a propriedade de ano automaticamente é renderizado como uma lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="36d11-153">Notice that when in the edit mode the value for the Year property is automatically rendered as a drop down list.</span></span> <span data-ttu-id="36d11-154">A propriedade Year é um valor de enumeração e o modelo de dados dinâmicos para um valor de enumeração Especifica uma lista suspensa de edição.</span><span class="sxs-lookup"><span data-stu-id="36d11-154">The Year property is an enumeration value, and the dynamic data template for an enumeration value specifies a drop down list for editing.</span></span> <span data-ttu-id="36d11-155">Você pode encontrar esse modelo abrindo o **enumeração\_Edit.ascx** arquivo o **DynamicData**/**FieldTemplates** pasta.</span><span class="sxs-lookup"><span data-stu-id="36d11-155">You can find that template by opening the **Enumeration\_Edit.ascx** file in the **DynamicData**/**FieldTemplates** folder.</span></span>

<span data-ttu-id="36d11-156">Se você fornecer valores válidos, a atualização for concluída com êxito.</span><span class="sxs-lookup"><span data-stu-id="36d11-156">If you provide valid values, the update completes successfully.</span></span> <span data-ttu-id="36d11-157">Se você violar um dos requisitos de validação, a atualização não continuará e uma mensagem de erro é exibida acima da grade.</span><span class="sxs-lookup"><span data-stu-id="36d11-157">If you violate one of the validation requirements, the update does not proceed and an error message is displayed above the grid.</span></span>

![Mensagem de erro](updating-deleting-and-creating-data/_static/image4.png)

## <a name="add-new-records"></a><span data-ttu-id="36d11-159">Adicionar novos registros</span><span class="sxs-lookup"><span data-stu-id="36d11-159">Add new records</span></span>

<span data-ttu-id="36d11-160">O controle GridView não inclui o **InsertMethod** propriedade e, portanto, não pode ser usado para adicionar um novo registro com associação de modelo.</span><span class="sxs-lookup"><span data-stu-id="36d11-160">The GridView control does not include the **InsertMethod** property and therefore cannot be used for adding a new record with model binding.</span></span> <span data-ttu-id="36d11-161">Você pode encontrar a propriedade InsertMethod no **FormView**, **DetailsView**, ou **ListView** controles.</span><span class="sxs-lookup"><span data-stu-id="36d11-161">You can find the InsertMethod property in the **FormView**, **DetailsView**, or **ListView** controls.</span></span> <span data-ttu-id="36d11-162">Neste tutorial, você usará um controle FormView para adicionar um novo registro.</span><span class="sxs-lookup"><span data-stu-id="36d11-162">In this tutorial, you will use a FormView control to add a new record.</span></span>

<span data-ttu-id="36d11-163">Primeiro, adicione um link para a nova página que você criará para adicionar um novo registro.</span><span class="sxs-lookup"><span data-stu-id="36d11-163">First, add a link to the new page you will create for adding a new record.</span></span> <span data-ttu-id="36d11-164">Acima ValidationSummary, adicione:</span><span class="sxs-lookup"><span data-stu-id="36d11-164">Above the ValidationSummary, add:</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample5.aspx)]

<span data-ttu-id="36d11-165">O novo link será exibido na parte superior do conteúdo para a página de alunos.</span><span class="sxs-lookup"><span data-stu-id="36d11-165">The new link will appear at the top of the content for the Students page.</span></span>

![novo link](updating-deleting-and-creating-data/_static/image5.png)

<span data-ttu-id="36d11-167">Em seguida, adicione um novo formulário da web usando uma página mestra e nomeie-o **AddStudent**.</span><span class="sxs-lookup"><span data-stu-id="36d11-167">Then, add a new web form using a master page, and name it **AddStudent**.</span></span> <span data-ttu-id="36d11-168">Selecione Site.Master como a página mestra.</span><span class="sxs-lookup"><span data-stu-id="36d11-168">Select Site.Master as the master page.</span></span>

<span data-ttu-id="36d11-169">Você irá processar os campos para adicionar um novo aluno usando um **DynamicEntity** controle.</span><span class="sxs-lookup"><span data-stu-id="36d11-169">You will render the fields for adding a new student by using a **DynamicEntity** control.</span></span> <span data-ttu-id="36d11-170">O controle DynamicEntity renderiza que propriedades editáveis na classe especificada na propriedade ItemType.</span><span class="sxs-lookup"><span data-stu-id="36d11-170">The DynamicEntity control renders that editable properties in the class specified in the ItemType property.</span></span> <span data-ttu-id="36d11-171">A propriedade StudentID foi marcada com o **[ScaffoldColumn(false)]** atributo para que ele não será renderizado.</span><span class="sxs-lookup"><span data-stu-id="36d11-171">The StudentID property was marked with the **[ScaffoldColumn(false)]** attribute so it is not rendered.</span></span> <span data-ttu-id="36d11-172">O espaço reservado de MainContent da página AddStudent, adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="36d11-172">In the MainContent placeholder of the AddStudent page, add the following code.</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample6.aspx)]

<span data-ttu-id="36d11-173">No arquivo code-behind (AddStudent.aspx.cs), adicione um **usando** instrução para o **ContosoUniversityModelBinding.Models** namespace.</span><span class="sxs-lookup"><span data-stu-id="36d11-173">In the code-behind file (AddStudent.aspx.cs), add a **using** statement for the **ContosoUniversityModelBinding.Models** namespace.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample7.cs)]

<span data-ttu-id="36d11-174">Em seguida, adicione os seguintes métodos para especificar como inserir um novo registro e um manipulador de eventos para o botão Cancelar.</span><span class="sxs-lookup"><span data-stu-id="36d11-174">Then, add the following methods to specify how to insert a new record and an event handler for the cancel button.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample8.cs)]

<span data-ttu-id="36d11-175">Salve todas as alterações.</span><span class="sxs-lookup"><span data-stu-id="36d11-175">Save all of the changes.</span></span>

<span data-ttu-id="36d11-176">Executar o aplicativo web e criar um novo aluno.</span><span class="sxs-lookup"><span data-stu-id="36d11-176">Run the web application and create a new student.</span></span>

![Adicionar novo aluno](updating-deleting-and-creating-data/_static/image6.png)

<span data-ttu-id="36d11-178">Clique em **inserir** e observe o novo aluno foi criado.</span><span class="sxs-lookup"><span data-stu-id="36d11-178">Click **Insert** and notice the new student has been created.</span></span>

![Exibir o novo aluno](updating-deleting-and-creating-data/_static/image7.png)

## <a name="conclusion"></a><span data-ttu-id="36d11-180">Conclusão</span><span class="sxs-lookup"><span data-stu-id="36d11-180">Conclusion</span></span>

<span data-ttu-id="36d11-181">Neste tutorial, você habilitou a atualização, exclusão e criação de dados.</span><span class="sxs-lookup"><span data-stu-id="36d11-181">In this tutorial, you enabled updating, deleting, and creating data.</span></span> <span data-ttu-id="36d11-182">Você garante que as regras de validação são aplicadas ao interagir com os dados.</span><span class="sxs-lookup"><span data-stu-id="36d11-182">You ensured validation rules are applied when interacting with the data.</span></span>

<span data-ttu-id="36d11-183">Na próxima [tutorial](sorting-paging-and-filtering-data.md) nesta série, você habilitará a classificação, paginação e filtragem de dados.</span><span class="sxs-lookup"><span data-stu-id="36d11-183">In the next [tutorial](sorting-paging-and-filtering-data.md) in this series, you will enable sorting, paging, and filtering data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="36d11-184">[Anterior](retrieving-data.md)
> [Próximo](sorting-paging-and-filtering-data.md)</span><span class="sxs-lookup"><span data-stu-id="36d11-184">[Previous](retrieving-data.md)
[Next](sorting-paging-and-filtering-data.md)</span></span>
