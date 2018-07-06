---
uid: web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
title: Criando e usando um auxiliar em uma Web do ASP.NET (Razor) sites de páginas | Microsoft Docs
author: tfitzmac
description: Este artigo descreve como criar um auxiliar em um site de páginas da Web do ASP.NET (Razor). Um auxiliar é um componente reutilizável que inclui código e marcação para perf...
ms.author: aspnetcontent
ms.date: 02/17/2014
ms.assetid: 46bff772-01e0-40f0-9ae6-9e18c5442ee6
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: c2edc10e01f4d2358dd0b0bdf450348f01eb2bfa
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811893"
---
<a name="creating-and-using-a-helper-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="bc012-104">Criando e usando um auxiliar em um Site do ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="bc012-104">Creating and Using a Helper in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="bc012-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="bc012-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="bc012-106">Este artigo descreve como criar um auxiliar em um site de páginas da Web do ASP.NET (Razor).</span><span class="sxs-lookup"><span data-stu-id="bc012-106">This article describes how to create a helper in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="bc012-107">Um *auxiliar* é um componente reutilizável que inclui código e marcação para executar uma tarefa que pode ser entediante ou complexos.</span><span class="sxs-lookup"><span data-stu-id="bc012-107">A *helper* is a reusable component that includes code and markup to perform a task that might be tedious or complex.</span></span>
> 
> <span data-ttu-id="bc012-108">**O que você aprenderá:**</span><span class="sxs-lookup"><span data-stu-id="bc012-108">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="bc012-109">Como criar e usar um auxiliar simple.</span><span class="sxs-lookup"><span data-stu-id="bc012-109">How to create and use a simple helper.</span></span>
> 
> <span data-ttu-id="bc012-110">Estes são os recursos do ASP.NET introduzidos no artigo:</span><span class="sxs-lookup"><span data-stu-id="bc012-110">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="bc012-111">O `@helper` sintaxe.</span><span class="sxs-lookup"><span data-stu-id="bc012-111">The `@helper` syntax.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="bc012-112">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="bc012-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="bc012-113">Páginas da Web do ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="bc012-113">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="bc012-114">Este tutorial também funciona com ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="bc012-114">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="overview-of-helpers"></a><span data-ttu-id="bc012-115">Visão geral dos auxiliares</span><span class="sxs-lookup"><span data-stu-id="bc012-115">Overview of Helpers</span></span>

<span data-ttu-id="bc012-116">Se você precisar executar as mesmas tarefas em diferentes páginas em seu site, você pode usar um auxiliar.</span><span class="sxs-lookup"><span data-stu-id="bc012-116">If you need to perform the same tasks on different pages in your site, you can use a helper.</span></span> <span data-ttu-id="bc012-117">Páginas da Web ASP.NET inclui uma série de auxiliares, e há muito mais que você pode baixar e instalar.</span><span class="sxs-lookup"><span data-stu-id="bc012-117">ASP.NET Web Pages includes a number of helpers, and there are many more that you can download and install.</span></span> <span data-ttu-id="bc012-118">(Uma lista dos auxiliares internos em páginas da Web do ASP.NET está listada na [referência rápida de API ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202907).) Se nenhum dos auxiliares existentes atender às suas necessidades, você pode criar seu próprios auxiliar.</span><span class="sxs-lookup"><span data-stu-id="bc012-118">(A list of the built-in helpers in ASP.NET Web Pages is listed in the [ASP.NET API Quick Reference](https://go.microsoft.com/fwlink/?LinkId=202907).) If none of the existing helpers meet your needs, you can create your own helper.</span></span>

<span data-ttu-id="bc012-119">Um auxiliar permite que você use um bloco comum de código em várias páginas.</span><span class="sxs-lookup"><span data-stu-id="bc012-119">A helper lets you use a common block of code across multiple pages.</span></span> <span data-ttu-id="bc012-120">Suponha que em sua página você geralmente deseja criar um item de observação é definido, além de parágrafos normais.</span><span class="sxs-lookup"><span data-stu-id="bc012-120">Suppose that in your page you often want to create a note item that's set apart from normal paragraphs.</span></span> <span data-ttu-id="bc012-121">Talvez a anotação é criada como uma `<div>` elemento que tem o estilo como uma caixa com uma borda.</span><span class="sxs-lookup"><span data-stu-id="bc012-121">Perhaps the note is created as a `<div>` element that's styled as a box with a border.</span></span> <span data-ttu-id="bc012-122">Em vez de adicionar essa mesma marcação para uma página sempre que você deseja exibir uma anotação, você pode empacotar a marcação como um auxiliar.</span><span class="sxs-lookup"><span data-stu-id="bc012-122">Rather than add this same markup to a page every time you want to display a note, you can package the markup as a helper.</span></span> <span data-ttu-id="bc012-123">Em seguida, você pode inserir a anotação com uma única linha de código em qualquer lugar você precisa.</span><span class="sxs-lookup"><span data-stu-id="bc012-123">You can then insert the note with a single line of code anywhere you need it.</span></span>

<span data-ttu-id="bc012-124">Uso de um auxiliar, como isso torna o código em cada uma das suas páginas mais simples e fácil de ler.</span><span class="sxs-lookup"><span data-stu-id="bc012-124">Using a helper like this makes the code in each of your pages simpler and easier to read.</span></span> <span data-ttu-id="bc012-125">Ele também torna mais fácil de manter seu site, como se você precisar alterar a aparência de notas, você pode alterar a marcação em um só lugar.</span><span class="sxs-lookup"><span data-stu-id="bc012-125">It also makes it easier to maintain your site, because if you need to change how the notes look, you can change the markup in one place.</span></span>

## <a name="creating-a-helper"></a><span data-ttu-id="bc012-126">Criar um auxiliar</span><span class="sxs-lookup"><span data-stu-id="bc012-126">Creating a Helper</span></span>

<span data-ttu-id="bc012-127">Este procedimento mostra como criar o auxiliar que cria a observação, como acabamos de descrever.</span><span class="sxs-lookup"><span data-stu-id="bc012-127">This procedure shows you how to create the helper that creates the note, as just described.</span></span> <span data-ttu-id="bc012-128">Este é um exemplo simples, mas o auxiliar personalizado pode incluir qualquer marcação e código do ASP.NET que você precisa.</span><span class="sxs-lookup"><span data-stu-id="bc012-128">This is a simple example, but the custom helper can include any markup and ASP.NET code that you need.</span></span>

1. <span data-ttu-id="bc012-129">Na pasta raiz do site, crie uma pasta chamada *App\_código*.</span><span class="sxs-lookup"><span data-stu-id="bc012-129">In the root folder of the website, create a folder named *App\_Code*.</span></span> <span data-ttu-id="bc012-130">Isso é um nome de pasta reservado no ASP.NET, onde você pode colocar o código para componentes como auxiliares.</span><span class="sxs-lookup"><span data-stu-id="bc012-130">This is a reserved folder name in ASP.NET where you can put code for components like helpers.</span></span>
2. <span data-ttu-id="bc012-131">No *App\_código* pasta criar uma nova *. cshtml* de arquivo e nomeie-a *MyHelpers.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="bc012-131">In the *App\_Code* folder create a new *.cshtml* file and name it *MyHelpers.cshtml*.</span></span>
3. <span data-ttu-id="bc012-132">Substitua o conteúdo existente pelo seguinte:</span><span class="sxs-lookup"><span data-stu-id="bc012-132">Replace the existing content with the following:</span></span>

    [!code-cshtml[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    <span data-ttu-id="bc012-133">O código usa o `@helper` sintaxe para declarar um novo auxiliar denominado `MakeNote`.</span><span class="sxs-lookup"><span data-stu-id="bc012-133">The code uses the `@helper` syntax to declare a new helper named `MakeNote`.</span></span> <span data-ttu-id="bc012-134">Esse auxiliar particular permite que você passe um parâmetro chamado `content` que pode conter uma combinação de texto e marcação.</span><span class="sxs-lookup"><span data-stu-id="bc012-134">This particular helper lets you pass a parameter named `content` that can contain a combination of text and markup.</span></span> <span data-ttu-id="bc012-135">O auxiliar insere a cadeia de caracteres no corpo de Observação usando o `@content` variável.</span><span class="sxs-lookup"><span data-stu-id="bc012-135">The helper inserts the string into the note body using the `@content` variable.</span></span>

    <span data-ttu-id="bc012-136">Observe que o arquivo é denominado *MyHelpers.cshtml*, mas o auxiliar chamado `MakeNote`.</span><span class="sxs-lookup"><span data-stu-id="bc012-136">Notice that the file is named *MyHelpers.cshtml*, but the helper is named `MakeNote`.</span></span> <span data-ttu-id="bc012-137">Você pode colocar vários auxiliares personalizados em um único arquivo.</span><span class="sxs-lookup"><span data-stu-id="bc012-137">You can put multiple custom helpers into a single file.</span></span>
4. <span data-ttu-id="bc012-138">Salve e feche o arquivo.</span><span class="sxs-lookup"><span data-stu-id="bc012-138">Save and close the file.</span></span>

## <a name="using-the-helper-in-a-page"></a><span data-ttu-id="bc012-139">Usando o auxiliar em uma página</span><span class="sxs-lookup"><span data-stu-id="bc012-139">Using the Helper in a Page</span></span>

1. <span data-ttu-id="bc012-140">Na pasta raiz, crie um novo arquivo vazio chamado *TestHelper.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="bc012-140">In the root folder, create a new blank file called *TestHelper.cshtml*.</span></span>
2. <span data-ttu-id="bc012-141">Adicione o seguinte código ao arquivo:</span><span class="sxs-lookup"><span data-stu-id="bc012-141">Add the following code to the file:</span></span>

    [!code-html[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample2.html)]

    <span data-ttu-id="bc012-142">Para chamar o auxiliar que você criou, use `@` seguido pelo nome do arquivo onde é o auxiliar, um ponto e, em seguida, o nome do auxiliar.</span><span class="sxs-lookup"><span data-stu-id="bc012-142">To call the helper you created, use `@` followed by the file name where the helper is, a dot, and then the helper name.</span></span> <span data-ttu-id="bc012-143">(Se você tiver várias pastas na *App\_código* pasta, você poderá usar a sintaxe `@FolderName.FileName.HelperName` chamar o auxiliar em qualquer aninhados no nível de pasta).</span><span class="sxs-lookup"><span data-stu-id="bc012-143">(If you had multiple folders in the *App\_Code* folder, you could use the syntax `@FolderName.FileName.HelperName` to call your helper within any nested folder level).</span></span> <span data-ttu-id="bc012-144">O texto que você adicione aspas dentro dos parênteses é o texto que o auxiliar será exibido como parte da nota, na página da web.</span><span class="sxs-lookup"><span data-stu-id="bc012-144">The text that you add in quotation marks within the parentheses is the text that the helper will display as part of the note in the web page.</span></span>
3. <span data-ttu-id="bc012-145">Salve a página e executá-lo em um navegador.</span><span class="sxs-lookup"><span data-stu-id="bc012-145">Save the page and run it in a browser.</span></span> <span data-ttu-id="bc012-146">O auxiliar gera o item de Observação à direita no qual você chamou o auxiliar: entre dois parágrafos.</span><span class="sxs-lookup"><span data-stu-id="bc012-146">The helper generates the note item right where you called the helper: between the two paragraphs.</span></span>

    ![Captura de tela mostrando a página no navegador e como o auxiliar de geração de marcação que coloca uma caixa ao redor do texto especificado.](creating-and-using-a-helper-in-an-aspnet-web-pages-site/_static/image1.jpg)

## <a name="additional-resources"></a><span data-ttu-id="bc012-148">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="bc012-148">Additional Resources</span></span>


<span data-ttu-id="bc012-149">[Menu horizontal como um auxiliar de Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341).</span><span class="sxs-lookup"><span data-stu-id="bc012-149">[Horizontal menu as a Razor helper](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341).</span></span> <span data-ttu-id="bc012-150">Essa entrada de blog por Mike Pope mostra como criar um menu horizontal como um auxiliar usando marcação, CSS e código.</span><span class="sxs-lookup"><span data-stu-id="bc012-150">This blog entry by Mike Pope shows how to create a horizontal menu as a helper using markup, CSS, and code.</span></span>

<span data-ttu-id="bc012-151">[Aproveitar o HTML5 na Web do ASP.NET páginas auxiliares para o WebMatrix e do ASP.NET MVC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx).</span><span class="sxs-lookup"><span data-stu-id="bc012-151">[Leveraging HTML5 in ASP.NET Web Pages Helpers for WebMatrix and ASP.NET MVC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx).</span></span> <span data-ttu-id="bc012-152">Essa entrada de blog por Sam Abraham mostra um auxiliar que renderiza um HTML5 `Canvas` elemento.</span><span class="sxs-lookup"><span data-stu-id="bc012-152">This blog entry by Sam Abraham shows a helper that renders an HTML5 `Canvas` element.</span></span>

<span data-ttu-id="bc012-153">[A diferença entre @Helpers e @Functions no WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix).</span><span class="sxs-lookup"><span data-stu-id="bc012-153">[The Difference Between @Helpers and @Functions in WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix).</span></span> <span data-ttu-id="bc012-154">Descreve essa entrada de blog por Mike Brind `@helper` sintaxe e `@function` sintaxe e quando usar cada um.</span><span class="sxs-lookup"><span data-stu-id="bc012-154">This blog entry by Mike Brind describes `@helper` syntax and `@function` syntax and when to use each.</span></span>
