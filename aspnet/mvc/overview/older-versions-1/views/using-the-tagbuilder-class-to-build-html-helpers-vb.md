---
uid: mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-vb
title: Usando a classe TagBuilder para compilar auxiliares HTML (VB) | Microsoft Docs
author: StephenWalther
description: Stephen Walther apresenta uma classe de utilitário útil na estrutura do ASP.NET MVC, denominada a classe TagBuilder. Você pode usar a classe TagBuilder facilmente...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: ec26f264-d0ea-4031-9943-825505a3ac4b
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-vb
msc.type: authoredcontent
ms.openlocfilehash: 2b72e08dff646f66252f210543230186cab6e641
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872225"
---
<a name="using-the-tagbuilder-class-to-build-html-helpers-vb"></a><span data-ttu-id="6fb05-104">Usando a classe TagBuilder para compilar auxiliares HTML (VB)</span><span class="sxs-lookup"><span data-stu-id="6fb05-104">Using the TagBuilder Class to Build HTML Helpers (VB)</span></span>
====================
<span data-ttu-id="6fb05-105">por [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="6fb05-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="6fb05-106">Stephen Walther apresenta uma classe de utilitário útil na estrutura do ASP.NET MVC, denominada a classe TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="6fb05-106">Stephen Walther introduces you to a useful utility class in the ASP.NET MVC framework named the TagBuilder class.</span></span> <span data-ttu-id="6fb05-107">Você pode usar a classe TagBuilder para criar facilmente marcas HTML.</span><span class="sxs-lookup"><span data-stu-id="6fb05-107">You can use the TagBuilder class to easily build HTML tags.</span></span>


<span data-ttu-id="6fb05-108">A estrutura ASP.NET MVC inclui uma classe de utilitário útil denominada classe TagBuilder que você pode usar ao criar auxiliares HTML.</span><span class="sxs-lookup"><span data-stu-id="6fb05-108">The ASP.NET MVC framework includes a useful utility class named the TagBuilder class that you can use when building HTML helpers.</span></span> <span data-ttu-id="6fb05-109">A classe TagBuilder, como o nome da classe sugere, permite criar facilmente marcas HTML.</span><span class="sxs-lookup"><span data-stu-id="6fb05-109">The TagBuilder class, as the name of the class suggests, enables you to easily build HTML tags.</span></span> <span data-ttu-id="6fb05-110">Este breve tutorial, você terá uma visão geral da classe TagBuilder e saiba como usar essa classe ao criar um auxiliar HTML simple que renderiza HTML &lt;img&gt; marcas.</span><span class="sxs-lookup"><span data-stu-id="6fb05-110">In this brief tutorial, you are provided with an overview of the TagBuilder class and you learn how to use this class when building a simple HTML helper that renders HTML &lt;img&gt; tags.</span></span>

## <a name="overview-of-the-tagbuilder-class"></a><span data-ttu-id="6fb05-111">Visão geral da classe TagBuilder</span><span class="sxs-lookup"><span data-stu-id="6fb05-111">Overview of the TagBuilder Class</span></span>

<span data-ttu-id="6fb05-112">A classe TagBuilder está contida no namespace System.Web.Mvc.</span><span class="sxs-lookup"><span data-stu-id="6fb05-112">The TagBuilder class is contained in the System.Web.Mvc namespace.</span></span> <span data-ttu-id="6fb05-113">Ele tem cinco métodos:</span><span class="sxs-lookup"><span data-stu-id="6fb05-113">It has five methods:</span></span>

- <span data-ttu-id="6fb05-114">AddCssClass() – permite que você adicione um novo *classe = ""* atributo a uma marca.</span><span class="sxs-lookup"><span data-stu-id="6fb05-114">AddCssClass() – Enables you to add a new *class=""* attribute to a tag.</span></span>
- <span data-ttu-id="6fb05-115">GenerateId() – permite que você adicione um atributo de id para uma marca.</span><span class="sxs-lookup"><span data-stu-id="6fb05-115">GenerateId() – Enables you to add an id attribute to a tag.</span></span> <span data-ttu-id="6fb05-116">Esse método substitui automaticamente os períodos em que a id (por padrão, períodos são substituídos por sublinhados)</span><span class="sxs-lookup"><span data-stu-id="6fb05-116">This method automatically replaces periods in the id (by default, periods are replaced by underscores)</span></span>
- <span data-ttu-id="6fb05-117">MergeAttribute() – permite que você adicionar atributos a uma marca.</span><span class="sxs-lookup"><span data-stu-id="6fb05-117">MergeAttribute() – Enables you to add attributes to a tag.</span></span> <span data-ttu-id="6fb05-118">Há várias sobrecargas do método.</span><span class="sxs-lookup"><span data-stu-id="6fb05-118">There are multiple overloads of this method.</span></span>
- <span data-ttu-id="6fb05-119">SetInnerText() – permite que você defina o texto interno da marca.</span><span class="sxs-lookup"><span data-stu-id="6fb05-119">SetInnerText() – Enables you to set the inner text of the tag.</span></span> <span data-ttu-id="6fb05-120">O texto interno é a codificação HTML automaticamente.</span><span class="sxs-lookup"><span data-stu-id="6fb05-120">The inner text is HTML encode automatically.</span></span>
- <span data-ttu-id="6fb05-121">ToString () – permite que você renderize a marca.</span><span class="sxs-lookup"><span data-stu-id="6fb05-121">ToString() – Enables you to render the tag.</span></span> <span data-ttu-id="6fb05-122">Você pode especificar se deseja criar uma marca normal, uma marca de início, uma marca de fim ou uma marca de fechamento automático.</span><span class="sxs-lookup"><span data-stu-id="6fb05-122">You can specify whether you want to create a normal tag, a start tag, an end tag, or a self-closing tag.</span></span>
  

<span data-ttu-id="6fb05-123">A classe TagBuilder tem quatro propriedades importantes:</span><span class="sxs-lookup"><span data-stu-id="6fb05-123">The TagBuilder class has four important properties:</span></span>

- <span data-ttu-id="6fb05-124">Atributos – representa todos os atributos da marca.</span><span class="sxs-lookup"><span data-stu-id="6fb05-124">Attributes – Represents all of the attributes of the tag.</span></span>
- <span data-ttu-id="6fb05-125">IdAttributeDotReplacement – representa o caractere usado pelo método GenerateId() para substituir pontos (o padrão é um caractere de sublinhado).</span><span class="sxs-lookup"><span data-stu-id="6fb05-125">IdAttributeDotReplacement – Represents the character used by the GenerateId() method to replace periods (the default is an underscore).</span></span>
- <span data-ttu-id="6fb05-126">InnerHTML – representa o conteúdo interno da marca.</span><span class="sxs-lookup"><span data-stu-id="6fb05-126">InnerHTML – Represents the inner contents of the tag.</span></span> <span data-ttu-id="6fb05-127">Atribuir uma cadeia de caracteres para esta propriedade *não* HTML codificar a cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="6fb05-127">Assigning a string to this property *does not* HTML encode the string.</span></span>
- <span data-ttu-id="6fb05-128">TagName – representa o nome da marca.</span><span class="sxs-lookup"><span data-stu-id="6fb05-128">TagName – Represents the name of the tag.</span></span>

<span data-ttu-id="6fb05-129">Esses métodos e propriedades oferecem todos os métodos básicos e as propriedades que você precisa criar uma marca HTML.</span><span class="sxs-lookup"><span data-stu-id="6fb05-129">These methods and properties give you all of the basic methods and properties that you need to build up an HTML tag.</span></span> <span data-ttu-id="6fb05-130">Você não precisa usar a classe TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="6fb05-130">You don't really need to use the TagBuilder class.</span></span> <span data-ttu-id="6fb05-131">Você pode usar uma classe StringBuilder.</span><span class="sxs-lookup"><span data-stu-id="6fb05-131">You could use a StringBuilder class instead.</span></span> <span data-ttu-id="6fb05-132">No entanto, a classe TagBuilder facilita sua vida um pouco.</span><span class="sxs-lookup"><span data-stu-id="6fb05-132">However, the TagBuilder class makes your life a little easier.</span></span>

## <a name="creating-an-image-html-helper"></a><span data-ttu-id="6fb05-133">Criando um auxiliar HTML de imagem</span><span class="sxs-lookup"><span data-stu-id="6fb05-133">Creating an Image HTML Helper</span></span>

<span data-ttu-id="6fb05-134">Quando você cria uma instância da classe TagBuilder, você pode passar o nome da marca que você deseja criar para o construtor TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="6fb05-134">When you create an instance of the TagBuilder class, you pass the name of the tag that you want to build to the TagBuilder constructor.</span></span> <span data-ttu-id="6fb05-135">Em seguida, você pode chamar métodos, como os métodos AddCssClass e MergeAttribute() para modificar os atributos da marca.</span><span class="sxs-lookup"><span data-stu-id="6fb05-135">Next, you can call methods like the AddCssClass and MergeAttribute() methods to modify the attributes of the tag.</span></span> <span data-ttu-id="6fb05-136">Por fim, você pode chamar o método ToString () para renderizar a marca.</span><span class="sxs-lookup"><span data-stu-id="6fb05-136">Finally, you call the ToString() method to render the tag.</span></span>

<span data-ttu-id="6fb05-137">Por exemplo, a listagem 1 contém um auxiliar de imagem HTML.</span><span class="sxs-lookup"><span data-stu-id="6fb05-137">For example, Listing 1 contains an Image HTML helper.</span></span> <span data-ttu-id="6fb05-138">O auxiliar de imagem é implementado internamente com um TagBuilder que representa um HTML &lt;img&gt; marca.</span><span class="sxs-lookup"><span data-stu-id="6fb05-138">The Image helper is implemented internally with a TagBuilder that represents an HTML &lt;img&gt; tag.</span></span>

<span data-ttu-id="6fb05-139">**Listando 1 – Helpers\ImageHelper.vb**</span><span class="sxs-lookup"><span data-stu-id="6fb05-139">**Listing 1 – Helpers\ImageHelper.vb**</span></span>

[!code-vb[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample1.vb)]

<span data-ttu-id="6fb05-140">O módulo na listagem 1 contém dois métodos sobrecarregados denominados Image().</span><span class="sxs-lookup"><span data-stu-id="6fb05-140">The module in Listing 1 contains two overloaded methods named Image().</span></span> <span data-ttu-id="6fb05-141">Quando você chama o método Image(), você pode passar um objeto que representa um conjunto de atributos HTML ou não.</span><span class="sxs-lookup"><span data-stu-id="6fb05-141">When you call the Image() method, you can pass an object which represents a set of HTML attributes or not.</span></span>

<span data-ttu-id="6fb05-142">Observe como o método TagBuilder.MergeAttribute() é usado para adicionar atributos individuais, como o atributo src para o TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="6fb05-142">Notice how the TagBuilder.MergeAttribute() method is used to add individual attributes such as the src attribute to the TagBuilder.</span></span> <span data-ttu-id="6fb05-143">Observe, além disso, como o método TagBuilder.MergeAttributes() é usado para adicionar uma coleção de atributos para o TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="6fb05-143">Notice, furthermore, how the TagBuilder.MergeAttributes() method is used to add a collection of attributes to the TagBuilder.</span></span> <span data-ttu-id="6fb05-144">O método MergeAttributes() aceita um dicionário&lt;de cadeia de caracteres, objeto&gt; parâmetro.</span><span class="sxs-lookup"><span data-stu-id="6fb05-144">The MergeAttributes() method accepts a Dictionary&lt;string,object&gt; parameter.</span></span> <span data-ttu-id="6fb05-145">A classe RouteValueDictionary é usada para converter o objeto que representa a coleção de atributos em um dicionário&lt;de cadeia de caracteres, objeto&gt;.</span><span class="sxs-lookup"><span data-stu-id="6fb05-145">The RouteValueDictionary class is used to convert the Object representing the collection of attributes into a Dictionary&lt;string,object&gt;.</span></span>

<span data-ttu-id="6fb05-146">Depois de criar o auxiliar de imagem, você pode usar o auxiliar em exibições ASP.NET MVC assim como qualquer um dos outros auxiliares HTML padrão.</span><span class="sxs-lookup"><span data-stu-id="6fb05-146">After you create the Image helper, you can use the helper in your ASP.NET MVC views just like any of the other standard HTML helpers.</span></span> <span data-ttu-id="6fb05-147">O modo de exibição na lista 2 usa o auxiliar de imagem para exibir a mesma imagem de um Xbox duas vezes (consulte a Figura 1).</span><span class="sxs-lookup"><span data-stu-id="6fb05-147">The view in Listing 2 uses the Image helper to display the same image of an Xbox twice (see Figure 1).</span></span> <span data-ttu-id="6fb05-148">O auxiliar Image() é chamado com e sem uma coleção de atributos HTML.</span><span class="sxs-lookup"><span data-stu-id="6fb05-148">The Image() helper is called both with and without an HTML attribute collection.</span></span>

<span data-ttu-id="6fb05-149">**A listagem 2 – Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="6fb05-149">**Listing 2 – Home\Index.aspx**</span></span>

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample2.aspx)]


<span data-ttu-id="6fb05-150">[![A caixa de diálogo Novo projeto](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6fb05-150">[![The New Project dialog box](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.png)</span></span>

<span data-ttu-id="6fb05-151">**Figura 01**: usando o auxiliar de imagem ([clique para exibir a imagem em tamanho normal](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="6fb05-151">**Figure 01**: Using the Image helper([Click to view full-size image](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image2.png))</span></span>


<span data-ttu-id="6fb05-152">Observe que você deve importar o namespace associado ao auxiliar de imagem na parte superior do modo de exibição Index.aspx.</span><span class="sxs-lookup"><span data-stu-id="6fb05-152">Notice that you must import the namespace associated with the Image helper at the top of the Index.aspx view.</span></span> <span data-ttu-id="6fb05-153">O auxiliar é importado com a seguinte diretiva:</span><span class="sxs-lookup"><span data-stu-id="6fb05-153">The helper is imported with the following directive:</span></span>

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample3.aspx)]

<span data-ttu-id="6fb05-154">Em um aplicativo do Visual Basic, o namespace padrão é o mesmo que o nome do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="6fb05-154">In a Visual Basic application, the default namespace is the same as the name of the application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6fb05-155">[Anterior](creating-custom-html-helpers-vb.md)
> [Próximo](creating-page-layouts-with-view-master-pages-vb.md)</span><span class="sxs-lookup"><span data-stu-id="6fb05-155">[Previous](creating-custom-html-helpers-vb.md)
[Next](creating-page-layouts-with-view-master-pages-vb.md)</span></span>
