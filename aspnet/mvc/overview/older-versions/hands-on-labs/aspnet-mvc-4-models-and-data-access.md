---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
title: Acesso a dados e modelos do ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: 'Observação: Este laboratório prático supõe que você tenha um conhecimento básico do ASP.NET MVC. Se você não tiver usado o ASP.NET MVC antes, recomendamos que você falar sobre o ASP.NET MVC 4...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 634ea84b-f904-4afe-b71b-49cccef4d9cc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
msc.type: authoredcontent
ms.openlocfilehash: 57477cf15bf6755523f28356d5384517bea24982
ms.sourcegitcommit: 5ae0c125ee3bbd324edef3818d1d160f4dd84602
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/17/2018
---
# <a name="aspnet-mvc-4-models-and-data-access"></a><span data-ttu-id="93c2d-104">Acesso a dados e modelos do ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="93c2d-104">ASP.NET MVC 4 Models and Data Access</span></span>

<span data-ttu-id="93c2d-105">Por [Web Camps Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="93c2d-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="93c2d-106">Baixar o Kit de treinamento de Camps de Web</span><span class="sxs-lookup"><span data-stu-id="93c2d-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="93c2d-107">Este laboratório prático supõe que você tenha um conhecimento básico de **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="93c2d-107">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="93c2d-108">Se você não usou **ASP.NET MVC** antes, é recomendável que você passe **conceitos básicos do ASP.NET MVC 4** laboratório prático.</span><span class="sxs-lookup"><span data-stu-id="93c2d-108">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>

<span data-ttu-id="93c2d-109">Este laboratório orienta os aperfeiçoamentos e novos recursos descritos anteriormente, aplicando alterações secundárias a um aplicativo da Web de exemplo fornecido na pasta de origem.</span><span class="sxs-lookup"><span data-stu-id="93c2d-109">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>

> [!NOTE]
> <span data-ttu-id="93c2d-110">Todo o código de exemplo e trechos de código são incluídos no Web Camps treinamento Kit, disponíveis em [versões Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="93c2d-110">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="93c2d-111">O projeto específico para este laboratório está disponível em [acesso a dados e modelos do ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).</span><span class="sxs-lookup"><span data-stu-id="93c2d-111">The project specific to this lab is available at [ASP.NET MVC 4 Models and Data Access](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).</span></span>

<span data-ttu-id="93c2d-112">Em **conceitos básicos do ASP.NET MVC** laboratório prático, você tem foi passando dados embutida dos controladores de para os modelos de exibição.</span><span class="sxs-lookup"><span data-stu-id="93c2d-112">In **ASP.NET MVC Fundamentals** Hands-on Lab, you have been passing hard-coded data from the Controllers to the View templates.</span></span> <span data-ttu-id="93c2d-113">Mas, para criar um aplicativo Web real, você talvez queira usar um banco de dados real.</span><span class="sxs-lookup"><span data-stu-id="93c2d-113">But, in order to build a real Web application, you might want to use a real database.</span></span>

<span data-ttu-id="93c2d-114">Este laboratório prático mostram como usar um mecanismo de banco de dados para armazenar e recuperar os dados necessários para o aplicativo de repositório de música.</span><span class="sxs-lookup"><span data-stu-id="93c2d-114">This Hands-on Lab will show you how to use a database engine in order to store and retrieve the data needed for the Music Store application.</span></span> <span data-ttu-id="93c2d-115">Para fazer isso, inicie com um banco de dados existente e criar o modelo de dados de entidade a partir dele.</span><span class="sxs-lookup"><span data-stu-id="93c2d-115">To accomplish that, you will start with an existing database and create the Entity Data Model from it.</span></span> <span data-ttu-id="93c2d-116">Durante este laboratório, você atenderá a **Database First** abordagem, bem como a **Code First** abordagem.</span><span class="sxs-lookup"><span data-stu-id="93c2d-116">Throughout this lab, you will meet the **Database First** approach as well as the **Code First** approach.</span></span>

<span data-ttu-id="93c2d-117">No entanto, você também pode usar o **Model First** abordagem, criar o mesmo modelo usando as ferramentas e, em seguida, gerar o banco de dados a partir dele.</span><span class="sxs-lookup"><span data-stu-id="93c2d-117">However, you can also use the **Model First** approach, create the same model using the tools, and then generate the database from it.</span></span>

<span data-ttu-id="93c2d-118">![Vs primeiro do banco de dados. Modelo primeiro](aspnet-mvc-4-models-and-data-access/_static/image1.png "vs primeiro banco de dados. Modelo pela primeira vez")</span><span class="sxs-lookup"><span data-stu-id="93c2d-118">![Database First vs. Model First](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First vs. Model First")</span></span>

<span data-ttu-id="93c2d-119">*Vs primeiro do banco de dados. Modelo pela primeira vez*</span><span class="sxs-lookup"><span data-stu-id="93c2d-119">*Database First vs. Model First*</span></span>

<span data-ttu-id="93c2d-120">Depois de gerar o modelo, você fará os ajustes adequados no StoreController para fornecer os modos de exibição do repositório com os dados obtidos do banco de dados, em vez de usar dados embutidos.</span><span class="sxs-lookup"><span data-stu-id="93c2d-120">After generating the Model, you will make the proper adjustments in the StoreController to provide the Store Views with the data taken from the database, instead of using hard-coded data.</span></span> <span data-ttu-id="93c2d-121">Você não precisará fazer qualquer alteração nos modelos de exibição porque o StoreController será retornada mesmo ViewModels para os modelos de exibição, embora esse tempo os dados virão do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="93c2d-121">You will not need to make any change to the View templates because the StoreController will be returning the same ViewModels to the View templates, although this time the data will come from the database.</span></span>

<span data-ttu-id="93c2d-122">**A primeira abordagem de código**</span><span class="sxs-lookup"><span data-stu-id="93c2d-122">**The Code First Approach**</span></span>

<span data-ttu-id="93c2d-123">A abordagem de Code First permite definir o modelo do código sem gerar classes que são geralmente juntamente com a estrutura.</span><span class="sxs-lookup"><span data-stu-id="93c2d-123">The Code First approach allows us to define the model from the code without generating classes that are generally coupled with the framework.</span></span>

<span data-ttu-id="93c2d-124">No código em primeiro lugar, objetos de modelo são definidos com POCOs, &quot;Plain Old CLR Objects&quot;.</span><span class="sxs-lookup"><span data-stu-id="93c2d-124">In code first, model objects are defined with POCOs, &quot;Plain Old CLR Objects&quot;.</span></span> <span data-ttu-id="93c2d-125">POCOs são simples classes simples que não herança e não implementam as interfaces.</span><span class="sxs-lookup"><span data-stu-id="93c2d-125">POCOs are simple plain classes that have no inheritance and do not implement interfaces.</span></span> <span data-ttu-id="93c2d-126">Podemos gerar automaticamente o banco de dados deles, ou podemos usar um banco de dados e gera o mapeamento de classe a partir do código.</span><span class="sxs-lookup"><span data-stu-id="93c2d-126">We can automatically generate the database from them, or we can use an existing database and generate the class mapping from the code.</span></span>

<span data-ttu-id="93c2d-127">Os benefícios de usar essa abordagem é que o modelo permaneça independente da estrutura de persistência (nesse caso, Entity Framework), como as classes de POCOs não são juntamente com a estrutura de mapeamento.</span><span class="sxs-lookup"><span data-stu-id="93c2d-127">The benefits of using this approach is that the Model remains independent from the persistence framework (in this case, Entity Framework), as the POCOs classes are not coupled with the mapping framework.</span></span>

> [!NOTE]
> <span data-ttu-id="93c2d-128">Este laboratório é baseado no ASP.NET MVC 4 e uma versão do aplicativo de exemplo do repositório de música personalizada e minimizada para ajustar somente os recursos mostrados neste laboratório prático.</span><span class="sxs-lookup"><span data-stu-id="93c2d-128">This Lab is based on ASP.NET MVC 4 and a version of the Music Store sample application customized and minimized to fit only the features shown in this Hands-On Lab.</span></span>
> 
> <span data-ttu-id="93c2d-129">Se você quiser explorar todo o **repositório de música** aplicativo tutorial, você pode encontrar no [repositório de música MVC](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="93c2d-129">If you wish to explore the whole **Music Store** tutorial application you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="93c2d-130">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="93c2d-130">Prerequisites</span></span>

<span data-ttu-id="93c2d-131">Você deve ter os seguintes itens para concluir este laboratório:</span><span class="sxs-lookup"><span data-stu-id="93c2d-131">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="93c2d-132">[Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou superior (leitura [Apêndice A](#AppendixA) para obter instruções sobre como instalá-lo).</span><span class="sxs-lookup"><span data-stu-id="93c2d-132">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="93c2d-133">Configuração</span><span class="sxs-lookup"><span data-stu-id="93c2d-133">Setup</span></span>

<span data-ttu-id="93c2d-134">**Instalando os trechos de código**</span><span class="sxs-lookup"><span data-stu-id="93c2d-134">**Installing Code Snippets**</span></span>

<span data-ttu-id="93c2d-135">Para sua conveniência, boa parte do código que você estiver gerenciando ao longo deste laboratório está disponível como trechos de código do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="93c2d-135">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="93c2d-136">Para instalar os trechos de código executar **.\Source\Setup\CodeSnippets.vsi** arquivo.</span><span class="sxs-lookup"><span data-stu-id="93c2d-136">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="93c2d-137">Se você não estiver familiarizado com os trechos de código do Visual Studio e aprender como usá-los, consulte o Apêndice deste documento &quot; [trechos de código usando do apêndice c:](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="93c2d-137">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="93c2d-138">Exercícios</span><span class="sxs-lookup"><span data-stu-id="93c2d-138">Exercises</span></span>

<span data-ttu-id="93c2d-139">Este laboratório prático é composto pelos exercícios a seguir:</span><span class="sxs-lookup"><span data-stu-id="93c2d-139">This Hands-on Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="93c2d-140">Exercício 1: Adicionar um banco de dados</span><span class="sxs-lookup"><span data-stu-id="93c2d-140">Exercise 1: Adding a Database</span></span>](#Exercise1)
2. [<span data-ttu-id="93c2d-141">Exercício 2: Criar um banco de dados usando o Code First</span><span class="sxs-lookup"><span data-stu-id="93c2d-141">Exercise 2: Creating a Database using Code First</span></span>](#Exercise2)
3. [<span data-ttu-id="93c2d-142">Exercício 3: Consultando o banco de dados com parâmetros</span><span class="sxs-lookup"><span data-stu-id="93c2d-142">Exercise 3: Querying the Database with Parameters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="93c2d-143">Cada exercício é acompanhado por um **final** pasta que contém a solução resultante, você deve obter depois de concluir os exercícios.</span><span class="sxs-lookup"><span data-stu-id="93c2d-143">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="93c2d-144">Você pode usar essa solução como um guia se você precisar trabalhar com os exercícios de ajuda adicional.</span><span class="sxs-lookup"><span data-stu-id="93c2d-144">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="93c2d-145">Tempo estimado para concluir este laboratório: **35 minutos**.</span><span class="sxs-lookup"><span data-stu-id="93c2d-145">Estimated time to complete this lab: **35 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a><span data-ttu-id="93c2d-146">Exercício 1: Adicionar um banco de dados</span><span class="sxs-lookup"><span data-stu-id="93c2d-146">Exercise 1: Adding a Database</span></span>

<span data-ttu-id="93c2d-147">Neste exercício, você aprenderá como adicionar um banco de dados com as tabelas do aplicativo MusicStore à solução para consumir seus dados.</span><span class="sxs-lookup"><span data-stu-id="93c2d-147">In this exercise, you will learn how to add a database with the tables of the MusicStore application to the solution in order to consume its data.</span></span> <span data-ttu-id="93c2d-148">Depois que o banco de dados é gerado com o modelo e adicionado à solução, você modificará a classe StoreController para fornecer o modelo de exibição com os dados obtidos do banco de dados, em vez de usar valores embutidos.</span><span class="sxs-lookup"><span data-stu-id="93c2d-148">Once the database is generated with the model, and added to the solution, you will modify the StoreController class to provide the View template with the data taken from the database, instead of using hard-coded values.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a><span data-ttu-id="93c2d-149">Tarefa 1: adicionar um banco de dados</span><span class="sxs-lookup"><span data-stu-id="93c2d-149">Task 1 - Adding a Database</span></span>

<span data-ttu-id="93c2d-150">Nesta tarefa, você irá adicionar um banco de dados já criado com as principais tabelas do aplicativo MusicStore à solução.</span><span class="sxs-lookup"><span data-stu-id="93c2d-150">In this task, you will add an already created database with the main tables of the MusicStore application to the solution.</span></span>

1. <span data-ttu-id="93c2d-151">Abra o **começar** solução localizado em **fonte/Ex1-AddingADatabaseDBFirst/Begin/** pasta.</span><span class="sxs-lookup"><span data-stu-id="93c2d-151">Open the **Begin** solution located at **Source/Ex1-AddingADatabaseDBFirst/Begin/** folder.</span></span>

   1. <span data-ttu-id="93c2d-152">Você precisará baixar alguns pacotes do NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="93c2d-152">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="93c2d-153">Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="93c2d-153">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="93c2d-154">No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="93c2d-154">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="93c2d-155">Por fim, compile a solução clicando **criar** | **compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="93c2d-155">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="93c2d-156">Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="93c2d-156">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="93c2d-157">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="93c2d-157">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="93c2d-158">É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.</span><span class="sxs-lookup"><span data-stu-id="93c2d-158">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="93c2d-159">Adicionar **MvcMusicStore** arquivo de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="93c2d-159">Add **MvcMusicStore** database file.</span></span> <span data-ttu-id="93c2d-160">Neste laboratório prático, você usará um banco de dados já criado chamado **MvcMusicStore.mdf**.</span><span class="sxs-lookup"><span data-stu-id="93c2d-160">In this Hands-on Lab, you will use an already created database called **MvcMusicStore.mdf**.</span></span> <span data-ttu-id="93c2d-161">Para fazer isso, clique com botão direito **aplicativo\_dados** pasta, aponte para **adicionar** e, em seguida, clique em **Item existente**.</span><span class="sxs-lookup"><span data-stu-id="93c2d-161">To do that, right-click **App\_Data** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="93c2d-162">Navegue até **\Source\Assets** e selecione o **MvcMusicStore.mdf** arquivo.</span><span class="sxs-lookup"><span data-stu-id="93c2d-162">Browse to **\Source\Assets** and select the **MvcMusicStore.mdf** file.</span></span>

    <span data-ttu-id="93c2d-163">![Adicionar um Item existente](aspnet-mvc-4-models-and-data-access/_static/image2.png "adicionar um Item existente")</span><span class="sxs-lookup"><span data-stu-id="93c2d-163">![Adding an Existing Item](aspnet-mvc-4-models-and-data-access/_static/image2.png "Adding an Existing Item")</span></span>

    <span data-ttu-id="93c2d-164">*Adicionar um Item existente*</span><span class="sxs-lookup"><span data-stu-id="93c2d-164">*Adding an Existing Item*</span></span>

    <span data-ttu-id="93c2d-165">![Arquivo de banco de dados de MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf arquivo de banco de dados")</span><span class="sxs-lookup"><span data-stu-id="93c2d-165">![MvcMusicStore.mdf database file](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf database file")</span></span>

    <span data-ttu-id="93c2d-166">*Arquivo de banco de dados MvcMusicStore.mdf*</span><span class="sxs-lookup"><span data-stu-id="93c2d-166">*MvcMusicStore.mdf database file*</span></span>

    <span data-ttu-id="93c2d-167">O banco de dados foi adicionado ao projeto.</span><span class="sxs-lookup"><span data-stu-id="93c2d-167">The database has been added to the project.</span></span> <span data-ttu-id="93c2d-168">Mesmo quando o banco de dados está localizado dentro da solução, você pode consultar e atualizá-lo como ele foi armazenado em um servidor de banco de dados diferente.</span><span class="sxs-lookup"><span data-stu-id="93c2d-168">Even when the database is located inside the solution, you can query and update it as it was hosted in a different database server.</span></span>

    <span data-ttu-id="93c2d-169">![Banco de dados MvcMusicStore no Gerenciador de soluções](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore banco de dados no Gerenciador de soluções")</span><span class="sxs-lookup"><span data-stu-id="93c2d-169">![MvcMusicStore database in Solution Explorer](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore database in Solution Explorer")</span></span>

    <span data-ttu-id="93c2d-170">*Banco de dados MvcMusicStore no Gerenciador de soluções*</span><span class="sxs-lookup"><span data-stu-id="93c2d-170">*MvcMusicStore database in Solution Explorer*</span></span>
3. <span data-ttu-id="93c2d-171">Verifique se a conexão ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="93c2d-171">Verify the connection to the database.</span></span> <span data-ttu-id="93c2d-172">Para fazer isso, clique duas vezes em **MvcMusicStore.mdf** para estabelecer uma conexão.</span><span class="sxs-lookup"><span data-stu-id="93c2d-172">To do this, double-click **MvcMusicStore.mdf** to establish a connection.</span></span>

    <span data-ttu-id="93c2d-173">![Conectando-se a MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "conectando-se a MvcMusicStore.mdf")</span><span class="sxs-lookup"><span data-stu-id="93c2d-173">![Connecting to MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "Connecting to MvcMusicStore.mdf")</span></span>

    <span data-ttu-id="93c2d-174">*Conectando-se a MvcMusicStore.mdf*</span><span class="sxs-lookup"><span data-stu-id="93c2d-174">*Connecting to MvcMusicStore.mdf*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a><span data-ttu-id="93c2d-175">Tarefa 2: criar um modelo de dados</span><span class="sxs-lookup"><span data-stu-id="93c2d-175">Task 2 - Creating a Data Model</span></span>

<span data-ttu-id="93c2d-176">Nesta tarefa, você criará um modelo de dados para interagir com o banco de dados adicionado na tarefa anterior.</span><span class="sxs-lookup"><span data-stu-id="93c2d-176">In this task, you will create a data model to interact with the database added in the previous task.</span></span>

1. <span data-ttu-id="93c2d-177">Crie um modelo de dados que representa o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="93c2d-177">Create a data model that will represent the database.</span></span> <span data-ttu-id="93c2d-178">Para fazer isso, no botão direito do mouse do Gerenciador de soluções de **modelos** pasta, aponte para **adicionar** e, em seguida, clique em **Novo Item**.</span><span class="sxs-lookup"><span data-stu-id="93c2d-178">To do this, in Solution Explorer right-click the **Models** folder, point to **Add** and then click **New Item**.</span></span> <span data-ttu-id="93c2d-179">No **Adicionar Novo Item** caixa de diálogo, selecione o **dados** modelo e, em seguida, o **modelo de dados de entidade ADO.NET** item.</span><span class="sxs-lookup"><span data-stu-id="93c2d-179">In the **Add New Item** dialog, select the **Data** template and then the **ADO.NET Entity Data Model** item.</span></span> <span data-ttu-id="93c2d-180">Altere o nome do modelo de dados para **StoreDB.edmx** e clique em **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="93c2d-180">Change the data model name to **StoreDB.edmx** and click **Add**.</span></span>

    <span data-ttu-id="93c2d-181">![Adicionar o modelo de dados de entidade ADO.NET StoreDB](aspnet-mvc-4-models-and-data-access/_static/image6.png "adicionando o modelo de dados de entidade ADO.NET StoreDB")</span><span class="sxs-lookup"><span data-stu-id="93c2d-181">![Adding the StoreDB ADO.NET Entity Data Model](aspnet-mvc-4-models-and-data-access/_static/image6.png "Adding the StoreDB ADO.NET Entity Data Model")</span></span>

    <span data-ttu-id="93c2d-182">*Adicionar o modelo de dados de entidade ADO.NET StoreDB*</span><span class="sxs-lookup"><span data-stu-id="93c2d-182">*Adding the StoreDB ADO.NET Entity Data Model*</span></span>
2. <span data-ttu-id="93c2d-183">O **Assistente de modelo de dados de entidade** será exibida.</span><span class="sxs-lookup"><span data-stu-id="93c2d-183">The **Entity Data Model Wizard** will appear.</span></span> <span data-ttu-id="93c2d-184">Este assistente o guiará através da criação da camada de modelo.</span><span class="sxs-lookup"><span data-stu-id="93c2d-184">This wizard will guide you through the creation of the model layer.</span></span> <span data-ttu-id="93c2d-185">Uma vez que o modelo deve ser criado com base no recentyl existentes do banco de dados adicionado, selecione **gerar do banco de dados** e clique em **próximo**.</span><span class="sxs-lookup"><span data-stu-id="93c2d-185">Since the model should be created based on the existing database recentyl added, select **Generate from database** and click **Next**.</span></span>

    <span data-ttu-id="93c2d-186">![Escolhendo o conteúdo do modelo](aspnet-mvc-4-models-and-data-access/_static/image7.png "escolhendo o conteúdo do modelo")</span><span class="sxs-lookup"><span data-stu-id="93c2d-186">![Choosing the model content](aspnet-mvc-4-models-and-data-access/_static/image7.png "Choosing the model content")</span></span>

    <span data-ttu-id="93c2d-187">*Escolhendo o conteúdo do modelo*</span><span class="sxs-lookup"><span data-stu-id="93c2d-187">*Choosing the model content*</span></span>
3. <span data-ttu-id="93c2d-188">Desde que você estiver gerando um modelo de banco de dados, você precisará especificar a conexão para usar.</span><span class="sxs-lookup"><span data-stu-id="93c2d-188">Since you are generating a model from a database, you will need to specify the connection to use.</span></span> <span data-ttu-id="93c2d-189">Clique em **nova Conexão**.</span><span class="sxs-lookup"><span data-stu-id="93c2d-189">Click **New Connection**.</span></span>
4. <span data-ttu-id="93c2d-190">Selecione **o arquivo de banco de dados do Microsoft SQL Server** e clique em **continuar**.</span><span class="sxs-lookup"><span data-stu-id="93c2d-190">Select **Microsoft SQL Server Database File** and click **Continue**.</span></span>

    <span data-ttu-id="93c2d-191">![Escolher fonte de dados](aspnet-mvc-4-models-and-data-access/_static/image8.png "Escolher fonte de dados")</span><span class="sxs-lookup"><span data-stu-id="93c2d-191">![Choose data source](aspnet-mvc-4-models-and-data-access/_static/image8.png "Choose data source")</span></span>

    <span data-ttu-id="93c2d-192">*Escolha a caixa de diálogo de fonte de dados*</span><span class="sxs-lookup"><span data-stu-id="93c2d-192">*Choose data source dialog*</span></span>
5. <span data-ttu-id="93c2d-193">Clique em **procurar** e selecione o banco de dados **MvcMusicStore.mdf** localizado no **aplicativo\_dados** pasta e clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="93c2d-193">Click **Browse** and select the database **MvcMusicStore.mdf** you located in the **App\_Data** folder and click **OK**.</span></span>

    <span data-ttu-id="93c2d-194">![Propriedades de Conexão](aspnet-mvc-4-models-and-data-access/_static/image9.png "propriedades de Conexão")</span><span class="sxs-lookup"><span data-stu-id="93c2d-194">![Connection properties](aspnet-mvc-4-models-and-data-access/_static/image9.png "Connection properties")</span></span>

    <span data-ttu-id="93c2d-195">*Propriedades de Conexão*</span><span class="sxs-lookup"><span data-stu-id="93c2d-195">*Connection properties*</span></span>
6. <span data-ttu-id="93c2d-196">A classe gerada deve ter o mesmo nome que a cadeia de caracteres de conexão de entidade, portanto, altere seu nome para **MusicStoreEntities** e clique em **próximo**.</span><span class="sxs-lookup"><span data-stu-id="93c2d-196">The generated class should have the same name as the entity connection string, so change its name to **MusicStoreEntities** and click **Next**.</span></span>

    <span data-ttu-id="93c2d-197">![Escolha a conexão de dados](aspnet-mvc-4-models-and-data-access/_static/image10.png "escolhendo a conexão de dados")</span><span class="sxs-lookup"><span data-stu-id="93c2d-197">![Choosing the data connection](aspnet-mvc-4-models-and-data-access/_static/image10.png "Choosing the data connection")</span></span>

    <span data-ttu-id="93c2d-198">*Escolha a conexão de dados*</span><span class="sxs-lookup"><span data-stu-id="93c2d-198">*Choosing the data connection*</span></span>
7. <span data-ttu-id="93c2d-199">Escolha os objetos de banco de dados para usar.</span><span class="sxs-lookup"><span data-stu-id="93c2d-199">Choose the database objects to use.</span></span> <span data-ttu-id="93c2d-200">Como o modelo de entidade usará apenas os tabelas do banco de dados, selecione o **tabelas** opção e certifique-se de que o **incluir colunas de chave estrangeira no modelo** e **Pluralizar ou singularizar gerado nomes de objeto** opções também são selecionadas.</span><span class="sxs-lookup"><span data-stu-id="93c2d-200">As the Entity Model will use just the database's tables, select the **Tables** option, and make sure that the **Include foreign key columns in the model** and **Pluralize or singularize generated object names** options are also selected.</span></span> <span data-ttu-id="93c2d-201">Alterar o Namespace de modelo para **MvcMusicStore.Model** e clique em **concluir**.</span><span class="sxs-lookup"><span data-stu-id="93c2d-201">Change the Model Namespace to **MvcMusicStore.Model** and click **Finish**.</span></span>

    <span data-ttu-id="93c2d-202">![Escolha os objetos de banco de dados](aspnet-mvc-4-models-and-data-access/_static/image11.png "escolhendo os objetos de banco de dados")</span><span class="sxs-lookup"><span data-stu-id="93c2d-202">![Choosing the database objects](aspnet-mvc-4-models-and-data-access/_static/image11.png "Choosing the database objects")</span></span>

    <span data-ttu-id="93c2d-203">*Escolha os objetos de banco de dados*</span><span class="sxs-lookup"><span data-stu-id="93c2d-203">*Choosing the database objects*</span></span>

    > [!NOTE]
    > <span data-ttu-id="93c2d-204">Se uma caixa de diálogo de aviso de segurança for exibida, clique em **Okey** para executar o modelo e gerar as classes para as entidades de modelo.</span><span class="sxs-lookup"><span data-stu-id="93c2d-204">If a Security Warning dialog is shown, click **OK** to run the template and generate the classes for the model entities.</span></span>
8. <span data-ttu-id="93c2d-205">Um diagrama de entidade do banco de dados serão exibidos, enquanto uma classe separada que mapeia cada tabela no banco de dados será criada.</span><span class="sxs-lookup"><span data-stu-id="93c2d-205">An entity diagram for the database will appear, while a separate class that maps each table to the database will be created.</span></span> <span data-ttu-id="93c2d-206">Por exemplo, o **álbuns** tabela será representada por um **álbum** classe, onde cada coluna na tabela será mapeado para uma propriedade de classe.</span><span class="sxs-lookup"><span data-stu-id="93c2d-206">For example, the **Albums** table will be represented by an **Album** class, where each column in the table will map to a class property.</span></span> <span data-ttu-id="93c2d-207">Isso permitirá que você consultar e trabalhar com objetos que representam linhas no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="93c2d-207">This will allow you to query and work with objects that represent rows in the database.</span></span>

    <span data-ttu-id="93c2d-208">![Diagrama de entidade](aspnet-mvc-4-models-and-data-access/_static/image12.png "diagrama de entidade")</span><span class="sxs-lookup"><span data-stu-id="93c2d-208">![Entity diagram](aspnet-mvc-4-models-and-data-access/_static/image12.png "Entity diagram")</span></span>

    <span data-ttu-id="93c2d-209">*Diagrama de entidade*</span><span class="sxs-lookup"><span data-stu-id="93c2d-209">*Entity diagram*</span></span>

    > [!NOTE]
    > <span data-ttu-id="93c2d-210">Os modelos T4. (TT) execute o código para gerar as classes de entidades e substituirão as classes existentes com o mesmo nome.</span><span class="sxs-lookup"><span data-stu-id="93c2d-210">The T4 templates (.tt) run code to generate the entities classes and will overwrite the existing classes with the same name.</span></span> <span data-ttu-id="93c2d-211">Neste exemplo, as classes &quot;álbum&quot;, &quot;gênero&quot; e &quot;artista&quot; foram substituídos com o código gerado.</span><span class="sxs-lookup"><span data-stu-id="93c2d-211">In this example, the classes &quot;Album&quot;, &quot;Genre&quot; and &quot;Artist&quot; were overwritten with the generated code.</span></span>


<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a><span data-ttu-id="93c2d-212">Tarefa 3: criar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="93c2d-212">Task 3 - Building the Application</span></span>

<span data-ttu-id="93c2d-213">Nesta tarefa, você verificará que, embora a geração de modelo removeu o **álbum**, **gênero** e **artista** classes de modelo, o projeto é compilado com êxito usando as novas classes de modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="93c2d-213">In this task, you will check that, although the model generation have removed the **Album**, **Genre** and **Artist** model classes, the project builds successfully by using the new data model classes.</span></span>

1. <span data-ttu-id="93c2d-214">Compile o projeto selecionando o **criar** item de menu e, em seguida, **MvcMusicStore criar**.</span><span class="sxs-lookup"><span data-stu-id="93c2d-214">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="93c2d-215">![Compilar o projeto](aspnet-mvc-4-models-and-data-access/_static/image13.png "compilar o projeto")</span><span class="sxs-lookup"><span data-stu-id="93c2d-215">![Building the project](aspnet-mvc-4-models-and-data-access/_static/image13.png "Building the project")</span></span>

    <span data-ttu-id="93c2d-216">*Criar o projeto*</span><span class="sxs-lookup"><span data-stu-id="93c2d-216">*Building the project*</span></span>
2. <span data-ttu-id="93c2d-217">O projeto foi criado com êxito.</span><span class="sxs-lookup"><span data-stu-id="93c2d-217">The project builds successfully.</span></span> <span data-ttu-id="93c2d-218">Por que ele ainda funciona?</span><span class="sxs-lookup"><span data-stu-id="93c2d-218">Why does it still work?</span></span> <span data-ttu-id="93c2d-219">Ele funciona porque as tabelas de banco de dados têm campos que incluem as propriedades que você estava usando nas classes removidas **álbum** e **gênero**.</span><span class="sxs-lookup"><span data-stu-id="93c2d-219">It works because the database tables have fields that include the properties that you were using in the removed classes **Album** and **Genre**.</span></span>

    <span data-ttu-id="93c2d-220">![Compilações bem-sucedida](aspnet-mvc-4-models-and-data-access/_static/image14.png "compilações foi bem-sucedida")</span><span class="sxs-lookup"><span data-stu-id="93c2d-220">![Builds succeeded](aspnet-mvc-4-models-and-data-access/_static/image14.png "Builds succeeded")</span></span>

    <span data-ttu-id="93c2d-221">*Compilações foi bem-sucedida*</span><span class="sxs-lookup"><span data-stu-id="93c2d-221">*Builds succeeded*</span></span>
3. <span data-ttu-id="93c2d-222">Enquanto o designer exibe as entidades em um formato de diagrama, eles são realmente classes c#.</span><span class="sxs-lookup"><span data-stu-id="93c2d-222">While the designer displays the entities in a diagram format, they are really C# classes.</span></span> <span data-ttu-id="93c2d-223">Expanda o **StoreDB.edmx** nó no Gerenciador de soluções e, em seguida, **StoreDB.tt**, você verá as novas entidades geradas.</span><span class="sxs-lookup"><span data-stu-id="93c2d-223">Expand the **StoreDB.edmx** node in the Solution Explorer and then **StoreDB.tt**, you will see the new generated entities.</span></span>

    <span data-ttu-id="93c2d-224">![Os arquivos gerados](aspnet-mvc-4-models-and-data-access/_static/image15.png "arquivos gerados")</span><span class="sxs-lookup"><span data-stu-id="93c2d-224">![Generated files](aspnet-mvc-4-models-and-data-access/_static/image15.png "Generated files")</span></span>

    <span data-ttu-id="93c2d-225">*Arquivos gerados*</span><span class="sxs-lookup"><span data-stu-id="93c2d-225">*Generated files*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="93c2d-226">Tarefa 4 - consulta o banco de dados</span><span class="sxs-lookup"><span data-stu-id="93c2d-226">Task 4 - Querying the Database</span></span>

<span data-ttu-id="93c2d-227">Nesta tarefa, você atualizará a classe StoreController para que, em vez de usar dados codificados, ele consultará o banco de dados para recuperar as informações.</span><span class="sxs-lookup"><span data-stu-id="93c2d-227">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will query the database to retrieve the information.</span></span>

1. <span data-ttu-id="93c2d-228">Abra **Controllers\StoreController.cs** e adicione o campo a seguir à classe para manter uma instância do **MusicStoreEntities** classe, denominada **storeDB**:</span><span class="sxs-lookup"><span data-stu-id="93c2d-228">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="93c2d-229">(Código de trecho - *modelos e acesso a dados - Ex1 storeDB*)</span><span class="sxs-lookup"><span data-stu-id="93c2d-229">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
~~~
2. <span data-ttu-id="93c2d-230">O **MusicStoreEntities** classe expõe uma propriedade de coleção para cada tabela no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="93c2d-230">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="93c2d-231">Atualização **procurar** método de ação para recuperar um gênero com todos os **álbuns**.</span><span class="sxs-lookup"><span data-stu-id="93c2d-231">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="93c2d-232">(Código de trecho - *modelos e acesso a dados - Ex1 repositório procurar*)</span><span class="sxs-lookup"><span data-stu-id="93c2d-232">(Code Snippet - *Models And Data Access - Ex1 Store Browse*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]
~~~

> [!NOTE]
> <span data-ttu-id="93c2d-233">Você está usando um recurso do .NET chamado **LINQ** (consulta integrada à linguagem) para escrever expressões de consulta fortemente tipada em relação a essas coleções - que executará o código no banco de dados e retornar objetos que você pode programar com relação.</span><span class="sxs-lookup"><span data-stu-id="93c2d-233">You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.</span></span>
> 
> <span data-ttu-id="93c2d-234">Para obter mais informações sobre o LINQ, visite o [site msdn](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="93c2d-234">For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).</span></span>


3. <span data-ttu-id="93c2d-235">Atualização **índice** método de ação para recuperar todos os gêneros.</span><span class="sxs-lookup"><span data-stu-id="93c2d-235">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="93c2d-236">(Código de trecho - *modelos e acesso a dados - índice de repositório Ex1*)</span><span class="sxs-lookup"><span data-stu-id="93c2d-236">(Code Snippet - *Models And Data Access - Ex1 Store Index*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
~~~
4. <span data-ttu-id="93c2d-237">Atualização **índice** método de ação para recuperar todos os gêneros e transformar a coleção para uma lista.</span><span class="sxs-lookup"><span data-stu-id="93c2d-237">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="93c2d-238">(Código de trecho - *modelos e acesso a dados - Ex1 repositório GenreMenu*)</span><span class="sxs-lookup"><span data-stu-id="93c2d-238">(Code Snippet - *Models And Data Access - Ex1 Store GenreMenu*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]
~~~

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="93c2d-239">Tarefa 5: executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="93c2d-239">Task 5 - Running the Application</span></span>

<span data-ttu-id="93c2d-240">Nesta tarefa, você verificará que a página de índice de repositório agora exibirá os gêneros armazenados no banco de dados em vez do aqueles codificados.</span><span class="sxs-lookup"><span data-stu-id="93c2d-240">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="93c2d-241">Não é necessário alterar o modelo de exibição porque o **StoreController** está retornando as mesmas entidades como antes, embora esse tempo os dados virão do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="93c2d-241">There is no need to change the View template because the **StoreController** is returning the same entities as before, although this time the data will come from the database.</span></span>

1. <span data-ttu-id="93c2d-242">Recompile a solução e pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="93c2d-242">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="93c2d-243">O projeto é iniciado na Home page.</span><span class="sxs-lookup"><span data-stu-id="93c2d-243">The project starts in the Home page.</span></span> <span data-ttu-id="93c2d-244">Verificar se o menu de **gêneros** não é mais uma lista codificada, e os dados são recuperados diretamente do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="93c2d-244">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    <span data-ttu-id="93c2d-246">*Navegação gêneros do banco de dados*</span><span class="sxs-lookup"><span data-stu-id="93c2d-246">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="93c2d-247">Agora vá para qualquer gênero e verifique se que o álbuns são preenchidos a partir do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="93c2d-247">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="93c2d-248">![Navegação álbuns do banco de dados](aspnet-mvc-4-models-and-data-access/_static/image17.png "álbuns de navegação do banco de dados")</span><span class="sxs-lookup"><span data-stu-id="93c2d-248">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image17.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="93c2d-249">*Álbuns de navegação do banco de dados*</span><span class="sxs-lookup"><span data-stu-id="93c2d-249">*Browsing Albums from the database*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a><span data-ttu-id="93c2d-250">Exercício 2: Criar um banco de dados usando o código</span><span class="sxs-lookup"><span data-stu-id="93c2d-250">Exercise 2: Creating a Database Using Code First</span></span>

<span data-ttu-id="93c2d-251">Neste exercício, você aprenderá como usar a abordagem de Code First para criar um banco de dados com as tabelas do aplicativo MusicStore e como acessar seus dados.</span><span class="sxs-lookup"><span data-stu-id="93c2d-251">In this exercise, you will learn how to use the Code First approach to create a database with the tables of the MusicStore application, and how to access its data.</span></span>

<span data-ttu-id="93c2d-252">Depois que o modelo é gerado, você modificará o StoreController para fornecer o modelo de exibição com os dados obtidos do banco de dados, em vez de usar valores embutidos em código.</span><span class="sxs-lookup"><span data-stu-id="93c2d-252">Once the model is generated, you will modify the StoreController to provide the View template with the data taken from the database, instead of using hardcoded values.</span></span>

> [!NOTE]
> <span data-ttu-id="93c2d-253">Se você concluiu o exercício 1 e já tiver trabalhado com a banco de dados primeira abordagem, agora você aprenderá como obter os mesmos resultados com um processo diferente.</span><span class="sxs-lookup"><span data-stu-id="93c2d-253">If you have completed Exercise 1 and have already worked with the Database First approach, you will now learn how to get the same results with a different process.</span></span> <span data-ttu-id="93c2d-254">As tarefas que estão em comum com o exercício 1 foram marcadas para facilitar sua leitura.</span><span class="sxs-lookup"><span data-stu-id="93c2d-254">The tasks that are in common with Exercise 1 have been marked to make your reading easier.</span></span> <span data-ttu-id="93c2d-255">Se você não concluiu o exercício 1, mas gostaria de saber a abordagem de Code First, você pode iniciar este exercício e obter uma cobertura completa do tópico.</span><span class="sxs-lookup"><span data-stu-id="93c2d-255">If you have not completed Exercise 1 but would like to learn the Code First approach, you can start from this exercise and get a full coverage of the topic.</span></span>


<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a><span data-ttu-id="93c2d-256">Tarefa 1 - popular os dados de exemplo</span><span class="sxs-lookup"><span data-stu-id="93c2d-256">Task 1 - Populating Sample Data</span></span>

<span data-ttu-id="93c2d-257">Nesta tarefa, você preencherá o banco de dados com dados de exemplo, quando ele é criado inicialmente usando Code First.</span><span class="sxs-lookup"><span data-stu-id="93c2d-257">In this task, you will populate the database with sample data when it is intially created using Code-First.</span></span>

1. <span data-ttu-id="93c2d-258">Abra o **começar** solução localizado em **fonte/o Ex2-CreatingADatabaseCodeFirst/Begin/** pasta.</span><span class="sxs-lookup"><span data-stu-id="93c2d-258">Open the **Begin** solution located at **Source/Ex2-CreatingADatabaseCodeFirst/Begin/** folder.</span></span> <span data-ttu-id="93c2d-259">Caso contrário, você pode continuar usando o **final** solução obtido executando o exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="93c2d-259">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="93c2d-260">Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="93c2d-260">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="93c2d-261">Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="93c2d-261">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="93c2d-262">No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="93c2d-262">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="93c2d-263">Por fim, compile a solução clicando **criar** | **compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="93c2d-263">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="93c2d-264">Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="93c2d-264">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="93c2d-265">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="93c2d-265">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="93c2d-266">É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.</span><span class="sxs-lookup"><span data-stu-id="93c2d-266">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="93c2d-267">Adicionar o **SampleData.cs** o arquivo para o **modelos** pasta.</span><span class="sxs-lookup"><span data-stu-id="93c2d-267">Add the **SampleData.cs** file to the **Models** folder.</span></span> <span data-ttu-id="93c2d-268">Para fazer isso, clique com botão direito **modelos** pasta, aponte para **adicionar** e, em seguida, clique em **Item existente**.</span><span class="sxs-lookup"><span data-stu-id="93c2d-268">To do that, right-click **Models** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="93c2d-269">Navegue até **\Source\Assets** e selecione o **SampleData.cs** arquivo.</span><span class="sxs-lookup"><span data-stu-id="93c2d-269">Browse to **\Source\Assets** and select the **SampleData.cs** file.</span></span>

    <span data-ttu-id="93c2d-270">![Dados de exemplo popular código](aspnet-mvc-4-models-and-data-access/_static/image18.png "código de popular de dados de exemplo")</span><span class="sxs-lookup"><span data-stu-id="93c2d-270">![Sample data populate code](aspnet-mvc-4-models-and-data-access/_static/image18.png "Sample data populate code")</span></span>

    <span data-ttu-id="93c2d-271">*Código de popular de dados de exemplo*</span><span class="sxs-lookup"><span data-stu-id="93c2d-271">*Sample data populate code*</span></span>
3. <span data-ttu-id="93c2d-272">Abra o **Global.asax.cs** de arquivo e adicione o seguinte *usando* instruções.</span><span class="sxs-lookup"><span data-stu-id="93c2d-272">Open the **Global.asax.cs** file and add the following *using* statements.</span></span>

    <span data-ttu-id="93c2d-273">(Código de trecho - *modelos e acesso a dados - o Ex2 Global Asax usos*)</span><span class="sxs-lookup"><span data-stu-id="93c2d-273">(Code Snippet - *Models And Data Access - Ex2 Global Asax Usings*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
~~~
4. <span data-ttu-id="93c2d-274">No **aplicativo\_Start ()** método adicionar a linha a seguir para definir o inicializador de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="93c2d-274">In the **Application\_Start()** method add the following line to set the database initializer.</span></span>

    <span data-ttu-id="93c2d-275">(Código de trecho - *modelos e acesso a dados - o Ex2 Global Asax SetInitializer*)</span><span class="sxs-lookup"><span data-stu-id="93c2d-275">(Code Snippet - *Models And Data Access - Ex2 Global Asax SetInitializer*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]
~~~

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a><span data-ttu-id="93c2d-276">Tarefa 2: configurar a conexão ao banco de dados</span><span class="sxs-lookup"><span data-stu-id="93c2d-276">Task 2 - Configuring the connection to the Database</span></span>

<span data-ttu-id="93c2d-277">Agora que você já adicionou um banco de dados ao nosso projeto, você escreverá **Web. config** a cadeia de caracteres de conexão do arquivo.</span><span class="sxs-lookup"><span data-stu-id="93c2d-277">Now that you have already added a database to our project, you will write in the **Web.config** file the connection string.</span></span>

1. <span data-ttu-id="93c2d-278">Adicionar uma cadeia de caracteres de conexão em **Web. config**. Para fazer isso, abra **Web. config** na raiz do projeto e substituir a cadeia de caracteres de conexão chamado DefaultConnection com esta linha de **&lt;connectionStrings&gt;** seção:</span><span class="sxs-lookup"><span data-stu-id="93c2d-278">Add a connection string at **Web.config**. To do that, open **Web.config** at project root and replace the connection string named DefaultConnection with this line in the **&lt;connectionStrings&gt;** section:</span></span>

    <span data-ttu-id="93c2d-279">![Local do arquivo Web. config](aspnet-mvc-4-models-and-data-access/_static/image19.png "local do arquivo Web. config")</span><span class="sxs-lookup"><span data-stu-id="93c2d-279">![Web.config file location](aspnet-mvc-4-models-and-data-access/_static/image19.png "Web.config file location")</span></span>

    <span data-ttu-id="93c2d-280">*local do arquivo Web. config*</span><span class="sxs-lookup"><span data-stu-id="93c2d-280">*Web.config file location*</span></span>


~~~
[!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]
~~~

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a><span data-ttu-id="93c2d-281">Tarefa 3: Trabalhando com o modelo</span><span class="sxs-lookup"><span data-stu-id="93c2d-281">Task 3 - Working with the Model</span></span>

<span data-ttu-id="93c2d-282">Agora que você já tiver configurado a conexão ao banco de dados, você vinculará o modelo com as tabelas de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="93c2d-282">Now that you have already configured the connection to the database, you will link the model with the database tables.</span></span> <span data-ttu-id="93c2d-283">Nesta tarefa, você criará uma classe que será vinculada ao banco de dados com o Code First.</span><span class="sxs-lookup"><span data-stu-id="93c2d-283">In this task, you will create a class that will be linked to the database with Code First.</span></span> <span data-ttu-id="93c2d-284">Lembre-se de que há uma classe de modelo existente POCO que deve ser modificada.</span><span class="sxs-lookup"><span data-stu-id="93c2d-284">Remember that there is an existent POCO model class that should be modified.</span></span>

   > [!NOTE]
> <span data-ttu-id="93c2d-285">Se você concluiu o exercício 1, você observará que esta etapa foi executada por um assistente.</span><span class="sxs-lookup"><span data-stu-id="93c2d-285">If you completed Exercise 1, you will note that this step was performed by a wizard.</span></span> <span data-ttu-id="93c2d-286">Fazendo Code First, você criará manualmente classes que serão vinculados entidades de dados.</span><span class="sxs-lookup"><span data-stu-id="93c2d-286">By doing Code First, you will manually create classes that will be linked to data entities.</span></span>


1. <span data-ttu-id="93c2d-287">Abra a classe de modelo POCO **gênero** de **modelos** pasta do projeto e incluir uma ID.</span><span class="sxs-lookup"><span data-stu-id="93c2d-287">Open the POCO model class **Genre** from **Models** project folder and include an ID.</span></span> <span data-ttu-id="93c2d-288">Use uma int de propriedade com o nome **GenreId**.</span><span class="sxs-lookup"><span data-stu-id="93c2d-288">Use an int property with the name **GenreId**.</span></span>

    <span data-ttu-id="93c2d-289">(Código de trecho - *modelos e acesso a dados - o Ex2 código primeiro gênero*)</span><span class="sxs-lookup"><span data-stu-id="93c2d-289">(Code Snippet - *Models And Data Access - Ex2 Code First Genre*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

> [!NOTE]
> To work with Code First conventions, the class Genre must have a primary key property that will be automatically detected.
> 
> You can read more about Code First Conventions in this [msdn article](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).
~~~
2. <span data-ttu-id="93c2d-290">Agora, abra a classe de modelo POCO **álbum** de **modelos** pasta do projeto e inclui as chaves estrangeiras, crie propriedades com os nomes **GenreId** e  **ArtistId**.</span><span class="sxs-lookup"><span data-stu-id="93c2d-290">Now, open the POCO model class **Album** from **Models** project folder and include the foreign keys, create properties with the names **GenreId** and **ArtistId**.</span></span> <span data-ttu-id="93c2d-291">Essa classe já tiver o **GenreId** para a chave primária.</span><span class="sxs-lookup"><span data-stu-id="93c2d-291">This class already have the **GenreId** for the primary key.</span></span>

    <span data-ttu-id="93c2d-292">(Código de trecho - *modelos e acesso a dados - o Ex2 código primeiro álbum*)</span><span class="sxs-lookup"><span data-stu-id="93c2d-292">(Code Snippet - *Models And Data Access - Ex2 Code First Album*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
~~~
3. <span data-ttu-id="93c2d-293">Abra a classe de modelo POCO **artista** e incluir o **ArtistId** propriedade.</span><span class="sxs-lookup"><span data-stu-id="93c2d-293">Open the POCO model class **Artist** and include the **ArtistId** property.</span></span>

    <span data-ttu-id="93c2d-294">(Código de trecho - *modelos e acesso a dados - o Ex2 código primeiro artista*)</span><span class="sxs-lookup"><span data-stu-id="93c2d-294">(Code Snippet - *Models And Data Access - Ex2 Code First Artist*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
~~~
4. <span data-ttu-id="93c2d-295">Clique com botão direito do **modelos** pasta do projeto e selecione **adicionar | Classe**.</span><span class="sxs-lookup"><span data-stu-id="93c2d-295">Right-click the **Models** project folder and select **Add | Class**.</span></span> <span data-ttu-id="93c2d-296">Nomeie o arquivo **MusicStoreEntities.cs**.</span><span class="sxs-lookup"><span data-stu-id="93c2d-296">Name the file **MusicStoreEntities.cs**.</span></span> <span data-ttu-id="93c2d-297">Em seguida, clique em **adicionar.**</span><span class="sxs-lookup"><span data-stu-id="93c2d-297">Then, click **Add.**</span></span>

    <span data-ttu-id="93c2d-298">![Adicionando uma classe](aspnet-mvc-4-models-and-data-access/_static/image20.png "adicionando uma classe")</span><span class="sxs-lookup"><span data-stu-id="93c2d-298">![Adding a class](aspnet-mvc-4-models-and-data-access/_static/image20.png "Adding a class")</span></span>

    <span data-ttu-id="93c2d-299">*Adicionar um novo item*</span><span class="sxs-lookup"><span data-stu-id="93c2d-299">*Adding a new item*</span></span>

    <span data-ttu-id="93c2d-300">![Adicionando um class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "adicionando um class2")</span><span class="sxs-lookup"><span data-stu-id="93c2d-300">![Adding a class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "Adding a class2")</span></span>

    <span data-ttu-id="93c2d-301">*Adicionando uma classe*</span><span class="sxs-lookup"><span data-stu-id="93c2d-301">*Adding a class*</span></span>
5. <span data-ttu-id="93c2d-302">Abra a classe que você acabou de criar, **MusicStoreEntities.cs**e incluem os namespaces **System.Data.Entity** e **System.Data.Entity.Infrastructure**.</span><span class="sxs-lookup"><span data-stu-id="93c2d-302">Open the class you have just created, **MusicStoreEntities.cs**, and include the namespaces **System.Data.Entity** and **System.Data.Entity.Infrastructure**.</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
~~~
6. <span data-ttu-id="93c2d-303">Substitua a declaração de classe para estender o **DbContext** classe: declarar um público **DBSet** e substituir **OnModelCreating** método.</span><span class="sxs-lookup"><span data-stu-id="93c2d-303">Replace the class declaration to extend the **DbContext** class: declare a public **DBSet** and override **OnModelCreating** method.</span></span> <span data-ttu-id="93c2d-304">Após essa etapa, você obterá uma classe de domínio que será vinculado a seu modelo com o Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="93c2d-304">After this step you will get a domain class that will link your model with the Entity Framework.</span></span> <span data-ttu-id="93c2d-305">Para fazer isso, substitua o código de classe com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="93c2d-305">In order to do that, replace the class code with the following:</span></span>

    <span data-ttu-id="93c2d-306">(Código de trecho - *modelos e acesso a dados - o Ex2 código primeiro MusicStoreEntities*)</span><span class="sxs-lookup"><span data-stu-id="93c2d-306">(Code Snippet - *Models And Data Access - Ex2 Code First MusicStoreEntities*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

> [!NOTE]
> With Entity Framework **DbContext** and **DBSet** you will be able to query the POCO class Genre. By extending **OnModelCreating** method, you are specifying in the **code** how Genre will be mapped to a database table. You can find more information about DBContext and DBSet in this msdn article: [link](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)
~~~

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="93c2d-307">Tarefa 4 - consulta o banco de dados</span><span class="sxs-lookup"><span data-stu-id="93c2d-307">Task 4 - Querying the Database</span></span>

<span data-ttu-id="93c2d-308">Nesta tarefa, você atualizará a classe StoreController para que, em vez de usar dados codificados, ele irá recuperá-lo do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="93c2d-308">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will retrieve it from the database.</span></span>

> [!NOTE]
> <span data-ttu-id="93c2d-309">Esta tarefa está em comum com o exercício 1.</span><span class="sxs-lookup"><span data-stu-id="93c2d-309">This task is in common with Exercise 1.</span></span>
> 
> <span data-ttu-id="93c2d-310">Se você concluiu o exercício 1, você observará que essas etapas são as mesmas em ambas as abordagens (banco de dados pela primeira vez ou código primeiro).</span><span class="sxs-lookup"><span data-stu-id="93c2d-310">If you completed Exercise 1 you will note these steps are the same in both approaches (Database first or Code first).</span></span> <span data-ttu-id="93c2d-311">São diferentes em como os dados estão vinculados com o modelo, mas o acesso a entidades de dados ainda é transparente do controlador.</span><span class="sxs-lookup"><span data-stu-id="93c2d-311">They are different in how the data is linked with the model, but the access to data entities is yet transparent from the controller.</span></span>


1. <span data-ttu-id="93c2d-312">Abra **Controllers\StoreController.cs** e adicione o campo a seguir à classe para manter uma instância do **MusicStoreEntities** classe, denominada **storeDB**:</span><span class="sxs-lookup"><span data-stu-id="93c2d-312">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="93c2d-313">(Código de trecho - *modelos e acesso a dados - Ex1 storeDB*)</span><span class="sxs-lookup"><span data-stu-id="93c2d-313">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
~~~
2. <span data-ttu-id="93c2d-314">O **MusicStoreEntities** classe expõe uma propriedade de coleção para cada tabela no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="93c2d-314">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="93c2d-315">Atualização **procurar** método de ação para recuperar um gênero com todos os **álbuns**.</span><span class="sxs-lookup"><span data-stu-id="93c2d-315">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="93c2d-316">(Código de trecho - *modelos e acesso a dados - o Ex2 repositório procurar*)</span><span class="sxs-lookup"><span data-stu-id="93c2d-316">(Code Snippet - *Models And Data Access - Ex2 Store Browse*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

> [!NOTE]
> You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.
> 
> For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).
~~~
3. <span data-ttu-id="93c2d-317">Atualização **índice** método de ação para recuperar todos os gêneros.</span><span class="sxs-lookup"><span data-stu-id="93c2d-317">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="93c2d-318">(Código de trecho - *modelos e acesso a dados - índice de repositório o Ex2*)</span><span class="sxs-lookup"><span data-stu-id="93c2d-318">(Code Snippet - *Models And Data Access - Ex2 Store Index*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
~~~
4. <span data-ttu-id="93c2d-319">Atualização **índice** método de ação para recuperar todos os gêneros e transformar a coleção para uma lista.</span><span class="sxs-lookup"><span data-stu-id="93c2d-319">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="93c2d-320">(Código de trecho - *modelos e acesso a dados - o Ex2 repositório GenreMenu*)</span><span class="sxs-lookup"><span data-stu-id="93c2d-320">(Code Snippet - *Models And Data Access - Ex2 Store GenreMenu*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]
~~~

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="93c2d-321">Tarefa 5: executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="93c2d-321">Task 5 - Running the Application</span></span>

<span data-ttu-id="93c2d-322">Nesta tarefa, você verificará que a página de índice de repositório agora exibirá os gêneros armazenados no banco de dados em vez do aqueles codificados.</span><span class="sxs-lookup"><span data-stu-id="93c2d-322">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="93c2d-323">Não é necessário alterar o modelo de exibição porque o **StoreController** está retornando o mesmo **StoreIndexViewModel** como antes, mas desta vez os dados virão do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="93c2d-323">There is no need to change the View template because the **StoreController** is returning the same **StoreIndexViewModel** as before, but this time the data will come from the database.</span></span>

1. <span data-ttu-id="93c2d-324">Recompile a solução e pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="93c2d-324">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="93c2d-325">O projeto é iniciado na Home page.</span><span class="sxs-lookup"><span data-stu-id="93c2d-325">The project starts in the Home page.</span></span> <span data-ttu-id="93c2d-326">Verificar se o menu de **gêneros** não é mais uma lista codificada, e os dados são recuperados diretamente do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="93c2d-326">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    <span data-ttu-id="93c2d-328">*Navegação gêneros do banco de dados*</span><span class="sxs-lookup"><span data-stu-id="93c2d-328">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="93c2d-329">Agora vá para qualquer gênero e verifique se que o álbuns são preenchidos a partir do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="93c2d-329">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="93c2d-330">![Navegação álbuns do banco de dados](aspnet-mvc-4-models-and-data-access/_static/image23.png "álbuns de navegação do banco de dados")</span><span class="sxs-lookup"><span data-stu-id="93c2d-330">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image23.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="93c2d-331">*Álbuns de navegação do banco de dados*</span><span class="sxs-lookup"><span data-stu-id="93c2d-331">*Browsing Albums from the database*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a><span data-ttu-id="93c2d-332">Exercício 3: Consultando o banco de dados com parâmetros</span><span class="sxs-lookup"><span data-stu-id="93c2d-332">Exercise 3: Querying the Database with Parameters</span></span>

<span data-ttu-id="93c2d-333">Neste exercício, você aprenderá como o banco de dados usando parâmetros de consulta e como usar a formatação de resultados de consulta, um recurso que reduz o banco de dados de número acessa a recuperação de dados de forma mais eficiente.</span><span class="sxs-lookup"><span data-stu-id="93c2d-333">In this exercise, you will learn how to query the database using parameters, and how to use Query Result Shaping, a feature that reduces the number database accesses retrieving data in a more efficient way.</span></span>

> [!NOTE]
> <span data-ttu-id="93c2d-334">Para obter mais informações sobre formatação de resultados de consulta, visite o [artigo do msdn](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="93c2d-334">For further information on Query Result Shaping, visit the following [msdn article](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).</span></span>


<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a><span data-ttu-id="93c2d-335">Tarefa 1 - StoreController modificação para recuperar álbuns de banco de dados</span><span class="sxs-lookup"><span data-stu-id="93c2d-335">Task 1 - Modifying StoreController to Retrieve Albums from Database</span></span>

<span data-ttu-id="93c2d-336">Nesta tarefa, você alterará a **StoreController** classe para acessar o banco de dados para recuperar álbuns de um gênero específico.</span><span class="sxs-lookup"><span data-stu-id="93c2d-336">In this task, you will change the **StoreController** class to access the database to retrieve albums from a specific genre.</span></span>

1. <span data-ttu-id="93c2d-337">Abra o **começar** solução localizado no **Source\Ex3 QueryingTheDatabaseWithParametersCodeFirst\Begin** pasta se você quiser usar a abordagem de código ou **Source\ Ex3 QueryingTheDatabaseWithParametersDBFirst\Begin** pasta se você quiser usar a abordagem de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="93c2d-337">Open the **Begin** solution located at the **Source\Ex3-QueryingTheDatabaseWithParametersCodeFirst\Begin** folder if you want to use Code-First approach or **Source\Ex3-QueryingTheDatabaseWithParametersDBFirst\Begin** folder if you want to use Database-First approach.</span></span> <span data-ttu-id="93c2d-338">Caso contrário, você pode continuar usando o **final** solução obtido executando o exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="93c2d-338">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="93c2d-339">Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="93c2d-339">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="93c2d-340">Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="93c2d-340">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="93c2d-341">No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="93c2d-341">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="93c2d-342">Por fim, compile a solução clicando **criar** | **compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="93c2d-342">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="93c2d-343">Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="93c2d-343">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="93c2d-344">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="93c2d-344">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="93c2d-345">É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.</span><span class="sxs-lookup"><span data-stu-id="93c2d-345">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="93c2d-346">Abra o **StoreController** classe para alterar o **procurar** método de ação.</span><span class="sxs-lookup"><span data-stu-id="93c2d-346">Open the **StoreController** class to change the **Browse** action method.</span></span> <span data-ttu-id="93c2d-347">Para fazer isso, no **Solution Explorer**, expanda o **controladores** pasta e clique duas vezes em **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="93c2d-347">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
3. <span data-ttu-id="93c2d-348">Alterar o **procurar** método de ação para recuperar álbuns de um gênero específico.</span><span class="sxs-lookup"><span data-stu-id="93c2d-348">Change the **Browse** action method to retrieve albums for a specific genre.</span></span> <span data-ttu-id="93c2d-349">Para fazer isso, substitua o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="93c2d-349">To do this, replace the following code:</span></span>

    <span data-ttu-id="93c2d-350">(Código de trecho - *modelos e acesso a dados - Ex3 StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="93c2d-350">(Code Snippet - *Models And Data Access - Ex3 StoreController BrowseMethod*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

> [!NOTE]
> To populate a collection of the entity, you need to use the **Include** method to specify you want to retrieve the albums too. You can use the .**Single()** extension in LINQ because in this case only one genre is expected for an album. The **Single()** method takes a Lambda expression as a parameter, which in this case specifies a single Genre object such that its name matches the value defined.
> 
> You will take advantage of a feature that allows you to indicate other related entities you want loaded as well when the Genre object is retrieved. This feature is called **Query Result Shaping**, and enables you to reduce the number of times needed to access the database to retrieve information. In this scenario, you will want to pre-fetch the Albums for the Genre you retrieve.
> 
> The query includes **Genres.Include(&quot;Albums&quot;)** to indicate that you want related albums as well. This will result in a more efficient application, since it will retrieve both Genre and Album data in a single database request.
~~~

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="93c2d-351">Tarefa 2: executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="93c2d-351">Task 2 - Running the Application</span></span>

<span data-ttu-id="93c2d-352">Nesta tarefa, você executar o aplicativo e recuperará álbuns de um gênero específico do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="93c2d-352">In this task, you will run the application and retrieve albums of a specific genre from the database.</span></span>

1. <span data-ttu-id="93c2d-353">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="93c2d-353">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="93c2d-354">O projeto é iniciado na Home page.</span><span class="sxs-lookup"><span data-stu-id="93c2d-354">The project starts in the Home page.</span></span> <span data-ttu-id="93c2d-355">Altere a URL para **repositório/procurar? gênero = Pop** para verificar se os resultados são recuperados do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="93c2d-355">Change the URL to **/Store/Browse?genre=Pop** to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="93c2d-356">![Procurando por gênero](aspnet-mvc-4-models-and-data-access/_static/image24.png "navegação por gênero")</span><span class="sxs-lookup"><span data-stu-id="93c2d-356">![Browsing by genre](aspnet-mvc-4-models-and-data-access/_static/image24.png "Browsing by genre")</span></span>

    <span data-ttu-id="93c2d-357">*Pesquisa/repositório/procurar? gênero = Pop*</span><span class="sxs-lookup"><span data-stu-id="93c2d-357">*Browsing /Store/Browse?genre=Pop*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a><span data-ttu-id="93c2d-358">Tarefa 3: acessando álbuns por Id</span><span class="sxs-lookup"><span data-stu-id="93c2d-358">Task 3 - Accessing Albums by Id</span></span>

<span data-ttu-id="93c2d-359">Nesta tarefa, repita o procedimento anterior para obter álbuns por seu ID.</span><span class="sxs-lookup"><span data-stu-id="93c2d-359">In this task, you will repeat the previous procedure to get albums by their Id.</span></span>

1. <span data-ttu-id="93c2d-360">Feche o navegador se necessário, para retornar ao Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="93c2d-360">Close the browser if needed, to return to Visual Studio.</span></span> <span data-ttu-id="93c2d-361">Abra o **StoreController** classe para alterar o **detalhes** método de ação.</span><span class="sxs-lookup"><span data-stu-id="93c2d-361">Open the **StoreController** class to change the **Details** action method.</span></span> <span data-ttu-id="93c2d-362">Para fazer isso, no **Solution Explorer**, expanda o **controladores** pasta e clique duas vezes em **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="93c2d-362">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
2. <span data-ttu-id="93c2d-363">Alterar o **detalhes** método de ação ao recuperar detalhes de álbuns com base em seus **Id**. Para fazer isso, substitua o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="93c2d-363">Change the **Details** action method to retrieve albums details based on their **Id**. To do this, replace the following code:</span></span>

    <span data-ttu-id="93c2d-364">(Código de trecho - *modelos e acesso a dados - Ex3 StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="93c2d-364">(Code Snippet - *Models And Data Access - Ex3 StoreController DetailsMethod*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]
~~~

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="93c2d-365">Tarefa 4 - executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="93c2d-365">Task 4 - Running the Application</span></span>

<span data-ttu-id="93c2d-366">Nesta tarefa, você executar o aplicativo em um navegador da web e obter detalhes sobre o álbum por seu ID.</span><span class="sxs-lookup"><span data-stu-id="93c2d-366">In this task, you will run the Application in a web browser and obtain album details by their Id.</span></span>

1. <span data-ttu-id="93c2d-367">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="93c2d-367">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="93c2d-368">O projeto é iniciado na Home page.</span><span class="sxs-lookup"><span data-stu-id="93c2d-368">The project starts in the Home page.</span></span> <span data-ttu-id="93c2d-369">Altere a URL para **/Store/Details/51** ou procurar os gêneros e selecione um álbum para verificar se os resultados são recuperados do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="93c2d-369">Change the URL to **/Store/Details/51** or browse the genres and select an album to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="93c2d-370">![Procurando detalhes](aspnet-mvc-4-models-and-data-access/_static/image25.png "procurando detalhes")</span><span class="sxs-lookup"><span data-stu-id="93c2d-370">![Browsing Details](aspnet-mvc-4-models-and-data-access/_static/image25.png "Browsing Details")</span></span>

    <span data-ttu-id="93c2d-371">*Navegação /Store/Details/51*</span><span class="sxs-lookup"><span data-stu-id="93c2d-371">*Browsing /Store/Details/51*</span></span>

> [!NOTE]
> <span data-ttu-id="93c2d-372">Além disso, você pode implantar esse aplicativo para Windows Azure Web Sites a seguir [apêndice b: publicação um aplicativo ASP.NET MVC 4 usando a implantação da Web](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="93c2d-372">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="93c2d-373">Resumo</span><span class="sxs-lookup"><span data-stu-id="93c2d-373">Summary</span></span>

<span data-ttu-id="93c2d-374">Por concluir este laboratório prático você aprendeu os fundamentos do acesso a dados e modelos do ASP.NET MVC, usando o **Database First** abordagem, bem como a **Code First** abordagem:</span><span class="sxs-lookup"><span data-stu-id="93c2d-374">By completing this Hands-on Lab you have learned the fundamentals of ASP.NET MVC Models and Data Access, using the **Database First** approach as well as the **Code First** Approach:</span></span>

- <span data-ttu-id="93c2d-375">Como adicionar um banco de dados para a solução para consumir seus dados</span><span class="sxs-lookup"><span data-stu-id="93c2d-375">How to add a database to the solution in order to consume its data</span></span>
- <span data-ttu-id="93c2d-376">Como atualizar os controladores para fornecer exibir modelos com os dados obtidos do banco de dados em vez de um embutido</span><span class="sxs-lookup"><span data-stu-id="93c2d-376">How to update Controllers to provide View templates with the data taken from the database instead of hard-coded one</span></span>
- <span data-ttu-id="93c2d-377">Como consultar o banco de dados usando parâmetros</span><span class="sxs-lookup"><span data-stu-id="93c2d-377">How to query the database using parameters</span></span>
- <span data-ttu-id="93c2d-378">Como usar a consulta resultado modelagem, um recurso que reduz o número de acessos de banco de dados, recuperação de dados de forma mais eficiente</span><span class="sxs-lookup"><span data-stu-id="93c2d-378">How to use the Query Result Shaping, a feature that reduces the number of database accesses, retrieving data in a more efficient way</span></span>
- <span data-ttu-id="93c2d-379">Como usar abordagens Database First e Code First no Entity Framework da Microsoft para vincular o banco de dados com o modelo</span><span class="sxs-lookup"><span data-stu-id="93c2d-379">How to use both Database First and Code First approaches in Microsoft Entity Framework to link the database with the model</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="93c2d-380">Apêndice a: instalar o Visual Studio Express 2012 para Web</span><span class="sxs-lookup"><span data-stu-id="93c2d-380">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="93c2d-381">Você pode instalar **Microsoft Visual Studio Express 2012 para Web** ou outro &quot;Express&quot; versão usando o **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="93c2d-381">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="93c2d-382">As instruções a seguir guiá-lo pelas etapas necessárias para instalar o *Visual studio Express 2012 para Web* usando *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="93c2d-382">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="93c2d-383">Vá para [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="93c2d-383">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="93c2d-384">Como alternativa, se você já tiver instalado o Web Platform Installer, você pode abri-la e procure o produto &quot; <em>Visual Studio Express 2012 para Web com o SDK do Windows Azure</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="93c2d-384">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="93c2d-385">Clique em **instalar agora**.</span><span class="sxs-lookup"><span data-stu-id="93c2d-385">Click on **Install Now**.</span></span> <span data-ttu-id="93c2d-386">Se você não tem **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.</span><span class="sxs-lookup"><span data-stu-id="93c2d-386">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="93c2d-387">Uma vez **Web Platform Installer** é aberto, clique em **instalar** para iniciar a instalação.</span><span class="sxs-lookup"><span data-stu-id="93c2d-387">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="93c2d-388">![Instalar Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "instalar Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="93c2d-388">![Install Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="93c2d-389">*Instalar Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="93c2d-389">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="93c2d-390">Leia os termos e licenças de todos os produtos e clique em **aceito** para continuar.</span><span class="sxs-lookup"><span data-stu-id="93c2d-390">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Aceitar os termos de licença](aspnet-mvc-4-models-and-data-access/_static/image27.png)

    <span data-ttu-id="93c2d-392">*Aceitar os termos de licença*</span><span class="sxs-lookup"><span data-stu-id="93c2d-392">*Accepting the license terms*</span></span>
5. <span data-ttu-id="93c2d-393">Aguarde até que o processo de download e instalação seja concluída.</span><span class="sxs-lookup"><span data-stu-id="93c2d-393">Wait until the downloading and installation process completes.</span></span>

    ![Progresso da instalação](aspnet-mvc-4-models-and-data-access/_static/image28.png)

    <span data-ttu-id="93c2d-395">*Progresso da instalação*</span><span class="sxs-lookup"><span data-stu-id="93c2d-395">*Installation progress*</span></span>
6. <span data-ttu-id="93c2d-396">Quando a instalação for concluída, clique em **concluir**.</span><span class="sxs-lookup"><span data-stu-id="93c2d-396">When the installation completes, click **Finish**.</span></span>

    ![Instalação concluída](aspnet-mvc-4-models-and-data-access/_static/image29.png)

    <span data-ttu-id="93c2d-398">*Instalação concluída*</span><span class="sxs-lookup"><span data-stu-id="93c2d-398">*Installation completed*</span></span>
7. <span data-ttu-id="93c2d-399">Clique em **saída** para fechar o Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="93c2d-399">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="93c2d-400">Para abrir o Visual Studio Express para Web, vá para o **iniciar** tela e comece a escrever &quot; **VS Express**&quot;, em seguida, clique no **VS Express para Web** lado a lado.</span><span class="sxs-lookup"><span data-stu-id="93c2d-400">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express para o bloco de Web](aspnet-mvc-4-models-and-data-access/_static/image30.png)

    <span data-ttu-id="93c2d-402">*VS Express para o bloco de Web*</span><span class="sxs-lookup"><span data-stu-id="93c2d-402">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="93c2d-403">Apêndice b: publicar um aplicativo ASP.NET MVC 4 usando a implantação da Web</span><span class="sxs-lookup"><span data-stu-id="93c2d-403">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="93c2d-404">Este apêndice mostram como criar um novo site do Portal de gerenciamento do Windows Azure e publicar o aplicativo que você obteve seguindo o laboratório, aproveitando o recurso de publicação de implantação da Web fornecido pelo Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="93c2d-404">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="93c2d-405">Tarefa 1: criar um novo Site do Windows Azure Portal</span><span class="sxs-lookup"><span data-stu-id="93c2d-405">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="93c2d-406">Vá para o [Portal de gerenciamento do Windows Azure](https://manage.windowsazure.com/) e entre usando as credenciais da Microsoft associadas à sua assinatura.</span><span class="sxs-lookup"><span data-stu-id="93c2d-406">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="93c2d-407">Com o Windows Azure, você pode hospedar 10 Sites da Web ASP.NET gratuitamente e, em seguida, dimensione conforme seu tráfego cresce.</span><span class="sxs-lookup"><span data-stu-id="93c2d-407">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="93c2d-408">Você pode inscrever [aqui](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="93c2d-408">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="93c2d-409">![Faça logon no portal do Windows Azure](aspnet-mvc-4-models-and-data-access/_static/image31.png "faça logon no portal do Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="93c2d-409">![Log on to Windows Azure portal](aspnet-mvc-4-models-and-data-access/_static/image31.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="93c2d-410">*Faça logon no Portal de gerenciamento do Azure do Windows*</span><span class="sxs-lookup"><span data-stu-id="93c2d-410">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="93c2d-411">Clique em **novo** na barra de comandos.</span><span class="sxs-lookup"><span data-stu-id="93c2d-411">Click **New** on the command bar.</span></span>

    <span data-ttu-id="93c2d-412">![Criando um novo Site](aspnet-mvc-4-models-and-data-access/_static/image32.png "criar um novo Site")</span><span class="sxs-lookup"><span data-stu-id="93c2d-412">![Creating a new Web Site](aspnet-mvc-4-models-and-data-access/_static/image32.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="93c2d-413">*Criando um novo Site*</span><span class="sxs-lookup"><span data-stu-id="93c2d-413">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="93c2d-414">Clique em **de computação** | **Site da Web**.</span><span class="sxs-lookup"><span data-stu-id="93c2d-414">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="93c2d-415">Em seguida, selecione **criação rápida** opção.</span><span class="sxs-lookup"><span data-stu-id="93c2d-415">Then select **Quick Create** option.</span></span> <span data-ttu-id="93c2d-416">Forneça uma URL disponível para o novo site e clique em **Criar Site**.</span><span class="sxs-lookup"><span data-stu-id="93c2d-416">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="93c2d-417">Um Site do Windows Azure é o host para um aplicativo web em execução na nuvem que você pode controlar e gerenciar.</span><span class="sxs-lookup"><span data-stu-id="93c2d-417">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="93c2d-418">A opção criação rápida permite que você implante um aplicativo web concluído ao Site do Windows Azure de fora do portal.</span><span class="sxs-lookup"><span data-stu-id="93c2d-418">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="93c2d-419">Ele não inclui etapas para configurar um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="93c2d-419">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="93c2d-420">![Criando um novo Site usando a criação rápida](aspnet-mvc-4-models-and-data-access/_static/image33.png "criar um novo Site usando a criação rápida")</span><span class="sxs-lookup"><span data-stu-id="93c2d-420">![Creating a new Web Site using Quick Create](aspnet-mvc-4-models-and-data-access/_static/image33.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="93c2d-421">*Criando um novo Site usando a criação rápida*</span><span class="sxs-lookup"><span data-stu-id="93c2d-421">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="93c2d-422">Aguarde até que o novo **Site da Web** é criado.</span><span class="sxs-lookup"><span data-stu-id="93c2d-422">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="93c2d-423">Depois que o Site da Web é criado, clique no link sob a **URL** coluna.</span><span class="sxs-lookup"><span data-stu-id="93c2d-423">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="93c2d-424">Verifique se o novo Site da Web está funcionando.</span><span class="sxs-lookup"><span data-stu-id="93c2d-424">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="93c2d-425">![Navegando para o novo site](aspnet-mvc-4-models-and-data-access/_static/image34.png "navegando para o novo site")</span><span class="sxs-lookup"><span data-stu-id="93c2d-425">![Browsing to the new web site](aspnet-mvc-4-models-and-data-access/_static/image34.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="93c2d-426">*Navegando para o novo site*</span><span class="sxs-lookup"><span data-stu-id="93c2d-426">*Browsing to the new web site*</span></span>

    <span data-ttu-id="93c2d-427">![Site da Web em execução](aspnet-mvc-4-models-and-data-access/_static/image35.png "site em execução")</span><span class="sxs-lookup"><span data-stu-id="93c2d-427">![Web site running](aspnet-mvc-4-models-and-data-access/_static/image35.png "Web site running")</span></span>

    <span data-ttu-id="93c2d-428">*Site da Web em execução*</span><span class="sxs-lookup"><span data-stu-id="93c2d-428">*Web site running*</span></span>
6. <span data-ttu-id="93c2d-429">Acesse o portal e clique no nome do site sob o **nome** coluna para exibir as páginas de gerenciamento.</span><span class="sxs-lookup"><span data-stu-id="93c2d-429">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="93c2d-430">![Abrir as páginas de gerenciamento do site da web](aspnet-mvc-4-models-and-data-access/_static/image36.png "abrir páginas de gerenciamento do site")</span><span class="sxs-lookup"><span data-stu-id="93c2d-430">![Opening the web site management pages](aspnet-mvc-4-models-and-data-access/_static/image36.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="93c2d-431">*Abrir as páginas de gerenciamento do Site da Web*</span><span class="sxs-lookup"><span data-stu-id="93c2d-431">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="93c2d-432">No **painel** página no **visão rápida** seção, clique o **baixar perfil de publicação** link.</span><span class="sxs-lookup"><span data-stu-id="93c2d-432">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="93c2d-433">O *perfil de publicação* contém todas as informações necessárias para publicar um aplicativo web para um site do Windows Azure para cada método de publicação habilitado.</span><span class="sxs-lookup"><span data-stu-id="93c2d-433">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="93c2d-434">O perfil de publicação contém as URLs, as credenciais do usuário e cadeias de caracteres de banco de dados necessárias para se conectar e autenticar cada um dos pontos de extremidade para o qual um método de publicação está habilitado.</span><span class="sxs-lookup"><span data-stu-id="93c2d-434">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="93c2d-435">**Microsoft WebMatrix 2**, **Microsoft Visual Studio para Web Express** e **Microsoft Visual Studio 2012** oferecem suporte à leitura perfis de publicação para automatizar a configuração desses programas para publicação de aplicativos web para sites do Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="93c2d-435">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="93c2d-436">![Baixando o site da web de perfil de publicação](aspnet-mvc-4-models-and-data-access/_static/image37.png "baixando o site da web de perfil de publicação")</span><span class="sxs-lookup"><span data-stu-id="93c2d-436">![Downloading the web site publish profile](aspnet-mvc-4-models-and-data-access/_static/image37.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="93c2d-437">*Baixando o Site da Web de perfil de publicação*</span><span class="sxs-lookup"><span data-stu-id="93c2d-437">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="93c2d-438">Baixe o arquivo de perfil de publicação para um local conhecido.</span><span class="sxs-lookup"><span data-stu-id="93c2d-438">Download the publish profile file to a known location.</span></span> <span data-ttu-id="93c2d-439">Mais neste exercício, você verá como usar esse arquivo para publicar um aplicativo web para um Windows Azure Web Sites do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="93c2d-439">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="93c2d-440">![Salvando o arquivo de perfil de publicação](aspnet-mvc-4-models-and-data-access/_static/image38.png "salvar o perfil de publicação")</span><span class="sxs-lookup"><span data-stu-id="93c2d-440">![Saving the publish profile file](aspnet-mvc-4-models-and-data-access/_static/image38.png "Saving the publish profile")</span></span>

    <span data-ttu-id="93c2d-441">*Salvando o arquivo de perfil de publicação*</span><span class="sxs-lookup"><span data-stu-id="93c2d-441">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="93c2d-442">Tarefa 2 – Configurando o servidor de banco de dados</span><span class="sxs-lookup"><span data-stu-id="93c2d-442">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="93c2d-443">Se seu aplicativo utiliza o SQL Server bancos de dados que você precisará criar um servidor de banco de dados SQL.</span><span class="sxs-lookup"><span data-stu-id="93c2d-443">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="93c2d-444">Se você deseja implantar um aplicativo simples que não usa o SQL Server, você pode ignorar esta tarefa.</span><span class="sxs-lookup"><span data-stu-id="93c2d-444">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="93c2d-445">Você precisará de um servidor de banco de dados SQL para armazenar o banco de dados do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="93c2d-445">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="93c2d-446">Você pode exibir os servidores de banco de dados SQL de sua assinatura no portal de gerenciamento do Windows Azure em **bancos de dados Sql** | **servidores** | **do servidor Painel**.</span><span class="sxs-lookup"><span data-stu-id="93c2d-446">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="93c2d-447">Se você não tiver um servidor criado, você pode criar um usando o **adicionar** botão na barra de comandos.</span><span class="sxs-lookup"><span data-stu-id="93c2d-447">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="93c2d-448">Anote o **nome do servidor e o URL, o nome de logon de administrador e senha**, pois você usará nas próximas tarefas.</span><span class="sxs-lookup"><span data-stu-id="93c2d-448">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="93c2d-449">Não crie o banco de dados, como ele será criado em um estágio posterior.</span><span class="sxs-lookup"><span data-stu-id="93c2d-449">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="93c2d-450">![Painel do servidor de banco de dados SQL](aspnet-mvc-4-models-and-data-access/_static/image39.png "painel de banco de dados do SQL Server")</span><span class="sxs-lookup"><span data-stu-id="93c2d-450">![SQL Database Server Dashboard](aspnet-mvc-4-models-and-data-access/_static/image39.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="93c2d-451">*Painel de banco de dados do SQL Server*</span><span class="sxs-lookup"><span data-stu-id="93c2d-451">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="93c2d-452">A próxima tarefa, você testará a conexão de banco de dados do Visual Studio, por esse motivo, você precisa incluir seu endereço IP local na lista do servidor de **endereços IP permitidos**.</span><span class="sxs-lookup"><span data-stu-id="93c2d-452">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="93c2d-453">Para fazer isso, clique em **configurar**, selecione o endereço IP do **endereço IP do cliente atual** e cole-o no **endereço IP inicial** e **oendereçoIPfinal** caixas de texto e clique no ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) botão.</span><span class="sxs-lookup"><span data-stu-id="93c2d-453">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) button.</span></span>

    ![Adicionar endereço IP do cliente](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    <span data-ttu-id="93c2d-455">*Adicionar endereço IP do cliente*</span><span class="sxs-lookup"><span data-stu-id="93c2d-455">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="93c2d-456">Uma vez o **endereço IP do cliente** é adicionado para os endereços IP permitidos, clique em **salvar** para confirmar as alterações.</span><span class="sxs-lookup"><span data-stu-id="93c2d-456">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Confirmar alterações](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    <span data-ttu-id="93c2d-458">*Confirmar alterações*</span><span class="sxs-lookup"><span data-stu-id="93c2d-458">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="93c2d-459">Tarefa 3: publicar um aplicativo ASP.NET MVC 4 usando a implantação da Web</span><span class="sxs-lookup"><span data-stu-id="93c2d-459">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="93c2d-460">Volte para a solução do ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="93c2d-460">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="93c2d-461">No **Solution Explorer**, com o botão direito no projeto de site da web e selecione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="93c2d-461">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="93c2d-462">![Publicar o aplicativo](aspnet-mvc-4-models-and-data-access/_static/image43.png "a publicação do aplicativo")</span><span class="sxs-lookup"><span data-stu-id="93c2d-462">![Publishing the Application](aspnet-mvc-4-models-and-data-access/_static/image43.png "Publishing the Application")</span></span>

    <span data-ttu-id="93c2d-463">*Publicar o site*</span><span class="sxs-lookup"><span data-stu-id="93c2d-463">*Publishing the web site*</span></span>
2. <span data-ttu-id="93c2d-464">Importe o perfil de publicação que você salvou na primeira tarefa.</span><span class="sxs-lookup"><span data-stu-id="93c2d-464">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="93c2d-465">![Importar o perfil de publicação](aspnet-mvc-4-models-and-data-access/_static/image44.png "importar o perfil de publicação")</span><span class="sxs-lookup"><span data-stu-id="93c2d-465">![Importing the publish profile](aspnet-mvc-4-models-and-data-access/_static/image44.png "Importing the publish profile")</span></span>

    <span data-ttu-id="93c2d-466">*Importar o perfil de publicação*</span><span class="sxs-lookup"><span data-stu-id="93c2d-466">*Importing publish profile*</span></span>
3. <span data-ttu-id="93c2d-467">Clique em **validar Conexão**.</span><span class="sxs-lookup"><span data-stu-id="93c2d-467">Click **Validate Connection**.</span></span> <span data-ttu-id="93c2d-468">Quando a validação estiver concluída, clique em **próximo**.</span><span class="sxs-lookup"><span data-stu-id="93c2d-468">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="93c2d-469">A validação é concluída depois que aparecer uma marca de seleção verde ao lado do botão Validar Conexão.</span><span class="sxs-lookup"><span data-stu-id="93c2d-469">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="93c2d-470">![Validando a conexão](aspnet-mvc-4-models-and-data-access/_static/image45.png "validação de conexão")</span><span class="sxs-lookup"><span data-stu-id="93c2d-470">![Validating connection](aspnet-mvc-4-models-and-data-access/_static/image45.png "Validating connection")</span></span>

    <span data-ttu-id="93c2d-471">*Validação de conexão*</span><span class="sxs-lookup"><span data-stu-id="93c2d-471">*Validating connection*</span></span>
4. <span data-ttu-id="93c2d-472">No **configurações** página no **bancos de dados** seção, clique no botão ao lado da caixa de texto da sua conexão de banco de dados (ou seja, **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="93c2d-472">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="93c2d-473">![Configuração de implantação da Web](aspnet-mvc-4-models-and-data-access/_static/image46.png "configuração de implantação da Web")</span><span class="sxs-lookup"><span data-stu-id="93c2d-473">![Web deploy configuration](aspnet-mvc-4-models-and-data-access/_static/image46.png "Web deploy configuration")</span></span>

    <span data-ttu-id="93c2d-474">*Configuração de implantação da Web*</span><span class="sxs-lookup"><span data-stu-id="93c2d-474">*Web deploy configuration*</span></span>
5. <span data-ttu-id="93c2d-475">Configure a conexão de banco de dados da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="93c2d-475">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="93c2d-476">No **nome do servidor** digite sua URL de servidor de banco de dados SQL usando o *tcp:* prefixo.</span><span class="sxs-lookup"><span data-stu-id="93c2d-476">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="93c2d-477">Em **nome de usuário** digite seu nome de logon de administrador do servidor.</span><span class="sxs-lookup"><span data-stu-id="93c2d-477">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="93c2d-478">Em **senha** digite sua senha de logon de administrador do servidor.</span><span class="sxs-lookup"><span data-stu-id="93c2d-478">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="93c2d-479">Digite um novo nome de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="93c2d-479">Type a new database name.</span></span>

     <span data-ttu-id="93c2d-480">![Configurando a cadeia de caracteres de conexão de destino](aspnet-mvc-4-models-and-data-access/_static/image47.png "Configurando a cadeia de caracteres de conexão de destino")</span><span class="sxs-lookup"><span data-stu-id="93c2d-480">![Configuring destination connection string](aspnet-mvc-4-models-and-data-access/_static/image47.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="93c2d-481">*Configurando a cadeia de caracteres de conexão de destino*</span><span class="sxs-lookup"><span data-stu-id="93c2d-481">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="93c2d-482">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="93c2d-482">Then click **OK**.</span></span> <span data-ttu-id="93c2d-483">Quando solicitado a criar o banco de dados, clique em **Sim**.</span><span class="sxs-lookup"><span data-stu-id="93c2d-483">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="93c2d-484">![Criando o banco de dados](aspnet-mvc-4-models-and-data-access/_static/image48.png "criar a cadeia de caracteres do banco de dados")</span><span class="sxs-lookup"><span data-stu-id="93c2d-484">![Creating the database](aspnet-mvc-4-models-and-data-access/_static/image48.png "Creating the database string")</span></span>

    <span data-ttu-id="93c2d-485">*Criando o banco de dados*</span><span class="sxs-lookup"><span data-stu-id="93c2d-485">*Creating the database*</span></span>
7. <span data-ttu-id="93c2d-486">A cadeia de caracteres de conexão que você usará para se conectar ao banco de dados SQL no Windows Azure é mostrada na caixa de texto de Conexão padrão.</span><span class="sxs-lookup"><span data-stu-id="93c2d-486">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="93c2d-487">Clique em **Avançar**.</span><span class="sxs-lookup"><span data-stu-id="93c2d-487">Then click **Next**.</span></span>

    <span data-ttu-id="93c2d-488">![Cadeia de caracteres de Conexão que aponta para o banco de dados SQL](aspnet-mvc-4-models-and-data-access/_static/image49.png "cadeia de caracteres de Conexão que aponta para o banco de dados SQL")</span><span class="sxs-lookup"><span data-stu-id="93c2d-488">![Connection string pointing to SQL Database](aspnet-mvc-4-models-and-data-access/_static/image49.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="93c2d-489">*Cadeia de caracteres de Conexão que aponta para o banco de dados SQL*</span><span class="sxs-lookup"><span data-stu-id="93c2d-489">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="93c2d-490">No **visualização** , clique em **publicar**.</span><span class="sxs-lookup"><span data-stu-id="93c2d-490">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="93c2d-491">![Publicar o aplicativo web](aspnet-mvc-4-models-and-data-access/_static/image50.png "publicar o aplicativo web")</span><span class="sxs-lookup"><span data-stu-id="93c2d-491">![Publishing the web application](aspnet-mvc-4-models-and-data-access/_static/image50.png "Publishing the web application")</span></span>

    <span data-ttu-id="93c2d-492">*Publicar o aplicativo web*</span><span class="sxs-lookup"><span data-stu-id="93c2d-492">*Publishing the web application*</span></span>
9. <span data-ttu-id="93c2d-493">Quando termina o processo de publicação, o navegador padrão abrirá o site da web publicado.</span><span class="sxs-lookup"><span data-stu-id="93c2d-493">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="93c2d-494">Apêndice c: usar trechos de código</span><span class="sxs-lookup"><span data-stu-id="93c2d-494">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="93c2d-495">Com trechos de código, você tem todo o código que é necessário ao seu alcance.</span><span class="sxs-lookup"><span data-stu-id="93c2d-495">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="93c2d-496">O documento de laboratório informará exatamente quando você pode usá-los, conforme mostrado na figura a seguir.</span><span class="sxs-lookup"><span data-stu-id="93c2d-496">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="93c2d-497">![Usando trechos de código do Visual Studio para inserir o código no seu projeto](aspnet-mvc-4-models-and-data-access/_static/image51.png "trechos de código com o Visual Studio para inserir código em seu projeto")</span><span class="sxs-lookup"><span data-stu-id="93c2d-497">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-models-and-data-access/_static/image51.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="93c2d-498">*Usando trechos de código do Visual Studio para inserir código em seu projeto*</span><span class="sxs-lookup"><span data-stu-id="93c2d-498">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="93c2d-499">***Para adicionar um trecho de código usando o teclado (apenas c#)***</span><span class="sxs-lookup"><span data-stu-id="93c2d-499">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="93c2d-500">Posicione o cursor onde você deseja inserir o código.</span><span class="sxs-lookup"><span data-stu-id="93c2d-500">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="93c2d-501">Comece a digitar o nome do trecho (sem espaços ou hifens).</span><span class="sxs-lookup"><span data-stu-id="93c2d-501">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="93c2d-502">Observe como IntelliSense exibe os nomes dos trechos de código correspondentes.</span><span class="sxs-lookup"><span data-stu-id="93c2d-502">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="93c2d-503">Selecione o trecho correto (ou continue digitando até que o nome do trecho de código inteiro é selecionado).</span><span class="sxs-lookup"><span data-stu-id="93c2d-503">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="93c2d-504">Pressione a tecla Tab duas vezes para inserir o trecho de código no local do cursor.</span><span class="sxs-lookup"><span data-stu-id="93c2d-504">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="93c2d-505">![Comece a digitar o nome do fragmento](aspnet-mvc-4-models-and-data-access/_static/image52.png "comece a digitar o nome do trecho de código")</span><span class="sxs-lookup"><span data-stu-id="93c2d-505">![Start typing the snippet name](aspnet-mvc-4-models-and-data-access/_static/image52.png "Start typing the snippet name")</span></span>

<span data-ttu-id="93c2d-506">*Comece a digitar o nome do trecho de código*</span><span class="sxs-lookup"><span data-stu-id="93c2d-506">*Start typing the snippet name*</span></span>

<span data-ttu-id="93c2d-507">![Pressione Tab para selecionar o trecho de código realçado](aspnet-mvc-4-models-and-data-access/_static/image53.png "pressione Tab para selecionar o trecho de código realçado")</span><span class="sxs-lookup"><span data-stu-id="93c2d-507">![Press Tab to select the highlighted snippet](aspnet-mvc-4-models-and-data-access/_static/image53.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="93c2d-508">*Pressione Tab para selecionar o trecho de código realçado*</span><span class="sxs-lookup"><span data-stu-id="93c2d-508">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="93c2d-509">![Pressione Tab novamente e o trecho expandirá](aspnet-mvc-4-models-and-data-access/_static/image54.png "expandirá pressione Tab novamente e o trecho de código")</span><span class="sxs-lookup"><span data-stu-id="93c2d-509">![Press Tab again and the snippet will expand](aspnet-mvc-4-models-and-data-access/_static/image54.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="93c2d-510">*Pressione Tab novamente e o trecho de código serão expandida*</span><span class="sxs-lookup"><span data-stu-id="93c2d-510">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="93c2d-511">***Para adicionar um trecho de código usando o mouse (c#, Visual Basic e XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="93c2d-511">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="93c2d-512">Clique com botão direito em que você deseja inserir o trecho de código.</span><span class="sxs-lookup"><span data-stu-id="93c2d-512">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="93c2d-513">Selecione **Inserir trecho** seguido por **Meus trechos de código**.</span><span class="sxs-lookup"><span data-stu-id="93c2d-513">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="93c2d-514">Selecione o trecho relevante na lista, clicando nele.</span><span class="sxs-lookup"><span data-stu-id="93c2d-514">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="93c2d-515">![Com o botão direito em que você deseja inserir o trecho de código e selecione Inserir trecho](aspnet-mvc-4-models-and-data-access/_static/image55.png "com o botão direito em que você deseja inserir o trecho de código e selecione Inserir trecho")</span><span class="sxs-lookup"><span data-stu-id="93c2d-515">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-models-and-data-access/_static/image55.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="93c2d-516">*Clique com botão direito em que você deseja inserir o trecho de código e selecione Inserir trecho*</span><span class="sxs-lookup"><span data-stu-id="93c2d-516">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="93c2d-517">![Selecione o trecho relevante na lista, clicando nele](aspnet-mvc-4-models-and-data-access/_static/image56.png "escolher o trecho relevante na lista, clicando nele")</span><span class="sxs-lookup"><span data-stu-id="93c2d-517">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-models-and-data-access/_static/image56.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="93c2d-518">*Selecione o trecho relevante na lista, clicando nele*</span><span class="sxs-lookup"><span data-stu-id="93c2d-518">*Pick the relevant snippet from the list, by clicking on it*</span></span>
