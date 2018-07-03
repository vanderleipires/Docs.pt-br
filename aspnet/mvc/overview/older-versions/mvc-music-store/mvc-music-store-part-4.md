---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: 'Parte 4: Acesso a dados e modelos | Microsoft Docs'
author: jongalloway
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo de Store de música do ASP.NET MVC. Parte 4 aborda o acesso a dados e modelos.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: ea8fe623a1b59b80fd7f087036b9ed716eafadbe
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402032"
---
<a name="part-4-models-and-data-access"></a><span data-ttu-id="fdf3e-104">Parte 4: Modelos e acesso a dados</span><span class="sxs-lookup"><span data-stu-id="fdf3e-104">Part 4: Models and Data Access</span></span>
====================
<span data-ttu-id="fdf3e-105">por [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="fdf3e-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="fdf3e-106">A Store de música do MVC é um aplicativo tutorial que apresenta e explica passo a passo de como usar o ASP.NET MVC e o Visual Studio para desenvolvimento da web.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="fdf3e-107">A Store de música do MVC é uma implementação de repositório de exemplo leve que vende álbuns de música online e implementa a administração de site básico, entrada do usuário e a funcionalidade de carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>
> 
> <span data-ttu-id="fdf3e-108">Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo de Store de música do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="fdf3e-109">Parte 4 aborda o acesso a dados e modelos.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-109">Part 4 covers Models and Data Access.</span></span>


<span data-ttu-id="fdf3e-110">Até agora, podemos tiver apenas foi passando "dados fictícios" de nossos controladores para nossos modelos de exibição.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-110">So far, we've just been passing "dummy data" from our Controllers to our View templates.</span></span> <span data-ttu-id="fdf3e-111">Agora estamos prontos para ligar a um banco de dados real.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-111">Now we're ready to hook up a real database.</span></span> <span data-ttu-id="fdf3e-112">Neste tutorial falaremos sobre como usar o SQL Server Compact Edition (geralmente chamado de SQL CE) como nosso mecanismo de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-112">In this tutorial we'll be covering how to use SQL Server Compact Edition (often called SQL CE) as our database engine.</span></span> <span data-ttu-id="fdf3e-113">SQL CE é um banco de dados de arquivo incorporado, gratuito, com base em que não requer qualquer instalação ou configuração, o que torna muito conveniente para o desenvolvimento local.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-113">SQL CE is a free, embedded, file based database that doesn't require any installation or configuration, which makes it really convenient for local development.</span></span>

## <a name="database-access-with-entity-framework-code-first"></a><span data-ttu-id="fdf3e-114">Acesso de banco de dados com o Entity Framework Code-First</span><span class="sxs-lookup"><span data-stu-id="fdf3e-114">Database access with Entity Framework Code-First</span></span>

<span data-ttu-id="fdf3e-115">Vamos usar o suporte do Entity Framework (EF) que é incluído em projetos do ASP.NET MVC 3 para consultar e atualizar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-115">We'll use the Entity Framework (EF) support that is included in ASP.NET MVC 3 projects to query and update the database.</span></span> <span data-ttu-id="fdf3e-116">O EF é uma API que permite aos desenvolvedores consultar e atualizar dados armazenados em um banco de dados de forma orientada a objeto de dados do (ORM) de mapeamento relacional de objeto flexível.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-116">EF is a flexible object relational mapping (ORM) data API that enables developers to query and update data stored in a database in an object-oriented way.</span></span>

<span data-ttu-id="fdf3e-117">Entity Framework versão 4 dá suporte a um paradigma de desenvolvimento, primeiro código de chamada.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-117">Entity Framework version 4 supports a development paradigm called code-first.</span></span> <span data-ttu-id="fdf3e-118">Primeiro de código permite que você crie o objeto de modelo ao escrever classes simples (também conhecido como POCO de objetos CLR "bom e velho") e pode até mesmo criar o banco de dados em tempo real de suas classes.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-118">Code-first allows you to create model object by writing simple classes (also known as POCO from "plain-old" CLR objects), and can even create the database on the fly from your classes.</span></span>

### <a name="changes-to-our-model-classes"></a><span data-ttu-id="fdf3e-119">Alterações em nossas Classes de modelo</span><span class="sxs-lookup"><span data-stu-id="fdf3e-119">Changes to our Model Classes</span></span>

<span data-ttu-id="fdf3e-120">Vamos aproveitar o recurso de criação de banco de dados no Entity Framework neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-120">We will be leveraging the database creation feature in Entity Framework in this tutorial.</span></span> <span data-ttu-id="fdf3e-121">Antes de fazer isso, no entanto, vamos fazer algumas pequenas alterações nossas classes de modelo para adicionar algumas coisas que usaremos mais tarde.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-121">Before we do that, though, let's make a few minor changes to our model classes to add in some things we'll be using later on.</span></span>

#### <a name="adding-the-artist-model-classes"></a><span data-ttu-id="fdf3e-122">Adicionando Classes de modelo do artista</span><span class="sxs-lookup"><span data-stu-id="fdf3e-122">Adding the Artist Model Classes</span></span>

<span data-ttu-id="fdf3e-123">Nosso álbuns será associados com artistas, portanto, vamos adicionar uma classe de modelo simples para descrever um artista.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-123">Our Albums will be associated with Artists, so we'll add a simple model class to describe an Artist.</span></span> <span data-ttu-id="fdf3e-124">Adicione uma nova classe para a pasta modelos chamada Artist.cs usando o código mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-124">Add a new class to the Models folder named Artist.cs using the code shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a><span data-ttu-id="fdf3e-125">Atualizando a nossas Classes de modelo</span><span class="sxs-lookup"><span data-stu-id="fdf3e-125">Updating our Model Classes</span></span>

<span data-ttu-id="fdf3e-126">Atualize a classe de álbum, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-126">Update the Album class as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

<span data-ttu-id="fdf3e-127">Em seguida, faça as seguintes atualizações para a classe de gênero.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-127">Next, make the following updates to the Genre class.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-appdata-folder"></a><span data-ttu-id="fdf3e-128">Adicionar o aplicativo\_pasta de dados</span><span class="sxs-lookup"><span data-stu-id="fdf3e-128">Adding the App\_Data folder</span></span>

<span data-ttu-id="fdf3e-129">Vamos adicionar um aplicativo\_diretório de dados ao nosso projeto para manter nossos arquivos de banco de dados SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-129">We'll add an App\_Data directory to our project to hold our SQL Server Express database files.</span></span> <span data-ttu-id="fdf3e-130">Aplicativo\_dados são um diretório especial no ASP.NET que já tem as permissões de acesso de segurança correto para acesso ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-130">App\_Data is a special directory in ASP.NET which already has the correct security access permissions for database access.</span></span> <span data-ttu-id="fdf3e-131">No menu projeto, selecione Adicionar pasta ASP.NET e, em seguida, aplicativo\_dados.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-131">From the Project menu, select Add ASP.NET Folder, then App\_Data.</span></span>

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a><span data-ttu-id="fdf3e-132">Criando uma cadeia de Conexão no arquivo Web. config</span><span class="sxs-lookup"><span data-stu-id="fdf3e-132">Creating a Connection String in the web.config file</span></span>

<span data-ttu-id="fdf3e-133">Adicionaremos algumas linhas ao arquivo de configuração do site para que o Entity Framework saiba como se conectar ao nosso banco de dados.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-133">We will add a few lines to the website's configuration file so that Entity Framework knows how to connect to our database.</span></span> <span data-ttu-id="fdf3e-134">Clique duas vezes no arquivo Web. config localizado na raiz do projeto.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-134">Double-click on the Web.config file located in the root of the project.</span></span>

![](mvc-music-store-part-4/_static/image2.png)

<span data-ttu-id="fdf3e-135">Role até o final desse arquivo e adicione uma &lt;connectionStrings&gt; seção diretamente acima da última linha, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-135">Scroll to the bottom of this file and add a &lt;connectionStrings&gt; section directly above the last line, as shown below.</span></span>

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a><span data-ttu-id="fdf3e-136">Adicionando uma classe de contexto</span><span class="sxs-lookup"><span data-stu-id="fdf3e-136">Adding a Context Class</span></span>

<span data-ttu-id="fdf3e-137">Clique com botão direito na pasta modelos e adicione uma nova classe chamada MusicStoreEntities.cs.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-137">Right-click the Models folder and add a new class named MusicStoreEntities.cs.</span></span>

![](mvc-music-store-part-4/_static/image3.png)

<span data-ttu-id="fdf3e-138">Essa classe será representar o contexto de banco de dados do Entity Framework e será lidar com nosso criar, ler, atualizar e excluir operações para nós.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-138">This class will represent the Entity Framework database context, and will handle our create, read, update, and delete operations for us.</span></span> <span data-ttu-id="fdf3e-139">O código para essa classe é mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-139">The code for this class is shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

<span data-ttu-id="fdf3e-140">É isso – não há nenhuma outra configuração, interfaces especiais, etc. Estendendo a classe base DbContext, nossa classe MusicStoreEntities é capaz de lidar com nossas operações de banco de dados para nós.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-140">That's it - there's no other configuration, special interfaces, etc. By extending the DbContext base class, our MusicStoreEntities class is able to handle our database operations for us.</span></span> <span data-ttu-id="fdf3e-141">Agora que temos que ligado, vamos adicionar mais algumas propriedades para nossas classes de modelo para aproveitar algumas das informações adicionais em nosso banco de dados.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-141">Now that we've got that hooked up, let's add a few more properties to our model classes to take advantage of some of the additional information in our database.</span></span>

### <a name="adding-our-store-catalog-data"></a><span data-ttu-id="fdf3e-142">Adição de nossos dados de catálogo do repositório</span><span class="sxs-lookup"><span data-stu-id="fdf3e-142">Adding our store catalog data</span></span>

<span data-ttu-id="fdf3e-143">Obtemos proveito de um recurso no Entity Framework, que adiciona dados de "semente" para um banco de dados recém-criado.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-143">We will take advantage of a feature in Entity Framework which adds "seed" data to a newly created database.</span></span> <span data-ttu-id="fdf3e-144">Isso irá preencher previamente o nosso catálogo de repositório com uma lista dos álbuns, artistas e gêneros.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-144">This will pre-populate our store catalog with a list of Genres, Artists, and Albums.</span></span> <span data-ttu-id="fdf3e-145">O download de MvcMusicStore Assets.zip - que incluía nossos arquivos de design de site usados neste tutorial - tem um arquivo de classe com esses dados de semente, localizados em uma pasta chamada código.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-145">The MvcMusicStore-Assets.zip download - which included our site design files used earlier in this tutorial - has a class file with this seed data, located in a folder named Code.</span></span>

<span data-ttu-id="fdf3e-146">Dentro do código / pasta de modelos, localize o arquivo SampleData.cs e solte-o para a pasta de modelos em nosso projeto, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-146">Within the Code / Models folder, locate the SampleData.cs file and drop it into the Models folder in our project, as shown below.</span></span>

![](mvc-music-store-part-4/_static/image4.png)

<span data-ttu-id="fdf3e-147">Agora, precisamos adicionar uma linha de código para informar ao Entity Framework sobre essa classe SampleData.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-147">Now we need to add one line of code to tell Entity Framework about that SampleData class.</span></span> <span data-ttu-id="fdf3e-148">Clique duas vezes no arquivo global asax na raiz do projeto para abri-lo e adicione a seguinte linha à parte superior do aplicativo\_método Start.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-148">Double-click on the Global.asax file in the root of the project to open it and add the following line to the top the Application\_Start method.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

<span data-ttu-id="fdf3e-149">Neste ponto, concluímos o trabalho necessário para configurar o Entity Framework para nosso projeto.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-149">At this point, we've completed the work necessary to configure Entity Framework for our project.</span></span>

## <a name="querying-the-database"></a><span data-ttu-id="fdf3e-150">Consultando o banco de dados</span><span class="sxs-lookup"><span data-stu-id="fdf3e-150">Querying the Database</span></span>

<span data-ttu-id="fdf3e-151">Agora vamos atualizar nossos StoreController para que em vez de usar "fictício dados" em vez disso, chame em nosso banco de dados para consultar todas as suas informações.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-151">Now let's update our StoreController so that instead of using "dummy data" it instead calls into our database to query all of its information.</span></span> <span data-ttu-id="fdf3e-152">Vamos começar com a declaração de um campo na **StoreController** para manter uma instância da classe MusicStoreEntities, chamada storeDB:</span><span class="sxs-lookup"><span data-stu-id="fdf3e-152">We'll start by declaring a field on the **StoreController** to hold an instance of the MusicStoreEntities class, named storeDB:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a><span data-ttu-id="fdf3e-153">Atualizar o índice da Store para consultar o banco de dados</span><span class="sxs-lookup"><span data-stu-id="fdf3e-153">Updating the Store Index to query the database</span></span>

<span data-ttu-id="fdf3e-154">A classe MusicStoreEntities é mantida pelo Entity Framework e expõe uma propriedade de coleção para cada tabela no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-154">The MusicStoreEntities class is maintained by the Entity Framework and exposes a collection property for each table in our database.</span></span> <span data-ttu-id="fdf3e-155">Vamos atualizar ação de índice do nosso StoreController para recuperar todos os gêneros em nosso banco de dados.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-155">Let's update our StoreController's Index action to retrieve all Genres in our database.</span></span> <span data-ttu-id="fdf3e-156">Anteriormente fizemos isso por codificar dados de cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-156">Previously we did this by hard-coding string data.</span></span> <span data-ttu-id="fdf3e-157">Agora, podemos em vez disso, basta usar o contexto do Entity Framework Generes coleção:</span><span class="sxs-lookup"><span data-stu-id="fdf3e-157">Now we can instead just use the Entity Framework context Generes collection:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

<span data-ttu-id="fdf3e-158">Nenhuma alteração precisa acontecer ao nosso modelo de exibição, pois ainda estamos retornando o mesmo StoreIndexViewModel nós retornados antes - estamos apenas retornando dados ao vivo do nosso banco de dados agora.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-158">No changes need to happen to our View template since we're still returning the same StoreIndexViewModel we returned before - we're just returning live data from our database now.</span></span>

<span data-ttu-id="fdf3e-159">Quando executar o projeto novamente e acesse a URL "/ Store", podemos agora verá uma lista de todos os gêneros em nosso banco de dados:</span><span class="sxs-lookup"><span data-stu-id="fdf3e-159">When we run the project again and visit the "/Store" URL, we'll now see a list of all Genres in our database:</span></span>

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a><span data-ttu-id="fdf3e-160">Atualizando Store procurar e detalhes para usar dados dinâmicos</span><span class="sxs-lookup"><span data-stu-id="fdf3e-160">Updating Store Browse and Details to use live data</span></span>

<span data-ttu-id="fdf3e-161">Com/Store/procurar? genre =*[alguns gênero]* método de ação, procurando por um gênero por nome.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-161">With the /Store/Browse?genre=*[some-genre]* action method, we're searching for a Genre by name.</span></span> <span data-ttu-id="fdf3e-162">Esperamos que apenas um resultado, já que não deve nunca temos duas entradas para o mesmo nome de gênero e, portanto, podemos usar o. Extensão de Single () em LINQ para consultar o objeto de gênero apropriado como esta (não digite isso ainda):</span><span class="sxs-lookup"><span data-stu-id="fdf3e-162">We only expect one result, since we shouldn't ever have two entries for the same Genre name, and so we can use the .Single() extension in LINQ to query for the appropriate Genre object like this (don't type this yet):</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

<span data-ttu-id="fdf3e-163">O método único usa uma expressão Lambda como um parâmetro que especifica o que queremos que um único objeto de gênero, de modo que seu nome corresponde ao valor que definimos.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-163">The Single method takes a Lambda expression as a parameter, which specifies that we want a single Genre object such that its name matches the value we've defined.</span></span> <span data-ttu-id="fdf3e-164">No caso acima, estamos carregando um único objeto de gênero com um valor de nome correspondente de Disco.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-164">In the case above, we are loading a single Genre object with a Name value matching Disco.</span></span>

<span data-ttu-id="fdf3e-165">Vamos dar vantagem de um recurso do Entity Framework que permite indicar a outras entidades relacionadas que queremos também carregados quando o objeto de gênero é recuperado.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-165">We'll take advantage of an Entity Framework feature that allows us to indicate other related entities we want loaded as well when the Genre object is retrieved.</span></span> <span data-ttu-id="fdf3e-166">Esse recurso é chamado de Shaping de resultado de consulta e nos permite reduzir o número de vezes que é necessário para acessar o banco de dados para recuperar todas as informações que necessárias.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-166">This feature is called Query Result Shaping, and enables us to reduce the number of times we need to access the database to retrieve all of the information we need.</span></span> <span data-ttu-id="fdf3e-167">Queremos que a buscar previamente os álbuns de gênero recuperamos, portanto, atualizaremos nossa consulta para incluir em Genres.Include("Albums") para indicar que desejamos álbuns relacionados também.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-167">We want to pre-fetch the Albums for Genre we retrieve, so we'll update our query to include from Genres.Include("Albums") to indicate that we want related albums as well.</span></span> <span data-ttu-id="fdf3e-168">Isso é mais eficiente, porque ele irá recuperar dados de nosso gênero e do álbum em uma solicitação de banco de dados individual.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-168">This is more efficient, since it will retrieve both our Genre and Album data in a single database request.</span></span>

<span data-ttu-id="fdf3e-169">Com as explicações fora do caminho, esta é a aparência de nossa ação de controlador procurar atualizada:</span><span class="sxs-lookup"><span data-stu-id="fdf3e-169">With the explanations out of the way, here's how our updated Browse controller action looks:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

<span data-ttu-id="fdf3e-170">Agora podemos atualizar a exibição do Store Procurar para exibir os álbuns que estão disponíveis em cada gênero.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-170">We can now update the Store Browse View to display the albums which are available in each Genre.</span></span> <span data-ttu-id="fdf3e-171">Abra o modelo de exibição (encontrado na /Views/Store/Browse.cshtml) e adicione uma lista com marcadores de álbuns, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-171">Open the view template (found in /Views/Store/Browse.cshtml) and add a bulleted list of Albums as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

<span data-ttu-id="fdf3e-172">Executar o aplicativo e navegando até/Store/procurar? genre = mostra Jazz nossos resultados agora estão sendo extraídos do banco de dados, exibindo todos os álbuns em nosso gênero selecionado.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-172">Running our application and browsing to /Store/Browse?genre=Jazz shows that our results are now being pulled from the database, displaying all albums in our selected Genre.</span></span>

![](mvc-music-store-part-4/_static/image2.jpg)

<span data-ttu-id="fdf3e-173">Podemos fazer o mesmo alterar para nossa URL de /Store/detalhes / [id] e substituir nossos dados fictícios com uma consulta de banco de dados que carrega um álbum cuja identificação corresponde ao valor do parâmetro.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-173">We'll make the same change to our /Store/Details/[id] URL, and replace our dummy data with a database query which loads an Album whose ID matches the parameter value.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

<span data-ttu-id="fdf3e-174">Executar o aplicativo e navegando até /Store/Details/1 mostram que nossos resultados agora estão sendo extraídos do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-174">Running our application and browsing to /Store/Details/1 shows that our results are now being pulled from the database.</span></span>

![](mvc-music-store-part-4/_static/image5.png)

<span data-ttu-id="fdf3e-175">Agora que nossa página de detalhes de Store é configurada para exibir um álbum pela ID do álbum, vamos atualizar o **procurar** modo de exibição para vincular a exibição de detalhes.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-175">Now that our Store Details page is set up to display an album by the Album ID, let's update the **Browse** view to link to the Details view.</span></span> <span data-ttu-id="fdf3e-176">Nós usaremos o HTML. ActionLink, exatamente como fizemos para vincular de Store de índice para procurar Store no final da seção anterior.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-176">We will use Html.ActionLink, exactly as we did to link from Store Index to Store Browse at the end of the previous section.</span></span> <span data-ttu-id="fdf3e-177">A fonte completa para o modo de exibição do navegador é exibida abaixo.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-177">The complete source for the Browse view appears below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

<span data-ttu-id="fdf3e-178">Agora estamos capazes de navegar de nossa página da Store para uma página de gênero, que lista os álbuns disponíveis, e clicando em um álbum, podemos exibir detalhes do álbum.</span><span class="sxs-lookup"><span data-stu-id="fdf3e-178">We're now able to browse from our Store page to a Genre page, which lists the available albums, and by clicking on an album we can view details for that album.</span></span>

![](mvc-music-store-part-4/_static/image6.png)

> [!div class="step-by-step"]
> <span data-ttu-id="fdf3e-179">[Anterior](mvc-music-store-part-3.md)
> [Próximo](mvc-music-store-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="fdf3e-179">[Previous](mvc-music-store-part-3.md)
[Next](mvc-music-store-part-5.md)</span></span>
