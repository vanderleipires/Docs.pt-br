---
uid: web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
title: "Criando e usando um auxiliar em uma Web ASP.NET páginas Site (Razor) | Microsoft Docs"
author: tfitzmac
description: "Este artigo descreve como criar um auxiliar em um site de páginas da Web do ASP.NET (Razor). Um auxiliar é um componente reutilizável que inclui o código e a marcação para o desempenho..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 46bff772-01e0-40f0-9ae6-9e18c5442ee6
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 5d0c1ae09d8fbc91ff76cd4045d439abafee7736
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="creating-and-using-a-helper-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="39d0b-104">Criando e usando um auxiliar em um Site de páginas da Web (Razor) do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="39d0b-104">Creating and Using a Helper in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="39d0b-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="39d0b-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="39d0b-106">Este artigo descreve como criar um auxiliar em um site de páginas da Web do ASP.NET (Razor).</span><span class="sxs-lookup"><span data-stu-id="39d0b-106">This article describes how to create a helper in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="39d0b-107">Um *auxiliar* é um componente reutilizável que inclui o código e marcação para executar uma tarefa que pode ser entediante ou complexos.</span><span class="sxs-lookup"><span data-stu-id="39d0b-107">A *helper* is a reusable component that includes code and markup to perform a task that might be tedious or complex.</span></span>
> 
> <span data-ttu-id="39d0b-108">**O que você aprenderá:**</span><span class="sxs-lookup"><span data-stu-id="39d0b-108">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="39d0b-109">Como criar e usar um assistente simple.</span><span class="sxs-lookup"><span data-stu-id="39d0b-109">How to create and use a simple helper.</span></span>
> 
> <span data-ttu-id="39d0b-110">Estes são os recursos ASP.NET introduzidos no artigo:</span><span class="sxs-lookup"><span data-stu-id="39d0b-110">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="39d0b-111">O `@helper` sintaxe.</span><span class="sxs-lookup"><span data-stu-id="39d0b-111">The `@helper` syntax.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="39d0b-112">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="39d0b-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="39d0b-113">Páginas da Web do ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="39d0b-113">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="39d0b-114">Este tutorial também funciona com 2 de páginas da Web do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="39d0b-114">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="overview-of-helpers"></a><span data-ttu-id="39d0b-115">Visão geral de auxiliares</span><span class="sxs-lookup"><span data-stu-id="39d0b-115">Overview of Helpers</span></span>

<span data-ttu-id="39d0b-116">Se você precisar executar as mesmas tarefas em diferentes páginas em seu site, você pode usar um auxiliar.</span><span class="sxs-lookup"><span data-stu-id="39d0b-116">If you need to perform the same tasks on different pages in your site, you can use a helper.</span></span> <span data-ttu-id="39d0b-117">Páginas da Web ASP.NET inclui uma série de auxiliares, e há muito mais que você pode baixar e instalar.</span><span class="sxs-lookup"><span data-stu-id="39d0b-117">ASP.NET Web Pages includes a number of helpers, and there are many more that you can download and install.</span></span> <span data-ttu-id="39d0b-118">(Uma lista dos internas auxiliares em páginas da Web do ASP.NET está listada no [referência rápida de API ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202907).) Se nenhum dos auxiliares existentes atender às suas necessidades, você pode criar seu próprios auxiliar.</span><span class="sxs-lookup"><span data-stu-id="39d0b-118">(A list of the built-in helpers in ASP.NET Web Pages is listed in the [ASP.NET API Quick Reference](https://go.microsoft.com/fwlink/?LinkId=202907).) If none of the existing helpers meet your needs, you can create your own helper.</span></span>

<span data-ttu-id="39d0b-119">Um auxiliar permite que você use um bloco comum de código em várias páginas.</span><span class="sxs-lookup"><span data-stu-id="39d0b-119">A helper lets you use a common block of code across multiple pages.</span></span> <span data-ttu-id="39d0b-120">Suponha que em sua página você geralmente deseja criar um item de observação que é definido além de parágrafos normais.</span><span class="sxs-lookup"><span data-stu-id="39d0b-120">Suppose that in your page you often want to create a note item that's set apart from normal paragraphs.</span></span> <span data-ttu-id="39d0b-121">Talvez a anotação é criada como uma `<div>` elemento que tem o estilo como uma caixa com uma borda.</span><span class="sxs-lookup"><span data-stu-id="39d0b-121">Perhaps the note is created as a `<div>` element that's styled as a box with a border.</span></span> <span data-ttu-id="39d0b-122">Em vez de adicionar essa mesma marcação para uma página toda vez que você deseja exibir uma anotação, você pode empacotar a marcação como um auxiliar.</span><span class="sxs-lookup"><span data-stu-id="39d0b-122">Rather than add this same markup to a page every time you want to display a note, you can package the markup as a helper.</span></span> <span data-ttu-id="39d0b-123">Você pode inserir a anotação com uma única linha de código em qualquer local necessário.</span><span class="sxs-lookup"><span data-stu-id="39d0b-123">You can then insert the note with a single line of code anywhere you need it.</span></span>

<span data-ttu-id="39d0b-124">Usar um auxiliar de como isso torna o código em cada uma das suas páginas e facilitar a leitura.</span><span class="sxs-lookup"><span data-stu-id="39d0b-124">Using a helper like this makes the code in each of your pages simpler and easier to read.</span></span> <span data-ttu-id="39d0b-125">Ele também torna mais fácil manter seu site, como se você precisar alterar a aparência de notas, você pode alterar a marcação em um único local.</span><span class="sxs-lookup"><span data-stu-id="39d0b-125">It also makes it easier to maintain your site, because if you need to change how the notes look, you can change the markup in one place.</span></span>

## <a name="creating-a-helper"></a><span data-ttu-id="39d0b-126">Criando um auxiliar</span><span class="sxs-lookup"><span data-stu-id="39d0b-126">Creating a Helper</span></span>

<span data-ttu-id="39d0b-127">Este procedimento mostra como criar o auxiliar que cria a observação, conforme descrito apenas.</span><span class="sxs-lookup"><span data-stu-id="39d0b-127">This procedure shows you how to create the helper that creates the note, as just described.</span></span> <span data-ttu-id="39d0b-128">Este é um exemplo simples, mas o auxiliar personalizado pode incluir qualquer marcação e código ASP.NET que você precisa.</span><span class="sxs-lookup"><span data-stu-id="39d0b-128">This is a simple example, but the custom helper can include any markup and ASP.NET code that you need.</span></span>

1. <span data-ttu-id="39d0b-129">Na pasta raiz do site, crie uma pasta chamada *aplicativo\_código*.</span><span class="sxs-lookup"><span data-stu-id="39d0b-129">In the root folder of the website, create a folder named *App\_Code*.</span></span> <span data-ttu-id="39d0b-130">Este é um nome de pasta reservado no ASP.NET, onde você pode colocar o código para componentes como auxiliares.</span><span class="sxs-lookup"><span data-stu-id="39d0b-130">This is a reserved folder name in ASP.NET where you can put code for components like helpers.</span></span>
2. <span data-ttu-id="39d0b-131">No *aplicativo\_código* pasta criar uma nova *. cshtml* de arquivo e nomeie-o *MyHelpers.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="39d0b-131">In the *App\_Code* folder create a new *.cshtml* file and name it *MyHelpers.cshtml*.</span></span>
3. <span data-ttu-id="39d0b-132">Substitua o conteúdo existente com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="39d0b-132">Replace the existing content with the following:</span></span>

    [!code-cshtml[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    <span data-ttu-id="39d0b-133">O código usa o `@helper` a sintaxe para declarar um auxiliar novo chamado `MakeNote`.</span><span class="sxs-lookup"><span data-stu-id="39d0b-133">The code uses the `@helper` syntax to declare a new helper named `MakeNote`.</span></span> <span data-ttu-id="39d0b-134">Este auxiliar específico permite que você passe um parâmetro denominado `content` que pode conter uma combinação de texto e marcação.</span><span class="sxs-lookup"><span data-stu-id="39d0b-134">This particular helper lets you pass a parameter named `content` that can contain a combination of text and markup.</span></span> <span data-ttu-id="39d0b-135">O auxiliar insere a cadeia de caracteres no corpo de Observação usando o `@content` variável.</span><span class="sxs-lookup"><span data-stu-id="39d0b-135">The helper inserts the string into the note body using the `@content` variable.</span></span>

    <span data-ttu-id="39d0b-136">Observe que o arquivo é nomeado *MyHelpers.cshtml*, mas o auxiliar é denominado `MakeNote`.</span><span class="sxs-lookup"><span data-stu-id="39d0b-136">Notice that the file is named *MyHelpers.cshtml*, but the helper is named `MakeNote`.</span></span> <span data-ttu-id="39d0b-137">Você pode colocar vários auxiliares personalizados em um único arquivo.</span><span class="sxs-lookup"><span data-stu-id="39d0b-137">You can put multiple custom helpers into a single file.</span></span>
4. <span data-ttu-id="39d0b-138">Salve e feche o arquivo.</span><span class="sxs-lookup"><span data-stu-id="39d0b-138">Save and close the file.</span></span>

## <a name="using-the-helper-in-a-page"></a><span data-ttu-id="39d0b-139">Usando o auxiliar em uma página</span><span class="sxs-lookup"><span data-stu-id="39d0b-139">Using the Helper in a Page</span></span>

1. <span data-ttu-id="39d0b-140">Na pasta raiz, crie um novo arquivo vazio chamado *TestHelper.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="39d0b-140">In the root folder, create a new blank file called *TestHelper.cshtml*.</span></span>
2. <span data-ttu-id="39d0b-141">Adicione o seguinte código ao arquivo:</span><span class="sxs-lookup"><span data-stu-id="39d0b-141">Add the following code to the file:</span></span>

    [!code-html[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample2.html)]

    <span data-ttu-id="39d0b-142">Para chamar o auxiliar que você criou, use `@` seguido pelo nome do arquivo onde está o auxiliar, um ponto e, em seguida, o nome de auxiliar.</span><span class="sxs-lookup"><span data-stu-id="39d0b-142">To call the helper you created, use `@` followed by the file name where the helper is, a dot, and then the helper name.</span></span> <span data-ttu-id="39d0b-143">(Se você tiver várias pastas *aplicativo\_código* pasta, você pode usar a sintaxe `@FolderName.FileName.HelperName` chamar o auxiliar em qualquer aninhados no nível de pasta).</span><span class="sxs-lookup"><span data-stu-id="39d0b-143">(If you had multiple folders in the *App\_Code* folder, you could use the syntax `@FolderName.FileName.HelperName` to call your helper within any nested folder level).</span></span> <span data-ttu-id="39d0b-144">O texto que você adicionar aspas dentro dos parênteses é o texto que o auxiliar será exibido como parte da anotação na página da web.</span><span class="sxs-lookup"><span data-stu-id="39d0b-144">The text that you add in quotation marks within the parentheses is the text that the helper will display as part of the note in the web page.</span></span>
3. <span data-ttu-id="39d0b-145">Salve a página e executá-lo em um navegador.</span><span class="sxs-lookup"><span data-stu-id="39d0b-145">Save the page and run it in a browser.</span></span> <span data-ttu-id="39d0b-146">O auxiliar gera o item de anotação direita onde você chamou o auxiliar: entre dois parágrafos.</span><span class="sxs-lookup"><span data-stu-id="39d0b-146">The helper generates the note item right where you called the helper: between the two paragraphs.</span></span>

    ![Captura de tela mostrando a página no navegador e como o auxiliar gerado marcação que coloca uma caixa ao redor do texto especificado.](creating-and-using-a-helper-in-an-aspnet-web-pages-site/_static/image1.jpg)

## <a name="additional-resources"></a><span data-ttu-id="39d0b-148">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="39d0b-148">Additional Resources</span></span>


<span data-ttu-id="39d0b-149">[Menu horizontal como um auxiliar de Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341).</span><span class="sxs-lookup"><span data-stu-id="39d0b-149">[Horizontal menu as a Razor helper](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341).</span></span> <span data-ttu-id="39d0b-150">Essa entrada de blog, Mike Pope mostra como criar um menu horizontal como um auxiliar usando marcação, CSS e código.</span><span class="sxs-lookup"><span data-stu-id="39d0b-150">This blog entry by Mike Pope shows how to create a horizontal menu as a helper using markup, CSS, and code.</span></span>

<span data-ttu-id="39d0b-151">[Aproveitar o HTML5 com o ASP.NET Web páginas auxiliares para o WebMatrix e ASP.NET MVC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx).</span><span class="sxs-lookup"><span data-stu-id="39d0b-151">[Leveraging HTML5 in ASP.NET Web Pages Helpers for WebMatrix and ASP.NET MVC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx).</span></span> <span data-ttu-id="39d0b-152">Essa entrada de blog pelo Sam Abraham mostra um auxiliar que renderiza uma HTML5 `Canvas` elemento.</span><span class="sxs-lookup"><span data-stu-id="39d0b-152">This blog entry by Sam Abraham shows a helper that renders an HTML5 `Canvas` element.</span></span>

<span data-ttu-id="39d0b-153">[A diferença entre @Helpers e @Functions no WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix).</span><span class="sxs-lookup"><span data-stu-id="39d0b-153">[The Difference Between @Helpers and @Functions in WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix).</span></span> <span data-ttu-id="39d0b-154">Descreve essa entrada de blog, Mike Brind `@helper` sintaxe e `@function` sintaxe e quando usar cada um.</span><span class="sxs-lookup"><span data-stu-id="39d0b-154">This blog entry by Mike Brind describes `@helper` syntax and `@function` syntax and when to use each.</span></span>
