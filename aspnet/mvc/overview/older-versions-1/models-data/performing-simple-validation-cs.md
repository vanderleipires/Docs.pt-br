---
uid: mvc/overview/older-versions-1/models-data/performing-simple-validation-cs
title: Realizar validação simples (c#) | Microsoft Docs
author: StephenWalther
description: Saiba como executar a validação em um aplicativo ASP.NET MVC. Neste tutorial, Stephen Walther apresenta você ao estado de modelo e o auxiliar de validação HTML...
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 21383c9d-6aea-4bad-a99b-b5f2c9d6503f
msc.legacyurl: /mvc/overview/older-versions-1/models-data/performing-simple-validation-cs
msc.type: authoredcontent
ms.openlocfilehash: 14d7857c64268df3b998e05797f749f03509dd4b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834920"
---
<a name="performing-simple-validation-c"></a><span data-ttu-id="11b20-104">Realizar validação simples (c#)</span><span class="sxs-lookup"><span data-stu-id="11b20-104">Performing Simple Validation (C#)</span></span>
====================
<span data-ttu-id="11b20-105">por [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="11b20-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="11b20-106">Saiba como executar a validação em um aplicativo ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="11b20-106">Learn how to perform validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="11b20-107">Neste tutorial, Stephen Walther apresenta você ao estado de modelo e os auxiliares de validação HTML.</span><span class="sxs-lookup"><span data-stu-id="11b20-107">In this tutorial, Stephen Walther introduces you to model state and the validation HTML helpers.</span></span>


<span data-ttu-id="11b20-108">O objetivo deste tutorial é explicar como você pode executar a validação dentro de um aplicativo ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="11b20-108">The goal of this tutorial is to explain how you can perform validation within an ASP.NET MVC application.</span></span> <span data-ttu-id="11b20-109">Por exemplo, você aprenderá a evitar que alguém envie um formulário que não contém um valor para um campo obrigatório.</span><span class="sxs-lookup"><span data-stu-id="11b20-109">For example, you learn how to prevent someone from submitting a form that does not contain a value for a required field.</span></span> <span data-ttu-id="11b20-110">Você aprenderá a usar o estado de modelo e os auxiliares HTML de validação.</span><span class="sxs-lookup"><span data-stu-id="11b20-110">You learn how to use model state and the validation HTML helpers.</span></span>

## <a name="understanding-model-state"></a><span data-ttu-id="11b20-111">Compreendendo o estado do modelo</span><span class="sxs-lookup"><span data-stu-id="11b20-111">Understanding Model State</span></span>

<span data-ttu-id="11b20-112">Use o estado do modelo - ou mais precisamente, o dicionário de estado de modelo - para representar erros de validação.</span><span class="sxs-lookup"><span data-stu-id="11b20-112">You use model state - or more accurately, the model state dictionary - to represent validation errors.</span></span> <span data-ttu-id="11b20-113">Por exemplo, a ação Create () na listagem 1 valida as propriedades de uma classe de produto antes de adicionar a classe de produto a um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="11b20-113">For example, the Create() action in Listing 1 validates the properties of a Product class before adding the Product class to a database.</span></span>


<span data-ttu-id="11b20-114">Eu não estou recomendando que você adicione sua lógica de validação ou o banco de dados a um controlador.</span><span class="sxs-lookup"><span data-stu-id="11b20-114">I'm not recommending that you add your validation or database logic to a controller.</span></span> <span data-ttu-id="11b20-115">Um controlador deve conter apenas a lógica relacionada ao controle de fluxo do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="11b20-115">A controller should contain only logic related to application flow control.</span></span> <span data-ttu-id="11b20-116">Estamos realizando um atalho para manter as coisas simples.</span><span class="sxs-lookup"><span data-stu-id="11b20-116">We are taking a shortcut to keep things simple.</span></span>


<span data-ttu-id="11b20-117">**Listagem 1 - Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="11b20-117">**Listing 1 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](performing-simple-validation-cs/samples/sample1.cs)]

<span data-ttu-id="11b20-118">Na listagem 1, as propriedades de nome, descrição e UnitsInStock da classe Product são validadas.</span><span class="sxs-lookup"><span data-stu-id="11b20-118">In Listing 1, the Name, Description, and UnitsInStock properties of the Product class are validated.</span></span> <span data-ttu-id="11b20-119">Se qualquer uma dessas propriedades realizar um teste de validação um erro será adicionado ao dicionário de estado de modelo (representado pela propriedade ModelState da classe Controller).</span><span class="sxs-lookup"><span data-stu-id="11b20-119">If any of these properties fail a validation test then an error is added to the model state dictionary (represented by the ModelState property of the Controller class).</span></span>

<span data-ttu-id="11b20-120">Se houver erros no estado de modelo, em seguida, a propriedade ModelState retornará false.</span><span class="sxs-lookup"><span data-stu-id="11b20-120">If there are any errors in model state then the ModelState.IsValid property returns false.</span></span> <span data-ttu-id="11b20-121">Nesse caso, o formulário HTML para a criação de um novo produto é exibida novamente.</span><span class="sxs-lookup"><span data-stu-id="11b20-121">In that case, the HTML form for creating a new product is redisplayed.</span></span> <span data-ttu-id="11b20-122">Caso contrário, se não houver nenhum erro de validação, o novo produto é adicionado ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="11b20-122">Otherwise, if there are no validation errors, the new Product is added to the database.</span></span>

## <a name="using-the-validation-helpers"></a><span data-ttu-id="11b20-123">Usando os auxiliares de validação</span><span class="sxs-lookup"><span data-stu-id="11b20-123">Using the Validation Helpers</span></span>

<span data-ttu-id="11b20-124">O ASP.NET MVC framework inclui dois auxiliares de validação: o auxiliar de Html.ValidationMessage() e o auxiliar Html.ValidationSummary().</span><span class="sxs-lookup"><span data-stu-id="11b20-124">The ASP.NET MVC framework includes two validation helpers: the Html.ValidationMessage() helper and the Html.ValidationSummary() helper.</span></span> <span data-ttu-id="11b20-125">Você pode usar esses dois auxiliares em uma exibição para exibir mensagens de erro de validação.</span><span class="sxs-lookup"><span data-stu-id="11b20-125">You use these two helpers in a view to display validation error messages.</span></span>

<span data-ttu-id="11b20-126">Os auxiliares Html.ValidationMessage() e Html.ValidationSummary() são usados nas exibições de criar e editar que são geradas automaticamente pelo scaffolding MVC do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="11b20-126">The Html.ValidationMessage() and Html.ValidationSummary() helpers are used in the Create and Edit views that are generated automatically by the ASP.NET MVC scaffolding.</span></span> <span data-ttu-id="11b20-127">Siga estas etapas para gerar a exibição Create:</span><span class="sxs-lookup"><span data-stu-id="11b20-127">Follow these steps to generate the Create view:</span></span>

1. <span data-ttu-id="11b20-128">A ação Create () no controlador de produto com o botão direito e selecione a opção de menu **adicionar exibição** (veja a Figura 1).</span><span class="sxs-lookup"><span data-stu-id="11b20-128">Right-click the Create() action in the Product controller and select the menu option **Add View** (see Figure 1).</span></span>
2. <span data-ttu-id="11b20-129">No **adicionar exibição** caixa de diálogo, marque a caixa de seleção **criar uma exibição fortemente tipada** (veja a Figura 2).</span><span class="sxs-lookup"><span data-stu-id="11b20-129">In the **Add View** dialog, check the checkbox labeled **Create a strongly-typed view** (see Figure 2).</span></span>
3. <span data-ttu-id="11b20-130">Dos **exibir dados de classe** lista suspensa, selecione a classe Product.</span><span class="sxs-lookup"><span data-stu-id="11b20-130">From the **View data class** dropdown list, select the Product class.</span></span>
4. <span data-ttu-id="11b20-131">Dos **exibir o conteúdo** lista suspensa, selecione Criar.</span><span class="sxs-lookup"><span data-stu-id="11b20-131">From the **View content** dropdown list, select Create.</span></span>
5. <span data-ttu-id="11b20-132">Clique no botão **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="11b20-132">Click the **Add** button.</span></span>


<span data-ttu-id="11b20-133">Certifique-se de que você compila seu aplicativo antes de adicionar um modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="11b20-133">Make sure that you build your application before adding a view.</span></span> <span data-ttu-id="11b20-134">Caso contrário, a lista de classes não aparecerá na **exibir dados de classe** lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="11b20-134">Otherwise, the list of classes won't appear in the **View data class** dropdown list.</span></span>


<span data-ttu-id="11b20-135">[![A caixa de diálogo Novo projeto](performing-simple-validation-cs/_static/image1.jpg)](performing-simple-validation-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="11b20-135">[![The New Project dialog box](performing-simple-validation-cs/_static/image1.jpg)](performing-simple-validation-cs/_static/image1.png)</span></span>

<span data-ttu-id="11b20-136">**Figura 01**: adicionando uma exibição ([clique para exibir a imagem em tamanho normal](performing-simple-validation-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="11b20-136">**Figure 01**: Adding a view([Click to view full-size image](performing-simple-validation-cs/_static/image2.png))</span></span>


<span data-ttu-id="11b20-137">[![A caixa de diálogo Novo projeto](performing-simple-validation-cs/_static/image2.jpg)](performing-simple-validation-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="11b20-137">[![The New Project dialog box](performing-simple-validation-cs/_static/image2.jpg)](performing-simple-validation-cs/_static/image3.png)</span></span>

<span data-ttu-id="11b20-138">**Figura 02**: criar uma exibição fortemente tipada ([clique para exibir a imagem em tamanho normal](performing-simple-validation-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="11b20-138">**Figure 02**: Creating a strongly-typed view ([Click to view full-size image](performing-simple-validation-cs/_static/image4.png))</span></span>


<span data-ttu-id="11b20-139">Depois de concluir essas etapas, você obtém o modo de exibição criar na listagem 2.</span><span class="sxs-lookup"><span data-stu-id="11b20-139">After you complete these steps, you get the Create view in Listing 2.</span></span>

<span data-ttu-id="11b20-140">**Listagem 2 - Views\Product\Create.aspx**</span><span class="sxs-lookup"><span data-stu-id="11b20-140">**Listing 2 - Views\Product\Create.aspx**</span></span>

[!code-aspx[Main](performing-simple-validation-cs/samples/sample2.aspx)]

<span data-ttu-id="11b20-141">Na listagem 2, o auxiliar Html.ValidationSummary() é chamado imediatamente acima do formulário HTML.</span><span class="sxs-lookup"><span data-stu-id="11b20-141">In Listing 2, the Html.ValidationSummary() helper is called immediately above the HTML form.</span></span> <span data-ttu-id="11b20-142">Esse auxiliar é usado para exibir uma lista de mensagens de erro de validação.</span><span class="sxs-lookup"><span data-stu-id="11b20-142">This helper is used to display a list of validation error messages.</span></span> <span data-ttu-id="11b20-143">O auxiliar Html.ValidationSummary() renderiza os erros em uma lista com marcadores.</span><span class="sxs-lookup"><span data-stu-id="11b20-143">The Html.ValidationSummary() helper renders the errors in a bulleted list.</span></span>

<span data-ttu-id="11b20-144">O auxiliar Html.ValidationMessage() é chamado ao lado de cada um dos campos de formulário HTML.</span><span class="sxs-lookup"><span data-stu-id="11b20-144">The Html.ValidationMessage() helper is called next to each of the HTML form fields.</span></span> <span data-ttu-id="11b20-145">Esse auxiliar é usado para exibir uma mensagem de erro ao lado de um campo de formulário.</span><span class="sxs-lookup"><span data-stu-id="11b20-145">This helper is used to display an error message right next to a form field.</span></span> <span data-ttu-id="11b20-146">No caso da listagem 2, o auxiliar Html.ValidationMessage() exibe um asterisco, quando ocorre um erro.</span><span class="sxs-lookup"><span data-stu-id="11b20-146">In the case of Listing 2, the Html.ValidationMessage() helper displays an asterisk when there is an error.</span></span>

<span data-ttu-id="11b20-147">A página na Figura 3 ilustra as mensagens de erro renderizadas pelos auxiliares de validação quando o formulário é enviado com campos ausentes e valores inválidos.</span><span class="sxs-lookup"><span data-stu-id="11b20-147">The page in Figure 3 illustrates the error messages rendered by the validation helpers when the form is submitted with missing fields and invalid values.</span></span>


<span data-ttu-id="11b20-148">[![A caixa de diálogo Novo projeto](performing-simple-validation-cs/_static/image3.jpg)](performing-simple-validation-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="11b20-148">[![The New Project dialog box](performing-simple-validation-cs/_static/image3.jpg)](performing-simple-validation-cs/_static/image5.png)</span></span>

<span data-ttu-id="11b20-149">**Figura 03**: criar o modo de exibição enviado com problemas ([clique para exibir a imagem em tamanho normal](performing-simple-validation-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="11b20-149">**Figure 03**: The Create view submitted with problems ([Click to view full-size image](performing-simple-validation-cs/_static/image6.png))</span></span>


<span data-ttu-id="11b20-150">Observe que a aparência do HTML entrada campos também são modificados quando há um erro de validação.</span><span class="sxs-lookup"><span data-stu-id="11b20-150">Notice that the appearance of the HTML input fields are also modified when there is a validation error.</span></span> <span data-ttu-id="11b20-151">Os renderizadores de auxiliar Html.TextBox() uma *classe = "Erro de validação de entrada"* atributo quando há um erro de validação associado à propriedade renderizada pelo auxiliar de Html.TextBox().</span><span class="sxs-lookup"><span data-stu-id="11b20-151">The Html.TextBox() helper renders a *class="input-validation-error"* attribute when there is a validation error associated with the property rendered by the Html.TextBox() helper.</span></span>

<span data-ttu-id="11b20-152">Há três classes de folha de estilo em cascata usadas para controlar a aparência de erros de validação:</span><span class="sxs-lookup"><span data-stu-id="11b20-152">There are three cascading style sheet classes used to control the appearance of validation errors:</span></span>

- <span data-ttu-id="11b20-153">entrada-erro de validação - aplicado para o &lt;entrada&gt; renderizada pelo Html.TextBox() auxiliar de marca.</span><span class="sxs-lookup"><span data-stu-id="11b20-153">input-validation-error - Applied to the &lt;input&gt; tag rendered by Html.TextBox() helper.</span></span>
- <span data-ttu-id="11b20-154">campo--erro de validação - aplicado à &lt;span&gt; renderizada pelo Html.ValidationMessage() auxiliar de marca.</span><span class="sxs-lookup"><span data-stu-id="11b20-154">field-validation-error - Applied to the &lt;span&gt; tag rendered by the Html.ValidationMessage() helper.</span></span>
- <span data-ttu-id="11b20-155">– Resumo – erros de validação - aplicado para o &lt;ul&gt; renderizada pelo Html.ValidationSumamry() auxiliar de marca.</span><span class="sxs-lookup"><span data-stu-id="11b20-155">validation-summary-errors - Applied to the &lt;ul&gt; tag rendered by the Html.ValidationSumamry() helper.</span></span>

<span data-ttu-id="11b20-156">Você pode modificar essas classes de folha de estilo em cascata e, portanto, modificar a aparência dos erros de validação, modificando o arquivo CSS localizado na pasta de conteúdo.</span><span class="sxs-lookup"><span data-stu-id="11b20-156">You can modify these cascading style sheet classes, and therefore modify the appearance of the validation errors, by modifying the Site.css file located in the Content folder.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="11b20-157">A classe HtmlHelper inclui propriedades estáticas somente leitura para recuperar os nomes de validação relacionados a CSS classes.</span><span class="sxs-lookup"><span data-stu-id="11b20-157">The HtmlHelper class includes read-only static properties for retrieving the names of the validation related CSS classes.</span></span> <span data-ttu-id="11b20-158">Essas propriedades estáticas são nomeadas ValidationInputCssClassName, ValidationFieldCssClassName e ValidationSummaryCssClassName.</span><span class="sxs-lookup"><span data-stu-id="11b20-158">These static properties are named ValidationInputCssClassName, ValidationFieldCssClassName, and ValidationSummaryCssClassName.</span></span>


## <a name="prebinding-validation-and-postbinding-validation"></a><span data-ttu-id="11b20-159">Prebinding validação e a validação de Postbinding</span><span class="sxs-lookup"><span data-stu-id="11b20-159">Prebinding Validation and Postbinding Validation</span></span>

<span data-ttu-id="11b20-160">Se você enviar o formulário HTML para a criação de um produto e insira um valor inválido para o campo de preço e nenhum valor para o campo UnitsInStock, em seguida, você receberá as mensagens de validação exibidas na Figura 4.</span><span class="sxs-lookup"><span data-stu-id="11b20-160">If you submit the HTML form for creating a Product, and you enter an invalid value for the price field and no value for the UnitsInStock field, then you'll get the validation messages displayed in Figure 4.</span></span> <span data-ttu-id="11b20-161">Onde vêm essas mensagens de erro de validação?</span><span class="sxs-lookup"><span data-stu-id="11b20-161">Where do these validation error messages come from?</span></span>


<span data-ttu-id="11b20-162">[![A caixa de diálogo Novo projeto](performing-simple-validation-cs/_static/image4.jpg)](performing-simple-validation-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="11b20-162">[![The New Project dialog box](performing-simple-validation-cs/_static/image4.jpg)](performing-simple-validation-cs/_static/image7.png)</span></span>

<span data-ttu-id="11b20-163">**Figura 04**: erros de validação Prebinding ([clique para exibir a imagem em tamanho normal](performing-simple-validation-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="11b20-163">**Figure 04**: Prebinding Validation Errors([Click to view full-size image](performing-simple-validation-cs/_static/image8.png))</span></span>


<span data-ttu-id="11b20-164">Há realmente dois tipos de mensagens de erro de validação - aqueles gerados antes que os campos de formulário HTML são associados a uma classe e aqueles gerados depois que os campos de formulário são associados à classe.</span><span class="sxs-lookup"><span data-stu-id="11b20-164">There are actually two types of validation error messages - those generated before the HTML form fields are bound to a class and those generated after the form fields are bound to the class.</span></span> <span data-ttu-id="11b20-165">Em outras palavras, há prebinding erros de validação e postbinding erros de validação.</span><span class="sxs-lookup"><span data-stu-id="11b20-165">In other words, there are prebinding validation errors and postbinding validation errors.</span></span>

<span data-ttu-id="11b20-166">A ação Create () exposta pelo controlador de produto na listagem 1 aceita uma instância da classe Product.</span><span class="sxs-lookup"><span data-stu-id="11b20-166">The Create() action exposed by the Product controller in Listing 1 accepts an instance of the Product class.</span></span> <span data-ttu-id="11b20-167">A assinatura do método Create tem esta aparência:</span><span class="sxs-lookup"><span data-stu-id="11b20-167">The signature of the Create method looks like this:</span></span>

[!code-csharp[Main](performing-simple-validation-cs/samples/sample3.cs)]

<span data-ttu-id="11b20-168">Os valores dos campos de formulário HTML do formulário Criar são associados à classe productToCreate por algo chamado de um associador de modelo.</span><span class="sxs-lookup"><span data-stu-id="11b20-168">The values of the HTML form fields from the Create form are bound to the productToCreate class by something called a model binder.</span></span> <span data-ttu-id="11b20-169">O associador de modelo padrão adiciona uma mensagem de erro para o estado de modelo automaticamente quando ele não é possível associar um campo de formulário a uma propriedade de formulário.</span><span class="sxs-lookup"><span data-stu-id="11b20-169">The default model binder adds an error message to model state automatically when it cannot bind a form field to a form property.</span></span>

<span data-ttu-id="11b20-170">O associador de modelo padrão não é possível associar a cadeia de caracteres "apple" para a propriedade de preço da classe Product.</span><span class="sxs-lookup"><span data-stu-id="11b20-170">The default model binder cannot bind the string "apple" to the Price property of the Product class.</span></span> <span data-ttu-id="11b20-171">É possível atribuir uma cadeia de caracteres a uma propriedade decimal.</span><span class="sxs-lookup"><span data-stu-id="11b20-171">You can't assign a string to a decimal property.</span></span> <span data-ttu-id="11b20-172">Portanto, o associador de modelo adiciona um erro para o estado de modelo.</span><span class="sxs-lookup"><span data-stu-id="11b20-172">Therefore, the model binder adds an error to model state.</span></span>

<span data-ttu-id="11b20-173">O associador de modelo padrão também não é possível atribuir um valor nulo a uma propriedade que não aceita valores nulos.</span><span class="sxs-lookup"><span data-stu-id="11b20-173">The default model binder also cannot assign a null value to a property that does not accept nulls.</span></span> <span data-ttu-id="11b20-174">Em particular, o associador de modelo não é possível atribuir um valor nulo para a propriedade UnitsInStock.</span><span class="sxs-lookup"><span data-stu-id="11b20-174">In particular, the model binder cannot assign a null value to the UnitsInStock property.</span></span> <span data-ttu-id="11b20-175">Mais uma vez, o associador de modelo desiste e adiciona uma mensagem de erro para o estado de modelo.</span><span class="sxs-lookup"><span data-stu-id="11b20-175">Once again, the model binder gives up and adds an error message to model state.</span></span>

<span data-ttu-id="11b20-176">Se você quiser personalizar a aparência desses prebinding mensagens de erro, em seguida, você precisa criar cadeias de caracteres de recurso para essas mensagens.</span><span class="sxs-lookup"><span data-stu-id="11b20-176">If you want to customize the appearance of these prebinding error messages then you need to create resource strings for these messages.</span></span>

## <a name="summary"></a><span data-ttu-id="11b20-177">Resumo</span><span class="sxs-lookup"><span data-stu-id="11b20-177">Summary</span></span>

<span data-ttu-id="11b20-178">O objetivo deste tutorial era descrever a mecânica básica de validação na estrutura do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="11b20-178">The goal of this tutorial was to describe the basic mechanics of validation in the ASP.NET MVC framework.</span></span> <span data-ttu-id="11b20-179">Você aprendeu a usar o estado de modelo e os auxiliares HTML de validação.</span><span class="sxs-lookup"><span data-stu-id="11b20-179">You learned how to use model state and the validation HTML helpers.</span></span> <span data-ttu-id="11b20-180">Também discutimos a distinção entre prebinding e postbinding validação.</span><span class="sxs-lookup"><span data-stu-id="11b20-180">We also discussed the distinction between prebinding and postbinding validation.</span></span> <span data-ttu-id="11b20-181">Outros tutoriais, discutiremos várias estratégias para mover o código de validação para fora de seus controladores de e para suas classes de modelo.</span><span class="sxs-lookup"><span data-stu-id="11b20-181">In other tutorials, we'll discuss various strategies for moving your validation code out of your controllers and into your model classes.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="11b20-182">[Anterior](displaying-a-table-of-database-data-cs.md)
> [Próximo](validating-with-the-idataerrorinfo-interface-cs.md)</span><span class="sxs-lookup"><span data-stu-id="11b20-182">[Previous](displaying-a-table-of-database-data-cs.md)
[Next](validating-with-the-idataerrorinfo-interface-cs.md)</span></span>
