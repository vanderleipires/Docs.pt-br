---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-new-field
title: Adicionar um novo campo para o modelo de filme e tabela (c#) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial ensina as Noções básicas de criação de um aplicativo Web do ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: b4e76c1a-f66e-43a0-aa72-f39df79c07c1
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: f6364e438bbb7e128945255a5150e1e84e593ac4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-new-field-to-the-movie-model-and-table-c"></a><span data-ttu-id="a1cd9-103">Adicionar um novo campo para o modelo de filme e tabela (c#)</span><span class="sxs-lookup"><span data-stu-id="a1cd9-103">Adding a New Field to the Movie Model and Table (C#)</span></span>
====================
<span data-ttu-id="a1cd9-104">por [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="a1cd9-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="a1cd9-105">Uma versão atualizada deste tutorial está disponível [aqui](../../../getting-started/introduction/getting-started.md) que usa o ASP.NET MVC 5 e Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="a1cd9-105">An updated version of this tutorial is available [here](../../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="a1cd9-106">É muito mais simples a seguir, mais segura e demonstra mais recursos.</span><span class="sxs-lookup"><span data-stu-id="a1cd9-106">It's more secure, much simpler to follow and demonstrates more features.</span></span>
> 
> 
> <span data-ttu-id="a1cd9-107">Este tutorial ensina as Noções básicas de criação de um aplicativo Web do ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é uma versão gratuita do Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a1cd9-107">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="a1cd9-108">Antes de começar, verifique se que você instalou os pré-requisitos listados abaixo.</span><span class="sxs-lookup"><span data-stu-id="a1cd9-108">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="a1cd9-109">Você pode instalar todos eles clicando no link a seguir: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="a1cd9-109">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="a1cd9-110">Como alternativa, você pode instalar individualmente os pré-requisitos usando os links a seguir:</span><span class="sxs-lookup"><span data-stu-id="a1cd9-110">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="a1cd9-111">Pré-requisitos de Visual Studio Web Developer Express SP1</span><span class="sxs-lookup"><span data-stu-id="a1cd9-111">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="a1cd9-112">Atualização de ferramentas do ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="a1cd9-112">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="a1cd9-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(tempo de execução + ferramentas de suportam)</span><span class="sxs-lookup"><span data-stu-id="a1cd9-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="a1cd9-114">Se você estiver usando o Visual Studio 2010 em vez do Visual Web Developer 2010, instale os pré-requisitos clicando no link a seguir: [pré-requisitos do Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="a1cd9-114">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="a1cd9-115">Um projeto do Visual Web Developer ao código-fonte c# está disponível para acompanhar este tópico.</span><span class="sxs-lookup"><span data-stu-id="a1cd9-115">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="a1cd9-116">[Baixe a versão c#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="a1cd9-116">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="a1cd9-117">Se você preferir o Visual Basic, alterne para o [versão do Visual Basic](../vb/intro-to-aspnet-mvc-3.md) deste tutorial.</span><span class="sxs-lookup"><span data-stu-id="a1cd9-117">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.</span></span>


<span data-ttu-id="a1cd9-118">Nesta seção, você fazer algumas alterações para as classes de modelo e saiba como você pode atualizar o esquema de banco de dados de acordo com as alterações do modelo.</span><span class="sxs-lookup"><span data-stu-id="a1cd9-118">In this section you'll make some changes to the model classes and learn how you can update the database schema to match the model changes.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="a1cd9-119">Adicionando uma propriedade de classificação ao modelo de filme</span><span class="sxs-lookup"><span data-stu-id="a1cd9-119">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="a1cd9-120">Comece adicionando um novo `Rating` propriedade existente `Movie` classe.</span><span class="sxs-lookup"><span data-stu-id="a1cd9-120">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="a1cd9-121">Abra o *Movie.cs* e adicione o `Rating` propriedade como esta:</span><span class="sxs-lookup"><span data-stu-id="a1cd9-121">Open the *Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

<span data-ttu-id="a1cd9-122">Completo `Movie` classe agora parece com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="a1cd9-122">The complete `Movie` class now looks like the following code:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

<span data-ttu-id="a1cd9-123">Recompilar o aplicativo usando o **depurar** &gt; **criar filme** comando de menu.</span><span class="sxs-lookup"><span data-stu-id="a1cd9-123">Recompile the application using the **Debug** &gt;**Build Movie** menu command.</span></span>

<span data-ttu-id="a1cd9-124">Agora que você atualizou o `Model` classe, você também precisa atualizar o *\Views\Movies\Index.cshtml* e *\Views\Movies\Create.cshtml* exibir modelos para oferecer suporte a novos `Rating`propriedade.</span><span class="sxs-lookup"><span data-stu-id="a1cd9-124">Now that you've updated the `Model` class, you also need to update the *\Views\Movies\Index.cshtml* and *\Views\Movies\Create.cshtml* view templates in order to support the new `Rating` property.</span></span>

<span data-ttu-id="a1cd9-125">Abra o *\Views\Movies\Index.cshtml* e adicione um `<th>Rating</th>` cabeçalho da coluna logo após o **preço** coluna.</span><span class="sxs-lookup"><span data-stu-id="a1cd9-125">Open the *\Views\Movies\Index.cshtml* file and add a `<th>Rating</th>` column heading just after the **Price** column.</span></span> <span data-ttu-id="a1cd9-126">Em seguida, adicione um `<td>` coluna próximo ao final do modelo para renderizar o `@item.Rating` valor.</span><span class="sxs-lookup"><span data-stu-id="a1cd9-126">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="a1cd9-127">Abaixo está o que a atualização *cshtml* aparência do modelo de exibição:</span><span class="sxs-lookup"><span data-stu-id="a1cd9-127">Below is what the updated *Index.cshtml* view template looks like:</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample3.cshtml)]

<span data-ttu-id="a1cd9-128">Em seguida, abra o *\Views\Movies\Create.cshtml* de arquivo e adicione a seguinte marcação próximo ao final do formulário.</span><span class="sxs-lookup"><span data-stu-id="a1cd9-128">Next, open the *\Views\Movies\Create.cshtml* file and add the following markup near the end of the form.</span></span> <span data-ttu-id="a1cd9-129">Isso apresenta uma caixa de texto para que você pode especificar uma classificação quando um filme novo é criado.</span><span class="sxs-lookup"><span data-stu-id="a1cd9-129">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample4.cshtml)]

## <a name="managing-model-and-database-schema-differences"></a><span data-ttu-id="a1cd9-130">Gerenciamento de modelo e as diferenças de esquema de banco de dados</span><span class="sxs-lookup"><span data-stu-id="a1cd9-130">Managing Model and Database Schema Differences</span></span>

<span data-ttu-id="a1cd9-131">Agora que você atualizou o código do aplicativo para dar suporte aos novos `Rating` propriedade.</span><span class="sxs-lookup"><span data-stu-id="a1cd9-131">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="a1cd9-132">Agora execute o aplicativo e navegue até o */Movies* URL.</span><span class="sxs-lookup"><span data-stu-id="a1cd9-132">Now run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="a1cd9-133">Quando você fizer isso, no entanto, você verá o seguinte erro:</span><span class="sxs-lookup"><span data-stu-id="a1cd9-133">When you do this, though, you'll see the following error:</span></span>

![](adding-a-new-field/_static/image1.png)

<span data-ttu-id="a1cd9-134">Você está vendo este erro, porque a atualização `Movie` classe de modelo no aplicativo agora é diferente do esquema do `Movie` tabela do banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="a1cd9-134">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="a1cd9-135">(Não há nenhuma coluna `Rating` na tabela de banco de dados.)</span><span class="sxs-lookup"><span data-stu-id="a1cd9-135">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="a1cd9-136">Por padrão, ao usar o Entity Framework Code First para criar automaticamente um banco de dados, como você fez anteriormente neste tutorial, Code First adiciona uma tabela no banco de dados para ajudar a controlar se o esquema do banco de dados está em sincronizado com classes de modelo do que qual ela foi gerada.</span><span class="sxs-lookup"><span data-stu-id="a1cd9-136">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="a1cd9-137">Se não estiverem sincronizados, o Entity Framework gerará um erro.</span><span class="sxs-lookup"><span data-stu-id="a1cd9-137">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="a1cd9-138">Isso torna mais fácil rastrear os problemas em tempo de desenvolvimento que você pode caso contrário, apenas encontrar (por erros obscuros) em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="a1cd9-138">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span> <span data-ttu-id="a1cd9-139">O recurso de verificação de sincronização é o que faz com que a mensagem de erro a ser exibido que você acabou de ver.</span><span class="sxs-lookup"><span data-stu-id="a1cd9-139">The sync-checking feature is what causes the error message to be displayed that you just saw.</span></span>

<span data-ttu-id="a1cd9-140">Há duas abordagens para resolver o erro:</span><span class="sxs-lookup"><span data-stu-id="a1cd9-140">There are two approaches to resolving the error:</span></span>

1. <span data-ttu-id="a1cd9-141">Faça com que o Entity Framework remova automaticamente e recrie o banco de dados com base no novo esquema de classe de modelo.</span><span class="sxs-lookup"><span data-stu-id="a1cd9-141">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="a1cd9-142">Essa abordagem é muito conveniente ao fazer o desenvolvimento ativo em um banco de dados de teste, porque ele permite que você rapidamente desenvolver o esquema de banco de dados e modelo juntos.</span><span class="sxs-lookup"><span data-stu-id="a1cd9-142">This approach is very convenient when doing active development on a test database, because it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="a1cd9-143">No entanto, a desvantagem é que você perca os dados existentes no banco de dados — para que você *não* para usar essa abordagem em um banco de dados de produção!</span><span class="sxs-lookup"><span data-stu-id="a1cd9-143">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span>
2. <span data-ttu-id="a1cd9-144">Modifique explicitamente o esquema do banco de dados existente para que ele corresponda às classes de modelo.</span><span class="sxs-lookup"><span data-stu-id="a1cd9-144">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="a1cd9-145">A vantagem dessa abordagem é que você mantém os dados.</span><span class="sxs-lookup"><span data-stu-id="a1cd9-145">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="a1cd9-146">Faça essa alteração manualmente ou criando um script de alteração de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="a1cd9-146">You can make this change either manually or by creating a database change script.</span></span>

<span data-ttu-id="a1cd9-147">Para este tutorial, vamos usar a primeira abordagem, você terá o Entity Framework Code First automaticamente recriar o banco de dados sempre que o modelo é alterado.</span><span class="sxs-lookup"><span data-stu-id="a1cd9-147">For this tutorial, we'll use the first approach — you'll have the Entity Framework Code First automatically re-create the database anytime the model changes.</span></span>

## <a name="automatically-re-creating-the-database-on-model-changes"></a><span data-ttu-id="a1cd9-148">Recriando o banco de dados do modelo é alterado automaticamente</span><span class="sxs-lookup"><span data-stu-id="a1cd9-148">Automatically Re-Creating the Database on Model Changes</span></span>

<span data-ttu-id="a1cd9-149">Vamos atualizar o aplicativo para que o Code First automaticamente descarta e recria o banco de dados sempre que você alterar o modelo para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="a1cd9-149">Let's update the application so that Code First automatically drops and re-creates the database anytime you change the model for the application.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="a1cd9-150">**Aviso** você deve habilitar essa abordagem de automaticamente descartar e recriar o banco de dados somente quando você estiver usando um banco de dados de desenvolvimento ou teste, e *nunca* em um banco de dados de produção que contém os dados reais.</span><span class="sxs-lookup"><span data-stu-id="a1cd9-150">**Warning** You should enable this approach of automatically dropping and re-creating the database only when you're using a development or test database, and *never* on a production database that contains real data.</span></span> <span data-ttu-id="a1cd9-151">Usá-lo em um servidor de produção pode causar a perda de dados.</span><span class="sxs-lookup"><span data-stu-id="a1cd9-151">Using it on a production server can lead to data loss.</span></span>


<span data-ttu-id="a1cd9-152">Em **Solution Explorer**, clique com botão direito do *modelos* pasta, selecione **adicionar**e, em seguida, selecione **classe**.</span><span class="sxs-lookup"><span data-stu-id="a1cd9-152">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-new-field/_static/image2.png)

<span data-ttu-id="a1cd9-153">Nomeie a classe "MovieInitializer".</span><span class="sxs-lookup"><span data-stu-id="a1cd9-153">Name the class "MovieInitializer".</span></span> <span data-ttu-id="a1cd9-154">Atualização de `MovieInitializer` classe para conter o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="a1cd9-154">Update the `MovieInitializer` class to contain the following code:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

<span data-ttu-id="a1cd9-155">O `MovieInitializer` classe especifica que o banco de dados usado pelo modelo deve ser descartado e automaticamente recriado se as classes de modelo mudar.</span><span class="sxs-lookup"><span data-stu-id="a1cd9-155">The `MovieInitializer` class specifies that the database used by the model should be dropped and automatically re-created if the model classes ever change.</span></span> <span data-ttu-id="a1cd9-156">O código inclui um `Seed` método para especificar que alguns dados padrão para adicionar automaticamente para o banco de dados de qualquer uma vez em que foi criado (ou recriado).</span><span class="sxs-lookup"><span data-stu-id="a1cd9-156">The code includes a `Seed` method to specify some default data to automatically add to the database any time it's created (or re-created).</span></span> <span data-ttu-id="a1cd9-157">Isso fornece uma maneira útil para preencher o banco de dados com alguns dados de exemplo, sem exigir que você popular manualmente cada vez que você alterar um modelo.</span><span class="sxs-lookup"><span data-stu-id="a1cd9-157">This provides a useful way to populate the database with some sample data, without requiring you to manually populate it each time you make a model change.</span></span>

<span data-ttu-id="a1cd9-158">Agora que você definiu o `MovieInitializer` classe, você desejará conectá-lo para que cada vez que o aplicativo é executado, ele verifica se as classes de modelo são diferentes do esquema no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="a1cd9-158">Now that you've defined the `MovieInitializer` class, you'll want to wire it up so that each time the application runs, it checks whether the model classes are different from the schema in the database.</span></span> <span data-ttu-id="a1cd9-159">Se estiverem, você pode executar o inicializador para recriar o banco de dados para coincidir com o modelo e, em seguida, preencher o banco de dados com os dados de exemplo.</span><span class="sxs-lookup"><span data-stu-id="a1cd9-159">If they are, you can run the initializer to re-create the database to match the model and then populate the database with the sample data.</span></span>

<span data-ttu-id="a1cd9-160">Abra o *global. asax* arquivo que está na raiz do `MvcMovies` projeto:</span><span class="sxs-lookup"><span data-stu-id="a1cd9-160">Open the *Global.asax* file that's at the root of the `MvcMovies` project:</span></span>

[![](adding-a-new-field/_static/image4.png)](adding-a-new-field/_static/image3.png)

<span data-ttu-id="a1cd9-161">O *global. asax* arquivo contém a classe que define o aplicativo inteiro para o projeto e contém um `Application_Start` manipulador de eventos que é executado quando o aplicativo é iniciado pela primeira vez.</span><span class="sxs-lookup"><span data-stu-id="a1cd9-161">The *Global.asax* file contains the class that defines the entire application for the project, and contains an `Application_Start` event handler that runs when the application first starts.</span></span>

<span data-ttu-id="a1cd9-162">Vamos adicionar dois usando as instruções para a parte superior do arquivo.</span><span class="sxs-lookup"><span data-stu-id="a1cd9-162">Let's add two using statements to the top of the file.</span></span> <span data-ttu-id="a1cd9-163">O primeiro faz referência ao namespace de Entity Framework e o segundo faz referência ao namespace onde nosso `MovieInitializer` classe vidas:</span><span class="sxs-lookup"><span data-stu-id="a1cd9-163">The first references the Entity Framework namespace, and the second references the namespace where our `MovieInitializer` class lives:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs)]

<span data-ttu-id="a1cd9-164">Localize o `Application_Start` método e adicionar uma chamada para `Database.SetInitializer` no início do método, conforme mostrado abaixo:</span><span class="sxs-lookup"><span data-stu-id="a1cd9-164">Then find the `Application_Start` method and add a call to `Database.SetInitializer` at the beginning of the method, as shown below:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs)]

<span data-ttu-id="a1cd9-165">O `Database.SetInitializer` instrução que você acabou de adicionar indica que o banco de dados usado pela `MovieDBContext` instância deve ser automaticamente excluída e criada novamente se o esquema e o banco de dados não correspondem.</span><span class="sxs-lookup"><span data-stu-id="a1cd9-165">The `Database.SetInitializer` statement you just added indicates that the database used by the `MovieDBContext` instance should be automatically deleted and re-created if the schema and the database don't match.</span></span> <span data-ttu-id="a1cd9-166">E como você viu, ele preencherá o banco de dados com os dados de exemplo que são especificados no também o `MovieInitializer` classe.</span><span class="sxs-lookup"><span data-stu-id="a1cd9-166">And as you saw, it will also populate the database with the sample data that's specified in the `MovieInitializer` class.</span></span>

<span data-ttu-id="a1cd9-167">Fechar o *global. asax* arquivo.</span><span class="sxs-lookup"><span data-stu-id="a1cd9-167">Close the *Global.asax* file.</span></span>

<span data-ttu-id="a1cd9-168">Execute novamente o aplicativo e navegue até o */Movies* URL.</span><span class="sxs-lookup"><span data-stu-id="a1cd9-168">Re-run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="a1cd9-169">Quando o aplicativo for iniciado, ele detecta que a estrutura do modelo não coincide com o esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="a1cd9-169">When the application starts, it detects that the model structure no longer matches the database schema.</span></span> <span data-ttu-id="a1cd9-170">Ele automaticamente recria o banco de dados para coincidir com a nova estrutura de modelo e preenche o banco de dados com os filmes de exemplo:</span><span class="sxs-lookup"><span data-stu-id="a1cd9-170">It automatically re-creates the database to match the new model structure and populates the database with the sample movies:</span></span>

![7_MyMovieList_SM](adding-a-new-field/_static/image5.png)

<span data-ttu-id="a1cd9-172">Clique o **criar novo** link para adicionar um novo filme.</span><span class="sxs-lookup"><span data-stu-id="a1cd9-172">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="a1cd9-173">Observe que você pode adicionar uma classificação.</span><span class="sxs-lookup"><span data-stu-id="a1cd9-173">Note that you can add a rating.</span></span>

<span data-ttu-id="a1cd9-174">[![7_CreateRioII](adding-a-new-field/_static/image7.png)](adding-a-new-field/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="a1cd9-174">[![7_CreateRioII](adding-a-new-field/_static/image7.png)](adding-a-new-field/_static/image6.png)</span></span>

<span data-ttu-id="a1cd9-175">Clique em **Criar**.</span><span class="sxs-lookup"><span data-stu-id="a1cd9-175">Click **Create**.</span></span> <span data-ttu-id="a1cd9-176">Novo filme, incluindo classificação, agora é exibido nos filmes listando:</span><span class="sxs-lookup"><span data-stu-id="a1cd9-176">The new movie, including the rating, now shows up in the movies listing:</span></span>

<span data-ttu-id="a1cd9-177">[![7_ourNewMovie_SM](adding-a-new-field/_static/image9.png)](adding-a-new-field/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="a1cd9-177">[![7_ourNewMovie_SM](adding-a-new-field/_static/image9.png)](adding-a-new-field/_static/image8.png)</span></span>

<span data-ttu-id="a1cd9-178">Nesta seção, você viu como você pode modificar objetos de modelo e manter o banco de dados em sincronia com as alterações.</span><span class="sxs-lookup"><span data-stu-id="a1cd9-178">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="a1cd9-179">Você também aprendeu uma maneira para popular um banco de dados recém-criado com dados de exemplo para que você pode experimentar os cenários.</span><span class="sxs-lookup"><span data-stu-id="a1cd9-179">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="a1cd9-180">Em seguida, vamos dar uma olhada em como você pode adicionar lógica de validação mais rica para as classes de modelo e habilitar algumas regras de negócio a serem aplicadas.</span><span class="sxs-lookup"><span data-stu-id="a1cd9-180">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a1cd9-181">[Anterior](examining-the-edit-methods-and-edit-view.md)
> [Próximo](adding-validation-to-the-model.md)</span><span class="sxs-lookup"><span data-stu-id="a1cd9-181">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-validation-to-the-model.md)</span></span>
