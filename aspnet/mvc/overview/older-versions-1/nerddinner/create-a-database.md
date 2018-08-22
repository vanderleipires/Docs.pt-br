---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: Criar um banco de dados | Microsoft Docs
author: microsoft
description: Etapa 2 mostra as etapas para criar o banco de dados que contém todos os o jantar e CONFIRME os dados de nosso aplicativo NerdDinner.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: b6ab0740f251889f0fa0561809cac2bbe79bcb3a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823507"
---
<a name="create-a-database"></a><span data-ttu-id="97ac2-103">Criar um banco de dados</span><span class="sxs-lookup"><span data-stu-id="97ac2-103">Create a Database</span></span>
====================
<span data-ttu-id="97ac2-104">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="97ac2-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="97ac2-105">Baixar PDF</span><span class="sxs-lookup"><span data-stu-id="97ac2-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="97ac2-106">Esta é a etapa 2 do grátis [aplicativo "NerdDinner"](introducing-the-nerddinner-tutorial.md) que orienta-through como criar um pequeno, mas concluir, o aplicativo web usando ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="97ac2-106">This is step 2 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="97ac2-107">Etapa 2 mostra as etapas para criar o banco de dados que contém todos os o jantar e CONFIRME os dados de nosso aplicativo NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="97ac2-107">Step 2 shows the steps to create the database holding all of the dinner and RSVP data for our NerdDinner application.</span></span>
> 
> <span data-ttu-id="97ac2-108">Se você estiver usando o ASP.NET MVC 3, recomendamos que você siga a [obtendo iniciado com o MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [Store de música do MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriais.</span><span class="sxs-lookup"><span data-stu-id="97ac2-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-2-creating-the-database"></a><span data-ttu-id="97ac2-109">Etapa 2 do NerdDinner: Criando o banco de dados</span><span class="sxs-lookup"><span data-stu-id="97ac2-109">NerdDinner Step 2: Creating the Database</span></span>

<span data-ttu-id="97ac2-110">Usaremos um banco de dados para armazenar todos os dados de jantar e RSVP para nosso aplicativo NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="97ac2-110">We'll be using a database to store all of the Dinner and RSVP data for our NerdDinner application.</span></span>

<span data-ttu-id="97ac2-111">As etapas a seguir mostram a criação de banco de dados usando a edição gratuita do SQL Server Express (que você pode instalar facilmente usando V2 do [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)).</span><span class="sxs-lookup"><span data-stu-id="97ac2-111">The steps below show creating the database using the free SQL Server Express edition (which you can easily install using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)).</span></span> <span data-ttu-id="97ac2-112">Todo o código que escreveremos funciona com o SQL Server Express e o SQL Server completo.</span><span class="sxs-lookup"><span data-stu-id="97ac2-112">All of the code we'll write works with both SQL Server Express and the full SQL Server.</span></span>

### <a name="creating-a-new-sql-server-express-database"></a><span data-ttu-id="97ac2-113">Criando um novo banco de dados do SQL Server Express</span><span class="sxs-lookup"><span data-stu-id="97ac2-113">Creating a new SQL Server Express database</span></span>

<span data-ttu-id="97ac2-114">Vamos começar clicando-se em nosso projeto web e, em seguida, selecione a **Add -&gt;Novo Item** comando de menu:</span><span class="sxs-lookup"><span data-stu-id="97ac2-114">We'll begin by right-clicking on our web project, and then select the **Add-&gt;New Item** menu command:</span></span>

![](create-a-database/_static/image1.png)

<span data-ttu-id="97ac2-115">Isso exibirá a caixa de diálogo de "Adicionar Novo Item" do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="97ac2-115">This will bring up Visual Studio's "Add New Item" dialog.</span></span> <span data-ttu-id="97ac2-116">Vamos filtrar por categoria "Dados" e selecione o modelo de item de "Banco de dados do SQL Server":</span><span class="sxs-lookup"><span data-stu-id="97ac2-116">We'll filter by the "Data" category and select the "SQL Server Database" item template:</span></span>

![](create-a-database/_static/image2.png)

<span data-ttu-id="97ac2-117">Nomear o banco de dados do SQL Server Express que desejamos criar "NerdDinner.mdf" e pressionar okey.</span><span class="sxs-lookup"><span data-stu-id="97ac2-117">We'll name the SQL Server Express database we want to create "NerdDinner.mdf" and hit ok.</span></span> <span data-ttu-id="97ac2-118">Visual Studio, então, solicitará-nos se quisermos adicionar esse arquivo para nosso \App\_diretório de dados (que é um diretório já de instalação com a leitura e gravação de ACLs de segurança):</span><span class="sxs-lookup"><span data-stu-id="97ac2-118">Visual Studio will then ask us if we want to add this file to our \App\_Data directory (which is a directory already setup with both read and write security ACLs):</span></span>

![](create-a-database/_static/image3.png)

<span data-ttu-id="97ac2-119">Vamos clicar em "Sim" e nosso novo banco de dados será criado e adicionado ao nosso Gerenciador de soluções:</span><span class="sxs-lookup"><span data-stu-id="97ac2-119">We'll click "Yes" and our new database will be created and added to our Solution Explorer:</span></span>

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a><span data-ttu-id="97ac2-120">Criando tabelas dentro de nosso banco de dados</span><span class="sxs-lookup"><span data-stu-id="97ac2-120">Creating Tables within our Database</span></span>

<span data-ttu-id="97ac2-121">Agora temos um novo banco de dados vazio.</span><span class="sxs-lookup"><span data-stu-id="97ac2-121">We now have a new empty database.</span></span> <span data-ttu-id="97ac2-122">Vamos adicionar algumas tabelas a ele.</span><span class="sxs-lookup"><span data-stu-id="97ac2-122">Let's add some tables to it.</span></span>

<span data-ttu-id="97ac2-123">Para fazer isso vamos navegar até a janela de guia "Gerenciador de servidores" dentro do Visual Studio, que permite gerenciar servidores e bancos de dados.</span><span class="sxs-lookup"><span data-stu-id="97ac2-123">To do this we'll navigate to the "Server Explorer" tab window within Visual Studio, which enables us to manage databases and servers.</span></span> <span data-ttu-id="97ac2-124">Bancos de dados SQL Server Express armazenados em do \App\_pasta de dados do nosso aplicativo aparecerá automaticamente no Gerenciador de servidores.</span><span class="sxs-lookup"><span data-stu-id="97ac2-124">SQL Server Express databases stored in the \App\_Data folder of our application will automatically show up within the Server Explorer.</span></span> <span data-ttu-id="97ac2-125">Pode, opcionalmente, usamos o ícone de "Conectar-se ao banco de dados" na parte superior da janela "Server Explorer" para adicionar bancos de dados do SQL Server adicionais (locais e remotos) para a lista também:</span><span class="sxs-lookup"><span data-stu-id="97ac2-125">We can optionally use the "Connect to Database" icon on the top of the "Server Explorer" window to add additional SQL Server databases (both local and remote) to the list as well:</span></span>

![](create-a-database/_static/image5.png)

<span data-ttu-id="97ac2-126">Vamos adicionar duas tabelas para nosso banco de dados do NerdDinner – um para armazenar nosso jantares e outro para acompanhar o RSVP aceitações a eles.</span><span class="sxs-lookup"><span data-stu-id="97ac2-126">We will add two tables to our NerdDinner database – one to store our Dinners, and the other to track RSVP acceptances to them.</span></span> <span data-ttu-id="97ac2-127">Podemos criar novas tabelas clicando duas vezes na pasta "Tabelas" dentro do nosso banco de dados e escolher o comando de menu "Adicionar nova tabela":</span><span class="sxs-lookup"><span data-stu-id="97ac2-127">We can create new tables by right-clicking on the "Tables" folder within our database and choosing the "Add New Table" menu command:</span></span>

![](create-a-database/_static/image6.png)

<span data-ttu-id="97ac2-128">Isso abrirá um designer de tabela que permite que configuremos o esquema de nossa tabela.</span><span class="sxs-lookup"><span data-stu-id="97ac2-128">This will open up a table designer that allows us to configure the schema of our table.</span></span> <span data-ttu-id="97ac2-129">Para nossa tabela "Jantares", adicionaremos 10 colunas de dados:</span><span class="sxs-lookup"><span data-stu-id="97ac2-129">For our "Dinners" table we will add 10 columns of data:</span></span>

![](create-a-database/_static/image7.png)

<span data-ttu-id="97ac2-130">Queremos que a coluna "DinnerID" para ser uma chave primária exclusiva para a tabela.</span><span class="sxs-lookup"><span data-stu-id="97ac2-130">We want the "DinnerID" column to be a unique primary key for the table.</span></span> <span data-ttu-id="97ac2-131">Podemos configurar isso clicando duas vezes na coluna "DinnerID" e escolhendo o item de menu "Definir chave primária":</span><span class="sxs-lookup"><span data-stu-id="97ac2-131">We can configure this by right-clicking on the "DinnerID" column and choosing the "Set Primary Key" menu item:</span></span>

![](create-a-database/_static/image8.png)

<span data-ttu-id="97ac2-132">Além de tornar DinnerID uma chave primária, também queremos que configurá-lo como uma coluna de "identidade" cujo valor é incrementado automaticamente conforme novas linhas de dados são adicionadas à tabela (ou seja, a primeira linha inserida do jantar terá um DinnerID de 1, o segundo inserido linha terá um DinnerID de 2, etc).</span><span class="sxs-lookup"><span data-stu-id="97ac2-132">In addition to making DinnerID a primary key, we also want configure it as an "identity" column whose value is automatically incremented as new rows of data are added to the table (meaning the first inserted Dinner row will have a DinnerID of 1, the second inserted row will have a DinnerID of 2, etc).</span></span>

<span data-ttu-id="97ac2-133">Podemos fazer isso, selecione a coluna "DinnerID" e, em seguida, use o editor de "Propriedades da coluna" para definir a propriedade "(é identidade)" na coluna como "Sim".</span><span class="sxs-lookup"><span data-stu-id="97ac2-133">We can do this by selecting the "DinnerID" column and then use the "Column Properties" editor to set the "(Is Identity)" property on the column to "Yes".</span></span> <span data-ttu-id="97ac2-134">Usaremos os padrões de identidade padrão (iniciam em 1 e incrementar 1 em cada nova linha de jantar):</span><span class="sxs-lookup"><span data-stu-id="97ac2-134">We will use the standard identity defaults (start at 1 and increment 1 on each new Dinner row):</span></span>

![](create-a-database/_static/image9.png)

<span data-ttu-id="97ac2-135">Em seguida, salvaremos nossa tabela digitando Ctrl-S ou usando o **arquivo -&gt;salvar** comando de menu.</span><span class="sxs-lookup"><span data-stu-id="97ac2-135">We'll then save our table by typing Ctrl-S or by using the **File-&gt;Save** menu command.</span></span> <span data-ttu-id="97ac2-136">Essa ação solicitará que nos dê um nome de tabela.</span><span class="sxs-lookup"><span data-stu-id="97ac2-136">This will prompt us to name the table.</span></span> <span data-ttu-id="97ac2-137">Vamos chamá-la de "Jantares":</span><span class="sxs-lookup"><span data-stu-id="97ac2-137">We'll name it "Dinners":</span></span>

![](create-a-database/_static/image10.png)

<span data-ttu-id="97ac2-138">Nossa nova tabela de jantares, em seguida, serão exibidos em nosso banco de dados no Gerenciador de servidores.</span><span class="sxs-lookup"><span data-stu-id="97ac2-138">Our new Dinners table will then show up within our database in the server explorer.</span></span>

<span data-ttu-id="97ac2-139">Vamos, em seguida, repita as etapas acima e criar uma tabela de "Confirmação".</span><span class="sxs-lookup"><span data-stu-id="97ac2-139">We'll then repeat the above steps and create a "RSVP" table.</span></span> <span data-ttu-id="97ac2-140">Essa tabela com tem 3 colunas.</span><span class="sxs-lookup"><span data-stu-id="97ac2-140">This table with have 3 columns.</span></span> <span data-ttu-id="97ac2-141">Vamos configurar a coluna RsvpID como a chave primária e também o tornam uma coluna de identidade:</span><span class="sxs-lookup"><span data-stu-id="97ac2-141">We will setup the RsvpID column as the primary key, and also make it an identity column:</span></span>

![](create-a-database/_static/image11.png)

<span data-ttu-id="97ac2-142">Vamos salvá-lo e dê a ele o nome "RSVP".</span><span class="sxs-lookup"><span data-stu-id="97ac2-142">We'll save it and give it the name "RSVP".</span></span>

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a><span data-ttu-id="97ac2-143">Como configurar uma relação de chave estrangeira entre tabelas</span><span class="sxs-lookup"><span data-stu-id="97ac2-143">Setting up a Foreign Key Relationship between Tables</span></span>

<span data-ttu-id="97ac2-144">Agora temos duas tabelas dentro de nosso banco de dados.</span><span class="sxs-lookup"><span data-stu-id="97ac2-144">We now have two tables within our database.</span></span> <span data-ttu-id="97ac2-145">Nossa última etapa de design de esquema será configurar um relacionamento entre essas duas tabelas – de "um-para-muitos", de modo que podemos pode associar cada linha de jantar com zero ou mais linhas RSVP que se aplicam a ele.</span><span class="sxs-lookup"><span data-stu-id="97ac2-145">Our last schema design step will be to setup a "one-to-many" relationship between these two tables – so that we can associate each Dinner row with zero or more RSVP rows that apply to it.</span></span> <span data-ttu-id="97ac2-146">Faremos isso configurando a RSVP coluna da tabela "DinnerID" para ter uma relação de chave estrangeira para a coluna "DinnerID" na tabela "Jantares".</span><span class="sxs-lookup"><span data-stu-id="97ac2-146">We will do this by configuring the RSVP table's "DinnerID" column to have a foreign-key relationship to the "DinnerID" column in the "Dinners" table.</span></span>

<span data-ttu-id="97ac2-147">Para fazer isso, que vamos abri a tabela de RSVP dentro do designer de tabela clicando duas vezes no Gerenciador de servidores.</span><span class="sxs-lookup"><span data-stu-id="97ac2-147">To do this we'll open up the RSVP table within the table designer by double-clicking it in the server explorer.</span></span> <span data-ttu-id="97ac2-148">Em seguida, vamos selecionar a coluna "DinnerID" dentro dele, direito do mouse e escolha o comando de menu de contexto "Relationshps...":</span><span class="sxs-lookup"><span data-stu-id="97ac2-148">We'll then select the "DinnerID" column within it, right-click, and choose the "Relationshps…" context menu command:</span></span>

![](create-a-database/_static/image12.png)

<span data-ttu-id="97ac2-149">Isso abrirá uma caixa de diálogo que podemos usar relações de instalação entre tabelas:</span><span class="sxs-lookup"><span data-stu-id="97ac2-149">This will bring up a dialog that we can use to setup relationships between tables:</span></span>

![](create-a-database/_static/image13.png)

<span data-ttu-id="97ac2-150">Clicaremos no botão "Adicionar" para adicionar uma nova relação na caixa de diálogo.</span><span class="sxs-lookup"><span data-stu-id="97ac2-150">We'll click the "Add" button to add a new relationship to the dialog.</span></span> <span data-ttu-id="97ac2-151">Após a adição de uma relação, podemos vai expandir o nó do modo de exibição de árvore de "Especificação de tabelas e coluna" dentro da grade de propriedade à direita da caixa de diálogo e, em seguida, clique no botão "..." à direita dele:</span><span class="sxs-lookup"><span data-stu-id="97ac2-151">Once a relationship has been added, we'll expand the "Tables and Column Specification" tree-view node within the property grid to the right of the dialog, and then click the "…" button to the right of it:</span></span>

![](create-a-database/_static/image14.png)

<span data-ttu-id="97ac2-152">Clicar no botão "..." abrirá outra caixa de diálogo que nos permite especificar quais tabelas e colunas estão envolvidas na relação, como também nos permitem nomear a relação.</span><span class="sxs-lookup"><span data-stu-id="97ac2-152">Clicking the "…" button will bring up another dialog that allows us to specify which tables and columns are involved in the relationship, as well as allow us to name the relationship.</span></span>

<span data-ttu-id="97ac2-153">Iremos alterar a tabela de chave primária para ser "Jantares" e selecione a coluna "DinnerID" dentro da tabela de jantares como a chave primária.</span><span class="sxs-lookup"><span data-stu-id="97ac2-153">We will change the Primary Key Table to be "Dinners", and select the "DinnerID" column within the Dinners table as the primary key.</span></span> <span data-ttu-id="97ac2-154">Nossa tabela RSVP será o RSVP e a tabela de chave estrangeira. Coluna DinnerID será associada como a chave estrangeira:</span><span class="sxs-lookup"><span data-stu-id="97ac2-154">Our RSVP table will be the foreign-key table, and the RSVP.DinnerID column will be associated as the foreign-key:</span></span>

![](create-a-database/_static/image15.png)

<span data-ttu-id="97ac2-155">Agora, cada linha na tabela RSVP será associada uma linha na tabela de jantar.</span><span class="sxs-lookup"><span data-stu-id="97ac2-155">Now each row in the RSVP table will be associated with a row in the Dinner table.</span></span> <span data-ttu-id="97ac2-156">SQL Server irá manter a integridade referencial para que possamos – e nos impedem de adicionar uma nova linha RSVP se ele não aponta para uma linha válida do jantar.</span><span class="sxs-lookup"><span data-stu-id="97ac2-156">SQL Server will maintain referential integrity for us – and prevent us from adding a new RSVP row if it does not point to a valid Dinner row.</span></span> <span data-ttu-id="97ac2-157">Ele também impedirá nos de exclusão de uma linha de jantar se não houver ainda RSVP linhas se referindo a ele.</span><span class="sxs-lookup"><span data-stu-id="97ac2-157">It will also prevent us from deleting a Dinner row if there are still RSVP rows referring to it.</span></span>

### <a name="adding-data-to-our-tables"></a><span data-ttu-id="97ac2-158">Adicionando dados a nossas tabelas</span><span class="sxs-lookup"><span data-stu-id="97ac2-158">Adding Data to our Tables</span></span>

<span data-ttu-id="97ac2-159">Vamos terminar com a adição de alguns dados de exemplo para nossa tabela jantares.</span><span class="sxs-lookup"><span data-stu-id="97ac2-159">Let's finish by adding some sample data to our Dinners table.</span></span> <span data-ttu-id="97ac2-160">Podemos adicionar dados a uma tabela clicando duas vezes no Gerenciador de servidores e escolhendo o comando "Mostrar dados da tabela":</span><span class="sxs-lookup"><span data-stu-id="97ac2-160">We can add data to a table by right-clicking on it within the Server Explorer and choosing the "Show Table Data" command:</span></span>

![](create-a-database/_static/image16.png)

<span data-ttu-id="97ac2-161">Vamos adicionar algumas linhas de dados de Dinner que usaremos mais tarde, como podemos começar a implementar o aplicativo:</span><span class="sxs-lookup"><span data-stu-id="97ac2-161">We'll add a few rows of Dinner data that we can use later as we start implementing the application:</span></span>

![](create-a-database/_static/image17.png)

### <a name="next-step"></a><span data-ttu-id="97ac2-162">Próxima etapa</span><span class="sxs-lookup"><span data-stu-id="97ac2-162">Next Step</span></span>

<span data-ttu-id="97ac2-163">Podemos concluir a criação de nosso banco de dados.</span><span class="sxs-lookup"><span data-stu-id="97ac2-163">We've finished creating our database.</span></span> <span data-ttu-id="97ac2-164">Agora, vamos criar classes de modelo que podemos usar para consultar e atualizá-lo.</span><span class="sxs-lookup"><span data-stu-id="97ac2-164">Let's now create model classes that we can use to query and update it.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="97ac2-165">[Anterior](create-a-new-aspnet-mvc-project.md)
> [Próximo](build-a-model-with-business-rule-validations.md)</span><span class="sxs-lookup"><span data-stu-id="97ac2-165">[Previous](create-a-new-aspnet-mvc-project.md)
[Next](build-a-model-with-business-rule-validations.md)</span></span>
