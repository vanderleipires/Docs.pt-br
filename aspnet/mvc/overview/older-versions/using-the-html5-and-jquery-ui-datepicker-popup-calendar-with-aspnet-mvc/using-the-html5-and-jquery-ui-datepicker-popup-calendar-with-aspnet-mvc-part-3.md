---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
title: Usando HTML5 e jQuery UI Datepicker pop-up de calendário com ASP.NET MVC - parte 3 | Microsoft Docs
author: Rick-Anderson
description: Este tutorial ensina as Noções básicas de como trabalhar com modelos de editor, modelos de exibição e o calendário de pop-up jQuery UI datepicker em uma MV ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 8f5f91ae-12d7-4cf3-ac09-4bb53d07ee60
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
msc.type: authoredcontent
ms.openlocfilehash: fd1ae746f4f134b779c7eee50cf6c840bbb7068e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a><span data-ttu-id="ec80f-103">Usando HTML5 e jQuery UI Datepicker pop-up de calendário com ASP.NET MVC - parte 3</span><span class="sxs-lookup"><span data-stu-id="ec80f-103">Using the HTML5 and jQuery UI Datepicker Popup Calendar with ASP.NET MVC - Part 3</span></span>
====================
<span data-ttu-id="ec80f-104">por [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="ec80f-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="ec80f-105">Este tutorial irá ensiná-lo os fundamentos de como trabalhar com modelos de editor, modelos de exibição e calendário pop-up jQuery UI datepicker em um aplicativo ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="ec80f-105">This tutorial will teach you the basics of how to work with editor templates, display templates, and the jQuery UI datepicker popup calendar in an ASP.NET MVC Web application.</span></span>


## <a name="working-with-complex-types"></a><span data-ttu-id="ec80f-106">Trabalhando com tipos complexos</span><span class="sxs-lookup"><span data-stu-id="ec80f-106">Working with Complex Types</span></span>

<span data-ttu-id="ec80f-107">Nesta seção você criará uma classe de endereço e saiba como criar um modelo para exibi-la.</span><span class="sxs-lookup"><span data-stu-id="ec80f-107">In this section you'll create an address class and learn how to create a template to display it.</span></span>

<span data-ttu-id="ec80f-108">No *modelos* pasta, crie um novo arquivo de classe chamado *Person.cs* onde você colocará dois tipos: um `Person` classe e um `Address` classe.</span><span class="sxs-lookup"><span data-stu-id="ec80f-108">In the *Models* folder, create a new class file named *Person.cs* where you'll put two types: a `Person` class and an `Address` class.</span></span> <span data-ttu-id="ec80f-109">O `Person` classe conterá uma propriedade é digitada como `Address`.</span><span class="sxs-lookup"><span data-stu-id="ec80f-109">The `Person` class will contain a property that's typed as `Address`.</span></span> <span data-ttu-id="ec80f-110">O `Address` é um tipo complexo, que significa que não é um dos tipos internos como `int`, `string`, ou `double`.</span><span class="sxs-lookup"><span data-stu-id="ec80f-110">The `Address` type is a complex type, meaning it's not one of the built-in types like `int`, `string`, or `double`.</span></span> <span data-ttu-id="ec80f-111">Em vez disso, ele tem várias propriedades.</span><span class="sxs-lookup"><span data-stu-id="ec80f-111">Instead, it has several properties.</span></span> <span data-ttu-id="ec80f-112">O código para as novas classes tem esta aparência:</span><span class="sxs-lookup"><span data-stu-id="ec80f-112">The code for the new classes looks like this:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

<span data-ttu-id="ec80f-113">No `Movie` controlador, adicione o seguinte `PersonDetail` ação para exibir uma instância de pessoa:</span><span class="sxs-lookup"><span data-stu-id="ec80f-113">In the `Movie` controller, add the following `PersonDetail` action to display a person instance:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

<span data-ttu-id="ec80f-114">Em seguida, adicione o seguinte código para o `Movie` controlador para preencher o `Person` modelo com alguns dados de exemplo:</span><span class="sxs-lookup"><span data-stu-id="ec80f-114">Then add the following code to the `Movie` controller to populate the `Person` model with some sample data:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

<span data-ttu-id="ec80f-115">Abra o *Views\Movies\PersonDetail.cshtml* de arquivo e adicione a seguinte marcação para o `PersonDetail` exibição.</span><span class="sxs-lookup"><span data-stu-id="ec80f-115">Open the *Views\Movies\PersonDetail.cshtml* file and add the following markup for the `PersonDetail` view.</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

<span data-ttu-id="ec80f-116">Pressione Ctrl + F5 para executar o aplicativo e navegue até *filmes/PersonDetail*.</span><span class="sxs-lookup"><span data-stu-id="ec80f-116">Press Ctrl+F5 to run the application and navigate to *Movies/PersonDetail*.</span></span>

<span data-ttu-id="ec80f-117">O `PersonDetail` exibição não contiver o `Address` tipo complexo, como você pode ver nesta captura de tela.</span><span class="sxs-lookup"><span data-stu-id="ec80f-117">The `PersonDetail` view doesn't contain the `Address` complex type, as you can see in this screenshot.</span></span> <span data-ttu-id="ec80f-118">(Nenhum endereço é exibido.)</span><span class="sxs-lookup"><span data-stu-id="ec80f-118">(No address is shown.)</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

<span data-ttu-id="ec80f-119">O `Address` dados de modelo não são exibidos porque ele é um tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="ec80f-119">The `Address` model data is not displayed because it's a complex type.</span></span> <span data-ttu-id="ec80f-120">Para exibir as informações de endereço, abra o *Views\Movies\PersonDetail.cshtml* novamente e adicione a seguinte marcação.</span><span class="sxs-lookup"><span data-stu-id="ec80f-120">To display the address information, open the *Views\Movies\PersonDetail.cshtml* file again and add the following markup.</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

<span data-ttu-id="ec80f-121">A marcação concluída para o `PersonDetail` agora exibição tem esta aparência:</span><span class="sxs-lookup"><span data-stu-id="ec80f-121">The complete markup for the `PersonDetail` now view looks like this:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

<span data-ttu-id="ec80f-122">Execute novamente o aplicativo e exibir o `PersonDetail` exibição.</span><span class="sxs-lookup"><span data-stu-id="ec80f-122">Run the application again and display the `PersonDetail` view.</span></span> <span data-ttu-id="ec80f-123">As informações de endereço agora são exibidas:</span><span class="sxs-lookup"><span data-stu-id="ec80f-123">The address information is now displayed:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a><span data-ttu-id="ec80f-124">Criando um modelo para um tipo complexo</span><span class="sxs-lookup"><span data-stu-id="ec80f-124">Creating a Template for a Complex Type</span></span>

<span data-ttu-id="ec80f-125">Nesta seção, você criará um modelo que será usado para renderizar o `Address` tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="ec80f-125">In this section you'll create a template that will be used to render the `Address` complex type.</span></span> <span data-ttu-id="ec80f-126">Quando você cria um modelo para o `Address` tipo, ASP.NET MVC pode automaticamente ser usado para formatar um modelo de endereço em qualquer lugar no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ec80f-126">When you create a template for the `Address` type, ASP.NET MVC can automatically use it to format an address model anywhere in the application.</span></span> <span data-ttu-id="ec80f-127">Isso fornece uma maneira de controlar o processamento do `Address` tipo de um só lugar no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ec80f-127">This gives you a way to control the rendering of the `Address` type from just one place in the application.</span></span>

<span data-ttu-id="ec80f-128">No *\ compartilhadas \ modelosdeexibição* pasta, criar uma exibição parcial fortemente tipada denominada **endereço**:</span><span class="sxs-lookup"><span data-stu-id="ec80f-128">In the *Views\Shared\DisplayTemplates* folder, create a strongly typed partial view named **Address**:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

<span data-ttu-id="ec80f-129">Clique em **adicionar**e, em seguida, abra o novo *Views\Shared\DisplayTemplates\Address.cshtml* arquivo.</span><span class="sxs-lookup"><span data-stu-id="ec80f-129">Click **Add**, and then open the new *Views\Shared\DisplayTemplates\Address.cshtml* file.</span></span> <span data-ttu-id="ec80f-130">O novo modo de exibição contém a seguinte marcação gerada:</span><span class="sxs-lookup"><span data-stu-id="ec80f-130">The new view contains the following generated markup:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

<span data-ttu-id="ec80f-131">Execute o aplicativo e exibir o `PersonDetail` exibição.</span><span class="sxs-lookup"><span data-stu-id="ec80f-131">Run the application and display the `PersonDetail` view.</span></span> <span data-ttu-id="ec80f-132">Neste momento, o `Address` modelo que você acabou de criar é usado para exibir o `Address` tipo complexo, para que a exibição é semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="ec80f-132">This time, the `Address` template that you just created is used to display the `Address` complex type, so the display looks like the following:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a><span data-ttu-id="ec80f-133">Resumo: Maneiras para especificar o formato de exibição do modelo e o modelo</span><span class="sxs-lookup"><span data-stu-id="ec80f-133">Summary: Ways to Specify the Model Display Format and Template</span></span>

<span data-ttu-id="ec80f-134">Você viu que você pode especificar o formato ou o modelo para uma propriedade de modelo usando as seguintes abordagens:</span><span class="sxs-lookup"><span data-stu-id="ec80f-134">You've seen that you can specify the format or template for a model property by using the following approaches:</span></span>

- <span data-ttu-id="ec80f-135">Aplicar o `DisplayFormat` atributo a uma propriedade no modelo.</span><span class="sxs-lookup"><span data-stu-id="ec80f-135">Applying the `DisplayFormat` attribute to a property in the model.</span></span> <span data-ttu-id="ec80f-136">Por exemplo, o código a seguir faz com que a data a ser exibida sem o tempo:</span><span class="sxs-lookup"><span data-stu-id="ec80f-136">For example, the following code causes the date to be displayed without the time:</span></span>

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- <span data-ttu-id="ec80f-137">Aplicando um [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) atributo a uma propriedade no modelo e especificar o tipo de dados.</span><span class="sxs-lookup"><span data-stu-id="ec80f-137">Applying a [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attribute to a property in the model and specifying the data type.</span></span> <span data-ttu-id="ec80f-138">Por exemplo, o código a seguir faz com que a data a ser exibida sem o tempo.</span><span class="sxs-lookup"><span data-stu-id="ec80f-138">For example, the following code causes the date to be displayed without the time.</span></span>

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    <span data-ttu-id="ec80f-139">Se o aplicativo contém um *date.cshtml* modelo o *\ compartilhadas \ modelosdeexibição* pasta ou o *Views\Movies\DisplayTemplates* pasta, modelo será usado para renderizar o `DateTime` propriedade.</span><span class="sxs-lookup"><span data-stu-id="ec80f-139">If the application contains a *date.cshtml* template in the *Views\Shared\DisplayTemplates* folder or the *Views\Movies\DisplayTemplates* folder, that template will be used to render the `DateTime` property.</span></span> <span data-ttu-id="ec80f-140">Caso contrário, o sistema de modelos interno do ASP.NET exibe a propriedade como uma data.</span><span class="sxs-lookup"><span data-stu-id="ec80f-140">Otherwise the built-in ASP.NET templating system displays the property as a date.</span></span>
- <span data-ttu-id="ec80f-141">Criando um modelo de exibição no *\ compartilhadas \ modelosdeexibição* pasta ou o *Views\Movies\DisplayTemplates* pasta cujo nome corresponde ao tipo de dados que você deseja formatar.</span><span class="sxs-lookup"><span data-stu-id="ec80f-141">Creating a display template in the *Views\Shared\DisplayTemplates* folder or the *Views\Movies\DisplayTemplates* folder whose name matches the data type that you want to format.</span></span> <span data-ttu-id="ec80f-142">Por exemplo, você viu que o *Views\Shared\DisplayTemplates\DateTime.cshtml* foi usado para renderizar `DateTime` propriedades em um modelo, sem adicionar um atributo para o modelo e sem adicionar qualquer marcação a modos de exibição.</span><span class="sxs-lookup"><span data-stu-id="ec80f-142">For example, you saw that the *Views\Shared\DisplayTemplates\DateTime.cshtml* was used to render `DateTime` properties in a model, without adding an attribute to the model and without adding any markup to views.</span></span>
- <span data-ttu-id="ec80f-143">Usando o [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) atributo no modelo para especificar o modelo para exibir a propriedade de modelo.</span><span class="sxs-lookup"><span data-stu-id="ec80f-143">Using the [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) attribute on the model to specify the template to display the model property.</span></span>
- <span data-ttu-id="ec80f-144">Adicionar explicitamente o nome do modelo de exibição para o [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) chamada em um modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="ec80f-144">Explicitly adding the display template name to the [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) call in a view.</span></span>

<span data-ttu-id="ec80f-145">A abordagem usada depende do que você precisa fazer em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ec80f-145">The approach you use depends on what you need to do in your application.</span></span> <span data-ttu-id="ec80f-146">Não é incomum combinar essas abordagens para obter exatamente o tipo de formatação que você precisa.</span><span class="sxs-lookup"><span data-stu-id="ec80f-146">It's not uncommon to mix these approaches to get exactly the kind of formatting that you need.</span></span>

<span data-ttu-id="ec80f-147">Na próxima seção, você alternará marcha um pouco e mover-se de personalizar como os dados são exibidos para personalizar como ele é inserido.</span><span class="sxs-lookup"><span data-stu-id="ec80f-147">In the next section, you'll switch gears a bit and move from customizing how data is displayed to customizing how it's entered.</span></span> <span data-ttu-id="ec80f-148">Você vai conectar de datepicker jQuery para os modos de exibição de edição no aplicativo para fornecer uma ótima maneira de especificar as datas.</span><span class="sxs-lookup"><span data-stu-id="ec80f-148">You'll hook up the jQuery datepicker to the edit views in the application in order to provide a slick way to specify dates.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ec80f-149">[Anterior](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
> [Próximo](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="ec80f-149">[Previous](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
[Next](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)</span></span>
