---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: "Banco de dados EF primeiro com o ASP.NET MVC: personalizar um modo de exibição | Microsoft Docs"
author: tfitzmac
description: "Usando o MVC, Entity Framework e estrutura do ASP.NET, você pode criar um aplicativo web que fornece uma interface para um banco de dados existente. Este tutorial série..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: af9609396cff18b08824732731ddb9c5cca578fa
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="ef-database-first-with-aspnet-mvc-customizing-a-view"></a><span data-ttu-id="088d1-104">Banco de dados EF primeiro com o ASP.NET MVC: personalizar um modo de exibição</span><span class="sxs-lookup"><span data-stu-id="088d1-104">EF Database First with ASP.NET MVC: Customizing a View</span></span>
====================
<span data-ttu-id="088d1-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="088d1-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="088d1-106">Usando o MVC, Entity Framework e estrutura do ASP.NET, você pode criar um aplicativo web que fornece uma interface para um banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="088d1-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="088d1-107">Esta série de tutorial mostra como automaticamente gerar um código que permite aos usuários exibir, editar, criar e excluir dados que residem em uma tabela de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="088d1-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="088d1-108">O código gerado corresponde às colunas na tabela de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="088d1-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="088d1-109">Esta parte da série se concentra em como alterar os modos de exibição gerado automaticamente para aprimorar a apresentação.</span><span class="sxs-lookup"><span data-stu-id="088d1-109">This part of the series focuses on changing the automatically-generated views to enhance the presentation.</span></span>


## <a name="add-enrolled-courses-to-student-details"></a><span data-ttu-id="088d1-110">Adicionar cursos registrados detalhes do aluno</span><span class="sxs-lookup"><span data-stu-id="088d1-110">Add enrolled courses to student details</span></span>

<span data-ttu-id="088d1-111">O código gerado fornece um bom ponto de partida para o seu aplicativo, mas não necessariamente fornece toda a funcionalidade que você necessita em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="088d1-111">The generated code provides a good starting point for your application but it does not necessarily provide all of the functionality that you need in your application.</span></span> <span data-ttu-id="088d1-112">Você pode personalizar o código para atender os requisitos específicos de seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="088d1-112">You can customize the code to meet the particular requirements of your application.</span></span> <span data-ttu-id="088d1-113">Atualmente, o aplicativo não exibir os cursos registrados do aluno selecionado.</span><span class="sxs-lookup"><span data-stu-id="088d1-113">Currently, your application does not display the enrolled courses for the selected student.</span></span> <span data-ttu-id="088d1-114">Nesta seção, você adicionará os cursos registrados para cada aluno para o **detalhes** exibição do aluno.</span><span class="sxs-lookup"><span data-stu-id="088d1-114">In this section, you will add the enrolled courses for each student to the **Details** view for the student.</span></span>

<span data-ttu-id="088d1-115">Abra **Students/Details.cshtml**e abaixo do último &lt;/dl&gt; guia, mas antes do fechamento &lt;/div&gt; marca, adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="088d1-115">Open **Students/Details.cshtml**, and below the last &lt;/dl&gt; tab, but before the closing &lt;/div&gt; tag, add the following code.</span></span>

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

<span data-ttu-id="088d1-116">Esse código cria uma tabela que exibe uma linha para cada registro na tabela de registro do aluno selecionado.</span><span class="sxs-lookup"><span data-stu-id="088d1-116">This code creates a table that displays a row for each record in the Enrollment table for the selected student.</span></span> <span data-ttu-id="088d1-117">O **exibição** método renderiza HTML para o objeto (modelItem) que representa a expressão.</span><span class="sxs-lookup"><span data-stu-id="088d1-117">The **Display** method renders HTML for the object (modelItem) that represents the expression.</span></span> <span data-ttu-id="088d1-118">Use o método de exibição (em vez de simplesmente inserindo o valor da propriedade no código) para verificar se o valor é formatado corretamente com base no seu tipo e o modelo para esse tipo.</span><span class="sxs-lookup"><span data-stu-id="088d1-118">You use the Display method (rather than simply embedding the property value in the code) to make sure the value is formatted correctly based on its type and the template for that type.</span></span> <span data-ttu-id="088d1-119">Neste exemplo, cada expressão retorna uma única propriedade de registro atual no loop e os valores são tipos primitivos que são processados como texto.</span><span class="sxs-lookup"><span data-stu-id="088d1-119">In this example, each expression returns a single property from the current record in the loop, and the values are primitive types which are rendered as text.</span></span>

<span data-ttu-id="088d1-120">Navegue até o modo de exibição de alunos/índice novamente e selecione **detalhes** para um dos alunos.</span><span class="sxs-lookup"><span data-stu-id="088d1-120">Browse to the Students/Index view again and select **Details** for one of the students.</span></span> <span data-ttu-id="088d1-121">Você verá que os cursos registrados foram incluídos na exibição.</span><span class="sxs-lookup"><span data-stu-id="088d1-121">You will see the enrolled courses have been included in the view.</span></span>

![aluno com registro](customizing-a-view/_static/image1.png)

>[!div class="step-by-step"]
<span data-ttu-id="088d1-123">[Anterior](changing-the-database.md)
[Próximo](enhancing-data-validation.md)</span><span class="sxs-lookup"><span data-stu-id="088d1-123">[Previous](changing-the-database.md)
[Next](enhancing-data-validation.md)</span></span>
