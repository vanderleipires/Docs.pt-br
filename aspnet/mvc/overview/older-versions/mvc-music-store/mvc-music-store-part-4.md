---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: 'Parte 4: Acesso a dados e modelos | Microsoft Docs'
author: jongalloway
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo do repositório de música do ASP.NET MVC. Parte 4 aborda o acesso a dados e modelos.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: 76671bbc7050d111b4d156c45584ba5aa4f1ea8f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879472"
---
<a name="part-4-models-and-data-access"></a><span data-ttu-id="08076-104">Parte 4: Modelos e acesso a dados</span><span class="sxs-lookup"><span data-stu-id="08076-104">Part 4: Models and Data Access</span></span>
====================
<span data-ttu-id="08076-105">por [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="08076-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="08076-106">O repositório de música MVC é um aplicativo tutorial que apresenta e explica passo a passo sobre como usar o ASP.NET MVC e o Visual Studio para desenvolvimento na web.</span><span class="sxs-lookup"><span data-stu-id="08076-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="08076-107">O repositório de música MVC é uma implementação de repositório de exemplo leve que vende álbuns de música online e implementa a administração de site básico, entrada do usuário e funcionalidade do carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="08076-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>
> 
> <span data-ttu-id="08076-108">Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo do repositório de música do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="08076-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="08076-109">Parte 4 aborda o acesso a dados e modelos.</span><span class="sxs-lookup"><span data-stu-id="08076-109">Part 4 covers Models and Data Access.</span></span>


<span data-ttu-id="08076-110">Até agora, nós já apenas foi passando "dados fictícios" de nossos controladores para nossos modelos de exibição.</span><span class="sxs-lookup"><span data-stu-id="08076-110">So far, we've just been passing "dummy data" from our Controllers to our View templates.</span></span> <span data-ttu-id="08076-111">Agora estamos prontos para conectar um banco de dados real.</span><span class="sxs-lookup"><span data-stu-id="08076-111">Now we're ready to hook up a real database.</span></span> <span data-ttu-id="08076-112">Neste tutorial falaremos sobre como usar o SQL Server Compact Edition (geralmente chamado de SQL CE) como nosso mecanismo de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="08076-112">In this tutorial we'll be covering how to use SQL Server Compact Edition (often called SQL CE) as our database engine.</span></span> <span data-ttu-id="08076-113">SQL CE é um arquivo incorporado, gratuito, com base em dados que não requerem qualquer instalação ou configuração, o que dificulta muito conveniente para desenvolvimento local.</span><span class="sxs-lookup"><span data-stu-id="08076-113">SQL CE is a free, embedded, file based database that doesn't require any installation or configuration, which makes it really convenient for local development.</span></span>

## <a name="database-access-with-entity-framework-code-first"></a><span data-ttu-id="08076-114">Acesso de banco de dados com o Entity Framework Code-First</span><span class="sxs-lookup"><span data-stu-id="08076-114">Database access with Entity Framework Code-First</span></span>

<span data-ttu-id="08076-115">Vamos usar o suporte do Entity Framework (EF) que está incluído em projetos do ASP.NET MVC 3 para consultar e atualizar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="08076-115">We'll use the Entity Framework (EF) support that is included in ASP.NET MVC 3 projects to query and update the database.</span></span> <span data-ttu-id="08076-116">EF é uma API que permite aos desenvolvedores consultar e atualizar dados armazenados em um banco de dados de uma maneira orientada a objeto de dados do (ORM) de mapeamento relacional de objeto flexível.</span><span class="sxs-lookup"><span data-stu-id="08076-116">EF is a flexible object relational mapping (ORM) data API that enables developers to query and update data stored in a database in an object-oriented way.</span></span>

<span data-ttu-id="08076-117">Versão 4 do Entity Framework oferece suporte a um paradigma de desenvolvimento chamado código primeiro.</span><span class="sxs-lookup"><span data-stu-id="08076-117">Entity Framework version 4 supports a development paradigm called code-first.</span></span> <span data-ttu-id="08076-118">Primeiro código permite que você crie o objeto de modelo, escrevendo classes simples (também conhecido como POCO dos objetos de CLR "plain old") e pode até mesmo criar o banco de dados em tempo real de suas classes.</span><span class="sxs-lookup"><span data-stu-id="08076-118">Code-first allows you to create model object by writing simple classes (also known as POCO from "plain-old" CLR objects), and can even create the database on the fly from your classes.</span></span>

### <a name="changes-to-our-model-classes"></a><span data-ttu-id="08076-119">Alterações em nossos Classes de modelo</span><span class="sxs-lookup"><span data-stu-id="08076-119">Changes to our Model Classes</span></span>

<span data-ttu-id="08076-120">Podemos aproveitará o recurso de criação de banco de dados do Entity Framework neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="08076-120">We will be leveraging the database creation feature in Entity Framework in this tutorial.</span></span> <span data-ttu-id="08076-121">Antes de fazer isso, no entanto, vamos fazer algumas pequenas alterações para nosso classes de modelo para adicionar algumas coisas que será usado posteriormente.</span><span class="sxs-lookup"><span data-stu-id="08076-121">Before we do that, though, let's make a few minor changes to our model classes to add in some things we'll be using later on.</span></span>

#### <a name="adding-the-artist-model-classes"></a><span data-ttu-id="08076-122">Adicionando as Classes de modelo artista</span><span class="sxs-lookup"><span data-stu-id="08076-122">Adding the Artist Model Classes</span></span>

<span data-ttu-id="08076-123">Nosso álbuns será associados artistas, então vamos adicionar uma classe de modelo simples para descrever um artista.</span><span class="sxs-lookup"><span data-stu-id="08076-123">Our Albums will be associated with Artists, so we'll add a simple model class to describe an Artist.</span></span> <span data-ttu-id="08076-124">Adicione uma nova classe para a pasta modelos chamada Artist.cs usando o código mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="08076-124">Add a new class to the Models folder named Artist.cs using the code shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a><span data-ttu-id="08076-125">Atualizando nossas Classes de modelo</span><span class="sxs-lookup"><span data-stu-id="08076-125">Updating our Model Classes</span></span>

<span data-ttu-id="08076-126">Atualize a classe álbum conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="08076-126">Update the Album class as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

<span data-ttu-id="08076-127">Em seguida, faça as seguintes atualizações para a classe de gênero.</span><span class="sxs-lookup"><span data-stu-id="08076-127">Next, make the following updates to the Genre class.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-appdata-folder"></a><span data-ttu-id="08076-128">Adicionar o aplicativo\_pasta de dados</span><span class="sxs-lookup"><span data-stu-id="08076-128">Adding the App\_Data folder</span></span>

<span data-ttu-id="08076-129">Vamos adicionar um aplicativo\_diretório de dados ao nosso projeto para manter os arquivos de banco de dados SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="08076-129">We'll add an App\_Data directory to our project to hold our SQL Server Express database files.</span></span> <span data-ttu-id="08076-130">Aplicativo\_dados são um diretório especial do ASP.NET que já possui as permissões de acesso de segurança corretas para acesso ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="08076-130">App\_Data is a special directory in ASP.NET which already has the correct security access permissions for database access.</span></span> <span data-ttu-id="08076-131">No menu projeto, selecione Adicionar pasta do ASP.NET e, em seguida, aplicativo\_dados.</span><span class="sxs-lookup"><span data-stu-id="08076-131">From the Project menu, select Add ASP.NET Folder, then App\_Data.</span></span>

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a><span data-ttu-id="08076-132">Criando uma cadeia de caracteres de Conexão no arquivo Web. config</span><span class="sxs-lookup"><span data-stu-id="08076-132">Creating a Connection String in the web.config file</span></span>

<span data-ttu-id="08076-133">Adicionaremos algumas linhas ao arquivo de configuração do site para que o Entity Framework saiba como conectar-se ao nosso banco de dados.</span><span class="sxs-lookup"><span data-stu-id="08076-133">We will add a few lines to the website's configuration file so that Entity Framework knows how to connect to our database.</span></span> <span data-ttu-id="08076-134">Clique duas vezes no arquivo Web. config localizado na raiz do projeto.</span><span class="sxs-lookup"><span data-stu-id="08076-134">Double-click on the Web.config file located in the root of the project.</span></span>

![](mvc-music-store-part-4/_static/image2.png)

<span data-ttu-id="08076-135">Role até o final desse arquivo e adicione um &lt;connectionStrings&gt; seção diretamente acima da última linha, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="08076-135">Scroll to the bottom of this file and add a &lt;connectionStrings&gt; section directly above the last line, as shown below.</span></span>

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a><span data-ttu-id="08076-136">Adicionando uma classe de contexto</span><span class="sxs-lookup"><span data-stu-id="08076-136">Adding a Context Class</span></span>

<span data-ttu-id="08076-137">Com o botão direito na pasta modelos e adicione uma nova classe chamada MusicStoreEntities.cs.</span><span class="sxs-lookup"><span data-stu-id="08076-137">Right-click the Models folder and add a new class named MusicStoreEntities.cs.</span></span>

![](mvc-music-store-part-4/_static/image3.png)

<span data-ttu-id="08076-138">Essa classe será representar o contexto de banco de dados do Entity Framework e será lidar com nosso criar, ler, atualizar e excluir operações para nós.</span><span class="sxs-lookup"><span data-stu-id="08076-138">This class will represent the Entity Framework database context, and will handle our create, read, update, and delete operations for us.</span></span> <span data-ttu-id="08076-139">O código para essa classe é mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="08076-139">The code for this class is shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

<span data-ttu-id="08076-140">Isso é tudo - não há nenhuma outra configuração interfaces especiais, etc. Estendendo a classe base DbContext, nossa classe MusicStoreEntities é capaz de lidar com nossas operações de banco de dados para nós.</span><span class="sxs-lookup"><span data-stu-id="08076-140">That's it - there's no other configuration, special interfaces, etc. By extending the DbContext base class, our MusicStoreEntities class is able to handle our database operations for us.</span></span> <span data-ttu-id="08076-141">Agora que temos que vinculada, vamos adicionar mais propriedades para nossos classes de modelo para tirar proveito de algumas das informações adicionais em nosso banco de dados.</span><span class="sxs-lookup"><span data-stu-id="08076-141">Now that we've got that hooked up, let's add a few more properties to our model classes to take advantage of some of the additional information in our database.</span></span>

### <a name="adding-our-store-catalog-data"></a><span data-ttu-id="08076-142">Adicionando nossos dados de catálogo do repositório</span><span class="sxs-lookup"><span data-stu-id="08076-142">Adding our store catalog data</span></span>

<span data-ttu-id="08076-143">Nós o conduziremos proveito de um recurso no Entity Framework que adiciona dados de "semente" para um banco de dados recém-criado.</span><span class="sxs-lookup"><span data-stu-id="08076-143">We will take advantage of a feature in Entity Framework which adds "seed" data to a newly created database.</span></span> <span data-ttu-id="08076-144">Isso preencherá previamente nosso catálogo de repositório com uma lista de gêneros, artistas e álbuns.</span><span class="sxs-lookup"><span data-stu-id="08076-144">This will pre-populate our store catalog with a list of Genres, Artists, and Albums.</span></span> <span data-ttu-id="08076-145">O download de MvcMusicStore Assets.zip - que nosso site design de arquivos incluídos usados neste tutorial - tem um arquivo de classe com esses dados semente, localizados em uma pasta denominada de código.</span><span class="sxs-lookup"><span data-stu-id="08076-145">The MvcMusicStore-Assets.zip download - which included our site design files used earlier in this tutorial - has a class file with this seed data, located in a folder named Code.</span></span>

<span data-ttu-id="08076-146">Dentro do código de pasta de modelos, localize o arquivo SampleData.cs e solte-o para a pasta de modelos em nosso projeto, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="08076-146">Within the Code / Models folder, locate the SampleData.cs file and drop it into the Models folder in our project, as shown below.</span></span>

![](mvc-music-store-part-4/_static/image4.png)

<span data-ttu-id="08076-147">Agora, precisamos adicionar uma linha de código para informar o Entity Framework sobre classe SampleData.</span><span class="sxs-lookup"><span data-stu-id="08076-147">Now we need to add one line of code to tell Entity Framework about that SampleData class.</span></span> <span data-ttu-id="08076-148">Clique duas vezes no arquivo global asax na raiz do projeto para abri-lo e adicione a seguinte linha à parte superior do aplicativo\_método Start.</span><span class="sxs-lookup"><span data-stu-id="08076-148">Double-click on the Global.asax file in the root of the project to open it and add the following line to the top the Application\_Start method.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

<span data-ttu-id="08076-149">Neste ponto, o trabalho necessário para configurar o Entity Framework para nosso projeto foram concluídas.</span><span class="sxs-lookup"><span data-stu-id="08076-149">At this point, we've completed the work necessary to configure Entity Framework for our project.</span></span>

## <a name="querying-the-database"></a><span data-ttu-id="08076-150">Consultando o banco de dados</span><span class="sxs-lookup"><span data-stu-id="08076-150">Querying the Database</span></span>

<span data-ttu-id="08076-151">Agora vamos atualizar nossos StoreController para que em vez de usar "fictício dados" em vez disso, chame em nosso banco de dados para consultar todas as suas informações.</span><span class="sxs-lookup"><span data-stu-id="08076-151">Now let's update our StoreController so that instead of using "dummy data" it instead calls into our database to query all of its information.</span></span> <span data-ttu-id="08076-152">Vamos começar declarando um campo no **StoreController** para manter uma instância da classe MusicStoreEntities, denominada storeDB:</span><span class="sxs-lookup"><span data-stu-id="08076-152">We'll start by declaring a field on the **StoreController** to hold an instance of the MusicStoreEntities class, named storeDB:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a><span data-ttu-id="08076-153">Atualizando o índice de repositório para consultar o banco de dados</span><span class="sxs-lookup"><span data-stu-id="08076-153">Updating the Store Index to query the database</span></span>

<span data-ttu-id="08076-154">A classe MusicStoreEntities é mantida pelo Entity Framework e expõe uma propriedade de coleção para cada tabela em nosso banco de dados.</span><span class="sxs-lookup"><span data-stu-id="08076-154">The MusicStoreEntities class is maintained by the Entity Framework and exposes a collection property for each table in our database.</span></span> <span data-ttu-id="08076-155">Vamos atualizar ação de índice do nosso StoreController para recuperar todos os gêneros em nosso banco de dados.</span><span class="sxs-lookup"><span data-stu-id="08076-155">Let's update our StoreController's Index action to retrieve all Genres in our database.</span></span> <span data-ttu-id="08076-156">Anteriormente fizemos isso por codificar dados de cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="08076-156">Previously we did this by hard-coding string data.</span></span> <span data-ttu-id="08076-157">Agora podemos em vez disso, usar apenas o contexto do Entity Framework Generes coleção:</span><span class="sxs-lookup"><span data-stu-id="08076-157">Now we can instead just use the Entity Framework context Generes collection:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

<span data-ttu-id="08076-158">Nenhuma alteração precisa ocorrer ao nosso modelo de exibição, pois ainda estamos retornando o mesmo StoreIndexViewModel são retornados antes - estamos apenas retornando dados dinâmicos do nosso banco de dados agora.</span><span class="sxs-lookup"><span data-stu-id="08076-158">No changes need to happen to our View template since we're still returning the same StoreIndexViewModel we returned before - we're just returning live data from our database now.</span></span>

<span data-ttu-id="08076-159">Quando executar o projeto novamente e visita a URL "/ armazenar", agora veremos uma lista de todos os gêneros em nosso banco de dados:</span><span class="sxs-lookup"><span data-stu-id="08076-159">When we run the project again and visit the "/Store" URL, we'll now see a list of all Genres in our database:</span></span>

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a><span data-ttu-id="08076-160">Navegue de repositório de atualização e detalhes para usar dados dinâmicos</span><span class="sxs-lookup"><span data-stu-id="08076-160">Updating Store Browse and Details to use live data</span></span>

<span data-ttu-id="08076-161">Com o repositório/procurar? gênero =*[alguns gênero]* método de ação, estamos pesquisando um gênero por nome.</span><span class="sxs-lookup"><span data-stu-id="08076-161">With the /Store/Browse?genre=*[some-genre]* action method, we're searching for a Genre by name.</span></span> <span data-ttu-id="08076-162">Esperamos que apenas um resultado, desde que não deve ter duas entradas para o mesmo nome de gênero e, portanto, podemos usar o. Extensão de Single() em LINQ para consultar o objeto apropriado de gênero assim (não digite isso ainda):</span><span class="sxs-lookup"><span data-stu-id="08076-162">We only expect one result, since we shouldn't ever have two entries for the same Genre name, and so we can use the .Single() extension in LINQ to query for the appropriate Genre object like this (don't type this yet):</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

<span data-ttu-id="08076-163">O único método usa uma expressão Lambda como um parâmetro que especifica que desejamos um único objeto de gênero, de modo que seu nome corresponde ao valor que definimos.</span><span class="sxs-lookup"><span data-stu-id="08076-163">The Single method takes a Lambda expression as a parameter, which specifies that we want a single Genre object such that its name matches the value we've defined.</span></span> <span data-ttu-id="08076-164">Nesse caso, estamos carregando um único objeto de gênero com um valor de nome correspondente de Disco.</span><span class="sxs-lookup"><span data-stu-id="08076-164">In the case above, we are loading a single Genre object with a Name value matching Disco.</span></span>

<span data-ttu-id="08076-165">Vamos dar um recurso do Entity Framework que permite indicar outras entidades relacionadas que desejamos também carregados quando o objeto de gênero é recuperado.</span><span class="sxs-lookup"><span data-stu-id="08076-165">We'll take advantage of an Entity Framework feature that allows us to indicate other related entities we want loaded as well when the Genre object is retrieved.</span></span> <span data-ttu-id="08076-166">Esse recurso é chamado de formatação de resultados de consulta e nos permite reduzir o número de vezes que é necessário para acessar o banco de dados para recuperar todas as informações que necessárias.</span><span class="sxs-lookup"><span data-stu-id="08076-166">This feature is called Query Result Shaping, and enables us to reduce the number of times we need to access the database to retrieve all of the information we need.</span></span> <span data-ttu-id="08076-167">Queremos a buscar previamente álbuns de gênero recuperamos, portanto vamos atualizar a consulta para incluir em Genres.Include("Albums") para indicar que desejamos também álbuns relacionados.</span><span class="sxs-lookup"><span data-stu-id="08076-167">We want to pre-fetch the Albums for Genre we retrieve, so we'll update our query to include from Genres.Include("Albums") to indicate that we want related albums as well.</span></span> <span data-ttu-id="08076-168">Isso é mais eficiente, pois ela recuperará dados nossos gênero e álbum na solicitação de um único banco de dados.</span><span class="sxs-lookup"><span data-stu-id="08076-168">This is more efficient, since it will retrieve both our Genre and Album data in a single database request.</span></span>

<span data-ttu-id="08076-169">Com explicações de lado, é como nosso ação do controlador procurar atualizada:</span><span class="sxs-lookup"><span data-stu-id="08076-169">With the explanations out of the way, here's how our updated Browse controller action looks:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

<span data-ttu-id="08076-170">Agora podemos atualizar a exibição do repositório de procurar para exibir os álbuns que estão disponíveis em cada gênero.</span><span class="sxs-lookup"><span data-stu-id="08076-170">We can now update the Store Browse View to display the albums which are available in each Genre.</span></span> <span data-ttu-id="08076-171">Abra o modelo de exibição (localizado em /Views/Store/Browse.cshtml) e adicione uma lista com marcadores de álbuns, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="08076-171">Open the view template (found in /Views/Store/Browse.cshtml) and add a bulleted list of Albums as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

<span data-ttu-id="08076-172">Executando o nosso aplicativo e navegando até o repositório/procurar? gênero = mostra Jazz que nossos resultados agora são sendo extraídos do banco de dados, exibir álbuns de todos os nossos gênero selecionado.</span><span class="sxs-lookup"><span data-stu-id="08076-172">Running our application and browsing to /Store/Browse?genre=Jazz shows that our results are now being pulled from the database, displaying all albums in our selected Genre.</span></span>

![](mvc-music-store-part-4/_static/image2.jpg)

<span data-ttu-id="08076-173">Verifique o mesmo alterar para nosso /Store/detalhes / [id] URL e substitua nossos dados fictícios com uma consulta de banco de dados que carrega um álbum cuja ID corresponde ao valor do parâmetro.</span><span class="sxs-lookup"><span data-stu-id="08076-173">We'll make the same change to our /Store/Details/[id] URL, and replace our dummy data with a database query which loads an Album whose ID matches the parameter value.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

<span data-ttu-id="08076-174">Executando o nosso aplicativo e navegando até /Store/Details/1 mostram que nossos resultados agora são sendo extraídos do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="08076-174">Running our application and browsing to /Store/Details/1 shows that our results are now being pulled from the database.</span></span>

![](mvc-music-store-part-4/_static/image5.png)

<span data-ttu-id="08076-175">Agora que nossa página de detalhes do repositório está configurada para exibir um álbum pela ID de álbum, vamos atualizar o **procurar** link para o modo de exibição de detalhes no modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="08076-175">Now that our Store Details page is set up to display an album by the Album ID, let's update the **Browse** view to link to the Details view.</span></span> <span data-ttu-id="08076-176">Usaremos ActionLink, exatamente como foi feito para link do índice de repositório para procurar repositório no final da seção anterior.</span><span class="sxs-lookup"><span data-stu-id="08076-176">We will use Html.ActionLink, exactly as we did to link from Store Index to Store Browse at the end of the previous section.</span></span> <span data-ttu-id="08076-177">A fonte completa para o modo de exibição do navegador é exibida abaixo.</span><span class="sxs-lookup"><span data-stu-id="08076-177">The complete source for the Browse view appears below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

<span data-ttu-id="08076-178">Agora estamos capazes de navegar em nossa página de armazenamento para uma página de gênero, que lista os álbuns disponíveis, e clicando em um álbum, pode exibir detalhes de álbum.</span><span class="sxs-lookup"><span data-stu-id="08076-178">We're now able to browse from our Store page to a Genre page, which lists the available albums, and by clicking on an album we can view details for that album.</span></span>

![](mvc-music-store-part-4/_static/image6.png)

> [!div class="step-by-step"]
> <span data-ttu-id="08076-179">[Anterior](mvc-music-store-part-3.md)
> [Próximo](mvc-music-store-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="08076-179">[Previous](mvc-music-store-part-3.md)
[Next](mvc-music-store-part-5.md)</span></span>
