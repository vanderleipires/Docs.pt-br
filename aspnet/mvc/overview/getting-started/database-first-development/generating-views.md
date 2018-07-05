---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: 'Banco de dados do EF primeiro com o ASP.NET MVC: gerando exibições | Microsoft Docs'
author: tfitzmac
description: Usando o MVC, Entity Framework e o Scaffolding do ASP.NET, você pode criar um aplicativo web que fornece uma interface para um banco de dados existente. Esta série de tutoriais...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/29/2014
ms.topic: article
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: 58f8be181f80f5b27353e9ef5cafe48bcb370e29
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399604"
---
<a name="ef-database-first-with-aspnet-mvc-generating-views"></a><span data-ttu-id="e2409-104">Banco de dados do EF primeiro com o ASP.NET MVC: gerando exibições</span><span class="sxs-lookup"><span data-stu-id="e2409-104">EF Database First with ASP.NET MVC: Generating Views</span></span>
====================
<span data-ttu-id="e2409-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="e2409-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="e2409-106">Usando o MVC, Entity Framework e o Scaffolding do ASP.NET, você pode criar um aplicativo web que fornece uma interface para um banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="e2409-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="e2409-107">Esta série de tutoriais mostra como automaticamente gerar um código que permite aos usuários exibir, editar, criar e excluir dados que residem em uma tabela de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e2409-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="e2409-108">O código gerado corresponde às colunas na tabela de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e2409-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="e2409-109">Esta parte da série se concentra no uso de Scaffolding do ASP.NET para gerar os controladores e modos de exibição.</span><span class="sxs-lookup"><span data-stu-id="e2409-109">This part of the series focuses on using ASP.NET Scaffolding to generate the controllers and views.</span></span>


## <a name="add-scaffold"></a><span data-ttu-id="e2409-110">Adicionar scaffold</span><span class="sxs-lookup"><span data-stu-id="e2409-110">Add scaffold</span></span>

<span data-ttu-id="e2409-111">Você está pronto para gerar o código que irá fornecer operações de dados padrão para as classes de modelo.</span><span class="sxs-lookup"><span data-stu-id="e2409-111">You are ready to generate code that will provide standard data operations for the model classes.</span></span> <span data-ttu-id="e2409-112">Adicione o código adicionando um item de scaffold.</span><span class="sxs-lookup"><span data-stu-id="e2409-112">You add the code by adding a scaffold item.</span></span> <span data-ttu-id="e2409-113">Há muitas opções para o tipo de scaffolding que você pode adicionar; Neste tutorial, o scaffold incluirá um controlador e exibições que correspondem aos modelos de aluno e registro que você criou na seção anterior.</span><span class="sxs-lookup"><span data-stu-id="e2409-113">There are many options for the type of scaffolding you can add; in this tutorial, the scaffold will include a controller and views that correspond to the Student and Enrollment models you created in the previous section.</span></span>

<span data-ttu-id="e2409-114">Para manter a consistência em seu projeto, você adicionará o novo controlador ao existente **controladores** pasta.</span><span class="sxs-lookup"><span data-stu-id="e2409-114">To maintain consistency in your project, you will add the new controller to the existing **Controllers** folder.</span></span> <span data-ttu-id="e2409-115">Clique com botão direito do **controladores** pasta e selecione **Add** – **New Scaffolded Item**.</span><span class="sxs-lookup"><span data-stu-id="e2409-115">Right-click the **Controllers** folder, and select **Add** – **New Scaffolded Item**.</span></span>

![Adicionar scaffold](generating-views/_static/image1.png)

<span data-ttu-id="e2409-117">Selecione o **controlador MVC 5 com modos de exibição usando o Entity Framework** opção.</span><span class="sxs-lookup"><span data-stu-id="e2409-117">Select the **MVC 5 Controller with views, using Entity Framework** option.</span></span> <span data-ttu-id="e2409-118">Esta opção irá gerar o controlador e exibições para atualizar, excluir, criando e exibindo os dados em seu modelo.</span><span class="sxs-lookup"><span data-stu-id="e2409-118">This option will generate the controller and views for updating, deleting, creating and displaying the data in your model.</span></span>

![Adicionar controlador mvc](generating-views/_static/image2.png)

<span data-ttu-id="e2409-120">Selecione **aluno** para a classe de modelo e selecione o **ContosoUniversityEntities** para a classe de contexto.</span><span class="sxs-lookup"><span data-stu-id="e2409-120">Select **Student** for the model class, and select the **ContosoUniversityEntities** for the context class.</span></span> <span data-ttu-id="e2409-121">Mantenha o nome do controlador como **StudentsController**,</span><span class="sxs-lookup"><span data-stu-id="e2409-121">Keep the controller name as **StudentsController**,</span></span>

![Especifique o controlador](generating-views/_static/image3.png)

<span data-ttu-id="e2409-123">Clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="e2409-123">Click **Add**.</span></span>

<span data-ttu-id="e2409-124">Se você receber um erro, pode ser porque você não tiver criado o projeto na seção anterior.</span><span class="sxs-lookup"><span data-stu-id="e2409-124">If you receive an error, it may be because you did not build the project in the previous section.</span></span> <span data-ttu-id="e2409-125">Nesse caso, tente compilar o projeto e, em seguida, adicione o item com Scaffold novamente.</span><span class="sxs-lookup"><span data-stu-id="e2409-125">If so, try building the project, and then add the scaffolded item again.</span></span>

<span data-ttu-id="e2409-126">Após a conclusão do processo de geração de código, você verá um novo controlador e exibições em seu projeto.</span><span class="sxs-lookup"><span data-stu-id="e2409-126">After the code generation process is complete, you will see a new controller and views in your project.</span></span>

![Mostrar modos de exibição](generating-views/_static/image4.png)

<span data-ttu-id="e2409-128">Execute as mesmas etapas novamente, mas adicionar um scaffold para a classe de registro.</span><span class="sxs-lookup"><span data-stu-id="e2409-128">Perform the same steps again, but add a scaffold for the Enrollment class.</span></span> <span data-ttu-id="e2409-129">Quando terminar, você deve ter uma **EnrollmentsController.cs** arquivo e uma pasta sob **exibições** denominada **registros** com Create, Delete, detalhes, editar e índice Modos de exibição.</span><span class="sxs-lookup"><span data-stu-id="e2409-129">When finished, you should have an **EnrollmentsController.cs** file, and a folder under **Views** named **Enrollments** with the Create, Delete, Details, Edit and Index views.</span></span>

![Mostrar modos de exibição](generating-views/_static/image5.png)

## <a name="add-links-to-new-views"></a><span data-ttu-id="e2409-131">Adicionar links às novas exibições</span><span class="sxs-lookup"><span data-stu-id="e2409-131">Add links to new views</span></span>

<span data-ttu-id="e2409-132">Para tornar mais fácil de navegar para seus novos modos de exibição, você pode adicionar alguns dos hiperlinks para as exibições de índice para estudantes e registros.</span><span class="sxs-lookup"><span data-stu-id="e2409-132">To make it easier for you to navigate to your new views, you can add a couple of hyperlinks to the Index views for students and enrollments.</span></span> <span data-ttu-id="e2409-133">Abra o arquivo no **Views/Home/Index.cshtml**, que é a home page do seu site.</span><span class="sxs-lookup"><span data-stu-id="e2409-133">Open the file at **Views/Home/Index.cshtml**, which is the home page for your site.</span></span> <span data-ttu-id="e2409-134">Adicione o código a seguir o jumbotron.</span><span class="sxs-lookup"><span data-stu-id="e2409-134">Add the following code below the jumbotron.</span></span>

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

<span data-ttu-id="e2409-135">Para o método ActionLink, o primeiro parâmetro é o texto a ser exibido no link.</span><span class="sxs-lookup"><span data-stu-id="e2409-135">For the ActionLink method, the first parameter is the text to display in the link.</span></span> <span data-ttu-id="e2409-136">O segundo parâmetro é a ação e o terceiro parâmetro é o nome do controlador.</span><span class="sxs-lookup"><span data-stu-id="e2409-136">The second parameter is the action and the third parameter is the name of the controller.</span></span> <span data-ttu-id="e2409-137">Por exemplo, o primeiro link aponta para a ação de índice em StudentsController.</span><span class="sxs-lookup"><span data-stu-id="e2409-137">For example, the first link points to the Index action in StudentsController.</span></span> <span data-ttu-id="e2409-138">O hiperlink real é construído com esses valores.</span><span class="sxs-lookup"><span data-stu-id="e2409-138">The actual hyperlink is constructed from these values.</span></span> <span data-ttu-id="e2409-139">O primeiro link, por fim, leva os usuários para o **index. cshtml** dentro do arquivo a **modos de exibição/alunos** pasta.</span><span class="sxs-lookup"><span data-stu-id="e2409-139">The first link ultimately takes users to the **Index.cshtml** file within the **Views/Students** folder.</span></span>

## <a name="display-student-views"></a><span data-ttu-id="e2409-140">Mostrar exibições de aluno</span><span class="sxs-lookup"><span data-stu-id="e2409-140">Display student views</span></span>

<span data-ttu-id="e2409-141">Você verificará que o código adicionado ao seu projeto corretamente exibe uma lista dos alunos e permite aos usuários editar, criar ou excluir os registros de alunos no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e2409-141">You will verify that the code added to your project correctly displays a list of the students, and enables users to edit, create, or delete the student records in the database.</span></span>

<span data-ttu-id="e2409-142">Clique com botão direito do **Views/Home/Index.cshtml** do arquivo e selecione **exibir no navegador**.</span><span class="sxs-lookup"><span data-stu-id="e2409-142">Right-click the **Views/Home/Index.cshtml** file, and select **View in Browser**.</span></span> <span data-ttu-id="e2409-143">Nessa página, clique no link para obter a lista de alunos.</span><span class="sxs-lookup"><span data-stu-id="e2409-143">On this page, click the link for the list of students.</span></span>

![](generating-views/_static/image6.png)

<span data-ttu-id="e2409-144">Nessa página, observe a lista de alunos e links para modificar esses dados.</span><span class="sxs-lookup"><span data-stu-id="e2409-144">On this page, notice the list of the students and links to modify this data.</span></span>

![lista de alunos](generating-views/_static/image7.png)

<span data-ttu-id="e2409-146">Clique o **criar novo** vincular e fornecer alguns valores para um novo aluno.</span><span class="sxs-lookup"><span data-stu-id="e2409-146">Click the **Create New** link and provide some values for a new student.</span></span>

![Criar novo aluno](generating-views/_static/image8.png)

<span data-ttu-id="e2409-148">Clique em **criar**e observe o novo aluno é adicionado à sua lista.</span><span class="sxs-lookup"><span data-stu-id="e2409-148">Click **Create**, and notice the new student is added to your list.</span></span>

![lista com o novo aluno](generating-views/_static/image9.png)

<span data-ttu-id="e2409-150">Selecione o **editar** link e alterar alguns dos valores de um aluno.</span><span class="sxs-lookup"><span data-stu-id="e2409-150">Select the **Edit** link, and change some of the values for a student.</span></span>

![Editar aluno](generating-views/_static/image10.png)

<span data-ttu-id="e2409-152">Clique em **salvar**e observe o registro de aluno foi alterado.</span><span class="sxs-lookup"><span data-stu-id="e2409-152">Click **Save**, and notice the student record has been changed.</span></span>

<span data-ttu-id="e2409-153">Por fim, selecione o **exclua** vincular e confirme que você deseja excluir o registro clicando o **excluir** botão.</span><span class="sxs-lookup"><span data-stu-id="e2409-153">Finally, select the **Delete** link and confirm that you want to delete the record by clicking the **Delete** button.</span></span>

![Excluir aluno](generating-views/_static/image11.png)

<span data-ttu-id="e2409-155">Sem escrever nenhum código, você adicionou as exibições que executam operações comuns nos dados na tabela aluno.</span><span class="sxs-lookup"><span data-stu-id="e2409-155">Without writing any code, you have added views that perform common operations on the data in the Student table.</span></span>

<span data-ttu-id="e2409-156">Você deve ter notado que o rótulo de texto para um campo se baseia na propriedade de banco de dados (como **LastName**) que não é necessariamente o que você deseja exibir na página da web.</span><span class="sxs-lookup"><span data-stu-id="e2409-156">You may have noticed that the text label for a field is based on the database property (such as **LastName**) which is not necessarily what you want to display on the web page.</span></span> <span data-ttu-id="e2409-157">Por exemplo, você pode preferir o rótulo seja **Sobrenome**.</span><span class="sxs-lookup"><span data-stu-id="e2409-157">For example, you may prefer the label to be **Last Name**.</span></span> <span data-ttu-id="e2409-158">Você corrigirá esse problema de exibição posteriormente no tutorial.</span><span class="sxs-lookup"><span data-stu-id="e2409-158">You will fix this display issue later in the tutorial.</span></span>

## <a name="display-enrollment-views"></a><span data-ttu-id="e2409-159">Mostrar exibições de registro</span><span class="sxs-lookup"><span data-stu-id="e2409-159">Display enrollment views</span></span>

<span data-ttu-id="e2409-160">Seu banco de dados inclui uma relação um-para-muitos entre as tabelas Student e registro e uma relação um-para-muitos entre as tabelas de registro e curso.</span><span class="sxs-lookup"><span data-stu-id="e2409-160">Your database includes a one-to-many relationship between the Student and Enrollment tables, and a one-to-many relationship between the Course and Enrollment tables.</span></span> <span data-ttu-id="e2409-161">Os modos de exibição para o registro corretamente lidar com essas relações.</span><span class="sxs-lookup"><span data-stu-id="e2409-161">The views for Enrollment correctly handle these relationships.</span></span> <span data-ttu-id="e2409-162">Navegue até a home page do seu site e selecione o **lista de registros** link e, em seguida, o **criar novo** link.</span><span class="sxs-lookup"><span data-stu-id="e2409-162">Navigate to the home page for your site and select the **List of enrollments** link and then the **Create New** link.</span></span> <span data-ttu-id="e2409-163">O modo de exibição exibe um formulário para criar um novo registro de registro.</span><span class="sxs-lookup"><span data-stu-id="e2409-163">The view displays a form for creating a new enrollment record.</span></span> <span data-ttu-id="e2409-164">Em particular, observe que o formulário contém duas listas suspensas que são preenchidas com valores das tabelas relacionadas.</span><span class="sxs-lookup"><span data-stu-id="e2409-164">In particular, notice that the form contains two drop-down lists that are populated with values from the related tables.</span></span>

![Criar registro](generating-views/_static/image12.png)

<span data-ttu-id="e2409-166">Além disso, a validação dos valores fornecidos é aplicada automaticamente com base no tipo de dados do campo.</span><span class="sxs-lookup"><span data-stu-id="e2409-166">Furthermore, validation of the provided values is automatically applied based on the data type of the field.</span></span> <span data-ttu-id="e2409-167">Nível empresarial requer um número, portanto, uma mensagem de erro será exibida se você tentar fornecer um valor incompatível.</span><span class="sxs-lookup"><span data-stu-id="e2409-167">Grade requires a number, so an error message is displayed if you try to provide an incompatible value.</span></span>

![mensagem de validação](generating-views/_static/image13.png)

<span data-ttu-id="e2409-169">Você verificou que os modos de exibição gerados automaticamente permitem que os usuários trabalhar com os dados no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e2409-169">You have verified that the automatically-generated views enable users to work with the data in the database.</span></span> <span data-ttu-id="e2409-170">No próximo tutorial desta série, você irá atualizar o banco de dados e faça as alterações correspondentes no aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="e2409-170">In the next tutorial in this series, you will update the database and make the corresponding changes in the web application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e2409-171">[Anterior](creating-the-web-application.md)
> [Próximo](changing-the-database.md)</span><span class="sxs-lookup"><span data-stu-id="e2409-171">[Previous](creating-the-web-application.md)
[Next](changing-the-database.md)</span></span>
