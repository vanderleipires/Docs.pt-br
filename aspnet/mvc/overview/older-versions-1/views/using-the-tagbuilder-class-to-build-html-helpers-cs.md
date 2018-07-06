---
uid: mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
title: Usando a classe TagBuilder para criar auxiliares de HTML (c#) | Microsoft Docs
author: StephenWalther
description: Stephen Walther apresenta uma classe de utilitário útil na estrutura do ASP.NET MVC, a classe TagBuilder nomeada. Você pode usar facilmente a classe TagBuilder para...
ms.author: aspnetcontent
ms.date: 03/02/2009
ms.assetid: 3975a52f-bd15-4edd-8f3d-1df93672515b
msc.legacyurl: /mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: 9990fc7ad8093643a564a5e02ff65264d4a7fe15
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813220"
---
<a name="using-the-tagbuilder-class-to-build-html-helpers-c"></a><span data-ttu-id="b9365-104">Usando a classe TagBuilder para criar auxiliares de HTML (c#)</span><span class="sxs-lookup"><span data-stu-id="b9365-104">Using the TagBuilder Class to Build HTML Helpers (C#)</span></span>
====================
<span data-ttu-id="b9365-105">por [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="b9365-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="b9365-106">Stephen Walther apresenta uma classe de utilitário útil na estrutura do ASP.NET MVC, a classe TagBuilder nomeada.</span><span class="sxs-lookup"><span data-stu-id="b9365-106">Stephen Walther introduces you to a useful utility class in the ASP.NET MVC framework named the TagBuilder class.</span></span> <span data-ttu-id="b9365-107">Você pode usar a classe TagBuilder para criar facilmente as marcas HTML.</span><span class="sxs-lookup"><span data-stu-id="b9365-107">You can use the TagBuilder class to easily build HTML tags.</span></span>


<span data-ttu-id="b9365-108">A estrutura ASP.NET MVC inclui uma classe de utilitário útil nomeada a classe TagBuilder que você pode usar ao criar auxiliares HTML.</span><span class="sxs-lookup"><span data-stu-id="b9365-108">The ASP.NET MVC framework includes a useful utility class named the TagBuilder class that you can use when building HTML helpers.</span></span> <span data-ttu-id="b9365-109">A classe TagBuilder, como sugere o nome da classe, permite criar facilmente as marcas HTML.</span><span class="sxs-lookup"><span data-stu-id="b9365-109">The TagBuilder class, as the name of the class suggests, enables you to easily build HTML tags.</span></span> <span data-ttu-id="b9365-110">Neste breve tutorial, você é provido com uma visão geral da classe TagBuilder e você aprenderá a usar essa classe quando a criação de um auxiliar HTML simple que renderiza o HTML &lt;img&gt; marcas.</span><span class="sxs-lookup"><span data-stu-id="b9365-110">In this brief tutorial, you are provided with an overview of the TagBuilder class and you learn how to use this class when building a simple HTML helper that renders HTML &lt;img&gt; tags.</span></span>

## <a name="overview-of-the-tagbuilder-class"></a><span data-ttu-id="b9365-111">Visão geral da classe TagBuilder</span><span class="sxs-lookup"><span data-stu-id="b9365-111">Overview of the TagBuilder Class</span></span>

<span data-ttu-id="b9365-112">A classe TagBuilder está contida no namespace System.Web.Mvc.</span><span class="sxs-lookup"><span data-stu-id="b9365-112">The TagBuilder class is contained in the System.Web.Mvc namespace.</span></span> <span data-ttu-id="b9365-113">Ele tem cinco métodos:</span><span class="sxs-lookup"><span data-stu-id="b9365-113">It has five methods:</span></span>

- <span data-ttu-id="b9365-114">AddCssClass() - permite que você adicione um novo *classe = ""* do atributo a uma marca.</span><span class="sxs-lookup"><span data-stu-id="b9365-114">AddCssClass() - Enables you to add a new *class=""* attribute to a tag.</span></span>
- <span data-ttu-id="b9365-115">GenerateId() - permite que você adicione um atributo de id para uma marca.</span><span class="sxs-lookup"><span data-stu-id="b9365-115">GenerateId() - Enables you to add an id attribute to a tag.</span></span> <span data-ttu-id="b9365-116">Esse método substitui automaticamente os períodos em que a id (por padrão, períodos são substituídos por sublinhados)</span><span class="sxs-lookup"><span data-stu-id="b9365-116">This method automatically replaces periods in the id (by default, periods are replaced by underscores)</span></span>
- <span data-ttu-id="b9365-117">MergeAttribute() - permite que você adicione atributos a uma marca.</span><span class="sxs-lookup"><span data-stu-id="b9365-117">MergeAttribute() - Enables you to add attributes to a tag.</span></span> <span data-ttu-id="b9365-118">Há várias sobrecargas desse método.</span><span class="sxs-lookup"><span data-stu-id="b9365-118">There are multiple overloads of this method.</span></span>
- <span data-ttu-id="b9365-119">SetInnerText() - permite que você defina o texto interno da marca.</span><span class="sxs-lookup"><span data-stu-id="b9365-119">SetInnerText() - Enables you to set the inner text of the tag.</span></span> <span data-ttu-id="b9365-120">O texto interno é a codificação HTML automaticamente.</span><span class="sxs-lookup"><span data-stu-id="b9365-120">The inner text is HTML encode automatically.</span></span>
- <span data-ttu-id="b9365-121">ToString () - permite que você renderize a marca.</span><span class="sxs-lookup"><span data-stu-id="b9365-121">ToString() - Enables you to render the tag.</span></span> <span data-ttu-id="b9365-122">Você pode especificar se deseja criar uma marca normal, uma marca de início, uma marca de fim ou uma marca de fechamento automático.</span><span class="sxs-lookup"><span data-stu-id="b9365-122">You can specify whether you want to create a normal tag, a start tag, an end tag, or a self-closing tag.</span></span>
  

<span data-ttu-id="b9365-123">A classe TagBuilder tem quatro propriedades importantes:</span><span class="sxs-lookup"><span data-stu-id="b9365-123">The TagBuilder class has four important properties:</span></span>

- <span data-ttu-id="b9365-124">Atributos – representa todos os atributos da marca.</span><span class="sxs-lookup"><span data-stu-id="b9365-124">Attributes - Represents all of the attributes of the tag.</span></span>
- <span data-ttu-id="b9365-125">IdAttributeDotReplacement - representa o caractere usado pelo método GenerateId() substituir períodos (o padrão é um caractere de sublinhado).</span><span class="sxs-lookup"><span data-stu-id="b9365-125">IdAttributeDotReplacement - Represents the character used by the GenerateId() method to replace periods (the default is an underscore).</span></span>
- <span data-ttu-id="b9365-126">InnerHTML - representa o conteúdo interno da marca.</span><span class="sxs-lookup"><span data-stu-id="b9365-126">InnerHTML - Represents the inner contents of the tag.</span></span> <span data-ttu-id="b9365-127">Atribuir uma cadeia de caracteres para esta propriedade *não* HTML codificar a cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="b9365-127">Assigning a string to this property *does not* HTML encode the string.</span></span>
- <span data-ttu-id="b9365-128">TagName - representa o nome da marca.</span><span class="sxs-lookup"><span data-stu-id="b9365-128">TagName - Represents the name of the tag.</span></span>

<span data-ttu-id="b9365-129">Esses métodos e propriedades oferecem todos os métodos básicos e as propriedades que você precisa para criar uma marca HTML.</span><span class="sxs-lookup"><span data-stu-id="b9365-129">These methods and properties give you all of the basic methods and properties that you need to build up an HTML tag.</span></span> <span data-ttu-id="b9365-130">Você não precisa usar a classe TagBuilder realmente.</span><span class="sxs-lookup"><span data-stu-id="b9365-130">You don't really need to use the TagBuilder class.</span></span> <span data-ttu-id="b9365-131">Você pode usar uma classe StringBuilder.</span><span class="sxs-lookup"><span data-stu-id="b9365-131">You could use a StringBuilder class instead.</span></span> <span data-ttu-id="b9365-132">No entanto, a classe TagBuilder facilita sua vida um pouco.</span><span class="sxs-lookup"><span data-stu-id="b9365-132">However, the TagBuilder class makes your life a little easier.</span></span>

## <a name="creating-an-image-html-helper"></a><span data-ttu-id="b9365-133">Criar um auxiliar de HTML de imagem</span><span class="sxs-lookup"><span data-stu-id="b9365-133">Creating an Image HTML Helper</span></span>

<span data-ttu-id="b9365-134">Quando você cria uma instância da classe TagBuilder, você pode passar o nome da marca que você deseja compilar para o construtor TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="b9365-134">When you create an instance of the TagBuilder class, you pass the name of the tag that you want to build to the TagBuilder constructor.</span></span> <span data-ttu-id="b9365-135">Em seguida, você pode chamar métodos, como os métodos AddCssClass e MergeAttribute() para modificar os atributos da marca.</span><span class="sxs-lookup"><span data-stu-id="b9365-135">Next, you can call methods like the AddCssClass and MergeAttribute() methods to modify the attributes of the tag.</span></span> <span data-ttu-id="b9365-136">Por fim, você pode chamar o método ToString () para processar a tag.</span><span class="sxs-lookup"><span data-stu-id="b9365-136">Finally, you call the ToString() method to render the tag.</span></span>

<span data-ttu-id="b9365-137">Por exemplo, a listagem 1 contém um auxiliar de imagem HTML.</span><span class="sxs-lookup"><span data-stu-id="b9365-137">For example, Listing 1 contains an Image HTML helper.</span></span> <span data-ttu-id="b9365-138">O auxiliar de imagem é implementado internamente com um que representa um HTML TagBuilder &lt;img&gt; marca.</span><span class="sxs-lookup"><span data-stu-id="b9365-138">The Image helper is implemented internally with a TagBuilder that represents an HTML &lt;img&gt; tag.</span></span>

<span data-ttu-id="b9365-139">**Listagem 1 - Helpers\ImageHelper.cs**</span><span class="sxs-lookup"><span data-stu-id="b9365-139">**Listing 1 - Helpers\ImageHelper.cs**</span></span>

[!code-csharp[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample1.cs)]

<span data-ttu-id="b9365-140">A classe na listagem 1 contém dois métodos sobrecarregados estáticos chamados Image.</span><span class="sxs-lookup"><span data-stu-id="b9365-140">The class in Listing 1 contains two static overloaded methods named Image.</span></span> <span data-ttu-id="b9365-141">Quando você chama o método Image(), você pode passar um objeto que representa um conjunto de atributos HTML ou não.</span><span class="sxs-lookup"><span data-stu-id="b9365-141">When you call the Image() method, you can pass an object which represents a set of HTML attributes or not.</span></span>

<span data-ttu-id="b9365-142">Observe como o método TagBuilder.MergeAttribute() é usado para adicionar atributos individuais, como o atributo src para o TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="b9365-142">Notice how the TagBuilder.MergeAttribute() method is used to add individual attributes such as the src attribute to the TagBuilder.</span></span> <span data-ttu-id="b9365-143">Observe, além disso, como o método de TagBuilder.MergeAttributes() é usado para adicionar uma coleção de atributos para o TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="b9365-143">Notice, furthermore, how the TagBuilder.MergeAttributes() method is used to add a collection of attributes to the TagBuilder.</span></span> <span data-ttu-id="b9365-144">O método MergeAttributes() aceita um dicionário&lt;cadeia de caracteres, objeto&gt; parâmetro.</span><span class="sxs-lookup"><span data-stu-id="b9365-144">The MergeAttributes() method accepts a Dictionary&lt;string,object&gt; parameter.</span></span> <span data-ttu-id="b9365-145">A classe RouteValueDictionary é usada para converter o objeto que representa a coleção de atributos em um dicionário&lt;cadeia de caracteres, objeto&gt;.</span><span class="sxs-lookup"><span data-stu-id="b9365-145">The RouteValueDictionary class is used to convert the Object representing the collection of attributes into a Dictionary&lt;string,object&gt;.</span></span>

<span data-ttu-id="b9365-146">Depois de criar o auxiliar de imagem, você pode usar o auxiliar em suas exibições ASP.NET MVC, assim como qualquer um dos outros auxiliares HTML padrão.</span><span class="sxs-lookup"><span data-stu-id="b9365-146">After you create the Image helper, you can use the helper in your ASP.NET MVC views just like any of the other standard HTML helpers.</span></span> <span data-ttu-id="b9365-147">O modo de exibição na listagem 2 usa o auxiliar de imagem para exibir a mesma imagem de um Xbox duas vezes (veja a Figura 1).</span><span class="sxs-lookup"><span data-stu-id="b9365-147">The view in Listing 2 uses the Image helper to display the same image of an Xbox twice (see Figure 1).</span></span> <span data-ttu-id="b9365-148">O auxiliar Image() é chamado com e sem uma coleção de atributos HTML.</span><span class="sxs-lookup"><span data-stu-id="b9365-148">The Image() helper is called both with and without an HTML attribute collection.</span></span>

<span data-ttu-id="b9365-149">**Listagem 2 - Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="b9365-149">**Listing 2 - Home\Index.aspx**</span></span>

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample2.aspx)]


<span data-ttu-id="b9365-150">[![A caixa de diálogo Novo projeto](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b9365-150">[![The New Project dialog box](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.png)</span></span>

<span data-ttu-id="b9365-151">**Figura 01**: usando o auxiliar de imagem ([clique para exibir a imagem em tamanho normal](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="b9365-151">**Figure 01**: Using the Image helper([Click to view full-size image](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image2.png))</span></span>


<span data-ttu-id="b9365-152">Observe que você deve importar o namespace associado com o auxiliar de imagem na parte superior do modo de exibição aspx.</span><span class="sxs-lookup"><span data-stu-id="b9365-152">Notice that you must import the namespace associated with the Image helper at the top of the Index.aspx view.</span></span> <span data-ttu-id="b9365-153">O auxiliar é importado com a seguinte diretiva:</span><span class="sxs-lookup"><span data-stu-id="b9365-153">The helper is imported with the following directive:</span></span>

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample3.aspx)]

> [!div class="step-by-step"]
> <span data-ttu-id="b9365-154">[Anterior](creating-custom-html-helpers-cs.md)
> [Próximo](creating-page-layouts-with-view-master-pages-cs.md)</span><span class="sxs-lookup"><span data-stu-id="b9365-154">[Previous](creating-custom-html-helpers-cs.md)
[Next](creating-page-layouts-with-view-master-pages-cs.md)</span></span>
