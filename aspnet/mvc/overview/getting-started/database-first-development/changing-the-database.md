---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: 'Banco de dados do EF primeiro com o ASP.NET MVC: alterar o banco de dados | Microsoft Docs'
author: tfitzmac
description: Usando o MVC, Entity Framework e o Scaffolding do ASP.NET, você pode criar um aplicativo web que fornece uma interface para um banco de dados existente. Esta série de tutoriais...
ms.author: riande
ms.date: 10/01/2014
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 4ee73dc066a56944dd2e5600460628656ae87e37
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831634"
---
<a name="ef-database-first-with-aspnet-mvc-changing-the-database"></a><span data-ttu-id="fda1b-104">Banco de dados do EF primeiro com o ASP.NET MVC: alterar o banco de dados</span><span class="sxs-lookup"><span data-stu-id="fda1b-104">EF Database First with ASP.NET MVC: Changing the Database</span></span>
====================
<span data-ttu-id="fda1b-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="fda1b-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="fda1b-106">Usando o MVC, Entity Framework e o Scaffolding do ASP.NET, você pode criar um aplicativo web que fornece uma interface para um banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="fda1b-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="fda1b-107">Esta série de tutoriais mostra como automaticamente gerar um código que permite aos usuários exibir, editar, criar e excluir dados que residem em uma tabela de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="fda1b-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="fda1b-108">O código gerado corresponde às colunas na tabela de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="fda1b-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="fda1b-109">Esta parte da série se concentra em fazer uma atualização para a estrutura de banco de dados e propagar essa alteração em todo o aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="fda1b-109">This part of the series focuses on making an update to the database structure and propagating that change throughout the web application.</span></span>


## <a name="add-a-column"></a><span data-ttu-id="fda1b-110">Adicionar uma coluna</span><span class="sxs-lookup"><span data-stu-id="fda1b-110">Add a column</span></span>

<span data-ttu-id="fda1b-111">Se você atualizar a estrutura de uma tabela no banco de dados, você precisa garantir que sua alteração seja propagada para o modelo de dados, modos de exibição e controlador.</span><span class="sxs-lookup"><span data-stu-id="fda1b-111">If you update the structure of a table in your database, you need to ensure that your change is propagated to the data model, views, and controller.</span></span>

<span data-ttu-id="fda1b-112">Para este tutorial, você adicionará uma nova coluna à tabela aluno para registrar o nome do meio do aluno.</span><span class="sxs-lookup"><span data-stu-id="fda1b-112">For this tutorial, you will add a new column to the Student table to record the middle name of the student.</span></span> <span data-ttu-id="fda1b-113">Para adicionar essa coluna, abra o projeto de banco de dados e abra o arquivo Student.sql.</span><span class="sxs-lookup"><span data-stu-id="fda1b-113">To add this column, open the database project, and open the Student.sql file.</span></span> <span data-ttu-id="fda1b-114">Por meio do designer ou o código T-SQL, adicione uma coluna denominada **MiddleName** que é um nvarchar (50) e permite valores nulos.</span><span class="sxs-lookup"><span data-stu-id="fda1b-114">Through either the designer or the T-SQL code, add a column named **MiddleName** that is an NVARCHAR(50) and allows NULL values.</span></span>

![Adicionar nome do meio](changing-the-database/_static/image1.png)

<span data-ttu-id="fda1b-116">Implante essa alteração em seu banco de dados local Iniciando a seu projeto de banco de dados (ou F5).</span><span class="sxs-lookup"><span data-stu-id="fda1b-116">Deploy this change to your local database by starting your database project (or F5).</span></span> <span data-ttu-id="fda1b-117">O novo campo é adicionado à tabela.</span><span class="sxs-lookup"><span data-stu-id="fda1b-117">The new field is added to the table.</span></span> <span data-ttu-id="fda1b-118">Se você não vê-lo no Pesquisador de objetos do SQL Server, clique no botão Atualizar no painel.</span><span class="sxs-lookup"><span data-stu-id="fda1b-118">If you do not see it in the SQL Server Object Explorer, click the Refresh button in the pane.</span></span>

![Mostrar a nova coluna](changing-the-database/_static/image2.png)

<span data-ttu-id="fda1b-120">A nova coluna existe na tabela de banco de dados, mas ele não existe atualmente na classe de modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="fda1b-120">The new column exists in the database table, but it does not currently exist in the data model class.</span></span> <span data-ttu-id="fda1b-121">Você deve atualizar o modelo para incluir sua nova coluna.</span><span class="sxs-lookup"><span data-stu-id="fda1b-121">You must update the model to include your new column.</span></span> <span data-ttu-id="fda1b-122">No **modelos** pasta, abra o **ContosoModel.edmx** arquivo para exibir o diagrama de modelo.</span><span class="sxs-lookup"><span data-stu-id="fda1b-122">In the **Models** folder, open the **ContosoModel.edmx** file to display the model diagram.</span></span> <span data-ttu-id="fda1b-123">Observe que o modelo aluno não contém a propriedade MiddleName.</span><span class="sxs-lookup"><span data-stu-id="fda1b-123">Notice that the Student model does not contain the MiddleName property.</span></span> <span data-ttu-id="fda1b-124">Clique com botão direito em qualquer lugar na superfície de design e, em seguida, selecione **modelo de atualização do banco de dados**.</span><span class="sxs-lookup"><span data-stu-id="fda1b-124">Right-click anywhere on the design surface, and select **Update Model from Database**.</span></span>

![Atualizar modelo](changing-the-database/_static/image3.png)

<span data-ttu-id="fda1b-126">No Assistente de atualização, selecione o **Refresh** guia e o **aluno** tabela.</span><span class="sxs-lookup"><span data-stu-id="fda1b-126">In the Update Wizard, select the **Refresh** tab and the **Student** table.</span></span>

![Assistente de atualização](changing-the-database/_static/image4.png)

<span data-ttu-id="fda1b-128">Clique em **Finalizar**.</span><span class="sxs-lookup"><span data-stu-id="fda1b-128">Click **Finish**.</span></span>

<span data-ttu-id="fda1b-129">Depois que o processo de atualização for concluído, o diagrama de banco de dados inclui o novo **MiddleName** propriedade.</span><span class="sxs-lookup"><span data-stu-id="fda1b-129">After the update process is finished, the database diagram includes the new **MiddleName** property.</span></span> <span data-ttu-id="fda1b-130">Salvar a **ContosoModel.edmx** arquivo.</span><span class="sxs-lookup"><span data-stu-id="fda1b-130">Save the **ContosoModel.edmx** file.</span></span> <span data-ttu-id="fda1b-131">Você deve salvar este arquivo para a nova propriedade ser propagada para o **Student.cs** classe.</span><span class="sxs-lookup"><span data-stu-id="fda1b-131">You must save this file for the new property to be propagated to the **Student.cs** class.</span></span> <span data-ttu-id="fda1b-132">Você atualizou o banco de dados e o modelo.</span><span class="sxs-lookup"><span data-stu-id="fda1b-132">You have now updated the database and the model.</span></span>

<span data-ttu-id="fda1b-133">Compile a solução.</span><span class="sxs-lookup"><span data-stu-id="fda1b-133">Build the solution.</span></span>

<span data-ttu-id="fda1b-134">Infelizmente, os modos de exibição ainda não contêm a nova propriedade.</span><span class="sxs-lookup"><span data-stu-id="fda1b-134">Unfortunately, the views still do not contain the new property.</span></span> <span data-ttu-id="fda1b-135">Para atualizar os modos de exibição você tem duas opções – você pode gerar novamente as exibições, adicionando mais uma vez o scaffolding para a classe de aluno ou você pode adicionar manualmente a nova propriedade para exibições existentes.</span><span class="sxs-lookup"><span data-stu-id="fda1b-135">To update the views you have two options - you can either re-generate the views by once again adding scaffolding for the Student class, or you can manually add the new property to your existing views.</span></span> <span data-ttu-id="fda1b-136">Neste tutorial, você adicionará o scaffolding novamente porque você não tiver feito alterações personalizadas aos modos de exibição gerados automaticamente.</span><span class="sxs-lookup"><span data-stu-id="fda1b-136">In this tutorial, you will add the scaffolding again because you have not made any customized changes to the automatically-generated views.</span></span> <span data-ttu-id="fda1b-137">Você pode considerar adicionar manualmente a propriedade quando você tiver feito alterações aos modos de exibição e não quiser perder essas alterações.</span><span class="sxs-lookup"><span data-stu-id="fda1b-137">You might consider manually adding the property when you have made changes to the views and do not want to lose those changes.</span></span>

<span data-ttu-id="fda1b-138">Para garantir que as exibições são recriadas, exclua o **alunos** pasta sob **exibições**e excluir os **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="fda1b-138">To ensure the views are re-created, delete the **Students** folder under **Views**, and delete the **StudentsController**.</span></span> <span data-ttu-id="fda1b-139">Em seguida, clique com botão direito do **controladores** pasta e adicionar o scaffolding para o **aluno** modelo.</span><span class="sxs-lookup"><span data-stu-id="fda1b-139">Then, right-click the **Controllers** folder and add scaffolding for the **Student** model.</span></span> <span data-ttu-id="fda1b-140">Novamente, nomeie o controlador **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="fda1b-140">Again, name the controller **StudentsController**.</span></span> <span data-ttu-id="fda1b-141">Selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="fda1b-141">Select **OK**.</span></span>

<span data-ttu-id="fda1b-142">As exibições contêm agora a propriedade MiddleName.</span><span class="sxs-lookup"><span data-stu-id="fda1b-142">The views now contain the MiddleName property.</span></span>

![Mostrar o nome do meio](changing-the-database/_static/image5.png)

<span data-ttu-id="fda1b-144">Na próxima seção, você adicionará código para personalizar o modo de exibição para mostrar os detalhes sobre um registro de aluno.</span><span class="sxs-lookup"><span data-stu-id="fda1b-144">In the next section, you will add code to customize the view for showing details about a student record.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fda1b-145">[Anterior](generating-views.md)
> [Próximo](customizing-a-view.md)</span><span class="sxs-lookup"><span data-stu-id="fda1b-145">[Previous](generating-views.md)
[Next](customizing-a-view.md)</span></span>
