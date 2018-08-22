---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
title: Usando HTML5 e calendário jQuery UI Datepicker pop-up com o ASP.NET MVC – parte 3 | Microsoft Docs
author: Rick-Anderson
description: Este tutorial ensinará os fundamentos de como trabalhar com modelos do editor, modelos de exibição e o calendário de pop-up jQuery UI datepicker em uma MV do ASP.NET...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 8f5f91ae-12d7-4cf3-ac09-4bb53d07ee60
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
msc.type: authoredcontent
ms.openlocfilehash: 184d93323ac6f2cb7f14d32b401cf6a400eb79a6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824197"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a><span data-ttu-id="a1457-103">Usando HTML5 e calendário jQuery UI Datepicker pop-up com o ASP.NET MVC – parte 3</span><span class="sxs-lookup"><span data-stu-id="a1457-103">Using the HTML5 and jQuery UI Datepicker Popup Calendar with ASP.NET MVC - Part 3</span></span>
====================
<span data-ttu-id="a1457-104">por [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="a1457-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="a1457-105">Este tutorial ensinará os fundamentos de como trabalhar com modelos do editor, modelos de exibição e calendário pop-up jQuery UI datepicker em um aplicativo Web ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="a1457-105">This tutorial will teach you the basics of how to work with editor templates, display templates, and the jQuery UI datepicker popup calendar in an ASP.NET MVC Web application.</span></span>


## <a name="working-with-complex-types"></a><span data-ttu-id="a1457-106">Trabalhando com tipos complexos</span><span class="sxs-lookup"><span data-stu-id="a1457-106">Working with Complex Types</span></span>

<span data-ttu-id="a1457-107">Nesta seção você criará uma classe de endereço e saiba como criar um modelo para exibi-lo.</span><span class="sxs-lookup"><span data-stu-id="a1457-107">In this section you'll create an address class and learn how to create a template to display it.</span></span>

<span data-ttu-id="a1457-108">No *modelos* pasta, crie um novo arquivo de classe chamado *Person.cs* onde você colocará dois tipos: uma `Person` classe e um `Address` classe.</span><span class="sxs-lookup"><span data-stu-id="a1457-108">In the *Models* folder, create a new class file named *Person.cs* where you'll put two types: a `Person` class and an `Address` class.</span></span> <span data-ttu-id="a1457-109">O `Person` classe conterá uma propriedade que é digitada como `Address`.</span><span class="sxs-lookup"><span data-stu-id="a1457-109">The `Person` class will contain a property that's typed as `Address`.</span></span> <span data-ttu-id="a1457-110">O `Address` tipo é um tipo complexo, que significa que ele não é um dos tipos internos, como `int`, `string`, ou `double`.</span><span class="sxs-lookup"><span data-stu-id="a1457-110">The `Address` type is a complex type, meaning it's not one of the built-in types like `int`, `string`, or `double`.</span></span> <span data-ttu-id="a1457-111">Em vez disso, ele tem várias propriedades.</span><span class="sxs-lookup"><span data-stu-id="a1457-111">Instead, it has several properties.</span></span> <span data-ttu-id="a1457-112">O código para as novas classes tem esta aparência:</span><span class="sxs-lookup"><span data-stu-id="a1457-112">The code for the new classes looks like this:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

<span data-ttu-id="a1457-113">No `Movie` controlador, adicione o seguinte `PersonDetail` ação para exibir uma instância de pessoa:</span><span class="sxs-lookup"><span data-stu-id="a1457-113">In the `Movie` controller, add the following `PersonDetail` action to display a person instance:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

<span data-ttu-id="a1457-114">Em seguida, adicione o seguinte código para o `Movie` controlador para preencher o `Person` modelo com alguns dados de exemplo:</span><span class="sxs-lookup"><span data-stu-id="a1457-114">Then add the following code to the `Movie` controller to populate the `Person` model with some sample data:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

<span data-ttu-id="a1457-115">Abra o *Views\Movies\PersonDetail.cshtml* arquivo e adicione a seguinte marcação para o `PersonDetail` modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="a1457-115">Open the *Views\Movies\PersonDetail.cshtml* file and add the following markup for the `PersonDetail` view.</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

<span data-ttu-id="a1457-116">Pressione Ctrl + F5 para executar o aplicativo e navegue até *filmes/PersonDetail*.</span><span class="sxs-lookup"><span data-stu-id="a1457-116">Press Ctrl+F5 to run the application and navigate to *Movies/PersonDetail*.</span></span>

<span data-ttu-id="a1457-117">O `PersonDetail` modo de exibição não contiver o `Address` tipo complexo, como você pode ver nessa captura de tela.</span><span class="sxs-lookup"><span data-stu-id="a1457-117">The `PersonDetail` view doesn't contain the `Address` complex type, as you can see in this screenshot.</span></span> <span data-ttu-id="a1457-118">(Nenhum endereço é mostrado).</span><span class="sxs-lookup"><span data-stu-id="a1457-118">(No address is shown.)</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

<span data-ttu-id="a1457-119">O `Address` dados de modelo não são exibidos porque ele é um tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="a1457-119">The `Address` model data is not displayed because it's a complex type.</span></span> <span data-ttu-id="a1457-120">Para exibir as informações de endereço, abra o *Views\Movies\PersonDetail.cshtml* novamente e adicione a seguinte marcação.</span><span class="sxs-lookup"><span data-stu-id="a1457-120">To display the address information, open the *Views\Movies\PersonDetail.cshtml* file again and add the following markup.</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

<span data-ttu-id="a1457-121">A marcação completa para o `PersonDetail` agora exibição tenha esta aparência:</span><span class="sxs-lookup"><span data-stu-id="a1457-121">The complete markup for the `PersonDetail` now view looks like this:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

<span data-ttu-id="a1457-122">Executar o aplicativo novamente e exibir o `PersonDetail` modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="a1457-122">Run the application again and display the `PersonDetail` view.</span></span> <span data-ttu-id="a1457-123">As informações de endereço agora são exibidas:</span><span class="sxs-lookup"><span data-stu-id="a1457-123">The address information is now displayed:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a><span data-ttu-id="a1457-124">Criar um modelo para um tipo complexo</span><span class="sxs-lookup"><span data-stu-id="a1457-124">Creating a Template for a Complex Type</span></span>

<span data-ttu-id="a1457-125">Nesta seção, você criará um modelo que será usado para renderizar o `Address` tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="a1457-125">In this section you'll create a template that will be used to render the `Address` complex type.</span></span> <span data-ttu-id="a1457-126">Quando você cria um modelo para o `Address` tipo, ASP.NET MVC pode automaticamente ser usado para formatar um modelo de endereço em qualquer lugar no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="a1457-126">When you create a template for the `Address` type, ASP.NET MVC can automatically use it to format an address model anywhere in the application.</span></span> <span data-ttu-id="a1457-127">Isso lhe dá uma maneira de controlar o processamento do `Address` tipo de um só lugar no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="a1457-127">This gives you a way to control the rendering of the `Address` type from just one place in the application.</span></span>

<span data-ttu-id="a1457-128">No *\ compartilhadas \ modelosdeexibição* pasta, crie uma exibição parcial com rigidez de tipos nomeada **endereço**:</span><span class="sxs-lookup"><span data-stu-id="a1457-128">In the *Views\Shared\DisplayTemplates* folder, create a strongly typed partial view named **Address**:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

<span data-ttu-id="a1457-129">Clique em **Add**e, em seguida, abra o novo *Views\Shared\DisplayTemplates\Address.cshtml* arquivo.</span><span class="sxs-lookup"><span data-stu-id="a1457-129">Click **Add**, and then open the new *Views\Shared\DisplayTemplates\Address.cshtml* file.</span></span> <span data-ttu-id="a1457-130">O novo modo de exibição contém a marcação gerada a seguir:</span><span class="sxs-lookup"><span data-stu-id="a1457-130">The new view contains the following generated markup:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

<span data-ttu-id="a1457-131">Executar o aplicativo e exibir o `PersonDetail` modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="a1457-131">Run the application and display the `PersonDetail` view.</span></span> <span data-ttu-id="a1457-132">Neste momento, o `Address` modelo que você acabou de criar é usado para exibir o `Address` tipo complexo, portanto, a exibição é semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="a1457-132">This time, the `Address` template that you just created is used to display the `Address` complex type, so the display looks like the following:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a><span data-ttu-id="a1457-133">Resumo: Maneiras de especificar o formato de exibição do modelo e o modelo</span><span class="sxs-lookup"><span data-stu-id="a1457-133">Summary: Ways to Specify the Model Display Format and Template</span></span>

<span data-ttu-id="a1457-134">Você já viu que você pode especificar o formato ou o modelo para uma propriedade de modelo usando as seguintes abordagens:</span><span class="sxs-lookup"><span data-stu-id="a1457-134">You've seen that you can specify the format or template for a model property by using the following approaches:</span></span>

- <span data-ttu-id="a1457-135">Aplicando o `DisplayFormat` de atributo a uma propriedade no modelo.</span><span class="sxs-lookup"><span data-stu-id="a1457-135">Applying the `DisplayFormat` attribute to a property in the model.</span></span> <span data-ttu-id="a1457-136">Por exemplo, o código a seguir faz com que a data a ser exibida sem o tempo:</span><span class="sxs-lookup"><span data-stu-id="a1457-136">For example, the following code causes the date to be displayed without the time:</span></span>

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- <span data-ttu-id="a1457-137">Aplicação de um [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) atributo a uma propriedade no modelo e especificar o tipo de dados.</span><span class="sxs-lookup"><span data-stu-id="a1457-137">Applying a [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attribute to a property in the model and specifying the data type.</span></span> <span data-ttu-id="a1457-138">Por exemplo, o código a seguir faz com que a data a ser exibida sem o tempo.</span><span class="sxs-lookup"><span data-stu-id="a1457-138">For example, the following code causes the date to be displayed without the time.</span></span>

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    <span data-ttu-id="a1457-139">Se o aplicativo contém um *date.cshtml* modelo na *\ compartilhadas \ modelosdeexibição* pasta ou o *Views\Movies\DisplayTemplates* pasta, modelo será usado para renderizar o `DateTime` propriedade.</span><span class="sxs-lookup"><span data-stu-id="a1457-139">If the application contains a *date.cshtml* template in the *Views\Shared\DisplayTemplates* folder or the *Views\Movies\DisplayTemplates* folder, that template will be used to render the `DateTime` property.</span></span> <span data-ttu-id="a1457-140">Caso contrário, o sistema de modelos internos do ASP.NET exibe a propriedade como uma data.</span><span class="sxs-lookup"><span data-stu-id="a1457-140">Otherwise the built-in ASP.NET templating system displays the property as a date.</span></span>
- <span data-ttu-id="a1457-141">Criando um modelo de exibição na *\ compartilhadas \ modelosdeexibição* pasta ou o *Views\Movies\DisplayTemplates* pasta cujo nome corresponde ao tipo de dados que você deseja formatar.</span><span class="sxs-lookup"><span data-stu-id="a1457-141">Creating a display template in the *Views\Shared\DisplayTemplates* folder or the *Views\Movies\DisplayTemplates* folder whose name matches the data type that you want to format.</span></span> <span data-ttu-id="a1457-142">Por exemplo, você viu que o *Views\Shared\DisplayTemplates\DateTime.cshtml* foi usado para renderizar `DateTime` propriedades em um modelo, sem adicionar um atributo para o modelo e sem adicionar qualquer marcação para modos de exibição.</span><span class="sxs-lookup"><span data-stu-id="a1457-142">For example, you saw that the *Views\Shared\DisplayTemplates\DateTime.cshtml* was used to render `DateTime` properties in a model, without adding an attribute to the model and without adding any markup to views.</span></span>
- <span data-ttu-id="a1457-143">Usando o [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) atributo no modelo para especificar o modelo para exibir a propriedade de modelo.</span><span class="sxs-lookup"><span data-stu-id="a1457-143">Using the [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) attribute on the model to specify the template to display the model property.</span></span>
- <span data-ttu-id="a1457-144">Adicionar explicitamente o nome do modelo de exibição para o [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) chamar em um modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="a1457-144">Explicitly adding the display template name to the [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) call in a view.</span></span>

<span data-ttu-id="a1457-145">A abordagem usada depende do que você precisa fazer em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="a1457-145">The approach you use depends on what you need to do in your application.</span></span> <span data-ttu-id="a1457-146">Não é incomum combinar essas abordagens para obter exatamente o tipo de formatação que você precisa.</span><span class="sxs-lookup"><span data-stu-id="a1457-146">It's not uncommon to mix these approaches to get exactly the kind of formatting that you need.</span></span>

<span data-ttu-id="a1457-147">Na próxima seção, você vai mudar a marcha um pouco e mover de personalizar como os dados são exibidos para personalizar como está inserido.</span><span class="sxs-lookup"><span data-stu-id="a1457-147">In the next section, you'll switch gears a bit and move from customizing how data is displayed to customizing how it's entered.</span></span> <span data-ttu-id="a1457-148">Vou ligar o datepicker do jQuery para os modos de exibição de edição no aplicativo para fornecer uma ótima maneira de especificar as datas.</span><span class="sxs-lookup"><span data-stu-id="a1457-148">You'll hook up the jQuery datepicker to the edit views in the application in order to provide a slick way to specify dates.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a1457-149">[Anterior](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
> [Próximo](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="a1457-149">[Previous](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
[Next](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)</span></span>
