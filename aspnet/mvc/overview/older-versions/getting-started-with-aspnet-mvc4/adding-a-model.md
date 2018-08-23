---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
title: Adicionando um modelo | Microsoft Docs
author: Rick-Anderson
description: 'Observação: Uma versão atualizada deste tutorial está disponível aqui que usa o ASP.NET MVC 5 e Visual Studio 2013. Ele é mais seguro e muito mais simples a seguir e demonstração...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 53db72da-e0b9-44d9-b60b-6e6988c00b28
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: a7e732da3b056980c87660f6b80366438b5823c1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825008"
---
<a name="adding-a-model"></a><span data-ttu-id="15b68-104">Adicionando um modelo</span><span class="sxs-lookup"><span data-stu-id="15b68-104">Adding a Model</span></span>
====================
<span data-ttu-id="15b68-105">por [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="15b68-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="15b68-106">Uma versão atualizada deste tutorial está disponível [aqui](../../getting-started/introduction/getting-started.md) que usa o ASP.NET MVC 5 e Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="15b68-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="15b68-107">É mais seguro e muito mais simples a seguir e apresenta mais recursos.</span><span class="sxs-lookup"><span data-stu-id="15b68-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="15b68-108">Nesta seção, você adicionará algumas classes para o gerenciamento de filmes em um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="15b68-108">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="15b68-109">Essas classes serão a &quot;modelo&quot; faz parte do aplicativo ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="15b68-109">These classes will be the &quot;model&quot; part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="15b68-110">Você usará uma tecnologia de acesso a dados do .NET Framework conhecida como o [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) para definir e trabalhar com essas classes de modelo.</span><span class="sxs-lookup"><span data-stu-id="15b68-110">You'll use a .NET Framework data-access technology known as the [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) to define and work with these model classes.</span></span> <span data-ttu-id="15b68-111">A Entity Framework (também conhecido como EF) oferece suporte a um paradigma de desenvolvimento chamado *Code First*.</span><span class="sxs-lookup"><span data-stu-id="15b68-111">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="15b68-112">Código primeiro permite que você crie objetos de modelo ao escrever classes simples.</span><span class="sxs-lookup"><span data-stu-id="15b68-112">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="15b68-113">(Esses também são conhecidas como classes POCO, do &quot;objetos CLR de BOM e velho.&quot;) Em seguida, você pode ter o banco de dados criado em tempo real de suas classes, que permite que um fluxo de trabalho de desenvolvimento muito claro e rápido.</span><span class="sxs-lookup"><span data-stu-id="15b68-113">(These are also known as POCO classes, from &quot;plain-old CLR objects.&quot;) You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="15b68-114">Adicionando Classes de modelo</span><span class="sxs-lookup"><span data-stu-id="15b68-114">Adding Model Classes</span></span>

<span data-ttu-id="15b68-115">Na **Gerenciador de soluções**, clique com botão direito do *modelos* pasta, selecione **Add**e, em seguida, selecione **classe**.</span><span class="sxs-lookup"><span data-stu-id="15b68-115">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="15b68-116">Insira o *classe* nome &quot;filme&quot;.</span><span class="sxs-lookup"><span data-stu-id="15b68-116">Enter the *class* name &quot;Movie&quot;.</span></span>

<span data-ttu-id="15b68-117">Adicione as seguintes propriedades de cinco para o `Movie` classe:</span><span class="sxs-lookup"><span data-stu-id="15b68-117">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="15b68-118">Vamos usar o `Movie` classe para representar os filmes em um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="15b68-118">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="15b68-119">Cada instância de um `Movie` objeto corresponderá a uma linha em uma tabela de banco de dados e cada propriedade do `Movie` classe será mapeado para uma coluna na tabela.</span><span class="sxs-lookup"><span data-stu-id="15b68-119">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="15b68-120">No mesmo arquivo, adicione o seguinte `MovieDBContext` classe:</span><span class="sxs-lookup"><span data-stu-id="15b68-120">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

<span data-ttu-id="15b68-121">O `MovieDBContext` classe representa o contexto de banco de dados de filme do Entity Framework, que lida com a busca, armazenar e atualizar `Movie` instâncias em um banco de dados de classe.</span><span class="sxs-lookup"><span data-stu-id="15b68-121">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="15b68-122">O `MovieDBContext` deriva o `DbContext` fornecido pela estrutura de entidades de classe base.</span><span class="sxs-lookup"><span data-stu-id="15b68-122">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span>

<span data-ttu-id="15b68-123">Para poder referenciar `DbContext` e `DbSet`, você precisará adicionar o seguinte `using` instrução na parte superior do arquivo:</span><span class="sxs-lookup"><span data-stu-id="15b68-123">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="15b68-124">A conclusão *Movie.cs* arquivo é mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="15b68-124">The complete *Movie.cs* file is shown below.</span></span> <span data-ttu-id="15b68-125">(Várias instruções que não são necessários foram removidos.)</span><span class="sxs-lookup"><span data-stu-id="15b68-125">(Several using statements that are not needed have been removed.)</span></span>

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="15b68-126">Criando uma cadeia de Conexão e trabalhando com LocalDB do SQL Server</span><span class="sxs-lookup"><span data-stu-id="15b68-126">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="15b68-127">O `MovieDBContext` classe criada por você lida com a tarefa de conectar-se ao banco de dados e mapeamento `Movie` objetos para os registros de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="15b68-127">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="15b68-128">Uma pergunta que você pode fazer, no entanto, é como especificar qual banco de dados que ele se conectará.</span><span class="sxs-lookup"><span data-stu-id="15b68-128">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="15b68-129">Você terá de fazer isso adicionando as informações de conexão na *Web. config* arquivo do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="15b68-129">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="15b68-130">Abra a raiz do aplicativo *Web. config* arquivo.</span><span class="sxs-lookup"><span data-stu-id="15b68-130">Open the application root *Web.config* file.</span></span> <span data-ttu-id="15b68-131">(Não o *Web. config* arquivo na *modos de exibição* pasta.) Abra o *Web. config* arquivo descrito em vermelho.</span><span class="sxs-lookup"><span data-stu-id="15b68-131">(Not the *Web.config* file in the *Views* folder.) Open the *Web.config* file outlined in red.</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="15b68-132">Adicione a seguinte cadeia de conexão para o `<connectionStrings>` elemento na *Web. config* arquivo.</span><span class="sxs-lookup"><span data-stu-id="15b68-132">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="15b68-133">O exemplo a seguir mostra uma parte do *Web. config* arquivo com a cadeia de caracteres de conexão nova adicionada:</span><span class="sxs-lookup"><span data-stu-id="15b68-133">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml?highlight=6-9)]

<span data-ttu-id="15b68-134">Essa pequena quantidade de código e o XML é tudo o que precisa ser escrita para representar e armazenar os dados de filmes em um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="15b68-134">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="15b68-135">Em seguida, você criará um novo `MoviesController` classe que você pode usar para exibir os dados de filme e permitir que usuários criem novas listagens de filme.</span><span class="sxs-lookup"><span data-stu-id="15b68-135">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="15b68-136">[Anterior](adding-a-view.md)
> [Próximo](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="15b68-136">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
